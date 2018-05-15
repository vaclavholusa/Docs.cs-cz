---
title: Migrace z rozhraní ASP.NET Web API na jádro ASP.NET
author: ardalis
description: Zjistěte, jak migrovat implementace webového rozhraní API z rozhraní ASP.NET Web API ASP.NET MVC jádra.
manager: wpickett
ms.author: riande
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/webapi
ms.openlocfilehash: 8d842877e49e317323d453e71ebb3302245f388d
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="2fd49-103">Migrace z rozhraní ASP.NET Web API na jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2fd49-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="2fd49-104">Podle [Steve Smith](https://ardalis.com/) a [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="2fd49-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="2fd49-105">Webová rozhraní API jsou služeb HTTP, které využity širokou škálou klientů včetně prohlížečů a mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="2fd49-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="2fd49-106">Jádro ASP.NET MVC zahrnuje podporu pro vytváření webových rozhraní API poskytuje jednoduchý a konzistentní způsob vytváření webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="2fd49-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="2fd49-107">V tomto článku jsme ukazují na kroky potřebné k migraci implementace webového rozhraní API z webového rozhraní API ASP.NET na ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="2fd49-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="2fd49-108">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2fd49-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="2fd49-109">Zkontrolujte ASP.NET Web API projektu</span><span class="sxs-lookup"><span data-stu-id="2fd49-109">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="2fd49-110">Tento článek používá ukázkový projekt *ProductsApp*, které byly vytvořeny v článku [Začínáme s ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) jako výchozí bod.</span><span class="sxs-lookup"><span data-stu-id="2fd49-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="2fd49-111">V tomto projektu jednoduché projektu ASP.NET Web API nakonfigurovat následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="2fd49-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="2fd49-112">V *Global.asax.cs*, Přišla žádost o `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="2fd49-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="2fd49-113">`WebApiConfig` je definována v *App_Start*, a má jenom jeden statický `Register` metoda:</span><span class="sxs-lookup"><span data-stu-id="2fd49-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="2fd49-114">Tato třída nakonfiguruje [směrováním atributů](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), i když je ve skutečnosti není používán v projektu.</span><span class="sxs-lookup"><span data-stu-id="2fd49-114">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="2fd49-115">Nakonfiguruje taky do směrovací tabulky, který je používán rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="2fd49-115">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="2fd49-116">V takovém případě bude rozhraní ASP.NET Web API očekávat adres URL pro odpovídat formátu */api/ {controller} / {id}*, s *{id}* je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="2fd49-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="2fd49-117">*ProductsApp* projekt obsahuje pouze jednu jednoduché řadiči, který dědí z `ApiController` a zveřejňuje dvě metody:</span><span class="sxs-lookup"><span data-stu-id="2fd49-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="2fd49-118">Nakonec model *produktu*používané nástrojem *ProductsApp*, je jednoduchá:</span><span class="sxs-lookup"><span data-stu-id="2fd49-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="2fd49-119">Teď, když máme Jednoduchý projekt, ze kterého má začít, budeme ukazují, jak migrovat tento projekt webového rozhraní API ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="2fd49-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="2fd49-120">Vytvoření projektu cílové</span><span class="sxs-lookup"><span data-stu-id="2fd49-120">Create the Destination Project</span></span>

<span data-ttu-id="2fd49-121">Pomocí sady Visual Studio, vytvořte nový, prázdný řešení a pojmenujte ji *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="2fd49-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="2fd49-122">Přidat existující *ProductsApp* projektu do ní a pak přidejte nový projekt webové aplikace ASP.NET Core do řešení.</span><span class="sxs-lookup"><span data-stu-id="2fd49-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="2fd49-123">Název nového projektu *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="2fd49-123">Name the new project *ProductsCore*.</span></span>

![Otevřete dialogové okno Nový projekt webové šablon](webapi/_static/add-web-project.png)

<span data-ttu-id="2fd49-125">V dalším kroku vyberte šablona projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2fd49-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="2fd49-126">Jsme bude migrovat *ProductsApp* obsah do nového projektu.</span><span class="sxs-lookup"><span data-stu-id="2fd49-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Dialogové okno nové webové aplikace pomocí šablony projektu webového rozhraní API v seznamu šablon ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="2fd49-128">Odstranit `Project_Readme.html` soubor z nového projektu.</span><span class="sxs-lookup"><span data-stu-id="2fd49-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="2fd49-129">Řešení by měl nyní vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="2fd49-129">Your solution should now look like this:</span></span>

![Otevřít v Průzkumníkovi řešení zobrazující soubory a složky z projektů ProductsApp a ProductsCore řešení aplikace](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="2fd49-131">Migruje konfiguraci</span><span class="sxs-lookup"><span data-stu-id="2fd49-131">Migrate Configuration</span></span>

<span data-ttu-id="2fd49-132">Už používá ASP.NET Core *Global.asax*, *web.config*, nebo *App_Start* složky.</span><span class="sxs-lookup"><span data-stu-id="2fd49-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="2fd49-133">Místo toho jsou všechny spuštění úlohy provádějí *Startup.cs* v kořenovém adresáři projektu (viz [spuštění aplikace](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="2fd49-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="2fd49-134">V aplikaci ASP.NET MVC jádra, na základě atributů směrování je nyní zahrnutá ve výchozím nastavení při `UseMvc()` nazývá; a toto je doporučený postup pro konfiguraci tras webového rozhraní API (a je jak starter projekt webového rozhraní API zpracovává směrování).</span><span class="sxs-lookup"><span data-stu-id="2fd49-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

<span data-ttu-id="2fd49-135">Za předpokladu, že chcete použít atribut směrování ve vašem projektu do budoucna, je potřeba žádná další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2fd49-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="2fd49-136">Jednoduše použít atributy podle potřeby řadiče a akce, jak se provádí v ukázce `ValuesController` třídu, která je součástí starter projekt webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="2fd49-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="2fd49-137">Všimněte si přítomnost *[controller]* na řádku 8.</span><span class="sxs-lookup"><span data-stu-id="2fd49-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="2fd49-138">Na základě atributů směrování nyní podporuje určité tokeny, například *[controller]* a *[akce]*.</span><span class="sxs-lookup"><span data-stu-id="2fd49-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="2fd49-139">Tyto tokeny se nahradí za běhu název kontroler nebo akce, v uvedeném pořadí, do které byl použit atribut.</span><span class="sxs-lookup"><span data-stu-id="2fd49-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="2fd49-140">Tato možnost slouží k snížit počet magic řetězců v projektu, a zajišťuje, že trasy se zachová synchronizované s jejich odpovídající kontrolerů a akcí při použití automatické přejmenování refaktoring.</span><span class="sxs-lookup"><span data-stu-id="2fd49-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="2fd49-141">Pokud chcete migrovat kontroler API produktů, jsme musíte nejdříve zkopírovat *ProductsController* do nového projektu.</span><span class="sxs-lookup"><span data-stu-id="2fd49-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="2fd49-142">Pak jednoduše uvést atribut trasy na řadiči:</span><span class="sxs-lookup"><span data-stu-id="2fd49-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="2fd49-143">Také je nutné přidat `[HttpGet]` atribut tyto dvě metody, protože obě by měla být volána prostřednictvím protokolu HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="2fd49-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="2fd49-144">Zahrnout do atribut pro se očekává parametr typu "id" `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="2fd49-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="2fd49-145">Směrování je nyní nakonfigurován správně; ale jsme ho nemůže ještě otestovat.</span><span class="sxs-lookup"><span data-stu-id="2fd49-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="2fd49-146">Další změny, je nutné provést před *ProductsController* bude zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="2fd49-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="2fd49-147">Migrace modely a řadiče</span><span class="sxs-lookup"><span data-stu-id="2fd49-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="2fd49-148">Poslední krok v procesu migrace pro tento jednoduchý projekt webového rozhraní API je zkopírovat přes do řadičů a žádné modely používají.</span><span class="sxs-lookup"><span data-stu-id="2fd49-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="2fd49-149">V takovém případě jednoduše zkopírovat *Controllers/ProductsController.cs* z původního do nového projektu.</span><span class="sxs-lookup"><span data-stu-id="2fd49-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="2fd49-150">Potom zkopírujte celou složku modely z původního projektu do nového.</span><span class="sxs-lookup"><span data-stu-id="2fd49-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="2fd49-151">Upravit obory názvů tak, aby odpovídala nové název projektu (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="2fd49-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="2fd49-152">V tomto okamžiku můžete sestavit aplikace, a zjistíte, určitý počet chyb kompilace.</span><span class="sxs-lookup"><span data-stu-id="2fd49-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="2fd49-153">Obecně musí spadat do následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="2fd49-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="2fd49-154">*Objektu ApiController* neexistuje</span><span class="sxs-lookup"><span data-stu-id="2fd49-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="2fd49-155">*System.Web.Http* obor názvů neexistuje.</span><span class="sxs-lookup"><span data-stu-id="2fd49-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="2fd49-156">*IHttpActionResult* neexistuje</span><span class="sxs-lookup"><span data-stu-id="2fd49-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="2fd49-157">Naštěstí tyto lze velmi snadno opravte:</span><span class="sxs-lookup"><span data-stu-id="2fd49-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="2fd49-158">Změna *objektu ApiController* k *řadič* (budete muset přidat *pomocí Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="2fd49-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="2fd49-159">Odstraní všechny pomocí příkazu odkazující na *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="2fd49-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="2fd49-160">Změňte všechny metoda vrací *IHttpActionResult* vrátit *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="2fd49-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="2fd49-161">Jakmile se tyto změny byly učiněna a nepoužívané pomocí příkazů odebrat, migrovaných *ProductsController* třída vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2fd49-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="2fd49-162">Teď by měla být moci spustit migrované projekt a přejděte do */api/produkty*; a měli byste vidět úplný seznam produktů 3.</span><span class="sxs-lookup"><span data-stu-id="2fd49-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="2fd49-163">Přejděte do */api/products/1* a měli byste vidět první produktu.</span><span class="sxs-lookup"><span data-stu-id="2fd49-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="microsoftaspnetcoremvcwebapicompatshim"></a><span data-ttu-id="2fd49-164">Microsoft.AspNetCore.Mvc.WebApiCompatShim</span><span class="sxs-lookup"><span data-stu-id="2fd49-164">Microsoft.AspNetCore.Mvc.WebApiCompatShim</span></span>

<span data-ttu-id="2fd49-165">Je užitečné nástroje při migraci rozhraní ASP.NET Web API projektů ASP.NET Core [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) knihovny.</span><span class="sxs-lookup"><span data-stu-id="2fd49-165">A useful tool when migrating ASP.NET Web API projects to ASP.NET Core is the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library.</span></span> <span data-ttu-id="2fd49-166">Shim kompatibility rozšiřuje základní technologie ASP.NET povolíte počet různých pravidel webovém rozhraní API 2 má být použit.</span><span class="sxs-lookup"><span data-stu-id="2fd49-166">The compatibility shim extends ASP.NET Core to allow a number of different Web API 2 conventions to be used.</span></span> <span data-ttu-id="2fd49-167">Ukázka, která je součástí dříve v tomto dokumentu je dostatečně základní, že shim kompatibility není nezbytné.</span><span class="sxs-lookup"><span data-stu-id="2fd49-167">The sample ported previously in this document is basic enough that the compatibility shim was not necessary.</span></span> <span data-ttu-id="2fd49-168">U větších projektů pomocí shimu kompatibility může být užitečná pro dočasně přemostění rozhraní API mezera mezi ASP.NET Core a ASP.NET Web API 2.</span><span class="sxs-lookup"><span data-stu-id="2fd49-168">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET Web API 2.</span></span>

<span data-ttu-id="2fd49-169">Shim kompatibility webového rozhraní API je určen pro použití jako dočasné opatření ke zjednodušení migrace velké projekty webového rozhraní API pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2fd49-169">The Web API compatibility shim is meant to be used as a temporary measure to facilitate migrating large Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="2fd49-170">V čase je třeba aktualizovat projekty využít ASP.NET Core sítích, aniž byste museli spoléhat na shim kompatibility.</span><span class="sxs-lookup"><span data-stu-id="2fd49-170">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span> 

<span data-ttu-id="2fd49-171">Kompatibilita funkce obsažené v Microsoft.AspNetCore.Mvc.WebApiCompatShim patří:</span><span class="sxs-lookup"><span data-stu-id="2fd49-171">Compatibility features included in Microsoft.AspNetCore.Mvc.WebApiCompatShim include:</span></span>

* <span data-ttu-id="2fd49-172">Přidá `ApiController` zadejte tak, aby základních typů řadičů není třeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="2fd49-172">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="2fd49-173">Umožňuje webové rozhraní API-style vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="2fd49-173">Enables Web API-style model binding.</span></span> <span data-ttu-id="2fd49-174">Jádro ASP.NET MVC model vazby funkcí podobně MVC 5, ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="2fd49-174">ASP.NET Core MVC model binding functions similarly to MVC 5, by default.</span></span> <span data-ttu-id="2fd49-175">Změny shim kompatibility model vazby více podobný konvence vazby modelu webovém rozhraní API 2.</span><span class="sxs-lookup"><span data-stu-id="2fd49-175">The compatibility shim changes model binding to be more similar to Web API 2 model binding conventions.</span></span> <span data-ttu-id="2fd49-176">Komplexní typy jsou automaticky svázané z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="2fd49-176">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="2fd49-177">Rozšiřuje vazby modelu tak, aby akce kontroleru může trvat parametrů typu `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="2fd49-177">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="2fd49-178">Přidá formátování zpráv umožňuje akce vracet výsledky typu `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="2fd49-178">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="2fd49-179">Přidá další odpovědi metody, které webové rozhraní API 2 akce může mít používají k obsluze odpovědí:</span><span class="sxs-lookup"><span data-stu-id="2fd49-179">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
    * <span data-ttu-id="2fd49-180">Objekt HttpResponseMessage generátory:</span><span class="sxs-lookup"><span data-stu-id="2fd49-180">HttpResponseMessage generators:</span></span>
        * `CreateResponse<T>`
        * `CreateErrorResponse`
    * <span data-ttu-id="2fd49-181">Výsledek metody akce:</span><span class="sxs-lookup"><span data-stu-id="2fd49-181">Action result methods:</span></span>
        * `BadResuestErrorMessageResult`
        * `ExceptionResult`
        * `InternalServerErrorResult`
        * `InvalidModelStateResult`
        * `NegotiatedContentResult`
        * `ResponseMessageResult`
* <span data-ttu-id="2fd49-182">Přidá instanci `IContentNegotiator` do kontejneru DI aplikace a související vyjednávání typů z obsahu díky [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2fd49-182">Adds an instance of `IContentNegotiator` to the app's DI container and makes content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) available.</span></span> <span data-ttu-id="2fd49-183">To zahrnuje typy jako `DefaultContentNegotiator`, `MediaTypeFormatter`atd.</span><span class="sxs-lookup"><span data-stu-id="2fd49-183">This includes types like `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span></span>

<span data-ttu-id="2fd49-184">Pokud chcete používat shim kompatibility, budete muset:</span><span class="sxs-lookup"><span data-stu-id="2fd49-184">To use the compatibility shim, you need to:</span></span>

* <span data-ttu-id="2fd49-185">Odkaz [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="2fd49-185">Reference the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
* <span data-ttu-id="2fd49-186">Registrace služby shim kompatibility s kontejnerem aplikace DI voláním `services.AddWebApiConventions()` do aplikace `Startup.ConfigureServices` metoda.</span><span class="sxs-lookup"><span data-stu-id="2fd49-186">Register the compatibility shim's services with the app's DI container by calling `services.AddWebApiConventions()` in the application's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="2fd49-187">Definování tras specifické pro webové rozhraní API pomocí `MapWebApiRoute` na `IRouteBuilder` do aplikace `IApplicationBuilder.UseMvc` volání.</span><span class="sxs-lookup"><span data-stu-id="2fd49-187">Define Web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the application's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="summary"></a><span data-ttu-id="2fd49-188">Souhrn</span><span class="sxs-lookup"><span data-stu-id="2fd49-188">Summary</span></span>

<span data-ttu-id="2fd49-189">Migrace Jednoduchý projekt webového rozhraní API ASP.NET na ASP.NET MVC základní je docela jasné, thanks integrovanou podporu pro webové rozhraní API v aplikaci ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="2fd49-189">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="2fd49-190">Hlavní součásti, potřebné pro každé projektu ASP.NET Web API pro migraci jsou tras, kontrolerů a modely, společně s typy používané kontrolery a akce aktualizace.</span><span class="sxs-lookup"><span data-stu-id="2fd49-190">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
