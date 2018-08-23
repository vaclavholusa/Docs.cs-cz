---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Principy procesu spuštění ASP.NET MVC | Dokumentace Microsoftu
author: microsoft
description: Zjistěte, jak rozhraní ASP.NET MVC zpracovává žádost prohlížeče krok za krokem.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 3a3bcf4b78e3b19fb4d293f67b68c3a790d05221
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752010"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="d77e8-103">Principy procesu spuštění ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="d77e8-103">Understanding the ASP.NET MVC Execution Process</span></span>
====================
<span data-ttu-id="d77e8-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d77e8-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d77e8-105">Zjistěte, jak rozhraní ASP.NET MVC zpracovává žádost prohlížeče krok za krokem.</span><span class="sxs-lookup"><span data-stu-id="d77e8-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>


<span data-ttu-id="d77e8-106">Požadavky na aplikace založené na ASP.NET MVC nejdřív projít **UrlRoutingModule** objekt, který je modul HTTP.</span><span class="sxs-lookup"><span data-stu-id="d77e8-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="d77e8-107">Tento modul analyzuje požadavek a provádí výběr trasy.</span><span class="sxs-lookup"><span data-stu-id="d77e8-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="d77e8-108">**UrlRoutingModule** objekt vybere první objekt trasy, která odpovídá aktuální požadavek.</span><span class="sxs-lookup"><span data-stu-id="d77e8-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="d77e8-109">(Objekt trasy je třída, která implementuje **RouteBase**, a je obvykle instance **trasy** třídy.) Pokud neodpovídají žádné trasy **UrlRoutingModule** objekt nemá žádný účinek a umožňuje vrátit zpět k pravidelné žádosti ASP.NET nebo IIS zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="d77e8-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="d77e8-110">Z vybraného **trasy** objektu, **UrlRoutingModule** získává objekt **IRouteHandler** objekt, který je přidružený **trasy**objektu.</span><span class="sxs-lookup"><span data-stu-id="d77e8-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="d77e8-111">V aplikaci MVC to bude obvykle instance **MvcRouteHandler**.</span><span class="sxs-lookup"><span data-stu-id="d77e8-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="d77e8-112">**IRouteHandler** vytvoří instanci **IHttpHandler** objektu a předává je **IHttpContext** objektu.</span><span class="sxs-lookup"><span data-stu-id="d77e8-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="d77e8-113">Ve výchozím nastavení **IHttpHandler** instance pro MVC je **MvcHandler** objektu.</span><span class="sxs-lookup"><span data-stu-id="d77e8-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="d77e8-114">**MvcHandler** objekt potom vybere kontroler, který bude žádost zpracovat.</span><span class="sxs-lookup"><span data-stu-id="d77e8-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="d77e8-115">Při spuštění aplikace ASP.NET MVC Web ve službě IIS 7.0, bez přípony názvu souboru je vyžadována pro projekty MVC.</span><span class="sxs-lookup"><span data-stu-id="d77e8-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="d77e8-116">Ale ve službě IIS 6.0, obslužná rutina vyžaduje namapovat příponu názvu souboru .mvc na knihovnu DLL ISAPI technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d77e8-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>


<span data-ttu-id="d77e8-117">Modul a obslužné rutiny jsou vstupními body k rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d77e8-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="d77e8-118">Provádějí následující akce:</span><span class="sxs-lookup"><span data-stu-id="d77e8-118">They perform the following actions:</span></span>

- <span data-ttu-id="d77e8-119">Vyberte příslušný řadič v MVC webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d77e8-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="d77e8-120">Získání instance konkrétní kontroleru.</span><span class="sxs-lookup"><span data-stu-id="d77e8-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="d77e8-121">Volání kontroler **Execute** metody.</span><span class="sxs-lookup"><span data-stu-id="d77e8-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="d77e8-122">Následuje seznam fáze spuštění pro projekt MVC:</span><span class="sxs-lookup"><span data-stu-id="d77e8-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="d77e8-123">Zobrazí první požadavek pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="d77e8-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="d77e8-124">V souboru Global.asax **trasy** objekty jsou přidány do **směrovací tabulky** objektu.</span><span class="sxs-lookup"><span data-stu-id="d77e8-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="d77e8-125">Provést směrování</span><span class="sxs-lookup"><span data-stu-id="d77e8-125">Perform routing</span></span> 

    - <span data-ttu-id="d77e8-126">**UrlRoutingModule** modul použije první odpovídající **trasy** objekt **směrovací tabulky** kolekce k vytvoření **RouteData** objekt, který pak použije k vytvoření **RequestContext** (**IHttpContext**) objektu.</span><span class="sxs-lookup"><span data-stu-id="d77e8-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="d77e8-127">Vytvořte obslužnou rutinu požadavků MVC</span><span class="sxs-lookup"><span data-stu-id="d77e8-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="d77e8-128">**MvcRouteHandler** vytvoří instanci objektu **MvcHandler** třídy a předává je **RequestContext** instance.</span><span class="sxs-lookup"><span data-stu-id="d77e8-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="d77e8-129">Vytvoření kontroleru</span><span class="sxs-lookup"><span data-stu-id="d77e8-129">Create controller</span></span> 

    - <span data-ttu-id="d77e8-130">**MvcHandler** objektu používá **RequestContext** k identifikaci instance **IControllerFactory** objektu (obvykle instance  **DefaultControllerFactory** třídy) k vytvoření instance kontroleru se.</span><span class="sxs-lookup"><span data-stu-id="d77e8-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="d77e8-131">Spustit kontroler – **MvcHandler** instance volá řadiče s **Execute** – metoda.</span><span class="sxs-lookup"><span data-stu-id="d77e8-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="d77e8-132">Vyvolání akce</span><span class="sxs-lookup"><span data-stu-id="d77e8-132">Invoke action</span></span> 

    - <span data-ttu-id="d77e8-133">Většina řadičů dědí **řadič** základní třídy.</span><span class="sxs-lookup"><span data-stu-id="d77e8-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="d77e8-134">Pro kontrolery, které uděláte to tak **ControllerActionInvoker** objekt, který je spojen s řadičem Určuje, jakou metodu akce kontroleru třídy volat a pak volá tuto metodu.</span><span class="sxs-lookup"><span data-stu-id="d77e8-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="d77e8-135">Spuštění výsledku</span><span class="sxs-lookup"><span data-stu-id="d77e8-135">Execute result</span></span> 

    - <span data-ttu-id="d77e8-136">Metoda typické akce může přijímat uživatelský vstup, připravit data odpovídající odpověď a následné provádění výsledek tak, že vrací typ výsledku.</span><span class="sxs-lookup"><span data-stu-id="d77e8-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="d77e8-137">Typy integrovaných výsledků, které mohou být provedeny patří následující: **ViewResult** (který vykreslí zobrazení a je většina často používaný výsledným typem), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, a **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="d77e8-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
