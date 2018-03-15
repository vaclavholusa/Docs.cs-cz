---
title: "Začínáme s NSwag"
author: zuckerthoben
description: "V tomto kurzu poskytuje návod k přidávání NSwag generovat dokumentaci a stránky pro aplikaci pomocí webového rozhraní API."
keywords: "ASP.NET Core, Swagger, NSwag, pomůže stránky webového rozhraní API"
ms.author: scaddie
manager: wpickett
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: ce30ad3538c6294003a4f38ca80ebd73c0f52542
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="get-started-with-nswag"></a>Začínáme s NSwag

Podle [Christoph Nienaber](https://twitter.com/zuckerthoben) a [Portoriku Suter](https://rsuter.com)

Pomocí [NSwag](https://github.com/RSuter/NSwag) s ASP.NET Core vyžaduje middlewaru [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) balíček NuGet. Balíček se skládá z generátor Swagger, uživatelské rozhraní Swagger (v2 a v3), a [uživatelského rozhraní ReDoc](https://github.com/Rebilly/ReDoc).

Má důrazně doporučujeme používat pro NSwag možnosti generování kódu. Vyberte jednu z následujících možností pro generování kódu:

* Použití [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), aplikace na ploše systému Windows pro generování kódu klienta pro vaše rozhraní API v C# a TypeScript
* Použití [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) nebo [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) balíčky NuGet pro generování uvnitř projektu kódu
* Použít NSwag z [příkazového řádku](https://github.com/NSwag/NSwag/wiki/CommandLine)
* Použití [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) balíček NuGet

## <a name="features"></a>Funkce

Hlavním důvodem pro použití NSwag je schopnost nejen vzniku uživatelské rozhraní Swagger a Swagger generátor, ale ujistěte se, chcete-li použít možnosti generování flexibilní kódu. Nepotřebujete existujícího rozhraní API&mdash;můžete použít rozhraní API třetích stran, která začlenit Swagger a nechat NSwag generovat implementace klienta. V obou případech je rychlé cyklu vývoje a snadno můžete akceptovat změny rozhraní API.

## <a name="package-installation"></a>Instalace balíčku

Balíček NSwag NuGet lze přidat pomocí následujících postupů:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Konzola správce balíčků** okno:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* Z **spravovat balíčky NuGet** dialogové okno:
  * Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** > **spravovat balíčky NuGet**
  * Nastavte **zdroj balíčku** na "nuget.org"
  * Do vyhledávacího pole zadejte "NSwag.AspNetCore"
  * Vyberte balíček "NSwag.AspNetCore" z **Procházet** a klikněte na **instalace**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Klikněte pravým tlačítkem myši *balíčky* složky v **řešení Pad** > **přidat balíčky...**
* Nastavte **přidat balíčky** uživatele systému Windows **zdroj** rozevírací seznam pro "nuget.org"
* Do vyhledávacího pole zadejte NSwag.AspNetCore
* Vyberte balíček NSwag.AspNetCore v podokně výsledků a klikněte na tlačítko **přidat balíček**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Spusťte následující příkaz z **integrované Terminálové**:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Spusťte následující příkaz:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Přidejte a nakonfigurujte Swagger middleware

Importovat následující obory názvů v `Info` třídy:

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

V `Startup.Configure` metoda, povolit middleware pro obsluhující generovaného specifikace Swagger a uživatelské rozhraní Swagger:

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

Spusťte aplikaci. Přejděte na `/swagger` Chcete-li zobrazit uživatelské rozhraní Swagger. Přejděte na `/swagger/v1/swagger.json` zobrazíte specifikace Swagger.

## <a name="code-generation"></a>Generování kódu

### <a name="via-nswagstudio"></a>Via NSwagStudio

* Nainstalujte `NSwagStudio` z oficiálního [úložiště GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).

* Spusťte NSwagStudio. Zadejte umístění vaší *swagger.json* nebo ho zkopírujte přímo:

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* Označuje typ výstupu požadované klienta. Mezi možnosti patří **TypeScript klienta**, **CSharp klienta**, nebo **kontroler Web API CSharp**. Pomocí řadiče webové rozhraní API je v podstatě zpětného generace. Specifikace služby používá k opětovnému sestavení službu.

* Klikněte na tlačítko **generovat výstupy**.

* Tady vidíte na implementace klientů *TodoApi.NSwag* ukázku v jazyce C#:

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* Uložte soubor do projektu klienta (například [Xamarin.Forms](/xamarin/xamarin-forms/) aplikace). Začněte spotřebovávat rozhraní API:

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> Základní adresu URL nebo klienta HTTP můžete vložit do rozhraní API klienta. Osvědčeným postupem je vždy [znovu použít HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

Nyní můžete spustit snadno implementace rozhraní API do projektů klienta.

### <a name="other-ways-to-generate-client-code"></a>Další způsoby, jak generovat kód klienta

Můžete generovat kód jinými způsoby, další vhodné do pracovního postupu:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [V kódu](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [Šablony T4](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a>Přizpůsobení

### <a name="xml-comments"></a>XML – komentáře

XML – komentáře jsou povolené pomocí následujících postupů:

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

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

---

## <a name="data-annotations"></a>Datové poznámky

Používá NSwag [reflexe](/dotnet/csharp/programming-guide/concepts/reflection), a doporučený postup pro akce webového rozhraní API je vrátit [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). V důsledku toho NSwag nelze odvodit činnosti akci a návratovou hodnotu. Podívejte se na následující příklad:

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

Vrátí předchozí akce `IActionResult`, ale uvnitř akce vrací buď [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) nebo [struktura BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Poznámky data se používají k sdělte který odpověď HTTP, tato akce vrací klientů. Uspořádání akce s následujícími atributy:

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

Generátor Swagger můžete nyní přesně popisují tato akce a vygenerovaný klienti věděli, co se zobrazí při volání metody koncový bod. Architekturu všechny akce s tyto atributy se důrazně doporučuje. Na jaké odpovědi HTTP vaše rozhraní API akce by měla vrátit, naleznete na adrese [specifikaci RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
