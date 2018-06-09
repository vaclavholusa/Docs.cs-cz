---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamické v. Silného typu zobrazení | Microsoft Docs
author: Rick-Anderson
description: ''
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
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "26565531"
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="c479f-103">Dynamické v.</span><span class="sxs-lookup"><span data-stu-id="c479f-103">Dynamic v.</span></span> <span data-ttu-id="c479f-104">Zobrazení se silnými typy</span><span class="sxs-lookup"><span data-stu-id="c479f-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="c479f-105">podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="c479f-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="c479f-106">Existují tři způsoby, jak předat informace z řadiče zobrazení v architektuře ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="c479f-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="c479f-107">Jako objekt silného typu modelu.</span><span class="sxs-lookup"><span data-stu-id="c479f-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="c479f-108">Jako typ dynamické (pomocí @model dynamické)</span><span class="sxs-lookup"><span data-stu-id="c479f-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="c479f-109">Pomocí objekt ViewBag</span><span class="sxs-lookup"><span data-stu-id="c479f-109">Using the ViewBag</span></span>

<span data-ttu-id="c479f-110">I jste zapisují jednoduchou aplikaci MVC 3 horní Blog porovnat a kontrastu zobrazení dynamické a silného typu.</span><span class="sxs-lookup"><span data-stu-id="c479f-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="c479f-111">Řadičem začíná se jednoduchým seznamem blogy:</span><span class="sxs-lookup"><span data-stu-id="c479f-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="c479f-112">Klikněte pravým tlačítkem v metodě IndexNotStonglyTyped() a přidat zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="c479f-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="c479f-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c479f-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="c479f-114">Zajistěte, aby **vytvořit zobrazení silného typu** není zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="c479f-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="c479f-115">Výsledné zobrazení neobsahuje mnohem:</span><span class="sxs-lookup"><span data-stu-id="c479f-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="c479f-116">Vzhledem k tomu, že používáme dynamický a ne zobrazení se silnými typy, není nám pomůže intellisense.</span><span class="sxs-lookup"><span data-stu-id="c479f-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="c479f-117">Dokončený kód je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="c479f-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="c479f-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c479f-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="c479f-119">Teď přidáme zobrazení se silnými typy.</span><span class="sxs-lookup"><span data-stu-id="c479f-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="c479f-120">Přidejte následující kód do kontroleru:</span><span class="sxs-lookup"><span data-stu-id="c479f-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="c479f-121">Všimněte si, že je přesně stejnou návratový View(topBlogs); volají se jako jiný silného typu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c479f-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="c479f-122">Klikněte pravým tlačítkem uvnitř *StonglyTypedIndex()* a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="c479f-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="c479f-123">Vyberte tuto chvíli **Blog** třída modelu a vyberte **seznamu** jako šablonu vygenerované uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c479f-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="c479f-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c479f-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="c479f-125">Uvnitř novou šablonu zobrazení se nám získat podporu technologie intellisense.</span><span class="sxs-lookup"><span data-stu-id="c479f-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="c479f-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="c479f-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="c479f-127">Projekt c# lze stáhnout [zde](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="c479f-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
