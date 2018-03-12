---
title: "Začínáme s Swashbuckle"
author: zuckerthoben
description: "V tomto kurzu poskytuje návod k přidávání Swashbuckle do projektu integrovat uživatelské rozhraní Swagger."
keywords: "ASP.NET Core, Swagger, Swashbuckle, pomůže stránky webového rozhraní API"
ms.author: scaddie
manager: wpickett
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 705fbe7c3aa65fed89f5fda02af3cb8dd757b71c
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="get-started-with-swashbuckle"></a>Začínáme s Swashbuckle

Podle [Shayne Boyer](https://twitter.com/spboyer) a [Scott Addie](https://twitter.com/Scott_Addie)

Existují tři hlavní komponenty k Swashbuckle:

* [Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): Swagger objektový model a middleware vystavit `SwaggerDocument` objekty jako koncové body JSON.

* [Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): Generátor Swagger, který sestaví `SwaggerDocument` objekty přímo z vašeho tras, kontrolerů a modely. Obvykle se zkombinuje s middlewarem koncového bodu Swaggeru automaticky vystavit JSON pro Swagger.

* [Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): embedded verzi nástroj uživatelské rozhraní Swagger, který interpretuje JSON pro Swagger bohaté, přizpůsobitelné prostředí pro popisující funkce webového rozhraní API. Zahrnuje integrovanou testovací nevodivou pro veřejné metody.

## <a name="package-installation"></a>Instalace balíčku

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
dotnet add TodoApi.Swashbuckle.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Spusťte následující příkaz:

```console
dotnet add TodoApi.Swashbuckle.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Přidejte a nakonfigurujte Swagger middleware

Přidání generátoru Swagger ke kolekci služby v `Startup.ConfigureServices` metoda:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

Importovat následující obory názvů v `Info` třídy:

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

V `Startup.Configure` metoda, povolit middleware pro obsluhující generovaného dokumentu JSON a uživatelské rozhraní Swagger:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

Spusťte aplikaci a přejděte na `http://localhost:<random_port>/swagger/v1/swagger.json`. Vygenerovaný dokument popisující koncové body se zobrazí, jak je znázorněno v [Swagger specifikace (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).

Uživatelské rozhraní Swagger naleznete na adrese `http://localhost:<random_port>/swagger`. Nyní vám umožní zkoumat rozhraní API přes uživatelské rozhraní Swagger a začlenit v jiných aplikacích.

## <a name="customize--extend"></a>Přizpůsobit a rozšířit

Swagger poskytuje možnosti pro dokumentaci v objektovém modelu a přizpůsobení uživatelského rozhraní tak, aby odpovídaly vaší motivu.

### <a name="api-info-and-description"></a>Informace o rozhraní API a popis

Akce konfigurace předaný `AddSwaggerGen` metoda slouží k přidání informací jako autor, licence a popis:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?range=20-30,36)]

Následující obrázek znázorňuje rozhraní Swagger, zobrazení informací o verzi:

![Uživatelské rozhraní swagger se informace o verzi: popis, vytvářet a další odkaz](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>XML – komentáře

XML – komentáře lze je aktivovat pomocí následujících postupů:

# <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml)

* Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **vlastnosti**
* Zkontrolujte **souborů dokumentace XML** pole v části **výstup** části **sestavení** karty:

![Sestavení projektu vlastností](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio for Mac](#tab/visual-studio-mac-xml)

* Otevřete **možnosti projektu** dialogové okno > **sestavení** > **kompilátoru**
* Zkontrolujte **generování dokumentace xml** pole v části **Obecné možnosti** části:

![Obecné možnosti část možnosti projektu](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml)

Ručně přidejte následující fragment k *.csproj* souboru:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/TodoApiSwashbuckle.csproj?range=7-9)]

---

Nakonfigurujte Swagger používat generovaný soubor XML. Pro operační systémy jiný systém než Windows nebo Linux může být malá a velká písmena názvů a cest souborů. Například *TodoApi.Swashbuckle.XML* je soubor na systém Windows, ale CentOS není platný.

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

V předchozí kód `ApplicationBasePath` získá základní cesta aplikace. Základní cesta se používá k vyhledání souboru komentáře XML. *TodoApi.Swashbuckle.xml* funguje pouze v tomto příkladu vzhledem k tomu, že název generovaného kódu XML komentáře soubor podle názvu aplikace.

Přidávání komentářů triple lomítko metodě vylepšuje uživatelské rozhraní Swagger přidáním popis do záhlaví části:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

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

Přidat [ \<Poznámky >](/dotnet/csharp/programming-guide/xmldoc/remarks) označení pro `Create` dokumentace metoda akce. Doplňuje informace uvedené v [ \<souhrnné >](/dotnet/csharp/programming-guide/xmldoc/summary) značky a poskytuje robustnější uživatelské rozhraní Swagger. `<remarks>` Obsah značky se může skládat z textu JSON a XML.

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

Všimněte si, vylepšení uživatelského rozhraní s tyto další komentáři.

![Uživatelské rozhraní swagger s další komentáři zobrazí](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>Datové poznámky

Uspořádání model s atributy, které jsou součástí [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) oboru názvů, k rozvoji součásti uživatelského rozhraní Swagger.

Přidat `[Required]` atribut `Name` vlastnost `TodoItem` třídy:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

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

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

**Typ obsahu odpovědi** rozevíracího seznamu vybere jako výchozí pro akce kontroleru GET tento typ obsahu:

![Uživatelské rozhraní swagger s výchozí typ obsahu odpovědi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

Při použití datové poznámky v rozhraní Web API rostoucí, uživatelského rozhraní a rozhraní API pomohou stránky stát větší popisnosti a užitečné.

### <a name="describe-response-types"></a>Popisují typy odezvy

Využívání vývojáři jsou nejvíce zajímají co je vrácen&mdash;konkrétně odpovědi typy a kódy chyb (Pokud není standard). Tyto jsou zpracovávány v XML komentáře a data poznámky.

`Create` Akce vrátí `201 Created` v případě úspěchu nebo `400 Bad Request` při textu požadavku odeslaného má hodnotu null. Bez správné dokumentace v uživatelském rozhraní Swagger nemá příjemce znalost těchto očekávaných výsledků. Tento problém vyřešen přidáním zvýrazněné řádky v následujícím příkladu:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

Uživatelské rozhraní Swagger teď jasně dokumenty očekávané kódy odpovědi HTTP:

![Zobrazuje popis třídy odpovědi POST, vrátí nově vytvořená položka Todo, uživatelské rozhraní swagger a '400 - Pokud položka má hodnotu null' stavový kód a důvod pod odpovědí na zprávy](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a>Přizpůsobení uživatelského rozhraní

Stock uživatelského rozhraní je funkční a přístupné; ale při vytváření stránky dokumentace pro vaše rozhraní API, chcete ho k reprezentaci značky nebo motiv. Provedení této úlohy se součástmi Swashbuckle vyžaduje přidání prostředků poskytovat statické soubory a pak vytváření strukturu složek pro hostování těchto souborů.

Pokud cílení na rozhraní .NET Framework nebo .NET Core 1.x, přidejte [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) balíček NuGet do projektu:

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

Předchozí balíček NuGet je již nainstalován, pokud cílení na .NET Core 2.x a pomocí [metapackage](xref:fundamentals/metapackage).

Povolte middleware statické soubory:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

Získat obsah *dist* složky z [úložiště GitHub uživatelského rozhraní Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist). Tato složka obsahuje nezbytné prostředky pro stránka uživatelského rozhraní Swagger.

Vytvoření *wwwroot/swagger/uživatelského rozhraní* složku a zkopírujte do ní obsah *dist* složky.

Vytvoření *wwwroot/swagger/ui/css/custom.css* soubor s následující šablon stylů CSS, chcete-li přizpůsobit záhlaví stránky:

[!code-css[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/wwwroot/swagger/ui/css/custom.css)]

Referenční dokumentace *custom.css* v *index.html* souboru:

[!code-html[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?range=14)]

Vyhledejte *index.html* stránky v `http://localhost:<random_port>/swagger/ui/index.html`. Zadejte `http://localhost:<random_port>/swagger/v1/swagger.json` textové pole hlavičky a klikněte na **prozkoumat** tlačítko. Výsledná stránka vypadá takto:

![Uživatelské rozhraní swagger s názvem vlastní hlavičky](web-api-help-pages-using-swagger/_static/custom-header.png)

Je mnohem víc, že můžete provést pomocí stránky. Najdete v části úplné možnosti pro prostředky uživatelského rozhraní na [úložiště GitHub uživatelského rozhraní Swagger](https://github.com/swagger-api/swagger-ui).
