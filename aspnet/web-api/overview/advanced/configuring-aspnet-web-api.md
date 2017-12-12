---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: "Konfigurace rozhraní ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 1c007c4c327b7cde6ff52c6b0022acdff3c9b137
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="configuring-aspnet-web-api-2"></a><span data-ttu-id="a6be3-102">Konfigurace rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="a6be3-102">Configuring ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="a6be3-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a6be3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a6be3-104">Toto téma popisuje postup konfigurace webového rozhraní API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a6be3-104">This topic describes how to configure ASP.NET Web API.</span></span>

- [<span data-ttu-id="a6be3-105">Nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="a6be3-105">Configuration Settings</span></span>](#settings)
- [<span data-ttu-id="a6be3-106">Konfigurace webového rozhraní API s hostování prostředí ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a6be3-106">Configuring Web API with ASP.NET Hosting</span></span>](#webhost)
- [<span data-ttu-id="a6be3-107">Konfiguraci webového rozhraní API pomocí vlastní hostování OWIN</span><span class="sxs-lookup"><span data-stu-id="a6be3-107">Configuring Web API with OWIN Self-Hosting</span></span>](#selfhost)
- [<span data-ttu-id="a6be3-108">Globální webové rozhraní API služby</span><span class="sxs-lookup"><span data-stu-id="a6be3-108">Global Web API Services</span></span>](#services)
- [<span data-ttu-id="a6be3-109">Konfigurace Kontroleru</span><span class="sxs-lookup"><span data-stu-id="a6be3-109">Per-Controller Configuration</span></span>](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a><span data-ttu-id="a6be3-110">Nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="a6be3-110">Configuration Settings</span></span>

<span data-ttu-id="a6be3-111">Nastavení konfigurace webového rozhraní API jsou definovány v [HttpConfiguration](https://msdn.microsoft.com/en-us/library/system.web.http.httpconfiguration.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="a6be3-111">Web API configuration setttings are defined in the [HttpConfiguration](https://msdn.microsoft.com/en-us/library/system.web.http.httpconfiguration.aspx) class.</span></span>

| <span data-ttu-id="a6be3-112">Člen</span><span class="sxs-lookup"><span data-stu-id="a6be3-112">Member</span></span> | <span data-ttu-id="a6be3-113">Popis</span><span class="sxs-lookup"><span data-stu-id="a6be3-113">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a6be3-114">**Překladače závislostí**</span><span class="sxs-lookup"><span data-stu-id="a6be3-114">**DependencyResolver**</span></span> | <span data-ttu-id="a6be3-115">Umožňuje vkládání závislostí pro řadiče.</span><span class="sxs-lookup"><span data-stu-id="a6be3-115">Enables dependency injection for controllers.</span></span> <span data-ttu-id="a6be3-116">V tématu [pomocí překladače závislostí webového rozhraní API](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="a6be3-116">See [Using the Web API Dependency Resolver](dependency-injection.md).</span></span> |
| <span data-ttu-id="a6be3-117">**Filtry**</span><span class="sxs-lookup"><span data-stu-id="a6be3-117">**Filters**</span></span> | <span data-ttu-id="a6be3-118">Filtry akcí.</span><span class="sxs-lookup"><span data-stu-id="a6be3-118">Action filters.</span></span> |
| <span data-ttu-id="a6be3-119">**Formátovací moduly**</span><span class="sxs-lookup"><span data-stu-id="a6be3-119">**Formatters**</span></span> | <span data-ttu-id="a6be3-120">[Formátovací moduly typu média](../formats-and-model-binding/media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="a6be3-120">[Media-type formatters](../formats-and-model-binding/media-formatters.md).</span></span> |
| <span data-ttu-id="a6be3-121">**IncludeErrorDetailPolicy**</span><span class="sxs-lookup"><span data-stu-id="a6be3-121">**IncludeErrorDetailPolicy**</span></span> | <span data-ttu-id="a6be3-122">Určuje, zda server by měla obsahovat podrobnosti o chybě, například zprávy o výjimkách nebo trasování zásobníku zpráv odpovědí HTTP.</span><span class="sxs-lookup"><span data-stu-id="a6be3-122">Specifies whether the server should include error details, such as exception messages and stack traces, in HTTP response messages.</span></span> <span data-ttu-id="a6be3-123">V tématu [IncludeErrorDetailPolicy](https://msdn.microsoft.com/en-us/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span><span class="sxs-lookup"><span data-stu-id="a6be3-123">See [IncludeErrorDetailPolicy](https://msdn.microsoft.com/en-us/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span></span> |
| <span data-ttu-id="a6be3-124">**Inicializátor**</span><span class="sxs-lookup"><span data-stu-id="a6be3-124">**Initializer**</span></span> | <span data-ttu-id="a6be3-125">Funkci, která provede konečnou inicializaci **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="a6be3-125">A function that performs final initialization of the **HttpConfiguration**.</span></span> |
| <span data-ttu-id="a6be3-126">**MessageHandlers**</span><span class="sxs-lookup"><span data-stu-id="a6be3-126">**MessageHandlers**</span></span> | <span data-ttu-id="a6be3-127">[Obslužné rutiny zpráv HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="a6be3-127">[HTTP message handlers](http-message-handlers.md).</span></span> |
| <span data-ttu-id="a6be3-128">**ParameterBindingRules**</span><span class="sxs-lookup"><span data-stu-id="a6be3-128">**ParameterBindingRules**</span></span> | <span data-ttu-id="a6be3-129">Kolekce pravidel pro vazby parametrů na akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="a6be3-129">A collection of rules for binding parameters on controller actions.</span></span> |
| <span data-ttu-id="a6be3-130">**Vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="a6be3-130">**Properties**</span></span> | <span data-ttu-id="a6be3-131">Obecná vlastnost kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a6be3-131">A generic property bag.</span></span> |
| <span data-ttu-id="a6be3-132">**Trasy**</span><span class="sxs-lookup"><span data-stu-id="a6be3-132">**Routes**</span></span> | <span data-ttu-id="a6be3-133">Kolekce tras.</span><span class="sxs-lookup"><span data-stu-id="a6be3-133">The collection of routes.</span></span> <span data-ttu-id="a6be3-134">V tématu [směrování v rozhraní ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="a6be3-134">See [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="a6be3-135">**Služby**</span><span class="sxs-lookup"><span data-stu-id="a6be3-135">**Services**</span></span> | <span data-ttu-id="a6be3-136">Kolekce služeb.</span><span class="sxs-lookup"><span data-stu-id="a6be3-136">The collection of services.</span></span> <span data-ttu-id="a6be3-137">V tématu [služby](#services).</span><span class="sxs-lookup"><span data-stu-id="a6be3-137">See [Services](#services).</span></span> |


## <a name="prerequisites"></a><span data-ttu-id="a6be3-138">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a6be3-138">Prerequisites</span></span>

<span data-ttu-id="a6be3-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional nebo Enterprise Edition.</span><span class="sxs-lookup"><span data-stu-id="a6be3-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition.</span></span>

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a><span data-ttu-id="a6be3-140">Konfigurace webového rozhraní API s hostování prostředí ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a6be3-140">Configuring Web API with ASP.NET Hosting</span></span>

<span data-ttu-id="a6be3-141">V aplikaci ASP.NET, nakonfigurujte rozhraní Web API voláním [GlobalConfiguration.Configure](https://msdn.microsoft.com/en-us/library/system.web.http.globalconfiguration.configure.aspx) v **aplikace\_spustit** metoda.</span><span class="sxs-lookup"><span data-stu-id="a6be3-141">In an ASP.NET application, configure Web API by calling [GlobalConfiguration.Configure](https://msdn.microsoft.com/en-us/library/system.web.http.globalconfiguration.configure.aspx) in the **Application\_Start** method.</span></span> <span data-ttu-id="a6be3-142">**Konfigurace** metoda přebírá delegáta pomocí jediného parametru typu **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="a6be3-142">The **Configure** method takes a delegate with a single parameter of type **HttpConfiguration**.</span></span> <span data-ttu-id="a6be3-143">Všechny vaše konfiguračním uvnitř delegát proveďte.</span><span class="sxs-lookup"><span data-stu-id="a6be3-143">Perform all of your configuation inside the delegate.</span></span>

<span data-ttu-id="a6be3-144">Tady je příklad použití delegáta anonymní:</span><span class="sxs-lookup"><span data-stu-id="a6be3-144">Here is an example using an anonymous delegate:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="a6be3-145">V aplikaci Visual Studio 2017 šablona projektu "webové aplikace ASP.NET" automaticky nastaví kód konfigurace, pokud zvolíte možnost "Webového rozhraní API" v **nový projekt ASP.NET** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a6be3-145">In Visual Studio 2017, the "ASP.NET Web Application" project template automatically sets up the configuration code, if you select "Web API" in the **New ASP.NET Project** dialog.</span></span>

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

<span data-ttu-id="a6be3-146">Šablona projektu vytvoří soubor s názvem WebApiConfig.cs uvnitř aplikace\_spouštěcí složka.</span><span class="sxs-lookup"><span data-stu-id="a6be3-146">The project template creates a file named WebApiConfig.cs inside the App\_Start folder.</span></span> <span data-ttu-id="a6be3-147">Tento soubor kód definuje delegáta, kde byste měli umístit kódu webového rozhraní API konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a6be3-147">This code file defines the delegate where you should put your Web API configuration code.</span></span>

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

<span data-ttu-id="a6be3-148">Šablona projektu také přidá kód, který vyvolá delegáta z **aplikace\_spustit**.</span><span class="sxs-lookup"><span data-stu-id="a6be3-148">The project template also adds the code that calls the delegate from **Application\_Start**.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a><span data-ttu-id="a6be3-149">Konfiguraci webového rozhraní API pomocí vlastní hostování OWIN</span><span class="sxs-lookup"><span data-stu-id="a6be3-149">Configuring Web API with OWIN Self-Hosting</span></span>

<span data-ttu-id="a6be3-150">Pokud jste samoobslužné hostování s OWIN, vytvořte novou **HttpConfiguration** instance.</span><span class="sxs-lookup"><span data-stu-id="a6be3-150">If you are self-hosting with OWIN, create a new **HttpConfiguration** instance.</span></span> <span data-ttu-id="a6be3-151">V této instanci provádět žádnou konfiguraci a poté předat instanci systému na **Owin.UseWebApi** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a6be3-151">Perform any configuration on this instance, and then pass the instance to the **Owin.UseWebApi** extension method.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="a6be3-152">Tento kurz [OWIN použití Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) ukazuje úplné kroky.</span><span class="sxs-lookup"><span data-stu-id="a6be3-152">The tutorial [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) shows the complete steps.</span></span>

<a id="services"></a>
## <a name="global-web-api-services"></a><span data-ttu-id="a6be3-153">Globální webové rozhraní API služby</span><span class="sxs-lookup"><span data-stu-id="a6be3-153">Global Web API Services</span></span>

<span data-ttu-id="a6be3-154">**HttpConfiguration.Services** kolekce obsahuje sadu služeb global services, která webového rozhraní API používá k provádění různých úloh, jako jsou řadiče výběr a vyjednávání obsahu.</span><span class="sxs-lookup"><span data-stu-id="a6be3-154">The **HttpConfiguration.Services** collection contains a set of global services that Web API uses to perform various tasks, such as controller selection and content negotiation.</span></span>

> [!NOTE]
> <span data-ttu-id="a6be3-155">**Služby** kolekce není pro obecné účely mechanismus pro službu zjišťování nebo závislost vkládání.</span><span class="sxs-lookup"><span data-stu-id="a6be3-155">The **Services** collection is not a general-purpose mechanism for service discovery or dependency injection.</span></span> <span data-ttu-id="a6be3-156">Ukládá pouze typy služeb, které jsou známé rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="a6be3-156">It only stores service types that are known to the Web API framework.</span></span>


<span data-ttu-id="a6be3-157">**Služby** kolekce je inicializována s výchozí sadou služeb a může poskytnout vlastní vlastních implementací.</span><span class="sxs-lookup"><span data-stu-id="a6be3-157">The **Services** collection is initialized with a default set of services, and you can provide your own custom implementations.</span></span> <span data-ttu-id="a6be3-158">Některé služby podporuje víc instancí, zatímco ostatní může mít pouze jednu instanci.</span><span class="sxs-lookup"><span data-stu-id="a6be3-158">Some services support multiple instances, while others can have only one instance.</span></span> <span data-ttu-id="a6be3-159">(Však také poskytuje služby na úrovni kontroleru; viz [-Controller konfigurace](#percontrollerconfig).</span><span class="sxs-lookup"><span data-stu-id="a6be3-159">(However, you can also provide services at the controller level; see [Per-Controller Configuration](#percontrollerconfig).</span></span>

<span data-ttu-id="a6be3-160">Jednou instancí služby</span><span class="sxs-lookup"><span data-stu-id="a6be3-160">Single-Instance Services</span></span>


| <span data-ttu-id="a6be3-161">Služba</span><span class="sxs-lookup"><span data-stu-id="a6be3-161">Service</span></span> | <span data-ttu-id="a6be3-162">Popis</span><span class="sxs-lookup"><span data-stu-id="a6be3-162">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a6be3-163">**IActionValueBinder**</span><span class="sxs-lookup"><span data-stu-id="a6be3-163">**IActionValueBinder**</span></span> | <span data-ttu-id="a6be3-164">Získá vazbu pro parametr.</span><span class="sxs-lookup"><span data-stu-id="a6be3-164">Gets a binding for a parameter.</span></span> |
| <span data-ttu-id="a6be3-165">**IApiExplorer**</span><span class="sxs-lookup"><span data-stu-id="a6be3-165">**IApiExplorer**</span></span> | <span data-ttu-id="a6be3-166">Získá popisy rozhraní API vystavené aplikace.</span><span class="sxs-lookup"><span data-stu-id="a6be3-166">Gets descriptions of the APIs exposed by the application.</span></span> <span data-ttu-id="a6be3-167">V tématu [vytváření stránky nápovědy pro webové rozhraní API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span><span class="sxs-lookup"><span data-stu-id="a6be3-167">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="a6be3-168">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="a6be3-168">**IAssembliesResolver**</span></span> | <span data-ttu-id="a6be3-169">Získá seznam sestavení pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a6be3-169">Gets a list of the assemblies for the application.</span></span> <span data-ttu-id="a6be3-170">V tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="a6be3-170">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="a6be3-171">**IBodyModelValidator**</span><span class="sxs-lookup"><span data-stu-id="a6be3-171">**IBodyModelValidator**</span></span> | <span data-ttu-id="a6be3-172">Ověří model, který je číst z textu žádosti formátovací modul typu média.</span><span class="sxs-lookup"><span data-stu-id="a6be3-172">Validates a model that is read from the request body by a media-type formatter.</span></span> |
| <span data-ttu-id="a6be3-173">**IContentNegotiator**</span><span class="sxs-lookup"><span data-stu-id="a6be3-173">**IContentNegotiator**</span></span> | <span data-ttu-id="a6be3-174">Provede vyjednávání obsahu.</span><span class="sxs-lookup"><span data-stu-id="a6be3-174">Performs content negotiation.</span></span> |
| <span data-ttu-id="a6be3-175">**IDocumentationProvider**</span><span class="sxs-lookup"><span data-stu-id="a6be3-175">**IDocumentationProvider**</span></span> | <span data-ttu-id="a6be3-176">Poskytuje dokumentaci k rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a6be3-176">Provides documentation for APIs.</span></span> <span data-ttu-id="a6be3-177">Výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="a6be3-177">The default is **null**.</span></span> <span data-ttu-id="a6be3-178">V tématu [vytváření stránky nápovědy pro webové rozhraní API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span><span class="sxs-lookup"><span data-stu-id="a6be3-178">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="a6be3-179">**IHostBufferPolicySelector**</span><span class="sxs-lookup"><span data-stu-id="a6be3-179">**IHostBufferPolicySelector**</span></span> | <span data-ttu-id="a6be3-180">Určuje, zda by měl hostitel vyrovnávací paměti obsah entity zprávy HTTP.</span><span class="sxs-lookup"><span data-stu-id="a6be3-180">Indicates whether the host should buffer HTTP message entity bodies.</span></span> |
| <span data-ttu-id="a6be3-181">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="a6be3-181">**IHttpActionInvoker**</span></span> | <span data-ttu-id="a6be3-182">Vyvolá akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="a6be3-182">Invokes a controller action.</span></span> <span data-ttu-id="a6be3-183">V tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="a6be3-183">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="a6be3-184">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="a6be3-184">**IHttpActionSelector**</span></span> | <span data-ttu-id="a6be3-185">Vybere akci kontroleru.</span><span class="sxs-lookup"><span data-stu-id="a6be3-185">Selects a controller action.</span></span> <span data-ttu-id="a6be3-186">V tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="a6be3-186">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="a6be3-187">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="a6be3-187">**IHttpControllerActivator**</span></span> | <span data-ttu-id="a6be3-188">Aktivuje řadiči.</span><span class="sxs-lookup"><span data-stu-id="a6be3-188">Activates a controller.</span></span> <span data-ttu-id="a6be3-189">V tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="a6be3-189">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="a6be3-190">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="a6be3-190">**IHttpControllerSelector**</span></span> | <span data-ttu-id="a6be3-191">Vybere kontroler.</span><span class="sxs-lookup"><span data-stu-id="a6be3-191">Selects a controller.</span></span> <span data-ttu-id="a6be3-192">V tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="a6be3-192">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="a6be3-193">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="a6be3-193">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="a6be3-194">Obsahuje seznam typů kontroleru webového rozhraní API v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a6be3-194">Provides a list of the Web API controller types in the application.</span></span> <span data-ttu-id="a6be3-195">V tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="a6be3-195">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="a6be3-196">**ITraceManager**</span><span class="sxs-lookup"><span data-stu-id="a6be3-196">**ITraceManager**</span></span> | <span data-ttu-id="a6be3-197">Inicializuje rámce pro trasování.</span><span class="sxs-lookup"><span data-stu-id="a6be3-197">Initializes the tracing framework.</span></span> <span data-ttu-id="a6be3-198">V tématu [trasování v rozhraní ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="a6be3-198">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="a6be3-199">**ITraceWriter**</span><span class="sxs-lookup"><span data-stu-id="a6be3-199">**ITraceWriter**</span></span> | <span data-ttu-id="a6be3-200">Poskytuje zapisovač trasování.</span><span class="sxs-lookup"><span data-stu-id="a6be3-200">Provides a trace writer.</span></span> <span data-ttu-id="a6be3-201">Výchozí hodnota je zapisovač trasování "no-op".</span><span class="sxs-lookup"><span data-stu-id="a6be3-201">The default is a "no-op" trace writer.</span></span> <span data-ttu-id="a6be3-202">V tématu [trasování v rozhraní ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="a6be3-202">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="a6be3-203">**IModelValidatorCache**</span><span class="sxs-lookup"><span data-stu-id="a6be3-203">**IModelValidatorCache**</span></span> | <span data-ttu-id="a6be3-204">Poskytuje mezipaměť validátory modelu.</span><span class="sxs-lookup"><span data-stu-id="a6be3-204">Provides a cache of model validators.</span></span> |

<span data-ttu-id="a6be3-205">Více instancí služby</span><span class="sxs-lookup"><span data-stu-id="a6be3-205">Multiple-Instance Services</span></span>


| <span data-ttu-id="a6be3-206">Služba</span><span class="sxs-lookup"><span data-stu-id="a6be3-206">Service</span></span> | <span data-ttu-id="a6be3-207">Popis</span><span class="sxs-lookup"><span data-stu-id="a6be3-207">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a6be3-208">**IFilterProvider**</span><span class="sxs-lookup"><span data-stu-id="a6be3-208">**IFilterProvider**</span></span> | <span data-ttu-id="a6be3-209">Vrátí seznam filtrů pro akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="a6be3-209">Returns a list of filters for a controller action.</span></span> |
| <span data-ttu-id="a6be3-210">**ModelBinderProvider**</span><span class="sxs-lookup"><span data-stu-id="a6be3-210">**ModelBinderProvider**</span></span> | <span data-ttu-id="a6be3-211">Vrátí vazač modelu pro daného typu.</span><span class="sxs-lookup"><span data-stu-id="a6be3-211">Returns a model binder for a given type.</span></span> |
| <span data-ttu-id="a6be3-212">**ModelMetadataProvider**</span><span class="sxs-lookup"><span data-stu-id="a6be3-212">**ModelMetadataProvider**</span></span> | <span data-ttu-id="a6be3-213">Poskytuje metadata pro model.</span><span class="sxs-lookup"><span data-stu-id="a6be3-213">Provides metadata for a model.</span></span> |
| <span data-ttu-id="a6be3-214">**ModelValidatorProvider**</span><span class="sxs-lookup"><span data-stu-id="a6be3-214">**ModelValidatorProvider**</span></span> | <span data-ttu-id="a6be3-215">Poskytuje validátor pro model.</span><span class="sxs-lookup"><span data-stu-id="a6be3-215">Provides a validator for a model.</span></span> |
| <span data-ttu-id="a6be3-216">**ValueProviderFactory**</span><span class="sxs-lookup"><span data-stu-id="a6be3-216">**ValueProviderFactory**</span></span> | <span data-ttu-id="a6be3-217">Vytvoří zprostředkovatele hodnot.</span><span class="sxs-lookup"><span data-stu-id="a6be3-217">Creates a value provider.</span></span> <span data-ttu-id="a6be3-218">Další informace najdete v tématu Karel místo blogu [vytvoření zprostředkovatele vlastní hodnoty v WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span><span class="sxs-lookup"><span data-stu-id="a6be3-218">For more information, see Mike Stall's blog post [How to create a custom value provider in WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span></span> |<span data-ttu-id="a6be3-219">.</span><span class="sxs-lookup"><span data-stu-id="a6be3-219">.</span></span>

<span data-ttu-id="a6be3-220">Chcete-li přidat vlastní implementaci služby s více instancemi, volejte **přidat** nebo **vložit** na **služby** kolekce:</span><span class="sxs-lookup"><span data-stu-id="a6be3-220">To add a custom implementation to a multi-instance service, call **Add** or **Insert** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="a6be3-221">Pokud chcete nahradit vlastní implementaci jedné instance služby, volání **nahradit** na **služby** kolekce:</span><span class="sxs-lookup"><span data-stu-id="a6be3-221">To replace a single-instance service with a custom implementation, call **Replace** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a><span data-ttu-id="a6be3-222">Konfigurace Kontroleru</span><span class="sxs-lookup"><span data-stu-id="a6be3-222">Per-Controller Configuration</span></span>

<span data-ttu-id="a6be3-223">Můžete přepsat na základě-controller následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="a6be3-223">You can override the following settings on a per-controller basis:</span></span>

- <span data-ttu-id="a6be3-224">Formátovací moduly typu média</span><span class="sxs-lookup"><span data-stu-id="a6be3-224">Media-type formatters</span></span>
- <span data-ttu-id="a6be3-225">Parametr vazby pravidla</span><span class="sxs-lookup"><span data-stu-id="a6be3-225">Parameter binding rules</span></span>
- <span data-ttu-id="a6be3-226">Služby</span><span class="sxs-lookup"><span data-stu-id="a6be3-226">Services</span></span>

<span data-ttu-id="a6be3-227">Uděláte to tak definovat vlastní atribut, který implementuje **IControllerConfiguration** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a6be3-227">To do so, define a custom attribute that implements the **IControllerConfiguration** interface.</span></span> <span data-ttu-id="a6be3-228">Pak použijte atribut kontroleru.</span><span class="sxs-lookup"><span data-stu-id="a6be3-228">Then apply the attribute to the controller.</span></span>

<span data-ttu-id="a6be3-229">Následující příklad nahradí formátovací moduly typu média výchozí vlastní formátování.</span><span class="sxs-lookup"><span data-stu-id="a6be3-229">The following example replaces the default media-type formatters with a custom formatter.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="a6be3-230">**IControllerConfiguration.Initialize** metoda přebírá dva parametry:</span><span class="sxs-lookup"><span data-stu-id="a6be3-230">The **IControllerConfiguration.Initialize** method takes two parameters:</span></span>

- <span data-ttu-id="a6be3-231">**HttpControllerSettings** objektu</span><span class="sxs-lookup"><span data-stu-id="a6be3-231">An **HttpControllerSettings** object</span></span>
- <span data-ttu-id="a6be3-232">**HttpControllerDescriptor** objektu</span><span class="sxs-lookup"><span data-stu-id="a6be3-232">An **HttpControllerDescriptor** object</span></span>

<span data-ttu-id="a6be3-233">**HttpControllerDescriptor** obsahuje popis řadiči, který můžete zkontrolovat pro informační účely (k rozlišení dvou řadičích indikované).</span><span class="sxs-lookup"><span data-stu-id="a6be3-233">The **HttpControllerDescriptor** contains a description of the controller, which you can examine for informational purposes (say, to distinguish between two controllers).</span></span>

<span data-ttu-id="a6be3-234">Použití **HttpControllerSettings** objekt, který chcete nakonfigurovat kontroleru.</span><span class="sxs-lookup"><span data-stu-id="a6be3-234">Use the **HttpControllerSettings** object to configure the controller.</span></span> <span data-ttu-id="a6be3-235">Tento objekt obsahuje podmnožinu konfigurační parametry, které je možné přepsat na základě-controller.</span><span class="sxs-lookup"><span data-stu-id="a6be3-235">This object contains the subset of configuration parameters that you can override on a per-controller basis.</span></span> <span data-ttu-id="a6be3-236">Všechna nastavení, která Neměnit výchozí na globální **HttpConfiguration** objektu.</span><span class="sxs-lookup"><span data-stu-id="a6be3-236">Any settings that you don't change default to the global **HttpConfiguration** object.</span></span>
