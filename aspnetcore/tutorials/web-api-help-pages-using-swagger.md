---
title: "Základní webové rozhraní API pomůže stránky ASP.NET pomocí Swagger"
author: spboyer
description: "V tomto kurzu poskytuje návod k přidávání Swagger ke generování dokumentaci, abyste stránky pro aplikaci webového rozhraní API."
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 302199bb0b32d4f6610e04455bb28372095e9873
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a>Základní webové rozhraní API pomůže stránky ASP.NET pomocí Swagger

<a name="web-api-help-pages-using-swagger"></a>

Podle [Shayne Boyer](https://twitter.com/spboyer) a [Scott Addie](https://twitter.com/Scott_Addie)

Vysvětlení různých metod rozhraní API může být složité vývojář při sestavování spotřebitelskou aplikací.

Generování správné stránky dokumentace a nápovědu pro vaše webové rozhraní API, pomocí [Swagger](https://swagger.io/) s implementací .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), je stejně snadná jako několik balíčků NuGet pro přidání a úpravy *Startup.cs*.

* [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) je projektu pro generování dokumenty Swagger pro rozhraní API ASP.NET Core webové aplikace s otevřeným zdrojem.

* [Swagger](https://swagger.io/) je strojově čitelným reprezentace rozhraní RESTful API, která umožňuje podporu pro interaktivní dokumentace, generování klienta SDK a možnosti rozpoznání.

V tomto kurzu vychází z vzorku [vytváření vaše první rozhraní Web API s ASP.NET MVC jádra a sady Visual Studio](xref:tutorials/first-web-api). Pokud chcete sledovat, stažení ukázky v [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).

## <a name="getting-started"></a>Začínáme

Existují tři hlavní komponenty k Swashbuckle:

* `Swashbuckle.AspNetCore.Swagger`: Swagger objektový model a middleware vystavit `SwaggerDocument` objekty jako koncové body JSON.

* `Swashbuckle.AspNetCore.SwaggerGen`: Generátor Swagger, který sestaví `SwaggerDocument` objekty přímo z vašeho tras, kontrolerů a modely. Obvykle se zkombinuje s middlewarem koncového bodu Swaggeru automaticky vystavit JSON pro Swagger.

* `Swashbuckle.AspNetCore.SwaggerUI`: embedded verzi nástroj uživatelské rozhraní Swagger, který interpretuje JSON pro Swagger bohaté, přizpůsobitelné prostředí pro popisující funkce webového rozhraní API. Zahrnuje integrovanou testovací nevodivou pro veřejné metody.

## <a name="nuget-packages"></a>Balíčky NuGet

Swashbuckle lze přidat pomocí následujících postupů:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Konzola správce balíčků** okno:

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* Z **spravovat balíčky NuGet** dialogové okno:

     * Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** > **spravovat balíčky NuGet**
     * Nastavte **zdroj balíčku** na "nuget.org"
     * Do vyhledávacího pole zadejte "Swashbuckle.AspNetCore"
     * Vyberte balíček "Swashbuckle.AspNetCore" z **Procházet** a klikněte na **instalace**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Klikněte pravým tlačítkem myši *balíčky* složky v **řešení Pad** > **přidat balíčky...**
* Nastavte **přidat balíčky** uživatele systému Windows **zdroj** rozevírací seznam pro "nuget.org"
* Do vyhledávacího pole zadejte Swashbuckle.AspNetCore
* Vyberte balíček Swashbuckle.AspNetCore v podokně výsledků a klikněte na tlačítko **přidat balíček**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Spusťte následující příkaz z **integrované Terminálové**:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Spusťte následující příkaz:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a>Přidejte a nakonfigurujte Swagger pro middleware

Přidání generátoru Swagger ke kolekci služby v `ConfigureServices` metodu *Startup.cs*:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

Přidejte následující příkaz pro použití `Info` třídy:

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

V `Configure` metodu *Startup.cs*, povolit middleware pro obsluhující generovaného dokumentu JSON a SwaggerUI:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

Spusťte aplikaci a přejděte na `http://localhost:<random_port>/swagger/v1/swagger.json`. Zobrazí se vygenerovaný dokument popisující koncových bodů.

**Poznámka:** Microsoft Edge, Google Chrome a Firefox zobrazení dokumentů JSON nativně. Existují rozšíření pro Chrome, která formátovat dokument pro snadnější čtení. *V následujícím příkladu se snižuje jako stručný výtah.*

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

Tento dokument disky uživatelské rozhraní Swagger, které lze zobrazit přechodem na `http://localhost:<random_port>/swagger`:

![Uživatelské rozhraní swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

Každá metoda veřejné akce v `TodoController` může být testována z uživatelského rozhraní. Klikněte na název metody rozbalte v části. Přidejte všechny potřebné parametry a klikněte na tlačítko "Vyzkoušejte ji!".

![Příklad Swagger získat testu](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a>Přizpůsobení a rozšíření

Swagger poskytuje možnosti pro dokumentaci v objektovém modelu a přizpůsobení uživatelského rozhraní tak, aby odpovídaly vaší motivu.

### <a name="api-info-and-description"></a>Informace o rozhraní API a popis

Akce konfigurace předaný `AddSwaggerGen` metoda slouží k přidání informací jako autor, licence a popis:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

Následující obrázek znázorňuje rozhraní Swagger, zobrazení informací o verzi:

![Uživatelské rozhraní swagger se informace o verzi: popis, vytvářet a další odkaz](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>XML – komentáře

XML – komentáře lze je aktivovat pomocí následujících postupů:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **vlastnosti**
* Zkontrolujte **souborů dokumentace XML** pole v části **výstup** části **sestavení** karty:

![Sestavení projektu vlastností](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Otevřete **možnosti projektu** dialogové okno > **sestavení** > **kompilátoru**
* Zkontrolujte **generování dokumentace xml** pole v části **Obecné možnosti** části:

![Obecné možnosti část možnosti projektu](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Ručně přidejte následující fragment k *.csproj* souboru:

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Najdete v sadě Visual Studio Code.

---

Povolení komentáře XML poskytuje informace o ladění pro nedokumentovanými veřejných typů a členů. Nezdokumentovaný typy a členy indikován upozornění: *komentář chybí XML pro veřejně viditelný typ nebo člen*.

Nakonfigurujte Swagger používat generovaný soubor XML. Pro operační systémy jiný systém než Windows nebo Linux může být malá a velká písmena názvů a cest souborů. Například *ToDoApi.XML* soubor by v systému Windows, ale není CentOS nalezen.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

V předchozí kód `ApplicationBasePath` získá základní cesta aplikace. Základní cesta se používá k vyhledání souboru komentáře XML. *TodoApi.xml* funguje pouze v tomto příkladu vzhledem k tomu, že název generovaného kódu XML komentáře soubor podle názvu aplikace.

Přidávání komentářů triple lomítko metodě vylepšuje uživatelské rozhraní Swagger přidáním popis do záhlaví části:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Zobrazuje komentáře XML odstraní konkrétní TodoItem. uživatelské rozhraní swagger pro metodu DELETE](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

Vygenerovaný soubor JSON, který také obsahuje tyto komentáře doprovází uživatelského rozhraní:

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

Přidat [ <remarks> ](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) označení pro `Create` dokumentace metoda akce. Doplňuje informace uvedené v `<summary>` značky a poskytuje robustnější uživatelské rozhraní Swagger. `<remarks>` Obsah značky se může skládat z textu JSON a XML.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

Všimněte si, vylepšení uživatelského rozhraní s tyto další komentáři.

![Uživatelské rozhraní swagger s další komentáři zobrazí](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>Datových poznámek

Uspořádání model s ve byly nalezeny atributy `System.ComponentModel.DataAnnotations`, aby k rozvoji součásti uživatelského rozhraní Swagger.

Přidat `[Required]` atribut `Name` vlastnost `TodoItem` třídy:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

Přítomnost tento atribut se změní chování uživatelského rozhraní a mění základní schématu JSON:

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

Přidat `[Produces("application/json")]` atribut kontroleru rozhraní API. Jejím účelem je deklarovat podporovat akce kontroleru vrátí typ obsahu *application/json*:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

**Typ obsahu odpovědi** rozevíracího seznamu vybere jako výchozí pro akce kontroleru GET tento typ obsahu:

![Uživatelské rozhraní swagger s výchozí typ obsahu odpovědi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

Při použití datové poznámky v rozhraní Web API rostoucí, uživatelského rozhraní a rozhraní API pomohou stránky stát větší popisnosti a užitečné.

### <a name="describing-response-types"></a>Popisující typy odezvy

Využívání vývojáři jsou nejvíce zajímají co je vrácen &mdash; konkrétně odpovědi typy a kódy chyb (Pokud není standard). Tyto jsou zpracovávány v XML komentáře a data poznámky.

`Create` Akce vrátí `201 Created` v případě úspěchu nebo `400 Bad Request` při textu požadavku odeslaného má hodnotu null. Bez správné dokumentace v uživatelském rozhraní Swagger nemá příjemce znalost těchto očekávaných výsledků. Tento problém vyřešen přidáním zvýrazněné řádky v následujícím příkladu:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

Uživatelské rozhraní Swagger teď jasně dokumenty očekávané kódy odpovědi HTTP:

![Zobrazuje popis třídy odpovědi POST, vrátí nově vytvořená položka Todo, uživatelské rozhraní swagger a '400 - Pokud položka má hodnotu null' stavový kód a důvod pod odpovědí na zprávy](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a>Přizpůsobení uživatelského rozhraní

Stock uživatelského rozhraní je funkční a přístupné; ale při vytváření stránky dokumentace pro vaše rozhraní API, chcete ho k reprezentaci značky nebo motiv. Provedení této úlohy se součástmi Swashbuckle vyžaduje přidání prostředků poskytovat statické soubory a pak vytváření strukturu složek pro hostování těchto souborů.

Pokud cílení na rozhraní .NET Framework, přidejte `Microsoft.AspNetCore.StaticFiles` balíček NuGet do projektu:

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

Povolte middleware statické soubory:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

Získat obsah *dist* složky z [úložiště GitHub uživatelského rozhraní Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist). Tato složka obsahuje nezbytné prostředky pro stránka uživatelského rozhraní Swagger.

Vytvoření *wwwroot/swagger/uživatelského rozhraní* složku a zkopírujte do ní obsah *dist* složky.

Vytvoření *wwwroot/swagger/ui/css/custom.css* soubor s následující šablon stylů CSS, chcete-li přizpůsobit záhlaví stránky:

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

Referenční dokumentace *custom.css* v *index.html* souboru:

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

Vyhledejte *index.html* stránky v `http://localhost:<random_port>/swagger/ui/index.html`. Zadejte `http://localhost:<random_port>/swagger/v1/swagger.json` textové pole hlavičky a klikněte na **prozkoumat** tlačítko. Výsledná stránka vypadá takto:

![Uživatelské rozhraní swagger s názvem vlastní hlavičky](web-api-help-pages-using-swagger/_static/custom-header.png)

Je mnohem víc, že můžete provést pomocí stránky. Najdete v části úplné možnosti pro prostředky uživatelského rozhraní na [úložiště GitHub uživatelského rozhraní Swagger](https://github.com/swagger-api/swagger-ui).
