---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: "ASP.NET a webové nástroje 2012.2 poznámky k verzi | Microsoft Docs"
author: rick-anderson
description: "Poznámky k verzi pro technologii ASP.NET a webové nástroje 2012.2."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2013
ms.topic: article
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: 52559a47f86e572f873d4eaaab50e87eb51722fd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>Technologie ASP.NET a webové nástroje 2012.2 poznámky k verzi
====================
> Tento dokument popisuje verzi technologie ASP.NET a webové nástroje 2012.2. Je aktualizace Visual Studio webových nástrojů a technologií ASP.NET.


- [Poznámky k instalaci](#_Installation)
- [Dokumentace](#_Documentation)
- [Podpora](#_Support)
- [Požadavky na software](#_Software_Requirements)
- [Nové funkce v technologii ASP.NET a nástroje pro Web 2012.2](#_New_Features_in)

    - [Nástrojů](#_Tooling)
    - [Publikování na webu](#_Web_Publishing)
    - [Šablony ASP.NET MVC](#_Templates)
    - [Webové rozhraní API v ASP.NET](#_ASP.NET_Web_API)

    - [Funkce SignalR technologie ASP.NET](#_ASP.NET_SignalR)
    - [ASP.NET přátelské adresy URL](#_ASP.NET_Friendly_URLs)
- [Známé problémy a nejnovější změny](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Poznámky k instalaci

Technologie ASP.NET a webové nástroje 2012.2 pro sadu Visual Studio 2012 můžete nainstalovat pomocí [instalačního programu webové platformy](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Toto je aktualizace Visual Studio 2012 nebo Visual Studio Express 2012 pro Web, který je vyžadován. Pokud nemáte nainstalovanou sadu Visual Studio, Visual Studio Express 2012 pro Web se nainstaluje.

Můžete také nainstalovat technologii ASP.NET a webové nástroje 2012.2 ručně. Musíte mít Visual Studio 2012 nebo Visual Studio Express 2012 pro Web nainstalovat. Potom postupujte podle následujících pokynů: 

1. Stáhněte si [ASP.NET a webové Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) instalačního programu ze služby Stažení softwaru.
2. Když výzvami klikněte na tlačítko spustit. Také můžete uložit soubor spustit později.
3. Ověřte verzi sady Visual Studio bude aktualizovat. Můžete provést spuštěním sady Visual Studio, které chcete aktualizovat. Pak klikněte na položku nabídky nápovědy.   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. Pokud se zobrazí položky nabídky &quot;o Microsoft Visual Studio 2012 pro Web&quot; pak stáhnout [Web Developer Tools 2012.2 - Visual Studio Express 2012 pro Web](https://go.microsoft.com/fwlink/?LinkID=282228). V opačném případě Stáhnout [webové Developer Tools 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Když výzvami klikněte na tlačítko spustit. Také můžete uložit soubor spustit později.

> [!NOTE]
> Verze technologie ASP.NET a webové nástroje 2012.2 nezahrnuje SQL Server Data Tools. SQL Server a databáze SQL Windows Azure poskytuje bohatší sada nástrojů, včetně offline zálohovaná projektu vývoj, porovnání schématu a databáze rozšířené možnosti nasazení databáze. Další informace nebo informace o instalaci nástroje SQL Server Data Tools navštivte [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Dokumentace

Kurzy a další informace o technologii ASP.NET a webové nástroje 2012.2 jsou k dispozici na webu technologie ASP.NET (https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Podpora

Technologie ASP.NET a webové nástroje 2012.2 je oficiálním vydání a podporována. Můžete vytvořit kanál normální podpory. Také můžete pokládat otázky ve fórech ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), kde jsou často schopen poskytnout neformální podporu členové komunity služby ASP.NET.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Požadavky na software

Technologie ASP.NET a webové nástroje 2012.2 vyžaduje Visual Studio 2012 nebo Visual Studio Express 2012 pro Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Nové funkce v technologii ASP.NET a nástroje pro Web 2012.2

Tato část popisuje funkce, které byly zavedeny v verzi technologie ASP.NET a webové nástroje 2012.2.

<a id="_Tooling"></a>
### <a name="tooling"></a>Nástrojů

- Inspektor stránek 

    - Podpora jazyka JavaScript výběr mapování povolení nástroj Page Inspector mapovat položky, které byly dynamicky přidat na stránku zpět na odpovídající kód jazyka JavaScript.
    - Umožňuje zobrazit aktualizace šablon stylů CSS v reálném čase.
    - Další informace najdete v tématu [Automatická synchronizace šablon stylů CSS a JavaScript výběr mapování v nástroj Page Inspector](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Podpora pro CoffeeScript, Mustache, Řídítka kola a JsRender zvýraznění syntaxe.
    - HTML editor poskytuje technologie Intellisense pro Knockout vazby.
    - MENŠÍ úpravy a kompilátoru podporu a povolit vytváření dynamické šablon stylů CSS pomocí menší.
    - Vložit formát JSON jako třídy rozhraní .NET. Použití tohoto příkazu speciální vložit vložit JSON do jazyka C# nebo souboru kódu VB.NET a Visual Studio automaticky vygeneruje rozhraní .NET třídy odvozené z formátu JSON.
- Podporu emulátoru Mobile přidá rozšíření háky emulátorů jiných výrobců je možné nainstalovat jako VSIX. Nainstalované emulátorů se zobrazí v rozevírací nabídce F5, aby vývojáři můžete zobrazit náhled svých webů na různých mobilních zařízení. Další informace o této funkci v položce blogu Scott Hanselman na [nové BrowserStack integrace se sadou Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Publikování na webu

- Webové projekty Teď máte stejné prostředí pro publikování jako projektů webové aplikace, včetně publikování pro weby systému Windows Azure.
- Publikování selektivní &#8211; pro jeden nebo více souborů můžete provádět následující akce (po publikování do koncového bodu Web Deploy): 

    - Publikuje vybrané soubory.
    - V tématu rozdíl mezi místního souboru a ke vzdálenému souboru.
    - Aktualizovat místní soubor s vzdáleného souboru nebo aktualizujte vzdáleného souboru místního souboru.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Šablony ASP.NET MVC

- Nová šablona aplikací pro Facebook usnadňuje vytváření Facebook Canvas aplikací. V několika jednoduchých kroků můžete vytvořit aplikaci pro Facebook, která získává data od přihlášeného uživatele a integruje se s jeho přáteli. Šablona obsahuje novou knihovnu, která se postará o všechny záležitosti související se sestavováním aplikací pro Facebook, včetně oprávnění, ověřování, přístup k datům sítě Facebook a další. Další informace o použití šablona Facebook aplikací najdete v části [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921).
- Nová šablona MVC jednostránkové aplikace umožňuje vývojářům vytvářet interaktivní webové klientské aplikace pomocí standardu HTML 5, CSS 3 a Oblíbené Knockout a knihovny jQuery jazyka JavaScript, nad rozhraní ASP.NET Web API. Šablona obsahuje "aplikaci seznamu úkolů, která demonstruje obvyklé postupy při sestavování aplikací JavaScript HTML5 využívající rozhraní API serveru RESTful. Další informace v [https://www.asp.net/single-page-application](../single-page-application/index.md).
- Nyní můžete vytvořit VSIX, který přidává nové šablony do dialogového okna Nový projekt ASP.NET MVC. Zjistěte, jak zde: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Balíček FixedDisplayModes &#8211; Šablon projektu MVC byly aktualizovány zahrnout nový balíček NuGet 'FixedDisplayModes', který obsahuje alternativní řešení pro chyby v MVC 4. Další informace o opravu obsažené v balíčku, najdete v tomto příspěvku na blogu ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) od týmu MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>Rozhraní API pro ASP.NET Web

Rozhraní ASP.NET Web API se rozšířily o několik nových funkcí:

- ASP.NET Web API OData
- Trasování rozhraní ASP.NET Web API
- Stránku nápovědy rozhraní ASP.NET Web API

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

Rozhraní ASP.NET Web API OData vám dává flexibilní potřebujete k vytváření koncových bodů protokolu OData s bohatou obchodní logiku pro libovolný zdroj dat. ASP.NET Web API OData kontrolu množství sémantiku OData, kterou chcete vystavit. ASP.NET Web API OData je součástí šablony projektu ASP.NET MVC 4 a je dostupná také prostřednictvím NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData aktuálně podporuje následující funkce:

- Povolte sémantiku dotazů OData s použitím atribut [Queryable].
- Snadno ověření dotazů OData a omezit sadu možnosti podporované dotazu, operátory a funkcí.
- Parametr vytvořit vazbu na ODataQueryOptions přímo pro získání reprezentaci abstraktního syntaktického stromu dotazu, který lze potom ověřit a použít na IQueryable nebo rozhraní IEnumerable.
- Povolte stránkování řízené služby a další generování stránky odkaz zadáním výsledek omezení pro atribut [Queryable].
- Žádost vložená počet celkový počet prostředků odpovídající pomocí $inlinecount.
- Ovládací prvek šíření hodnoty null.
- Všechny nebo všechny operátory v $filter.
- Odvození datového modelu entity podle konvence nebo explicitně přizpůsobení modelu Entity Framework Code-první podobným způsobem.
- Zveřejněte entity nastaví odvozené z EntitySetController.
- Jednoduché, přizpůsobitelné konvence pro vystavení navigační vlastnosti, manipulaci s odkazy a provádění akcí OData.
- Zjednodušený směrování pomocí metody MapODataRoute rozšíření.
- Podpora pro správu verzí díky zpřístupnění více modelů EDM.
- Vystavení dokumentu služby a $metadata, můžete vygenerovat klientů (.NET, Windows Phone, Windows Store atd.) pro Web API.
- Podpora pro podrobné formátů OData Atom, JSON a JSON.
- Vytvářet, aktualizovat, částečně aktualizovat (oprava) a odstraňovat entity.
- Dotaz a upravit relace mezi entitami.
- Vytvořte vztah odkazy, které propojit až trasy.
- Komplexní typy.
- Dědičnost typu entity.
- Vlastnosti kolekce.
- Výčty.
- Akce OData.
- Založena na stejném základu jako součásti WCF Data Services, a to ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Další informace o ASP.NET Web API OData v tématu [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>Trasování rozhraní ASP.NET Web API

Trasování rozhraní ASP.NET Web API integruje data trasování z webových rozhraní API pomocí rozhraní .NET trasování. Teď je povolené ve výchozím nastavení v šabloně projektu webového rozhraní API. Data pro váš web trasování rozhraní API je odeslán do okna výstupu a je k dispozici prostřednictvím IntelliTrace. ASP.NET Web API Tracing umožňuje informace trasování o webové rozhraní API při hostování v systému Windows Azure prostřednictvím integrace s [Windows Azure Diagnostics](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Můžete také nainstalovat a povolit ASP.NET Web API Tracing v jakékoli aplikaci pomocí balíčku NuGet trasování ASP.NET Web API ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Další informace o konfiguraci a použití technologie ASP.NET Web API Tracing v tématu [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>Stránku nápovědy rozhraní ASP.NET Web API

Ve výchozím nastavení v šabloně projektu webového rozhraní API je nyní zahrnutá ASP.NET webové rozhraní API pomůže stránky. ASP.NET webové rozhraní API pomůže stránky automaticky vygeneruje dokumentace pro webové rozhraní API včetně koncových bodů protokolu HTTP, podporovaných metod HTTP, parametry a datové části zprávy příklad žádosti a odpovědi. Dokumentace je automaticky vyžádat ze komentáře v kódu. Můžete také přidat pomůže stránku rozhraní API ASP.NET Web pro žádnou aplikaci pomocí balíčku pomoci NuGet stránky ASP.NET Web API ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Další informace o nastavení a přizpůsobení stránce nápovědy k serveru ASP.NET Web API najdete [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

Funkce SignalR technologie ASP.NET usnadňuje přidání webu v reálném čase možností do aplikace ASP.NET pomocí technologie WebSockets, pokud je k dispozici a automaticky návratem zpět k jinými technikami, když není.

Další informace o použití funkce SignalR technologie ASP.NET naleznete v části [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET přátelské adresy URL

ASP.NET FriendlyURLs lze velmi snadno pro vývojáře webového formuláře ke generování čisticí hledání adresy URL (bez příponou .aspx). To vyžaduje málo na žádná konfigurace a lze použít s existující v4.0 aplikace ASP.NET. Funkci FriendlyURLs také usnadňuje vývojářům přidat mobilní podporu do svých aplikací díky podpoře přepínání mezi zobrazeními desktop a mobile.

Další informace o instalaci a použití přátelské adresy URL technologie ASP.NET naleznete v části [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a nejnovější změny

Tato část popisuje známé problémy a dodatečné změny, které jsou ve verzi ASP.NET a webové nástroje 2012.2.

### <a name="installation-issues"></a>Potíže s instalací

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Mimo pořadí instalaci sady Visual Studio 2012

Po instalaci ASP.NET a webové nástroje 2012.2 bude vyžadovat operace opravy, instalace další SKU systému Visual Studio 2012. Vezměte v úvahu následující sekvenci:

1. Nainstalovat Visual Studio 2012 Express pro Web
2. Instalace technologie ASP.NET a webové nástroje 2012.2
3. Nainstalovat Visual Studio 2012 Professional, Premium nebo Ultimate

Krok 2 pouze povedou k instalaci aktualizací pro Express pro Web. K zajištění, že další SKU nainstalován během kroku 3 obsahuje aktualizace musíte opravit ASP.NET a webové nástroje 2012.2, k instalaci aktualizací pro poslední SKU nainstalována. To platí také pokud SKU v kroku 1 a 3 se vrátit zpět.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Instalace Microsoft ASP.NET a webové nástroje 2012.2 po otevřete Visual Studio

Pokud během instalace Microsoft ASP.NET a webové nástroje 2012.2 je otevřená VS, Visual Studio může skončit ve špatném stavu. Doporučuje se, že uživatelé zavřete všechny instance sady Visual Studio, než budete pokračovat v instalaci.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Zrušení instalace technologie ASP.NET a webové nástroje 2012.2 uprostřed instalace

Zrušení ASP.NET a webové nástroje 2012.2 ponechá instalace uprostřed instalace sady Visual Studio ve špatném stavu. Chcete-li vyřešit tento problém, postupujte takto: 

- Přejděte na Přidat nebo odebrat programy
- Odinstalujte Microsoft ASP.NET a webové nástroje 2012.2, pokud je k dispozici.
- Znovu nainstalujte technologii Microsoft ASP.NET a nástroje pro Web 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Po odinstalování technologie ASP.NET a webové nástroje 2012.2 ASP.NET MVC 4 se šablony a Razor v2 webu chybí

Odinstalování technologie ASP.NET a webové nástroje 2012.2 odinstaluje také všechny ASP.NET MVC 4 a Razor v2 webu šablony ze sady Visual Studio 2012.

Alternativní řešení je k opravě instalace sady Visual Studio 2012 a znovu nainstalujte technologii ASP.NET MVC 4 a šablony webu Razor v2.

### <a name="tooling-issues"></a>Problémy nástroje

#### <a name="nuget-error-reported-during-project-creation"></a>Chyba NuGet během vytváření projektu

Po instalaci ASP.NET a webové nástroje 2012.2 může dojít k následující chybě při vytváření projektu MVC 4

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

ASP.NET a webové nástroje 2012.2 dodává NuGet 2.1 a provede upgrade rozšíření v sadě Visual Studio 2012. V některých případech se instalační program VSIX nepodaří správně aktualizovat VSIX. Následující postup vám umožní řešení tohoto problému:

1. Spusťte sadu Visual Studio 2012 jako správce
2. Přejděte na nástroje -&gt;rozšíření a aktualizace a odinstalovat NuGet.
3. Zavřete Visual Studio.
4. Přejděte do složky instalace technologie ASP.NET a webové nástroje 2012.2:

    1. Pro sadu Visual Studio 2012: **Program Files\Microsoft ASP.NET\ASP.NET webové Stack\Visual Studio 2012**
    2. Pro Visual Studio Express 2012 pro Web: **Program Files\Microsoft ASP.NET\ASP.NET webové Stack\Visual Studio Express 2012 pro Web**
5. Dvakrát klikněte na NuGet.Tools.vsix přeinstalovat NuGet

### <a name="web-api-issues"></a>Problémy s webových rozhraní API

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Analýza problémů v $filter a literály data a času

Analyzátor identifikátoru URI protokolu OData se nepodaří správně analyzovat literály částečné data a času. Například $filter = počáteční hodnotou datetime eq'2012-12-31T12:00 se nepodaří správně analyzovat. Alternativní řešení je použití úplného literálu $filter = počáteční hodnotou datetime eq'2012-12-31T12:00:00'.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData nepodporuje názvy vlastností velká a malá písmena.

OData nepodporuje názvy vlastností velká a malá písmena v dotazů OData a cestu odata. V tématu pracovních:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Pokud uživatelé používají různé velká a malá písmena na javascript na straně klienta a na straně serveru, budou pravděpodobně bude tento problém nastane. Tento problém je v protokolu odata. Řada uživatelů sestavy však tento problém. Obejít ho, uživatelé mají k odstranění jejich případech v adrese URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Výchozí OData směrování konvence nepodporuje POST nebo PUT na navigační vlastnost.

Výchozí OData směrování konvence nepodporuje POST nebo PUT na navigační vlastnost. Najdete v části pracovní položka [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366). Chybí jsme touto konvencí běžně používané ve výchozích konvencí.

Obejít ji, musí uživatelé rozšířit nové konvencí směrování pro její podporu.

### <a name="facebook-template-issues"></a>Problémy se šablonou služby Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Šablona Facebook aplikace funguje pouze pomocí rozhraní .NET 4.5

V rozevíracím seznamu framework v dialogovém okně Nový projekt zobrazíte šablona aplikací pro Facebook v architektuře ASP.NET MVC 4 je nutné vybrat .NET 4.5.

#### <a name="real-time-update-controller"></a>Aktualizace v reálném čase řadiče

Šablona Facebook aplikací umožňuje uživateli snadno vytvořit řadič webové rozhraní API pro zpracování v reálném čase aktualizace ze sítě Facebook. Pokud je počítači pro vývoj za serverem NAT, řadiči nemusí fungovat bez další konfigurace sítě. See here for details: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Dotaz, jestli řetězcové hodnoty v konfliktu s parametry OAuth pro Facebook

Následující pole v konfliktu s OAuth pro Facebook dialogu volání zpět adresy URL. Nepřidávejte vlastní hodnoty řetězců dotazu s těmito názvy: kód, chyba, chyba\_popis, chyba\_důvod.

#### <a name="using-page-inspector-with-facebook-template"></a>Díky nástroji Page Inspector se šablonou služby Facebook

Nástroj Page Inspector funkci nelze použít v sadě Visual Studio 2012 při ladění aplikace Facebook. Nástroj Page Inspector aktuálně nepodporuje vložené rámce.

### <a name="single-page-application-template-issues"></a>Šablona jednostránkové aplikace problémy

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>S JQuery zadejte při spuštění projektu MVC SPA výchozí, nové úpravy položek todo 1.9/Knockout 2.2.1 update není správné zpracování událostí fokus.

S JQuery 1.9/Knockout 2.2.1 aktualizací, při spuštění výchozí projekt MVC SPA, nové úpravy položek todo zadejte už fokus zpět do pole edit nové položky todo po zadání nová položka todo do seznamu úkolů.

Alternativní řešení odkazy [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html)a proveďte opravu podobné následující vzorový kód:

Soubor todo.model.js  
 Funkce todolist(data), přidejte následující:  
 **self.isSelected = ko.observable(false);**

Funkce todoList.prototype.addTodo, přidejte následující blacked text:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

Soubor index.cshtml, přidejte následující blacked text:  
 &lt;vytvoří data-bind =&quot;odeslat: addTodo&quot;&gt;  
 &lt;vstup – třída =&quot;addTodo&quot; typ =&quot;text&quot; data-bind =&quot;hodnota: newTodoTitle, zástupný symbol: "Typ zde přidat", blurOnEnter: true, **hasfocus: isSelected**, události: {rozostření: addTodo}&quot; /&gt;  
 &lt;/ Form&gt;
