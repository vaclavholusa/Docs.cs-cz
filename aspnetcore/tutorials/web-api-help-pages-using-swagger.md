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
# <a name="aspnet-core-web-api-help-pages-with-swagger--open-api"></a>Stránky ASP.NET Core Web API nápovědy s Swagger / Open rozhraní API

Podle [Christoph Nienaber](https://twitter.com/zuckerthoben) a [Portoriku Suter](http://rsuter.com)

Při využívání webového rozhraní API, může vývojář náročné pochopení její různé metody. [Swagger](https://swagger.io/), také známé jako otevřené rozhraní API, řeší problém generování užitečné stránky dokumentace a nápovědu pro webové rozhraní API. Poskytuje výhod, třeba interaktivní dokumentace, generování klienta SDK a možnosti rozpoznání rozhraní API.

V tomto článku [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) a [NSwag](https://github.com/RSuter/NSwag) jsou showcased .NET Swagger implementace:

* **Swashbuckle.AspNetCore** je projektu pro generování dokumenty Swagger pro rozhraní API ASP.NET Core webové aplikace s otevřeným zdrojem.

* **NSwag** je jiný projekt s otevřeným zdrojem pro integraci [uživatelské rozhraní Swagger](https://swagger.io/swagger-ui/) nebo [ReDoc](https://github.com/Rebilly/ReDoc) do rozhraní API ASP.NET Core Web. Nabízí přístupy k vygenerování C# a TypeScript kód klienta pro vaše rozhraní API.

## <a name="what-is-swagger--open-api"></a>Co je Swagger a otevřete rozhraní API?

Swagger je bez ohledu na jazyková specifikace popisujících [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) rozhraní API. Projekt Swagger byl věnován na [OpenAPI Initiative](https://www.openapis.org/), kde ji se nyní označuje jako otevřené rozhraní API. Oba názvy jsou použity zcela zaměnitelným významem; je však upřednostňované otevřené rozhraní API. To umožňuje počítače i lidí na Seznamte se s možnostmi služby bez jakékoli přímý přístup k implementaci (zdrojový kód, přístup k síti, dokumentace). Jeden cíl je minimalizovat množství práce potřebné pro připojení některému programu služby. Jiné cílem je snížit množství času potřebné pro přesně dokumentu služby.

## <a name="swagger-specification-swaggerjson"></a>Specifikace swagger (swagger.json)

Základní s tokem Swagger je specifikace Swagger&mdash;ve výchozím nastavení, dokument s názvem *swagger.json*. Je generován Swagger nástroj řetězu (nebo jiných výrobců implementace je) na základě vaší služby. Popisuje možnosti rozhraní API a jak přístup pomocí protokolu HTTP. Ho jednotky uživatelské rozhraní Swagger a řetězu nástroj používá k povolení generování kódu klienta a zjišťování. Tady je příklad specifikace Swagger, snížit jako stručný výtah:

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

## <a name="swagger-ui"></a>Uživatelské rozhraní swagger

[Uživatelské rozhraní swagger](https://swagger.io/swagger-ui/) nabízí uživatelské rozhraní založené na webu, které poskytuje informace o službě, pomocí generovaného specifikace Swagger. Swashbuckle a NSwag obsahovat embedded verzi uživatelské rozhraní Swagger, tak, aby může být hostovaný v aplikaci ASP.NET Core pomocí volání registrace middleware. Webové uživatelské rozhraní vypadá takto:

![Uživatelské rozhraní swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

Každá metoda veřejné akce v řadičích může být testována z uživatelského rozhraní. Klikněte na název metody rozbalte v části. Přidat všechny potřebné parametry a klikněte na tlačítko **vyzkoušejte ji!**.

![Příklad Swagger získat testu](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> Je uživatelské rozhraní Swagger verze použitá pro na snímcích obrazovky je verze 2. Verze 3 příklad najdete v tématu [Petstore příklad](http://petstore.swagger.io/).

## <a name="next-steps"></a>Další kroky

* [Začínáme se službou Swashbuckle](xref:tutorials/get-started-with-swashbuckle)
* [Začínáme se službou NSwag](xref:tutorials/get-started-with-nswag)
