---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Globální zpracování chyb v rozhraní ASP.NET Web API 2 | Microsoft Docs
author: davidmatson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: c593c56ba3d0ee8ebf6dc425408d2c3b91c83f93
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>Globální zpracování chyb v rozhraní ASP.NET Web API 2
====================
podle [David Matson](https://github.com/davidmatson), [Rick Anderson](https://github.com/Rick-Anderson)

V rozhraní Web API protokolu nebo zpracovávat chyby globálně dnes neexistuje žádný snadný způsob. Některé neošetřené výjimky může zpracovat prostřednictvím [filtry výjimek](exception-handling.md), ale existuje několik případů, které nelze zpracovat filtry výjimek. Příklad:

1. Výjimky vydané z konstruktorů řadiče.
2. Výjimek vyvolaných z obslužné rutiny zpráv.
3. Výjimky vydané během trasování.
4. Výjimky vydané během serializace obsahu odpovědi.

Chceme jednoduchý a konzistentní způsob zpracování (kde je to možné) a přihlaste se tyto výjimky. 

Existují dvě hlavní případy pro zpracování výjimek, v případě, že jsme se moct posílat chybnou odpověď a protokolu výjimka je případ, kdy všechny můžeme dělat. Příklad pro druhém případě je, když je vyvolána výjimka uprostřed streamování obsahu odpovědi; v takovém případě je příliš pozdě odesílat novou zprávu odpovědi, protože stavový kód, hlavičky a Částečný obsah již šly přes přenosu, takže jsme jednoduše přerušit připojení. I když nelze zpracovat výjimku, k vytvoření nové zprávy odpovědi, stále podporu protokolování výjimky. V případech, kde jsme může rozpoznat chybu jsme vrátí odpověď příslušná chybová, jak je znázorněno v následující:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Stávající možnosti

Kromě [filtry výjimek](exception-handling.md), [obslužné rutiny zpráv](../advanced/http-message-handlers.md) umožňuje dnes sledovat všechny odpovědi na úrovni 500, ale funguje na tyto odpovědi je obtížné, protože nemají kontextu o původní chybě. Obslužné rutiny zpráv mít také některé stejná omezení jako filtry výjimek týkající se případy, které se může zpracovat. Během webového rozhraní API nemá infrastruktury trasování, které zaznamenává chybové stavy infrastruktury trasování je pro účely diagnostiky a není určená nebo vhodné pro spuštění v produkčním prostředí. Globální zpracování výjimek a protokolování by měla být služby, které můžete spustit během výroby a zapojené do stávajících řešení pro monitorování (například [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Přehled řešení

 Poskytujeme dva nové výměnná uživatele služby, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) a IExceptionHandler, protokolování a zpracování neošetřených výjimek. Služby jsou velmi podobné, s dva hlavní rozdíly:

1. Podporujeme registrace protokolovačů více výjimek, ale jenom jeden výjimka obslužná rutina.
2. Protokolovačů výjimek vždy zavolána, i když jsme se chystáte přerušit připojení. Obslužné rutiny výjimek získat volat jenom když jsme stále moci vybrat které zpráva odpověď k odeslání.

Obě služby poskytovat přístup k kontext výjimky obsahující příslušné informace z bodu, kde byla zjištěna výjimka, zejména v [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), vyvolána výjimka a zdroji výjimky (níže uvedené podrobnosti).

### <a name="design-principles"></a>Principy návrhu

1. **Žádné dodatečné změny** vzhledem k tomu, že tato funkce je přidáván v vedlejší verzi, ten je důležité omezení, které mají vliv na řešení, který existovat žádné nejnovější změny, buď na typ kontrakty nebo chování. Toto omezení vyloučit některé čištění, kterou jsme chtěli hotové z hlediska existující bloků catch vypnutí výjimky do odpovědi 500. Tato další čištění je něco, co jsme zvažte pro následné hlavní verze. Pokud to je důležité si prosím hlasovat o v [hlasové uživatelské rozhraní ASP.NET Web API](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Zachování konzistence s webového rozhraní API vytvoří** kanál rozhraní Web API filtru je skvělým způsobem, jak zpracovat mezi vyjímání obavy pružně použití logiku v určité akci, specifické pro řadič ani globální obor. Filtry, včetně filtry výjimek, akce a kontroler kontextů, mít vždy, i v případě, že je zaregistrován v globálním oboru. Kontrakt smysl pro filtry, ale znamená, že filtry výjimek, i globálně vymezená ty nejsou vhodné pro zpracování případech, například výjimky z obslužné rutiny zpráv, kde žádný kontext akce nebo kontroleru některé výjimek existuje. Pokud nám chcete použít, flexibilní zaměření poskytované filtry výjimek, potřebujeme ještě filtry výjimek. Ale pokud je zapotřebí zpracovat výjimka mimo kontext kontroleru, potřebujeme také samostatné konstrukce úplné globální při zpracování chyb (něco bez řadiče kontextu a akce kontextu omezení).

### <a name="when-to-use"></a>Kdy použít

- Protokolovací nástroje výjimky jsou řešení zobrazuje všechny nezpracované výjimce zachytila webového rozhraní API.
- Obslužné rutiny výjimek jsou řešení pro přizpůsobení všechny možné odpovědí neošetřených výjimek zachytila webového rozhraní API.
- Filtry výjimek jsou Nejsnazším řešením pro zpracování podmnožina neošetřené výjimky související s konkrétní akce nebo kontroleru.

### <a name="service-details"></a>Podrobnosti služby

 Rozhraní služby protokolovače a obslužné rutiny výjimky jsou jednoduché asynchronní metody trvá příslušných kontexty: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Poskytujeme také základní třídy pro obě tato rozhraní. Přepsání metody jádra (synchronizace nebo asynchronní) je všechno, co je potřeba protokolu nebo zpracování na doporučené časy. Pro protokolování `ExceptionLogger` základní třída zajistí, že základní protokolování se zavolá tato metoda pouze jednou pro každou výjimce (i v případě, že později rozšíří další až zásobník volání a znovu zasekne). `ExceptionHandler` Základní třídu bude volat základní metodu zpracování pouze pro výjimky v horní části zásobníku volání, ignoruje starší vnořené bloky catch. (Zjednodušená verzích tyto základní třídy jsou v příloze níže). Obě `IExceptionLogger` a `IExceptionHandler` získat informace o výjimce prostřednictvím `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Když architektura volá protokolovač výjimek nebo obslužné rutiny výjimek, bude vždy poskytovat `Exception` a `Request`. Výjimkou je testování částí, bude také vždy poskytovat `RequestContext`. Občas bude poskytovat `ControllerContext` a `ActionContext` (jen při volání z bloku catch pro filtry výjimek). Poskytne velmi zřídka `Response`(pouze v určitých případech IIS při uprostřed pokusu o zápis odpověď). Všimněte si, že vzhledem k tomu, že některé z těchto vlastností může být `null` je až příjemce zkontrolujte `null` před přístup ke členům třídy výjimek.`CatchBlock` je řetězec označující, které blok catch viděli výjimku. Řetězce bloku catch jsou následující:

- HttpServer (SendAsync metoda)
- HttpControllerDispatcher (SendAsync metoda)
- HttpBatchHandler (SendAsync metoda)
- IExceptionFilter (objektu ApiController na zpracování kanálu filtru výjimek v ExecuteAsync)
- OWIN hostitele:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (pro ukládání výstupu do vyrovnávací paměti)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (pro streaming výstup)
- Webového hostitele:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (pro ukládání výstupu do vyrovnávací paměti)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (pro streaming výstup)
    - HttpControllerHandler.WriteErrorResponseContentAsync (pro selhání v zotavení z chyb v režimu výstup do vyrovnávací paměti)

Seznam řetězců bloku catch je také k dispozici prostřednictvím vlastnosti statické jen pro čtení. (Řetězec bloku catch základní jsou na statické ExceptionCatchBlocks; zbytek zobrazí na jednu statickou třídu každý pro OWIN a webového hostitele).`IsTopLevelCatchBlock` je užitečné pro následující doporučené vzor zpracování výjimek pouze v horní části zásobníku volání. Místo vypnutí výjimky do 500 odpovědi, všude, kde dochází vnořený blok catch, obslužné rutiny výjimek nechat výjimky rozšířit až o do vidět hostitele.

Kromě `ExceptionContext`, protokoly získá jednu další část informací prostřednictvím kompletní `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

Druhou vlastností `CanBeHandled`, umožňuje protokolovacího nástroje k identifikaci výjimka, která nelze zpracovat. Při připojení je přerušena a nelze odesílat žádné nové zprávy odpovědi, nezavolá se protokolovacích nástrojů ale obslužná rutina se ***není*** volat, a protokolovacích nástrojů můžete identifikovat tento scénář z této vlastnosti.

V další `ExceptionContext`, obslužná rutina získá jeden další vlastnost můžete nastavit na celý `ExceptionHandlerContext` účelem ošetření výjimky:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Obslužné rutiny výjimek označuje, že se má výjimka ošetřena nastavením `Result` vlastnost na výsledek akce (například [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), nebo vlastní výsledek). Pokud `Result` vlastnost má hodnotu null, neošetřené výjimky a původní výjimka bude znovu.

Výjimky v horní části zásobníku volání vzali jsme krok navíc k zajištění, že odpověď je vhodný pro volající rozhraní API. Pokud se výjimka šíří až hostitel, volající by žlutý obrazovku smrti nebo některých jiných hostitele zadaný odpovědi, což je většinou HTML a obvykle odpovídající chybnou odpověď rozhraní API. V těchto případech spustí výsledek na jinou hodnotu než null a pouze v případě jeho obslužná rutina vlastní výjimky explicitně nastaví zpět na `null` (neošetřená) se výjimka šíří do hostitele. Nastavení `Result` k `null` v takových případech může být užitečná pro dva scénáře:

1. OWIN hostované webového rozhraní API s vlastní zpracování middleware zaregistrován webového rozhraní API před a vnější výjimek.
2. Místní ladění prostřednictvím prohlížeče, kde žlutý obrazovka smrti je ve skutečnosti užitečné odpovědi pro k neošetřené výjimce.

Pro protokolovačů výjimek a obslužné rutiny výjimek jsme nedělají nic nic k obnovení v případě protokolovacího nástroje nebo obslužná rutina, samotné vyvolá výjimku. (Než umožníte výjimka rozšířit, sdělit svůj názor v dolní části této stránky, pokud máte lepší přístup.) Kontrakt protokolovačů výjimek a obslužné rutiny je, že by neměl umožňují výjimky rozšířit až jejich volající; v opačném výjimka právě rozšíří, často zcela na hostitele, což vede k chybě ve formátu HTML (například soubor ASP. Žlutý obrazovky na NET) odesílány zpět na klientovi (a která obvykle není upřednostňovanou možnost pro volající rozhraní API, které očekávají XML nebo JSON).

## <a name="examples"></a>Příklady

### <a name="tracing-exception-logger"></a>Protokolovač výjimek trasování

Níže uvedené výjimky protokoly odeslat data výjimky nakonfigurované trasování zdrojů (včetně ve výstupním okně ladění v sadě Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Obslužná rutina výjimky vlastní chybové zprávy

Následující níže vytvoří vlastní chybové odpovědi pro klienty, včetně e-mailovou adresu pro kontaktování podpory.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Registrace filtry výjimek

Pokud použijete k vytvoření projektu šablony projektu "ASP.NET MVC 4 webové aplikace", uveďte kódu webového rozhraní API konfigurace uvnitř `WebApiConfig` třídy v *aplikace nebo s_pustit* složky:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Dodatek: Základní třída podrobnosti

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
