---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: "Vytváření objektů přenos dat (DTOs) | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: e179dcd52200734a45c84b3c7c3069a4727904c1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="2336f-102">Vytváření objektů přenos dat (DTOs)</span><span class="sxs-lookup"><span data-stu-id="2336f-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="2336f-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2336f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2336f-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="2336f-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="2336f-105">Pravé nyní naše webové rozhraní API zpřístupní entity databáze do klienta.</span><span class="sxs-lookup"><span data-stu-id="2336f-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="2336f-106">Klient obdrží data, která mapuje přímo do databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="2336f-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="2336f-107">Nicméně, který není vždy vhodné.</span><span class="sxs-lookup"><span data-stu-id="2336f-107">However, that's not always a good idea.</span></span> <span data-ttu-id="2336f-108">Někdy budete chtít změnit tvar data, která můžete odeslat do klienta.</span><span class="sxs-lookup"><span data-stu-id="2336f-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="2336f-109">Například můžete chtít:</span><span class="sxs-lookup"><span data-stu-id="2336f-109">For example, you might want to:</span></span>

- <span data-ttu-id="2336f-110">Odeberte cyklické odkazy (viz předchozí části).</span><span class="sxs-lookup"><span data-stu-id="2336f-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="2336f-111">Skryjte konkrétní vlastnosti, které klienti nemají co dělat k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2336f-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="2336f-112">Vynechte některé vlastnosti za účelem snížení velikosti datové části.</span><span class="sxs-lookup"><span data-stu-id="2336f-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="2336f-113">Vyrovnání objekt grafy, které obsahují vnořených objektů, aby je bylo pohodlnější pro klienty.</span><span class="sxs-lookup"><span data-stu-id="2336f-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="2336f-114">Vyhněte se "typu overpost" ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2336f-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="2336f-115">(Viz [ověření modelu](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) diskuzi o navýšení zveřejňování.)</span><span class="sxs-lookup"><span data-stu-id="2336f-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="2336f-116">Oddělte vrstvě služby z databázové vrstvě.</span><span class="sxs-lookup"><span data-stu-id="2336f-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="2336f-117">Pokud chcete dosáhnout, můžete definovat *objekt pro přenos dat* (DTO).</span><span class="sxs-lookup"><span data-stu-id="2336f-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="2336f-118">DTO je objekt, který definuje, jak budou data odeslány prostřednictvím sítě.</span><span class="sxs-lookup"><span data-stu-id="2336f-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="2336f-119">Podívejme se, jak funguje s entitou adresáře.</span><span class="sxs-lookup"><span data-stu-id="2336f-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="2336f-120">Ve složce modely přidejte dvě třídy DTO:</span><span class="sxs-lookup"><span data-stu-id="2336f-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="2336f-121">`BookDetailDTO` Třída obsahuje všechny vlastnosti z adresáře modelu, vyjma toho, že `AuthorName` je řetězec, který bude obsahovat jméno autora.</span><span class="sxs-lookup"><span data-stu-id="2336f-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="2336f-122">`BookDTO` Třída obsahuje podmnožinu vlastnosti z `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="2336f-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="2336f-123">Potom nahraďte tyto dvě metody GET v `BooksController` třídy s verzemi, které vracejí DTOs.</span><span class="sxs-lookup"><span data-stu-id="2336f-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="2336f-124">Použijeme LINQ **vyberte** příkaz Převést z adresáře entity do DTOs.</span><span class="sxs-lookup"><span data-stu-id="2336f-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="2336f-125">Tady je SQL generované nové `GetBooks` metoda.</span><span class="sxs-lookup"><span data-stu-id="2336f-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="2336f-126">Uvidíte, že EF překládá LINQ **vyberte** do příkazu SQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="2336f-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="2336f-127">Nakonec upravte `PostBook` metoda vrátí DTO.</span><span class="sxs-lookup"><span data-stu-id="2336f-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="2336f-128">V tomto kurzu jsme se převodu na DTOs ručně v kódu.</span><span class="sxs-lookup"><span data-stu-id="2336f-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="2336f-129">Další možností je použití knihovny jako [AutoMapper](http://automapper.org/) , která zpracovává převod automaticky.</span><span class="sxs-lookup"><span data-stu-id="2336f-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2336f-130">[Předchozí](part-4.md)
[další](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="2336f-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
