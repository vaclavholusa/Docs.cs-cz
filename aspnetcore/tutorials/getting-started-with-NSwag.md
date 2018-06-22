---
title: Začínáme s NSwag a ASP.NET Core
author: zuckerthoben
description: Další informace o použití NSwag generovat dokumentaci a stránky pro webové ASP.NET Core rozhraní API.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: f4cc9ec1f32ef2bd0056ba8d0cbbbe9228834d85
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279194"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>Začínáme s NSwag a ASP.NET Core

Podle [Christoph Nienaber](https://twitter.com/zuckerthoben) a [Portoriku Suter](https://rsuter.com)

::: moniker range="<= aspnetcore-2.0"
[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([stažení](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([stažení](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end

Pomocí [NSwag](https://github.com/RSuter/NSwag) s ASP.NET Core vyžaduje middlewaru [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) balíček NuGet. Balíček se skládá z generátor Swagger, uživatelské rozhraní Swagger (v2 a v3), a [uživatelského rozhraní ReDoc](https://github.com/Rebilly/ReDoc).

Má důrazně doporučujeme používat pro NSwag možnosti generování kódu. Vyberte jednu z následujících možností pro generování kódu:

* Použití [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), aplikace na ploše systému Windows pro generování kódu klienta v C# a TypeScript pro vaše rozhraní API.
* Použití [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) nebo [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) balíčky NuGet pro generování uvnitř projektu kódu.
* Použít NSwag z [příkazového řádku](https://github.com/NSwag/NSwag/wiki/CommandLine).
* Použití [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) balíček NuGet.

## <a name="features"></a>Funkce

Hlavním důvodem pro použití NSwag je schopnost nejen vzniku uživatelské rozhraní Swagger a Swagger generátor, ale ujistěte se, chcete-li použít možnosti generování flexibilní kódu. Nepotřebujete existujícího rozhraní API&mdash;můžete použít rozhraní API třetích stran, která začlenit Swagger a nechat NSwag generovat implementace klienta. V obou případech je rychlé cyklu vývoje a snadno můžete akceptovat změny rozhraní API.

## <a name="package-installation"></a>Instalace balíčku

Balíček NSwag NuGet lze přidat pomocí následujících postupů:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Konzola správce balíčků** okno:
  * Přejděte na **zobrazení** > **jinými** > **Konzola správce balíčků**
  * Přejděte do adresáře, ve kterém *TodoApi.csproj* soubor existuje
  * Spusťte následující příkaz:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* Z **spravovat balíčky NuGet** dialogové okno:
  * Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** > **spravovat balíčky NuGet**
  * Nastavte **zdroj balíčku** na "nuget.org"
  * Do vyhledávacího pole zadejte "NSwag.AspNetCore"
  * Vyberte balíček "NSwag.AspNetCore" z **Procházet** a klikněte na **instalace**

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Klikněte pravým tlačítkem myši *balíčky* složky v **řešení Pad** > **přidat balíčky...**
* Nastavte **přidat balíčky** uživatele systému Windows **zdroj** rozevírací seznam pro "nuget.org"
* Do vyhledávacího pole zadejte "NSwag.AspNetCore"
* V podokně výsledků vyberte balíček "NSwag.AspNetCore" a klikněte na **přidat balíček**

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Spusťte následující příkaz z **integrované Terminálové**:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Spusťte následující příkaz:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Přidejte a nakonfigurujte Swagger middleware

Importovat následující obory názvů v `Startup` třídy:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

V `Startup.Configure` metoda, povolit middleware pro obsluhující generovaného specifikace Swagger a uživatelské rozhraní Swagger:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

Spusťte aplikaci. Přejděte na `http://localhost:<port>/swagger` Chcete-li zobrazit uživatelské rozhraní Swagger. Přejděte na `http://localhost:<port>/swagger/v1/swagger.json` zobrazíte specifikace Swagger.

## <a name="code-generation"></a>Generování kódu

### <a name="via-nswagstudio"></a>Via NSwagStudio

* Nainstalujte NSwagStudio z oficiální [úložiště GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).
* Spusťte NSwagStudio. Zadejte *swagger.json* adresa URL v souboru **adresa URL Swaggeru specifikace** textovému poli a klikněte na tlačítko **vytvořit místní kopii** tlačítko.
* Vyberte **CSharp klienta** klienta výstup typu. Další možnosti zahrnují **TypeScript klienta** a **kontroler Web API CSharp**. Pomocí řadiče webové rozhraní API je v podstatě zpětného generace. Specifikace služby používá k opětovnému sestavení službu.
* Klikněte **generovat výstupy** tlačítko. Na dokončení C# klienta implementace *TodoApi.NSwag* vytváří projektu. Klikněte **CSharp klienta** kartě **výstupy** oddílu kód klienta vygenerovaný:

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> Generování kódu klienta C# na základě nastavení definované v **nastavení** kartě **CSharp klienta** kartě. Změňte nastavení a provádět úkoly jako výchozí obor názvů přejmenování a synchronní metoda generace.

* Zkopírujte vygenerovaný kód C# do souboru v projektu klienta (například [Xamarin.Forms](/xamarin/xamarin-forms/) aplikace).
* Začněte spotřebovávat webové rozhraní API:

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> Základní adresu URL nebo klienta HTTP můžete vložit do rozhraní API klienta. Osvědčeným postupem je vždy [znovu použít HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

### <a name="other-ways-to-generate-client-code"></a>Další způsoby, jak generovat kód klienta

Můžete generovat kód klienta jinými způsoby, další vhodné do pracovního postupu:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [V kódu](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [Šablony T4](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a>Přizpůsobit

Swagger poskytuje možnosti pro objektový model dokumentace k usnadnění používání webové rozhraní API.

### <a name="api-info-and-description"></a>Informace o rozhraní API a popis

V `Startup.Configure` metody akce konfigurace předán `UseSwagger` metoda přidá informace, jako autor, licence a popis:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

Uživatelské rozhraní Swagger zobrazí informace na verzi:

![Uživatelské rozhraní swagger se informace o verzi](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a>XML – komentáře

XML – komentáře jsou povolené pomocí následujících postupů:

# <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)

* Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **vlastnosti**
* Zkontrolujte **souborů dokumentace XML** pole v části **výstup** části **sestavení** karta

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio for Mac](#tab/visual-studio-mac-xml/)

* Otevřete **možnosti projektu** dialogové okno > **sestavení** > **kompilátoru**
* Zkontrolujte **generování dokumentace xml** pole v části **Obecné možnosti** části

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)

Ručně přidejte následující fragment k *.csproj* souboru:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement)]

---

### <a name="data-annotations"></a>Datové poznámky

::: moniker range="<= aspnetcore-2.0"
Používá NSwag [reflexe](/dotnet/csharp/programming-guide/concepts/reflection), a doporučené návratový typ pro akce webového rozhraní API je [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). V důsledku toho NSwag nelze odvodit činnosti akci a návratovou hodnotu. Podívejte se na následující příklad:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Vrátí předchozí akce `IActionResult`, ale uvnitř akce vrací buď [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) nebo [struktura BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Datových poznámek se používají klienti říct, kterých se ví, že tato akce vrátí stavové kódy HTTP. Uspořádání akce s následujícími atributy:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
Používá NSwag [reflexe](/dotnet/csharp/programming-guide/concepts/reflection), a doporučené návratový typ pro akce webového rozhraní API je [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1). V důsledku toho můžete NSwag pouze odvození návratový typ definované `T`. Nejde odvodit další možné návratové typy v akci. Podívejte se na následující příklad:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Vrátí předchozí akce `ActionResult<T>`, ale uvnitř akce vrací buď [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute). Vzhledem k tomu, že kontroler opatřen s [[objektu ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atribut [struktura BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) odpovědi je možné příliš. V tématu [automatické HTTP 400 odpovědí](xref:web-api/index#automatic-http-400-responses) Další informace. Datových poznámek se používají klienti říct, kterých se ví, že tato akce vrátí stavové kódy HTTP. Uspořádání akce s následujícími atributy:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end

Generátor Swagger můžete nyní přesně popisují tato akce a vygenerovaný klienti věděli, co se zobrazí při volání metody koncový bod. Architekturu všechny akce s tyto atributy se důrazně doporučuje. Na jaké odpovědi HTTP vaše rozhraní API akce by měla vrátit, naleznete na adrese [specifikaci RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
