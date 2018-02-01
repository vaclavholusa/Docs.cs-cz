---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: "Co je nového v rozhraní ASP.NET Web API 2.1 | Microsoft Docs"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a>Co je nového v rozhraní ASP.NET Web API 2.1
====================
podle [Microsoft](https://github.com/microsoft)

Toto téma popisuje, co je nového pro ASP.NET Web API 2.1.

- [Stáhnout](#download)
- [Dokumentace](#documentation)
- [Nové funkce v rozhraní ASP.NET Web API 2.1](#new-features)

    - [Zpracování chyb globální](#global-error)
    - [Atribut směrování vylepšení](#attribute-routing)
    - [Vylepšení stránky nápovědy](#help-page)
    - [Podpora IgnoreRoute](#ignoreroute)
    - [Formátovací modul typu média BSON](#bson)
    - [Lepší podporu pro asynchronní filtry](#async-filters)
    - [Analýza pro klienta formátování knihovny dotazu](#query-parsing)
- [Známé problémy a nejnovější změny](#known-issues)
- [Opravy chyb](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Stáhnout

Funkce modulu runtime vydávají jako balíčků NuGet v galerii NuGet. Všechny balíčky modulu runtime použijte [sémantické verze](http://semver.org/) specifikace. Nejnovější balíček ASP.NET Web API 2.1 RTM má následující verze: "5.1.2". Můžete instalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Verze také zahrnuje odpovídající lokalizované balíčky na NuGet.

Můžete instalovat nebo aktualizovat balíčky NuGet vydaná pomocí konzoly Správce balíčků NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentace

Kurzy a další informace o ASP.NET Web API 2.1 RTM jsou dostupné na webu technologie ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Nové funkce v rozhraní ASP.NET Web API 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Zpracování chyb globální

Přes jednu centrální mechanismus lze nyní protokolovat všechny neošetřené výjimky a lze přizpůsobit chování neošetřených výjimek.

Rozhraní framework podporuje více protokolovačů výjimek, které najdete v části neošetřené výjimky a informace o kontextu, ve kterém je došlo, jako je například požadavek zpracovávaný v době.

Například následující kód používá System.Diagnostics.TraceSource k protokolování všech neošetřených výjimek:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

Můžete také nahradit výchozí obslužnou rutinu výjimky, takže můžete plně přizpůsobit zpráva odpovědi HTTP, která je odeslána, když k neošetřené výjimce dochází.

Uvádíme [ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) který protokoluje všechny nezpracované výjimky pomocí Oblíbené ELMAH framework.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Atribut směrování vylepšení

Směrováním atributů podporuje omezení, povolení správy verzí a výběru na základě záhlaví trasy. Mnoho aspektů trasy atributů jsou také nyní přizpůsobit pomocí **IDirectRouteFactory** rozhraní a **RouteFactoryAttribute** třídy. Předpona trasy je nyní extensible prostřednictvím **IRoutePrefix** rozhraní a **RoutePrefixAttribute** třídy.

Uvádíme [ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) používající omezení dynamicky řadiče filtrovat podle záhlaví HTTP 'api-version'.

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Vylepšení stránky nápovědy

Webové rozhraní API 2.1 obsahuje následující vylepšení k [stránky nápovědy rozhraní API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Dokumentace jednotlivé vlastnosti parametrů nebo návratové typy akcí.
- Dokumentace anotací dat modelu.

Návrh uživatelského rozhraní stránky nápovědy také aktualizován, aby dokázala pojmout tyto změny.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>Podpora IgnoreRoute

Webové rozhraní API 2.1 podporuje ignoruje vzorů adresy URL v směrování rozhraní Web API pomocí sady **IgnoreRoute** rozšiřující metody na **HttpRouteCollection**. Tyto metody způsobit webového rozhraní API ignorovat všechny adresy URL, které odpovídají zadané šablony a povolit na hostiteli a poté použít další zpracování, je-li to vhodné.

Následující příklad ignoruje identifikátory URI, který bude začínat &quot;obsah&quot; segmentu:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>Formátovací modul typu média BSON

Webové rozhraní API nyní podporuje [BSON](http://bsonspec.org/) přenosový formát, v klientovi i na serveru.

Chcete-li povolit BSON na straně serveru, přidejte **BsonMediaTypeFormatter** do kolekce formátovacích modulů:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

Tady je způsob, jakým může klient .NET využívají formátu BSON:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Uvádíme [ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) který ukazuje straně klient i server.

Další informace najdete v tématu [BSON podpory v 2.1 webové rozhraní API](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Lepší podporu pro asynchronní filtry

Webové rozhraní API teď podporuje snadný způsob, jak vytvářet filtry, které se spouští asynchronně. Tato funkce je užitečná je filtr potřebuje k provedení asynchronní akce, jako je přístup a databáze. Dříve Pokud chcete vytvořit filtr asynchronní, jste museli implementovat rozhraní filtru, sami, protože základní třídy filtru dostupná jenom v případě synchronních metod. Teď můžete přepsat virtuální `On*Async` metody filtru základní třídy.

Příklad:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

**AuthorizationFilterAttribute**, **ActionFilterAttribute**, a **ExceptionFilterAttribute** třídy všechny podporují asynchronní v 2.1 webové rozhraní API.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Analýza pro klienta formátování knihovny dotazu

Dříve **System.Net.Http.Formatting** podporuje analýzy a aktualizaci dotazy identifikátoru URI pro kódu na straně serveru, ale ekvivalentní přenosné knihovny chyběl tuto funkci. Ve webové rozhraní API 2.1 klientskou aplikaci teď můžete snadno analyzovat a aktualizovat řetězec dotazu.

Následující příklady ukazují, jak analyzovat, úpravě a vygenerovat URI dotazy. (V příkladech konzolovou aplikaci pro jednoduchost.)

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a nejnovější změny

Tato část popisuje známé problémy a nejnovějších změn v RTM k 2.1 aplikace ASP.NET Web API.

### <a name="attribute-routing"></a>Atribut směrování

Mnohoznačnosti v atributu směrování odpovídá teď chybu místo výběru na první shodu.

Atribut trasy mají zakázáno používat *{controller}* parametr a pomocí *{action}* parametr v směrování umístit na akce. Tyto parametry velmi pravděpodobně by způsobilo nejednoznačnosti.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Generování uživatelského rozhraní MVC nebo webového rozhraní API do projektu s 5.1 výsledky balíčky v 5.0 balíčky pro šablony, které již nejsou v projektu

Aktualizace balíčků NuGet pro ASP.NET Web API 2.1 RTM neaktualizuje nástrojů Visual Studio, jako je ASP.NET generování uživatelského rozhraní nebo šablona projektu webové aplikace ASP.NET. Používají předchozí verzi modulu runtime ASP.NET balíčky (5.0.0.0). Generování uživatelského rozhraní ASP.NET v důsledku toho nainstaluje předchozí verzi (5.0.0.0) požadované balíčky, pokud již nejsou k dispozici v projektech. ASP.NET generování uživatelského rozhraní v Visual Studio 2013 RTM nebo Update 1, ale nepřepíše nejnovější balíčků ve vašich projektů.

Pokud používáte generování uživatelského rozhraní ASP.NET po aktualizaci balíčků pro webové rozhraní API 2.1 nebo ASP.NET MVC 5.1, ujistěte se, že verze webového rozhraní API a MVC jsou konzistentní.

### <a name="type-renames"></a>Přejmenuje typu

Některé typy používané pro atribut směrování rozšiřitelnost byly přejmenován ze RC na 2.1 RTM.

| Původní název typu (2.1 RC) | Název nového typu (2.1 RTM) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Filtry výjimek rozbalení není agregační výjimky vzniklé v asynchronní akce

Dříve Pokud v asynchronní akce došlo **AggregateException**, filtr výjimek by rozbalení výjimka, a **OnException** by získat základní výjimky. V 2.1, filtr výjimek není rozbalení, a **OnException** získá původní **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Opravy chyb

Tato verze rovněž obsahuje několik oprav chyb. Úplný seznam najdete:

- [5.1.0 balíček](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 balíčku](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

5.1.2 balíček obsahuje aktualizace IntelliSense, ale žádné opravy chyb.
