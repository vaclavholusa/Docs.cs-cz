---
title: Migrace z rozhraní ASP.NET Web API pro ASP.NET Core
author: ardalis
description: Zjistěte, jak migrovat implementaci webového rozhraní API z ASP.NET 4.x webového rozhraní API pro ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/01/2018
uid: migration/webapi
ms.openlocfilehash: f5d886a7c3182b5cd372762ade67c2e748051049
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207274"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="a7e54-103">Migrace z rozhraní ASP.NET Web API pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a7e54-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="a7e54-104">Podle [Scott Addie](https://twitter.com/scott_addie) a [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a7e54-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a7e54-105">Rozhraní Web API pro ASP.NET 4.x je služba HTTP, kterou půjde používat celou řadu klientů, včetně prohlížečů a mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="a7e54-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="a7e54-106">Webové rozhraní API aplikace modelů do jednodušší programovacího modelu říká ASP.NET Core MVC a sjednocuje ASP.NET Core MVC ASP.NET 4.x společnosti.</span><span class="sxs-lookup"><span data-stu-id="a7e54-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="a7e54-107">Tento článek popisuje kroky potřebné k migraci z ASP.NET 4.x webového rozhraní API pro ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="a7e54-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="a7e54-108">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([stažení](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a7e54-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a7e54-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a7e54-109">Prerequisites</span></span>

* [<span data-ttu-id="a7e54-110">Sady SDK .NET core 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="a7e54-110">.NET Core 2.1 SDK or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="a7e54-111">[Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15.7.3 nebo novější s **vývoj pro ASP.NET a web** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="a7e54-111">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="a7e54-112">Projděte si projekt webového rozhraní API technologie ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="a7e54-112">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="a7e54-113">Jako výchozí bod, tento článek používá *ProductsApp* projekt vytvořený v [Začínáme s ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span><span class="sxs-lookup"><span data-stu-id="a7e54-113">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="a7e54-114">V příslušném projektu je nakonfigurovaný Jednoduchý projekt webového rozhraní API technologie ASP.NET 4.x následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="a7e54-114">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="a7e54-115">V *Global.asax.cs*, je provedeno volání `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="a7e54-115">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="a7e54-116">`WebApiConfig` je definován v *App_Start* složky.</span><span class="sxs-lookup"><span data-stu-id="a7e54-116">`WebApiConfig` is defined in the *App_Start* folder.</span></span> <span data-ttu-id="a7e54-117">Má jenom jednu statickou `Register` metody:</span><span class="sxs-lookup"><span data-stu-id="a7e54-117">It has just one static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15-20)]

<span data-ttu-id="a7e54-118">Tato třída nakonfiguruje [směrováním atributů](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), i když se ve skutečnosti používá v projektu.</span><span class="sxs-lookup"><span data-stu-id="a7e54-118">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="a7e54-119">Nakonfiguruje taky směrovací tabulky, který používá rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="a7e54-119">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="a7e54-120">V takovém případě očekává, že webové rozhraní API ASP.NET 4.x adresy URL ve formátu `/api/{controller}/{id}`, s `{id}` je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="a7e54-120">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="a7e54-121">*ProductsApp* projekt obsahuje jeden kontroler.</span><span class="sxs-lookup"><span data-stu-id="a7e54-121">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="a7e54-122">Kontroler dědí z `ApiController` a poskytuje dvě metody:</span><span class="sxs-lookup"><span data-stu-id="a7e54-122">The controller inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="a7e54-123">`Product` Model používaný `ProductsController` je jednoduchou třídu:</span><span class="sxs-lookup"><span data-stu-id="a7e54-123">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="a7e54-124">Následující části ukazují migrace projektu webového rozhraní API pro ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="a7e54-124">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="a7e54-125">Vytvořte cílový projekt</span><span class="sxs-lookup"><span data-stu-id="a7e54-125">Create destination project</span></span>

<span data-ttu-id="a7e54-126">Proveďte následující kroky v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a7e54-126">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="a7e54-127">Přejděte na **souboru** > **nové** > **projektu** > **ostatní typy projektů**  >  **Řešení sady visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="a7e54-127">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="a7e54-128">Vyberte **prázdné řešení**a řešení nazvěte *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="a7e54-128">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="a7e54-129">Klikněte na tlačítko **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a7e54-129">Click the **OK** button.</span></span>
* <span data-ttu-id="a7e54-130">Přidat existující *ProductsApp* projektu do řešení.</span><span class="sxs-lookup"><span data-stu-id="a7e54-130">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="a7e54-131">Přidat nový **webové aplikace ASP.NET Core** projektu do řešení.</span><span class="sxs-lookup"><span data-stu-id="a7e54-131">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="a7e54-132">Vyberte **.NET Core** cílové rozhraní framework z rozevíracího seznamu a vyberte **API** šablony projektu.</span><span class="sxs-lookup"><span data-stu-id="a7e54-132">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="a7e54-133">Pojmenujte projekt *ProductsCore*a klikněte na tlačítko **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a7e54-133">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="a7e54-134">Řešení teď obsahuje dva projekty.</span><span class="sxs-lookup"><span data-stu-id="a7e54-134">The solution now contains two projects.</span></span> <span data-ttu-id="a7e54-135">Následující části popisují migraci *ProductsApp* obsah projektu *ProductsCore* projektu.</span><span class="sxs-lookup"><span data-stu-id="a7e54-135">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="a7e54-136">Migrace konfigurace</span><span class="sxs-lookup"><span data-stu-id="a7e54-136">Migrate configuration</span></span>

<span data-ttu-id="a7e54-137">ASP.NET Core nepoužívá *App_Start* složky nebo *Global.asax* souboru a *web.config* se přidá soubor na čas publikování.</span><span class="sxs-lookup"><span data-stu-id="a7e54-137">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="a7e54-138">*Startup.cs* je náhradou *Global.asax* a je umístěn v kořenové složce projektu.</span><span class="sxs-lookup"><span data-stu-id="a7e54-138">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="a7e54-139">`Startup` Třída zpracovává všechny úlohy po spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="a7e54-139">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="a7e54-140">Další informace naleznete v tématu <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="a7e54-140">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="a7e54-141">V ASP.NET Core MVC směrovací atribut je standardní součástí při <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> je volána `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a7e54-141">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="a7e54-142">Následující `UseMvc` volání nahradí *ProductsApp* projektu *App_Start/WebApiConfig.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="a7e54-142">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="a7e54-143">Migrace modelů a kontrolerů</span><span class="sxs-lookup"><span data-stu-id="a7e54-143">Migrate models and controllers</span></span>

<span data-ttu-id="a7e54-144">Zkopírovat *ProductApp* projektu kontroleru a používá model.</span><span class="sxs-lookup"><span data-stu-id="a7e54-144">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="a7e54-145">Postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="a7e54-145">Follow these steps:</span></span>

1. <span data-ttu-id="a7e54-146">Kopírování *Controllers/ProductsController.cs* z původního projektu do nové.</span><span class="sxs-lookup"><span data-stu-id="a7e54-146">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="a7e54-147">Zkopírujte celý *modely* složku z původního projektu do nové.</span><span class="sxs-lookup"><span data-stu-id="a7e54-147">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="a7e54-148">Změnit obory názvů zkopírované soubory tak, aby odpovídaly novým názvem projektu (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="a7e54-148">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="a7e54-149">Upravit `using ProductsApp.Models;` výroky *ProductsController.cs* příliš.</span><span class="sxs-lookup"><span data-stu-id="a7e54-149">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="a7e54-150">V tomto okamžiku vytváření aplikace výsledkem počet chyb kompilace.</span><span class="sxs-lookup"><span data-stu-id="a7e54-150">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="a7e54-151">Této chybě dojde, protože následující součásti neexistují v ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a7e54-151">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="a7e54-152">`ApiController` Třída</span><span class="sxs-lookup"><span data-stu-id="a7e54-152">`ApiController` class</span></span>
* <span data-ttu-id="a7e54-153">`System.Web.Http` Namespace</span><span class="sxs-lookup"><span data-stu-id="a7e54-153">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="a7e54-154">`IHttpActionResult` Rozhraní</span><span class="sxs-lookup"><span data-stu-id="a7e54-154">`IHttpActionResult` interface</span></span>

<span data-ttu-id="a7e54-155">Opravte chyby následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a7e54-155">Fix the errors as follows:</span></span>

1. <span data-ttu-id="a7e54-156">Změna `ApiController` k <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="a7e54-156">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="a7e54-157">Přidat `using Microsoft.AspNetCore.Mvc;` přeložit `ControllerBase` odkaz.</span><span class="sxs-lookup"><span data-stu-id="a7e54-157">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="a7e54-158">Odstraňte `using System.Web.Http;`.</span><span class="sxs-lookup"><span data-stu-id="a7e54-158">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="a7e54-159">Změnit `GetProduct` návratový typ akce z `IHttpActionResult` k `ActionResult<Product>`.</span><span class="sxs-lookup"><span data-stu-id="a7e54-159">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

## <a name="configure-routing"></a><span data-ttu-id="a7e54-160">Konfigurace směrování</span><span class="sxs-lookup"><span data-stu-id="a7e54-160">Configure routing</span></span>

<span data-ttu-id="a7e54-161">Konfigurace směrování následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a7e54-161">Configure routing as follows:</span></span>

1. <span data-ttu-id="a7e54-162">Uspořádání `ProductsController` třídy s následujícími atributy:</span><span class="sxs-lookup"><span data-stu-id="a7e54-162">Decorate the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="a7e54-163">Předchozí [[trasy]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) atribut nakonfiguruje vzor směrování atributů kontroleru.</span><span class="sxs-lookup"><span data-stu-id="a7e54-163">The preceding [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="a7e54-164">[[Objektu ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atribut díky atribut směrování požadavků pro všechny akce v tomto kontroleru.</span><span class="sxs-lookup"><span data-stu-id="a7e54-164">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="a7e54-165">Směrování atributů podporuje tokeny, jako například `[controller]` a `[action]`.</span><span class="sxs-lookup"><span data-stu-id="a7e54-165">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="a7e54-166">Za běhu každý token se nahradí názvem kontroler nebo akce, v uvedeném pořadí, ke které byl použit atribut.</span><span class="sxs-lookup"><span data-stu-id="a7e54-166">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="a7e54-167">Tokeny snížit počet magic řetězců v projektu.</span><span class="sxs-lookup"><span data-stu-id="a7e54-167">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="a7e54-168">Tokeny Ujistěte se také trasy zajistila synchronizovanost s odpovídající kontrolery a akce při automatické přejmenování refaktoringy používají.</span><span class="sxs-lookup"><span data-stu-id="a7e54-168">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="a7e54-169">Povolit požadavky HTTP Get `ProductController` akce:</span><span class="sxs-lookup"><span data-stu-id="a7e54-169">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="a7e54-170">Použít [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) atribut `GetAllProducts` akce.</span><span class="sxs-lookup"><span data-stu-id="a7e54-170">Apply the [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="a7e54-171">Použít `[HttpGet("{id}")]` atribut `GetProduct` akce.</span><span class="sxs-lookup"><span data-stu-id="a7e54-171">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="a7e54-172">Až tyto změny a odstranění nepoužívaných `using` příkazů *ProductsController.cs* souboru vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="a7e54-172">After these changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="a7e54-173">Spusťte projekt pro migrované a přejděte do `/api/products`.</span><span class="sxs-lookup"><span data-stu-id="a7e54-173">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="a7e54-174">Zobrazí se úplný seznam tří produktů.</span><span class="sxs-lookup"><span data-stu-id="a7e54-174">A full list of three products appears.</span></span> <span data-ttu-id="a7e54-175">Přejděte do `/api/products/1`.</span><span class="sxs-lookup"><span data-stu-id="a7e54-175">Browse to `/api/products/1`.</span></span> <span data-ttu-id="a7e54-176">Zobrazí se první produktu.</span><span class="sxs-lookup"><span data-stu-id="a7e54-176">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="a7e54-177">Překrytí kompatibility</span><span class="sxs-lookup"><span data-stu-id="a7e54-177">Compatibility shim</span></span>

<span data-ttu-id="a7e54-178">[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) knihovna poskytuje kompatibilitu překrytí pro přesun projekty webového rozhraní API technologie ASP.NET 4.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a7e54-178">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="a7e54-179">Překrytí kompatibility rozšiřuje podporu celé řady konvence z rozhraní Web API 2 ASP.NET 4.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a7e54-179">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="a7e54-180">Ukázka přenáší dříve v tomto dokumentu je dostatečně základní, že bylo překrytí kompatibility zbytečné.</span><span class="sxs-lookup"><span data-stu-id="a7e54-180">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="a7e54-181">U větších projektů pomocí shimu kompatibility může být užitečná pro dočasné překlenutí této propasti API mezi ASP.NET Core a ASP.NET 4.x webového rozhraní API 2.</span><span class="sxs-lookup"><span data-stu-id="a7e54-181">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="a7e54-182">Překrytí kompatibility webového rozhraní API je určen se použije jako dočasné opatření pro podporu migrace velkých projektech ASP.NET 4.x webového rozhraní API pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a7e54-182">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="a7e54-183">V průběhu času se musí aktualizovat projekty ASP.NET Core využít aniž byste museli spoléhat na překrytí kompatibility.</span><span class="sxs-lookup"><span data-stu-id="a7e54-183">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="a7e54-184">Kompatibilita funkce obsažené v `Microsoft.AspNetCore.Mvc.WebApiCompatShim` patří:</span><span class="sxs-lookup"><span data-stu-id="a7e54-184">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="a7e54-185">Přidá `ApiController` tak, aby základních typů řadičů není potřeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="a7e54-185">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="a7e54-186">Umožňuje vytvořit vazbu modelu webové rozhraní API – stylu.</span><span class="sxs-lookup"><span data-stu-id="a7e54-186">Enables Web API-style model binding.</span></span> <span data-ttu-id="a7e54-187">ASP.NET Core MVC model vazby funkcí podobně, který technologie ASP.NET 4.x MVC 5, ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="a7e54-187">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="a7e54-188">Změny překrytí kompatibility vazby se víc podobá vytváření vazby modelu 4.x webovým rozhraním API 2 technologie ASP.NET modelu.</span><span class="sxs-lookup"><span data-stu-id="a7e54-188">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="a7e54-189">Komplexní typy jsou automaticky svázán z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="a7e54-189">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="a7e54-190">Rozšiřuje vazby modelu, tak, aby akce kontroleru může mít parametry typu `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="a7e54-190">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="a7e54-191">Přidá formátování zpráv umožňuje akce vrátí výsledky typu `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="a7e54-191">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="a7e54-192">Přidá další odpovědi metody, které webové rozhraní API 2 akce může mít používají k předávání odpovědi:</span><span class="sxs-lookup"><span data-stu-id="a7e54-192">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="a7e54-193">`HttpResponseMessage` generátory:</span><span class="sxs-lookup"><span data-stu-id="a7e54-193">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="a7e54-194">Výsledek metody akce:</span><span class="sxs-lookup"><span data-stu-id="a7e54-194">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="a7e54-195">Přidá instanci `IContentNegotiator` aplikaci prvku kontejneru pro vkládání (DI) závislosti a zpřístupňuje obsahu vyjednávání související typy z [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span><span class="sxs-lookup"><span data-stu-id="a7e54-195">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="a7e54-196">Mezi tyto typy patří `DefaultContentNegotiator` a `MediaTypeFormatter`.</span><span class="sxs-lookup"><span data-stu-id="a7e54-196">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="a7e54-197">Chcete-li použít překrytí kompatibility:</span><span class="sxs-lookup"><span data-stu-id="a7e54-197">To use the compatibility shim:</span></span>

1. <span data-ttu-id="a7e54-198">Nainstalujte [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="a7e54-198">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="a7e54-199">Zaregistrovat překrytí kompatibility služby kontejnerů DI aplikace po zavolání `services.AddMvc().AddWebApiConventions()` v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a7e54-199">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="a7e54-200">Definování webové rozhraní API specifické směruje pomocí `MapWebApiRoute` na `IRouteBuilder` aplikace `IApplicationBuilder.UseMvc` volání.</span><span class="sxs-lookup"><span data-stu-id="a7e54-200">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a7e54-201">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a7e54-201">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
