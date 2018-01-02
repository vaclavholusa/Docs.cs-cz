---
title: "Základní webové rozhraní API pomůže stránky ASP.NET pomocí Swagger"
author: spboyer
description: "V tomto kurzu poskytuje návod k přidávání Swagger ke generování dokumentaci, abyste stránky pro aplikaci webového rozhraní API."
keywords: "ASP.NET Core, Swagger, Swashbuckle, pomůže stránky webového rozhraní API"
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.assetid: 54bb961d-29d9-4dee-8e2c-a93fc33c16f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 630378510f4182034735cb4c306dfc5a761543ab
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2017
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a><span data-ttu-id="4f7e2-104">Základní webové rozhraní API pomůže stránky ASP.NET pomocí Swagger</span><span class="sxs-lookup"><span data-stu-id="4f7e2-104">ASP.NET Core Web API Help Pages using Swagger</span></span>

<a name="web-api-help-pages-using-swagger"></a>

<span data-ttu-id="4f7e2-105">Podle [Shayne Boyer](https://twitter.com/spboyer) a [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="4f7e2-105">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="4f7e2-106">Vysvětlení různých metod rozhraní API může být složité vývojář při sestavování spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-106">Understanding the various methods of an API can be a challenge for a developer when building a consuming application.</span></span>

<span data-ttu-id="4f7e2-107">Generování správné stránky dokumentace a nápovědu pro vaše webové rozhraní API, pomocí [Swagger](https://swagger.io/) s implementací .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), je stejně snadná jako několik balíčků NuGet pro přidání a úpravy *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-107">Generating good documentation and help pages for your Web API, using [Swagger](https://swagger.io/) with the .NET Core implementation [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), is as easy as adding a couple of NuGet packages and modifying the *Startup.cs*.</span></span>

* <span data-ttu-id="4f7e2-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) je projektu pro generování dokumenty Swagger pro rozhraní API ASP.NET Core webové aplikace s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="4f7e2-109">[Swagger](https://swagger.io/) je strojově čitelným reprezentace rozhraní RESTful API, která umožňuje podporu pro interaktivní dokumentace, generování klienta SDK a možnosti rozpoznání.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-109">[Swagger](https://swagger.io/) is a machine-readable representation of a RESTful API that enables support for interactive documentation, client SDK generation, and discoverability.</span></span>

<span data-ttu-id="4f7e2-110">V tomto kurzu vychází z vzorku [vytváření vaše první rozhraní Web API s ASP.NET MVC jádra a sady Visual Studio](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="4f7e2-110">This tutorial builds on the sample on [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="4f7e2-111">Pokud chcete sledovat, stažení ukázky v [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span><span class="sxs-lookup"><span data-stu-id="4f7e2-111">If you'd like to follow along, download the sample at [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span></span>

## <a name="getting-started"></a><span data-ttu-id="4f7e2-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="4f7e2-112">Getting Started</span></span>

<span data-ttu-id="4f7e2-113">Existují tři hlavní komponenty k Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-113">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="4f7e2-114">`Swashbuckle.AspNetCore.Swagger`: Swagger objektový model a middleware vystavit `SwaggerDocument` objekty jako koncové body JSON.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-114">`Swashbuckle.AspNetCore.Swagger`: a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="4f7e2-115">`Swashbuckle.AspNetCore.SwaggerGen`: Generátor Swagger, který sestaví `SwaggerDocument` objekty přímo z vašeho tras, kontrolerů a modely.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-115">`Swashbuckle.AspNetCore.SwaggerGen`: a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="4f7e2-116">Obvykle se zkombinuje s middlewarem koncového bodu Swaggeru automaticky vystavit JSON pro Swagger.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-116">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="4f7e2-117">`Swashbuckle.AspNetCore.SwaggerUI`: embedded verzi nástroj uživatelské rozhraní Swagger, který interpretuje JSON pro Swagger bohaté, přizpůsobitelné prostředí pro popisující funkce webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-117">`Swashbuckle.AspNetCore.SwaggerUI`: an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="4f7e2-118">Zahrnuje integrovanou testovací nevodivou pro veřejné metody.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-118">It includes built-in test harnesses for the public methods.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="4f7e2-119">Balíčky NuGet</span><span class="sxs-lookup"><span data-stu-id="4f7e2-119">NuGet Packages</span></span>

<span data-ttu-id="4f7e2-120">Swashbuckle lze přidat pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-120">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f7e2-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f7e2-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4f7e2-122">Z **Konzola správce balíčků** okno:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-122">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="4f7e2-123">Z **spravovat balíčky NuGet** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-123">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="4f7e2-124">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** > **spravovat balíčky NuGet**</span><span class="sxs-lookup"><span data-stu-id="4f7e2-124">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="4f7e2-125">Nastavte **zdroj balíčku** na "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="4f7e2-125">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="4f7e2-126">Do vyhledávacího pole zadejte "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="4f7e2-126">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="4f7e2-127">Vyberte balíček "Swashbuckle.AspNetCore" z **Procházet** a klikněte na **instalace**</span><span class="sxs-lookup"><span data-stu-id="4f7e2-127">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f7e2-128">Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="4f7e2-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4f7e2-129">Klikněte pravým tlačítkem myši *balíčky* složky v **řešení Pad** > **přidat balíčky...**</span><span class="sxs-lookup"><span data-stu-id="4f7e2-129">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="4f7e2-130">Nastavte **přidat balíčky** uživatele systému Windows **zdroj** rozevírací seznam pro "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="4f7e2-130">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="4f7e2-131">Do vyhledávacího pole zadejte Swashbuckle.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="4f7e2-131">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="4f7e2-132">Vyberte balíček Swashbuckle.AspNetCore v podokně výsledků a klikněte na tlačítko **přidat balíček**</span><span class="sxs-lookup"><span data-stu-id="4f7e2-132">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f7e2-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4f7e2-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4f7e2-134">Spusťte následující příkaz z **integrované Terminálové**:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-134">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4f7e2-135">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="4f7e2-135">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4f7e2-136">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-136">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a><span data-ttu-id="4f7e2-137">Přidejte a nakonfigurujte Swagger pro middleware</span><span class="sxs-lookup"><span data-stu-id="4f7e2-137">Add and configure Swagger to the middleware</span></span>

<span data-ttu-id="4f7e2-138">Přidání generátoru Swagger ke kolekci služby v `ConfigureServices` metodu *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-138">Add the Swagger generator to the services collection in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="4f7e2-139">Přidejte následující příkaz pro použití `Info` třídy:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-139">Add the following using statement for the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="4f7e2-140">V `Configure` metodu *Startup.cs*, povolit middleware pro obsluhující generovaného dokumentu JSON a SwaggerUI:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-140">In the `Configure` method of *Startup.cs*, enable the middleware for serving the generated JSON document and the SwaggerUI:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="4f7e2-141">Spusťte aplikaci a přejděte na `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-141">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="4f7e2-142">Zobrazí se vygenerovaný dokument popisující koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-142">The generated document describing the endpoints appears.</span></span>

<span data-ttu-id="4f7e2-143">**Poznámka:** Microsoft Edge, Google Chrome a Firefox zobrazení dokumentů JSON nativně.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-143">**Note:** Microsoft Edge, Google Chrome, and Firefox display JSON documents natively.</span></span> <span data-ttu-id="4f7e2-144">Existují rozšíření pro Chrome, která formátovat dokument pro snadnější čtení.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-144">There are extensions for Chrome that format the document for easier reading.</span></span> <span data-ttu-id="4f7e2-145">*V následujícím příkladu se snižuje jako stručný výtah.*</span><span class="sxs-lookup"><span data-stu-id="4f7e2-145">*The following example is reduced for brevity.*</span></span>

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

<span data-ttu-id="4f7e2-146">Tento dokument disky uživatelské rozhraní Swagger, které lze zobrazit přechodem na `http://localhost:<random_port>/swagger`:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-146">This document drives the Swagger UI, which can be viewed by navigating to `http://localhost:<random_port>/swagger`:</span></span>

![Uživatelské rozhraní swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="4f7e2-148">Každá metoda veřejné akce v `TodoController` může být testována z uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-148">Each public action method in `TodoController` can be tested from the UI.</span></span> <span data-ttu-id="4f7e2-149">Klikněte na název metody rozbalte v části.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-149">Click a method name to expand the section.</span></span> <span data-ttu-id="4f7e2-150">Přidejte všechny potřebné parametry a klikněte na tlačítko "Vyzkoušejte ji!".</span><span class="sxs-lookup"><span data-stu-id="4f7e2-150">Add any necessary parameters, and click "Try it out!".</span></span>

![Příklad Swagger získat testu](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a><span data-ttu-id="4f7e2-152">Přizpůsobení a rozšíření</span><span class="sxs-lookup"><span data-stu-id="4f7e2-152">Customization & Extensibility</span></span>

<span data-ttu-id="4f7e2-153">Swagger poskytuje možnosti pro dokumentaci v objektovém modelu a přizpůsobení uživatelského rozhraní tak, aby odpovídaly vaší motivu.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-153">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="4f7e2-154">Informace o rozhraní API a popis</span><span class="sxs-lookup"><span data-stu-id="4f7e2-154">API Info and Description</span></span>

<span data-ttu-id="4f7e2-155">Akce konfigurace předaný `AddSwaggerGen` metoda slouží k přidání informací jako autor, licence a popis:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-155">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

<span data-ttu-id="4f7e2-156">Následující obrázek znázorňuje rozhraní Swagger, zobrazení informací o verzi:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-156">The following image depicts the Swagger UI displaying the version information:</span></span>

![Uživatelské rozhraní swagger se informace o verzi: popis, vytvářet a další odkaz](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="4f7e2-158">XML – komentáře</span><span class="sxs-lookup"><span data-stu-id="4f7e2-158">XML Comments</span></span>

<span data-ttu-id="4f7e2-159">XML – komentáře lze je aktivovat pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-159">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f7e2-160">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f7e2-160">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4f7e2-161">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="4f7e2-161">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="4f7e2-162">Zkontrolujte **souborů dokumentace XML** pole v části **výstup** části **sestavení** karty:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-162">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Sestavení projektu vlastností](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f7e2-164">Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="4f7e2-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4f7e2-165">Otevřete **možnosti projektu** dialogové okno > **sestavení** > **kompilátoru**</span><span class="sxs-lookup"><span data-stu-id="4f7e2-165">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="4f7e2-166">Zkontrolujte **generování dokumentace xml** pole v části **Obecné možnosti** části:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-166">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Obecné možnosti část možnosti projektu](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f7e2-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4f7e2-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4f7e2-169">Ručně přidejte následující fragment k *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-169">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4f7e2-170">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="4f7e2-170">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4f7e2-171">Najdete v sadě Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-171">See Visual Studio Code.</span></span>

---

<span data-ttu-id="4f7e2-172">Nakonfigurujte Swagger používat generovaný soubor XML.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-172">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="4f7e2-173">Pro operační systémy jiný systém než Windows nebo Linux může být malá a velká písmena názvů a cest souborů.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-173">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="4f7e2-174">Například *ToDoApi.XML* soubor by v systému Windows, ale není CentOS nalezen.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-174">For example, a *ToDoApi.XML* file would be found on Windows but not CentOS.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="4f7e2-175">V předchozí kód `ApplicationBasePath` získá základní cesta aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-175">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="4f7e2-176">Základní cesta se používá k vyhledání souboru komentáře XML.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-176">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="4f7e2-177">*TodoApi.xml* funguje pouze v tomto příkladu vzhledem k tomu, že název generovaného kódu XML komentáře soubor podle názvu aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-177">*TodoApi.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="4f7e2-178">Přidávání komentářů triple lomítko metodě vylepšuje uživatelské rozhraní Swagger přidáním popis do záhlaví části:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-178">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Zobrazuje komentáře XML odstraní konkrétní TodoItem. uživatelské rozhraní swagger](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="4f7e2-181">Vygenerovaný soubor JSON, který také obsahuje tyto komentáře doprovází uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-181">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="4f7e2-182">Přidat [ <remarks> ](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) označení pro `Create` dokumentace metoda akce.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-182">Add a [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="4f7e2-183">Doplňuje informace uvedené v `<summary>` značky a poskytuje robustnější uživatelské rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-183">It supplements information specified in the `<summary>` tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="4f7e2-184">`<remarks>` Obsah značky se může skládat z textu JSON a XML.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-184">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="4f7e2-185">Všimněte si, vylepšení uživatelského rozhraní s tyto další komentáři.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-185">Notice the UI enhancements with these additional comments.</span></span>

![Uživatelské rozhraní swagger s další komentáři zobrazí](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="4f7e2-187">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="4f7e2-187">Data Annotations</span></span>

<span data-ttu-id="4f7e2-188">Uspořádání model s ve byly nalezeny atributy `System.ComponentModel.DataAnnotations`, aby k rozvoji součásti uživatelského rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-188">Decorate the model with attributes, found in `System.ComponentModel.DataAnnotations`, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="4f7e2-189">Přidat `[Required]` atribut `Name` vlastnost `TodoItem` třídy:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-189">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="4f7e2-190">Přítomnost tento atribut se změní chování uživatelského rozhraní a mění základní schématu JSON:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-190">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="4f7e2-191">Přidat `[Produces("application/json")]` atribut kontroleru rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-191">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="4f7e2-192">Jejím účelem je deklarovat podporovat akce kontroleru vrátí typ obsahu *application/json*:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-192">Its purpose is to declare that the controller's actions support a return a content type of *application/json*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="4f7e2-193">**Typ obsahu odpovědi** rozevíracího seznamu vybere jako výchozí pro akce kontroleru GET tento typ obsahu:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-193">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Uživatelské rozhraní swagger s výchozí typ obsahu odpovědi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="4f7e2-195">Při použití datové poznámky v rozhraní Web API rostoucí, uživatelského rozhraní a rozhraní API pomohou stránky stát větší popisnosti a užitečné.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-195">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describing-response-types"></a><span data-ttu-id="4f7e2-196">Popisující typy odezvy</span><span class="sxs-lookup"><span data-stu-id="4f7e2-196">Describing Response Types</span></span>

<span data-ttu-id="4f7e2-197">Využívání vývojáři jsou nejvíce zajímají co je vrácen &mdash; konkrétně odpovědi typy a kódy chyb (Pokud není standard).</span><span class="sxs-lookup"><span data-stu-id="4f7e2-197">Consuming developers are most concerned with what is returned &mdash; specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="4f7e2-198">Tyto jsou zpracovávány v XML komentáře a data poznámky.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-198">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="4f7e2-199">`Create` Akce vrátí `201 Created` v případě úspěchu nebo `400 Bad Request` při textu požadavku odeslaného má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-199">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="4f7e2-200">Bez správné dokumentace v uživatelském rozhraní Swagger nemá příjemce znalost těchto očekávaných výsledků.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-200">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="4f7e2-201">Tento problém vyřešen přidáním zvýrazněné řádky v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-201">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="4f7e2-202">Uživatelské rozhraní Swagger teď jasně dokumenty očekávané kódy odpovědi HTTP:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-202">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Zobrazuje popis třídy odpovědi POST, vrátí nově vytvořená položka Todo, uživatelské rozhraní swagger a '400 - Pokud položka má hodnotu null' stavový kód a důvod pod odpovědí na zprávy](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a><span data-ttu-id="4f7e2-204">Přizpůsobení uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="4f7e2-204">Customizing the UI</span></span>

<span data-ttu-id="4f7e2-205">Stock uživatelského rozhraní je funkční a přístupné; ale při vytváření stránky dokumentace pro vaše rozhraní API, chcete ho k reprezentaci značky nebo motiv.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-205">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="4f7e2-206">Provedení této úlohy se součástmi Swashbuckle vyžaduje přidání prostředků poskytovat statické soubory a pak vytváření strukturu složek pro hostování těchto souborů.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-206">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="4f7e2-207">Pokud cílení na rozhraní .NET Framework, přidejte `Microsoft.AspNetCore.StaticFiles` balíček NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-207">If targeting .NET Framework, add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="4f7e2-208">Povolte middleware statické soubory:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-208">Enable the static files middleware:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="4f7e2-209">Získat obsah *dist* složky z [úložiště GitHub uživatelského rozhraní Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span><span class="sxs-lookup"><span data-stu-id="4f7e2-209">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="4f7e2-210">Tato složka obsahuje nezbytné prostředky pro stránka uživatelského rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-210">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="4f7e2-211">Vytvoření *wwwroot/swagger/uživatelského rozhraní* složku a zkopírujte do ní obsah *dist* složky.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-211">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="4f7e2-212">Vytvoření *wwwroot/swagger/ui/css/custom.css* soubor s následující šablon stylů CSS, chcete-li přizpůsobit záhlaví stránky:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-212">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="4f7e2-213">Referenční dokumentace *custom.css* v *index.html* souboru:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-213">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="4f7e2-214">Vyhledejte *index.html* stránky v `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-214">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="4f7e2-215">Zadejte `http://localhost:<random_port>/swagger/v1/swagger.json` textové pole hlavičky a klikněte na **prozkoumat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-215">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="4f7e2-216">Výsledná stránka vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="4f7e2-216">The resulting page looks as follows:</span></span>

![Uživatelské rozhraní swagger s názvem vlastní hlavičky](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="4f7e2-218">Je mnohem víc, že můžete provést pomocí stránky.</span><span class="sxs-lookup"><span data-stu-id="4f7e2-218">There is much more you can do with the page.</span></span> <span data-ttu-id="4f7e2-219">Najdete v části úplné možnosti pro prostředky uživatelského rozhraní na [úložiště GitHub uživatelského rozhraní Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="4f7e2-219">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
