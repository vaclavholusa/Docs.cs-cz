---
uid: visual-studio/overview/2013/release-notes
title: Technologie ASP.NET a webové nástroje pro poznámky k verzi 2013 Visual Studio | Microsoft Docs
author: microsoft
description: Tento dokument popisuje verzi technologie ASP.NET a webové nástroje pro sadu Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: e9ddd96f186564834ff6bb2c30cf0ed5444cbf1b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877798"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>Technologie ASP.NET a webové nástroje pro poznámky k verzi 2013 Visual Studio
====================
podle [Microsoft](https://github.com/microsoft)

> Tento dokument popisuje verzi technologie ASP.NET a webové nástroje pro sadu Visual Studio 2013.


## <a name="contents"></a>Obsah

- [Poznámky k instalaci](#TOC1)
- [Dokumentace](#TOC2)
- [Požadavky na software](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nové funkce v technologii ASP.NET a nástroje Web Tools pro sadu Visual Studio 2013

- [One ASP.NET](#TOC6)
- [Nové prostředí webového projektu](#newproj)
- [ASP.NET generování uživatelského rozhraní](#scaffold)
- [Browser Link](#browser-link)
- [Vylepšení editoru webové Visual Studio](#web-editor)
- [Podpora aplikací webové služby Azure App Service v sadě Visual Studio](#waws)
- [Vylepšení publikování webu](#publish)
- [NuGet 2.7](#nuget)
- [ASP.NET – webové formuláře](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [Rozhraní ASP.NET Web API 2](#TOC11)
- [Funkce SignalR technologie ASP.NET](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Komponenty Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [Syntaxe Razor rozhraní ASP.NET 3](#TOC14)
- [Pozastavení aplikace ASP.NET](#TOC15)
- [Známé problémy a nejnovější změny](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Poznámky k instalaci

Technologie ASP.NET a webové nástroje pro sadu Visual Studio 2013 jsou seskupeny v hlavní instalační program a můžete ho stáhnout [zde](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Dokumentace

Kurzy a další informace o ASP.NET a webové nástroje pro sadu Visual Studio 2013 jsou dostupné na [webové stránky ASP.NET](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Požadavky na software

ASP.NET a nástroje pro Web vyžaduje Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nové funkce v technologii ASP.NET a nástroje Web Tools pro sadu Visual Studio 2013

Následující části popisují funkce, které byly zavedeny v verzi.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Jeden ASP.NET

S verzí sady Visual Studio 2013 jsme provedli krokem k sjednocujícího prostředí pomocí technologie ASP.NET, takže snadno můžete kombinovat a párovat ty, které chcete. Například můžete můžete spustit na projekt MVC pomocí a snadno do projektu přidejte stránky webových formulářů později nebo vygenerovat webovým rozhraním API v projektu webové formuláře. Jeden ASP.NET se točí kolem usnadnit vám jako vývojář provádět akce, která nepatří technologie ASP.NET. Bez ohledu na to, jakou technologii zvolíte může mít jistotu, který vytváříte na důvěryhodné základní architektury jeden ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nové prostředí webového projektu

Budeme mít rozšířené možnosti vytvoření nové webové projekty v sadě Visual Studio 2013. V **nový webový projekt ASP.NET** dialogovém okně můžete vybrat typ projektu, nakonfigurovat libovolnou kombinaci technologie (webových formulářů, MVC, Web API), nakonfigurujte možnosti ověřování a přidejte projektu testování částí.

![Nový projekt ASP.NET](release-notes/_static/image1.png)

Dialogové okno Nový umožňuje změnit výchozí možnosti ověřování pro řadu šablony. Například při vytváření projektu webových formulářů ASP.NET můžete vybrat některý z následujících možností:

- Bez ověřování
- Jednotlivých uživatelských účtů (členství technologie ASP.NET nebo sociální zprostředkovatele přihlášení)
- Účty organizace (Active Directory v aplikaci internet)
- Ověřování systému Windows (Active Directory v intranetu aplikace)

![Možnosti ověřování](release-notes/_static/image2.png)

Další informace o nový proces pro vytváření webových projektů najdete v tématu [vytváření webové projekty ASP.NET v sadě Visual Studio 2013](creating-web-projects-in-visual-studio.md). Další informace o nové možnosti ověřování najdete v tématu [ASP.NET Identity](#TOC8) dále v tomto dokumentu.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET generování uživatelského rozhraní

Generování uživatelského rozhraní ASP.NET je architektura generování kódu pro webové aplikace ASP.NET. Usnadňuje přidat často používaný kód do projektu, který komunikuje s datovým modelem.

V předchozích verzích sady Visual Studio generování uživatelského rozhraní omezovala na projekty ASP.NET MVC. Visual Studio 2013 můžete nyní pomocí generování uživatelského rozhraní pro všechny projekt ASP.NET, včetně webových formulářů. Visual Studio 2013 pro projekt webové formuláře aktuálně nepodporuje generování stránek, ale když můžete nadále používat generování uživatelského rozhraní s webovými formuláři tak, že přidáte do projektu závislosti MVC. Podpora pro generování stránky pro webové formuláře bude přidána v budoucí aktualizaci.

Při použití generování uživatelského rozhraní, je zajištěno, že všechny požadované závislosti jsou nainstalované v projektu. Například když začít s projektem webových formulářů ASP.NET a přidání Kontroleru webového rozhraní API pomocí generování uživatelského rozhraní, požadované balíčky NuGet a odkazy se přidají do projektu automaticky.

Chcete-li přidat generování uživatelského rozhraní MVC do projektu webových formulářů, přidejte **novou vygenerovanou položku** a vyberte **závislosti MVC 5** v dialogovém okně. Existují dvě možnosti pro generování uživatelského rozhraní MVC; Minimální a úplné. Pokud vyberete minimální, jenom balíčky NuGet a odkazy pro architekturu ASP.NET MVC se přidají do projektu. Pokud vyberete možnost Úplná minimální závislosti přidají, a také požadované soubory obsahu pro projekt MVC.

Podpora pro generování uživatelského rozhraní asynchronní řadiče využívá nové funkce asynchronní z Entity Framework 6.

Další informace a podrobné pokyny najdete v tématu [generování uživatelského rozhraní ASP.NET: Přehled](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Browser Link – SignalR kanál mezi prohlížečem a Visual Studio

Nové [Browser Link](using-browser-link.md) funkce umožňuje připojení více prohlížečů pro Visual Studio a aktualizujte je všechny kliknutím na tlačítko na panelu nástrojů. Můžete se připojit více prohlížečů na vašem vývojovém webu, včetně emulátorů a všechny prohlížeče klikněte na tlačítko Aktualizovat na aktualizovat, všechny ve stejnou dobu. Browser Link taky zpřístupňuje rozhraní API a umožňuje vývojářům psát rozšíření odkazů prohlížeče.

![](release-notes/_static/image3.png)

Když zapnete vývojáři využít rozhraní API odkazů prohlížeče, bude možné vytvořit velmi pokročilé scénáře, které protne hranice mezi Visual Studio a žádný prohlížeč, který je připojen. Web Essentials využívá rozhraní API pro vytvoření integrované možnosti mezi Visual Studio a prohlížeče nástrojů pro vývojáře, vzdáleného řízení emulátorů a mnoho dalších.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Vylepšení editoru webové Visual Studio

Visual Studio 2013 zahrnuje nové editor HTML pro syntaxi Razor soubory a soubory ve formátu HTML v webových aplikací. Nový editor HTML poskytuje jeden jednotná schéma založené na HTML5. Má závorek automatické doplňování, jQuery uživatelského rozhraní a AngularJS atributu IntelliSense, atribut seskupení IntelliSense, ID a název třídy Intellisense a dalších vylepšení, včetně lepší výkon, formátování a inteligentní značky.

Následující snímek obrazovky ukazuje, jak pomocí Bootstrap atributu IntelliSense v editoru HTML.

![IntelliSense v editoru HTML](release-notes/_static/image4.png)

Visual Studio 2013 také obsahuje oba CoffeeScript a méně editory součástí. LESS editor pochází z šablon stylů CSS editor se skvělými funkcemi a má na všech menší dokumentech v konkrétní Intellisense pro proměnné a mixins @import řetězu.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Podpora aplikací webové služby Azure App Service v sadě Visual Studio

Ve Visual Studiu 2013 se sadou Azure SDK pro .NET 2.2, můžete použít **Průzkumníka serveru** pracovat přímo s vzdálené webové aplikace. Můžete se přihlásit k účtu Azure vytvářet nové webové aplikace, konfigurace aplikace a zobrazení protokolů v reálném čase a další. Vydání Připravujeme krátce po SDK, 2.2, budete moct spustit v režimu ladění vzdáleně v Azure. Většina nových funkcí pro Azure App Service Web Apps také fungovat v sadě Visual Studio 2012, když instalujete aktuální verzi sady Azure SDK pro .NET.

Další informace naleznete v následujících materiálech:

- [Vytvoření webové aplikace ASP.NET ve službě Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Řešení potíží s webovou aplikaci v Azure App Service pomocí sady Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Vylepšení publikování webu

Visual Studio 2013 zahrnuje nových a vylepšených funkcí publikování webů. Zde najdete několik z nich:

- Snadno [automatizovat šifrování souborů Web.config](https://go.microsoft.com/fwlink/?LinkId=325529). (Tento odkaz a následující dva bod dokumentaci na webu MSDN, která nemusí být dostupná, dokud nebude pozdní za den na 10/17.)
- Snadno [automatizovat trvá aplikace do režimu offline během nasazení](https://go.microsoft.com/fwlink/?LinkId=325530).
- Konfigurace nasazení webu pro [používat kontrolní součet souboru místo datum poslední změny](https://go.microsoft.com/fwlink/?LinkId=325531) týkajících se soubory, které by se měl zkopírovat na server.
- Pokud používáte FTP nebo systém souborů publikovat metody i pomocí nástroje nasazení webu rychle publikujte jednotlivé vybrané soubory (včetně souboru Web.config).

Další informace o nasazení webu ASP.NET najdete v tématu [webu ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 zahrnuje celou řadu nových funkcí, které jsou popsány podrobně v [poznámky k verzi 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Tato verze NuGet také eliminuje nutnost zajistit výslovný souhlas pro funkci obnovení balíčků NuGet stáhnout balíčky. Souhlasu (a související políčko v dialogovém okně Předvolby NuGet) je nyní uděleno nainstalováním NuGet. Obnovení balíčku nyní jednoduše funguje ve výchozím nastavení.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>ASP.NET – webové formuláře

### <a name="one-aspnet"></a>Jeden ASP.NET

Šablony projektů webové formuláře bezproblémově integrovat nové prostředí jeden ASP.NET. Můžete přidat MVC a webového rozhraní API podporují do projektu webových formulářů a ověřování pomocí Průvodce vytvořením projektu jeden ASP.NET můžete konfigurovat. Další informace najdete v tématu [vytváření webové projekty ASP.NET v sadě Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Šablony projektů webové formuláře podporovat nové architektury ASP.NET Identity. Kromě toho šablony teď podporují vytvoření projektu intranetu webových formulářů. Další informace najdete v tématu [metody ověřování](creating-web-projects-in-visual-studio.md#auth) v **vytváření webové projekty ASP.NET v sadě Visual Studio 2013**.

### <a name="bootstrap"></a>Bootstrap

Pomocí šablony webových formulářů [Bootstrap](http://twitter.github.io/bootstrap/) k poskytování elegantní a dobře reagovaly vzhled a chování, můžete snadno přizpůsobit. Další informace najdete v tématu [Bootstrap v šablony webových projektů Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Jeden ASP.NET

Šablon projektu MVC webové bezproblémově integrovat nové prostředí jeden ASP.NET. Můžete přizpůsobit projektu MVC a nakonfigurovat ověřování pomocí Průvodce vytvořením projektu jeden ASP.NET. Úvodní kurz k ASP.NET MVC 5 naleznete na adrese [Začínáme s ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Informace týkající se upgradu projekty MVC 4 až MVC 5 najdete v tématu [postup upgradu služby ASP.NET MVC 4 a projekt webového rozhraní API ASP.NET MVC 5 a webovém rozhraní API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Šablon projektu MVC byly aktualizovány na používání technologie ASP.NET Identity pro ověřování a správu identit. Kurz poskytuje funkci ověřování sítě Facebook a Google a členství v nové rozhraní API naleznete na adrese [vytvořit aplikaci ASP.NET MVC 5 s použitím Facebook a Google OAuth2 a přihlašování OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) a [vytvoření aplikace ASP.NET MVC pomocí ověřování a Databáze SQL a nasazení do Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap

Šablona projektu MVC je aktualizovaná tak, aby použít [Bootstrap](http://getbootstrap.com/) k poskytování elegantní a dobře reagovaly vzhled a chování, můžete snadno přizpůsobit. Další informace najdete v tématu [Bootstrap v šablony webových projektů Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtry ověřování

Filtry ověřování je nový typ filtru v architektuře ASP.NET MVC, která spustí před filtry autorizace v architektuře ASP.NET MVC kanálu a umožňují určit ověřování logiku za akce, kontroleru, nebo globálně pro všechny řadiče. Filtry ověřování zpracovat přihlašovací údaje v požadavku a zadejte odpovídající objekt zabezpečení. Filtry ověřování můžete také přidat výzev ověřování v reakci na neoprávněné žádosti.

### <a name="filter-overrides"></a>Přepsání filtru

Nyní můžete přepsat filtry, které platí pro danou akci metodu nebo řadiče zadáním filtru přepsání. Zadejte filtry přepsání typy filtrů, které by neměl být spuštěn pro daný obor (akce nebo kontroleru). To umožňuje nastavit filtry, které platí globálně, ale pak vyloučit určité globální filtry z použití řadiče nebo konkrétní akce.

### <a name="attribute-routing"></a>Atribut směrování

ASP.NET MVC teď podporuje atribut směrování díky příspěvek ve Tim McCall, Autor [ http://attributerouting.net ](http://attributerouting.net). Se směrováním atributů můžete zadat trasy zadávání poznámek k akce a kontrolery.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>Rozhraní API pro ASP.NET Web 2

### <a name="attribute-routing"></a>Atribut směrování

Rozhraní ASP.NET Web API nyní podporuje atribut směrování díky příspěvek ve Tim McCall, Autor [ http://attributerouting.net ](http://attributerouting.net). Se směrováním atributů můžete zadat trasy webového rozhraní API zadávání poznámek k vaše akce a kontrolery takto:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Atribut směrování vám dává větší kontrolu nad identifikátory URI ve vašem webovém rozhraní API. Můžete například snadno definovat hierarchie prostředků pomocí jediného kontroleru rozhraní API:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Atribut směrování také nabízí pohodlný syntaxi pro zadání volitelné parametry, výchozí hodnoty a omezení trasy:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Další informace o směrováním atributů najdete v tématu [atribut směrování ve webovém rozhraní API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Šablony projektu webového rozhraní API a jednu stránku aplikaci teď podporují autorizace pomocí OAuth 2.0. OAuth 2.0 je rozhraní pro autorizaci přístupu klientů k chráněným prostředkům. Funguje pro celou řadu klientů včetně prohlížečů a mobilních zařízení.

Podpora pro OAuth 2.0 je založena na nové middleware zabezpečení poskytované komponenty Microsoft OWIN pro ověřování nosiče a implementace roli serveru autorizace. Alternativně klientů může být ověřeny pomocí serveru organizační ověřování, jako je například Azure Active Directory nebo služba AD FS ve Windows serveru 2012 R2.

### <a name="odata-improvements"></a>Vylepšení OData

**Podpora pro $select $rozšířit, $batch a $value**

ASP.NET Web API OData teď má plnou podporu pro $select, $, rozbalte položku a $value. Můžete taky $batch pro žádost o dávkové zpracování a zpracování sady změn.

$Select a $expand umožňují změnit tvar data, která je vrácena z koncový bod OData. Další informace najdete v tématu [Introducing $select a $expand rozbalte podpory v Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Vylepšené rozšíření**

Rozšiřitelné jsou nyní formátovacích modulů OData. Můžete přidat položky metadat Atom, podporují záznamy s názvem datového proudu a média odkaz, přidejte poznámky instance a přizpůsobit způsob generování odkazů.

**Typ bez podpory**

Teď můžete sestavit služby OData, aniž by bylo třeba definovat typy CLR pro vaše typy entit. Místo toho můžete řadičů OData trvat nebo vrátit instanci **IEdmObject**, které jsou formátovacích modulů OData serializace či deserializace.

**Opakované použití existujícího modelu**

Pokud již máte existující model dat entity (EDM), můžete teď znovu použít ho přímo, aniž by museli vytvářet novou. Například pokud používáte rozhraní Entity Framework, můžete EDM, který sestaví EF za vás.

### <a name="request-batching"></a>Žádosti o dávkování

Žádost o dávkování kombinuje více operací do jedné žádosti HTTP POST, omezit přenos v síti a zadejte hladší, méně chatty uživatelské rozhraní. Rozhraní ASP.NET Web API nyní podporuje několik strategií pro dávkové zpracování žádosti:

- Použijte koncový bod $batch služby OData.
- Balíček více požadavků do jedné žádosti vícedílné zprávy standardu MIME.
- Použijte vlastní dávkování formát.

Povolit žádosti o dávkování, jednoduše přidejte trasa se obslužnou rutinu dávkování webového rozhraní API konfigurace:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

Můžete taky řídit, jestli požadavky nebo provést sekvenčně a v libovolném pořadí.

### <a name="portable-aspnet-web-api-client"></a>Přenosné ASP.NET Web API klienta

Klienta ASP.NET Web API nyní můžete vytvořit knihovny tříd portable, které fungují v rámci aplikace Windows Store a Windows Phone 8. Můžete také vytvořit přenosné formátovací moduly, které můžete sdílet mezi klientem a serverem.

### <a name="improved-testability"></a>Vylepšené možnosti testování

Webové rozhraní API 2 díky je mnohem snazší jednotky testovací kontrolery vaše rozhraní API. Právě doložit vaší kontroler API s zprávu žádosti a konfigurace a potom volejte metodu akce, které chcete otestovat. Je také snadno model **UrlHelper** třídu pro metody akce, které provádějí generování odkazů.

### <a name="ihttpactionresult"></a>IHttpActionResult

Nyní můžete implementovat IHttpActionResult k zapouzdření výsledek metody akce vašeho webového rozhraní API. IHttpActionResult vrátila z metody akce webového rozhraní API se spustí modulem runtime ASP.NET Web API pro vytvoření zprávy výsledné odpovědi. IHttpActionResult lze vrátit ze všech akcí webového rozhraní API pro zjednodušení jednotky testování vaší implementace webového rozhraní API. Pro usnadnění práce, počet IHttpActionResult implementace jsou uvedeny předinstalované včetně výsledky pro vrácení určitých stavových kódů ve formátu obsahu nebo vyjednal obsahu odpovědi.

### <a name="httprequestcontext"></a>HttpRequestContext

Nové **HttpRequestContext** sleduje nějaký stav, který je vázaný na žádost, ale nejsou ihned k dispozici z požadavku. Například můžete použít **HttpRequestContext** získat data trasy, která objekt přidružený k požadavku na certifikát klienta, **UrlHelper** a kořen virtuální cesty. Můžete snadno vytvořit **HttpRequestContext** účely testování jednotky.

Protože objekt zabezpečení pro daný požadavek je plynoucích s požadavkem, aniž byste museli spoléhat na **Thread.CurrentPrincipal**, objekt je nyní k dispozici po celou dobu požadavku zůstane v kanálu webového rozhraní API.

### <a name="cors"></a>CORS

Díky jiné skvělý příspěvek ze společnosti Brock Allen ASP.NET nyní plně podporuje křížové požadavku sdílení zdroji (CORS).

Zabezpečení prohlížeče brání provedení požadavky AJAX do jiné domény na webové stránce. [CORS](http://www.w3.org/TR/cors/) je standard W3C, který umožňuje serveru zmírnit zásady stejného původu. Pomocí CORS, server explicitně povolit některé žádostí napříč zdroji při odmítnutí ostatní.

Webové rozhraní API 2 teď podporuje CORS, včetně automatického zpracování předběžných požadavků. Další informace najdete v tématu [povolení žádostí napříč zdroji v rozhraní ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtry ověřování

Filtry ověřování je nový typ filtru v rozhraní ASP.NET Web API, která spustí před filtry autorizace v rozhraní ASP.NET Web API kanálu a umožňují určit ověřování logiku za akce, kontroleru, nebo globálně pro všechny řadiče. Filtry ověřování zpracovat přihlašovací údaje v požadavku a zadejte odpovídající objekt zabezpečení. Filtry ověřování můžete také přidat výzev ověřování v reakci na neoprávněné žádosti.

### <a name="filter-overrides"></a>Přepsání filtru

Nyní můžete přepsat filtry, které platí pro danou akci metodu nebo řadiče, zadáním filtru přepsání. Zadejte filtry přepsání typy filtrů, které by neměla běžet pro daný obor (akce nebo kontroleru). To umožňuje přidat globální filtry, ale pak vyloučit některé z řadiče nebo konkrétní akce.

### <a name="owin-integration"></a>Integrace OWIN

Rozhraní ASP.NET Web API nyní plně podporuje OWIN a dá se provozovat na jakéhokoli hostitele podporující OWIN. Další součástí je **HostAuthenticationFilter** integrace se sadou ověřování OWIN, který poskytuje.

Díky integraci OWIN můžete vlastní hostování webového rozhraní API v vlastní procesu spolu s další middleware OWIN, jako je například SignalR. Další informace najdete v tématu [OWIN použijte k Self-Host ASP.NET Web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>Funkce SignalR technologie ASP.NET 2.0

Následující části popisují funkce SignalR 2.0.

- [Založený na OWIN](#builtonowin)
- [MapHubs a MapConnection jsou nyní MapSignalR](#MapSignalR)
- [Cross-Domain Support](#crossdomain)
- [iOS a Android, podporují prostřednictvím MonoTouch a MonoDroid](#mobile)
- [Klient přenosné .NET](#portable)
- [Nový balíček pro hostování na vlastním serveru](#selfhost)
- [Podpora serveru zpětně kompatibilní](#backwardcompat)
- [Odebrat server podpory pro rozhraní .NET 4.0](#remove40)
- [Odesílání zprávy na seznam klientů a skupin](#messagelist)
- [Odesílání zprávy pro konkrétního uživatele](#sendtouser)
- [Lepší podporu zpracování chyb](#errorhandling)
- [Jednodušší jednotky testování rozbočovače](#unittesting)
- [Zpracování chyb jazyka JavaScript](#javascripterror)

Příklad upgradu existujícího projektu 1.x SignalR 2.0, naleznete v části [upgrade SignalR 1.x projektu](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Založený na OWIN

SignalR 2.0 je založený na úplně [OWIN (otevřít webové rozhraní pro rozhraní .NET)](http://owin.org/). Tato změna zajistí procesu instalace pro SignalR mnohem víc konzistenci mezi hostované webové a vlastním hostováním aplikací SignalR, ale také vyžaduje počet změn, rozhraní API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs a MapConnection jsou nyní MapSignalR

Pro kompatibilitu s normami OWIN, byla přejmenovaná tyto metody k `MapSignalR`. `MapSignalR` Voláno bez parametrů se namapuje všechny rozbočovače (jako `MapHubs` nemá ve verzi 1.x); k mapování jednotlivých **připojení PersistentConnection** objekty, zadejte jako parametr typu a přípona adresy URL pro připojení jako typ připojení první argument.

`MapSignalR` Metoda je volána v třídy pro spuštění Owin. Visual Studio 2013 obsahuje novou šablonu pro třídy pro spuštění Owin; pro použití této šablony, postupujte takto:

1. Klikněte pravým tlačítkem na projekt
2. Vyberte **přidat**, **novou položku...**
3. Vyberte **třídy pro spuštění Owin**. Pojmenujte novou třídu **Startup.cs**.

V **webovou aplikaci,** Owin při spuštění třída obsahující `MapSignalR` metoda se pak přidá do procesu spouštění na Owin pomocí položky v uzlu nastavení aplikace v souboru Web.Config, jak je uvedeno níže.

V **samoobslužně hostované aplikace**, třída při spuštění se předá jako parametr typu `WebApp.Start` metoda.

**Mapování rozbočovače a připojení v systému SignalR 1.x (ze souboru globální aplikací ve webové aplikaci):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Mapování rozbočovače a připojení v SignalR 2.0 (ze souboru třída Owin při spuštění):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

V **samoobslužně hostované aplikace**, třída při spuštění se předá jako parametr typu pro `WebApp.Start` metoda, jak je uvedeno níže.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Cross-Domain Support

V systému SignalR 1.x napříč požadavky domény kontrolovala jeden příznak EnableCrossDomain. Tento příznak řídí požadavky JSONP a CORS. Pro větší flexibilitu, podpora všechny CORS byla odebrána z komponenty serveru SignalR (JavaScript lients i nadále používat CORS normálně když se zjistí, zda prohlížeč podporuje), a nové middlewaru OWIN, který má je k dispozici pro tyto scénáře podporovat.

V systému SignalR 2.0, pokud JSONP je v klientovi požadováno (pro podporu požadavků napříč doménami v prohlížečích starší), ji budou muset být explicitně povoleno nastavením `EnableJSONP` na `HubConfiguration` do objektu `true`, jak je uvedeno níže. JSONP je standardně zakázána, protože je to méně bezpečné než CORS.

Chcete-li přidat nový middleware CORS do SignalR 2.0, přidejte `Microsoft.Owin.Cors` knihovna k projektu a volání `UseCors` před SignalR middleware, jak je znázorněno v následující části.

**Přidání do projektu Microsoft.Owin.Cors**: K instalaci této knihovny, spusťte následující příkaz v konzole Správce balíčků:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Tento příkaz přidá 2.0.0 verze balíčku do projektu.

**Volání metody UseCors**

Následující fragmenty kódu ukazují, jak implementovat připojení mezi doménami v systému SignalR 1.x a 2.0.

**Implementace žádosti napříč doménami v systému SignalR 1.x (ze souboru globální aplikace)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementace žádosti napříč doménami v rámci SignalR 2.0 (ze souboru kódu C#)**

Následující kód ukazuje, jak povolit CORS a JSONP v projektu SignalR 2.0. Tento příklad používá `Map` a `RunSignalR` místo `MapSignalR`, aby se spouštěla CORS middleware jenom pro SignalR požadavky, které vyžadují podporu CORS (nikoli pro všechny přenosy v zadané cestě v `MapSignalR`.) `Map` lze také použít pro veškerý middleware, který je potřeba spustit pro konkrétní předponu adresy URL, nikoli pro celou aplikaci.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>iOS a Android, podporují prostřednictvím MonoTouch a MonoDroid

Byla přidána podpora pro iOS a Android klienty, kteří používají MonoTouch a MonoDroid součásti z [Xamarin knihovny](https://xamarin.com/). Další informace o tom, jak je používat najdete v tématu [pomocí součásti Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Tyto součásti bude k dispozici v [Xamarin Storu](https://store.xamarin.com/) při vydání SignalR RTW je k dispozici.

<a id="portable"></a> ### Přenosný klient .NET

Pro lepší usnadňují vývoj pro různé platformy, Silverlight, WinRT a klienti Windows Phone nahradil jeden přenosný klient .NET, který podporuje tyto platformy:

- NET 4.5
- Silverlight 5
- WinRT (.NET pro aplikace Windows Store)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nový balíček pro hostování na vlastním serveru

Nyní je k balíčku NuGet, která usnadňují začátek práce s SignalR hostování na vlastním serveru (SignalR aplikace, které jsou hostované v procesu nebo jiná aplikace, nikoli hostované na webovém serveru). Upgrade projektu hostování na vlastním vytvořené s SignalR 1.x, odebrat balíček Microsoft.AspNet.SignalR.Owin a přidejte balíček Microsoft.AspNet.SignalR.SelfHost. Další informace o Začínáme s balíčkem hostování na vlastním serveru najdete v tématu [kurz: SignalR hostování na vlastním](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Podpora serveru zpětně kompatibilní

V předchozích verzích nástroje SignalR, verze balíčku SignalR v klientovi a serveru musela být identické. Za účelem podpory silná klientských aplikací, které by bylo obtížné aktualizovat, SignalR 2.0 teď podporuje pomocí staršího klienta na novější verzi serveru. **Poznámka: SignalR 2.0 nepodporuje serverů se staršími verzemi vytvořené s nástroji novějších klientů.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Odebrat server podpory pro rozhraní .NET 4.0

SignalR 2.0 klesla podpory interoperability serveru pomocí rozhraní .NET 4.0. Rozhraní .NET 4.5, musí být použit s servery SignalR 2.0. Je stále klienta rozhraní .NET 4.0 pro SignalR 2.0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Odesílání zprávy na seznam klientů a skupin

V systému SignalR 2.0 je možné odeslat zprávu pomocí seznamu klienta a ID skupin. Následující fragmenty kódu ukazují, jak to udělat.

**Odesílání zprávy na seznam klientů a skupin pomocí připojení PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Odesílání zprávy na seznam klientů a skupin pomocí centra**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Odesílání zprávy pro konkrétního uživatele

Tato funkce umožňuje uživatelům určit, co je ID uživatele podle IRequest prostřednictvím nové rozhraní IUserIdProvider:

**Rozhraní IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Ve výchozím nastavení bude implementace, která používá IPrincipal.Identity.Name uživatele jako uživatelské jméno.

V rozbočovače budete moct odesílat zprávy pro tyto uživatele pomocí nového rozhraní API:

**Pomocí Clients.User rozhraní API**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Lepší podporu zpracování chyb

Uživatelé nyní můžete vyvolat **HubException** ze všech volání rozbočovače. Konstruktoru **HubException** může trvat zprávu řetězec a objekt dodatečné údaje o chybě. SignalR bude automaticky serializovat výjimku a odešlete ji do klienta kde se bude používat k odmítnutí nebo selhání volání metody rozbočovače.

**Zobrazit podrobné rozbočovače výjimky** nastavení nemá žádný vliv na **HubException** odesílány zpět do klienta, nebo ne; vždy odesláním.

**Kódu na straně serveru demonstraci odesílání HubException do klienta**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Kód jazyka JavaScript klienta demonstraci neodpovídá na požadavky HubException odeslaných ze serveru**

[!code-html[Main](release-notes/samples/sample16.html)]

**Ukázka neodpovídá na požadavky HubException odeslaných ze serveru kód klienta rozhraní .NET**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Jednodušší jednotky testování rozbočovače

SignalR 2.0 obsahuje rozhraní nazvané `IHubCallerConnectionContext` na rozbočovačů, které usnadňuje vytvoření imitované klienta straně volání. Následující fragmenty kódu ukazují toto rozhraní pomocí Oblíbené testovací nevodivou [xUnit.net](https://github.com/xunit/xunit) a [moq](https://code.google.com/p/moq/).

**Testování částí SignalR pomocí xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Testování částí SignalR pomocí moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Zpracování chyb jazyka JavaScript

Všechny zpětná volání jazyka JavaScript zpracování chyb v SignalR 2.0 vrátit JavaScript – chyby objekty namísto nezpracovaná řetězců. To umožňuje SignalR přenést bohatší informace do vašeho chyba obslužné rutiny. Můžete získat v popisu vnitřní výjimky z `source` vlastnost chyby.

**Kód JavaScript klienta, který zpracovává Start.Fail výjimky**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Nový systém členství technologie ASP.NET

ASP.NET Identity je nový systém členství pro aplikace ASP.NET. ASP.NET Identity lze snadno integrovat data profilu uživatelská data aplikací. ASP.NET Identity můžete také vyberte model trvalosti pro profily uživatelů ve vaší aplikaci. Data můžete uložit do databáze systému SQL Server nebo jiného úložiště dat, včetně úložiště dat NoSQL, jako je například úložiště tabulek Azure. Další informace najdete v tématu [jednotlivé uživatelské účty](creating-web-projects-in-visual-studio.md#indauth) v **vytváření webové projekty ASP.NET v sadě Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Ověřování na základě deklarací

ASP.NET teď podporuje ověřování na základě deklarace identity, kde je identita uživatele reprezentován jako sadu deklarací identity z důvěryhodných vystavitelů. Uživatelé mohou být ověřené pomocí uživatelského jména a hesla udržované v databázi aplikace, nebo pomocí poskytovatelů identit sociálních (například: Accounts Microsoft, Facebook, Google, Twitter), nebo pomocí účty organizace prostřednictvím Azure Active Directory nebo Služba Active Directory Federation Services (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integrace s Azure Active Directory a Windows Server Active Directory

Nyní můžete vytvořit projekty ASP.NET, které používají Azure Active Directory nebo Windows Server Active Directory (AD) pro ověřování. Další informace najdete v tématu [účty organizace](creating-web-projects-in-visual-studio.md#orgauth) v **vytváření webové projekty ASP.NET v sadě Visual Studio 2013**.

### <a name="owin-integration"></a>Integrace OWIN

Ověřování pomocí technologie ASP.NET je nyní podle middleware OWIN, který lze použít na všechny hostitele na základě OWIN. Další informace o rozhraní OWIN, viz následující [komponenty Microsoft OWIN](#TOC7) části.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Komponenty Microsoft OWIN

[Otevřete Web Interface pro .NET](http://owin.org/) (OWIN) definuje abstrakci mezi .NET webové servery a webové aplikace. OWIN oddělí webové aplikace ze serveru, provedení hostitele vázané na webové aplikace. Například můžete hostitele na základě OWIN webovou aplikaci ve službě IIS nebo vlastní hostování v vlastní proces.

Změny uváděné ve komponenty Microsoft OWIN (také označované jako projekt Katana) zahrnují nové komponenty serveru a hostitele, nové pomocné knihovny a middleware a nové middleware ověřování.

Další informace o OWIN a Katana najdete v tématu [co je nového v OWIN a Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Poznámka: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) aplikace nemohou být spuštěny v klasickém režimu služby IIS; musí být spuštěn v integrovaném režimu.**

**Poznámka: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) aplikace musí být spuštěny v režimu plné důvěryhodnosti.**

### <a name="new-servers-and-hosts"></a>Nové servery a hostitele

V této verzi se nebyly přidány nové komponenty povolte scénáře hostování na vlastním serveru. Tyto součásti zahrnují následující balíčky NuGet:

- **Microsoft.Owin.Host.HttpListener**. Poskytuje serveru OWIN, který používá **HttpListener** naslouchat požadavkům HTTP a směrovat je do kanálu OWIN.
- **Microsoft.Owin.Hosting** poskytuje knihovnu pro vývojáře, kteří chtějí hostování na vlastním kanál OWIN v vlastní proces, jako je konzolová aplikace nebo služba systému Windows.
- **OwinHost**. Poskytuje samostatné spustitelný soubor, který zabalí `Microsoft.Owin.Hosting` a umožňuje hostování na vlastním kanálu OWIN bez nutnosti psaní vlastních hostitelskou aplikaci.

Kromě toho `Microsoft.Owin.Host.SystemWeb` balíček teď umožňuje middleware zajistit pomocné parametry **SystemWeb** serveru, která určuje, že by měla být volána middleware během konkrétní fáze kanálu ASP.NET. Tato funkce je obzvláště užitečná pro middleware ověřování, který se má spustit již v rané fázi v kanálu ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Pomocné knihovny a middlewaru.

I když můžete napsat OWIN komponent pomocí pouze funkce a typ definice z specifikace OWIN nové `Microsoft.Owin` balíček poskytuje sadu abstrakce přívětivější. Tento balíček kombinuje několik starších balíčky (například `Owin.Extensions`, `Owin.Types`) do jedné, dobře strukturovaný objekt modelu, který je pak snadno použít ostatními komponentami OWIN. Ve skutečnosti většina komponenty Microsoft OWIN teď tento balíček použít.

> [!NOTE]
> [OWIN](http://www.owin.org) aplikace nemohou být spuštěny v klasickém režimu služby IIS; musí být spuštěn v integrovaném režimu.

> [!NOTE]
> [OWIN](http://www.owin.org) aplikace musí být spuštěny v režimu plné důvěryhodnosti.

Tato verze rovněž obsahuje balíček Microsoft.Owin.Diagnostics, která zahrnuje ověření spuštěné aplikace OWIN middleware plus chybovou stránku middleware pomohou prozkoumat selhání.

### <a name="authentication-components"></a>Součásti ověření

Následující součásti ověřování jsou k dispozici.

- **Microsoft.Owin.Security.ActiveDirectory**. Povoluje ověřování v místní nebo cloudové adresářové služby.
- **Microsoft.Owin.Security.Cookies** umožňuje ověřování pomocí souborů cookie. Tento balíček byl dříve s názvem `Microsoft.Owin.Security.Forms`.
- **Microsoft.Owin.Security.Facebook** umožňuje ověřování pomocí služby na základě OAuth pro Facebook.
- **Microsoft.Owin.Security.Google** umožňuje ověřování pomocí služby Google OpenID založené.
- **Microsoft.Owin.Security.Jwt** umožňuje ověřování pomocí tokenů JWT.
- **Microsoft.Owin.Security.MicrosoftAccount** ověřování umožňuje použití účtů Microsoft.
- **Microsoft.Owin.Security.OAuth**. Poskytuje serveru ověřování OAuth, jakož i middleware pro ověřování tokenů nosiče.
- **Microsoft.Owin.Security.Twitter** umožňuje ověřování pomocí služby na základě OAuth pro Twitter.

Tato verze rovněž obsahuje `Microsoft.Owin.Cors` balíček, který obsahuje middleware pro zpracování požadavků HTTP mezi zdroji.

> [!NOTE]
> Odebrala se podpora pro podepisování tokenů JWT ve finální verzi Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Seznam nových funkcí a jiné změny ve Entity Framework 6 najdete v tématu [Entity Framework verze historie](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>Syntaxe Razor rozhraní ASP.NET 3

ASP.NET Razor 3 zahrnuje následující nové funkce:

- Podpora pro úpravy kartě. Preivously **formátovat dokument** příkaz, automaticky odsazení a automatické formátování v sadě Visual Studio nebude fungovat správně při použití **zachovat karty** možnost. Tato změna opraví formátování pro kódu Razor pro karta formátování sady Visual Studio.
- Podporu pravidel přepisování adres URL při generování odkazů.
- Odebrání transparentní atributu zabezpečení.
  > [!NOTE]
  > Toto je narušující změně a umožňuje Razor 3 nekompatibilní s MVC4 a starší, zatímco Razor 2 není kompatibilní s MVC5 nebo sestavení zkompilovaných proti MVC5.

Chyby syntaxe Razor 3 v sadě Visual Studio 2013 z předběžných verzí naleznete [zde](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>Pozastavení aplikace ASP.NET

Pozastavit aplikace ASP.NET je funkce Změna herní v rozhraní .NET Framework 4.5.1, které výrazně mění uživatelské rozhraní a hospodářského model pro hostování velkého počtu ASP.NET lokalit na jednom počítači. Další informace najdete v tématu [pozastavit aplikace ASP.NET – přizpůsobivý sdílené hostování webů .NET](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a nejnovější změny

Tato část popisuje známé problémy a nejnovějších změn v ASP.NET a webové nástroje pro sadu Visual Studio 2013.

### <a name="nuget"></a>NuGet

- [Nové obnovení balíčků nefunguje na Mono při použití souboru SLN –](https://nuget.codeplex.com/workitem/3596) – problém bude vyřešený v nadcházející nuget.exe stahování a [NuGet.CommandLine balíček](http://www.nuget.org/packages/NuGet.CommandLine/) aktualizovat.
- [Nové obnovení balíčků nefunguje s projekty Wix](https://nuget.codeplex.com/workitem/3598) – problém bude vyřešený v nadcházející nuget.exe stahování a [NuGet.CommandLine balíček](http://www.nuget.org/packages/NuGet.CommandLine/) aktualizovat.
- [Automatické obnovení balíčků nefunguje pro projekty v řešení složce](https://nuget.codeplex.com/workitem/3625) – problém bude vyřešený v NuGet 2.8.

### <a name="aspnet-web-api"></a>Rozhraní API pro ASP.NET Web

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` nevrací `IQueryable<T>` vždy, protože jsme doplnili podporu pro `$select` a `$expand`.

    Naše starší ukázky pro `ODataQueryOptions<T>` vždy převedena návratový z hodnoty `ApplyTo` k `IQueryable<T>`. To fungovala předtím, protože dotaz možnostech, které jsme podporováno dříve (`$filter`, `$orderby`, `$skip`, `$top`) neměňte tvaru dotazu. Teď, když podporujeme `$select` a `$expand` návratovou hodnotou z `ApplyTo` nebude `IQueryable<T>` vždy.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Pokud používáte ukázkový kód z dříve, pokud klient neodesílá bude pokračovat pracovní `$select` a `$expand`. Ale pokud chcete podporovat `$select` a `$expand` máte k tomuto tento kód změnit.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url nebo RequestContext.Url má hodnotu null při dávkového požadavku**

    Ve scénáři dávkování **UrlHelper** má hodnotu null, pokud k němu přistupovat z **Request.Url** nebo **RequestContext.Url**.

    Tento problém aktuálně sledovat tady: [BatchRequestContext.Url má hodnotu null pro dávkové zpracování požadavku](http://aspnetwebstack.codeplex.com/workitem/1301).

    Alternativní řešení tohoto problému je vytvoření nové instance **UrlHelper**, jako v následujícím příkladu:

    **Vytvoření nové instance UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Při použití MVC5 a OrgAuth, pokud máte zobrazení, které provést ověření AntiForgerToken, můžete narazit na k následující chybě při odesílání dat na zobrazení:

    **Chyba**:

    *Chyba serveru v aplikaci '/'.*

    <em>Deklarace typu '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'nebo'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' nebyl v zadané ClaimsIdentity. Povolení podpory token proti padělání s ověřováním na základě deklarace, zkontrolujte, zda zprostředkovatele nakonfigurované deklarací identity je zajištění obě tyto deklarace na ClaimsIdentity instance, které generuje. Pokud na jiný typ deklarací zprostředkovatele deklarací identity nakonfigurované místo toho používá jako jedinečný identifikátor, lze nakonfigurovat pomocí statické vlastnosti AntiForgeryConfig.UniqueClaimTypeIdentifier.</em>

    **Alternativní řešení**:

    V souboru Global.asax jeho řešení přidejte následující řádek:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Tento problém bude vyřešený na další vydání.
2. Po upgradu aplikace MVC4 na MVC5, sestavte řešení a spusťte jej. Byste měli vidět následující chybě:

    [A] Nelze přetypovat System.Web.WebPages.Razor.Configuration.HostSection [B]System.Web.WebPages.Razor.Configuration.HostSection. Typu A pochází z ' System.Web.WebPages.Razor, verze = 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' v kontextu "Default" v umístění ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ V4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'. Typ B pochází z ' System.Web.WebPages.Razor, verze = 3.0.0.0, jazykovou verzi = neutral, PublicKeyToken = 31bf3856ad364e35' v kontextu "Default" v umístění ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\ e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.

    Chcete-li opravit výše uvedené chyby, otevřete *všechny* (včetně těch v zobrazení složky) souborů Web.config v projektu a postupujte takto:

   1. Aktualizujte všechny výskyty verze "4.0.0.0" "System.Web.Mvc" na "5.0.0.0".
   2. Aktualizovat všechny výskyty verzi "System.Web.Helpers", "2.0.0.0" &quot;System.Web.WebPages&quot; a &quot;System.Web.WebPages.Razor&quot; k "3.0.0.0"

      Například po provedení výše změny vazby sestavení by měl vypadat takto:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Informace týkající se upgradu projekty MVC 4 až MVC 5 najdete v tématu [postup upgradu služby ASP.NET MVC 4 a projekt webového rozhraní API ASP.NET MVC 5 a webovém rozhraní API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Při použití ověřování na straně klienta s jQuery Nerušivý ověření, zobrazí se zpráva ověření je někdy nesprávný pro element HTML input s typem = "číslo". Chyba ověřování je pro požadovaná hodnota ("stáří pole je povinné") se zobrazí kdy je zadáno neplatné číslo místo správné zpráva, že je požadováno platné číslo.

    Tento problém je často najít s automaticky generovaný kód pro model se ve vlastnosti integer. v zobrazení vytvořit a upravit.

    Chcete-li tento problém obejít, změňte pomocný editor z:

    `@Html.EditorFor(person => person.Age)`

    Do:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 nepodporuje částečnou důvěryhodností. Odstraňte projekty propojení s MVC nebo WebAPI binární soubory [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) atribut a [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) atribut. Odebrat tyto atributy se eliminovat chyby kompilátoru podobně jako tento.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Všimněte si, jako jste to vy vedlejším účinkem nelze použít 4.0 a 5.0 sestavení ve stejné aplikaci. Je potřeba aktualizovat všechny z nich k 5.0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>SPA šablonu s Facebook autorizace může způsobit nestabilitu v aplikaci Internet Explorer, zatímco je hostovaná na webu do zóny sítě intranet

Šablona SPA poskytuje externí přihlášení pomocí Facebooku. Když je vytvořen pomocí šablony projektu běží místně, přihlášení může způsobit IE došlo k chybě.

Řešení:

1. Hostování webu v Internetové zóně; nebo

2. V prohlížeči než IE otestování tohoto scénáře.

### <a name="web-forms-scaffolding"></a>Webové formuláře generování uživatelského rozhraní

Webové formuláře generování uživatelského rozhraní byla odebrána z VS2013 a bude k dispozici v budoucí aktualizaci na Visual Studio. Generování uživatelského rozhraní v rámci projektu webové formuláře však můžete použít přidáním závislosti MVC a generování generování uživatelského rozhraní pro MVC. Projekt bude obsahovat kombinaci webových formulářů a MVC.

Pokud chcete přidat do projektu webové formuláře MVC, přidat novou položku vygenerované a vyberte **závislosti MVC 5**. Vyberte minimální nebo úplné v závislosti na tom, jestli potřebujete všechny soubory obsahu, jako třeba skripty. Potom přidáte vygenerované položky pro MVC, která vytvoří zobrazení a kontroler ve vašem projektu.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC a webových rozhraní API generování uživatelského rozhraní – chyby HTTP 404, nebyla nalezena chyba

Pokud dojde k chybě při přidání vygenerované položky do projektu, je možné, že projekt bude ponechána v nekonzistentním stavu. Některé změny provedené být generování uživatelského rozhraní se vrátí zpátky, ale ostatní změny, jako je například nainstalované balíčky NuGet, nebude vrácena zpět. Směrování změny konfigurace se vrátí zpátky, uživatelé obdrží chybu HTTP 404 při navigaci na generované uživatelské rozhraní položky.

Alternativní řešení:

- Chcete-li vyřešit tuto chybu pro MVC, přidat novou položku vygenerované a vyberte závislosti MVC 5 (minimální nebo úplná). Tento proces bude do projektu přidejte všechny požadované změny.
- Odstranění této chyby pro webového rozhraní API:

  1. Do projektu přidejte třídy WebApiConfig.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Konfigurace WebApiConfig.Register v aplikaci\_Start – metoda v souboru Global.asax následujícím způsobem:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
