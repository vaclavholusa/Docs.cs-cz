---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: "Přehled směrování rozhraní ASP.NET MVC (VB) | Microsoft Docs"
author: StephenWalther
description: "V tomto kurzu Stephen Walther ukazuje, jak rozhraní ASP.NET MVC mapuje požadavky prohlížeče akce kontroleru."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 1e4c74e61b1a0d5f5020154756e34dd2fa507034
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-routing-overview-vb"></a><span data-ttu-id="89b06-103">Přehled směrování rozhraní ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="89b06-103">ASP.NET MVC Routing Overview (VB)</span></span>
====================
<span data-ttu-id="89b06-104">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="89b06-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="89b06-105">V tomto kurzu Stephen Walther ukazuje, jak rozhraní ASP.NET MVC mapuje požadavky prohlížeče akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="89b06-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="89b06-106">V tomto kurzu jsou zavedené k důležitou součást každých aplikace ASP.NET MVC s názvem *směrování ASP.NET*.</span><span class="sxs-lookup"><span data-stu-id="89b06-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="89b06-107">Modul Směrování ASP.NET je zodpovědná za mapování příchozích požadavků prohlížeče na konkrétní akce kontroleru MVC.</span><span class="sxs-lookup"><span data-stu-id="89b06-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="89b06-108">Na konci tohoto kurzu bude pochopit, jak standardní směrovací tabulka mapuje požadavky akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="89b06-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="89b06-109">Pomocí výchozí směrovací tabulka</span><span class="sxs-lookup"><span data-stu-id="89b06-109">Using the Default Route Table</span></span>

<span data-ttu-id="89b06-110">Když vytvoříte novou aplikaci ASP.NET MVC, aplikace již byla konfigurována pro použití směrování ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="89b06-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="89b06-111">Směrování ASP.NET je nastavená na dvou místech.</span><span class="sxs-lookup"><span data-stu-id="89b06-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="89b06-112">Nejprve směrování ASP.NET je povolený v souboru konfigurace webové aplikace (soubor Web.config).</span><span class="sxs-lookup"><span data-stu-id="89b06-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="89b06-113">Existují čtyři části v konfiguračním souboru, které jsou relevantní pro směrování: části system.web.httpModules, v části system.web.httpHandlers, v části system.webserver.modules a v části system.webserver.handlers.</span><span class="sxs-lookup"><span data-stu-id="89b06-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="89b06-114">Dejte pozor, abyste tyto části odstranit, protože bez těchto částí směrování přestane fungovat.</span><span class="sxs-lookup"><span data-stu-id="89b06-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="89b06-115">Druhý a důležitější je v souboru Global.asax aplikace vytvoří směrovací tabulku.</span><span class="sxs-lookup"><span data-stu-id="89b06-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="89b06-116">Soubor Global.asax je speciální soubor, který obsahuje obslužné rutiny události pro události životního cyklu aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="89b06-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="89b06-117">Směrovací tabulka je vytvořen během události spustit aplikace.</span><span class="sxs-lookup"><span data-stu-id="89b06-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="89b06-118">Soubor v výpis 1 obsahuje výchozí soubor Global.asax pro aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="89b06-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="89b06-119">**Výpis 1 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="89b06-119">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

<span data-ttu-id="89b06-120">Při prvním spuštění aplikace MVC, aplikace\_Start() metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="89b06-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="89b06-121">Tuto metodu, volá metodu RegisterRoutes().</span><span class="sxs-lookup"><span data-stu-id="89b06-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="89b06-122">Metoda RegisterRoutes() vytvoří směrovací tabulka.</span><span class="sxs-lookup"><span data-stu-id="89b06-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="89b06-123">Výchozí směrovací tabulka obsahuje jednu trasu (s názvem výchozí).</span><span class="sxs-lookup"><span data-stu-id="89b06-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="89b06-124">Výchozí trasu mapy první segment adresy URL názvu kontroleru, druhý segment adresy URL k akci kontroleru a třetí segment, který má parametr s názvem **id**.</span><span class="sxs-lookup"><span data-stu-id="89b06-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="89b06-125">Představte si, zadejte následující adresu URL do adresního řádku webového prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="89b06-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="89b06-126">/ Home/Index nebo 3</span><span class="sxs-lookup"><span data-stu-id="89b06-126">/Home/Index/3</span></span>

<span data-ttu-id="89b06-127">Výchozí trasu mapuje tuto adresu URL následující parametry:</span><span class="sxs-lookup"><span data-stu-id="89b06-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="89b06-128">Řadič = Domů</span><span class="sxs-lookup"><span data-stu-id="89b06-128">controller = Home</span></span>

- <span data-ttu-id="89b06-129">Akce = indexu</span><span class="sxs-lookup"><span data-stu-id="89b06-129">action = Index</span></span>

- <span data-ttu-id="89b06-130">ID = 3</span><span class="sxs-lookup"><span data-stu-id="89b06-130">id = 3</span></span>

<span data-ttu-id="89b06-131">Pokud budete požadovat adresy URL/Home nebo Index nebo 3, se spustí následující kód:</span><span class="sxs-lookup"><span data-stu-id="89b06-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="89b06-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="89b06-132">HomeController.Index(3)</span></span>

<span data-ttu-id="89b06-133">Výchozí trasu zahrnuje výchozí hodnoty pro všechny tři parametry.</span><span class="sxs-lookup"><span data-stu-id="89b06-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="89b06-134">Pokud nezadáte řadič, pak parametr řadiče výchozí hodnotou je hodnota **Domů**.</span><span class="sxs-lookup"><span data-stu-id="89b06-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="89b06-135">Pokud nezadáte akce, parametr akce výchozí hodnotou je hodnota **Index**.</span><span class="sxs-lookup"><span data-stu-id="89b06-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="89b06-136">Nakonec Pokud nezadáte id, parametr id výchozí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="89b06-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="89b06-137">Podívejme se na několik příkladů způsobu výchozí trasu mapování adresy URL pro akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="89b06-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="89b06-138">Představte si, zadejte následující adresu URL do adresního řádku prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="89b06-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="89b06-139">Domů</span><span class="sxs-lookup"><span data-stu-id="89b06-139">/Home</span></span>

<span data-ttu-id="89b06-140">Z důvodu výchozí hodnoty výchozí trasy parametr zadáte tuto adresu URL způsobí, že metoda Index() HomeController třídy v výpis 2, která se má volat.</span><span class="sxs-lookup"><span data-stu-id="89b06-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="89b06-141">**Výpis 2 - HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="89b06-141">**Listing 2 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

<span data-ttu-id="89b06-142">Výpis 2 třída HomeController zahrnuje metodu s názvem Index(), který přijímá jeden parametr s názvem ID. Adresa URL/Home způsobí, že metoda Index() volat s hodnotou nic jako hodnotu parametru Id.</span><span class="sxs-lookup"><span data-stu-id="89b06-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with the value Nothing as the value of the Id parameter.</span></span>

<span data-ttu-id="89b06-143">Kvůli způsobu, že rozhraní MVC volá akce kontroleru adresy URL/Home také odpovídající metodu Index() třídy HomeController ve výpisu 3.</span><span class="sxs-lookup"><span data-stu-id="89b06-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="89b06-144">**Výpis 3 - HomeController.vb (indexu akce s žádný parametr)**</span><span class="sxs-lookup"><span data-stu-id="89b06-144">**Listing 3 - HomeController.vb (Index action with no parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

<span data-ttu-id="89b06-145">Metoda Index() ve výpisu 3 nepřijímá žádné parametry.</span><span class="sxs-lookup"><span data-stu-id="89b06-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="89b06-146">Adresa URL/Home způsobí, že tato metoda Index(), která se má volat.</span><span class="sxs-lookup"><span data-stu-id="89b06-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="89b06-147">Adresa URL/Home/indexu/3 také vyvolá tuto metodu (Id bude ignorován).</span><span class="sxs-lookup"><span data-stu-id="89b06-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="89b06-148">Adresa URL/Home také odpovídající metodu Index() třídy HomeController ve výpisu 4.</span><span class="sxs-lookup"><span data-stu-id="89b06-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="89b06-149">**Výpis 4 - HomeController.vb (indexu akce s parametrem s možnou hodnotou Null)**</span><span class="sxs-lookup"><span data-stu-id="89b06-149">**Listing 4 - HomeController.vb (Index action with nullable parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

<span data-ttu-id="89b06-150">Výpis 4 metodu Index() má jeden parametr celé číslo.</span><span class="sxs-lookup"><span data-stu-id="89b06-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="89b06-151">Protože parametr je parametr s možnou hodnotou Null (může mít hodnotu Nothing), je možné volat Index() bez vyvolání k chybě.</span><span class="sxs-lookup"><span data-stu-id="89b06-151">Because the parameter is a nullable parameter (can have the value Nothing), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="89b06-152">Nakonec volání metody Index() v výpis 5 s adresy URL/Home dojde k výjimce od parametr Id *není* parametr hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="89b06-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="89b06-153">Pokud se pokusíte k vyvolání metody Index() zobrazí chyba zobrazené na obrázku 1.</span><span class="sxs-lookup"><span data-stu-id="89b06-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="89b06-154">**Výpis 5 - HomeController.vb (indexu akce s parametrem Id)**</span><span class="sxs-lookup"><span data-stu-id="89b06-154">**Listing 5 - HomeController.vb (Index action with Id parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


<span data-ttu-id="89b06-155">[![Vyvolání akce kontroleru, která očekává hodnotu parametru](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="89b06-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="89b06-156">**Obrázek 01**: vyvolání akce kontroleru, která očekává hodnotu parametru ([Kliknutím zobrazit obrázek v plné velikosti](asp-net-mvc-routing-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="89b06-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="89b06-157">Adresa URL/Home nebo Index nebo 3, na druhé straně funguje správně, pomocí akce kontroleru indexu v výpis 5.</span><span class="sxs-lookup"><span data-stu-id="89b06-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="89b06-158">Žádost o /Home/Index/3 způsobí, že metoda Index() volat s parametrem Id, který má hodnotu 3.</span><span class="sxs-lookup"><span data-stu-id="89b06-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="89b06-159">Souhrn</span><span class="sxs-lookup"><span data-stu-id="89b06-159">Summary</span></span>

<span data-ttu-id="89b06-160">Cílem tohoto kurzu bylo poskytnout stručný úvod do směrování ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="89b06-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="89b06-161">Jsme se zaměřili na výchozí směrovací tabulka, kterou můžete získat pomocí nové aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="89b06-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="89b06-162">Jste se dozvěděli, jak výchozí trasu mapuje adresy URL akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="89b06-162">You learned how the default route maps URLs to controller actions.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="89b06-163">[Předchozí](creating-an-action-cs.md)
[další](understanding-action-filters-vb.md)</span><span class="sxs-lookup"><span data-stu-id="89b06-163">[Previous](creating-an-action-cs.md)
[Next](understanding-action-filters-vb.md)</span></span>
