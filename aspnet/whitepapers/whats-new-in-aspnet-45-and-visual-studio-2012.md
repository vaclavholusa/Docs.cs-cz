---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: "Co je nového v technologii ASP.NET 4.5 a Visual Studio 2012 | Microsoft Docs"
author: rick-anderson
description: "Tento dokument popisuje nové funkce a vylepšení, která jsou uvedena v technologii ASP.NET 4.5. Také popisuje vylepšení prováděné pro vývoj webů..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/29/2012
ms.topic: article
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 16a2fa4b1b6617430703853543cbf29e18ba1103
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Co je nového v technologii ASP.NET 4.5 a Visual Studio 2012
====================
> Tento dokument popisuje nové funkce a vylepšení, která jsou uvedena v technologii ASP.NET 4.5. Také popisuje vylepšení prováděné pro vývoj webů v sadě Visual Studio 2012. Tento dokument byl původně publikován na 29. února 2012.


- [Modul Runtime ASP.NET Core a Framework](#_Toc318097372)

    - [Asynchronně čtení a zápis požadavky a odpovědi HTTP](#_Toc318097373)
    - [Vylepšení zpracování požadavku HTTP](#_Toc318097374)
    - [Asynchronně abyste vyprázdnili odpověď](#_Toc318097375)
    - [Podpora pro *await* a *úloh*– na základě asynchronní moduly a obslužné rutiny](#_Toc318097376)
    - [Asynchronní modulů HTTP](#_Toc318097377)
    - [Asynchronní obslužné rutiny HTTP](#_Toc318097378)
    - [Nové funkce ověření požadavku ASP.NET](#_Toc318097379)
    - [Odložení ověření požadavku ("opožděné")](#_Toc318097380)
    - [Podpora pro neověřené požadavky](#_Toc318097381)
    - [AntiXSS Library](#_Toc318097382)
    - [Podpora pro protokol Websocket](#_Toc318097383)
    - [Vytváření sady a minifikace](#_Toc318097384)
    - [Vylepšení výkonu pro hostování webů](#_Toc_perf)

        - [Klíčové faktory](#_Toc_perf_1)
        - [Požadavky pro nové funkce výkonu](#_Toc_perf_2)
        - [Sdílení běžné sestavení](#_Toc_perf_3)
        - [Použití více jader JIT – kompilace pro rychlejší spuštění](#_Toc_perf_4)
        - [Ladění uvolňování paměti za účelem optimalizace paměti](#_Toc_perf_5)
        - [Prefetching pro webové aplikace](#_Toc_perf_6)
- [ASP.NET – webové formuláře](#_Toc318097385)

    - [Ovládací prvky dat silného typu](#_Toc318097386)
    - [Vazby modelu](#_Toc318097387)

        - [Výběr dat](#_Toc318097388)
        - [Zprostředkovatele hodnot](#_Toc318097389)
        - [Filtrování podle hodnoty z ovládacího prvku](#_Toc318097390)
    - [Výrazy datové vazby kódovaný jazykem HTML](#_Toc318097391)
    - [Ověření nerušivého](#_Toc318097392)
    - [HTML5 Updates](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [Rozhraní ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 Release Candidate](#_Toc318097396)

    - [Projekt sdílení mezi sady Visual Studio 2010 a Visual Studio 2012 Release Candidate (režim kompatibility projektu)](#project-compatibility)
    - [Změny konfigurace v šablonách technologie ASP.NET 4.5 webu](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Nativní podpora ve službě IIS 7 pro směrování ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [HTML Editor](#_Toc318097397)

        - [Inteligentní úlohy](#_Toc318097398)
        - [Podpora WAI ARIA](#_Toc318097399)
        - [Novou HTML5 fragmenty kódu](#_Toc318097400)
        - [Extrahovat do uživatelského ovládacího prvku](#_Toc318097401)
        - [IntelliSense pro útržky kódu v atributech](#_Toc318097402)
        - [Automatické přejmenování odpovídající značky při přejmenování počáteční a koncovou značku](#_Toc318097403)
        - [Generování obslužné rutiny událostí](#_Toc318097404)
        - [Inteligentní odsazení](#_Toc318097405)
        - [Snižte automatické dokončování](#_Toc318097406)
    - [JavaScript Editor](#_Toc318097407)

        - [Osnova kódu](#_Toc318097408)
        - [Související závorky](#_Toc318097409)
        - [Přechod na definici](#_Toc318097410)
        - [Podpora ECMAScript5](#_Toc318097411)
        - [DOM IntelliSense](#_Toc318097412)
        - [VSDOC podpis přetížení](#_Toc318097413)
        - [Implicitní odkazy](#_Toc318097414)
    - [CSS Editor](#_Toc318097415)

        - [Snižte automatické dokončování](#_Toc318097416)
        - [Hierarchická odsazení.](#_Toc318097417)
        - [Podpora napadnout šablon stylů CSS](#_Toc318097418)
        - [Schémata konkrétní dodavatele (- moz-, - webkit)](#_Toc318097419)
        - [Podpora přidávání poznámek a uncommenting](#_Toc318097420)
        - [Výběr barvy](#_Toc318097421)
        - [Fragmenty](#_Toc318097422)
        - [Vlastní oblasti](#_Toc318097423)
    - [Nástroj Page Inspector](#_Toc318097424)
    - [Publikování](#_Toc318097425)

        - [Publikační profily](#_Toc318097426)
        - [Předkompilace ASP.NET a sloučení](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Právní omezení](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>Modul Runtime ASP.NET Core a Framework

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Asynchronně čtení a zápis požadavky a odpovědi HTTP

ASP.NET 4 zavedla možnost číst entity požadavku HTTP jako datový proud pomocí *HttpRequest.GetBufferlessInputStream* metoda. Tato metoda poskytuje streamování přístup k entita požadavku. Však se provedla synchronně, který svázané až vlákno po dobu trvání žádost.

Technologie ASP.NET 4.5 podporuje možnost číst datové proudy asynchronně na entity požadavku HTTP a schopnost asynchronně vyprázdní. ASP.NET 4.5 také poskytuje možnost entity požadavku HTTP, který poskytuje jednodušší integraci se službou podřízené obslužné rutiny HTTP, například obslužné rutiny stránky .aspx a architektura ASP.NET MVC řadiče dvojitou vyrovnávací paměti.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Vylepšení zpracování požadavku HTTP

Odkaz na datový proud vrácený technologie ASP.NET 4.5 z *HttpRequest.GetBufferlessInputStream* podporuje synchronní a asynchronní metody pro čtení. *Datového proudu* objekt vrácený *GetBufferlessInputStream* teď implementuje BeginRead i EndRead metody. Asynchronní *datového proudu* metody umožňují asynchronně číst entita požadavku v bloky dat, zatímco ASP.NET verze aktuálního vlákna mezi každé iteraci smyčky asynchronního čtení.

ASP.NET 4.5 přidala také doprovodné metodu pro čtení entita požadavku způsobem ve vyrovnávací paměti: *HttpRequest.GetBufferedInputStream*. Toto nové přetížení funguje jako *GetBufferlessInputStream*, podpora synchronní a asynchronní čtení. Ale, jak ho načte, *GetBufferedInputStream* také zkopíruje bajtů entity do vyrovnávací paměti interní ASP.NET, aby podřízené moduly a obslužné rutiny stále přístup entita požadavku. Například pokud některé proti proudu kódu v kanálu má již číst entity žádosti o pomocí *GetBufferedInputStream*, můžete dál používat *HttpRequest.Form* nebo *HttpRequest.Files*. To vám umožní provádět asynchronní zpracování požadavku (například streamování nahrávání velkých souborů k databázi), ale stránky .aspx stále spuštění a MVC ASP.NET řadiče i později.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Asynchronně abyste vyprázdnili odpověď

Odeslání odpovědi klientovi HTTP může trvat značnou dobu, když klient je daleko nebo má připojení s malou šířkou pásma. Za normálních okolností ASP.NET ukládá do vyrovnávací paměti odpovědi bajtů vytváření aplikací. ASP.NET pak provede operaci odeslání jednoho nárůstu vyrovnávacích pamětí na konci velmi zpracování žádosti.

Pokud ve vyrovnávací paměti odpovědi velká (například streamování velký soubor pro klienta), musí pravidelně volat *HttpResponse.Flush* odesílat výstup do vyrovnávací paměti do klienta a zachovat využití paměti pod kontrolou. Ale vzhledem k tomu, *vyprázdnění* je synchronní volání, opakované volání *vyprázdnění* spotřebuje vlákno po dobu trvání potenciálně dlouhotrvající požadavky.

ASP.NET 4.5 přidává podporu pro provádění vyprázdnění asynchronně pomocí *BeginFlush* a *EndFlush* metody *HttpResponse* třídy. Pomocí těchto metod, můžete vytvořit asynchronní moduly a asynchronní obslužné rutiny, které postupně odesílání dat klientovi bez třeba zadávat vláken operačního systému. V rozmezí *BeginFlush* a *EndFlush* volání, technologii ASP.NET verze aktuálního vlákna. To významně snižuje celkový počet aktivních podprocesů, které jsou potřebné pro podporu dlouhodobé HTTP stahování.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Podpora pro *await* a *úloh* – na základě asynchronní moduly a obslužné rutiny

Asynchronní programování koncept, označuje jako zavedená rozhraní .NET Framework 4 *úloh*. Úlohy jsou reprezentované pomocí *úloh* typu a souvisejících typů v *System.Threading.Tasks* oboru názvů. Rozhraní .NET Framework 4.5 je založený na tato vylepšení kompilátoru, které práci s *úloh* jednoduché objekty. V rozhraní .NET Framework 4.5, kompilátory podporují dva nové klíčová slova: *await* a *asynchronní*. *Await* – klíčové slovo je syntaktické sdružená označující, kterou část kódu by měla asynchronně čekat na další část kódu. *Asynchronní* – klíčové slovo představuje pomocný parametr, který slouží k označení metody jako asynchronní metody založené na úlohách.

Kombinace *await*, *asynchronní*a *úloh* objektu je mnohem jednodušší pro psaní asynchronní kódu v rozhraní .NET 4.5. Technologie ASP.NET 4.5 podporuje tyto zjednodušení pomocí nových rozhraní API, která umožňují zápisu asynchronní modulů HTTP a pomocí nová vylepšení kompilátoru asynchronní obslužné rutiny HTTP.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Asynchronní modulů HTTP

Předpokládejme, že chcete pro asynchronní práci v rámci metodu, která vrátí *úloh* objektu. Následující příklad kódu definuje asynchronní metody, která provede asynchronní volání ke stažení domovské stránce Microsoftu. Všimněte si použití *asynchronní* – klíčové slovo v podpis metody a *await* volat na *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

To je všechno musíte napsat – rozhraní .NET Framework, bude automaticky zpracovávat unwinding zásobníku volání při čekání na dokončení stažení, jakož i po dokončení stahování automaticky obnovení zásobníku volání.

Nyní předpokládejme, že chcete použít tuto asynchronní metodu v modulu asynchronní ASP.NET HTTP. Technologie ASP.NET 4.5 obsahuje pomocnou metodu (*EventHandlerTaskAsyncHelper*) a nový typ delegáta (*TaskEventHandler*), můžete použít pro integraci starší založený na úlohách asynchronních metod asynchronní programovací model vystavené kanálu HTTP technologie ASP.NET. Tento příklad ukazuje, jak:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Asynchronní obslužné rutiny HTTP

Tradiční přístup k zápisu asynchronní obslužné rutiny technologie ASP.NET je implementace *IHttpAsyncHandler* rozhraní. Zavádí technologie ASP.NET 4.5 *HttpTaskAsyncHandler* asynchronní základní typ, který může být odvozen z, takže je mnohem jednodušší zápis asynchronní obslužné rutiny.

*HttpTaskAsyncHandler* typ je abstraktní a vyžaduje, abyste přepsat *ProcessRequestAsync* metoda. Interně ASP.NET postará integrace návratový podpis ( *úloh* objektu) z *ProcessRequestAsync* s starší asynchronní programovací model používá kanálu ASP.NET.

Následující příklad ukazuje, jak můžete použít *úloh* a *await* jako součást provádění asynchronní obslužné rutiny HTTP:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nové funkce ověření požadavku ASP.NET

Ve výchozím nastavení, ASP.NET provede ověření žádosti – zkontroluje požadavky a hledat v kódu nebo skriptu v polích, záhlaví, soubory cookie a tak dále. Pokud je zjištěn žádné, ASP.NET vyvolá výjimku. To funguje jako první linií obrany před potenciálními útoky skriptování webů.

ASP.NET 4.5 usnadňuje selektivně číst data neověřený požadavku. ASP.NET 4.5 taky integruje Oblíbené knihovny AntiXSS, která byla dříve vnější knihovny.

Vývojáři mají časté pro možnost selektivně vypnout ověření žádosti pro své aplikace. Pokud vaše aplikace je software, fórum, můžete chtít povolit uživatelům odesílat příspěvcích ve formátu HTML a komentáře, ale stále se ujistěte, ověření žádosti je kontrola nic jiného.

ASP.NET 4.5 zavádí dvě funkce, které usnadňují selektivně práci s neověřený vstup: odložení ověření požadavku ("opožděné") a přístup k datům neověřený požadavku.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Odložení ověření požadavku ("opožděné")

V technologii ASP.NET 4.5 ve výchozím nastavení všechna data žádosti podléhá ověření žádosti. Ale můžete nakonfigurovat aplikaci odložení ověření žádosti, dokud ve skutečnosti přístup k datům požadavku. (To se někdy označuje jako opožděné ověření na základě podmínek jako opožděného načítání pro určité scénáře dat.) Můžete nakonfigurovat aplikaci, aby používala odložené ověření v souboru Web.config nastavením *requestValidationMode* atribut 4.5 v *httpRUntime* elementu, jako v následujícím příkladu:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Při žádosti o ověření režim je nastaven na 4.5, aktivuje se pouze pro hodnotu konkrétního požadavku a jenom v případě, že váš kód přistupuje k této hodnotě ověření žádosti. Například, pokud kód získá hodnotu Request.Form["forum\_post"], je ověření žádosti vyvolat jenom pro daný element v kolekci formuláře. Žádný z elementů v *formuláře* kolekce se ověřují. V předchozích verzích technologie ASP.NET byla aktivována ověření žádosti pro shromažďování celý požadavků při přístupu k libovolný element v kolekci. Nové chování je jednodušší, podívejte se na různá data požadavku bez aktivace ověření požadavku na další požadované součásti jinou aplikaci.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Podpora pro neověřené požadavky

Odložené ověření samostatně nevyřeší problém selektivně vynecháním ověření požadavku. Volání Request.Form["forum\_post"] stále aktivační události žádosti o ověření pro tuto hodnotu konkrétního požadavku. Ale můžete chtít získat přístup k této poli bez aktivace ověření, protože budete chtít povolit kód v tomto poli.

K tomu technologie ASP.NET 4.5 teď podporuje neověřený přístup k datům požadavku. Technologie ASP.NET 4.5 obsahuje novou *Unvalidated* vlastnost kolekce v *požadavku HTTP* třídy. Tato kolekce poskytuje přístup k všechny běžné hodnoty dat požadavku, jako je třeba *formuláře*, *řetězce dotazu*, *soubory cookie*, a *Url*.

Pomocí příkladu fórum mohli číst data neověřený žádosti, musíte nejprve nakonfigurovat aplikaci, aby používala nový režim ověření žádosti:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Pak můžete použít *HttpRequest.Unvalidated* vlastnost načíst hodnotu neověřený formuláře:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> Zabezpečení – *použít data neověřený požadavku dát pozor!* ASP.NET 4.5 přidat vlastnosti neověřený požadavku a kolekce, aby bylo snazší k přístupu k velmi konkrétní žádost neověřená data. Vlastní ověření však musíte provést na požadavek nezpracovaných dat zajistit, že není nebezpečná text reprezentován uživatelům.


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>AntiXSS Library

Z důvodu v době Oblíbené Microsoft AntiXSS Library technologie ASP.NET 4.5 nyní zahrnuje rutiny kódování základní z verze 4.0 této knihovny.

Kódování rutiny jsou implementované *AntiXssEncoder* typu v novém *System.Web.Security.AntiXss* oboru názvů. Můžete použít *AntiXssEncoder* typu přímo voláním libovolnou metodu statické kódování, které jsou implementované v typu. Nejjednodušší způsob používání nové rutiny anti-XSS je však ke konfiguraci aplikace ASP.NET pro použití *AntiXssEncoder* třídy ve výchozím nastavení. Chcete-li to provést, přidejte následující atribut v souboru Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Když *encoderType* atributu je nastavena pro použití *AntiXssEncoder* typu, všechny výstup kódování v ASP.NET automaticky použije nové rutiny pro kódování.

Toto jsou části externí AntiXSS knihovny, které byly součástí technologie ASP.NET 4.5:

- *HtmlEncode*, *HtmlFormUrlEncode*, a *HtmlAttributeEncode*
- *XmlAttributeEncode* a *XmlEncode*
- *UrlEncode* a *UrlPathEncode* (Nový)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Podpora pro protokol Websocket

Protokol Websocket je standard based síťový protokol, který definuje, jak vytvořit zabezpečený, v reálném čase obousměrnou komunikaci mezi klientem a serverem pomocí protokolu HTTP. Společnost Microsoft ve spolupráci s IETF i W3C subjekty standardy, které chcete určit protokol. Protokol Websocket je podporována libovolného klienta (ne jenom prohlížeče), se začne investovat podporující protokol Websocket klienta i mobilních operačních systémů značné prostředky společností Microsoft.

Protokol Websocket je mnohem jednodušší k vytvoření dlouhodobé přenosech mezi klientem a serverem. Například zápis chatovací aplikace je mnohem snazší vzhledem k tomu může vytvořit hodnotu true dlouho běžící připojení mezi klientem a serverem. Nemáte uchýlit alternativní postupy, jako jsou pravidelně dotazuje nebo HTTP dlouho dotazování k simulaci chování soketu.

ASP.NET 4.5 a IIS 8 zahrnují podporu technologie WebSockets nízké úrovně, povolení ASP.NET vývojářům používat spravovaná rozhraní API pro asynchronně čtení a zápis řetězec a binární data na objekt Websocket. Pro technologii ASP.NET 4.5, je nový *System.Web.WebSockets* obor názvů, který obsahuje typy pro práci s protokolu Websocket.

Klientský prohlížeč naváže připojení Websocket tak, že vytvoříte DOM *WebSocket* objekt, který odkazuje na adresu URL v aplikaci ASP.NET, jako v následujícím příkladu:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Technologie WebSockets koncových bodů můžete vytvořit v technologii ASP.NET pomocí jakýkoli druh modul nebo obslužná rutina. V předchozím příkladu byl použit soubor .ashx, protože soubory .ashx je rychlý způsob, jak vytvořit obslužnou rutinu.

Na základě protokolu Websocket aplikace ASP.NET přijímá požadavek klienta objekty WebSockets indikující, že požadavek je potřeba upgradovat z požadavku HTTP GET na žádost o objekty WebSockets. Tady je příklad:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

*AcceptWebSocketRequest* metoda přijímá delegáta funkce, protože ASP.NET unwinds aktuální požadavek HTTP a pak předá řízení delegát funkce. Tento přístup je koncepčně podobné na použití *System.Threading.Thread*, kde můžete definovat v pozadí, které se provádí pracovní vlákno start delegáta.

Po ASP.NET a klient úspěšně dokončili metody handshake pro objekty WebSockets, ASP.NET vyvolá vaší delegáta a spuštění aplikace objekty WebSockets. Následující příklad kódu ukazuje jednoduchý odezvu aplikace, která používá integrovanou podporu pro objekty WebSockets technologie ASP.NET:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

Podpora v rozhraní .NET 4.5 pro *await* – klíčové slovo a asynchronní operace založené na úlohách je přirozené přizpůsobení pro psaní aplikací pro objekty WebSockets. Příklad kódu ukazuje, úplně asynchronně spouští žádost Websocket uvnitř ASP.NET. Aplikace asynchronně čeká na zprávu na odeslání od klienta voláním *await soketu. ReceiveAsync*. Podobně, odešle zprávu asynchronní do klienta voláním *await soketu. SendAsync*.

V prohlížeči aplikace přijímá zprávy Websocket prostřednictvím *onmessage* funkce. Chcete-li odeslat zprávu z prohlížeče, zavolejte *odeslat* metodu *protokolu WebSocket* DOM typu, jak je znázorněno v tomto příkladu:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

V budoucnu může vydáváme aktualizace tato funkce že abstraktní rychle některé nižší úrovně se tedy kódování požadované v této verzi pro objekty WebSockets aplikace.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Sdružování a minimalizace

Sdružování umožňuje zkombinovat jednotlivých souborů JavaScript a CSS do sady, které mohou být považovány za jeden soubor. Minimalizace zestruční souborů JavaScript a CSS odebráním prázdné znaky a dalšími znaky, které nejsou povinné. Tyto funkce pracují s webových formulářů ASP.NET MVC a webových stránek.

Sady jsou vytvořeny pomocí třídy sady nebo jedna z jejích podřízených tříd ScriptBundle a StyleBundle. Po dokončení konfigurace instance sady, je sada k dispozici na příchozí požadavky stačí je přidat do globální instance do kolekce BundleCollection. V šablonách výchozí konfigurace sady provádí v souboru BundleConfig. Toto výchozí nastavení vytvoří sady pro všechny základní skriptů a šablon stylů css soubory používané šablony.

Pomocí jedné z několik možných pomocné metody jsou odkazovány sady z v zobrazeních. Za účelem podpory různých značek pro sady v ladění oproti verzi režimu vykreslování, třídy ScriptBundle a StyleBundle mít pomocnou metodu, vykreslení. V režimu ladění, vykreslení vygeneruje kód pro všechny prostředky v sadě. V režimu vydání, vykreslení vygeneruje jednu značku element pro celou sadu. Přepnutím mezi debug a release režimu, můžete provést úpravy atribut ladění elementu kompilace v souboru web.config, jak je uvedeno níže:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Kromě toho povolení nebo zakázání optimalizace můžete nastavit přímo přes vlastnost BundleTable.EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Když jsou seskupeny soubory, se nejprve řadí abecedně (způsob, jak jsou zobrazeny v **Průzkumníku řešení**). Jsou pak uspořádány tak, aby známé knihovny a jejich vlastní rozšíření (například jQuery, MooTools a Dojo) se nejdřív načíst. Například konečné pořadí pro sdružování složky skriptů jako v příkladu nahoře, bude:

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a.js

Soubory šablon stylů CSS jsou také seřazené podle abecedy a pak znovu uspořádat tak, aby reset.css a normalize.css dřívější než jakýkoli jiný soubor. Provedení konečné řazení sdružování výše uvedenou složku styly, bude toto:

1. reset.css
2. content.css
3. forms.css
4. globals.css
5. menu.css
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Vylepšení výkonu pro hostování webů

Rozhraní .NET Framework 4.5 a systém Windows 8 zavádí funkce, které vám pomohou dosáhnout zvýšení významné výkonu pro zatížení webového serveru. To zahrnuje snížení (až 35 %) v obou čas spuštění a nároky na paměť pro webových hostitelských serverů, které používají technologii ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Klíčové faktory

V ideálním případě by měl být aktivní všechny weby a v paměti, aby zajistil, rychlou reakci na další požadavek vždy, když pochází. Faktory ovlivňující odezvy lokality patří:

- Čas potřebný pro lokalitu znovu po recykluje fond aplikací. Toto je doba potřebná ke spuštění procesu webového serveru pro lokalitu, když sestavení lokality již nejsou v paměti. (Platformy sestavení jsou v paměti, protože jsou používány jiné weby). Tato situace se označuje jako "cold server, spuštění záložním framework" nebo právě "studený spuštění webu."
- Kolik paměti zabírá webu. Podmínky pro tento jsou "využití paměti podle webu" nebo "odstranit pracovní sady."

Soustřeďte se na obě tyto faktory nových vylepšení výkonu.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Požadavky pro nové funkce výkonu

Požadavky na nové funkce můžete rozdělit do těchto kategorií:

- Vylepšení, které běží na rozhraní .NET Framework 4.
- Vylepšení, které vyžadují rozhraní .NET Framework 4.5, ale můžete spustit na libovolnou verzi systému Windows.
- Vylepšení, které jsou k dispozici pouze v rozhraní .NET Framework 4.5 a systémem Windows 8.

Výkon se zvyšuje s každou úroveň vylepšení, které budete moci povolit.

Některé z rozhraní .NET Framework 4.5 vylepšení využít výhod širší funkce výkonu, která se týkají také jiné scénáře.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Sdílení běžné sestavení

**Požadavek**: rozhraní .NET Framework 4 a Visual Studio 11 Developer Preview SDK

Různé lokality na serveru se často používají stejné pomocné rutiny sestavení (například sestavení z starter kit nebo ukázkové aplikace). Každá lokalita má svou vlastní kopii těchto sestavení v jeho adresáře Bin. I když je stejný jako kód objektu pro sestavení, jste fyzicky oddělené sestavení, takže každé sestavení má samostatně čtení během spouštění cold lokality a udržovány odděleně v paměti.

Nové funkce interning řeší tato neefektivnost a snižuje požadavky na paměť RAM a zatížení čas. Interning umožňuje Windows ponechte jednu kopii každého sestavení v systému souborů a jednotlivých sestavení do složky Koš lokality jsou nahrazeny symbolické odkazy na jedinou ponechanou kopii. Pokud jednotlivý web potřebuje odlišné verzi sestavení, symbolický odkaz je nahrazena novou verzi sestavení a má vliv pouze v dané lokalitě.

Sdílení sestavení s využitím symbolické odkazy vyžaduje nový nástroj s názvem aspnet\_intern.exe, který vám umožňuje vytvářet a spravovat úložiště interned sestavení. Je poskytována jako součást Visual Studio 11 Developer Preview SDK. (Ale bude fungovat v systému, který má pouze rozhraní .NET Framework 4 nainstalovaná, za předpokladu, že jste nainstalovali nejnovější [aktualizace](https://support.microsoft.com/kb/2468871).)

Aby byly všechny vhodné sestavení mít byla interned, spustíte aspnet\_intern.exe pravidelně (například jako naplánovaná úloha jednou za týden). Typické použití vypadá takto:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Pokud chcete zobrazit všechny možnosti, spusťte nástroj bez argumentů.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Použití více jader JIT – kompilace pro rychlejší spuštění

**Požadavek**: rozhraní .NET Framework 4.5

Pro spuštění studené webu nejen sestavení by měly být čtení z disku, ale webu musí být kompilována. Pro lokalitu komplexní to můžete přidat velkým prodlevám. Se nová pro obecné účely metoda v rozhraní .NET Framework 4.5 snižuje tyto zpoždění JIT – kompilace rozloží mezi jader procesoru k dispozici. Dělá to co nejvíc a co nejdříve pomocí informace shromážděné během předchozího spuštění webu. Tato funkce implementované [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) metoda.

JIT – kompilace pomocí více jader je zapnutá ve výchozím nastavení v technologii ASP.NET, takže nemusíte dělat nic využít této funkce. Pokud chcete tuto funkci zakázat, proveďte následující nastavení v souboru Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Ladění uvolňování paměti za účelem optimalizace paměti

**Požadavek**: rozhraní .NET Framework 4.5

Jakmile je systém lokality, může být jeho používání haldy uvolňování (GC) důležitým faktorem při jeho využití paměti. Uvolňování paměti rozhraní .NET Framework jako jakékoli uvolňování paměti, díky kompromisy mezi doba využití procesoru (četnost a násobek kolekcí) a využití paměti (Další místa, který se používá pro nové, uvolněné nebo mít volné objekty). Pro předchozí verze, uvádíme pokyny o tom, jak nakonfigurovat globální Katalog k dosažení rovnováhu mezi (například najdete v části [ASP.NET 2.0 nebo 3.5 sdílená hostování konfigurace](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Pro rozhraní .NET Framework 4.5, místo více samostatných nastavení, nastavení konfigurace definované zatížení je k dispozici všechny na dříve doporučená nastavení globálního katalogu a také nové ladění, který poskytuje další výkon pro jednotlivé servery umožňující pracovní sady.

Povolit optimalizace paměti GC, přidejte do souboru Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config následující nastavení:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Pokud jste obeznámeni s předchozí pokyny pro změny pro soubor aspnet.config, Všimněte si, že toto nastavení nahrazuje dřívější nastavení – například není nutné nastavit gcserver – gcconcurrent –, atd. Nemáte odeberte stará nastavení.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Prefetching pro webové aplikace

**Požadavek**: rozhraní .NET Framework 4.5 a systémem Windows 8

Několik verzí systému Windows má Započítaná technologie známé jako [prefetcher](http://en.wikipedia.org/wiki/Prefetcher) , snižuje náklady na čtení disku spuštění aplikace. Protože cold spuštění je převážně pro klientské aplikace, tato technologie nebyla zařazena do Windows serveru, který obsahuje pouze komponenty, které jsou nezbytné k serveru. Prefetching je nyní k dispozici v nejnovější verzi systému Windows Server, kde může optimalizovat spuštění jednotlivých webů.

Pro systém Windows Server prefetcher není povoleno ve výchozím nastavení. Chcete-li povolit a konfigurovat prefetcher pro hostování v podobě výkonných webů, spusťte následující sadu příkazů na příkazovém řádku:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Chcete-li prefetcher integrovat aplikace ASP.NET, přidejte následující v souboru Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>ASP.NET – webové formuláře

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Ovládací prvky Data silného typu

V technologii ASP.NET 4.5 webové formuláře zahrnuje některá vylepšení pro práci s daty. První zlepšení je ovládacích prvků data silného typu. Pro ovládací prvky webových formulářů v předchozích verzích technologie ASP.NET, můžete zobrazit hodnotu vázané na data pomocí *Eval* a výraz datové vazby:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Pro dvoucestné datovou vazbu, můžete použít *vazby*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

V době běhu pomocí těchto volání reflexe načíst hodnotu zadaného člena a následně se zobrazí výsledek do kódu. Tento přístup umožňuje snadno k vazbě dat na libovolný, unshaped data.

Datová vazba výrazy takto však nepodporují funkcí, jako je technologie IntelliSense pro názvy členů, navigace (např. přejít k definici) nebo kompilaci kontrola pro tyto názvy.

Chcete-li vyřešit tento problém, přidává technologie ASP.NET 4.5 možnost deklarovat datový typ dat, která je vázána ovládacího prvku. To provedete pomocí nové *ItemType* vlastnost. Když nastavíte tuto vlastnost, dva nové typy proměnné jsou k dispozici v oboru datové vazby výrazů: *položky* a *BindItem*. Protože proměnné jsou silného typu, získáte všech výhod vývojového prostředí sady Visual Studio.


Výrazy obousměrné vazby dat, použijte *BindItem* proměnné:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


Pro podporu byly aktualizovány většina ovládacích prvků v rámci webových formulářů ASP.NET s podporou datová vazba *ItemType* vlastnost.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Vazby modelu

Vazby modelu rozšiřuje datové vazby v ovládací prvky webových formulářů ASP.NET pro práci s přístup k datům zaměřené na kódu. Její součástí jsou koncepty z *ObjectDataSource* řízení a z vazby modelu v architektuře ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Výběr dat

Pokud chcete konfigurovat ovládací prvek dat pomocí vazby modelu vyberte data, nastavte ovládacího prvku *metody SelectMethod* vlastnost na název metody v kódu stránky. Ovládací prvek dat volá metodu v příslušnou dobu v životním cyklu stránky a automaticky vytvoří vazbu vrácená data. Není nutné explicitně volat *DataBind* metoda.

V následujícím příkladu *GridView* ovládací prvek je nakonfigurovaný na použití metodu s názvem *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Vytvoříte *GetCategories* metodu v kódu stránky. Pro jednoduché vyberte operaci, metoda potřebuje žádné parametry a by měla vrátit *rozhraní IEnumerable* nebo *IQueryable* objektu. Pokud nové *ItemType* je nastavena (tím umožníte silného typu výrazy vázání dat, jak je popsáno v části [silně typované ovládací prvky datových](#_Toc318097386) dříve), obecný verze těchto rozhraní má být vrácen – *rozhraní IEnumerable&lt;T&gt;*  nebo *IQueryable&lt;T&gt;*, s *T* Parametr odpovídající typ *ItemType* vlastnosti (například *IQueryable&lt;kategorie&gt;*).

Následující příklad ukazuje kód pro *GetCategories* metoda. Tento příklad používá model Entity Framework Code First s ukázková databáze Northwind. Kód zajišťuje, že dotaz vrátí podrobnosti o souvisejících produktů pro každou kategorii prostřednictvím *zahrnout* metoda. (To zajistí, že *TemplateField* elementu v kódu zobrazí počet produktů v každé kategorii bez nutnosti [n + 1 vyberte](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Při spuštění stránky, *GridView* řízení volání *GetCategories* metoda automaticky a vykreslí vrácená data pomocí nakonfigurované pole:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Protože vyberte metoda vrátí *IQueryable* objekt, *GridView* řízení můžete dále upravit dotaz před jeho provedením. Například *GridView* řízení můžete přidat výrazy dotazu pro řazení a stránkování s vráceným *IQueryable* objektu před jeho spuštěním, tak, aby tyto operace jsou prováděny základní Zprostředkovatel LINQ. V takovém případě Entity Framework zajistí, že tyto operace jsou prováděny v databázi.

Následující příklad ukazuje *GridView* ovládací prvek upravit tak, aby Povolit řazení a stránkování:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Teď při spuštění stránky, ovládací prvek měli jistotu, že se zobrazí pouze aktuální stránku dat a že je seřazené podle vybraného sloupce:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Pokud chcete filtrovat vrácená data, mít parametry pro přidání do vyberte metodu. Tyto parametry vyplní vazby modelu za běhu, a můžete je změnit dotaz před vrácením data.

Předpokládejme například, že chcete umožnit uživatelům filtru produkty zadáním klíčového slova v řetězci dotazu. Můžete přidat parametr metody a aktualizovat kód, který použije hodnotu parametru:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Tento kód obsahuje *kde* výrazu, pokud je zadána hodnota pro *– klíčové slovo* a vrátí výsledky dotazu.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Zprostředkovatele hodnot

Předchozí příklad nebyla konkrétní o tom, kde hodnota *– klíčové slovo* parametr byl pocházejících z. Udávajících tyto informace můžete použít parametr atribut. V tomto příkladu můžete použít *QueryStringAttribute* třída, která je v *System.Web.ModelBinding* obor názvů:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Tím se nastaví vazby modelů a pokuste se vytvořit vazbu hodnotu z řetězce dotazu do *– klíčové slovo* parametr za běhu. (To může zahrnovat provádění převod typů, i když v tomto případě nepodporuje.) Pokud nelze zadat hodnotu a typ je použití hodnot Null, je vyvolána výjimka.

Zdroje hodnot pro tyto metody jsou označovány jako zprostředkovatele hodnot a atributy parametru, které indikují hodnota poskytovatele, kterého chcete použít se označují jako atributy zprostředkovatele hodnot. Webové formuláře budou zahrnovat zprostředkovatele hodnot a odpovídající atributy pro všechny typické zdroje vstupu uživatele v aplikaci webových formulářů, jako je například řetězec dotazu, soubory cookie, hodnot formuláře, ovládací prvky, stav zobrazení, stav relace a vlastnosti profilu. Je také možné zapsat zprostředkovatele vlastních hodnot.

Ve výchozím nastavení název parametru slouží jako klíč najít hodnotu v kolekci zprostředkovatele hodnoty. V příkladu bude vypadat kód pro hodnotu řetězce dotazu s názvem – klíčové slovo (například ~ / default.aspx?keyword=chef). Předání jako argument do atribut parametru můžete zadat vlastní klíč. Například pokud chcete použít hodnotu s názvem q proměnné řetězce dotazu, může to uděláte:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Pokud tato metoda je v kódu stránky, uživatelé mohou filtrovat výsledky předáním klíčové slovo pomocí řetězce dotazu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Vazby modelu provede celou řadu úloh, které byste jinak museli ručně code: čtení hodnoty, kontrola hodnotu null, pokusu o převod na příslušný typ, kontrola, zda byl převod úspěšný a nakonec pomocí hodnoty v dotaz. Model vazby výsledky v mnohem méně kódu a možnosti znovu použít funkci v celé vaší aplikaci.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtrování podle hodnoty z ovládacího prvku

Předpokládejme, že chcete příklad rozšířit na nechat na uživateli hodnota filtru z rozevíracího seznamu. Přidejte následující rozevíracího seznamu pro značku a nakonfigurujte ji pro získání dat z pomocí jiné metody *metody SelectMethod* vlastnost:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

Obvykle můžete také přidat *EmptyDataTemplate* elementu, který chcete *GridView* řídit tak, aby ovládacího prvku se zobrazí zpráva, pokud nebyly nalezeny žádné odpovídající produkty:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

V kódu stránky přidáte že nové vyberte metodu pro rozevíracího seznamu:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Nakonec aktualizujte *GetProducts* metodu provést nový parametr, který obsahuje ID vybrané kategorie z rozevíracího seznamu vyberte:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Teď při spuštění stránky uživatelé můžete v rozevíracím seznamu vyberte kategorii a *GridView* je automaticky znovu navázané na zobrazení filtrovaných dat ovládacího prvku. To je možné, protože vazba modelu sleduje hodnoty parametrů pro vyberte metody a zjistí, zda je libovolná hodnota parametru změnila po zpětné volání. Pokud ano, vazby modelu vynutí opětovné vytvoření vazby na data ovládacího prvku přidružená data.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Výrazy datové vazby kódovaný jazykem HTML

Můžete teď kódování HTML výsledek výrazy datové vazby. Přidejte na konec dvojtečkou (:) &lt;% # předponu, která označuje výraz datové vazby:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Ověření nerušivého

Teď můžete konfigurovat ovládacích prvků pro integrované ověřování používat nerušivý JavaScript pro ověřování na straně klienta. To významně snižuje velikost JavaScript vykreslovány do kódu stránky a snižuje celkové velikosti stránky. Nerušivý JavaScript pro ovládací prvky můžete nakonfigurovat v některém z těchto způsobů:

- Globálně přidáním následující nastavení na  *&lt;appSettings&gt;*  element v souboru Web.config: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Globálně nastavením statických *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* vlastnost *UnobtrusiveValidationMode.WebForms* (obvykle ve *aplikace \_Spustit* metoda v souboru Global.asax).
- Jednotlivě pro stránku nastavením nové *UnobtrusiveValidationMode* vlastnost *stránky* třídy k *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>HTML5 Updates

Některé vylepšení byla provedena pro webové formuláře ovládací prvky serveru, abyste mohli využívat nové funkce HTML5:

- *v textovém režimu* vlastnost *TextBox* řízení aktualizoval na podporu nových typů HTML5 vstupní jako *e-mailu*, *data a času*, a atd.
- *Odesílání souborů při odpovědích* řízení nyní podporuje nahrávání více souborů z prohlížečů, které podporují tuto funkci HTML5.
- Validátor řídí teď podporu ověřování HTML5 elementy vstupu.
- Nové prvky HTML5, které mají atributy, které představují adresu URL nyní podporují runat = "server". V důsledku toho můžete použít ASP.NET konvence v URL cesty, jako je třeba ~ operátor představují kořenový adresář aplikace (například &lt;video runat = "server" src="~/myVideo.wmv" /&gt;).
- *UpdatePanel* napravení ovládací prvek pro podporu příspěvků HTML5 vstupních polí.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta je nyní součástí Visual Studio 11 Beta. ASP.NET MVC je rozhraní pro vývoj webových aplikací vysoce možností intenzivního testování a údržby pomocí využití vzoru Model-View-Controller (MVC). ASP.NET MVC 4 usnadňuje vytváření aplikací pro mobilní Web a zahrnuje rozhraní ASP.NET Web API, které vám usnadní vytváření služeb HTTP, které mohou být využity jakéhokoli zařízení. Další informace najdete v tématu [poznámky k verzi ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET – webové stránky 2

Nové funkce patří:

- Šablony webů nové a aktualizované.
- Přidávání na straně serveru a ověřování na straně klienta pomocí *ověření* pomocné rutiny.
- Možnost registrovat skripty pomocí Správce prostředků.
- Povolení přihlášení ze sítě Facebook a jiných lokalitách pomocí OAuth a OpenID.
- Přidání mapuje pomocí *mapuje* pomocné rutiny.
- Spuštění webové stránky aplikace vedle sebe.
- Vykreslování stránek pro mobilní zařízení.

Další informace o těchto funkcích a příklady celou stránku kódu najdete v tématu [horní části funkce ve verzi Beta 2 webové stránky](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta.

Tato část obsahuje informace o vylepšení pro vývoj webů v aplikaci Visual Web Developer 11 Beta a Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Projekt sdílení mezi sady Visual Studio 2010 a Visual Studio 2012 Release Candidate (režim kompatibility projektu)

Dokud Visual Studio 2012 Release Candidate otevření existujícího projektu v novější verzi sady Visual Studio spustit Průvodce převodu. Toto nové formáty, které nebyly zpětně kompatibilní vynutit upgrade obsahu (prostředků) projektu a řešení. Po převodu proto nelze otevřít projekt ve starší verzi sady Visual Studio.

Mnoho zákazníků jste nám sdělili, že to nebyl správný přístup. V produktu Visual Studio 11 Beta jsme teď podporují sdílení projekty a řešení s Visual Studio 2010 SP1. To znamená, že pokud otevření projektu 2010 ve Visual Studio 2012 Release Candidate, bude stále nebudete moct otevřete projekt v sadě Visual Studio 2010 SP1.

> [!NOTE]
> Několik typy projektů se nedají sdílet mezi Visual Studio 2010 SP1 a Visual Studio 2012 Release Candidate. To zahrnuje některé starší projekty (jako jsou projekty ASP.NET MVC 2) nebo projekty pro zvláštní účely (například nastavení projektů).

Když poprvé ve verzi Visual Studio 11 Beta otevřete projekt Visual Studio 2010 SP1, následující vlastnosti jsou přidány do souboru projektu:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags, UpgradeBackupLocation a OldToolsVersion jsou používány proces, který provede upgrade souboru projektu. Že nemají žádný vliv na práci s projektem v sadě Visual Studio 2010.

VisualStudioVersion je novou vlastnost používané MSBuild 4.5, který určuje verzi Visual Studia pro aktuální projekt. Protože tato vlastnost nebyla neexistují v nástroji MSBuild 4.0 (verze nástroje MSBuild, který používá Visual Studio 2010 SP1), vložíme výchozí hodnotu do souboru projektu.

Vlastnost VSToolsPath slouží k určení správné .targets soubor pro import z cesty reprezentována MSBuildExtensionsPath32 nastavení.

Existují také některé změny související se Import elementů. Tyto změny jsou požadované za účelem podpory kompatibilitu mezi obě verze sady Visual Studio.

> [!NOTE]
> Pokud projekt je sdílená mezi Visual Studio 2010 SP1 a Visual Studio 11 Beta na dvou různých počítačích, a pokud projekt obsahuje místní databázi v aplikaci\_složky dat, je nutné nejprve, že je verze použitá při databáze systému SQL Server v obou počítačích nainstalován.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Změny konfigurace v šablonách technologie ASP.NET 4.5 webu

Byly provedeny následující změny na výchozí hodnoty *Web.config* souborů pro lokalitu, která se vytvářejí pomocí šablon webu ve Visual Studio 2012 Release Candidate:

- V `<httpRuntime>` elementu, `encoderType` atribut je nyní ve výchozím nastavení používat AntiXSS typy, které byly přidány do technologie ASP.NET. Podrobnosti najdete v tématu [produktu AntiXSS knihovny](#_Toc318097382).
- Také v `<httpRuntime>` elementu, `requestValidationMode` je nastavena na hodnotu "4,5". To znamená, že ve výchozím nastavení, ověření žádosti konfigurován pro použití ověřování odložené ("opožděné"). Podrobnosti najdete v tématu [nových funkcí technologie ASP.NET žádosti o ověření](#_Toc318097379).
- `<modules>` Element `<system.webServer>` neobsahuje oddíl `runAllManagedModulesForAllRequests` atribut. (Výchozí hodnota je false). To znamená, že pokud používáte verzi služby IIS 7, který nebyl aktualizován na verzi SP1, můžete mít problémy se směrováním v nové lokalitě. Další informace najdete v tématu [nativní podpora ve službě IIS 7 pro směrování ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine).

Tyto změny neovlivní stávající aplikace. Může ale představují rozdíl v chování mezi weby stávající a nové weby, které vytvoříte pro technologii ASP.NET 4.5 pomocí nové šablony.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Nativní podpora ve službě IIS 7 pro směrování ASP.NET

Toto není ke změně ASP.NET jako takový, ale změnu v šablonách pro nové projekty webových stránek, které můžou ovlivnit při práci na verzi služby IIS 7, které nebyly použity aktualizace SP1.

V technologii ASP.NET můžete přidat následující konfigurační nastavení pro aplikace za účelem podpory směrování:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Když **runAllManagedModulesForAllRequests** je PRAVDA, adresu URL podobnou `http://mysite/myapp/home` přejde na technologii ASP.NET, přestože je na žádné *.aspx*, *.mvc*, nebo podobné rozšíření na ADRESA URL.

Aktualizace, která byla vytvořena pro službu IIS 7 umožňuje **runAllManagedModulesForAllRequests** nastavení nepotřebné a podporuje nativně směrování ASP.NET. (Informace o této aktualizaci najdete v tématu Článek Microsoft Support [aktualizace je dostupná, že umožňuje určité služby IIS 7.0 nebo IIS 7.5 obslužné rutiny pro zpracování požadavků, jejichž adresy URL na konci období](https://support.microsoft.com/kb/980368).)

Pokud váš web běží ve službě IIS 7 a pokud služby IIS byl aktualizován, není nutné nastavovat **runAllManagedModulesForAllRequests** na hodnotu true. Ve skutečnosti nastavení na hodnotu true nedoporučuje, protože přidá nepotřebné zpracování nároky na požadavek. Pokud toto nastavení platí, všechny požadavky, včetně těch, které pro *.htm*, *.jpg*, a další statické soubory, taky projít kanál požadavku.

Pokud vytvoříte nový web ASP.NET 4.5 pomocí šablon, které jsou k dispozici v sadě Visual Studio 2012 RC, konfigurace webu nezahrnuje **runAllManagedModulesForAllRequests** nastavení. To znamená, že ve výchozím nastavení je false.

Pokud pak spustí web v systému Windows 7 bez aktualizace SP1 nainstalován, nebude IIS 7 zahrnout požadované aktualizace. V důsledku toho směrování nebude fungovat a zobrazí se chyby. Pokud máte potíže, kde směrování nefunguje, můžete provést buď následující:

- Aktualizujte Windows 7 SP1, která přidá aktualizace do služby IIS 7.
- Nainstalujte aktualizaci popsanou v článek Microsoft Support uvedených výše.
- Nastavit **runAllManagedModulesForAllRequests** na hodnotu true v souboru Web.config tohoto webu. Všimněte si, že se přidá některé režijní náklady na požadavky.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Editor HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Inteligentní úlohy

V zobrazení návrhu komplexní vlastnosti ovládacích prvků serveru, často přidruženy dialogových oken a průvodce, které usnadňují jejich nastavení. Například můžete použít speciální dialogové okno Přidat zdroje dat *opakovače* řízení nebo přidat sloupce do *GridView* ovládacího prvku.

Tento typ nápovědy uživatelského rozhraní pro komplexní vlastnosti však nebyla k dispozici v zobrazení zdroje. Visual Studio 11 proto zavádí inteligentní úlohy pro zobrazení zdroje. Inteligentní úlohy jsou kontextově zkratky pro často používané funkce editorů jazyka C# a Visual Basic.

Pro ovládací prvky webových formulářů ASP.NET, inteligentní úkoly se zobrazí na server značky jako malé glyf po kurzor na místo uvnitř elementu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

Inteligentní úloh rozšíří klepnete na tlačítko glyf nebo stiskněte klávesy CTRL +. (tečka), jako v případě editory kódu. Pak zobrazí zkratky, které jsou podobné inteligentní úlohy v zobrazení návrhu.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Například inteligentní úloh na předchozím obrázku jsou uvedené možnosti GridView úlohy. Pokud si zvolíte možnost Upravit sloupce, zobrazí se následující dialogové okno:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Dialogovém okně se nastavují vyplnění stejné vlastnosti můžete nastavit v zobrazení návrhu. Po kliknutí na tlačítko OK, aktualizovat je kód pro ovládací prvek s novým nastavením:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Podpora WAI ARIA

Zápis webových míst je stále roste. [WAI ARIA usnadnění standardní](http://www.w3.org/WAI/intro/aria) definuje, jak vývojáři měli zapsat přístupné weby. Tento standard je nyní plně podporovány v sadě Visual Studio.

Například *role* atribut teď má plnou technologie IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

Standardní WAI ARIA také zavádí atributy, které mají předponu *aria -* které umožňují přidat sémantiku do dokument HTML5. Visual Studio také plně podporuje tyto *aria -* atributy:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png)![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Novou HTML5 fragmenty kódu

Chcete-li rychlejší a snazší běžně používané kód HTML5, Visual Studio obsahuje několik fragmenty kódu. Příkladem je video fragment kódu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

K vyvolání fragmentu, stiskněte klávesu Tab, dvakrát, když je vybraný element v IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

To vytváří fragment kódu, kterou si můžete přizpůsobit.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Extrahovat do uživatelského ovládacího prvku

Ve velkých webových stránkách může být vhodné přesunout jednotlivé do uživatelských ovládacích prvků. Tato forma refaktoring může pomoci zvýšit čitelnost stránky a zjednodušit strukturu stránky.

Pro zjednodušení, když upravujete stránky webových formulářů v zobrazení zdroje, můžete teď vybrat text na stránce, pravým tlačítkem myši a zvolte extrakce do uživatelského ovládacího prvku:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense pro útržky kódu v atributech

Visual Studio vždy poskytl IntelliSense pro útržky kódu na straně serveru v libovolném stránka nebo ovládací prvek. Visual Studio nyní zahrnuje IntelliSense pro útržky kódu v a atributů HTML.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

To usnadňuje vytvoření datové vazby výrazů:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Automatické přejmenování odpovídající značky při přejmenování počáteční a koncovou značku

Pokud přejmenujete HTML element (například změnit *div* značka, které se *záhlaví* značka), odpovídající otevřením nebo ukončovací značka také změní v reálném čase.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

To pomáhá zabránit chyba, kde zapomenete změnit uzavírací značka nebo změnit nesprávný.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Generování obslužné rutiny událostí

Visual Studio teď obsahuje funkce, v zobrazení zdroje, které vám pomohou zápis obslužné rutiny událostí a svázat je ručně. Pokud upravujete název události v zobrazení zdroje, IntelliSense zobrazí &lt;vytvořit novou událost&gt;, který vytvoří obslužnou rutinu události v kódu stránky, má správný podpis:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Ve výchozím nastavení použije obslužné rutiny události ID ovládacího prvku pro název metody zpracování událostí:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Výsledný obslužné rutiny události bude vypadat takto (v tomto případě v jazyce C#):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Inteligentní odsazení

Po stisknutí klávesy Enter při v prázdném prvku HTML, editor bude umístěte kurzor na správném místě:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Pokud stisknutí klávesy Enter v tomto umístění je uzavírací značku přesunout dolů a odsazené tak, aby odpovídaly počáteční značka. Kurzor je také odsazeny:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Snižte automatické dokončování

Seznam technologie IntelliSense v sadě Visual Studio nyní filtry založené na co zadáte, tak, aby se zobrazí pouze příslušné možnosti:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense také filtry na základě názvu případu jednotlivých slov v seznamu IntelliSense. Například pokud zadáte "dl", dl a asp: DataList se zobrazí:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Tato funkce umožňuje rychlejší získat doplňování výrazů pro známé prvky.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript – editor

Editor JavaScript ve Visual Studio 2012 Release Candidate je zcela nové a výrazně vylepšuje možností práce s JavaScript v sadě Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Osnova kódu

Osnovy oblasti se nyní vytvářejí automaticky pro všechny funkce, což umožňuje sbalit části souboru, které nejsou důležité pro váš aktuální výběr.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Související závorky

Po umístění kurzoru na otevírání nebo pravé složené závorce, označuje editoru odpovídající jeden.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Přechod na definici

Přejděte na příkaz definice umožňuje přejít na zdroj pro funkce nebo proměnná.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Podpora ECMAScript5

Editor podporuje rozhraní API a nové syntaxe v ECMAScript5, nejnovější verze standard, který popisuje jazyka JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>DOM IntelliSense

IntelliSense pro rozhraní API DOM vylepšila se podpora pro mnoho nových rozhraní API jazyka HTML5 včetně *querySelector*, DOM úložiště, zasílání zpráv a mezi dokumenty a *plátno*. DOM IntelliSense nyní vycházejí jeden jednoduchý soubor JavaScript, nikoli definici typu nativní knihovny. To umožňuje snadno rozšířit nebo nahradit.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>VSDOC podpis přetížení

Podrobné IntelliSense komentáře lze deklarovat teď pro samostatné přetížení funkce jazyka JavaScript pomocí nové  *&lt;podpis&gt;*  elementu, jak je uvedeno v následujícím příkladu:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Implicitní odkazy

Teď můžete přidat soubory JavaScript do centrální seznamu, který bude implicitně součástí seznam souborů, aby všechny dané JavaScript souboru nebo bloku odkazy, což znamená, že budete získat IntelliSense pro její obsah. Například můžete přidat soubory jQuery centrální seznam souborů a získáte IntelliSense pro funkce jQuery jakéhokoliv JavaScript bloku souboru, zda jste ji výslovně odkazována (pomocí / / / &lt;odkaz nebo&gt;) nebo ne.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS Editor

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Snižte automatické dokončování

Seznam IntelliSense pro filtry nyní šablon stylů CSS na základě vlastností šablon stylů CSS a hodnoty vybrané schéma.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense také podporuje případu hledání názvu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Hierarchická odsazení

CSS editor pomocí odsazení zobrazí hierarchické pravidel, který bude stručně charakterizovat logicky uspořádání kaskádových pravidla. V následujícím příkladu #list selektor je kaskádových podřízená seznamu a proto odsazeno.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

Následující příklad ukazuje složitější dědičnosti:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Odsazení pravidla je dáno jeho nadřazený pravidla. Hierarchická odsazení je ve výchozím nastavení povolené, ale ji můžete vypnout dialogové okno Možnosti (nástroje, možnosti z řádku nabídek):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>Podpora napadnout šablon stylů CSS

Analýza stovky souborů CSS reálného ukazuje, že jsou velmi běžné špionážní programy šablon stylů CSS a teď podporuje nejčastěji používané těm, které jsou v sadě Visual Studio. Tato podpora zahrnuje funkce IntelliSense a ověřování hvězdičkou (\*) a podtržítko (\_) špionážní programy vlastnost:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Špionážní programy typické selektor jsou podporovány také tak, aby hierarchické odsazení se zachová, i když tato nastavení se použijí. Typické selektor hackerský, používá k cílové aplikace Internet Explorer 7, je předřazení selektor s  *\*: first-child + html*. Pomocí tohoto pravidla budou udržovat hierarchické odsazení:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Schémata konkrétní dodavatele (- moz-- webkit)

CSS3 představuje mnoho vlastností, které byly implementovány v různých prohlížečích v různých časech. To se dříve vynutit vývojářům kódu pro konkrétní prohlížeče pomocí syntaxe specifické pro dodavatele. Tyto vlastnosti specifické pro prohlížeč jsou teď součástí IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Podpora přidávání poznámek a uncommenting

Teď můžete komentář a zrušte komentář u pravidla šablon stylů CSS pomocí stejné zkratek, které používáte v editoru kódu (Ctrl + K, C komentáře a Ctrl + K, abyste zrušte komentář u).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Výběr barvy

V předchozích verzích sady Visual Studio IntelliSense pro atributy Barva související se skládal z rozevíracího seznamu hodnot pojmenovaná barva. Výběr barvy plně funkční a nahradila tohoto seznamu.

Při zadání hodnoty barvy volby barev zobrazí se automaticky a zobrazí seznam použitých barvy, za nímž následuje výchozí palety barev. Můžete vybrat barvu pomocí myši nebo klávesnice.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

V seznamu lze rozšířit do dokončení barva výběru. Nástroje pro výběr umožňuje řídit automaticky převodu žádné barva RGBA při přesunutí jezdec krytí alfa kanálu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Fragmenty kódu

Fragmenty kódu v editoru stylů CSS umožňují snadnější a rychlejší vytvořit více prohlížečů stylů. Mnoho vlastností CSS3, které vyžadují nastavení specifických pro prohlížeč teď byly vráceny do fragmenty kódu.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Fragmenty šablon stylů CSS podporu pokročilých scénářích (jako jsou dotazy média CSS3) tak, že zadáte na – symbol (@), který zobrazuje seznam IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Když vyberete @media hodnotu a stiskněte klávesu Tab, editor šablon stylů CSS vloží následující fragment kódu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Stejně jako u fragmenty kódu, můžete vytvořit vlastní fragmenty šablon stylů CSS.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Vlastní oblasti

S názvem kód oblasti, které jsou již k dispozici v editoru kódu, jsou nyní k dispozici pro úpravy šablon stylů CSS. Díky tomu můžete snadno skupiny související styl bloky.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Pokud oblast sbalena zobrazí název oblasti:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Inspektor stránek

Nástroj Page Inspector je nástroj, který vykreslí webová stránka (HTML, webových formulářů, ASP.NET MVC nebo webové stránky) v prostředí Visual Studio IDE a umožňuje vám Zkontrolujte zdrojový kód a výsledný výstup. Pro stránky ASP.NET nástroj Page Inspector umožňuje určit, které kódu na straně serveru je tvořen kód HTML, které je vykresleno do prohlížeče.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Další informace o nástroj Page Inspector najdete v tématu následující kurzy:

- Díky nástroji Page Inspector ve [rozhraní ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Díky nástroji Page Inspector ve [webových formulářů ASP.NET](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Publikování

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Publikační profily

Informace o publikování pro projekty webových aplikací v sadě Visual Studio 2010, nejsou uloženy ve správě verzí a není určený pro sdílení s jinými uživateli. Visual Studio 2012 Release Candidate se změnil formát profilu publikování. Byly provedeny týmových artefaktů a nyní je snadno využít ze sestavení podle MSBuild. Informace o konfiguraci sestavení je v dialogovém okně Publikovat, takže můžete snadno přepínat konfigurace sestavení před publikováním.

Publikování profily jsou uloženy ve složce PublishProfiles. Umístění složky závisí na jaké programovací jazyk, kterou používáte:

- C#: Properties\PublishProfiles
- Visual Basic: Moje Project\PublishProfiles

Každý profil je soubor MSBuild. Během publikování, je tento soubor importovat do souboru projektu nástroje MSBuild. V sadě Visual Studio 2010, pokud chcete provést změny do procesu publikování nebo balíček, budete muset uložit příslušná přizpůsobení do souboru s názvem **ProjectName**. wpp.targets. To je podporováno, ale teď můžete umístit příslušná přizpůsobení v samotném profilu publikování. Tímto způsobem vlastní nastavení se použije pouze pro tento profil.

Můžete teď také využívají publikační profily z nástroje MSBuild. Uděláte to tak, použijte následující příkaz, při sestavování projektu:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Hodnota project.csproj je cesta k projektu a ProfileName je název profilu publikování. Alternativně neprochází název profilu *PublishProfile* vlastnost, abyste mohli předávat úplnou cestu k profilu publikování.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>Předkompilace ASP.NET a sloučení

Visual Studio 2012 Release Candidate pro projekty webových aplikací, přidá možnost na stránce vlastností balení/publikování webu, který vám umožní předkompilovat a sloučení obsahu vašeho webu při publikování nebo balíček projektu. Pokud chcete zobrazit tyto možnosti, klikněte pravým tlačítkem na projekt v Průzkumníku řešení, vyberte možnost Vlastnosti a zvolte na stránce vlastností balení/publikování webu. Následující obrázek znázorňuje Precompile tuto aplikaci před publikováním možnost.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Pokud je vybraná tato možnost, Visual Studio překompiluje vždy, když publikujete aplikaci nebo balíčku webové aplikace. Pokud chcete řídit způsob předkompilovaných webu nebo způsob sloučení sestavení, klikněte na tlačítko Upřesnit můžete nakonfigurovat tyto možnosti.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>Služby IIS Express

Výchozí webový server pro testování webové projekty v sadě Visual Studio je nyní IIS Express. Vývojový Server sady Visual Studio je stále možnost pro místní webový server během vývoje, ale služba IIS Express je teď doporučené serverem. Možností v produktu Visual Studio 11 Beta pomocí služby IIS Express je velmi podobné k použití v sadě Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Právní omezení

Toto je předběžná dokument a mohou být změněny podstatně uvedením produktu softwaru popsaných v tomto dokumentu.

Informace obsažené v tomto dokumentu představují aktuální pohled společnosti Microsoft Corporation na uvedené problémy k datu publikování. Protože společnost Microsoft musí reagovat na měnící se podmínky na trhu, by neměl být interpretovat jako závazek společnosti Microsoft a společnost Microsoft nemůže zaručit přesnost informací jsou prezentovány po provedení datu publikování.

Tento dokument White Paper je pouze informativní charakter. MICROSOFT NEPOSKYTUJE ŽÁDNÉ ZÁRUKY, VÝSLOVNÉ, PŘEDPOKLÁDANÉ NEBO STATUTÁRNÍ INFORMACE V TOMTO DOKUMENTU.

Dodržovat všechny platné zákony o autorských právech zodpovídá uživatele. Bez omezení autorských práv, tento dokument může být opakuje, uložené v systému načtení nebo přenést jakýmkoli způsobem (ať už elektronicky, mechanických, včetně kopírování nebo) nebo pro jakýkoli účel bez předchozího písemného svolení společnosti Microsoft Corporation.

Společnost Microsoft může být patentů, patentová aplikací, ochranné známky, autorská práva nebo jiná duševní vlastnictví předmět uvedený v tomto dokumentu. Výslovně uvedeno v licenční smlouvě společnost Microsoft neposkytuje tento dokument žádnou licenci na tyto patenty, ochranné známky, autorská práva nebo jiné duševní vlastnictví.

Pokud není uvedeno jinak, příklady společností, organizací, produktů, názvů domén, e-mailové adresy, loga, osob, míst a událostí použité v ukázkách jsou smyšlené a spojení se žádné skutečnou společností, organizace, produktu, název domény, e-mailu adresu, logem, osoba, místní nebo událostí je určený nebo událostmi.

© 2012 Microsoft Corporation. Všechna práva vyhrazena.

Microsoft a Windows jsou registrované ochranné známky nebo ochranné známky společnosti Microsoft Corporation ve Spojených státech a dalších zemích.

Názvy společností a produktů může být ochranné známky příslušných vlastníků.
