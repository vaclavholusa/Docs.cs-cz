---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Vytváření vlastní trasy omezení (C#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther ukazuje, jak můžete vytvořit vlastní trasy omezení. Implementaci jednoduchou vlastní omezení, které brání v dodržení trasa odpovídá w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 4c120a102b117433b6774f2ea7800f1c4a609f8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874340"
---
<a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="f45e5-104">Vytváření vlastní trasy omezení (C#)</span><span class="sxs-lookup"><span data-stu-id="f45e5-104">Creating a Custom Route Constraint (C#)</span></span>
====================
<span data-ttu-id="f45e5-105">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="f45e5-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="f45e5-106">Stephen Walther ukazuje, jak můžete vytvořit vlastní trasy omezení.</span><span class="sxs-lookup"><span data-stu-id="f45e5-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="f45e5-107">Můžeme implementovat jednoduchý vlastní omezení, které brání trasa se shoduje se, když prohlížeč požadavku ze vzdáleného počítače.</span><span class="sxs-lookup"><span data-stu-id="f45e5-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="f45e5-108">Cílem tohoto kurzu je ukazují, jak můžete vytvořit vlastní trasy omezení.</span><span class="sxs-lookup"><span data-stu-id="f45e5-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="f45e5-109">Omezení vlastní trasy umožňuje zabránit trasy probíhá shodná, pokud je nalezena shoda některé vlastní podmínky.</span><span class="sxs-lookup"><span data-stu-id="f45e5-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="f45e5-110">V tomto kurzu vytvoříme Localhost omezení trasy.</span><span class="sxs-lookup"><span data-stu-id="f45e5-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="f45e5-111">Omezení trasy Localhost pouze odpovídá požadavkům z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="f45e5-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="f45e5-112">Vzdálené žádosti z přes Internet není nalezena shoda.</span><span class="sxs-lookup"><span data-stu-id="f45e5-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="f45e5-113">Implementací rozhraní IRouteConstraint implementujete omezení vlastní trasy.</span><span class="sxs-lookup"><span data-stu-id="f45e5-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="f45e5-114">To je velmi jednoduché rozhraní, který je popsán způsob:</span><span class="sxs-lookup"><span data-stu-id="f45e5-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="f45e5-115">Metoda vrací logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f45e5-115">The method returns a Boolean value.</span></span> <span data-ttu-id="f45e5-116">Pokud jste vrátí hodnotu false, trasy přidružených k omezení nebude odpovídat požadavku prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="f45e5-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="f45e5-117">Výpis 1 je součástí omezení Localhost.</span><span class="sxs-lookup"><span data-stu-id="f45e5-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="f45e5-118">**Listing 1 - LocalhostConstraint.cs**</span><span class="sxs-lookup"><span data-stu-id="f45e5-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="f45e5-119">Omezení v výpis 1 využívá vlastnost IsLocal vystavené třídě požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="f45e5-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="f45e5-120">Tato vlastnost vrací hodnotu true, pokud se IP adresa požadavku je buď 127.0.0.1 nebo pokud IP adresa požadavku je stejný jako IP adresa serveru.</span><span class="sxs-lookup"><span data-stu-id="f45e5-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="f45e5-121">Můžete použít vlastní omezení v rámci trasy definované v souboru Global.asax.</span><span class="sxs-lookup"><span data-stu-id="f45e5-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="f45e5-122">Soubor Global.asax výpis 2 používá omezení Localhost zabránit uživatelům v požadavku stránky pro správu, pokud provádění požadavku z místního serveru.</span><span class="sxs-lookup"><span data-stu-id="f45e5-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="f45e5-123">Žádost o /Admin/DeleteAll se například nezdaří, když ze vzdáleného serveru.</span><span class="sxs-lookup"><span data-stu-id="f45e5-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="f45e5-124">**Listing 2 - Global.asax**</span><span class="sxs-lookup"><span data-stu-id="f45e5-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="f45e5-125">Omezení Localhost se používá v definici trasy, která správce.</span><span class="sxs-lookup"><span data-stu-id="f45e5-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="f45e5-126">Tato trasa nebude odpovídat požadavkem vzdáleného prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="f45e5-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="f45e5-127">Uvědomte si ale, že další trasy definované v souboru Global.asax může odpovídat stejném požadavku.</span><span class="sxs-lookup"><span data-stu-id="f45e5-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="f45e5-128">Je důležité si uvědomit, že omezení zabrání odpovídající žádost o konkrétní trasy a ne všechny trasy definované v souboru Global.asax.</span><span class="sxs-lookup"><span data-stu-id="f45e5-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="f45e5-129">Všimněte si, že výchozí trasu byla změněna na komentář ze souboru Global.asax v výpis 2.</span><span class="sxs-lookup"><span data-stu-id="f45e5-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="f45e5-130">Pokud zahrnete výchozí trasu, výchozí trasa odpovídá požadavky řadiče pro správu.</span><span class="sxs-lookup"><span data-stu-id="f45e5-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="f45e5-131">V takovém případě vzdálených uživatelů může stále vyvolání akce správce řadiče i v případě, že jejich požadavky nebude odpovídají správce trasy.</span><span class="sxs-lookup"><span data-stu-id="f45e5-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f45e5-132">[Předchozí](creating-a-route-constraint-cs.md)
> [další](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f45e5-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
