---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: ASP.NET a Web Tools 2012.2 poznámky k verzi | Dokumentace Microsoftu
author: rick-anderson
description: Zpráva k vydání verze pro ASP.NET and Web Tools 2012.2
ms.author: riande
ms.date: 02/14/2013
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: 50e96251f8add00f70193977e73f9af194571c49
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756730"
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>Zpráva k vydání verze technologie ASP.NET and Web Tools 2012.2
====================
> Tento dokument popisuje verzi technologie ASP.NET and Web Tools 2012.2. Je aktualizace Visual Studio webové nástroje a technologie ASP.NET.


- [Poznámky k instalaci](#_Installation)
- [Dokumentace](#_Documentation)
- [Podpora](#_Support)
- [Požadavky na software](#_Software_Requirements)
- [Novinky v ASP.NET and Web Tools 2012.2](#_New_Features_in)

    - [Nástroje](#_Tooling)
    - [Publikování na webu](#_Web_Publishing)
    - [Šablony ASP.NET MVC](#_Templates)
    - [Webové rozhraní API v ASP.NET](#_ASP.NET_Web_API)

    - [Funkce SignalR technologie ASP.NET](#_ASP.NET_SignalR)
    - [ASP.NET přátelské adresy URL](#_ASP.NET_Friendly_URLs)
- [Známé problémy a změny způsobující chyby](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Poznámky k instalaci

ASP.NET and Web Tools 2012.2 pro sadu Visual Studio 2012 můžete nainstalovat s použitím [instalačního programu webové platformy](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Toto je aktualizace pro sadu Visual Studio 2012 nebo Visual Studio Express 2012 pro Web, který se vyžaduje. Pokud nemáte nainstalovanou sadu Visual Studio, nainstaluje se Visual Studio Express 2012 pro Web.

Můžete také nainstalovat ASP.NET and Web Tools 2012.2 ručně. Musíte mít Visual Studio 2012 nebo Visual Studio Express 2012 pro Web nainstalovat. Potom použijte následující pokyny: 

1. Stáhněte si [technologie ASP.NET a Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) instalačního programu ze služby Stažení softwaru.
2. Když výzvami klepnutím na tlačítko spustit. Můžete také uložit soubor spustit později.
3. Zkontrolujte verzi sady Visual Studio budete aktualizovat. Můžete to provést spuštěním chcete provést aktualizaci sady Visual Studio. Pak klikněte na položku nabídky Nápověda.   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. Pokud se zobrazí položka nabídky &quot;o Microsoft Visual Studio 2012 pro Web&quot; pak si stáhnout [Web Developer Tools 2012.2 – Visual Studio Express 2012 pro Web](https://go.microsoft.com/fwlink/?LinkID=282228). Jinak stáhněte [Web Developer Tools 2012.2 – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Když výzvami klepnutím na tlačítko spustit. Můžete také uložit soubor spustit později.

> [!NOTE]
> Verze technologie ASP.NET and Web Tools 2012.2 nezahrnuje SQL Server Data Tools. SQL Server a Windows Azure SQL Database poskytuje širší nabídku sad databázové nástroje, včetně offline podporou projektu vývoje, porovnání schématu a možnosti nasazení rozšířené databáze. Další informace nebo informace o instalaci systému SQL Server Data Tools najdete [ https://go.microsoft.com/fwlink/?LinkID=237127 ](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Dokumentace

Kurzy a další informace o ASP.NET and Web Tools 2012.2 jsou k dispozici na webu technologie ASP.NET ( https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Podpora

ASP.NET and Web Tools 2012.2 oficiálně všeobecně dostupné a podporované. Můžete použít normální podpora kanálu. Je také možné poslat otázky na fóra ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), kde jsou často schopni poskytovat podporu neformální členové komunity technologie ASP.NET.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Požadavky na software

ASP.NET and Web Tools 2012.2 vyžaduje Visual Studio 2012 nebo Visual Studio Express 2012 pro Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Novinky v ASP.NET and Web Tools 2012.2

Tato část popisuje funkce, které byly zavedeny v ASP.NET and Web Tools 2012.2 verzi.

<a id="_Tooling"></a>
### <a name="tooling"></a>Nástroje

- Inspektor stránek 

    - Podpora jazyka JavaScript výběr mapování povolení nástroje Page Inspector k mapování položek, které byly dynamicky přidány na stránku zpět na odpovídající kód jazyka JavaScript.
    - Umožňuje zobrazit aktualizace šablon stylů CSS v reálném čase.
    - Další informace najdete v článku [Automatická synchronizace šablon stylů CSS a JavaScript výběr mapování v nástroje Page Inspector](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Podpora pro CoffeeScript, Mustache, Handlebars a JsRender zvýraznění syntaxe.
    - HTML editor poskytuje Intellisense pro Knockout vazby.
    - MENŠÍ úpravy a kompilátoru podpory umožňuje vytvářet dynamické šablon stylů CSS pomocí menší.
    - Vložit formát JSON jako třídy rozhraní .NET. Pomocí tohoto příkazu speciální vložit JSON vložit do jazyka C# nebo VB.NET soubor kódu a sady Visual Studio automaticky generovat třídy .NET odvodit z JSON.
- Podpora Mobile Emulator přidá háky rozšíření tak, aby emulátory třetích stran je možné nainstalovat jako rozšíření VSIX. Nainstalované emulátory se zobrazí v rozevírací nabídce F5, tak, aby vývojáři můžete zobrazit náhled svých webů na různých mobilních zařízení. Další informace o tuto funkci v blogu Scotta Hanselmana na [nově zavedené integraci se sadou Visual Studio Browserstackem](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Publikování na webu

- Webové projekty teď mají stejné možnosti publikování jako projektů webových aplikací včetně publikování na Windows Azure websites.
- Selektivní publikování &#8211; pro jeden nebo více souborů můžete provádět následující akce (po publikování do koncového bodu nasazení webu): 

    - Publikujte vybrané soubory.
    - Zobrazíte rozdíl mezi místního souboru a na vzdálený soubor.
    - Aktualizujte místní soubor vzdáleného souboru nebo aktualizovat vzdálený soubor s místním souborem.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Šablony ASP.NET MVC

- Nová šablona aplikací pro Facebook usnadňuje vytváření Facebook Canvas aplikací. V několika jednoduchých kroků můžete vytvořit aplikaci pro Facebook, která získává data od přihlášeného uživatele a integruje se s jeho přáteli. Šablona obsahuje novou knihovnu, která se stará o všechny záležitosti související se sestavováním aplikací pro Facebook, včetně ověřování, oprávnění k přístupu k datům sítě Facebook a další. Další informace o použití aplikace Facebook šablony najdete v části [ https://go.microsoft.com/fwlink/?LinkID=269921 ](https://go.microsoft.com/fwlink/?LinkID=269921).
- Nová šablona MVC jednostránkové aplikace umožňuje vývojářům vytvářet interaktivní webové klientské aplikace s využitím HTML 5, CSS 3 a Oblíbené Knockout a jQuery knihoven jazyka JavaScript, nad rámec rozhraní ASP.NET Web API. Šablona obsahuje aplikaci seznamu "úkolů", která demonstruje obvyklé postupy při sestavování aplikací JavaScript HTML5 využívající rozhraní API serveru RESTful. Další informace na [ https://www.asp.net/single-page-application ](../single-page-application/index.md).
- Nyní můžete vytvořit rozšíření VSIX, který přidává nové šablony do dialogového okna Nový projekt ASP.NET MVC. Získejte informace zde: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Balíček FixedDisplayModes &#8211; šablon projektu MVC byly aktualizovány zahrnout nový balíček NuGet "FixedDisplayModes", který obsahuje alternativní řešení chyb v MVC 4. Další informace o opravě obsažené v balíčku, najdete v tomto příspěvku na blogu ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) od týmu MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>Rozhraní API pro ASP.NET Web

Rozhraní ASP.NET Web API se rozšířily o několik nových funkcí:

- ASP.NET Web API OData
- Trasování rozhraní ASP.NET Web API
- Stránka nápovědy webové rozhraní API technologie ASP.NET

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

Rozhraní ASP.NET Web API OData poskytuje flexibilitu, které potřebujete k tvorbě koncových bodů protokolu OData s formátovaným obchodní logikou libovolný zdroj dat. S ASP.NET Web API OData řízení velikosti sémantiky OData, kterou chcete zveřejnit. ASP.NET Web API OData je součástí šablony projektu ASP.NET MVC 4 a je také k dispozici z NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData v současné době podporuje následující funkce:

- Povolí sémantiku dotazu OData s použitím atributu [Queryable].
- Snadno ověření dotazů OData a omezit sadu možností podporovaných dotazů, operátory a funkce.
- Parametr vytvořit vazbu na ODataQueryOptions přímo pro získání reprezentaci strom abstraktní syntaxe dotazu, který lze potom ověří a použije rozhraní IEnumerable nebo IQueryable.
- Povolte stránkování řízené služby a nové generace stránce odkaz zadáním výsledek omezení atributu [Queryable].
- Požádat o vložených počet celkový počet odpovídajících prostředků pomocí $inlinecount.
- Ovládací prvek šíření hodnoty null.
- / Všechny operátory v $filter.
- Odvodit entity data model podle úmluvy nebo explicitně přizpůsobení modelu Entity Framework Code-First podobným způsobem.
- Odvozením z EntitySetController sady entit vystavení.
- Jednoduchý a přizpůsobitelný konvence pro vystavení navigační vlastnosti, manipulaci s odkazy a provádění akcí OData.
- Zjednodušená čínština, směrování pomocí metody rozšíření MapODataRoute.
- Podpora verzního zveřejněním více modelů EDM.
- Dokument služby a zpřístupnit $metadata tak můžete vygenerovat klientů (.NET, Windows Phone, Windows Store atd.) pro vaše webové rozhraní API.
- Podpora pro podrobné formáty OData Atom, JSON a JSON.
- Vytvoření, aktualizace, částečně aktualizovat (opravu) a odstranění entit.
- Dotazování a manipulaci s relací mezi entitami.
- Vytvořte vztah odkazy, které propojí až trasy.
- Komplexní typy.
- Typ dědičnosti entit.
- Vlastnosti kolekce.
- Výčty.
- Akce OData.
- Postavené na stejném základu jako služeb WCF Data Services, a to ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Další informace o ASP.NET Web API OData najdete v části [ https://go.microsoft.com/fwlink/?LinkId=271141 ](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>Trasování rozhraní ASP.NET Web API

Trasování rozhraní ASP.NET Web API integruje data trasování z vašeho webového rozhraní API .NET trasování. Nyní je povolen ve výchozím nastavení v šabloně projektu webového rozhraní API. Sledování dat pro vaše webového rozhraní API je odeslán do okna výstup a je k dispozici pomocí nástroje IntelliTrace. Trasování rozhraní ASP.NET Web API umožňuje informace trasování o webové rozhraní API hostovaná na Windows Azure díky integraci se sadou [Windows Azure Diagnostics](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Můžete také nainstalovat a povolit trasování rozhraní ASP.NET Web API v libovolné aplikaci pomocí balíčku NuGet trasování ASP.NET Web API ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Další informace o konfiguraci a použití trasování rozhraní ASP.NET Web API najdete v části [ https://go.microsoft.com/fwlink/?LinkID=269874 ](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>Stránka nápovědy webové rozhraní API technologie ASP.NET

Technologie ASP.NET webové stránce nápovědy k API je nyní zahrnutá ve výchozím nastavení v šabloně projektu webového rozhraní API. Technologie ASP.NET webové stránce nápovědy k API automaticky generuje dokumentaci pro webová rozhraní API, včetně koncové body HTTP, podporovaných metod HTTP, parametry a datové části zprávy příklad požadavku a odpovědi. Dokumentace ke službě se automaticky použije komentáře v kódu. Technologie ASP.NET webové stránce nápovědy k API můžete také přidat do jakékoli aplikace s použitím balíčku ASP.NET Web API pomáhají stránka NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Další informace o vytváření a přizpůsobení viz stránka nápovědy k ASP.NET Web API [ https://go.microsoft.com/fwlink/?LinkId=271140 ](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

Funkce SignalR technologie ASP.NET umožňuje snadno přidat funkce webu v reálném čase pro aplikace ASP.NET používá objekty Websocket, pokud je k dispozici a automaticky použije se další postupy při není.

Další informace o použití funkce SignalR technologie ASP.NET naleznete v tématu [ https://go.microsoft.com/fwlink/?LinkId=271271 ](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET přátelské adresy URL

ASP.NET FriendlyURLs umožňuje velmi snadno pro webové vývojáře formuláře ke generování adresy URL čisticí vyhledávání (bez přípony .aspx). Není nutné málo o nic konfigurovat a je možné se stávajícími aplikacemi ASP.NET v4.0. Funkce FriendlyURLs také usnadňuje vývojářům přidat podporu mobilních do svých aplikací díky podpoře přepínání mezi zobrazeními desktop a mobile.

Další informace o instalaci a používání přátelské adresy URL technologie ASP.NET naleznete v tématu [ http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx ](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a změny způsobující chyby

Tato část popisuje známé problémy a velké změny, které jsou ve verzi ASP.NET and Web Tools 2012.2.

### <a name="installation-issues"></a>Potíže s instalací

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Mimo pořadí instalací sady Visual Studio 2012

Instalace další SKU pro Visual Studio 2012 po instalaci technologie ASP.NET and Web Tools 2012.2 bude vyžadovat operace opravy. Vezměte v úvahu následující posloupnost:

1. Nainstalovat Visual Studio 2012 Express pro Web
2. Instalace technologie ASP.NET and Web Tools 2012.2
3. Instalace sady Visual Studio 2012 Professional, Premium nebo Ultimate

Krok 2 pouze způsobí instalace aktualizací pro Express for Web. Chcete-li zajistit, aby obsahoval další SKU během kroku 3 nainstalovat aktualizaci je potřeba opravit ASP.NET and Web Tools 2012.2 můžete nainstalovat aktualizace pro poslední SKU nainstalovaná. To platí i pokud vrátit zpět na SKU v kroku 1 a 3.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Instalace Microsoft ASP.NET and Web Tools 2012.2 při otevření sady Visual Studio

Pokud během instalace nástroje Microsoft ASP.NET and Web Tools 2012.2 je otevřen VS, Visual Studio může skončit ve špatném stavu. Doporučuje se, že uživatelé zavřete všechny instance sady Visual Studio, než budete pokračovat v instalaci.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Ruší se instalace technologie ASP.NET and Web Tools 2012.2 uprostřed instalace

Ruší se ASP.NET and Web Tools 2012.2 ponechá nastavení uprostřed instalace sady Visual Studio ve špatném stavu. Chcete-li vyřešit tento problém, postupujte takto: 

- Přejít na Přidat nebo odebrat programy
- Odinstalujte Microsoft ASP.NET and Web Tools 2012.2, pokud jsou k dispozici.
- Znovu nainstalovat technologii Microsoft ASP.NET and Web Tools 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Po odinstalování serveru ASP.NET and Web Tools 2012.2 rozhraní ASP.NET MVC 4 nebyly nalezeny šablony a šablony webu Razor v2

Odinstalování technologie ASP.NET and Web Tools 2012.2 také odinstalovat všechny architektury ASP.NET MVC 4 a šablony webu Razor v2 ze sady Visual Studio 2012.

Alternativním řešením je opravte instalaci sady Visual Studio 2012 a znovu nainstalujte aplikaci ASP.NET MVC 4 a šablony webu Razor v2.

### <a name="tooling-issues"></a>Problémy nástroje

#### <a name="nuget-error-reported-during-project-creation"></a>NuGet Chyba hlášená během vytváření projektu

Po instalaci technologie ASP.NET and Web Tools 2012.2 může zobrazit následující chyba při vytváření projektu aplikace MVC 4

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

ASP.NET and Web Tools 2012.2 dodává NuGet 2.1 a provede upgrade rozšíření v sadě Visual Studio 2012. V některých případech se nepodaří správně aktualizovat VSIX instalátor VSIX. Následující postup vám umožní k vyřešení tohoto problému:

1. Jako správce spusťte Visual Studio 2012
2. Přejděte na nástroje -&gt;rozšíření a aktualizace a odinstalujte NuGet.
3. Zavřete Visual Studio.
4. Přejděte do instalační složky sady ASP.NET and Web Tools 2012.2:

    1. Pro sadu Visual Studio 2012: **Program Files\Microsoft ASP.NET\ASP.NET webové Stack\Visual Studio 2012**
    2. Pro Visual Studio Express 2012 pro Web: **Program Files\Microsoft ASP.NET\ASP.NET webové Stack\Visual Studio Express 2012 pro Web**
5. Dvakrát klikněte na NuGet.Tools.vsix přeinstalovat nástroj NuGet

### <a name="web-api-issues"></a>Problémy s webových rozhraní API

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Analýza problémů $filter a literály data a času

Analyzátor identifikátoru URI protokolu OData se nepodařilo správně analyzovat literály částečná data a času. Například $filter = počáteční hodnotou datetime eq'2012-12-31T12:00' se nepodařilo správně analyzovat. Alternativní řešení, je použít úplné literálu $filter = počáteční hodnotou datetime eq'2012-12-31T12:00:00 ".

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData nepodporuje názvy vlastností velká a malá písmena.

OData nepodporuje názvy vlastností velkých a malých písmen v dotazů OData a cestu odata. Zobrazit pracovní položky:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Pokud uživatelé používají jinou velikostí písmen na javascript na straně klienta a na straně serveru, jsou pravděpodobně bude tento problém nastane. Tento problém je záměrné v protokolu odata. Mnoho uživatelů sestavy, ale tento problém. Obejít ho, uživatelé mají k opravě případům v adrese URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>OData výchozích konvencí směrování nepodporuje POST a PUT na navigační vlastnost.

OData výchozích konvencí směrování nepodporuje POST a PUT na navigační vlastnost. Zobrazit pracovní položky [ http://aspnetwebstack.codeplex.com/workitem/366 ](http://aspnetwebstack.codeplex.com/workitem/366). Tato konvence běžně používaných ve výchozích konvencí jsme nebyly nalezeny.

Vyřešíte to uživatelé potřebují k rozšíření novou konvenci směrování pro její podporu.

### <a name="facebook-template-issues"></a>Problémy se šablonou služby Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Šablona aplikací pro Facebook funguje jenom pomocí rozhraní .NET 4.5

V rozevíracím seznamu framework v dialogovém okně Nový projekt zobrazíte šablona aplikací pro Facebook v architektuře ASP.NET MVC 4 je třeba vybrat .NET 4.5.

#### <a name="real-time-update-controller"></a>V reálném čase aktualizace Kontroleru

Šablona Facebook aplikací umožňuje uživatelům snadno vytvořit Kontroleru webového rozhraní API pro zpracování v reálném čase aktualizace ze sítě Facebook. Pokud je vývojovém počítači za serverem NAT, Kontrolér nemusí fungovat bez další konfigurace sítě. Podrobnosti najdete tady: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Řetězcové hodnoty jsou v konfliktu s OAuth pro Facebook parametry dotazu

Následující pole jsou v konfliktu s OAuth pro Facebook dialogu volání zpět adresy URL. Nepřidávejte vlastní hodnoty řetězce dotazu s následujícími názvy: kód, chyba, chyba\_popis, chyba\_důvod.

#### <a name="using-page-inspector-with-facebook-template"></a>Použití Page Inspectoru pomocí šablony služby Facebook

Funkce nástroje Page Inspector nelze použít v sadě Visual Studio 2012 při ladění aplikace Facebook. Nástroj Page Inspector v současné době nepodporuje prvky IFRAME.

### <a name="single-page-application-template-issues"></a>Problémy se šablonou jednostránkové aplikace

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>S JQuery zadat při spuštění projektu výchozí jednostránková aplikace MVC, nové úpravy položek todo 1.9/Knockout 2.2.1 update události fokusu nezpracovává správně.

S JQuery 1.9/Knockout 2.2.1 aktualizací při spuštění výchozího projektu jednostránková aplikace MVC, nové úpravy položek todo, zadejte, už fokus zpět do nového pole pro úpravy položek todo po zadání nová položka seznamu úkolů.

Alternativní řešení odkazu [ http://knockoutjs.com/documentation/hasfocus-binding.html ](http://knockoutjs.com/documentation/hasfocus-binding.html)a proveďte opravu podobně jako následující ukázkový kód:

Soubor todo.model.js  
 Funkce todolist(data), přidejte následující:  
 **self.isSelected = ko.observable(false);**

Funkce todoList.prototype.addTodo, přidejte následující blacked text:  
 **self.isSelected(true);**  
 self.newTodoTitle (&quot;&quot;);

Souboru index.cshtml, přidejte následující blacked text:  
 &lt;formulář data-bind =&quot;odeslat: addTodo&quot;&gt;  
 &lt;Zadejte třídu =&quot;addTodo&quot; typ =&quot;text&quot; data-bind =&quot;hodnota: newTodoTitle, zástupný symbol: "Typ tady přidáte", blurOnEnter: true, **hasfocus: isSelected**, události: {rozostření: addTodo}&quot; /&gt;  
 &lt;formuláře&gt;
