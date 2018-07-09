---
title: Migrace z rozhraní ASP.NET Web API pro ASP.NET Core
author: ardalis
description: Zjistěte, jak migrovat implementaci webového rozhraní API z webové rozhraní API ASP.NET do ASP.NET Core MVC.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894189"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="25988-103">Migrace z rozhraní ASP.NET Web API pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25988-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="25988-104">Podle [Steve Smith](https://ardalis.com/) a [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="25988-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="25988-105">Webová rozhraní API jsou služeb HTTP, které jsou poskytovány širokému spektru klientů, včetně prohlížečů a mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="25988-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="25988-106">ASP.NET Core MVC zahrnuje podporu pro vytváření webových rozhraní API poskytuje jednoduchý a konzistentní způsob vytváření webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="25988-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="25988-107">V tomto článku vám ukážeme kroky potřebné k migraci implementaci webového rozhraní API z webové rozhraní API ASP.NET do ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="25988-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="25988-108">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="25988-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="25988-109">Přezkoumání projektu ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="25988-109">Review ASP.NET Web API project</span></span>

<span data-ttu-id="25988-110">Tento článek používá ukázkový projekt *ProductsApp*, které byly vytvořeny v článku [Začínáme s ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) jako výchozí bod.</span><span class="sxs-lookup"><span data-stu-id="25988-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="25988-111">V příslušném projektu je jednoduchý projekt webového rozhraní API ASP.NET nakonfigurovat následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="25988-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="25988-112">V *Global.asax.cs*, je provedeno volání `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="25988-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="25988-113">`WebApiConfig` je definován v *App_Start*, a má jenom jednu statickou `Register` metody:</span><span class="sxs-lookup"><span data-stu-id="25988-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

<span data-ttu-id="25988-114">Tato třída nakonfiguruje [směrováním atributů](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), i když se ve skutečnosti používá v projektu.</span><span class="sxs-lookup"><span data-stu-id="25988-114">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="25988-115">Nakonfiguruje taky směrovací tabulky, který používá rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="25988-115">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="25988-116">V takovém případě bude rozhraní ASP.NET Web API očekávané adresy URL ve formátu */api/ {controller} / {id}*, s *{id}* je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="25988-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="25988-117">*ProductsApp* projekt obsahuje pouze jeden jednoduchý kontroleru, která dědí z `ApiController` a poskytuje dvě metody:</span><span class="sxs-lookup"><span data-stu-id="25988-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="25988-118">Nakonec modelu *produktu*, používá *ProductsApp*, je jednoduchou třídu:</span><span class="sxs-lookup"><span data-stu-id="25988-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="25988-119">Když teď máme Jednoduchý projekt, ze kterého se má spustit, jsme ukazují, jak migrovat tento projekt webového rozhraní API pro ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="25988-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="25988-120">Vytvořte cílový projekt</span><span class="sxs-lookup"><span data-stu-id="25988-120">Create the Destination Project</span></span>

<span data-ttu-id="25988-121">Pomocí sady Visual Studio, vytvořte novou, prázdnou řešení s názvem *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="25988-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="25988-122">Přidat existující *ProductsApp* projekt na ni a pak přidejte nový projekt webové aplikace ASP.NET Core do řešení.</span><span class="sxs-lookup"><span data-stu-id="25988-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="25988-123">Název nového projektu *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="25988-123">Name the new project *ProductsCore*.</span></span>

![Otevře dialogové okno Nový projekt webové šablony](webapi/_static/add-web-project.png)

<span data-ttu-id="25988-125">Potom vyberte šablonu projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="25988-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="25988-126">Budeme migrovat *ProductsApp* obsah do tohoto nového projektu.</span><span class="sxs-lookup"><span data-stu-id="25988-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Dialogové okno nové webové aplikace pomocí šablony projektu webového rozhraní API vybrali v seznamu šablon ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="25988-128">Odstranit `Project_Readme.html` z projektu nový soubor.</span><span class="sxs-lookup"><span data-stu-id="25988-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="25988-129">Řešení by teď měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="25988-129">Your solution should now look like this:</span></span>

![Otevřít v Průzkumníkovi řešení se soubory a složkami ProductsApp a ProductsCore projektů řešení aplikací](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="25988-131">Migrace konfigurace</span><span class="sxs-lookup"><span data-stu-id="25988-131">Migrate Configuration</span></span>

<span data-ttu-id="25988-132">ASP.NET Core se už používá *Global.asax*, *web.config*, nebo *App_Start* složek.</span><span class="sxs-lookup"><span data-stu-id="25988-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="25988-133">Místo toho všechny úlohy po spuštění se provádějí *Startup.cs* v kořenovém adresáři projektu (viz [spuštění aplikace](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="25988-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="25988-134">V ASP.NET Core MVC, směrování na základě atributů je nyní standardní součástí při `UseMvc()` nazývá; a toto je doporučený postup pro konfiguraci tras webového rozhraní API (a způsob, jakým zpracovává počáteční projekt webového rozhraní API směrování).</span><span class="sxs-lookup"><span data-stu-id="25988-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

<span data-ttu-id="25988-135">Za předpokladu, že chcete používat směrování atributů ve vašem projektu do budoucna, je potřeba žádná další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="25988-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="25988-136">Jednoduše použijte atributy podle potřeby do kontrolerů a akcí, jak je tomu v ukázce `ValuesController` třídu, která je součástí počáteční projekt webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="25988-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="25988-137">Všimněte si přítomnost *[řadič]* na řádku 8.</span><span class="sxs-lookup"><span data-stu-id="25988-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="25988-138">Směrování na základě atributu nyní podporuje určité tokeny, jako například *[řadič]* a *[action]*.</span><span class="sxs-lookup"><span data-stu-id="25988-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="25988-139">Tyto tokeny se nahradí za běhu s názvem kontroler nebo akce, v uvedeném pořadí, ke které byl použit atribut.</span><span class="sxs-lookup"><span data-stu-id="25988-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="25988-140">Tato možnost slouží k omezení počtu magic řetězce v projektu, a zajišťuje, že trasy se budou uchovávat synchronizované s jejich odpovídající kontrolery a akce při použití automatické přejmenování refaktoring.</span><span class="sxs-lookup"><span data-stu-id="25988-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="25988-141">K migraci kontroler API produkty, jsme nutné nejprve zkopírovat *ProductsController* do nového projektu.</span><span class="sxs-lookup"><span data-stu-id="25988-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="25988-142">Pak jednoduše uvést atribut trasy na kontroleru:</span><span class="sxs-lookup"><span data-stu-id="25988-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="25988-143">Budete taky muset přidat `[HttpGet]` atribut dvě metody, protože oba by měla být volána prostřednictvím HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="25988-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="25988-144">Zahrnout očekává parametr "id" atribut `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="25988-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="25988-145">V tomto okamžiku směrování je správně nakonfigurovaná; ale jsme ji nelze ještě otestovat.</span><span class="sxs-lookup"><span data-stu-id="25988-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="25988-146">Je nutné provést další změny před *ProductsController* bude zkompilována.</span><span class="sxs-lookup"><span data-stu-id="25988-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="25988-147">Migrace modelů a Kontrolerů</span><span class="sxs-lookup"><span data-stu-id="25988-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="25988-148">Posledním krokem v procesu migrace pro tento jednoduchý projekt webového rozhraní API se mají zkopírovat do řadičů a všechny modely, které používají.</span><span class="sxs-lookup"><span data-stu-id="25988-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="25988-149">V takovém případě stačí zkopírovat *Controllers/ProductsController.cs* z původního projektu do nové.</span><span class="sxs-lookup"><span data-stu-id="25988-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="25988-150">Potom zkopírujte celou složku modely z původního projektu do nové.</span><span class="sxs-lookup"><span data-stu-id="25988-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="25988-151">Upravit obory názvů tak, aby odpovídaly novým názvem projektu (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="25988-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="25988-152">V tomto okamžiku můžete vytvořit aplikaci a zjistíte, počet chyb kompilace.</span><span class="sxs-lookup"><span data-stu-id="25988-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="25988-153">Obecně by měla spadat do následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="25988-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="25988-154">*Objektu ApiController* neexistuje</span><span class="sxs-lookup"><span data-stu-id="25988-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="25988-155">*System.Web.Http* obor názvů neexistuje.</span><span class="sxs-lookup"><span data-stu-id="25988-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="25988-156">*IHttpActionResult* neexistuje</span><span class="sxs-lookup"><span data-stu-id="25988-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="25988-157">Naštěstí následují všechny velmi snadné, chcete-li opravit:</span><span class="sxs-lookup"><span data-stu-id="25988-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="25988-158">Změna *objektu ApiController* k *řadič* (budete muset přidat *pomocí Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="25988-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="25988-159">Odstraní všechny pomocí příkazu odkazující na *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="25988-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="25988-160">Změnit všechny metody vracející *IHttpActionResult* se vraťte *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="25988-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="25988-161">Jakmile tyto změny byly provedeny a nepoužívané pomocí příkazů odebrat, migrovaných *ProductsController* třídy vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="25988-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="25988-162">Teď by měl být schopni spustit migrovaného projektu a přejděte do */api/produkty*; a zobrazí se úplný seznam produktů 3.</span><span class="sxs-lookup"><span data-stu-id="25988-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="25988-163">Přejděte do */api/products/1* a měli byste vidět první produktu.</span><span class="sxs-lookup"><span data-stu-id="25988-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a><span data-ttu-id="25988-164">Překrytí kompatibility ASP.NET 4.x webovým rozhraním API 2</span><span class="sxs-lookup"><span data-stu-id="25988-164">ASP.NET 4.x Web API 2 compatibility shim</span></span>

<span data-ttu-id="25988-165">Je užitečné nástroje při migraci ASP.NET Web API projektů ASP.NET Core [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) knihovny.</span><span class="sxs-lookup"><span data-stu-id="25988-165">A useful tool when migrating ASP.NET Web API projects to ASP.NET Core is the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library.</span></span> <span data-ttu-id="25988-166">Překrytí kompatibility rozšiřuje ASP.NET Core umožňující celou řadou různých konvencí webovým rozhraním API 2, který se má použít.</span><span class="sxs-lookup"><span data-stu-id="25988-166">The compatibility shim extends ASP.NET Core to allow a number of different Web API 2 conventions to be used.</span></span> <span data-ttu-id="25988-167">Ukázka přenáší dříve v tomto dokumentu je dostatečně základní, že překrytí kompatibility není nezbytné.</span><span class="sxs-lookup"><span data-stu-id="25988-167">The sample ported previously in this document is basic enough that the compatibility shim was not necessary.</span></span> <span data-ttu-id="25988-168">U větších projektů pomocí shimu kompatibility může být užitečná pro dočasné překlenutí této propasti API mezi ASP.NET Core a ASP.NET Web API 2.</span><span class="sxs-lookup"><span data-stu-id="25988-168">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET Web API 2.</span></span>

<span data-ttu-id="25988-169">Překrytí kompatibility webového rozhraní API je určen se použije jako dočasné opatření pro usnadnění migrace velké projekty webového rozhraní API pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="25988-169">The Web API compatibility shim is meant to be used as a temporary measure to facilitate migrating large Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="25988-170">V průběhu času se musí aktualizovat projekty ASP.NET Core využít aniž byste museli spoléhat na překrytí kompatibility.</span><span class="sxs-lookup"><span data-stu-id="25988-170">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="25988-171">Kompatibilita funkce obsažené v `Microsoft.AspNetCore.Mvc.WebApiCompatShim` patří:</span><span class="sxs-lookup"><span data-stu-id="25988-171">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="25988-172">Přidá `ApiController` tak, aby základních typů řadičů není potřeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="25988-172">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="25988-173">Umožňuje vytvořit vazbu modelu webové rozhraní API – stylu.</span><span class="sxs-lookup"><span data-stu-id="25988-173">Enables Web API-style model binding.</span></span> <span data-ttu-id="25988-174">ASP.NET Core MVC model vazby funkce podobně jako na MVC 5, ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="25988-174">ASP.NET Core MVC model binding functions similarly to MVC 5, by default.</span></span> <span data-ttu-id="25988-175">Změny překrytí kompatibility vazby se víc podobá vytváření vazby modelu webovým rozhraním API 2 modelu.</span><span class="sxs-lookup"><span data-stu-id="25988-175">The compatibility shim changes model binding to be more similar to Web API 2 model binding conventions.</span></span> <span data-ttu-id="25988-176">Komplexní typy jsou automaticky svázán z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="25988-176">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="25988-177">Rozšiřuje vazby modelu, tak, aby akce kontroleru může mít parametry typu `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="25988-177">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="25988-178">Přidá formátování zpráv umožňuje akce vrátí výsledky typu `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="25988-178">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="25988-179">Přidá další odpovědi metody, které webové rozhraní API 2 akce může mít používají k předávání odpovědi:</span><span class="sxs-lookup"><span data-stu-id="25988-179">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="25988-180">Objekt HttpResponseMessage generátorů:</span><span class="sxs-lookup"><span data-stu-id="25988-180">HttpResponseMessage generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="25988-181">Výsledek metody akce:</span><span class="sxs-lookup"><span data-stu-id="25988-181">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="25988-182">Přidá instanci `IContentNegotiator` do kontejnerů DI aplikace a provede vyjednávání souvisejících typů z obsahu [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) k dispozici.</span><span class="sxs-lookup"><span data-stu-id="25988-182">Adds an instance of `IContentNegotiator` to the app's DI container and makes content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) available.</span></span> <span data-ttu-id="25988-183">Jedná se o typech, jako je `DefaultContentNegotiator`, `MediaTypeFormatter`atd.</span><span class="sxs-lookup"><span data-stu-id="25988-183">This includes types like `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span></span>

<span data-ttu-id="25988-184">Chcete-li použít překrytí kompatibility, budete muset:</span><span class="sxs-lookup"><span data-stu-id="25988-184">To use the compatibility shim, you need to:</span></span>

* <span data-ttu-id="25988-185">Nainstalujte [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="25988-185">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
* <span data-ttu-id="25988-186">Zaregistrovat překrytí kompatibility služby kontejnerů DI aplikace po zavolání `services.AddMvc().AddWebApiConventions()` aplikace `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="25988-186">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="25988-187">Definování tras specifické pro webové rozhraní API pomocí `MapWebApiRoute` na `IRouteBuilder` aplikace `IApplicationBuilder.UseMvc` volání.</span><span class="sxs-lookup"><span data-stu-id="25988-187">Define Web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="summary"></a><span data-ttu-id="25988-188">Souhrn</span><span class="sxs-lookup"><span data-stu-id="25988-188">Summary</span></span>

<span data-ttu-id="25988-189">Migrace Jednoduchý projekt webového rozhraní API ASP.NET do ASP.NET Core MVC je celkem jasné, Děkujeme integrovanou podporu pro webové rozhraní API v ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="25988-189">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="25988-190">Hlavní části, bude nutné každý projekt ASP.NET Web API pro migraci jsou tras, kontrolerů a modelů, spolu s aktualizace typů používaných modulem kontrolery a akce.</span><span class="sxs-lookup"><span data-stu-id="25988-190">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
