---
title: Začínáme s Swashbuckle a ASP.NET Core
author: zuckerthoben
description: Zjistěte, jak přidat do projektu ASP.NET Core webové rozhraní API pro integraci uživatelské rozhraní Swagger Swashbuckle.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 61ff42ebe785d8bf09c62f4909cc1678002a3182
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a>Začínáme s Swashbuckle a ASP.NET Core

Podle [Shayne Boyer](https://twitter.com/spboyer) a [Scott Addie](https://twitter.com/Scott_Addie)

::: moniker range="<= aspnetcore-2.0"
[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle) ([stažení](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle) ([stažení](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end

Existují tři hlavní komponenty k Swashbuckle:

* [Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): Swagger objektový model a middleware vystavit `SwaggerDocument` objekty jako koncové body JSON.

* [Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): Generátor Swagger, který sestaví `SwaggerDocument` objekty přímo z vašeho tras, kontrolerů a modely. Obvykle se zkombinuje s middlewarem koncového bodu Swaggeru automaticky vystavit JSON pro Swagger.

* [Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): embedded verzi nástroje uživatelského rozhraní Swagger. Interpretuje JSON pro Swagger bohaté, přizpůsobitelné prostředí pro popisující funkce webového rozhraní API. Zahrnuje integrovanou testovací nevodivou pro veřejné metody.

## <a name="package-installation"></a>Instalace balíčku

Swashbuckle lze přidat pomocí následujících postupů:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Konzola správce balíčků** okno:
  * Přejděte na **zobrazení** > **jinými** > **Konzola správce balíčků**
  * Přejděte do adresáře, ve kterém *TodoApi.csproj* soubor existuje
  * Spusťte následující příkaz:

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* Z **spravovat balíčky NuGet** dialogové okno:
  * Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** > **spravovat balíčky NuGet**
  * Nastavte **zdroj balíčku** na "nuget.org"
  * Do vyhledávacího pole zadejte "Swashbuckle.AspNetCore"
  * Vyberte balíček "Swashbuckle.AspNetCore" z **Procházet** a klikněte na **instalace**

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Klikněte pravým tlačítkem myši *balíčky* složky v **řešení Pad** > **přidat balíčky...**
* Nastavte **přidat balíčky** uživatele systému Windows **zdroj** rozevírací seznam pro "nuget.org"
* Do vyhledávacího pole zadejte "Swashbuckle.AspNetCore"
* V podokně výsledků vyberte balíček "Swashbuckle.AspNetCore" a klikněte na **přidat balíček**

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Spusťte následující příkaz z **integrované Terminálové**:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Spusťte následující příkaz:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Přidejte a nakonfigurujte Swagger middleware

Přidání generátoru Swagger ke kolekci služby v `Startup.ConfigureServices` metoda:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]
::: moniker-end

Importovat následující názvů použitý `Info` třídy:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

V `Startup.Configure` metoda, povolit middleware pro obsluhující generovaného dokumentu JSON a uživatelské rozhraní Swagger:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

Spusťte aplikaci a přejděte na `http://localhost:<port>/swagger/v1/swagger.json`. Vygenerovaný dokument popisující koncové body se zobrazí, jak je znázorněno v [Swagger specifikace (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).

Uživatelské rozhraní Swagger naleznete na adrese `http://localhost:<port>/swagger`. Prozkoumejte rozhraní API přes uživatelské rozhraní Swagger a začlenit v jiných aplikacích.

> [!TIP]
> K obsluze rozhraní Swagger, v kořenové aplikace (`http://localhost:<port>/`), můžete nastavit `RoutePrefix` vlastnost prázdný řetězec:
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize--extend"></a>Přizpůsobit a rozšířit

Swagger poskytuje možnosti pro dokumentaci v objektovém modelu a přizpůsobení uživatelského rozhraní tak, aby odpovídaly vaší motivu.

### <a name="api-info-and-description"></a>Informace o rozhraní API a popis

Akce konfigurace předaný `AddSwaggerGen` metoda přidá informace, jako autor, licence a popis:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

Uživatelské rozhraní Swagger zobrazí informace na verzi:

![Uživatelské rozhraní swagger se informace o verzi: popis, vytvářet a další odkaz](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>XML – komentáře

XML – komentáře lze je aktivovat pomocí následujících postupů:

#### <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)

* Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **vlastnosti**
* Zkontrolujte **souborů dokumentace XML** pole v části **výstup** části **sestavení** karta

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio for Mac](#tab/visual-studio-mac-xml/)

* Otevřete **možnosti projektu** dialogové okno > **sestavení** > **kompilátoru**
* Zkontrolujte **generování dokumentace xml** pole v části **Obecné možnosti** části

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)

Ručně přidejte následující fragment k *.csproj* souboru:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=2)]

---

Povolení komentáře XML poskytuje informace o ladění pro nedokumentovanými veřejných typů a členů. Upozornění jsou označeny nedokumentovanými typy a členy. Například následující zpráva označuje porušení upozornění kód. 1591:

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

Potlačení upozornění definováním seznam kódů upozornění ignorovat v oddělených středníky *.csproj* souboru:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

Nakonfigurujte Swagger používat generovaný soubor XML. Pro operační systémy jiný systém než Windows nebo Linux může být malá a velká písmena názvů a cest souborů. Například *TodoApi.XML* je soubor na systém Windows, ale CentOS není platný.

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]
::: moniker-end

V předchozí kód [reflexe](/dotnet/csharp/programming-guide/concepts/reflection) slouží k vytvoření, projekt webového rozhraní API odpovídající název souboru XML. Tento přístup zajišťuje, že generovaný název souboru XML odpovídá názvu projektu. [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) vlastnost se používá pro konstrukci cestu k souboru XML.

Přidávání komentářů triple lomítko na akci vylepšuje uživatelské rozhraní Swagger přidáním popis do záhlaví části. Přidat [ \<souhrnné >](/dotnet/csharp/programming-guide/xmldoc/summary) element výše `Delete` akce:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

Vnitřní text předchozí kód zobrazí uživatelské rozhraní Swagger `<summary>` element:

![Zobrazuje komentáře XML odstraní konkrétní TodoItem. uživatelské rozhraní swagger pro metodu DELETE](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

Uživatelské rozhraní vycházejí z generovaného schématu JSON:

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

Přidat [ \<Poznámky >](/dotnet/csharp/programming-guide/xmldoc/remarks) elementu, který chcete `Create` dokumentace metoda akce. Doplňuje informace uvedené v `<summary>` elementu a poskytuje robustnější uživatelského rozhraní Swagger. `<remarks>` Obsah elementu se může skládat z textu JSON a XML.

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]
::: moniker-end

Všimněte si, vylepšení uživatelského rozhraní s tyto další komentáři:

![Uživatelské rozhraní swagger s další komentáři zobrazí](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>Datové poznámky

Uspořádání model s atributy, které jsou součástí [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) oboru názvů, k rozvoji součásti uživatelského rozhraní Swagger.

Přidat `[Required]` atribut `Name` vlastnost `TodoItem` třídy:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

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

Přidat `[Produces("application/json")]` atribut kontroleru rozhraní API. Jejím účelem je deklarovat, že akce kontroleru podporují typ obsahu odpovědi z *application/json*:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]
::: moniker-end

**Typ obsahu odpovědi** rozevíracího seznamu vybere jako výchozí pro akce kontroleru GET tento typ obsahu:

![Uživatelské rozhraní swagger s výchozí typ obsahu odpovědi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

Při použití datové poznámky v rozhraní Web API rostoucí, uživatelského rozhraní a rozhraní API pomohou stránky stát větší popisnosti a užitečné.

### <a name="describe-response-types"></a>Popisují typy odezvy

Využívání vývojáři jsou nejvíce zajímají co je vrácen&mdash;konkrétně odpovědi typy a kódy chyb (Pokud není standard). V XML komentáře a data poznámky jsou rozlišeny odpovědi typy a kódy chyb.

`Create` Akce vrátí kód stavu HTTP 201 v případě úspěchu. Stavový kód HTTP 400 se vrátí při textu požadavku odeslaného má hodnotu null. Bez správné dokumentace v uživatelském rozhraní Swagger nemá příjemce znalost těchto očekávaných výsledků. Tento problém můžete vyřešte přidáním zvýrazněné řádky v následujícím příkladu:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]
::: moniker-end

Uživatelské rozhraní Swagger teď jasně dokumenty očekávané kódy odpovědi HTTP:

![Zobrazuje popis třídy odpovědi POST, vrátí nově vytvořená položka Todo, uživatelské rozhraní swagger a '400 - Pokud položka má hodnotu null' stavový kód a důvod pod odpovědí na zprávy](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a>Přizpůsobení uživatelského rozhraní

Stock uživatelského rozhraní je funkční a přístupné. Ale stránky dokumentace k rozhraní API by měl představovat značku nebo motivu. Branding komponenty Swashbuckle vyžaduje přidání prostředků poskytovat statické soubory a sestavování strukturu složek pro hostování těchto souborů.

Pokud cílení na rozhraní .NET Framework nebo .NET Core 1.x, přidejte [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) balíček NuGet do projektu:

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

Předchozí balíček NuGet je již nainstalován, pokud cílení na .NET Core 2.x a pomocí [metapackage](xref:fundamentals/metapackage).

Povolte middleware statické soubory:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

Získat obsah *dist* složky z [úložiště GitHub uživatelského rozhraní Swagger](https://github.com/swagger-api/swagger-ui/tree/master/dist). Tato složka obsahuje nezbytné prostředky pro stránka uživatelského rozhraní Swagger.

Vytvoření *wwwroot/swagger/uživatelského rozhraní* složku a zkopírujte do ní obsah *dist* složky.

Vytvoření *custom.css* v souboru *wwwroot/swagger/uživatelského rozhraní*, s následující šablon stylů CSS, chcete-li přizpůsobit záhlaví stránky:

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

Referenční dokumentace *custom.css* v *index.html* souboru po další soubory šablon stylů CSS:

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

Vyhledejte *index.html* stránky v `http://localhost:<port>/swagger/ui/index.html`. Zadejte `http://localhost:<port>/swagger/v1/swagger.json` textové pole hlavičky a klikněte na **prozkoumat** tlačítko. Výsledná stránka vypadá takto:

![Uživatelské rozhraní swagger s názvem vlastní hlavičky](web-api-help-pages-using-swagger/_static/custom-header.png)

Je mnohem víc, že můžete provést pomocí stránky. Najdete v části úplné možnosti pro prostředky uživatelského rozhraní na [úložiště GitHub uživatelského rozhraní Swagger](https://github.com/swagger-api/swagger-ui).
