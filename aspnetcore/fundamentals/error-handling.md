---
title: Zpracování chyb v ASP.NET Core
author: ardalis
description: Můžete zjistit, jak se budou zpracovávat chyby v aplikacích ASP.NET Core.
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
uid: fundamentals/error-handling
ms.openlocfilehash: 2fe46ecc32d61a7fafb2ad6e2a35456476608251
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273706"
---
# <a name="handle-errors-in-aspnet-core"></a>Zpracování chyb v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/) a [tní Dykstra](https://github.com/tdykstra/)

Tento článek popisuje běžné appoaches pro zpracování chyb v aplikacích ASP.NET Core.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Stránka výjimka vývojáře

Konfigurace aplikace pro zobrazení stránky, které jsou uvedeny podrobné informace o výjimkách, nainstalujte `Microsoft.AspNetCore.Diagnostics` NuGet balíček a přidat čáru, která [konfigurace metody ve třídě spuštění](xref:fundamentals/startup):

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Uveďte `UseDeveloperExceptionPage` před veškerý middleware chcete zachytit výjimky, jako například `app.UseMvc`.

>[!WARNING]
> Povolit výjimky stránce vývojáře **jenom když aplikace běží ve vývojovém prostředí**. Nechcete sdílet informace podrobné výjimky veřejně při spuštění aplikace v produkčním prostředí. [Další informace o konfiguraci prostředí](xref:fundamentals/environments).

Pokud chcete zobrazit stránku výjimka vývojáře, spusťte ukázkovou aplikaci s prostředím nastavena na `Development`a přidejte `?throw=true` pro základní adresu URL aplikace. Stránka obsahuje několik karet s informacemi o výjimku a požadavek. Na první kartě zahrnuje trasování zásobníku. 

![Trasování zásobníku](error-handling/_static/developer-exception-page.png)

Další karta ukazuje dotaz parametrů řetězce, pokud existuje.

![Parametrů řetězce dotazu](error-handling/_static/developer-exception-page-query.png)

Tento požadavek nebyly k dispozici žádné soubory cookie, ale pokud neodpovídala, by se na **soubory cookie** kartě. Můžete zobrazit seznam hlaviček, které byly předány na kartě poslední.

![Záhlaví](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Konfigurace vlastní výjimky zpracování stránky

Nakonfigurovat stránku obslužná rutina výjimky pro použití při není aplikace spuštěna `Development` prostředí.

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

V aplikaci pro stránky Razor [dotnet nové](/dotnet/core/tools/dotnet-new) šablona stránky Razor poskytuje chybovou stránku a `ErrorModel` stránky třídu modelu v *stránky* složky.

V aplikaci MVC nemáte uspořádání metody akce obslužnou rutinu chyby s atributy metody HTTP, jako například `HttpGet`. Explicitní příkazy zabránit v dosažení metodu některých požadavků. Povolí anonymní přístup k metodě tak, aby byly schopný přijímat zobrazení chyb neověřené uživatele.

Například poskytuje následující metody chyba obslužné rutiny [dotnet nové](/dotnet/core/tools/dotnet-new) šablony MVC a zobrazí se v adaptéru domovské:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a>Konfigurace stavu znakové stránky

Ve výchozím nastavení, aplikace neposkytuje znaková stránka bohaté stav pro stavové kódy HTTP, jako například *404 nebyl nalezen*. Pokud chcete zadat stav znakové stránky, nakonfigurujte Middleware stránky kód stavu přidáním řádek do `Startup.Configure` metoda:

```csharp
app.UseStatusCodePages();
```

Ve výchozím nastavení přidá Middleware stránky kód stavu jednoduchý, textovém obslužné rutiny pro běžné stavových kódů, třeba 404:

![404 stránky](error-handling/_static/default-404-status-code.png)

Middleware podporuje několik metod rozšíření. Jedna metoda má výrazu lambda:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

Jiná metoda přebírá obsahu typ a formát řetězce:

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Existují také přesměrování a znovu spouštět metody rozšíření. Metoda přesměrování odešle klientovi 302 stavový kód:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

Znovu spustit metodu vrátí původní kód stavu klienta, ale také provede obslužná rutina pro adresu URL přesměrování:

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Stav znakové stránky lze vypnout pro konkrétní požadavky v metodu obslužná rutina stránky Razor nebo kontroler MVC. Zakázat stav znakové stránky, pokus o načtení [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) v požadavku [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) kolekce a zakažte funkci, pokud je k dispozici:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Pokud se používá `UseStatusCodePages*` přetížení, směřuje na koncový bod v aplikaci, vytvořte zobrazení MVC nebo stránky Razor pro koncový bod. Například [dotnet nové](/dotnet/core/tools/dotnet-new) šablonu pro stránky Razor aplikace vytváří na následující stránku a třídu modelu stránky:

*Error.cshtml*:

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications</strong>, as it can result in sensitive information from exceptions being displayed to end users. For local debugging, development environment can be enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to <strong>Development</strong>, and restarting the application.
</p>
```

*Error.cshtml.cs*:

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a>Zpracování výjimek

Kód v zpracování stránky výjimek můžete vyvolat výjimky. Často je vhodné pro produkční chybové stránky, které se skládají z čistě statický obsah.

Navíc mějte na paměti, že po odeslání hlaviček pro odpovědi, nelze změnit stavový kód odpovědi na ani můžete všechny výjimky stránky nebo obslužné rutiny spustit. Odpověď se musí udělat nebo přerušena připojení.

## <a name="server-exception-handling"></a>Zpracování výjimek serveru

Kromě zpracování logiky aplikace, výjimek [server](xref:fundamentals/servers/index) hostování vaší aplikace provádí některé výjimek. Pokud server výjimku zachytí před odesláním hlavičky, server odešle *500 – Vnitřní chyba serveru* odpověď se žádný text. Pokud server výjimku zachytí po odeslání hlaviček, server se ukončí připojení. Žádosti, které nejsou zpracovávány aplikace jsou zpracovávány serverem. Jakékoli výjimky jsou zpracována výjimka serveru zpracování. Žádné nakonfigurovat vlastní chybové stránky nebo middleware výjimek nebo filtry toto chování neovlivňuje.

## <a name="startup-exception-handling"></a>Spuštění zpracování výjimek

Pouze vrstvě hostingu může zpracovávat výjimky, které se uskuteční během spuštění aplikace. Pomocí [webového hostitele](xref:fundamentals/host/web-host), můžete [konfigurace hostitele chování v reakci na chyby během spuštění](xref:fundamentals/host/web-host#detailed-errors) s `captureStartupErrors` a `detailedErrors` klíče.

Hostování můžete jenom zobrazit chybovou stránku pro spuštění zaznamenané chyby, pokud dojde k chybě po adresu nebo port hostitele vazby. Pokud z nějakého důvodu selže žádné vazby, vrstvě hostování protokoly ke kritické výjimce dotnet procesu havárií, a žádná chybová stránka se zobrazí, když aplikace běží [Kestrel](xref:fundamentals/servers/kestrel) serveru.

Při spuštění [IIS](/iis) nebo [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 selhání procesu* je vrácený [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) Pokud proces nemůže být spustit. Postupujte podle pomoc při řešení potíží v [řešení potíží s ASP.NET Core ve službě IIS](xref:host-and-deploy/iis/troubleshoot) tématu.

## <a name="aspnet-mvc-error-handling"></a>Zpracování chyb rozhraní ASP.NET MVC

[MVC](xref:mvc/overview) aplikací mít některé další možnosti pro zpracování chyb, jako je například konfigurace filtry výjimek a provedení ověření modelu.

### <a name="exception-filters"></a>Filtry výjimek

Filtry výjimek lze nastavit globálně nebo na základě-controller nebo na akce v aplikaci MVC. Tyto filtry zpracovat všechny neošetřené výjimky během provádění akce kontroleru nebo jiný filtr a nejsou s názvem jinak. Další informace o filtry výjimek v [filtry](xref:mvc/controllers/filters).

> [!TIP]
> Filtry výjimek jsou vhodné pro soutisku výjimky, které se vyskytují v akce MVC, jsou ale není tak účinná jako chyba zpracování middleware. Dáváte přednost middleware pro obecné případ a použít filtry, pouze pokud potřebujete udělat zpracování chyb *jinak* podle MVC akci, která jste vybrali.

### <a name="handling-model-state-errors"></a>Stav modelu zpracování chyb

[Ověření modelu](xref:mvc/models/validation) probíhá před vyvoláním každou akci kontroleru a metoda akce odpovídá kontrola `ModelState.IsValid` a náležitě reagovat.

Některé aplikace se rozhodnete v takovém případě postupujte podle standardní konvence pro řešení chyb při ověřování modelu, [filtru](xref:mvc/controllers/filters) může být příslušné místo pro implementaci tato zásada. Měli byste otestovat chování vaše akce se stavy neplatný model. Další informace v [testování řadiče logiku](xref:mvc/controllers/testing).
