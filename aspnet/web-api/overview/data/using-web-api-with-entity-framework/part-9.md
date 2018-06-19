---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Přidat novou položku do databáze | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 5845c092c4d7aee12b33b3f0a49c0e944c0fb9aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868370"
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="1f1fb-102">Přidat novou položku do databáze</span><span class="sxs-lookup"><span data-stu-id="1f1fb-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="1f1fb-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1f1fb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="1f1fb-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="1f1fb-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="1f1fb-105">V této části bude přidána možnost pro uživatele k vytvoření nového seznamu.</span><span class="sxs-lookup"><span data-stu-id="1f1fb-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="1f1fb-106">V app.js přidejte následující kód do modelu zobrazení:</span><span class="sxs-lookup"><span data-stu-id="1f1fb-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="1f1fb-107">V Index.cshtml vyměňte následující kód:</span><span class="sxs-lookup"><span data-stu-id="1f1fb-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="1f1fb-108">Pomocí:</span><span class="sxs-lookup"><span data-stu-id="1f1fb-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="1f1fb-109">Tento kód vytvoří formulář pro odeslání nové autora.</span><span class="sxs-lookup"><span data-stu-id="1f1fb-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="1f1fb-110">Hodnoty pro rozevíracího seznamu Autor jsou vázané na data do `authors` pozorovatelné v zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="1f1fb-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="1f1fb-111">Pro ostatní vstupy formuláře, hodnoty jsou vázané na data do `newBook` vlastnost modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1f1fb-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="1f1fb-112">Obslužná rutina odeslání formuláře je vázána `addBook` funkce:</span><span class="sxs-lookup"><span data-stu-id="1f1fb-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="1f1fb-113">`addBook` Funkce načte aktuální hodnoty vázané na data formuláře vstupy pro vytvoření objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="1f1fb-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="1f1fb-114">Pak se odešle objekt JSON, který má `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="1f1fb-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1f1fb-115">[Předchozí](part-8.md)
> [další](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="1f1fb-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
