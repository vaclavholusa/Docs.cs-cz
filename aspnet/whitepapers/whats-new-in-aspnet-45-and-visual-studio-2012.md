---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: Co je nového v technologii ASP.NET 4.5 a Visual Studio 2012 | Dokumentace Microsoftu
author: rick-anderson
description: Tento dokument popisuje nové funkce a vylepšení, která jsou uvedena v technologii ASP.NET 4.5. Také popisuje vylepšení pro vývoj pro web...
ms.author: aspnetcontent
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: cfe9b1a7f05b43d5eb638c8fa7cb581d1edac9d5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835810"
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Co je nového v technologii ASP.NET 4.5 a Visual Studio 2012
====================
> Tento dokument popisuje nové funkce a vylepšení, která jsou uvedena v technologii ASP.NET 4.5. Také popisuje vylepšení pro vývoj pro web v sadě Visual Studio 2012. Tento dokument byl původně publikován na 29. února 2012.


- [Modul Runtime ASP.NET Core a Framework](#_Toc318097372)

    - [Asynchronní čtení a zápis požadavků a odpovědí HTTP](#_Toc318097373)
    - [Zlepšení zpracování HttpRequest](#_Toc318097374)
    - [Asynchronně vyprazdňování odpověď](#_Toc318097375)
    - [Podpora pro *await* a *úloh*– na základě asynchronní moduly a obslužné rutiny](#_Toc318097376)
    - [Asynchronní moduly protokolu HTTP](#_Toc318097377)
    - [Asynchronní obslužné rutiny HTTP](#_Toc318097378)
    - [Nové funkce pro ověření žádosti ASP.NET](#_Toc318097379)
    - [Odložené ověření žádosti ("opožděné")](#_Toc318097380)
    - [Podpora pro neověřené žádosti](#_Toc318097381)
    - [Knihovny AntiXSS](#_Toc318097382)
    - [Podpora protokolu WebSockets](#_Toc318097383)
    - [Vytváření sady a minifikace](#_Toc318097384)
    - [Vylepšení výkonu pro hostování webů](#_Toc_perf)

        - [Výkon klíčové faktory](#_Toc_perf_1)
        - [Požadavky pro nové funkce výkonu](#_Toc_perf_2)
        - [Sdílení běžné sestavení](#_Toc_perf_3)
        - [Používání vícejádrové kompilace JIT pro rychlejší spuštění](#_Toc_perf_4)
        - [Optimalizace uvolňování paměti optimalizace paměti](#_Toc_perf_5)
        - [Předběžné načítání pro webové aplikace](#_Toc_perf_6)
- [Webové formuláře ASP.NET](#_Toc318097385)

    - [Ovládací prvky dat silného typu](#_Toc318097386)
    - [Vazby modelu](#_Toc318097387)

        - [Výběr dat](#_Toc318097388)
        - [Zprostředkovatele hodnot](#_Toc318097389)
        - [Filtrování podle hodnot pomocí ovládacího prvku](#_Toc318097390)
    - [Výrazy vázání dat kódovaný jazykem HTML](#_Toc318097391)
    - [Nerušivý ověření](#_Toc318097392)
    - [Aktualizace HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [Rozhraní ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 Release Candidate](#_Toc318097396)

    - [Projekt pro sdílení obsahu mezi Visual Studio 2010 a Visual Studio 2012 Release Candidate (Kompatibilita projektu)](#project-compatibility)
    - [Změny konfigurace v šablonách technologie ASP.NET 4.5 webu](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Nativní podpora ve službě IIS 7 pro směrování ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [HTML Editor](#_Toc318097397)

        - [Inteligentní úlohy](#_Toc318097398)
        - [Podpora v POČKA ARIA](#_Toc318097399)
        - [Nová specifikace HTML5 fragmenty kódu](#_Toc318097400)
        - [Extrahovat do uživatelského ovládacího prvku](#_Toc318097401)
        - [Technologie IntelliSense pro kód útržky v atributech](#_Toc318097402)
        - [Automatické přejmenování odpovídající značky při přejmenování otevírací nebo uzavírací značku](#_Toc318097403)
        - [Generování obslužné rutiny události](#_Toc318097404)
        - [Inteligentní odsazení](#_Toc318097405)
        - [Snižte automatické dokončování příkazů](#_Toc318097406)
    - [Editor jazyka JavaScript](#_Toc318097407)

        - [Sbalování kódu](#_Toc318097408)
        - [Párování závorek](#_Toc318097409)
        - [Přejít k definici](#_Toc318097410)
        - [Podpora ECMAScript5](#_Toc318097411)
        - [Technologie IntelliSense modelu DOM](#_Toc318097412)
        - [Signatura přetížení VSDOC](#_Toc318097413)
        - [Implicitní odkazy](#_Toc318097414)
    - [CSS Editor](#_Toc318097415)

        - [Snižte automatické dokončování příkazů](#_Toc318097416)
        - [Hierarchické odsazení.](#_Toc318097417)
        - [Změní podporu šablon stylů CSS](#_Toc318097418)
        - [Schémata pro konkrétní dodavatele (- moz-, - webkit)](#_Toc318097419)
        - [Podpora přidávání poznámek a odstraňuje se komentování](#_Toc318097420)
        - [Výběr barvy](#_Toc318097421)
        - [Fragmenty](#_Toc318097422)
        - [Vlastní oblastí](#_Toc318097423)
    - [Nástroj Page Inspector](#_Toc318097424)
    - [Publikování](#_Toc318097425)

        - [Profily publikování](#_Toc318097426)
        - [Předkompilace a merge](#_Toc318097427)
- [Služba IIS Express](#_Toc318097428)
- [Právní omezení](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>Modul Runtime ASP.NET Core a Framework

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Asynchronní čtení a zápis požadavků a odpovědí HTTP

ASP.NET 4 zavedena možnost číst entity požadavku HTTP jako datový proud pomocí *HttpRequest.GetBufferlessInputStream* metody. Tato metoda poskytuje datové proudy přístup k entitě žádosti. Ale provede synchronně, který spojený se vlákno po dobu trvání požadavku.

ASP.NET 4.5 podporuje oprávnění ke čtení datové proudy asynchronně na entity požadavku HTTP a schopnost asynchronně vyprázdní. ASP.NET 4.5 také poskytuje schopnost požadavek entity HTTP, která nabízí jednodušší integraci s podřízené obslužné rutiny HTTP, jako je například obslužných rutin stránky ASPX a architektura ASP.NET MVC řadiče dvojité vyrovnávací paměti.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Zlepšení zpracování HttpRequest

Odkaz na Stream vrácený z technologie ASP.NET 4.5 *HttpRequest.GetBufferlessInputStream* podporuje synchronní i asynchronní metody pro čtení. *Stream* objekt vrácený z *GetBufferlessInputStream* nyní implementuje BeginRead i funkci EndRead lze volat metody. Asynchronní *Stream* metody umožňují asynchronně číst entitě žádosti v blocích, zatímco technologie ASP.NET verze aktuálního vlákna mezi každé iteraci smyčky asynchronního čtení.

ASP.NET 4.5 již také přidal doprovodná metoda pro čtení ve vyrovnávací paměti jako entita požadavku: *HttpRequest.GetBufferedInputStream*. Toto nové přetížení funguje jako *GetBufferlessInputStream*, podporuje synchronní a asynchronní operace čtení. Nicméně, jak vypadal, *GetBufferedInputStream* také zkopíruje bajtů entity do vnitřní vyrovnávací paměti ASP.NET tak, aby podřízený moduly a obslužné rutiny můžete pořád přístup k entitě žádosti. Například když některé upstream kód v kanálu již načten žádost o entitu s využitím *GetBufferedInputStream*, můžete dál používat *HttpRequest.Form* nebo *HttpRequest.Files*. To vám umožní provádět asynchronní zpracování na vyžádání (třeba streamování nahrávání velkých souborů do databáze), ale stránky .aspx stále spuštění a kontrolery MVC ASP.NET později.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Asynchronně vyprazdňování odpověď

Odeslání odpovědi HTTP klientovi může trvat docela dlouho, když klient je daleko, nebo mají připojení s malou šířkou pásma. Obvykle technologie ASP.NET ukládá do vyrovnávací paměti bajty odpovědi jako vytvářené aplikace. ASP.NET potom provede operace odeslání jedné výši vyrovnávacích pamětí na konec zpracování požadavku.

Pokud ve vyrovnávací paměti odpovědí je velká (třeba streamování velkých souborů ke klientovi), musí pravidelně volala *HttpResponse.Flush* odeslat ve vyrovnávací paměti výstupního klienta a zachovat využití paměti pod kontrolou. Ale protože *vyprázdnění* je synchronní volání, využít iterativní volání *vyprázdnění* spotřebuje vlákna po dobu trvání potenciálně dlouhotrvající požadavky.

ASP.NET 4.5 přidává podporu pro provádění vyprázdní asynchronně pomocí *BeginFlush* a *EndFlush* metody *HttpResponse* třídy. Použití těchto metod, můžete vytvořit asynchronní moduly a asynchronní obslužné rutiny, které postupně odesílání dat klientovi bez obsadit vlákna operačního systému. Mezi *BeginFlush* a *EndFlush* volání, ASP.NET, verze aktuálního vlákna. To významně snižuje celkový počet aktivní vlákna, které jsou potřeba pro podporu dlouhotrvající HTTP soubory ke stažení.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Podpora pro *await* a *úloh* – na základě asynchronní moduly a obslužné rutiny

Rozhraní .NET Framework 4 zavedena asynchronní programovací koncept se označuje jako *úloh*. Úkoly jsou reprezentovány *úloh* typu a souvisejících typů v *System.Threading.Tasks* oboru názvů. Rozhraní .NET Framework 4.5 je založena na toto vylepšení kompilátoru, které usnadňuje práci s *úloh* jednoduché objekty. V rozhraní .NET Framework 4.5, kompilátory podporu dvou nových klíčových slov: *await* a *asynchronní*. *Await* – klíčové slovo je syntaktické sdružená hodnota určující, které jsou části kódu by měla asynchronně čekat na další část kódu. *Asynchronní* – klíčové slovo představuje pomocného parametru, který můžete použít k označení metod jako úkolově orientovanou asynchronní metody.

Kombinace *await*, *asynchronní*a *úloh* objektu je snazší pro vás bude psaní asynchronního kódu v rozhraní .NET 4.5. Technologie ASP.NET 4.5 podporuje tyto zjednodušení pomocí nových rozhraní API, které vám umožňují psát asynchronní moduly protokolu HTTP a asynchronní obslužné rutiny HTTP pomocí nových vylepšení kompilátoru.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Asynchronní moduly protokolu HTTP

Předpokládejme, že chcete provést v rámci metody, která vrací asynchronní práce *úloh* objektu. Následující příklad kódu definuje asynchronní metodu, která provádí asynchronní volání ke stažení na domovské stránce Microsoft. Všimněte si použití *asynchronní* – klíčové slovo v podpisu metody a *await* volání *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

To je všechno musíte napsat – rozhraní .NET Framework automaticky zpracovává odvíjení zásobníku volání při čekání na se stahování dokončí, jakož i po dokončení stahování automaticky obnovit zásobníku volání.

Nyní předpokládejme, že chcete použít tento asynchronní metody v asynchronní modulu HTTP technologie ASP.NET. ASP.NET 4.5 obsahuje pomocnou metodu (*EventHandlerTaskAsyncHelper*) a nový typ delegáta (*TaskEventHandler*), můžete použít k integraci založené na úlohách asynchronních metod pomocí staršího asynchronní programovací model, které jsou vystavené kanálu HTTP technologie ASP.NET. Tento příklad ukazuje, jak:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Asynchronní obslužné rutiny HTTP

Tradiční přístup k zápisu asynchronní obslužné rutiny v technologii ASP.NET je implementovat *IHttpAsyncHandler* rozhraní. Zavádí technologie ASP.NET 4.5 *HttpTaskAsyncHandler* asynchronní základní typ, který lze odvodit z, díky tomu je mnohem snazší psát asynchronní obslužné rutiny.

*HttpTaskAsyncHandler* typ je abstraktní a vyžaduje, abyste přepsat *ProcessRequestAsync* metody. Interně ASP.NET postará o integraci návratový podpis ( *úloh* objekt) z *ProcessRequestAsync* starší asynchronní programovací model používaný kanálu ASP.NET.

Následující příklad ukazuje, jak můžete *úloh* a *await* jako součást provádění asynchronní obslužné rutiny HTTP:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nové funkce pro ověření žádosti ASP.NET

Ve výchozím nastavení, provede ověření žádosti ASP.NET, zkontroluje požadavky k vyhledání kódu nebo skriptu v polích, záhlaví, soubory cookie a tak dále. Pokud se zjistí všechny ASP.NET dojde k výjimce. Funguje to jako první linie obrany proti potenciální útoky skriptování napříč weby.

ASP.NET 4.5 usnadňuje selektivně číst data neověřené žádosti. ASP.NET 4.5 se také integruje Oblíbené knihovny AntiXSS, která byla dříve externí knihovna.

Vývojáři mají často kladené umožňuje selektivně vypnout ověření žádosti pro své aplikace. Pokud vaše aplikace je software, fóra, můžete chtít umožnit uživatelům odeslat komentáře a příspěvky ve formátu HTML fórum, ale stále Ujistěte se, že žádost o ověření je kontrola všechno ostatní.

ASP.NET 4.5 zavádí dvě funkce, které usnadňují selektivně pracovat neověřený vstup: odložené ("opožděné") žádost o ověření a přístup k datům neověřené žádosti.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Odložené ověření žádosti ("opožděné")

V technologii ASP.NET 4.5 ve výchozím nastavení všechna data požadavku jsou neprošla ověřením požadavku. Ale můžete nakonfigurovat aplikaci do žádosti o ověření odložit, dokud skutečně přístup k datům požadavku. (To je někdy označovány jako ověření opožděné žádosti, na základě podmínek jako opožděné načtení pro určité scénáře data.) Můžete nakonfigurovat aplikaci, aby používala odložené ověření v souboru Web.config tak, že nastavíte *requestValidationMode* atribut 4.5 v *httpRUntime* element, jako v následujícím příkladu:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Pokud žádost o ověření režim je nastavený na 4.5, žádost o ověření se aktivuje pouze pro hodnotu konkrétního požadavku a pouze v případě, že váš kód přistupuje k hodnotě. Například, pokud váš kód získá hodnotu Request.Form["forum\_příspěvek"], ověření požadavku se vyvolá pouze pro daný element v kolekci formuláře. Žádný z elementů v *formuláře* kolekce se ověří. V předchozích verzích technologie ASP.NET byla aktivována ověření žádosti pro kolekci celý požadavek při přístupu k libovolnému prvku v kolekci. Nové chování usnadňuje různých aplikačních komponent podívat se na různé softwarové součásti dat požadavku bez aktivace ověření požadavku v dalších částech.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Podpora pro neověřené žádosti

Odložené žádost o ověření samostatně nebude vyřešen selektivně vynechání ověření žádosti. Volání Request.Form["forum\_příspěvek"] stále triggery žádost o ověření pro tuto hodnotu konkrétního požadavku. Však můžete chtít přístup toto pole bez vyvolání ověření, protože budete chtít povolit značky v daném poli.

K tomu, technologii ASP.NET 4.5 se teď podporuje neověřený přístup k datům požadavku. ASP.NET 4.5 obsahuje nový *Unvalidated* vlastnost kolekce v *HttpRequest* třídy. Tato kolekce poskytuje přístup ke všem z běžné hodnoty dat požadavku, jako je třeba *formuláře*, *QueryString*, *soubory cookie*, a *Url*.

Použijeme příklad fórum, bude moct číst data neověřené žádosti, Nejdřív musíte nakonfigurovat aplikaci, aby používala nový režim ověření požadavku:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Pak můžete použít *HttpRequest.Unvalidated* vlastnost načíst hodnotu neověřený formuláře:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> Zabezpečení – *používejte obezřetně, data neověřené žádosti!* ASP.NET 4.5 přidané vlastnosti neověřené žádosti a kolekcí zjednodušit přístup k datům specifickou neověřené žádosti. Vlastní ověřovací však musíte provést na požadavek nezpracovaných dat k zajištění, že není nebezpečné text vykreslen pro uživatele.


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Knihovny AntiXSS

Z důvodu oblíbenost Microsoft AntiXSS Library technologii ASP.NET 4.5 nyní zahrnuje základní rutiny kódování z verze 4.0 této knihovny.

Rutiny kódování jsou implementované *AntiXssEncoder* typu v novém *System.Web.Security.AntiXss* oboru názvů. Můžete použít *AntiXssEncoder* typ přímo pomocí volání metod statické kódování, které jsou implementovány v typu. Nejjednodušší způsob použití nové rutiny anti-XSS ale ke konfiguraci aplikace ASP.NET pro použití *AntiXssEncoder* třídy ve výchozím nastavení. Chcete-li to provést, přidejte následující atribut v souboru Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Když *encoderType* atribut nastaven pro použití *AntiXssEncoder* typ, veškerý výstup kódování v technologii ASP.NET automaticky použije nové rutiny pro kódování.

Toto jsou části externí knihovny AntiXSS, které byly zahrnuty do technologie ASP.NET 4.5:

- *HtmlEncode*, *HtmlFormUrlEncode*, a *HtmlAttributeEncode*
- *XmlAttributeEncode* a *XmlEncode*
- *UrlEncode* a *UrlPathEncode* (nové)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Podpora protokolu WebSockets

Protokol Websocket je založené na standardech síťový protokol, který definuje, jak vytvořit zabezpečené a v reálném čase obousměrnou komunikaci mezi klientem a serverem přes protokol HTTP. Společnost Microsoft spolupracuje s IETF i W3C těla standardy, aby pomohl definovat protokolu. Protokol Websocket je podporována libovolného klienta (nejen prohlížeče), s Microsoftem investic podpora protokolu Websocket na klientovi a mobilních operačních systémů značné prostředky.

Protokol Websocket je snazší vytvářet dlouhotrvající datové přenosy mezi klientem a serverem. Například psaní chatovací aplikaci je mnohem jednodušší, protože umí vytvořit true dlouhotrvajících připojení mezi klientem a serverem. Není potřeba uchýlíte k řešení, jako je pravidelná dotazování nebo HTTP s dlouhým intervalem dotazování pro simulaci chování soketu.

ASP.NET 4.5 a službu IIS 8 zahrnují nízké úrovně podporu Websocket, umožňuje vývojářům technologie ASP.NET použití spravovaného rozhraní API pro asynchronní čtení a zápis řetězec a binárních dat na objektu Websocket. Pro technologii ASP.NET 4.5, je tu nový *System.Web.WebSockets* obor názvů, který obsahuje typy pro práci s protokolu Websocket.

Klientský prohlížeč naváže připojení WebSockets tak, že vytvoříte modelu DOM *protokolu WebSocket* , která odkazuje na adresu URL v aplikaci ASP.NET, jako v následujícím příkladu:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

V technologii ASP.NET pomocí jakéhokoli druhu, modul nebo obslužná rutina můžete vytvořit koncové body objekty Websocket. V předchozím příkladu byl použit soubor .ashx, protože soubory .ashx rychlý způsob, jak vytvořit obslužnou rutinu.

Aplikace ASP.NET podle protokolu Websocket přijímá požadavku Websocket klienta tak, že, že žádost je potřeba upgradovat z požadavku HTTP GET na žádost o objekty Websocket. Tady je příklad:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

*AcceptWebSocketRequest* metoda přijímá delegáta funkce, protože technologie ASP.NET unwinds aktuální žádost HTTP a poté předá řízení delegáta funkce. Tento přístup se koncepčně podobá na použití *funkce System.Threading.Thread*, kde můžete definovat delegáta spuštění vlákna na pozadí, které se provádí práci.

Po technologie ASP.NET a klient úspěšně dokončili handshake protokoly Websocket, ASP.NET volání delegáta a spuštění aplikace objekty Websocket. Následující příklad kódu ukazuje jednoduchý odezvu aplikace, která používá předdefinovanou podporu Websocket v technologii ASP.NET:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

Podporu pro rozhraní .NET 4.5 *await* – klíčové slovo a asynchronních operací podle úloh je přirozeně vhodná pro psaní aplikací objekty Websocket. Příklad kódu ukazuje zcela asynchronně spustí požadavek Websocket uvnitř technologie ASP.NET. Aplikace asynchronně čeká na zprávu k odeslání z klienta pomocí volání *await soketu. ReceiveAsync*. Podobně můžete odeslat asynchronních zpráv do klienta voláním *await soketu. SendAsync*.

V prohlížeči, aplikace přijme WebSockets zpráv prostřednictvím *onmessage* funkce. Chcete-li odeslat zprávu z prohlížeče, zavolejte *odeslat* metodu *objektu websocket na straně* typ modelu DOM, jak je znázorněno v tomto příkladu:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

V budoucnu může být vydáváme aktualizace k této funkci, abstraktní okamžitě některé nízké úrovně kódování, který je povinné v této verzi pro objekty Websocket aplikace.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Sdružování a Minifikace

Sdružování umožňuje kombinovat jednotlivých souborů JavaScript a CSS do sady prostředků, které mohou být považovány za jeden soubor. Připravenost k minifikaci zestruční souborů JavaScript a CSS odebráním prázdné znaky a dalších znaků, které nejsou povinné. Tyto funkce pracují s webovými formuláři ASP.NET MVC a webové stránky.

Balíčky jsou vytvořené pomocí sady třídy nebo některé z jejích podřízených tříd ScriptBundle a StyleBundle. Po dokončení konfigurace instance sady, je sady k dispozici na příchozí požadavky tak, že jednoduše přidáte do globální instance BundleCollection. Ve výchozích šablonách konfigurace sady se provádí v souboru BundleConfig. Toto výchozí nastavení vytvoří svazky pro všechna jádra skriptů a souborů css používaný šablony.

Sady jsou odkazovány z v rámci zobrazení pomocí jedné z několika možných pomocné metody. Za účelem podpory vykreslování různých značek pro sadu při ladění vs. režimu vydání, mají třídy ScriptBundle a StyleBundle Pomocná metoda, vykreslování. Vykreslování v režimu ladění, vygeneruje kód pro každý prostředek v sadě. V režimu vydání, vygeneruje vykreslení elementu jeden kód pro celou sadu. Při přepínání mezi debug a release režimu můžete udělat upravením atributu ladění kompilace prvku v souboru web.config, jak je znázorněno níže:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Kromě toho povolení nebo zakázání optimalizace může být nastavena přímo prostřednictvím vlastnosti BundleTable.EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Když soubory jsou spojeny, jsou nejprve seřazené podle abecedy (způsob, jak jsou zobrazená v **Průzkumníka řešení**). Potom seřazené tak, aby známé knihovny a jejich vlastní rozšíření (jako je jQuery, MooTools a Dojo) jsou načten jako první. Například se bude konečnou verzi objednávky pro sdružování složky Scripts, jak je uvedeno výše:

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a.js

Soubory šablon stylů CSS jsou také seřazené podle abecedy a pak znovu uspořádat tak, aby reset.css a normalize.css předcházet jakýkoli jiný soubor. Konečné řazení sdružování složce styly uvedené výše, bude toto:

1. reset.css
2. Content.CSS
3. Forms.CSS
4. globals.css
5. menu.css
6. Styles.CSS

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Vylepšení výkonu pro hostování webů

Rozhraní .NET Framework 4.5 a Windows 8 zavádí funkce, které vám mohou pomoci dosáhnout zvýšení výkonu pro úlohy webového serveru. Jedná se o snížení (až o 35 %) v obou čas spuštění a nároky na paměť pro webové hostování webů, které používají technologii ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Výkon klíčové faktory

V ideálním případě by všechny weby musí být aktivní a v paměti, aby zajistil rychlé odpovědi na další požadavek, vždy, když jde o. Mezi faktory, které můžou ovlivnit rychlost odezvy serveru patří:

- Čas potřebný pro webový server restartovat po fond aplikací recykluje. To je čas potřebný ke spuštění procesu webového serveru pro lokalitu, když sestavení lokality již nejsou v paměti. (Sestavení platformy jsou stále v paměti, protože jsou používány jiné weby). Tato situace se označuje jako "studenou web, spuštění teplé framework" nebo jen "studeného lokality po spuštění."
- Kolik paměti zabírá webu. Podmínky pro to jsou "spotřeba paměti na serveru" nebo "odstranit pracovní sady".

Nová vylepšení výkonu se zaměřují na oba z těchto faktorů.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Požadavky pro nové funkce výkonu

Požadavky na nové funkce můžete rozdělit do těchto kategorií:

- Vylepšení, které běží na rozhraní .NET Framework 4.
- Vylepšení, které vyžadují rozhraní .NET Framework 4.5, ale můžete spustit na kteroukoli verzi Windows.
- Vylepšení, které jsou k dispozici pouze u rozhraní .NET Framework 4.5 a systémem Windows 8.

Výkon se zvyšuje s každou úroveň vylepšení, které je možné povolit.

Některá vylepšení rozhraní .NET Framework 4.5 využít širší funkce výkonu, které se vztahují i jiné scénáře.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Sdílení běžné sestavení

**Požadavek**: rozhraní .NET Framework 4 a Visual Studio 11 Developer Preview SDK

Stejné sestavení pomocné rutiny (například sestavení z starter kit či ukázkové aplikace) se často používají různé stránky ze serveru. Každá lokalita má svou vlastní kopii těchto sestavení v adresáři Bin. I když je stejný jako objektový kód pro sestavení, jsou fyzicky oddělená sestavení, tak každé sestavení má ke čtení samostatně během spouštění studenou lokality a udržovány odděleně v paměti.

Nové funkce interning řeší tato neefektivnost a snižuje požadavky na paměť RAM a načtení času. Interning umožňuje Windows udržovat jednu kopii každého sestavení v systému souborů a jednotlivá sestavení ve složce Bin lokality jsou nahrazeny symbolické odkazy na jedné kopii. Pokud jedna lokalita potřebuje různé verze sestavení, symbolický odkaz je nahrazena novou verzi sestavení a má vliv pouze v dané lokalitě.

Sdílení sestavení s pomocí symbolické odkazy vyžaduje nový nástroj s názvem aspnet\_intern.exe, která vám umožní vytvořit a spravovat úložiště internovány sestavení. Je zadaný jako součást sady Visual Studio 11 Developer Preview SDK. (Však bude fungovat v systému, který má pouze rozhraní .NET Framework 4 nainstalované, za předpokladu, že máte nainstalovanou nejnovější verzi [aktualizovat](https://support.microsoft.com/kb/2468871).)

Chcete-li Ujistěte se, že všechny oprávněné sestavení mají byla internovány, spusťte aspnet\_intern.exe pravidelně (třeba jednou za týden naplánované úlohy). Typické použití je následujícím způsobem:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Pokud chcete zobrazit všechny možnosti, spusťte nástroj bez argumentů.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Používání vícejádrové kompilace JIT pro rychlejší spuštění

**Požadavek**: rozhraní .NET Framework 4.5

Pro spuštění studenou lokality nejen sestavení by měly být přečtený z disku, ale web musí být JIT kompilován. Pro komplexní webový server to přidat významné zpoždění. Nová univerzální metoda v rozhraní .NET Framework 4.5 snižuje tyto zpoždění tím, že rozprostírá kompilace JIT mezi jader procesoru. Dělá to největší a napravovat je co možná s použitím informace shromážděné během předchozí spustí lokality. Tato funkce implementované [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) metody.

JIT kompilaci pomocí více jader je povolena ve výchozím nastavení technologie ASP.NET, takže není potřeba dělat nic. Chcete využít výhod této funkce. Pokud chcete tuto funkci zakázat, nastavte v souboru Web.config následující nastavení:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Optimalizace uvolňování paměti optimalizace paměti

**Požadavek**: rozhraní .NET Framework 4.5

Jakmile web běží, jeho použití haldy systému uvolňování paměti (GC) může být důležitým faktorem při jeho paměťové nároky. Stejně jako jakékoli systému uvolňování paměti uvolňování paměti rozhraní .NET Framework umožňuje kompromisy mezi čas procesoru (četnost a význam kolekcí) a využití paměti (místo navíc, který se používá pro nové, uvolněné nebo možné uvolnit objekty). Pro předchozí verze, poskytujeme pokyny o tom, jak nakonfigurovat uvolňování paměti, abyste dosáhli správné rovnováhy (viz například [ASP.NET 2.0 a 3.5 sdílená hostování konfigurace](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Pro rozhraní .NET Framework 4.5, namísto více samostatné nastavení, nastavení konfigurace definované úlohy je k dispozici, která umožňuje všechny dříve doporučená nastavení uvolňování paměti a také nové optimalizace, která poskytuje další výkon na pracoviště pracovní sadu.

Povolit optimalizace paměti uvolňování paměti, přidejte do souboru Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config následující nastavení:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Pokud jste obeznámeni s předchozím pokynů pro změny do aspnet.config, mějte na paměti, že toto nastavení nahradí původní nastavení – třeba není nutné nastavit gcServer, gcConcurrent atd. Nemáte k stará nastavení odebrala.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Předběžné načítání pro webové aplikace

**Požadavek**: rozhraní .NET Framework 4.5 a systémem Windows 8

Pro několik verzí Windows je součástí technologie označované jako [prefetcher](http://en.wikipedia.org/wiki/Prefetcher) , která snižuje náklady na čtení disku spuštění aplikace. Protože úplné spuštění je nějaký problém, převážně pro klientské aplikace, tato technologie nebylo součástí Windows serveru, který obsahuje jenom komponenty, které jsou nezbytné k serveru. Předběžné načítání je nyní k dispozici v nejnovější verzi Windows serveru, kde ji můžete optimalizovat spuštění jednotlivých webů.

Pro systém Windows Server prefetcher není povolená ve výchozím nastavení. Pokud chcete povolit a konfigurovat prefetcher pro hostování webů s vysokou hustotou, spusťte následující sady příkazů na příkazovém řádku:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Prefetcher integrovat s aplikací ASP.NET, přidejte následující v souboru Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>ASP.NET – webové formuláře

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Ovládací prvky dat silného typu

Webové formuláře v technologii ASP.NET 4.5 obsahuje několik vylepšení pro práci s daty. První zlepšení je ovládací prvky dat silného typu. Pro ovládací prvky webových formulářů v předchozích verzích technologie ASP.NET, můžete zobrazit hodnotu vázané na data pomocí *Eval* a výraz datové vazby:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Pro obousměrný datové vazby, je použít *svázat*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

V době běhu pomocí těchto volání reflexe načíst hodnotu zadaného člena a pak zobrazí výsledky v kódu. Tento přístup usnadňuje svázat data s daty libovolného, unshaped.

Výrazy vázání dat, jako je to ale nepodporují funkce jako IntelliSense pro názvy členů, navigace (jako přejít k definici) nebo kompilace kontrolu pro tyto názvy.

ASP.NET 4.5 a tento problém vyřešit, přidává možnost deklarovat datový typ dat, která je vytvořena vazba ovládacího prvku na. Můžete to provést pomocí nové *ItemType* vlastnost. Když nastavíte tuto vlastnost, dvě nové typované proměnné jsou k dispozici v oboru výrazy vázání dat: *položky* a *položku BindItem*. Protože proměnné jsou silného typu, získáte všechny výhody vývojové prostředí sady Visual Studio.


Obousměrný výrazy vázání dat, použijte *položku BindItem* proměnné:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


Většina ovládacích prvků v rámci webových formulářů ASP.NET, které podporují vytváření datových vazeb mají byla aktualizována o podporu *ItemType* vlastnost.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Vazby modelu

Vazby modelu rozšiřuje datové vazby v ovládací prvky webových formulářů ASP.NET pro práci s přístupem k datům zaměřený na kód. Zahrnuje koncepty z *ObjectDataSource* ovládacího prvku a z vazby modelu v architektuře ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Výběr dat

Chcete-li nakonfigurovat ovládací prvek data pomocí vazby modelu vyberte data, nastavte ovládacího prvku *metoda SelectMethod* nastavte název metody v kódu stránky. Ovládací prvek dat volá metodu v příslušnou dobu v životním cyklu stránky a automaticky sváže s vrácenými daty. Není nutné explicitně volat *DataBind* metody.

V následujícím příkladu *GridView* ovládací prvek je nakonfigurován na použití metodu s názvem *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Můžete vytvořit *GetCategories* metody v kódu stránky. Pro jednoduché operace select, metoda potřebuje žádné parametry a by měl vrátit *IEnumerable* nebo *IQueryable* objektu. Pokud nový *ItemType* je nastavena (tím umožníte silného typu výrazy vázání dat, jak je popsáno v části [silně typované ovládací prvky dat](#_Toc318097386) dříve), v obecné verzi tato rozhraní má být vrácen – *IEnumerable&lt;T&gt;*  nebo *IQueryable&lt;T&gt;*, se *T* parametr typu odpovídající *ItemType* vlastnosti (například *IQueryable&lt;kategorie&gt;*).

Následující příklad ukazuje kód *GetCategories* metody. Tento příklad používá model Entity Framework Code First s ukázkovou databází Northwind. Kód zajišťuje, že dotaz vrátí podrobnosti o souvisejících produktů pro každou kategorii prostřednictvím *zahrnout* metody. (To zajistí, že *TemplateField* elementu v kódu zobrazí počet produktů v každé kategorii bez nutnosti [n + 1 vyberte](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Při spuštění stránky, *GridView* řídit volání *GetCategories* metoda automaticky a vykreslí vrácená data s využitím nakonfigurované pole:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Protože vrací metody select *IQueryable* objektu, *GridView* ovládacího prvku můžete dál pracovat s dotaz před jeho provedením. Například *GridView* ovládacího prvku můžete přidat výrazy dotazu pro řazení a stránkování na vrácenou *IQueryable* objektu předtím, než se spustí, tak, aby tyto operace provádí základní Poskytovatel LINQ. V takovém případě Entity Framework zajistí, že tyto operace jsou prováděny v databázi.

Následující příklad ukazuje *GridView* ovládací prvek upravit tak, aby Povolit řazení a stránkování:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Nyní při spuštění stránky, ovládací prvek jistotu, že se zobrazí pouze na aktuální stránku dat a že jsou seřazená podle vybraného sloupce:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

K filtrování vrácená data, parametry se mají přidat do metody select. Tyto parametry vyplněn prostředkem vazby modelu za běhu a můžete je změnit dotaz před vrácením data.

Předpokládejme například, že chcete umožnit uživatelům filtrování produktů zadáním klíčového slova v řetězci dotazu. Můžete přidat parametr do metody a aktualizovat kód, který použije hodnotu parametru:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Tento kód obsahuje *kde* výrazu, je-li zadat hodnotu pro *– klíčové slovo* a vrátí výsledky dotazu.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Zprostředkovatele hodnot

V předchozím příkladu nebyl konkrétní o tom, kde hodnota *– klíčové slovo* parametr byl pocházející z. K označení tyto informace můžete použít atribut parametru. V tomto příkladu můžete použít *QueryStringAttribute* třídu, která je v *System.Web.ModelBinding* obor názvů:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Toto dá pokyn vazby modelu pro pokus o vytvoření vazby hodnotu z řetězce dotazu do *– klíčové slovo* parametr v době běhu. (To může zahrnovat provádění převodů, i když v tomto případě nepodporuje.) Pokud nelze zadat hodnotu a typ je null, je vyvolána výjimka.

Zdroje z hodnot pro tyto metody jsou označovány jako zprostředkovatele hodnot, a parametr atributy, které označují které zprostředkovatele hodnot pro použití se označují jako atributy zprostředkovatele hodnot. Webové formuláře bude obsahovat zprostředkovatele hodnot a odpovídající atributy pro všemi typické zdroji uživatelský vstup v aplikaci webových formulářů, jako je například řetězec dotazu, soubory cookie, hodnot formuláře, ovládací prvky, zobrazit stav, stav relace a vlastnosti profilu. Můžete taky psát vlastní hodnotu poskytovatelů.

Ve výchozím nastavení název parametru slouží jako klíč k vyhledání hodnoty v kolekci zprostředkovatele hodnot. V tomto příkladu bude vypadat kód pro hodnotu řetězce dotazu s názvem – klíčové slovo (například ~ / default.aspx?keyword=chef). Můžete zadat vlastní klíč předáním jako argument pro parametr atributu. Například pokud chcete použít hodnotu proměnné řetězce dotazu s názvem q, můžete tak učinit:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Pokud tato metoda je v kódu stránky, uživatelé mohou filtrovat výsledky tím, že předáte řetězec dotazu pomocí klíčového slova:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Vazby modelu provádí řadu úloh, které byste jinak museli naprogramovat ručně: čtení hodnoty, kontrola hodnot null, pokusu o převod na příslušný typ, kontroluje, zda byl převod úspěšný a nakonec pomocí hodnoty v dotaz. Model vazby výsledky v mnohem méně kódu a možnosti opakovaně používat funkce v rámci aplikace.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtrování podle hodnot pomocí ovládacího prvku

Předpokládejme, že chcete příklad rozšířit na uživatelům povolit, vyberte filtr hodnotu z rozevíracího seznamu. Přidejte následující rozevíracího seznamu kód a nakonfigurujte jej pro získání dat z jiné pomocí metody *metoda SelectMethod* vlastnost:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

Obvykle by také přidáte *EmptyDataTemplate* elementu *GridView* tak, aby ovládací prvek zobrazí zprávu, pokud nejsou nalezeny žádné odpovídající produkty:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

V kódu stránky přidejte nový, vyberte metodu pro rozevíracího seznamu:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Nakonec aktualizujte *GetProducts* vyberte metodu přijmout nový parametr, který obsahuje ID vybrané kategorie z rozevíracího seznamu:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Nyní při spuštění stránky mohou uživatelé vybrat kategorii z rozevíracího seznamu a *GridView* ovládací prvek je automaticky znovu vázaná na zobrazí filtrovaná data. To je možné, protože sleduje hodnoty parametrů pro výběr metody vazby modelu a zjistí, zda všechny hodnoty parametru změnila po zpětném odeslání. Pokud ano, vynutí vazby modelu ovládací prvek související data se znovu vytvořit vazbu na data.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Výrazy vázání dat kódovaný jazykem HTML

Můžete teď použije kódování HTML. výsledkem výrazů datové vazby. Přidejte dvojtečku (:) na konec objektu &lt;% # předponu, která označí vazbový výraz:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Nerušivý ověření

Teď můžete nakonfigurovat program pro ověření integrované ovládací prvky pro použití nerušivý JavaScript pro logiku ověřování na straně klienta. To významně snižuje počet JavaScript nevykreslují jako vložené v kódu stránky a snižuje celkové velikosti stránky. Nerušivý JavaScript pro ovládací prvky můžete nakonfigurovat v některém z těchto způsobů:

- Globálně přidáním následujícího nastavení *&lt;appSettings&gt;* element v souboru Web.config: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Globálně nastavením statické *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* vlastnost *UnobtrusiveValidationMode.WebForms* (obvykle v *aplikace \_Start* metody v souboru Global.asax).
- Jednotlivě pro stránku tak, že nastavíte nový *UnobtrusiveValidationMode* vlastnost *stránky* třídu *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Aktualizace HTML5

Několik vylepšení byly provedeny do webových formulářů serverové ovládací prvky, abyste mohli využívat nové funkce HTML5:

- *v textovém režimu* vlastnost *TextBox* ovládací prvek má byla aktualizována o podporu nových typů vstupu HTML5 jako *e-mailu*, *data a času*, a atd.
- *FileUpload* nyní podporuje nahrání více souborů z prohlížečů, které podporují tuto funkci HTML5 ovládací prvek.
- Program pro ověření řídí teď podporu ověřování HTML5 vstupních prvků.
- Nové prvky HTML5, které mají atributy, které představují adresy URL teď podporují runat = "server". V důsledku toho může používat technologie ASP.NET v cestě adresy URL, jako je třeba ~ – operátor pro reprezentaci kořenový adresář aplikace (například &lt;videa runat = "server" src="~/myVideo.wmv" /&gt;).
- *UpdatePanel* Opravili jsme ovládací prvek pro podporu vstupních polí účtování HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

Beta verze technologie ASP.NET MVC 4 je nyní součástí sady Visual Studio 11 Beta. ASP.NET MVC je architektura pro vývoj webových aplikací s možností intenzivního testování a údržby s využitím vzor Model-View-Controller (MVC). ASP.NET MVC 4 usnadňuje vytváření aplikací pro mobilní Web a zahrnuje rozhraní ASP.NET Web API, které usnadňuje sestavování služeb HTTP, které mají přístup všechna zařízení. Další informace najdete v tématu [zpráva k vydání verze technologie ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET – webové stránky 2

Nové funkce patří:

- Šablony webů nové a aktualizované.
- Přidání na straně serveru a ověřování na straně klienta pomocí *ověření* pomocné rutiny.
- Možnost registrovat skripty pomocí Správce prostředků.
- Povolení přihlášení z Facebooku a dalších lokalit pomocí OAuth a OpenID.
- Přidání map pomocí *mapuje* pomocné rutiny.
- Spuštění webové stránky aplikace vedle sebe.
- Vykreslení stránky pro mobilní zařízení.

Další informace o těchto funkcích a mají formu celostránkového příklady najdete v tématu [The horní funkce v beta verzi 2 webové stránky](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

Tato část obsahuje informace o vylepšení pro vývoj webů v aplikaci Visual Web Developer 11 Beta a verze Release Candidate sady Visual Studio 2012.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Projekt pro sdílení obsahu mezi Visual Studio 2010 a Visual Studio 2012 Release Candidate (Kompatibilita projektu)

Až do Visual Studio 2012 Release Candidate otevřete existující projekt v novější verzi sady Visual Studio spustí Průvodce převodu. To se upgrade obsahu (prostředků) v projektu a řešení vynutit nové formáty, které nejsou zpětně kompatibilní. Po převodu, proto nelze otevřít projekt ve starší verzi sady Visual Studio.

Mnoho zákazníků uvedli, že není správný přístup. V aplikaci Visual Studio 11 Beta podporujeme nyní sdílení projektů a řešení s Visual Studio 2010 SP1. To znamená, že pokud otevřete projekt 2010 ve Visual Studio 2012 Release Candidate, stále budete moci otevřít projekt v aplikaci Visual Studio 2010 SP1.

> [!NOTE]
> Několik typů projektů se nedají sdílet mezi Visual Studiem 2010 SP1 a Visual Studio 2012 Release Candidate. Patří mezi ně některé starší projekty (jako jsou projekty ASP.NET MVC 2) nebo projekty pro zvláštní účely (například projekty instalace).

Při otevření projektu Visual Studio 2010 SP1 Web poprvé ve verzi Visual Studio 11 Beta, následující vlastnosti jsou přidány do souboru projektu:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags UpgradeBackupLocation a OldToolsVersion se používají procesem, který provede upgrade souboru projektu. Nemají žádný vliv na práci v projektu v sadě Visual Studio 2010.

VisualStudioVersion je nová vlastnost používá MSBuild 4.5, která určuje verzi sady Visual Studio pro aktuální projekt. Protože tato vlastnost tenkrát neexistovaly v MSBuild 4.0 (verzi nástroje MSBuild, který používá Visual Studio 2010 SP1), vložíme výchozí hodnotu do souboru projektu.

Vlastnost VSToolsPath slouží k určení správné .targets soubor k importu z cesty reprezentována MSBuildExtensionsPath32 nastavení.

Existují také některé změny související s elementy importu. Tyto změny jsou potřeba, abyste mohli podporovat obě verze systému Visual Studio pro kompatibilitu.

> [!NOTE]
> Pokud je projekt sdílí mezi Visual Studiem 2010 SP1 a Visual Studio 11 Beta na dvou různých počítačích, a pokud projekt obsahuje místní databáze v aplikaci\_složce dat, ujistěte se, že je verze systému SQL Server používaný databází v obou počítačích nainstalován.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Změny konfigurace v šablonách technologie ASP.NET 4.5 webu

Následující změny byly provedeny na výchozí hodnotu *Web.config* souborů pro lokalitu, která jsou vytvořena pomocí šablony webu v aplikaci Visual Studio 2012 Release Candidate:

- V `<httpRuntime>` elementu, `encoderType` atribut je teď ve výchozím nastavení používají AntiXSS typy, které byly přidány do technologie ASP.NET. Podrobnosti najdete v tématu [knihovny AntiXSS](#_Toc318097382).
- Také v `<httpRuntime>` elementu, `requestValidationMode` atribut je nastaven na "4.5". To znamená, že ve výchozím nastavení, ověření žádosti konfigurován pro použití ověřování odložené ("opožděné"). Podrobnosti najdete v tématu [nových funkcí technologie ASP.NET žádost o ověření](#_Toc318097379).
- `<modules>` Elementu `<system.webServer>` neobsahuje oddíl `runAllManagedModulesForAllRequests` atribut. (Výchozí hodnota je false). To znamená, že pokud používáte verzi služby IIS 7, která nebyla aktualizována na verzi SP1, může být problémy se směrováním v nové lokalitě. Další informace najdete v tématu [nativní podporu ve službě IIS 7 pro směrování ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine).

Tyto změny neovlivní stávající aplikace. Může však představují rozdíl v chování mezi stávající a nové weby, kterou vytvoříte pro technologii ASP.NET 4.5 pomocí nové šablony.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Nativní podpora ve službě IIS 7 pro směrování ASP.NET

Toto není změnu ASP.NET jako takové, ale změna šablony pro nové projekty webových stránek, které můžou ovlivnit při práci na verzi služby IIS 7, která nemá nainstalovanou aktualizaci SP1.

V technologii ASP.NET můžete přidat následující nastavení konfigurace pro aplikace za účelem podpory směrování:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Při **runAllManagedModulesForAllRequests** je true, adresy URL ve `http://mysite/myapp/home` přejde na technologii ASP.NET, i když není žádný *.aspx*, *.mvc*, nebo podobné rozšíření na ADRESA URL.

Aktualizace, která byla vytvořená pro službu IIS 7 umožňuje **runAllManagedModulesForAllRequests** nastavení nepotřebné a podporuje nativně směrování ASP.NET. (Informace o této aktualizaci najdete v tématu Článek Microsoft Support [aktualizace je dostupná, umožňuje některé služby IIS 7.0 a IIS 7.5 obslužné rutiny pro zpracování požadavků, jejichž adresy URL nesmí končit tečkou](https://support.microsoft.com/kb/980368).)

Pokud váš web spuštěn ve službě IIS 7 a pokud služba IIS byla aktualizována, není nutné nastavovat **runAllManagedModulesForAllRequests** na hodnotu true. Ve skutečnosti nastavení na hodnotu true se nedoporučuje, protože přispějete ke zbytečné režie zpracování na požadavek. Když je toto nastavení hodnotu true, všechny požadavky, včetně těch, které pro *.htm*, *.jpg*, a další statické soubory taky projít kanálu požadavku ASP.NET.

Pokud vytvoříte nového webu technologie ASP.NET 4.5 pomocí šablony, které jsou k dispozici v sadě Visual Studio 2012 RC, konfigurace pro web neobsahuje **runAllManagedModulesForAllRequests** nastavení. To znamená, že ve výchozím nastavení je false.

Poté při spuštění webu ve Windows 7 bez aktualizace SP1 nainstalovaná, IIS 7 nezahrnuje požadované aktualizace. V důsledku toho směrování nebude fungovat a zobrazí se chyby. Pokud máte potíže, pokud směrování nefunguje, můžete provést buď následující:

- Aktualizujte Windows 7 SP1, který se přidá aktualizace do služby IIS 7.
- Nainstalujte aktualizaci podle popisu v předchozí článek Microsoft Support.
- Nastavte **runAllManagedModulesForAllRequests** na hodnotu true v souboru Web.config tohoto webu. Všimněte si, že tato možnost přidá režijní náklady na požadavky.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Editor HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Inteligentní úlohy

V návrhovém zobrazení komplexní vlastnosti serverových ovládacích prvků často přiřadili dialogová okna a průvodce, které usnadňují jejich nastavení. Například můžete použít speciální dialogové okno Přidat zdroj dat *Repeater* ovládací prvek nebo sloupce, které chcete přidat *GridView* ovládacího prvku.

Tento typ Nápověda k uživatelskému rozhraní pro komplexní vlastnosti však nebyl k dispozici v zobrazení zdroje. Proto Visual Studio 11 zavádí inteligentní úlohy pro zobrazení zdroje. Inteligentní úlohy jsou kontextová klávesové zkratky pro běžně používané funkce v editoru jazyka C# a Visual Basic.

Pro ovládací prvky webových formulářů ASP.NET, inteligentní úkoly se zobrazí na serverové značky jako malé piktogram při kurzor se nachází uvnitř elementu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

Inteligentní úkol rozbalí, když kliknete piktogram nebo stiskněte klávesy CTRL +. (tečka), stejně jako u editory kódu. Pak zobrazí klávesových zkratek, které jsou podobné úlohám inteligentní v zobrazení návrhu.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Například inteligentní úloh na předchozím obrázku jsou uvedené možnosti úloh ovládacího prvku GridView. Pokud vyberete možnost Upravit sloupce, zobrazí se následující dialogové okno:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Vyplnění pole sad dialogového okna stejné vlastnosti můžete nastavit v návrhovém zobrazení. Po kliknutí na OK, značky pro ovládací prvek se aktualizuje s novým nastavením:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Podpora v POČKA ARIA

Zápis dostupné weby se mění nabývá na důležitosti. [POČKA ARIA usnadnění standardní](http://www.w3.org/WAI/intro/aria) definuje, jak vývojáři musí psát dostupné weby. Tento standard jsou nyní plně podporovány v sadě Visual Studio.

Například *role* atribut teď má plnou podporou technologie IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

POČKA ARIA standard také zavádí atributy, které mají předponu *aria -* , který slouží k přidání sémantiky do dokumentu HTML5. Visual Studio také podporuje tyto *aria -* atributy:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png)![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Nová specifikace HTML5 fragmenty kódu

Chcete-li zrychluje a usnadňuje psaní běžně používaných značek HTML5, Visual Studio obsahuje několik fragmentů kódu. Příklad je grafické fragment kódu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Vyvolat fragment kódu, stiskněte klávesu Tab, dvakrát, pokud element je vybrán v IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Tímto se vytvoří fragment kódu, který si můžete přizpůsobit.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Extrahovat do uživatelského ovládacího prvku

Ve velkých webových stránek může být vhodné přesunout jednotlivé funkční součásti do uživatelské ovládací prvky. Tato forma refaktoringu můžete použít ke zvýšení přehlednosti stránky a zjednodušit struktura stránek.

Pro lepší pochopení, při úpravě stránky webových formulářů v zobrazení zdroje, můžete teď vybrat text na stránce, pravým tlačítkem myši a klikněte na tlačítko Extrahovat do uživatelského ovládacího prvku:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>Technologie IntelliSense pro kód útržky v atributech

Visual Studio stále poskytuje IntelliSense pro kód na straně serveru útržky v libovolnou stránku nebo ovládací prvek. Visual Studio teď obsahuje rozšíření IntelliSense pro útržky kódu a atributů HTML.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

To usnadňuje vytváření datové vazby výrazů:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Automatické přejmenování odpovídající značky při přejmenování otevírací nebo uzavírací značku

Pokud přejmenujete HTML element (například změnit *div* značka, které je možné *záhlaví* značka), odpovídající počáteční ani koncovou značku změní také v reálném čase.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

To pomáhá předejít chyba, pokud zapomenete změnit uzavírací značku nebo je nesprávné.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Generování obslužné rutiny události

Visual Studio nyní zahrnuje funkce v zobrazení zdroje při zápisu obslužných rutin událostí a svázat ho ručně. Pokud upravujete, název události v zobrazení zdroje, IntelliSense zobrazí &lt;vytvořit novou událost&gt;, který vytvoří obslužnou rutinu události v kódu stránky, která nemá správný podpis:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Ve výchozím nastavení použije obslužná rutina události ID ovládacího prvku pro název metody zpracování událostí:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Výsledný obslužné rutiny události bude vypadat takto (v tomto případě v jazyce C#):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Inteligentní odsazení

Po stisknutí klávesy Enter uvnitř prázdný element HTML, editor bude umístěte kurzor na správném místě:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Po stisknutí klávesy Enter v tomto umístění, je ukončovací značku přesunout dolů a odsazené tak, aby odpovídaly počáteční značka. Kurzor je také odsazen:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Snižte automatické dokončování příkazů

Seznam technologie IntelliSense v sadě Visual Studio nyní filtry podle, co zadáte tak, aby zobrazil pouze příslušné možnosti:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

Technologie IntelliSense také filtry podle názvu velikost písmen jednotlivých slov v seznamu technologie IntelliSense. Například pokud zadáte "dl", bude používat a asp: DataList se zobrazí:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Díky této funkci můžete rychleji získat doplňování výrazů pro známé elementy.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript – editor

Editor jazyka JavaScript v aplikaci Visual Studio 2012 Release Candidate je zcela nové a výrazně zlepšuje možností práce s použitím jazyka JavaScript v sadě Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Sbalování kódu

Sbalování oblastí jsou nyní automaticky vytvoří pro všechny funkce, díky tomu umožňuje sbalit části souboru, které nejsou relevantní pro váš aktuální výběr.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Párování závorek

Při umístění kurzoru na levou nebo pravou složenou závorku zvýrazní editor odpovídající jedné.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Přejít k definici

Přejít na definici – příkaz umožňuje přejít na zdroj pro funkce nebo proměnné.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Podpora ECMAScript5

Editor podporuje rozhraní API a nové syntaxe v ECMAScript5 nejnovější verzi standard, který popisuje jazyka JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>Technologie IntelliSense modelu DOM

Vylepšená technologie IntelliSense pro rozhraní API modelu DOM s podporou pro mnoho nových rozhraní API pro HTML5 včetně *querySelector*, úložiště modelu DOM, zasílání zpráv mezi dokumenty a *plátno*. Technologie IntelliSense modelu DOM teď vychází podle jednoho jednoduchého souboru jazyka JavaScript, nikoli podle definice knihovny nativního typu. To umožňuje snadno rozšířit nebo nahradit.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Signatura přetížení VSDOC

Nyní mohou být deklarovány podrobnými poznámkami technologie IntelliSense pro samostatné přetížení funkce jazyka JavaScript pomocí nových *&lt;podpis&gt;* elementu, jak je znázorněno v tomto příkladu:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Implicitní odkazy

Nyní můžete přidat soubory jazyka JavaScript do centrální seznamu, který se implicitně zahrne seznam souborů, že všechny daného jazyka JavaScript souboru nebo blok odkazy, to znamená, že získáte IntelliSense pro jeho obsah. Například můžete přidat soubory jQuery pro centrální seznam souborů a získáte technologie IntelliSense pro jQuery funkce v jakékoli bloku JavaScript na souboru, zda jste ji výslovně odkazována (pomocí / / / / / &lt;odkaz /&gt;) nebo ne.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS Editor

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Snižte automatické dokončování příkazů

Seznam technologie IntelliSense pro hodnoty vybrané schéma a šablon stylů CSS nyní filtry na základě vlastností šablon stylů CSS.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

Technologie IntelliSense také podporuje případu hledání názvu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Hierarchické odsazení

CSS editor používá odsazení k zobrazení hierarchické pravidla, která poskytuje přehled o logicky uspořádání šablony pravidla. V následujícím příkladu #list selektor je kaskádové podřízený seznam a proto odsazena.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

Následující příklad ukazuje mnohem složitější dědičnost:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Odsazení pravidlo je dáno jeho nadřazené pravidla. Hierarchické odsazení je ve výchozím nastavení povolené, ale lze je vypnout dialogové okno Možnosti (nástroje, možnosti na řádku nabídek):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>Změní podporu šablon stylů CSS

Analýzy stovky skutečné soubory šablon stylů CSS ukazuje, že jsou velmi běžné hackatony šablon stylů CSS a Visual Studio teď podporuje ty nejpoužívanější. Tato podpora zahrnuje funkce IntelliSense a ověřování na hvězdičku (\*) a podtržítko (\_) hackatony vlastnost:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Typické selektor hackatony jsou podporovány také tak, aby hierarchické odsazení se zachová, i když tato nastavení použijí. Typické selektor hack používá k cílové aplikaci Internet Explorer 7, je předřaďte selektor s  *\*: první podřízený + html*. Pomocí tohoto pravidla budou udržovat hierarchické odsazení:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Schémata pro konkrétní dodavatele (- moz-- webkit)

CSS3 přináší mnoho vlastností, které byly implementovány podle různých prohlížečích v různých časech. To se dříve vynutit vývojářům umožňuje psát kód pro konkrétní prohlížeče pomocí syntaxe specifické pro výrobce. Tyto vlastnosti specifické pro prohlížeče jsou teď součástí technologie IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Podpora přidávání poznámek a odstraňuje se komentování

Teď můžete komentovat a zrušte komentář u pravidla šablon stylů CSS pomocí stejné klávesové zkratky, které používáte v editoru kódu (Ctrl + K, C na komentář a Ctrl + K, můžete odkomentovat).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Výběr barvy

V předchozích verzích sady Visual Studio technologie IntelliSense pro atributy vztahující se barva se skládal z rozevíracího seznamu hodnot pojmenované barvy. Tento seznam se nahradil ovládacího prvku pro výběr barvy plně funkční.

Pokud zadáte hodnotu barvy, výběr barvy se automaticky zobrazí a zobrazí seznam použitých barvy, za nímž následuje výchozí palety barev. Můžete vybrat barvu pomocí myši nebo klávesnice.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

V seznamu se rozšířit do dokončení barev. Nástroje pro výběr umožňuje řídit alfa kanál převedením automaticky libovolné barvy na RGBA při přesunutí posuvníku krytí:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Fragmenty kódu

Fragmenty kódu v editoru stylů CSS umožňují snadněji a rychleji vytvořit styly napříč prohlížeči. Mnoho vlastností CSS3, které vyžadují nastavení specifická pro prohlížeč teď byly vráceny na fragmenty kódu.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Fragmenty šablony stylů CSS podporují pokročilé scénáře (jako jsou dotazy na média CSS3) tak, že zadáte na – symbol (@), která zobrazuje seznam IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Když vyberete @media hodnotu a stiskněte klávesu Tab, CSS editor vloží následující fragment kódu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Stejně jako u fragmenty kódu pro kód, můžete vytvořit své vlastní fragmenty šablony stylů CSS.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Vlastní oblastí

Název oblasti kódu, které jsou už k dispozici v editoru kódu, jsou teď k dispozici pro úpravy šablon stylů CSS. To umožňuje snadno seskupit související styl bloky.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Když je sbalené oblast zobrazí název oblasti:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Inspektor stránek

Nástroj Page Inspector je nástroj, který vykreslí webovou stránku (HTML, webových formulářů, ASP.NET MVC nebo webové stránky) v integrovaném vývojovém prostředí sady Visual Studio a umožňuje prozkoumat zdrojový kód a výsledný výstup. Pro stránky ASP.NET nástroj Page Inspector vám umožní určit, jaký kód na straně serveru je tvořen kód HTML, který se zobrazí v prohlížeči.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Další informace o nástroj Page Inspector najdete v následujících kurzech:

- Použití Page Inspectoru v [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Použití Page Inspectoru v [webových formulářů ASP.NET](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Publikování

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Profily publikování

Publikování informací o pro projekty webových aplikací v sadě Visual Studio 2010, nejsou uloženy ve správě verzí a není určená pro sdílení s ostatními. V aplikaci Visual Studio 2012 Release Candidate se změnil formát profil publikování. Byly provedeny týmových artefaktů a je teď snadno využít ze sestavení založené na MSBuild. Informace o konfiguraci sestavení je v dialogovém okně Publikovat, takže můžete snadno přepínat konfigurace sestavení před publikováním.

Publikování profilů jsou uloženy ve složce PublishProfiles. Umístění složky, závisí na jaký programovací jazyk, kterou používáte:

- C#: Properties\PublishProfiles
- Visual Basic: Moje Project\PublishProfiles

Každý profil je soubor MSBuild. Během publikování, je tento soubor importovat do souboru projektu MSBuild. V sadě Visual Studio 2010, pokud chcete provést změny s procesem publikování nebo balíček, budete muset vložit vaše vlastní nastavení do souboru s názvem **ProjectName**. wpp.targets. To je podporováno, ale teď vložit vaše vlastní nastavení v profilu publikování samotný. Tímto způsobem vlastní nastavení se použije pouze pro tento profil.

Můžete teď také využívají publikační profily v MSBuild. Uděláte to tak, použijte následující příkaz při sestavování projektu:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Hodnota project.csproj je cesta k projektu a název profilu je název profilu publikování. Alternativně namísto předání názvu profilu *PublishProfile* vlastností, můžete předat úplnou cestu do profilu publikování.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>Předkompilace a merge

Visual Studio 2012 Release Candidate pro projekty webových aplikací, přidá možnost na stránce vlastností balení/publikování webu, který umožňuje předkompilovat a sloučit obsah vašeho webu při publikování nebo balíčku projektu. Pokud chcete zobrazit tyto možnosti, klikněte pravým tlačítkem na projekt v Průzkumníku řešení, zvolte Vlastnosti a pak zvolte stránce vlastností balení/publikování webu. Následující obrázek znázorňuje Precompile tuto aplikaci před publikováním možnost.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Pokud je vybraná tato možnost, sada Visual Studio předkompiluje aplikace při každém publikování nebo balíčku webové aplikace. Pokud chcete řídit, jak je předkompilovaný web nebo způsob sloučení sestavení, klikněte na tlačítko Upřesnit můžete nakonfigurovat tyto možnosti.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>Služba IIS Express

Výchozí webový server pro testování webových projektů v sadě Visual Studio je teď služba IIS Express. Vývojový Server sady Visual Studio je stále možnost pro místní webový server během vývoje, ale služba IIS Express je nyní doporučené serveru. V aplikaci Visual Studio 11 Beta používáním služby IIS Express je velmi podobný používání v aplikaci Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Právní omezení

Toto je předběžná dokumentu a může podstatně změnit před finální komerční verzi softwaru, které jsou popsány zde.

Informace obsažené v tomto dokumentu představují aktuální pohled společnosti Microsoft Corporation na tyto problémy k datu publikování. Protože Microsoft reagovat na měnící se podmínky na trhu, neměly by být vykládány jako závazek Microsoftu a společnost Microsoft nemůže zaručit přesnost jakýchkoli informací, které jsou prezentovány po provedení datu publikování.

Tento dokument White Paper se pouze k informačním účelům. MICROSOFT NEPOSKYTUJE ŽÁDNÉ ZÁRUKY, VÝSLOVNÝCH, ODVOZENÝCH NEBO ZÁKONNÝCH, INFORMACE V TOMTO DOKUMENTU.

V souladu s příslušnými zákony o autorských právech je zodpovědný uživatel. Bez omezení autorská práva, žádná část tohoto dokumentu může být reprodukovat, uložené v zavedené rozšiřován nebo jakýmkoli způsobem (electronic, mechanickým, mechanicky, záznam nebo jinak) nebo pro libovolný účel, bez výslovného písemného povolení společnosti Microsoft Corporation.

Společnost Microsoft může vlastnit patenty, patentové přihlášky, ochranné známky, autorská práva nebo další práva na duševní vlastnictví přihlášky v tomto dokumentu. Výslovně uvedeno v písemné licenční smlouvě se společností Microsoft, poskytnutím tohoto dokumentu vám není udělena licence k těmto patentům, ochranné známky, autorská práva nebo jinému duševnímu vlastnictví.

Pokud není uvedeno jinak, společnosti, organizace, produkty, názvy domén, e-mailové adresy, loga, osoby, místa a události použité v ukázkách jsou smyšlené. proto žádné jejich spojení se žádné skutečnou společností, organizací, produktu, název domény, e-mailu adresy, loga, osoby, místa nebo událostí je určena ji vyvozovat.

© 2012 Microsoft Corporation. Všechna práva vyhrazena.

Microsoft a Windows jsou registrované ochranné známky nebo ochranné známky společnosti Microsoft Corporation ve Spojených státech amerických a dalších zemích.

Názvy skutečných společností a produktů mohou být ochrannými známkami příslušných vlastníků.
