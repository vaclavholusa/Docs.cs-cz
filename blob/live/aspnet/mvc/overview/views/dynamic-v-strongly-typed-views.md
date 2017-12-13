---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: "Dynamické v. Silného typu zobrazení | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="c5df5-103">Dynamické v.</span><span class="sxs-lookup"><span data-stu-id="c5df5-103">Dynamic v.</span></span> <span data-ttu-id="c5df5-104">Zobrazení se silnými typy</span><span class="sxs-lookup"><span data-stu-id="c5df5-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="c5df5-105">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="c5df5-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="c5df5-106">Existují tři způsoby, jak předat informace z řadiče zobrazení v architektuře ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="c5df5-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="c5df5-107">Jako objekt silného typu modelu.</span><span class="sxs-lookup"><span data-stu-id="c5df5-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="c5df5-108">Jako typ dynamické (pomocí @model dynamické)</span><span class="sxs-lookup"><span data-stu-id="c5df5-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="c5df5-109">Pomocí objekt ViewBag</span><span class="sxs-lookup"><span data-stu-id="c5df5-109">Using the ViewBag</span></span>

<span data-ttu-id="c5df5-110">I jste zapisují jednoduchou aplikaci MVC 3 horní Blog porovnat a kontrastu zobrazení dynamické a silného typu.</span><span class="sxs-lookup"><span data-stu-id="c5df5-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="c5df5-111">Řadičem začíná se jednoduchým seznamem blogy:</span><span class="sxs-lookup"><span data-stu-id="c5df5-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="c5df5-112">Klikněte pravým tlačítkem v metodě IndexNotStonglyTyped() a přidat zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="c5df5-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="c5df5-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c5df5-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="c5df5-114">Zajistěte, aby **vytvořit zobrazení silného typu** není zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="c5df5-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="c5df5-115">Výsledné zobrazení neobsahuje mnohem:</span><span class="sxs-lookup"><span data-stu-id="c5df5-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="c5df5-116">Vzhledem k tomu, že používáme dynamický a ne zobrazení se silnými typy, není nám pomůže intellisense.</span><span class="sxs-lookup"><span data-stu-id="c5df5-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="c5df5-117">Dokončený kód je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="c5df5-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="c5df5-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c5df5-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="c5df5-119">Teď přidáme zobrazení se silnými typy.</span><span class="sxs-lookup"><span data-stu-id="c5df5-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="c5df5-120">Přidejte následující kód do kontroleru:</span><span class="sxs-lookup"><span data-stu-id="c5df5-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="c5df5-121">Všimněte si, že je přesně stejnou návratový View(topBlogs); volají se jako jiný silného typu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c5df5-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="c5df5-122">Klikněte pravým tlačítkem uvnitř *StonglyTypedIndex()* a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="c5df5-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="c5df5-123">Vyberte tuto chvíli **Blog** třída modelu a vyberte **seznamu** jako šablonu vygenerované uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5df5-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="c5df5-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c5df5-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="c5df5-125">Uvnitř novou šablonu zobrazení se nám získat podporu technologie intellisense.</span><span class="sxs-lookup"><span data-stu-id="c5df5-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="c5df5-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="c5df5-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="c5df5-127">Projekt c# lze stáhnout [zde](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="c5df5-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
