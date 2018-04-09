---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Vytváření objektů přenos dat (DTOs) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 35e01f959072b96204de3e2ce3d507635720e110
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="ea6eb-102">Vytváření objektů přenos dat (DTOs)</span><span class="sxs-lookup"><span data-stu-id="ea6eb-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="ea6eb-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ea6eb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ea6eb-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="ea6eb-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="ea6eb-105">Pravé nyní naše webové rozhraní API zpřístupní entity databáze do klienta.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="ea6eb-106">Klient obdrží data, která mapuje přímo do databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="ea6eb-107">Nicméně, který není vždy vhodné.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-107">However, that's not always a good idea.</span></span> <span data-ttu-id="ea6eb-108">Někdy budete chtít změnit tvar data, která můžete odeslat do klienta.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="ea6eb-109">Například můžete chtít:</span><span class="sxs-lookup"><span data-stu-id="ea6eb-109">For example, you might want to:</span></span>

- <span data-ttu-id="ea6eb-110">Odeberte cyklické odkazy (viz předchozí části).</span><span class="sxs-lookup"><span data-stu-id="ea6eb-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="ea6eb-111">Skryjte konkrétní vlastnosti, které klienti nemají co dělat k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="ea6eb-112">Vynechte některé vlastnosti za účelem snížení velikosti datové části.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="ea6eb-113">Vyrovnání objekt grafy, které obsahují vnořených objektů, aby je bylo pohodlnější pro klienty.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="ea6eb-114">Vyhněte se "typu overpost" ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="ea6eb-115">(Viz [ověření modelu](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) diskuzi o navýšení zveřejňování.)</span><span class="sxs-lookup"><span data-stu-id="ea6eb-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="ea6eb-116">Oddělte vrstvě služby z databázové vrstvě.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="ea6eb-117">Pokud chcete dosáhnout, můžete definovat *objekt pro přenos dat* (DTO).</span><span class="sxs-lookup"><span data-stu-id="ea6eb-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="ea6eb-118">DTO je objekt, který definuje, jak budou data odeslány prostřednictvím sítě.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="ea6eb-119">Podívejme se, jak funguje s entitou adresáře.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="ea6eb-120">Ve složce modely přidejte dvě třídy DTO:</span><span class="sxs-lookup"><span data-stu-id="ea6eb-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="ea6eb-121">`BookDetailDTO` Třída obsahuje všechny vlastnosti z adresáře modelu, vyjma toho, že `AuthorName` je řetězec, který bude obsahovat jméno autora.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="ea6eb-122">`BookDTO` Třída obsahuje podmnožinu vlastnosti z `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="ea6eb-123">Potom nahraďte tyto dvě metody GET v `BooksController` třídy s verzemi, které vracejí DTOs.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="ea6eb-124">Použijeme LINQ **vyberte** příkaz Převést z adresáře entity do DTOs.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="ea6eb-125">Tady je SQL generované nové `GetBooks` metoda.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="ea6eb-126">Uvidíte, že EF překládá LINQ **vyberte** do příkazu SQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="ea6eb-127">Nakonec upravte `PostBook` metoda vrátí DTO.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="ea6eb-128">V tomto kurzu jsme se převodu na DTOs ručně v kódu.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="ea6eb-129">Další možností je použití knihovny jako [AutoMapper](http://automapper.org/) , která zpracovává převod automaticky.</span><span class="sxs-lookup"><span data-stu-id="ea6eb-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="ea6eb-130">[Předchozí](part-4.md)
> [další](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="ea6eb-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
