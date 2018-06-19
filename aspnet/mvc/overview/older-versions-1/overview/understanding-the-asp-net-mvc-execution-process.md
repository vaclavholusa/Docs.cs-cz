---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Principy spuštění procesu ASP.NET MVC | Microsoft Docs
author: microsoft
description: Zjistěte, jak rozhraní ASP.NET MVC zpracovává žádost prohlížeče krok za krokem.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26564523"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="c5c88-103">Principy spuštění procesu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c5c88-103">Understanding the ASP.NET MVC Execution Process</span></span>
====================
<span data-ttu-id="c5c88-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c5c88-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c5c88-105">Zjistěte, jak rozhraní ASP.NET MVC zpracovává žádost prohlížeče krok za krokem.</span><span class="sxs-lookup"><span data-stu-id="c5c88-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>


<span data-ttu-id="c5c88-106">První předání aplikaci založené na ASP.NET MVC webových požadavků **UrlRoutingModule** objekt, který je modul protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="c5c88-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="c5c88-107">Tento modul analyzuje požadavku a provede výběr trasy.</span><span class="sxs-lookup"><span data-stu-id="c5c88-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="c5c88-108">**UrlRoutingModule** objekt vybere první objekt trasy, který odpovídá aktuální požadavek.</span><span class="sxs-lookup"><span data-stu-id="c5c88-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="c5c88-109">(Objekt trasy je třída, která implementuje **RouteBase**, a je obvykle instance **trasy** třídy.) Pokud se shodují se žádné trasy, **UrlRoutingModule** objekt neprovede žádnou akci a vrátit zpět k regulární ASP.NET nebo služby IIS žádosti o zpracování žádosti.</span><span class="sxs-lookup"><span data-stu-id="c5c88-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="c5c88-110">Z vybraného **trasy** objekt, **UrlRoutingModule** získá objektu **IRouteHandler** objekt, který je přidružen **trasy**objektu.</span><span class="sxs-lookup"><span data-stu-id="c5c88-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="c5c88-111">Obvykle v aplikaci MVC to bude instance **MvcRouteHandler**.</span><span class="sxs-lookup"><span data-stu-id="c5c88-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="c5c88-112">**IRouteHandler** vytvoří instanci **IHttpHandler** objektu a předává je **IHttpContext** objektu.</span><span class="sxs-lookup"><span data-stu-id="c5c88-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="c5c88-113">Ve výchozím nastavení **IHttpHandler** instance pro MVC je **MvcHandler** objektu.</span><span class="sxs-lookup"><span data-stu-id="c5c88-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="c5c88-114">**MvcHandler** objekt vybere kontroler, který bude žádost zpracovat.</span><span class="sxs-lookup"><span data-stu-id="c5c88-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="c5c88-115">Při spuštění aplikace ASP.NET MVC Web ve službě IIS 7.0, je vyžadována pro projekty MVC bez přípony názvu souboru.</span><span class="sxs-lookup"><span data-stu-id="c5c88-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="c5c88-116">Ale ve službě IIS 6.0, obslužná rutina vyžaduje namapovat příponu názvu souboru .mvc na knihovnu DLL rozhraní ISAPI technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c5c88-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>


<span data-ttu-id="c5c88-117">Modul a obslužné rutiny jsou vstupní body rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c5c88-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="c5c88-118">Budou provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="c5c88-118">They perform the following actions:</span></span>

- <span data-ttu-id="c5c88-119">Vyberte příslušný řadič v MVC webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c5c88-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="c5c88-120">Získejte konkrétní řadič instance.</span><span class="sxs-lookup"><span data-stu-id="c5c88-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="c5c88-121">Volání kontroleru **Execute** metoda.</span><span class="sxs-lookup"><span data-stu-id="c5c88-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="c5c88-122">Je tomu u provádění na projekt MVC jsou následující:</span><span class="sxs-lookup"><span data-stu-id="c5c88-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="c5c88-123">Zobrazí první požadavek pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="c5c88-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="c5c88-124">V souboru Global.asax **trasy** objekty jsou přidány na **RouteTable** objektu.</span><span class="sxs-lookup"><span data-stu-id="c5c88-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="c5c88-125">Provést směrování</span><span class="sxs-lookup"><span data-stu-id="c5c88-125">Perform routing</span></span> 

    - <span data-ttu-id="c5c88-126">**UrlRoutingModule** používá modul pro první odpovídající **trasy** objekt v **RouteTable** kolekce k vytvoření **RouteData** objekt, který pak použije k vytvoření **kontext požadavku** (**IHttpContext**) objektu.</span><span class="sxs-lookup"><span data-stu-id="c5c88-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="c5c88-127">Vytvořte obslužnou rutinu požadavků MVC</span><span class="sxs-lookup"><span data-stu-id="c5c88-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="c5c88-128">**MvcRouteHandler** vytvoří instanci objektu **MvcHandler** třídy a předává je **kontext požadavku** instance.</span><span class="sxs-lookup"><span data-stu-id="c5c88-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="c5c88-129">Vytvoření řadiče</span><span class="sxs-lookup"><span data-stu-id="c5c88-129">Create controller</span></span> 

    - <span data-ttu-id="c5c88-130">**MvcHandler** objektu používá **kontext požadavku** instance k identifikaci **IControllerFactory** objektu (obvykle instance  **DefaultControllerFactory** třída) Chcete-li vytvořit instanci třídy controller s.</span><span class="sxs-lookup"><span data-stu-id="c5c88-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="c5c88-131">Execute řadič - **MvcHandler** instance volá řadičem s **Execute** metoda.</span><span class="sxs-lookup"><span data-stu-id="c5c88-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="c5c88-132">Vyvolání akce</span><span class="sxs-lookup"><span data-stu-id="c5c88-132">Invoke action</span></span> 

    - <span data-ttu-id="c5c88-133">Většina řadičů dědí **řadič** základní třídy.</span><span class="sxs-lookup"><span data-stu-id="c5c88-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="c5c88-134">Pro kontrolery, které uděláte tak, že **ControllerActionInvoker** určuje objekt, který je přidružen k řadiči, jakou metodu akce třídy controller volat a potom volá tuto metodu.</span><span class="sxs-lookup"><span data-stu-id="c5c88-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="c5c88-135">Výsledek spuštění</span><span class="sxs-lookup"><span data-stu-id="c5c88-135">Execute result</span></span> 

    - <span data-ttu-id="c5c88-136">Metoda typické akce může přijímat vstup uživatele, připravit odpovídající odpověď data a pak spusťte výsledek tak, že vrací typ výsledku.</span><span class="sxs-lookup"><span data-stu-id="c5c88-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="c5c88-137">Typy předdefinované výsledků, které mohou být provedeny patří: **ViewResult** (který vykreslí zobrazení a je většina často používané výsledný typ), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, a **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="c5c88-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
