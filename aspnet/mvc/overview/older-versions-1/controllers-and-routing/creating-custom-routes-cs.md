---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Vytváření vlastní trasy (C#) | Microsoft Docs
author: microsoft
description: Zjistěte, jak přidat vlastní trasy pro aplikace ASP.NET MVC. V tomto kurzu zjistěte, jak změnit výchozí směrovací tabulka v souboru Global.asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 573b6a3360124feea92788ff7a3de363840fa1ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867964"
---
<a name="creating-custom-routes-c"></a><span data-ttu-id="0a33c-104">Vytváření vlastní trasy (C#)</span><span class="sxs-lookup"><span data-stu-id="0a33c-104">Creating Custom Routes (C#)</span></span>
====================
<span data-ttu-id="0a33c-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0a33c-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="0a33c-106">Zjistěte, jak přidat vlastní trasy pro aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0a33c-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="0a33c-107">V tomto kurzu zjistěte, jak změnit výchozí směrovací tabulka v souboru Global.asax.</span><span class="sxs-lookup"><span data-stu-id="0a33c-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="0a33c-108">V tomto kurzu zjistěte, jak přidat vlastní trasy pro aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0a33c-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="0a33c-109">Zjistíte, jak změnit výchozí směrovací tabulka v souboru Global.asax s vlastní trasy.</span><span class="sxs-lookup"><span data-stu-id="0a33c-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="0a33c-110">Pro mnoho jednoduché aplikace ASP.NET MVC bude výchozí směrovací tabulka pracovat správně.</span><span class="sxs-lookup"><span data-stu-id="0a33c-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="0a33c-111">Však může zjišťovat specializovaný směrování potřebám.</span><span class="sxs-lookup"><span data-stu-id="0a33c-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="0a33c-112">V takovém případě můžete vytvořit vlastní trasy.</span><span class="sxs-lookup"><span data-stu-id="0a33c-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="0a33c-113">Představte si například, že vytváříte aplikaci blogu.</span><span class="sxs-lookup"><span data-stu-id="0a33c-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="0a33c-114">Můžete chtít zpracovávat příchozí požadavky, které vypadají takto:</span><span class="sxs-lookup"><span data-stu-id="0a33c-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="0a33c-115">/ Archivu/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="0a33c-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="0a33c-116">Když uživatel zadá tuto žádost, chcete vrátit položku blogu, která odpovídá datum 12/25/2009.</span><span class="sxs-lookup"><span data-stu-id="0a33c-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="0a33c-117">Aby bylo možné zpracovávat tento typ požadavku, musíte vytvořit vlastní trasy.</span><span class="sxs-lookup"><span data-stu-id="0a33c-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="0a33c-118">Soubor Global.asax v výpis 1 obsahuje nové vlastní trasy, s názvem Blog, která zpracovává požadavky, které vypadat /Archive/*datum položky*.</span><span class="sxs-lookup"><span data-stu-id="0a33c-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="0a33c-119">**Výpis 1 - Global.asax (s vlastní směrování)**</span><span class="sxs-lookup"><span data-stu-id="0a33c-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="0a33c-120">Je důležité pořadí tras, které přidáte do tabulky směrování.</span><span class="sxs-lookup"><span data-stu-id="0a33c-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="0a33c-121">Naše nová vlastní Blog trasa se přidá před stávající výchozí trasu.</span><span class="sxs-lookup"><span data-stu-id="0a33c-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="0a33c-122">Pokud je změněn na opačný pořadí, pak výchozí trasu vždy bude zavolána místo vlastní trasy.</span><span class="sxs-lookup"><span data-stu-id="0a33c-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="0a33c-123">Vlastní Blog trasa odpovídá každá žádost, který začíná/archivu /.</span><span class="sxs-lookup"><span data-stu-id="0a33c-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="0a33c-124">Proto že splňuje všechny následující adresy URL:</span><span class="sxs-lookup"><span data-stu-id="0a33c-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="0a33c-125">/ Archivu/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="0a33c-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="0a33c-126">/ Archivu/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="0a33c-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="0a33c-127">/ Archivu nebo apple</span><span class="sxs-lookup"><span data-stu-id="0a33c-127">/Archive/apple</span></span>

<span data-ttu-id="0a33c-128">Vlastní trasy příchozí žádost se mapuje na řadič archivu a vyvolá akci Entry().</span><span class="sxs-lookup"><span data-stu-id="0a33c-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="0a33c-129">Při volání metody Entry() datum položky se předá jako parametr s názvem entryDate.</span><span class="sxs-lookup"><span data-stu-id="0a33c-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="0a33c-130">Můžete vytvořit vlastní trasy Blog pomocí řadiče v výpis 2.</span><span class="sxs-lookup"><span data-stu-id="0a33c-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="0a33c-131">**Výpis 2 - ArchiveController.cs**</span><span class="sxs-lookup"><span data-stu-id="0a33c-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="0a33c-132">Všimněte si, že metoda Entry() v výpis 2 přijímá parametr typu data a času.</span><span class="sxs-lookup"><span data-stu-id="0a33c-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="0a33c-133">Architektura MVC je dostatečně inteligentní automaticky převést vstupní data z adresy URL na hodnotu data a času.</span><span class="sxs-lookup"><span data-stu-id="0a33c-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="0a33c-134">Pokud parametr vstupní data z adresy URL nelze převést na typ DateTime, je vyvolána chyba (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="0a33c-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="0a33c-135">**Obrázek 1 – Chyba z převodu parametru**</span><span class="sxs-lookup"><span data-stu-id="0a33c-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="0a33c-136">[![Dialogové okno Nový projekt](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0a33c-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="0a33c-137">**Obrázek 01**: Chyba z převodu parametr ([Kliknutím zobrazit obrázek v plné velikosti](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="0a33c-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="0a33c-138">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0a33c-138">Summary</span></span>

<span data-ttu-id="0a33c-139">Cílem tohoto kurzu bylo ukazují, jak můžete vytvořit vlastní trasy.</span><span class="sxs-lookup"><span data-stu-id="0a33c-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="0a33c-140">Jste zjistili, jak přidat vlastní trasy do směrovací tabulky v souboru Global.asax, která představuje položky blogu.</span><span class="sxs-lookup"><span data-stu-id="0a33c-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="0a33c-141">Jsme probrali jak mapovat požadavky pro položky blogu řadič ArchiveController a s názvem Entry() akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0a33c-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0a33c-142">[Předchozí](aspnet-mvc-controllers-overview-cs.md)
> [další](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="0a33c-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>
