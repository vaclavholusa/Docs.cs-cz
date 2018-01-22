---
title: "Migrace z rozhraní ASP.NET Web API"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/webapi
ms.openlocfilehash: aef00b63c9889100116facc610bec99f889e4c46
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-from-aspnet-web-api"></a><span data-ttu-id="b231b-102">Migrace z rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="b231b-102">Migrating from ASP.NET Web API</span></span>

<span data-ttu-id="b231b-103">Podle [Steve Smith](https://ardalis.com/) a [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="b231b-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="b231b-104">Webová rozhraní API jsou služeb HTTP, které využity širokou škálou klientů včetně prohlížečů a mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="b231b-104">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="b231b-105">Jádro ASP.NET MVC zahrnuje podporu pro vytváření webových rozhraní API poskytuje jednoduchý a konzistentní způsob vytváření webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="b231b-105">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="b231b-106">V tomto článku jsme ukazují na kroky potřebné k migraci implementace webového rozhraní API z webového rozhraní API ASP.NET na ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="b231b-106">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="b231b-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b231b-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="b231b-108">Zkontrolujte ASP.NET Web API projektu</span><span class="sxs-lookup"><span data-stu-id="b231b-108">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="b231b-109">Tento článek používá ukázkový projekt *ProductsApp*, které byly vytvořeny v článku [Začínáme s rozhraním ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) jako výchozí bod.</span><span class="sxs-lookup"><span data-stu-id="b231b-109">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="b231b-110">V tomto projektu jednoduché projektu ASP.NET Web API nakonfigurovat následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="b231b-110">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="b231b-111">V *Global.asax.cs*, Přišla žádost o `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="b231b-111">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="b231b-112">`WebApiConfig`je definována v *App_Start*, a má jenom jeden statický `Register` metoda:</span><span class="sxs-lookup"><span data-stu-id="b231b-112">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="b231b-113">Tato třída nakonfiguruje [směrováním atributů](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), i když je ve skutečnosti není používán v projektu.</span><span class="sxs-lookup"><span data-stu-id="b231b-113">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="b231b-114">Nakonfiguruje taky do směrovací tabulky, který je používán rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="b231b-114">It also configures the routing table which is used by ASP.NET Web API.</span></span> <span data-ttu-id="b231b-115">V takovém případě bude rozhraní ASP.NET Web API očekávat adres URL pro odpovídat formátu */api/ {controller} / {id}*, s *{id}* je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="b231b-115">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="b231b-116">*ProductsApp* projekt obsahuje pouze jednu jednoduché řadiči, který dědí z `ApiController` a zveřejňuje dvě metody:</span><span class="sxs-lookup"><span data-stu-id="b231b-116">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="b231b-117">Nakonec model *produktu*používané nástrojem *ProductsApp*, je jednoduchá:</span><span class="sxs-lookup"><span data-stu-id="b231b-117">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="b231b-118">Teď, když máme Jednoduchý projekt, ze kterého má začít, budeme ukazují, jak migrovat tento projekt webového rozhraní API ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="b231b-118">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="b231b-119">Vytvoření projektu cílové</span><span class="sxs-lookup"><span data-stu-id="b231b-119">Create the Destination Project</span></span>

<span data-ttu-id="b231b-120">Pomocí sady Visual Studio, vytvořte nový, prázdný řešení a pojmenujte ji *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="b231b-120">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="b231b-121">Přidat existující *ProductsApp* projektu do ní a pak přidejte nový projekt webové aplikace ASP.NET Core do řešení.</span><span class="sxs-lookup"><span data-stu-id="b231b-121">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="b231b-122">Název nového projektu *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="b231b-122">Name the new project *ProductsCore*.</span></span>

![Otevřete dialogové okno Nový projekt webové šablon](webapi/_static/add-web-project.png)

<span data-ttu-id="b231b-124">V dalším kroku vyberte šablona projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b231b-124">Next, choose the Web API project template.</span></span> <span data-ttu-id="b231b-125">Jsme bude migrovat *ProductsApp* obsah do nového projektu.</span><span class="sxs-lookup"><span data-stu-id="b231b-125">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Dialogové okno nové webové aplikace pomocí šablony projektu webového rozhraní API v seznamu šablon ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="b231b-127">Odstranit `Project_Readme.html` soubor z nového projektu.</span><span class="sxs-lookup"><span data-stu-id="b231b-127">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="b231b-128">Řešení by měl nyní vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="b231b-128">Your solution should now look like this:</span></span>

![Otevřít v Průzkumníkovi řešení zobrazující soubory a složky z projektů ProductsApp a ProductsCore řešení aplikace](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="b231b-130">Migruje konfiguraci</span><span class="sxs-lookup"><span data-stu-id="b231b-130">Migrate Configuration</span></span>

<span data-ttu-id="b231b-131">Už používá ASP.NET Core *Global.asax*, *web.config*, nebo *App_Start* složky.</span><span class="sxs-lookup"><span data-stu-id="b231b-131">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="b231b-132">Místo toho jsou všechny spuštění úlohy provádějí *Startup.cs* v kořenovém adresáři projektu (viz [spuštění aplikace](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="b231b-132">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="b231b-133">V aplikaci ASP.NET MVC jádra, na základě atributů směrování je nyní zahrnutá ve výchozím nastavení při `UseMvc()` nazývá; a toto je doporučený postup pro konfiguraci tras webového rozhraní API (a je jak starter projekt webového rozhraní API zpracovává směrování).</span><span class="sxs-lookup"><span data-stu-id="b231b-133">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]

<span data-ttu-id="b231b-134">Za předpokladu, že chcete použít atribut směrování ve vašem projektu do budoucna, je potřeba žádná další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b231b-134">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="b231b-135">Jednoduše použít atributy podle potřeby řadiče a akce, jak se provádí v ukázce `ValuesController` třídu, která je součástí starter projekt webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="b231b-135">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that is included in the Web API starter project:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="b231b-136">Všimněte si přítomnost *[controller]* na řádku 8.</span><span class="sxs-lookup"><span data-stu-id="b231b-136">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="b231b-137">Na základě atributů směrování nyní podporuje určité tokeny, například *[controller]* a *[akce]*.</span><span class="sxs-lookup"><span data-stu-id="b231b-137">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="b231b-138">Tyto tokeny se nahradí za běhu název kontroler nebo akce, v uvedeném pořadí, do které byl použit atribut.</span><span class="sxs-lookup"><span data-stu-id="b231b-138">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="b231b-139">Tato možnost slouží k snížit počet magic řetězců v projektu, a zajišťuje, že trasy se zachová synchronizované s jejich odpovídající kontrolerů a akcí při použití automatické přejmenování refaktoring.</span><span class="sxs-lookup"><span data-stu-id="b231b-139">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="b231b-140">Pokud chcete migrovat kontroler API produktů, jsme musíte nejdříve zkopírovat *ProductsController* do nového projektu.</span><span class="sxs-lookup"><span data-stu-id="b231b-140">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="b231b-141">Pak jednoduše uvést atribut trasy na řadiči:</span><span class="sxs-lookup"><span data-stu-id="b231b-141">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="b231b-142">Také je nutné přidat `[HttpGet]` atribut tyto dvě metody, protože obě by měla být volána prostřednictvím protokolu HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="b231b-142">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="b231b-143">Zahrnout do atribut pro se očekává parametr typu "id" `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="b231b-143">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="b231b-144">Směrování je nyní nakonfigurován správně; ale jsme ho nemůže ještě otestovat.</span><span class="sxs-lookup"><span data-stu-id="b231b-144">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="b231b-145">Další změny, je nutné provést před *ProductsController* bude zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="b231b-145">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="b231b-146">Migrace modely a řadiče</span><span class="sxs-lookup"><span data-stu-id="b231b-146">Migrate Models and Controllers</span></span>

<span data-ttu-id="b231b-147">Poslední krok v procesu migrace pro tento jednoduchý projekt webového rozhraní API je zkopírovat přes do řadičů a žádné modely používají.</span><span class="sxs-lookup"><span data-stu-id="b231b-147">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="b231b-148">V takovém případě jednoduše zkopírovat *Controllers/ProductsController.cs* z původního do nového projektu.</span><span class="sxs-lookup"><span data-stu-id="b231b-148">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="b231b-149">Potom zkopírujte celou složku modely z původního projektu do nového.</span><span class="sxs-lookup"><span data-stu-id="b231b-149">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="b231b-150">Upravit obory názvů tak, aby odpovídala nové název projektu (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="b231b-150">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="b231b-151">V tomto okamžiku můžete sestavit aplikace, a zjistíte, určitý počet chyb kompilace.</span><span class="sxs-lookup"><span data-stu-id="b231b-151">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="b231b-152">Obecně musí spadat do následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="b231b-152">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="b231b-153">*Objektu ApiController* neexistuje</span><span class="sxs-lookup"><span data-stu-id="b231b-153">*ApiController* does not exist</span></span>

* <span data-ttu-id="b231b-154">*System.Web.Http* namespace does not exist</span><span class="sxs-lookup"><span data-stu-id="b231b-154">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="b231b-155">*IHttpActionResult* neexistuje</span><span class="sxs-lookup"><span data-stu-id="b231b-155">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="b231b-156">Naštěstí tyto lze velmi snadno opravte:</span><span class="sxs-lookup"><span data-stu-id="b231b-156">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="b231b-157">Změna *objektu ApiController* k *řadič* (budete muset přidat *pomocí Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="b231b-157">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="b231b-158">Odstraní všechny pomocí příkazu odkazující na *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="b231b-158">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="b231b-159">Změňte všechny metoda vrací *IHttpActionResult* vrátit *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="b231b-159">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="b231b-160">Jakmile se tyto změny byly učiněna a nepoužívané pomocí příkazů odebrat, migrovaných *ProductsController* třída vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="b231b-160">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="b231b-161">Teď by měla být moci spustit migrované projekt a přejděte do */api/produkty*; a měli byste vidět úplný seznam produktů 3.</span><span class="sxs-lookup"><span data-stu-id="b231b-161">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="b231b-162">Přejděte do */api/products/1* a měli byste vidět první produktu.</span><span class="sxs-lookup"><span data-stu-id="b231b-162">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="summary"></a><span data-ttu-id="b231b-163">Souhrn</span><span class="sxs-lookup"><span data-stu-id="b231b-163">Summary</span></span>

<span data-ttu-id="b231b-164">Migrace Jednoduchý projekt webového rozhraní API ASP.NET na ASP.NET MVC základní je docela jasné, thanks integrovanou podporu pro webové rozhraní API v aplikaci ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="b231b-164">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="b231b-165">Hlavní součásti, potřebné pro každé projektu ASP.NET Web API pro migraci jsou tras, kontrolerů a modely, společně s typy používané kontrolery a akce aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b231b-165">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
