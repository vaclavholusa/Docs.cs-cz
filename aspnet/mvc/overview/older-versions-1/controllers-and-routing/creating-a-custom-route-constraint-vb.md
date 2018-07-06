---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Vytvoření vlastního omezení trasy (VB) | Dokumentace Microsoftu
author: StephenWalther
description: Stephen Walther ukazuje, jak můžete vytvořit vlastní trasy omezení. Můžeme implementovat jednoduché vlastního omezení trasy brání odpovídající w...
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 72389d11467cbf7baea4cc9452266edb8ab81125
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840465"
---
<a name="creating-a-custom-route-constraint-vb"></a><span data-ttu-id="601f2-104">Vytvoření vlastního omezení trasy (VB)</span><span class="sxs-lookup"><span data-stu-id="601f2-104">Creating a Custom Route Constraint (VB)</span></span>
====================
<span data-ttu-id="601f2-105">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="601f2-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="601f2-106">Stephen Walther ukazuje, jak můžete vytvořit vlastní trasy omezení.</span><span class="sxs-lookup"><span data-stu-id="601f2-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="601f2-107">Implementujeme jednoduchý vlastní omezení, které brání trasy je nalezena shoda, po provedení požadavku prohlížeče ze vzdáleného počítače.</span><span class="sxs-lookup"><span data-stu-id="601f2-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="601f2-108">Cílem tohoto kurzu je předvést, jak můžete vytvořit vlastní trasy omezení.</span><span class="sxs-lookup"><span data-stu-id="601f2-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="601f2-109">Omezení vlastní trasy umožňuje zabránit trasy je nalezena shoda, pokud je nalezena shoda s některé vlastní podmínky.</span><span class="sxs-lookup"><span data-stu-id="601f2-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="601f2-110">V tomto kurzu vytvoříme Localhost omezení trasy.</span><span class="sxs-lookup"><span data-stu-id="601f2-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="601f2-111">Dané omezení trasy Localhost odpovídá pouze požadavky z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="601f2-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="601f2-112">Vzdálené žádosti z přes Internet není nalezena shoda.</span><span class="sxs-lookup"><span data-stu-id="601f2-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="601f2-113">Implementace omezení vlastní trasy implementací rozhraní IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="601f2-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="601f2-114">To je velmi jednoduché rozhraní, které popisuje jedinou metodu:</span><span class="sxs-lookup"><span data-stu-id="601f2-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="601f2-115">Metoda vrátí hodnotu typu Boolean.</span><span class="sxs-lookup"><span data-stu-id="601f2-115">The method returns a Boolean value.</span></span> <span data-ttu-id="601f2-116">Pokud se vrátit hodnotu False, nebude odpovídat přidružené k dané omezení trasy požadavek prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="601f2-116">If you return False, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="601f2-117">Výpis 1 je součástí omezení Localhost.</span><span class="sxs-lookup"><span data-stu-id="601f2-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="601f2-118">**Výpis 1 - LocalhostConstraint.vb**</span><span class="sxs-lookup"><span data-stu-id="601f2-118">**Listing 1 - LocalhostConstraint.vb**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="601f2-119">Omezení v informacích 1 využívá výhod vlastnost IsLocal vystavené třídy HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="601f2-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="601f2-120">Tato vlastnost vrací hodnotu true, pokud IP adresa požadavku je buď 127.0.0.1 nebo pokud IP adresa požadavku je stejné jako IP adresa serveru.</span><span class="sxs-lookup"><span data-stu-id="601f2-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="601f2-121">Můžete použít vlastní omezení v rámci trasy definované v souboru Global.asax.</span><span class="sxs-lookup"><span data-stu-id="601f2-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="601f2-122">Soubor Global.asax výpis 2 používá Localhost omezení a zabránit uživatelům požadování stránky pro správu, není-li využívají žádost z místního serveru.</span><span class="sxs-lookup"><span data-stu-id="601f2-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="601f2-123">Žádost o /Admin/DeleteAll se například nezdaří při provedení ze vzdáleného serveru.</span><span class="sxs-lookup"><span data-stu-id="601f2-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="601f2-124">**Listing 2 - Global.asax**</span><span class="sxs-lookup"><span data-stu-id="601f2-124">**Listing 2 - Global.asax**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="601f2-125">Omezení Localhost se používá v definici trasy, která správce.</span><span class="sxs-lookup"><span data-stu-id="601f2-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="601f2-126">Tato trasa nebude odpovídat podvýrazu žádost vzdálené prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="601f2-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="601f2-127">Uvědomte si však, že stejný požadavek může shodovat s další trasy definované v souboru Global.asax.</span><span class="sxs-lookup"><span data-stu-id="601f2-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="601f2-128">Je důležité pochopit, že omezení zabrání konkrétní trasu odpovídající požadavek a ne všechny trasy definované v souboru Global.asax.</span><span class="sxs-lookup"><span data-stu-id="601f2-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="601f2-129">Všimněte si, že výchozí trasa byla zakomentované ze souboru Global.asax ve výpisu 2.</span><span class="sxs-lookup"><span data-stu-id="601f2-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="601f2-130">Pokud zahrnete výchozí trasu, výchozí trasu odpovídají požadavky řadiče pro správu.</span><span class="sxs-lookup"><span data-stu-id="601f2-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="601f2-131">V takovém případě vzdáleným uživatelům může stále vyvolání akce kontroleru pro správce i když jejich žádosti o shodě správce trasy.</span><span class="sxs-lookup"><span data-stu-id="601f2-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="601f2-132">Předchozí</span><span class="sxs-lookup"><span data-stu-id="601f2-132">Previous</span></span>](creating-a-route-constraint-vb.md)
