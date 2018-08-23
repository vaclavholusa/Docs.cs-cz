---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: ASP.NET MVC – Přehled směrování (VB) | Dokumentace Microsoftu
author: StephenWalther
description: V tomto kurzu Stephen Walther ukazuje, jak rozhraní ASP.NET MVC mapuje požadavky prohlížeče na akce kontroleru.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: c20626b24e43031fc4103365396fc26b6a6daf93
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755710"
---
<a name="aspnet-mvc-routing-overview-vb"></a><span data-ttu-id="8d90e-103">ASP.NET MVC – Přehled směrování (VB)</span><span class="sxs-lookup"><span data-stu-id="8d90e-103">ASP.NET MVC Routing Overview (VB)</span></span>
====================
<span data-ttu-id="8d90e-104">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="8d90e-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="8d90e-105">V tomto kurzu Stephen Walther ukazuje, jak rozhraní ASP.NET MVC mapuje požadavky prohlížeče na akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8d90e-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="8d90e-106">V tomto kurzu jste se seznámili s důležitou funkcí každou aplikaci ASP.NET MVC s názvem *směrování ASP.NET*.</span><span class="sxs-lookup"><span data-stu-id="8d90e-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="8d90e-107">Modul Směrování ASP.NET je zodpovědná za mapování příchozích požadavků prohlížeče na konkrétní akce kontroleru MVC.</span><span class="sxs-lookup"><span data-stu-id="8d90e-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="8d90e-108">Na konci tohoto kurzu se seznámíte s jak standardní směrovací tabulka mapuje požadavky na akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8d90e-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="8d90e-109">Pomocí výchozí směrovací tabulka</span><span class="sxs-lookup"><span data-stu-id="8d90e-109">Using the Default Route Table</span></span>

<span data-ttu-id="8d90e-110">Při vytváření nové aplikace ASP.NET MVC, aplikace je již nakonfigurována pro použití směrování ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8d90e-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="8d90e-111">Směrování ASP.NET je nastavená na dvou místech.</span><span class="sxs-lookup"><span data-stu-id="8d90e-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="8d90e-112">Nejprve směrování ASP.NET je povolena v souboru konfigurace webové aplikace (soubor Web.config).</span><span class="sxs-lookup"><span data-stu-id="8d90e-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="8d90e-113">Existují čtyři oddíly v konfiguračním souboru, které souvisí se směrováním: části system.web.httpModules, části system.web.httpHandlers, části system.webserver.modules a system.webserver.handlers oddílu.</span><span class="sxs-lookup"><span data-stu-id="8d90e-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="8d90e-114">Dejte pozor, abyste odstranit tyto oddíly, protože bez těchto oddílů směrování se už nebude fungovat.</span><span class="sxs-lookup"><span data-stu-id="8d90e-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="8d90e-115">Za druhé a důležitější je se vytvoří směrovací tabulku v souboru Global.asax.</span><span class="sxs-lookup"><span data-stu-id="8d90e-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="8d90e-116">Soubor Global.asax je zvláštní soubor, který obsahuje obslužné rutiny událostí pro události životního cyklu aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8d90e-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="8d90e-117">Směrovací tabulka se vytvoří během události spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="8d90e-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="8d90e-118">Soubor výpisu 1 obsahuje výchozí soubor Global.asax pro aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8d90e-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="8d90e-119">**Listing 1 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="8d90e-119">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

<span data-ttu-id="8d90e-120">Při prvním spuštění aplikace MVC, aplikace\_volání metody Start().</span><span class="sxs-lookup"><span data-stu-id="8d90e-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="8d90e-121">Tato metoda pak volá metodu RegisterRoutes().</span><span class="sxs-lookup"><span data-stu-id="8d90e-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="8d90e-122">Metoda RegisterRoutes() vytvoří směrovací tabulku.</span><span class="sxs-lookup"><span data-stu-id="8d90e-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="8d90e-123">Výchozí směrovací tabulka obsahuje jednu trasu (výchozí).</span><span class="sxs-lookup"><span data-stu-id="8d90e-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="8d90e-124">Výchozí trasu první segment adresy URL odpovídá názvu kontroleru, druhý segment adresy URL pro akce kontroleru a třetí parametr s názvem segmentu **id**.</span><span class="sxs-lookup"><span data-stu-id="8d90e-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="8d90e-125">Představte si, zadejte následující adresu URL do adresního řádku webového prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="8d90e-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="8d90e-126">/ Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="8d90e-126">/Home/Index/3</span></span>

<span data-ttu-id="8d90e-127">Výchozí trasu mapuje tuto adresu URL na následující parametry:</span><span class="sxs-lookup"><span data-stu-id="8d90e-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="8d90e-128">Kontroler = Home</span><span class="sxs-lookup"><span data-stu-id="8d90e-128">controller = Home</span></span>

- <span data-ttu-id="8d90e-129">Akce = Index</span><span class="sxs-lookup"><span data-stu-id="8d90e-129">action = Index</span></span>

- <span data-ttu-id="8d90e-130">id = 3</span><span class="sxs-lookup"><span data-stu-id="8d90e-130">id = 3</span></span>

<span data-ttu-id="8d90e-131">Pokud budete požadovat adresy URL/Home/Index/3, následující kód se spustí:</span><span class="sxs-lookup"><span data-stu-id="8d90e-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="8d90e-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="8d90e-132">HomeController.Index(3)</span></span>

<span data-ttu-id="8d90e-133">Výchozí trasa obsahuje výchozí hodnoty pro všemi třemi parametry.</span><span class="sxs-lookup"><span data-stu-id="8d90e-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="8d90e-134">Pokud nezadáte kontroleru, pak řadič parametr výchozí hodnotu k hodnotě **Domů**.</span><span class="sxs-lookup"><span data-stu-id="8d90e-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="8d90e-135">Pokud nezadáte akci, parametr akce výchozí hodnotou je hodnota **Index**.</span><span class="sxs-lookup"><span data-stu-id="8d90e-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="8d90e-136">Nakonec, pokud nezadáte id, id parametr výchozí hodnotu prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="8d90e-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="8d90e-137">Podívejme se na několik příkladů způsobu výchozí trasu mapování adres URL na akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8d90e-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="8d90e-138">Představte si, zadejte následující adresu URL do adresního řádku prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="8d90e-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="8d90e-139">Domů</span><span class="sxs-lookup"><span data-stu-id="8d90e-139">/Home</span></span>

<span data-ttu-id="8d90e-140">Z důvodu parametr výchozí trasy zadáte tuto adresu URL způsobí, že metoda Index() třídy HomeController v výpis 2, která se má volat.</span><span class="sxs-lookup"><span data-stu-id="8d90e-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="8d90e-141">**Výpis 2 - HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="8d90e-141">**Listing 2 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

<span data-ttu-id="8d90e-142">Výpis 2 HomeController třídy obsahuje metodu s názvem Index(), která přijímá jeden parametr s názvem ID. Adresa URL/Home způsobí, že metoda Index() volat s hodnotou Nothing jako hodnotu parametru Id.</span><span class="sxs-lookup"><span data-stu-id="8d90e-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with the value Nothing as the value of the Id parameter.</span></span>

<span data-ttu-id="8d90e-143">Vzhledem ke způsobu, že rozhraní MVC volá akce kontroleru adresy URL/Home také odpovídající metodu Index() HomeController třídy v informacích 3.</span><span class="sxs-lookup"><span data-stu-id="8d90e-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="8d90e-144">**Výpis 3 - HomeController.vb (Index akce s žádný parametr)**</span><span class="sxs-lookup"><span data-stu-id="8d90e-144">**Listing 3 - HomeController.vb (Index action with no parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

<span data-ttu-id="8d90e-145">Metoda Index() ve výpisu 3 nepřijímá žádné parametry.</span><span class="sxs-lookup"><span data-stu-id="8d90e-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="8d90e-146">Adresa URL/Home způsobí, že tato metoda Index() volat.</span><span class="sxs-lookup"><span data-stu-id="8d90e-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="8d90e-147">Adresa URL/Home/Index/3 také vyvolá tuto metodu (Id se ignoruje).</span><span class="sxs-lookup"><span data-stu-id="8d90e-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="8d90e-148">Adresa URL/Home také odpovídající metodu Index() HomeController třídy v informacích 4.</span><span class="sxs-lookup"><span data-stu-id="8d90e-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="8d90e-149">**Část 4 – HomeController.vb (Index akce pomocí parametru s možnou hodnotou Null)**</span><span class="sxs-lookup"><span data-stu-id="8d90e-149">**Listing 4 - HomeController.vb (Index action with nullable parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

<span data-ttu-id="8d90e-150">V zobrazení 4 Index() metoda má jeden parametr celé číslo.</span><span class="sxs-lookup"><span data-stu-id="8d90e-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="8d90e-151">Protože parametr je parametr s možnou hodnotou Null (může mít hodnotu Nothing), Index() lze volat bez vyvolání k chybě.</span><span class="sxs-lookup"><span data-stu-id="8d90e-151">Because the parameter is a nullable parameter (can have the value Nothing), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="8d90e-152">Volání metody Index() výpis 5 pomocí adresy URL/Home nakonec dojde k výjimce od parametru Id *není* parametru s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="8d90e-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="8d90e-153">Při pokusu o volání metody Index() zobrazí chyba zobrazí na obrázku 1.</span><span class="sxs-lookup"><span data-stu-id="8d90e-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="8d90e-154">**Výpis 5 - HomeController.vb (Index akce s parametrem Id)**</span><span class="sxs-lookup"><span data-stu-id="8d90e-154">**Listing 5 - HomeController.vb (Index action with Id parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


<span data-ttu-id="8d90e-155">[![Vyvolání akce kontroleru, který očekává, že hodnota parametru](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8d90e-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="8d90e-156">**Obrázek 01**: vyvolání akce kontroleru, který očekává, že hodnota parametru ([kliknutím ji zobrazíte obrázek v plné velikosti](asp-net-mvc-routing-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="8d90e-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="8d90e-157">Adresy URL/Home/Index/3, na druhé straně funguje jenom s akce kontroleru indexu v informacích 5.</span><span class="sxs-lookup"><span data-stu-id="8d90e-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="8d90e-158">Žádost o /Home/Index/3 způsobí, že metoda Index() nelze volat s parametrem Id, který má hodnotu 3.</span><span class="sxs-lookup"><span data-stu-id="8d90e-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="8d90e-159">Souhrn</span><span class="sxs-lookup"><span data-stu-id="8d90e-159">Summary</span></span>

<span data-ttu-id="8d90e-160">Cílem tohoto kurzu bylo poskytne stručný úvod do směrování ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8d90e-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="8d90e-161">Jsme se zaměřili na výchozí směrovací tabulka, která získáte pomocí nové aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8d90e-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="8d90e-162">Jste se naučili, jak mapuje výchozí trasu adresy URL na akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8d90e-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8d90e-163">[Předchozí](creating-an-action-cs.md)
> [další](understanding-action-filters-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8d90e-163">[Previous](creating-an-action-cs.md)
[Next](understanding-action-filters-vb.md)</span></span>
