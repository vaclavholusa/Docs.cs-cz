---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamické zobrazení vs. Zobrazení se silnými typy | Dokumentace Microsoftu
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 7622ca8248374da27f4190075df5a6bfc32bb2e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389082"
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="9a646-103">Dynamické zobrazení vs.</span><span class="sxs-lookup"><span data-stu-id="9a646-103">Dynamic v.</span></span> <span data-ttu-id="9a646-104">Zobrazení se silnými typy</span><span class="sxs-lookup"><span data-stu-id="9a646-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="9a646-105">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="9a646-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="9a646-106">Existují tři způsoby, jak předat informace z řadiče zobrazení v architektuře ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="9a646-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="9a646-107">Jako model silného typu objektu.</span><span class="sxs-lookup"><span data-stu-id="9a646-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="9a646-108">Jako dynamický typ (pomocí @model dynamické)</span><span class="sxs-lookup"><span data-stu-id="9a646-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="9a646-109">Pomocí objekt ViewBag</span><span class="sxs-lookup"><span data-stu-id="9a646-109">Using the ViewBag</span></span>

<span data-ttu-id="9a646-110">Můžu jste napsali jednoduchou aplikaci MVC 3 horní blogu můžete porovnat a kontrast zobrazení dynamických webů a silného typu.</span><span class="sxs-lookup"><span data-stu-id="9a646-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="9a646-111">Kontroler začíná jednoduchým seznamem blogy:</span><span class="sxs-lookup"><span data-stu-id="9a646-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="9a646-112">Klikněte pravým tlačítkem myši v metodě IndexNotStonglyTyped() a přidat zobrazení Razor.</span><span class="sxs-lookup"><span data-stu-id="9a646-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="9a646-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9a646-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="9a646-114">Ujistěte se, **vytvoření zobrazení se silnými typy** políčko není zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="9a646-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="9a646-115">Výsledné zobrazení neobsahuje většinu:</span><span class="sxs-lookup"><span data-stu-id="9a646-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="9a646-116">Protože používáme dynamické a ne zobrazení se silnými typy, intellisense vám nepomohly USA.</span><span class="sxs-lookup"><span data-stu-id="9a646-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="9a646-117">Dokončený kód je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="9a646-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="9a646-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="9a646-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="9a646-119">Teď přidáme zobrazení se silnými typy.</span><span class="sxs-lookup"><span data-stu-id="9a646-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="9a646-120">Přidejte následující kód do kontroleru:</span><span class="sxs-lookup"><span data-stu-id="9a646-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="9a646-121">Všimněte si, že je přesně stejný návratový View(topBlogs); volat jako jiné silného typu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9a646-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="9a646-122">Klikněte pravým tlačítkem uvnitř *StonglyTypedIndex()* a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="9a646-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="9a646-123">Tentokrát vyberte **blogu** třída modelu a vyberte **seznamu** jako šablona Scaffold.</span><span class="sxs-lookup"><span data-stu-id="9a646-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="9a646-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="9a646-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="9a646-125">Uvnitř novou šablonu zobrazení jsme získat podporu technologie intellisense.</span><span class="sxs-lookup"><span data-stu-id="9a646-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="9a646-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="9a646-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="9a646-127">Projekt c# si můžete stáhnout [tady](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="9a646-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
