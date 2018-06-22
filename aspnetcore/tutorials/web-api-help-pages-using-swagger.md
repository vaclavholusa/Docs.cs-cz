---
title: Stránky ASP.NET Core Web API nápovědy s Swagger / Open rozhraní API
author: rsuter
description: V tomto kurzu poskytuje návod k přidávání Swagger ke generování dokumentaci, abyste stránky pro aplikaci pomocí webového rozhraní API.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/09/2018
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 56e146337ad9e94298f72abf5ede009eea65fb46
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272248"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--open-api"></a><span data-ttu-id="25d2e-103">Stránky ASP.NET Core Web API nápovědy s Swagger / Open rozhraní API</span><span class="sxs-lookup"><span data-stu-id="25d2e-103">ASP.NET Core Web API help pages with Swagger / Open API</span></span>

<span data-ttu-id="25d2e-104">Podle [Christoph Nienaber](https://twitter.com/zuckerthoben) a [Portoriku Suter](http://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="25d2e-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](http://rsuter.com)</span></span>

<span data-ttu-id="25d2e-105">Při využívání webového rozhraní API, může vývojář náročné pochopení její různé metody.</span><span class="sxs-lookup"><span data-stu-id="25d2e-105">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="25d2e-106">[Swagger](https://swagger.io/), také známé jako otevřené rozhraní API, řeší problém generování užitečné stránky dokumentace a nápovědu pro webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="25d2e-106">[Swagger](https://swagger.io/), also known as Open API, solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="25d2e-107">Poskytuje výhod, třeba interaktivní dokumentace, generování klienta SDK a možnosti rozpoznání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="25d2e-107">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="25d2e-108">V tomto článku [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) a [NSwag](https://github.com/RSuter/NSwag) jsou showcased .NET Swagger implementace:</span><span class="sxs-lookup"><span data-stu-id="25d2e-108">In this article, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) and [NSwag](https://github.com/RSuter/NSwag) .NET Swagger implementations are showcased:</span></span>

* <span data-ttu-id="25d2e-109">**Swashbuckle.AspNetCore** je projektu pro generování dokumenty Swagger pro rozhraní API ASP.NET Core webové aplikace s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="25d2e-109">**Swashbuckle.AspNetCore** is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="25d2e-110">**NSwag** je jiný projekt s otevřeným zdrojem pro integraci [uživatelské rozhraní Swagger](https://swagger.io/swagger-ui/) nebo [ReDoc](https://github.com/Rebilly/ReDoc) do rozhraní API ASP.NET Core Web.</span><span class="sxs-lookup"><span data-stu-id="25d2e-110">**NSwag** is another open source project for integrating [Swagger UI](https://swagger.io/swagger-ui/) or [ReDoc](https://github.com/Rebilly/ReDoc) into ASP.NET Core Web APIs.</span></span> <span data-ttu-id="25d2e-111">Nabízí přístupy k vygenerování C# a TypeScript kód klienta pro vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="25d2e-111">It offers approaches to generate C# and TypeScript client code for your API.</span></span>

## <a name="what-is-swagger--open-api"></a><span data-ttu-id="25d2e-112">Co je Swagger a otevřete rozhraní API?</span><span class="sxs-lookup"><span data-stu-id="25d2e-112">What is Swagger / Open API?</span></span>

<span data-ttu-id="25d2e-113">Swagger je bez ohledu na jazyková specifikace popisujících [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="25d2e-113">Swagger is a language-agnostic specification for describing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) APIs.</span></span> <span data-ttu-id="25d2e-114">Projekt Swagger byl věnován na [OpenAPI Initiative](https://www.openapis.org/), kde ji se nyní označuje jako otevřené rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="25d2e-114">The Swagger project was donated to the [OpenAPI Initiative](https://www.openapis.org/), where it's now referred to as Open API.</span></span> <span data-ttu-id="25d2e-115">Oba názvy jsou použity zcela zaměnitelným významem; je však upřednostňované otevřené rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="25d2e-115">Both names are used interchangeably; however, Open API is preferred.</span></span> <span data-ttu-id="25d2e-116">To umožňuje počítače i lidí na Seznamte se s možnostmi služby bez jakékoli přímý přístup k implementaci (zdrojový kód, přístup k síti, dokumentace).</span><span class="sxs-lookup"><span data-stu-id="25d2e-116">It allows both computers and humans to understand the capabilities of a service without any direct access to the implementation (source code, network access, documentation).</span></span> <span data-ttu-id="25d2e-117">Jeden cíl je minimalizovat množství práce potřebné pro připojení některému programu služby.</span><span class="sxs-lookup"><span data-stu-id="25d2e-117">One goal is to minimize the amount of work needed to connect disassociated services.</span></span> <span data-ttu-id="25d2e-118">Jiné cílem je snížit množství času potřebné pro přesně dokumentu služby.</span><span class="sxs-lookup"><span data-stu-id="25d2e-118">Another goal is to reduce the amount of time needed to accurately document a service.</span></span>

## <a name="swagger-specification-swaggerjson"></a><span data-ttu-id="25d2e-119">Specifikace swagger (swagger.json)</span><span class="sxs-lookup"><span data-stu-id="25d2e-119">Swagger specification (swagger.json)</span></span>

<span data-ttu-id="25d2e-120">Základní s tokem Swagger je specifikace Swagger&mdash;ve výchozím nastavení, dokument s názvem *swagger.json*.</span><span class="sxs-lookup"><span data-stu-id="25d2e-120">The core to the Swagger flow is the Swagger specification&mdash;by default, a document named *swagger.json*.</span></span> <span data-ttu-id="25d2e-121">Je generován Swagger nástroj řetězu (nebo jiných výrobců implementace je) na základě vaší služby.</span><span class="sxs-lookup"><span data-stu-id="25d2e-121">It's generated by the Swagger tool chain (or third-party implementations of it) based on your service.</span></span> <span data-ttu-id="25d2e-122">Popisuje možnosti rozhraní API a jak přístup pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="25d2e-122">It describes the capabilities of your API and how to access it with HTTP.</span></span> <span data-ttu-id="25d2e-123">Ho jednotky uživatelské rozhraní Swagger a řetězu nástroj používá k povolení generování kódu klienta a zjišťování.</span><span class="sxs-lookup"><span data-stu-id="25d2e-123">It drives the Swagger UI and is used by the tool chain to enable discovery and client code generation.</span></span> <span data-ttu-id="25d2e-124">Tady je příklad specifikace Swagger, snížit jako stručný výtah:</span><span class="sxs-lookup"><span data-stu-id="25d2e-124">Here's an example of a Swagger specification, reduced for brevity:</span></span>

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

## <a name="swagger-ui"></a><span data-ttu-id="25d2e-125">Uživatelské rozhraní swagger</span><span class="sxs-lookup"><span data-stu-id="25d2e-125">Swagger UI</span></span>

<span data-ttu-id="25d2e-126">[Uživatelské rozhraní swagger](https://swagger.io/swagger-ui/) nabízí uživatelské rozhraní založené na webu, které poskytuje informace o službě, pomocí generovaného specifikace Swagger.</span><span class="sxs-lookup"><span data-stu-id="25d2e-126">[Swagger UI](https://swagger.io/swagger-ui/) offers a web-based UI that provides information about the service, using the generated Swagger specification.</span></span> <span data-ttu-id="25d2e-127">Swashbuckle a NSwag obsahovat embedded verzi uživatelské rozhraní Swagger, tak, aby může být hostovaný v aplikaci ASP.NET Core pomocí volání registrace middleware.</span><span class="sxs-lookup"><span data-stu-id="25d2e-127">Both Swashbuckle and NSwag include an embedded version of Swagger UI, so that it can be hosted in your ASP.NET Core app using a middleware registration call.</span></span> <span data-ttu-id="25d2e-128">Webové uživatelské rozhraní vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="25d2e-128">The web UI looks like this:</span></span>

![Uživatelské rozhraní swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="25d2e-130">Každá metoda veřejné akce v řadičích může být testována z uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="25d2e-130">Each public action method in your controllers can be tested from the UI.</span></span> <span data-ttu-id="25d2e-131">Klikněte na název metody rozbalte v části.</span><span class="sxs-lookup"><span data-stu-id="25d2e-131">Click a method name to expand the section.</span></span> <span data-ttu-id="25d2e-132">Přidat všechny potřebné parametry a klikněte na tlačítko **vyzkoušejte ji!**.</span><span class="sxs-lookup"><span data-stu-id="25d2e-132">Add any necessary parameters, and click **Try it out!**.</span></span>

![Příklad Swagger získat testu](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> <span data-ttu-id="25d2e-134">Je uživatelské rozhraní Swagger verze použitá pro na snímcích obrazovky je verze 2.</span><span class="sxs-lookup"><span data-stu-id="25d2e-134">The Swagger UI version used for the screenshots is version 2.</span></span> <span data-ttu-id="25d2e-135">Verze 3 příklad najdete v tématu [Petstore příklad](http://petstore.swagger.io/).</span><span class="sxs-lookup"><span data-stu-id="25d2e-135">For a version 3 example, see [Petstore example](http://petstore.swagger.io/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="25d2e-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25d2e-136">Next steps</span></span>

* [<span data-ttu-id="25d2e-137">Začínáme se službou Swashbuckle</span><span class="sxs-lookup"><span data-stu-id="25d2e-137">Get started with Swashbuckle</span></span>](xref:tutorials/get-started-with-swashbuckle)
* [<span data-ttu-id="25d2e-138">Začínáme se službou NSwag</span><span class="sxs-lookup"><span data-stu-id="25d2e-138">Get started with NSwag</span></span>](xref:tutorials/get-started-with-nswag)
