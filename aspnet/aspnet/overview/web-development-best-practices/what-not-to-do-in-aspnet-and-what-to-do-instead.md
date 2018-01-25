---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: "Postup není v technologii ASP.NET a co dělat, místo toho | Microsoft Docs"
author: tfitzmac
description: "Toto téma popisuje několik běžných chyb, které uživatelé provést v rámci webové projekty ASP.NET. Poskytuje doporučení pro co dělat, aby se zabránilo tyto commo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 829f3a024bc15bec8b60b91193ba9bca37b78009
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>Postup není v technologii ASP.NET a co dělat, místo toho
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Toto téma popisuje několik běžných chyb, které uživatelé provést v rámci webové projekty ASP.NET. Poskytuje doporučení pro co dělat, aby se zabránilo těchto běžných chyb. Je založena na [prezentace](http://vimeo.com/68390507) podle **Damianu Edwards** na konferenci Norská vývojáři.


## <a name="disclaimer"></a>Právní omezení

V tomto tématu není určen jako kompletní příručka k zajištění, že aplikace je nejbezpečnější a nejúčinnější. Potřebujete stále doporučené postupy pro zabezpečení a výkonu, které nejsou uvedené v tomto tématu. Navrhne pouze tom, jak zamezit obvyklé chyby týkající se třídy rozhraní .NET a procesy.

## <a name="overview"></a>Přehled

Toto téma obsahuje následující oddíly:

- [Dodržování standardů](#standards)

    - [Adaptéry ovládacích prvků](#adapters)
    - [Vlastnosti stylu na ovládací prvky](#styleprop)
    - [Stránky a ovládací prvek zpětných volání](#callback)
    - [Zjišťování schopností prohlížeče](#browsercap)
- [Zabezpečení](#security)

    - [Ověření žádosti](#validation)
    - [Ověřování pomocí formulářů bez souborů cookie a relace](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Středním vztahem důvěryhodnosti](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Spolehlivost a výkon](#performance)

    - [PreSendRequestHeaders a PreSendRequestContent](#presend)
    - [Události asynchronní stránky s webovými formuláři](#asyncevents)
    - [Ještě efektivněji a zapomněli pracovní](#fire)
    - [Obsah Entity žádosti](#requestentity)
    - [Metoda Response.Redirect a metody Response.End](#redirect)
    - [EnableViewState a ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Dlouhé žádosti systémem (> 110 sekund)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Dodržování standardů

<a id="adapters"></a>

### <a name="control-adapters"></a>Adaptéry ovládacích prvků

Doporučení: Přestat používat adaptéry ovládacích prvků pro adaptivního vykreslování a místo toho použít dotazy na média šablon stylů CSS a standardům HTML.

Ovládací prvky adaptéry byly zavedeny v rozhraní .NET 2.0 k vykreslení prezentace kód, který byl přizpůsobené pro prostředí a různých zařízení. Tento adaptivního vykreslování nyní můžete provést pomocí šablon stylů CSS a HTML. Měli přestat používat ovládací prvek adaptéry a převést žádné existující adaptéry a šablon stylů CSS, HTML.

Další informace najdete v tématu [dotazy na média](http://www.w3.org/TR/css3-mediaqueries/) a [postupy: Přidání mobilní stránky na vaše webové formuláře ASP.NET nebo aplikace MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Vlastnosti stylu na ovládací prvky

Doporučení: Zastavte styl hodnoty nastavení ve značce ovládacího prvku a místo toho nastavit formátování hodnoty v šablony stylů CSS.

Ovládací prvky webového serveru obsahovat desítek vlastností, které můžete použít k nastavení vlastnosti stylu v řádku. Například vlastnost ForeColor nastaví barvu textu pro ovládací prvek. Můžete provést tento stejného efektu efektivněji prostřednictvím šablony stylů CSS. Předlohy se styly umožňují centralizovat styl hodnoty a vyhnout se nastavení tyto hodnoty v celé vaší aplikaci.

Následující příklad ukazuje třídu CSS nastaví text, který se red.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

Další příklad ukazuje, jak dynamicky použít třídu CSS.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Stránky a ovládací prvek zpětných volání

Doporučení: Přestat používat stránky a ovládací prvek zpětná volání a místo toho použít některou z následujících: AJAX, UpdatePanel, metody akce MVC, webového rozhraní API nebo SignalR.

V předchozích verzích technologie ASP.NET metody zpětného volání stránky a ovládací prvek umožňují část webové stránky aktualizovat bez obnovení celou stránku. Nyní můžete provést částečné aktualizace stránky prostřednictvím [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [webového rozhraní API](../../../web-api/index.md) nebo [SignalR](../../../signalr/index.md). Byste měli zastavit pomocí metody zpětného volání, protože se může způsobit problémy s přátelské adresy URL a směrování. Ve výchozím nastavení ovládací prvky nepovolujte metody zpětného volání, ale pokud jste povolili tato funkce v ovládacím prvku, měli byste zakázat ho.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Zjišťování schopností prohlížeče

Doporučení: Přestat používat zjišťování schopností statické prohlížeče a místo toho použít dynamické funkce zjišťování.

V předchozích verzích technologie ASP.NET podporované funkce pro každý prohlížeč byly uloženy v souboru XML. Zjišťuje podpora funkce prostřednictvím statických vyhledávání není nejlepším postupem. Nyní můžete dynamicky zjistit pomocí funkce zjišťování rozhraní, jako je funkce podporované prohlížeče [Modernizr](http://modernizr.com/). Funkce zjišťování Určuje podporu tak, že pokus o použití metody nebo vlastnosti a potom zkontrolujete, pokud v prohlížeči vytváří požadovaný výsledek. Ve výchozím nastavení Modernizr je součástí šablony webové aplikace.

<a id="security"></a>

## <a name="security"></a>Zabezpečení

<a id="validation"></a>

### <a name="request-validation"></a>Ověření žádosti

Doporučení: Ověření vstupu uživatele a kódování výstup od uživatelů.

Ověření žádosti je funkce technologie ASP.NET, která kontroluje každý požadavek a zastaví požadavku, pokud je nalezena zjištěné hrozby. Nezávisí na ověření žádosti pro zabezpečení aplikace před útoky skriptování mezi weby. Místo toho ověřte veškerý vstup od uživatelů a kódování výstup. V některých omezených případech můžete použití regulárních výrazů pro ověření vstupu, ale v složitější případech by měl ověřit vstup uživatele s použitím rozhraní .NET třídy, které určí, zda hodnota odpovídá povolené hodnoty.

Následující příklad ukazuje, jak používat statickou metodu v třídě Uri k určení, zda je platný identifikátor Uri zadané uživatelem.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Ale dostatečně ověření identifikátor Uri, byste taky měli zkontrolovat a ujistěte se, určuje `http` nebo `https`. Následující příklad používá instanci metody ověření, že je platný identifikátor Uri.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Před vykreslování uživatelský vstup ve formátu HTML nebo v dotazu SQL včetně uživatelský vstup, zakódujte hodnoty tak, aby zajistěte, aby byl škodlivý kód není zahrnutý.

Můžete HTML kódování hodnota ve značkách s &lt;%: %&gt; syntaxe, jak je uvedeno níže.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

Nebo v syntaxi Razor můžete HTML kódování s @, jak je uvedeno níže.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

Další příklad ukazuje způsob do formátu HTML kódování hodnotu v kódu.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Ke kódování bezpečně hodnotu pro příkazy SQL, použijte parametry příkazu, jako [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Ověřování pomocí formulářů bez souborů cookie a relace

Doporučení: Vyžadovat soubory cookie.

Předávání informací o ověřování v řetězci dotazu není zabezpečený. Pokud vaše aplikace obsahuje ověřování proto vyžadovat soubory cookie. Pokud váš soubor cookie obsahuje citlivé informace, zvažte vyžadování protokolu SSL pro soubor cookie.

Následující příklad ukazuje, jak k určení v souboru Web.config, že ověřování pomocí formulářů vyžaduje souboru cookie, který se přenášejí přes protokol SSL.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Doporučení: Nikdy nastaven na hodnotu false.

Ve výchozím nastavení, EnbableViewStateMac nastavena na hodnotu true. I když se aplikace nepoužívá stav zobrazení, nenastavujte EnableViewStateMac na hodnotu false. Nastavení této hodnoty na hodnotu false bude ohrožení aplikace skriptování mezi servery.

Spouštění pomocí technologie ASP.NET 4.5.2, modul runtime vynucuje **EnableViewStateMac = true**. I v případě, že je nastavena na hodnotu false, modul runtime ignoruje tuto hodnotu a pokračuje v hodnotu nastavenou na hodnotu true. Další informace najdete v tématu [ASP.NET 4.5.2 a EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

Následující příklad ukazuje, jak nastavit EnableViewStateMac na hodnotu true. Nemusíte ve skutečnosti, nastavte hodnotu na hodnotu true, protože je ve výchozím nastavení hodnotu true. Ale pokud nastavíte ho na hodnotu false na libovolné stránce v aplikaci, musíte okamžitě opravit tuto hodnotu.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Středním vztahem důvěryhodnosti

Doporučení: Nezávisí na střední důvěryhodnosti (nebo jiné úroveň důvěryhodnosti) jako hranice zabezpečení.

Částečná důvěryhodnost adekvátní nechrání vaší aplikace a by se neměla používat. Místo toho použijte úplný vztah důvěryhodnosti a izolovat nedůvěryhodné aplikace v samostatných fondech aplikací. Také spouštět každý fond aplikací v rámci jedinečnou identitu. Další informace najdete v tématu [ASP.NET částečné důvěryhodnosti nezaručuje izolace aplikací](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Doporučení: Nezakazujte nastavení zabezpečení v &lt;appSettings&gt; elementu.

AppSettings element obsahuje hodně hodnot, které jsou požadovány pro aktualizace zabezpečení. Nesmí změnit nebo zakázat tyto hodnoty. Pokud při nasazení aktualizace, je nutné zakázat tyto hodnoty, okamžitě znovu povolte po dokončení nasazení.

Podrobnosti najdete v tématu [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Doporučení: Použijte [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) místo.

Metoda UrlPathEncode byla přidána do rozhraní .NET Framework, chcete-li vyřešit potíže s kompatibilitou velmi konkrétní prohlížeče. Neprovádí adekvátní kódování adresu URL a nechrání aplikace z skriptování mezi servery. Nikdy používejte ji v aplikaci. Místo toho použijte [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

Následující příklad ukazuje, jak předat kódovaného adresu URL jako parametr řetězce dotazu pro ovládacího prvku hypertextový odkaz.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Spolehlivost a výkon

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders a PreSendRequestContent

Doporučení: Nepoužívejte tyto události s spravované moduly. Místo toho zápisu nativní modul služby IIS k provedení požadované úlohy. V tématu [vytváření modulů HTTP v nativním kódu](https://msdn.microsoft.com/library/ms693629.aspx).

Můžete použít [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) a [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) události s nativní moduly služby IIS.
> [!WARNING]
> Nepoužívejte `PreSendRequestHeaders` a `PreSendRequestContent` s spravované moduly, které implementují `IHttpModule`. Nastavení těchto vlastností může způsobit problémy s asynchronní požadavky. Kombinace požadovaný směrování aplikace (ARR) a websockets může vést k výjimkám porušení přístupu, které můžou způsobit w3wp chyby. Například iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 v iiscore.dll způsobila výjimku porušení přístupu (0xC0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Události asynchronní stránky s webovými formuláři

Doporučení: V webových formulářů, vyhněte se asynchronní zápis void metod pro události životního cyklu stránky a použijte [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) pro asynchronní kód.

Pokud označíte událostí stránky s **asynchronní** a **void**, nelze určit dokončení asynchronní kódu. Místo toho použijte Page.RegisterAsyncTask spustit kód asynchronní způsobem, který umožňuje sledovat její dokončení.

Následující příklad ukazuje a tlačítko klikněte na tlačítko obslužná rutina, která obsahuje asynchronní kód. Tento příklad obsahuje čtení řetězcovou hodnotu asynchronně, který je k dispozici pouze jako zjednodušenou příklad asynchronní úlohu a ne jako doporučený postup.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Pokud používáte asynchronních úloh, nastavte Http runtime cílový framework 4.5 v souboru Web.config. Nastavení rozhraní target framework na oplátku 4.5 na nový kontext synchronizace, přidala se v rozhraní .NET 4.5. Tato hodnota je nastavena ve výchozím nastavení v nové projekty v sadě Visual Studio 2012, ale je, není možné nastavit při práci s existujícího projektu.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Ještě efektivněji a zapomněli pracovní

Doporučení: Při zpracování požadavku v prostředí ASP.NET, vyhněte se spouští pracovní ještě efektivněji a zapomněli (takové volání metody ThreadPool.QueueUserWorkItem nebo vytvoření časovače, která opakovaně volá delegáta).

Pokud aplikace obsahuje pracovní ještě efektivněji a zapomněli, který běží v prostředí ASP.NET, vaše aplikace mohou získat synchronizován. V každém okamžiku může být zničený doména aplikace, to znamená, že probíhající proces už neodpovídá aktuální stav aplikace.

Měli byste přejít tento typ práce mimo prostředí ASP.NET. Můžete použít webové úlohy, služba systému Windows nebo role pracovního procesu v Azure pro probíhající práci a spusťte tento kód z jiného procesu.

Pokud je třeba provést činnost v prostředí ASP.NET, můžete přidat balíček Nuget názvem [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) ke spuštění kódu.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Obsah Entity žádosti

Doporučení: Vyhněte se čtení Request.Form nebo Request.InputStream před spuštění obslužné rutiny událostí.

Nejdřívější, které byste si měli přečíst z Request.Form nebo Request.InputStream je během této obslužné rutiny spustit událostí. V MVC Kontroleru je obslužná rutina a spouštění událostí je při spuštění metody akce. V webových formulářů stránka je obslužná rutina a spouštění událostí je při aktivuje událost Page.Init. Pokud číst obsah entity žádosti starší než spouštění událostí, narušovat zpracování požadavku.

Pokud je třeba číst obsah entity žádosti před událostí spouštění, použijte buď [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) nebo [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). Při použití GetBufferlessInputStream získat nezpracovaná datový proud z požadavku a převzít odpovědnost za celý požadavek na zpracování. Po volání GetBufferlessInputStream, Request.Form a Request.InputStream nejsou k dispozici, protože nebyly vyplněna technologií ASP.NET. Při použití GetBufferedInputStream získat kopii datového proudu z požadavku. Request.Form a Request.InputStream jsou stále k dispozici novější v žádosti, protože ASP.NET naplní jiné kopie.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Metoda Response.Redirect a metody Response.End

Doporučení: Znát rozdíly ve zpracování vláken po volání [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) metoda volá metodu metody Response.End. Probíhající synchronní volání Request.Redirect způsobí, že aktuální vlákno k okamžitě přerušení. Ale v asynchronního procesu, volání Response.Redirect není přerušit aktuální vlákno, pokračuje v provádění kódu pro daný požadavek. V asynchronní proces musí vrátit úlohu z metody zastavit provádění kódu.

V projektu MVC nesmí volání Response.Redirect. Místo toho vrátí RedirectResult.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState a ViewStateMode

Doporučení: ViewStateMode použijte místo EnableViewState poskytnout přesná kontrola nad niž používat ovládací prvky zobrazení stavu.

Když nastavíte na hodnotu false v direktivě stránky EnableViewState, stav zobrazení je zakázán pro všechny ovládací prvky v rámci dané stránky a nelze povolit. Pokud chcete povolit stav zobrazení pro jenom některé ovládací prvky na stránce, nastavte pro stránku ViewStateMode na zakázáno.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Potom nastavte ViewStateMode na povoleno pouze ovládací prvky, které skutečně potřebují stav zobrazení.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Povolením stav zobrazení pro ovládací prvky, které je třeba ji můžete zmenšit velikost stav zobrazení pro webové stránky.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Doporučení: Použijte Universal Providers.

V aktuální šablony projektů SqlMembershipProvider nahradila [balíčku ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), která je k dispozici jako balíčku NuGet. Pokud používáte SqlMembershipProvider v projektu, který byl sestaven s dřívější verzi šablony, můžete přepnout do Universal Providers. Universal Providers pracovat se všechny databáze, které podporuje rozhraní Entity Framework.

Další informace najdete v tématu [představení balíčku ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Dlouhodobé požadavky (> 110 sekund)

Doporučení: Použijte [Websocket](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) nebo [SignalR](../../../signalr/index.md) pro připojené klienty a použití asynchronní vstupně-výstupních operací.

Dlouhodobé požadavky může způsobit nepředvídatelné výsledky a nízký výkon ve webové aplikaci. Výchozí nastavení časového limitu pro žádost je 110 sekund. Pokud používáte stav relace se dlouho běžící žádostí, ASP.NET se uvolní zámek na objektu Session po 110 sekund. Aplikace však může být uprostřed operace na objektu relace, pokud se uvolní zámek, operace se nemusí dokončit úspěšně. Pokud druhý žádost od uživatele je blokovaný, při prvním požadavku, může druhá žádost o přístup k objektu Session v nekonzistentním stavu.

Pokud vaše aplikace obsahuje blokování (nebo synchronní) vstupně-výstupních operací, aplikace bude reagovat.

Chcete-li zvýšit výkon, použijte asynchronní vstupně-výstupních operací v rozhraní .NET Framework. Technologie WebSockets nebo SignalR taky použijte pro připojení klientů k serveru. Tyto funkce jsou navrženy pro efektivní zpracování dlouho běžící požadavků.
