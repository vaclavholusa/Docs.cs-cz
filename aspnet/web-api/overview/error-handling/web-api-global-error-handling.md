---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Globální zpracování chyb v rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: davidmatson
description: ''
ms.author: aspnetcontent
ms.date: 02/03/2014
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: d1d1a081713da0771981229db8e8bd67a5103bf9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815860"
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>Globální zpracování chyb v rozhraní ASP.NET Web API 2
====================
podle [David Matson](https://github.com/davidmatson), [Rick Anderson](https://github.com/Rick-Anderson)

V rozhraní API webových protokolů nebo globálně zpracování chyb, které dnes není jednoduchý způsob. Některé neošetřené výjimky lze zpracovat prostřednictvím [filtry výjimek](exception-handling.md), ale existuje několik případů, které nemůže zpracovat filtry výjimek. Příklad:

1. Výjimky vyvolané z konstruktorů kontroleru.
2. Výjimky vyvolané z obslužné rutiny zpráv.
3. Výjimky vyvolané při směrování.
4. Výjimky vyvolané během serializace obsahu odpovědí.

Chceme poskytnout jednoduchý a konzistentní způsob, jak protokolování a zpracování (Pokud je to možné) tyto výjimky. 

Existují dva hlavní případy pro zpracování výjimek, v případě, že nemůžeme poslat reakce na chybu a je případ, kdy všechny můžeme udělat protokolu výjimku. Příklad pro druhém případě je při vyvolání výjimky uprostřed streamování obsahu odpovědi; v takovém případě je příliš pozdě odeslat nové zprávy odpovědi, protože stavový kód, záhlaví a Částečný obsah již šly voláním sítí, takže můžeme jednoduše přerušit připojení. Přestože výjimky nelze zpracovat k vytvoření nové zprávy odpovědi, stále podporovat protokolování výjimky. V případech, kde můžeme detekovat chyby jsme vrátit odpověď příslušnou chybovou, jak je znázorněno v následujících tématech:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Stávající možnosti

Kromě [filtry výjimek](exception-handling.md), [obslužné rutiny zpráv](../advanced/http-message-handlers.md) je možné ještě dnes sledovat všechny odpovědi na úrovni 500, ale reagovat na tyto odpovědi je obtížné, protože nemají kontextu o původní chyby. Obslužné rutiny zpráv mají také některé stejná omezení jako filtry výjimek, pokud jde o případy, které dokáže zpracovat. Webové rozhraní API se trasování infrastruktury, který zachytává chybové stavy sledování infrastruktury je pro účely diagnostiky a není určená nebo vhodný pro spouštění v produkčním prostředí. Globální zpracování výjimek a protokolování by měl být služeb, které lze spustit během produkčního prostředí a zapojí se do existujícího řešení monitorování (třeba [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Přehled řešení

 Nabízíme dvě nové služby replaceable uživatele [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) a IExceptionHandler, protokolování a zpracování neošetřených výjimek. Služby jsou velmi podobné, dva hlavní rozdíly:

1. Podporujeme registrace protokolovačů více výjimek, ale pouze jeden výjimka obslužné rutiny.
2. Protokolovací nástroje výjimka získat volána vždy, i v případě, že jsme se chystáte přerušit připojení. Obslužné rutiny výjimek pouze získat volá se, když jsme stále možnost rozhodnout, jaké zprávy s odpovědí k odeslání.

Obě služby poskytují přístup ke kontextu výjimky obsahující příslušné informace z bodu, kde byla zjištěna výjimka, zejména [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), vyvolána výjimka a zdroj výjimky (viz podrobnosti níže).

### <a name="design-principles"></a>Principy návrhu

1. **Žádné významné změny** vzhledem k tomu, že tato funkce se přidává vedlejší verze, jeden je důležité omezení dopadu na řešení, která existovat žádné významné změny, buď na typ smlouvy nebo chování. Toto omezení vyloučit některé vyčištění, které rádi bychom Nedokázali, co se týče stávající bloky catch zapnutí výjimky na 500 odpovědi. Tato další vyčištění je něco, co jsme zvážit další hlavní verze. Pokud to je důležité si prosím hlasovat o ji do [hlas uživatelů ASP.NET Web API](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Zachování konzistence s webovým rozhraním API vytvoří** filtr kanál rozhraní Web API je skvělý způsob, jak zpracovat vyskytující aspekty s flexibilitou použití logiky na konkrétní akci, kontroler konkrétní nebo globální obor. Filtry, včetně filtry výjimek, akce a kontroler kontextů, mají vždy, i v případě, že je zaregistrované v globálním oboru. Existuje, že kontrakt dává smysl pro filtry, ale znamená, že nejsou filtry výjimek, i s globálním oborem. ty, které jsou vhodné pro některé výjimky zpracování případů, jako jsou například výjimky z obslužné rutiny zpráv, pokud žádný kontext akce nebo kontroleru. Pokud chcete používat flexibilní oborů poskytované filtry pro zpracování výjimek, potřebujeme ještě filtry výjimek. Ale pokud bychom někdy potřebovali zpracovat výjimku mimo kontext kontroleru, potřebujeme také samostatnou konstrukci pro zpracování chyb celosvětovým (něco bez kontroleru kontextu a akce kontextu omezení).

### <a name="when-to-use"></a>Kdy použít

- Protokolovací nástroje výjimky jsou řešení tak, aby zobrazuje všechny neošetřené výjimky zachycuje se prostřednictvím webového rozhraní API.
- Obslužné rutiny výjimek jsou řešení pro přizpůsobení všechny možné odpovědi na neošetřených výjimkách zachycených v rozhraní Web API.
- Filtry výjimek jsou nejjednodušší řešení pro zpracování dílčí neošetřené výjimky související s konkrétní akce nebo kontroleru.

### <a name="service-details"></a>Podrobnosti služby

 Rozhraní služeb protokolovací nástroj a obslužné rutiny výjimek jsou jednoduché asynchronní metody provádění příslušných kontextech: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Poskytujeme také základní třídy pro oba z těchto rozhraní. Přepsání metody core (synchronizace nebo asynchronní) je vše, co je potřeba přihlášení nebo zpracování na doporučenou dobu. Pro protokolování, `ExceptionLogger` základní třídy se zajistí, že metoda základní protokolování je volat pouze jednou pro každou výjimku (i v případě, že ji později postoupí dále až zásobníku volání a zachycuje se znovu). `ExceptionHandler` Základní třídu bude volat základní metodu zpracování pouze pro výjimky v horní části zásobníku volání, ignoruje se starší verze vnořené bloky catch. (Zjednodušená verze těchto základních tříd jsou dále v příloze.) Obě `IExceptionLogger` a `IExceptionHandler` získat informace o výjimce prostřednictvím `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Když rozhraní volá protokolovač výjimek nebo obslužnou rutinu výjimky, bude vždy poskytovat `Exception` a `Request`. Výjimkou je testování částí, bude také vždy poskytovat `RequestContext`. Bude poskytovat jen zřídka `ControllerContext` a `ActionContext` (pouze při volání z bloku catch pro filtry výjimek). Bude poskytovat jen velmi zřídka `Response`(pouze v určitých případech služba IIS při uprostřed při zápisu odpovědi). Upozorňujeme, že některé z těchto vlastností mohou být `null` je až příjemce bude moct vyhledat `null` před přístup ke členům třídy výjimek.`CatchBlock` je řetězec určující, které bloku catch viděli výjimky. Řetězce bloku catch se následujícím způsobem:

- HttpServer (metoda SendAsync)
- HttpControllerDispatcher (metoda SendAsync)
- HttpBatchHandler (metoda SendAsync)
- Iexceptionfilter. (objektu ApiController zpracování kanálu filtru výjimky v ExecuteAsync)
- OWIN hostitele:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (pro ukládání do výstupní vyrovnávací paměti)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (pro streamování výstup)
- Webového hostitele:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (pro ukládání do výstupní vyrovnávací paměti)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (pro streamování výstup)
    - HttpControllerHandler.WriteErrorResponseContentAsync (pro selhání v zotavení z chyb v režimu výstup do vyrovnávací paměti)

Seznam řetězců blok catch, které je také k dispozici prostřednictvím vlastnosti statické jen pro čtení. (Řetězce bloku catch core jsou na statické ExceptionCatchBlocks; zbývající objeví na jednu statickou třídu každý pro OWIN a webového hostitele).`IsTopLevelCatchBlock` je užitečné pro postup dle doporučený model zpracování výjimek pouze v horní části zásobníku volání. Místo vypnutí výjimky na 500 odpovědi všude, kde dojde k vnořené zachytávací blok, můžete nechat obslužné rutiny výjimek výjimky nešířily, dokud mají vidět hostitele.

Kromě `ExceptionContext`, získá protokolovač jeden kousek informace prostřednictvím plně `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

Druhá vlastnost `CanBeHandled`, umožňuje protokolovací nástroj k identifikaci výjimku, která nelze zpracovat. Při připojení je dojít k přerušení a nelze odesílat žádné nové zprávy odpovědi, zavolá se Protokolovací nástroje, ale bude obslužnou rutinu ***není*** volat, a protokolovací nástroje můžete identifikovat tento scénář z této vlastnosti.

V další `ExceptionContext`, obslužná rutina načte jeden další vlastnosti můžete nastavit na plné `ExceptionHandlerContext` pro zpracování výjimky:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Obslužná rutina výjimky označuje, že ošetřila výjimku tak, že nastavíte `Result` vlastnost výsledek akce (například [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), nebo vlastní výsledek). Pokud `Result` vlastnost má hodnotu null, je výjimka neošetřená a bude původní výjimka znovu vyvolána.

Výjimky v horní části zásobníku volání nám trvalo přidat další krok k zajištění, že odpověď je vhodné pro volání rozhraní API. Pokud se výjimka šíří do hostitele, volající by zobrazí žlutý obrazovka úmrtí nebo některé hostitel k dispozici odpověď, která je obvykle ve formátu HTML a není obvykle vhodné chybová odpověď rozhraní API. V těchto případech výsledku spustí jinou hodnotu než null a pouze v případě, že obslužná rutina vlastní výjimky explicitně nastaví zpět do `null` (neošetřená) se výjimka šíří do hostitele. Nastavení `Result` k `null` v takových případech může být užitečné pro dva scénáře:

1. OWIN hostovaného webového rozhraní API s vlastní middleware zaregistrované před/mimo webového rozhraní API pro zpracování výjimek.
2. Místní ladění prostřednictvím prohlížeče, kde žlutý obrazovky smrti je ve skutečnosti užitečné odpovědi k neošetřené výjimce.

Pro protokolovačů výjimek a obslužné rutiny výjimek abychom nedělali nic k obnovení v případě stisknuté klávesy nebo samotná obslužná rutina vyvolá výjimku. (Jiné než umožníte tím výjimce šíření, Odeslat názor v dolní části této stránky, pokud máte lepším řešením.) Kontrakt pro protokolovačů výjimek a obslužné rutiny je, že by neměla umožňují výjimky nešířily až po jejich volajícím; v opačném případě výjimka právě rozšíří, často až po hostitele, což vede k chybě HTML (například ASP. Žlutý obrazovky na NET) odesílaných zpět do klienta (což obvykle není upřednostňovanou možnost pro volání rozhraní API, které očekávají JSON nebo XML).

## <a name="examples"></a>Příklady

### <a name="tracing-exception-logger"></a>Protokolovač výjimek trasování

Protokolovač výjimek níže odesílání dat výjimek do nakonfigurovaného zdroje trasování (včetně v okně výstupu ladění v sadě Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Obslužná rutina výjimky vlastní chybové zprávy

Následující níže vytvoří vlastní chybové odpovědi pro klienty, včetně e-mailovou adresu pro obrátíte na podporu.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Registrace filtry výjimek

Pokud používáte šablonu projektu "Aplikace pro Web ASP.NET MVC 4" k vytvoření projektu, ukládejte kód webového rozhraní API konfigurace uvnitř `WebApiConfig` třídy v *aplikace/_spustit* složky:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Dodatek: Podrobností základní třídy

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
