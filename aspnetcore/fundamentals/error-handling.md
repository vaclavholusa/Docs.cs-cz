---
title: "Zpracování chyb v ASP.NET Core"
author: ardalis
description: "Můžete zjistit, jak se budou zpracovávat chyby v aplikacích ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 5b0cda7b79b8a9523d1ba6a9b321d22d3ccc753a
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a>Úvod do zpracování chyb v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/) a [tní Dykstra](https://github.com/tdykstra/)

Tento článek popisuje běžné appoaches pro zpracování chyb v aplikacích ASP.NET Core.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Stránka výjimka vývojáře

Konfigurace aplikace pro zobrazení stránky, které jsou uvedeny podrobné informace o výjimkách, nainstalujte `Microsoft.AspNetCore.Diagnostics` NuGet balíček a přidat čáru, která [konfigurace metody ve třídě spuštění](startup.md):

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Uveďte `UseDeveloperExceptionPage` před veškerý middleware chcete zachytit výjimky, jako například `app.UseMvc`.

>[!WARNING]
> Povolit výjimky stránce vývojáře **jenom když aplikace běží ve vývojovém prostředí**. Nechcete sdílet informace podrobné výjimky veřejně při spuštění aplikace v produkčním prostředí. [Další informace o konfiguraci prostředí](environments.md).

Pokud chcete zobrazit stránku výjimka vývojáře, spusťte ukázkovou aplikaci s prostředím nastavena na `Development`a přidejte `?throw=true` pro základní adresu URL aplikace. Stránka obsahuje několik karet s informacemi o výjimku a požadavek. Na první kartě zahrnuje trasování zásobníku. 

![Trasování zásobníku](error-handling/_static/developer-exception-page.png)

Další karta ukazuje dotaz parametrů řetězce, pokud existuje.

![Parametrů řetězce dotazu](error-handling/_static/developer-exception-page-query.png)

Tento požadavek nebyly k dispozici žádné soubory cookie, ale pokud neodpovídala, by se na **soubory cookie** kartě. Můžete zobrazit seznam hlaviček, které byly předány na kartě poslední.

![Záhlaví](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Konfigurace vlastní výjimky zpracování stránky

Je vhodné nakonfigurovat stránku obslužná rutina výjimky pro použití při není aplikace spuštěna `Development` prostředí.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

V aplikaci MVC nemáte uspořádání explicitně metody akce obslužnou rutinu chyby s atributy metody HTTP, jako například `HttpGet`. Použití explicitní příkazů může zabránit dosažení metodu některých požadavků.

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>Konfigurace stavu znakové stránky

Ve výchozím nastavení nebudou aplikace zadejte znaková stránka bohaté stav stavové kódy HTTP, jako je například 500 (vnitřní chyba serveru) nebo 404 (není nalezena). Můžete nakonfigurovat `StatusCodePagesMiddleware` přidáním řádek do `Configure` metoda:

```csharp
app.UseStatusCodePages();
```

Ve výchozím nastavení přidá tento middleware jednoduchý, textovém obslužné rutiny pro běžné stavových kódů, třeba 404:

![404 stránky](error-handling/_static/default-404-status-code.png)

Middleware podporuje několik různých rozšiřující metody. Má výrazu lambda, jiné přijímá řetězec typ a formát obsahu.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Existují také metody rozšíření přesměrování. Jeden odešle klientovi 302 stavový kód a jeden vrátí původní stavový kód pro klienta, ale také provede obslužná rutina pro adresa URL pro přesměrování.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Pokud je nutné zakázat stav znakové stránky pro určité požadavky, můžete to udělat:

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>Zpracování výjimek

Kód v zpracování stránky výjimek můžete vyvolat výjimky. Často je vhodné pro produkční chybové stránky, které se skládají z čistě statický obsah.

Navíc mějte na paměti, že po odeslání hlaviček pro odpovědi, nelze změnit stavový kód odpovědi na ani můžete všechny výjimky stránky nebo obslužné rutiny spustit. Odpověď se musí udělat nebo přerušena připojení.

## <a name="server-exception-handling"></a>Zpracování výjimek serveru

Kromě zpracování logiky aplikace, výjimek [server](servers/index.md) hostování vaší aplikace provádí některé výjimek. Pokud server výjimku zachytí před odesláním hlavičky, server odešle 500 Vnitřní chyba serveru odpověď se žádný text. Pokud server výjimku zachytí po odeslání hlaviček, server se ukončí připojení. Žádosti, které nejsou zpracovávány aplikace jsou zpracovávány serverem. Jakékoli výjimky jsou zpracována výjimka serveru zpracování. Žádné nakonfigurovat vlastní chybové stránky nebo middleware výjimek nebo filtry toto chování neovlivňuje.

## <a name="startup-exception-handling"></a>Spuštění zpracování výjimek

Pouze vrstvě hostingu může zpracovávat výjimky, které se uskuteční během spuštění aplikace. Můžete [konfigurace hostitele chování v reakci na chyby během spuštění](hosting.md#detailed-errors) pomocí `captureStartupErrors` a `detailedErrors` klíč.

Hostování můžete jenom zobrazit chybovou stránku pro spuštění zaznamenané chyby, pokud dojde k chybě po adresu nebo port hostitele vazby. Pokud z nějakého důvodu selže žádné vazby, vrstvě hostování zaprotokoluje kritické výjimky, dotnet procesu havárií, a zobrazí se stránka žádné chyby.

## <a name="aspnet-mvc-error-handling"></a>Zpracování chyb rozhraní ASP.NET MVC

[MVC](xref:mvc/overview) aplikací mít některé další možnosti pro zpracování chyb, jako je například konfigurace filtry výjimek a provedení ověření modelu.

### <a name="exception-filters"></a>Filtry výjimek

Filtry výjimek lze nastavit globálně nebo na základě-controller nebo na akce v aplikaci MVC. Tyto filtry zpracovat všechny neošetřené výjimky během provádění akce kontroleru nebo jiný filtr a nejsou s názvem jinak. Další informace o filtry výjimek v [filtry](../mvc/controllers/filters.md).

>[!TIP]
> Filtry výjimek jsou vhodné pro soutisku výjimky, které se vyskytují v akce MVC, jsou ale není tak účinná jako chyba zpracování middleware. Dáváte přednost middleware pro obecné případ a použít filtry, pouze pokud potřebujete udělat zpracování chyb *jinak* podle MVC akci, která jste vybrali.

### <a name="handling-model-state-errors"></a>Stav modelu zpracování chyb

[Ověření modelu](../mvc/models/validation.md) probíhá před vyvoláním každou akci kontroleru a metoda akce odpovídá kontrola `ModelState.IsValid` a náležitě reagovat.

Některé aplikace se rozhodnete v takovém případě postupujte podle standardní konvence pro řešení chyb při ověřování modelu, [filtru](../mvc/controllers/filters.md) může být příslušné místo pro implementaci tato zásada. Měli byste otestovat chování vaše akce se stavy neplatný model. Další informace v [testování řadiče logiku](../mvc/controllers/testing.md).



