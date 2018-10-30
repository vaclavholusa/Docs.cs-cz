---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET and Web Tools pro Visual Studio 2013 – poznámky k | Dokumentace Microsoftu
author: microsoft
description: Tento dokument popisuje verzi technologie ASP.NET and Web Tools pro Visual Studio 2013.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 43878bc101ef97e8bbb6c150f4125707da7660c9
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244954"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET and Web Tools pro Visual Studio 2013 – poznámky k
====================
podle [Microsoft](https://github.com/microsoft)

> Tento dokument popisuje verzi technologie ASP.NET and Web Tools pro Visual Studio 2013.


## <a name="contents"></a>Obsah

- [Poznámky k instalaci](#TOC1)
- [Dokumentace](#TOC2)
- [Požadavky na software](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Novinky v ASP.NET and Web Tools pro Visual Studio 2013

- [One ASP.NET](#TOC6)
- [Nové prostředí webového projektu](#newproj)
- [ASP.NET generování uživatelského rozhraní](#scaffold)
- [Browser Link](#browser-link)
- [Vylepšení webového editoru sady Visual Studio](#web-editor)
- [Podpora aplikací webové služby Azure App Service v sadě Visual Studio](#waws)
- [Vylepšení publikování webu](#publish)
- [NuGet 2.7](#nuget)
- [Webové formuláře ASP.NET](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [Rozhraní ASP.NET Web API 2](#TOC11)
- [Funkce SignalR technologie ASP.NET](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Komponenty Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [Syntaxe Razor rozhraní ASP.NET 3](#TOC14)
- [Pozastavení aplikace technologie ASP.NET](#TOC15)
- [Známé problémy a změny způsobující chyby](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Poznámky k instalaci

ASP.NET and Web Tools pro Visual Studio 2013 jsou spojeny v instalačním programu pro hlavní a je možné stáhnout [tady](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Dokumentace

Kurzy a další informace o ASP.NET and Web Tools pro Visual Studio 2013 jsou k dispozici [Web ASP.NET s](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Požadavky na software

ASP.NET and Web Tools vyžaduje Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Novinky v ASP.NET and Web Tools pro Visual Studio 2013

Následující části popisují funkce, které byly zavedeny ve vydané verzi.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>One ASP.NET

S vydáním sady Visual Studio 2013 jsme udělali krok k zabezpečení sjednocujícího prostředí pomocí technologie ASP.NET, takže můžete snadno kombinovat a párovat ty, které chcete. Například můžete spustit projekt s použitím MVC a snadno přidat stránky webových formulářů do projektu později nebo generování uživatelského rozhraní webových rozhraní API v projektu webových formulářů. One ASP.NET se točí kolem usnadnit vám jako vývojáři k provádění akcí, které jste si oblíbili v technologii ASP.NET. Bez ohledu na to, jakou technologii zvolíte budete moci spolehnout, že vytváříte na důvěryhodné základní architektury One ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nové prostředí webového projektu

Vylepšili jsme možnosti vytváření nových webových projektů v sadě Visual Studio 2013. V **nový webový projekt ASP.NET** dialogového okna můžete vybrat typ projektu chcete, nakonfigurovat libovolnou kombinaci technologie (webové formuláře, MVC, webové rozhraní API), nakonfigurujte možnosti ověřování a přidejte projekt testu jednotek.

![Nový projekt ASP.NET](release-notes/_static/image1.png)

Nové dialogové okno umožňuje změnit výchozí nastavení ověřování pro celou řadu šablon. Například při vytváření projektu aplikace webových formulářů ASP.NET můžete vybrat některý z následujících možností:

- Bez ověřování
- Jednotlivých uživatelských účtů (členství technologie ASP.NET nebo sociální zprostředkovatele přihlášení)
- Účty organizace (Active Directory v aplikaci internet)
- Ověřování Windows (Active Directory v intranetu aplikace)

![Možnosti ověřování](release-notes/_static/image2.png)

Další informace o nový proces pro vytváření webových projektů, naleznete v tématu [vytváření webových projektů ASP.NET v sadě Visual Studio 2013](creating-web-projects-in-visual-studio.md). Další informace o nových možnostech ověřování najdete v tématu [ASP.NET Identity](#TOC8) dále v tomto dokumentu.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET generování uživatelského rozhraní

ASP.NET generování uživatelského rozhraní je architektura generování kódu pro webové aplikace ASP.NET. To umožňuje snadno přidat do projektu, který komunikuje s datovým modelem často používaný kód.

V předchozích verzích sady Visual Studio generování uživatelského rozhraní omezovala na projekty ASP.NET MVC. Pomocí sady Visual Studio 2013 můžete nyní používat generování uživatelského rozhraní pro libovolný projekt ASP.NET, včetně webových formulářů. Visual Studio 2013 pro projekt webových formulářů aktuálně nepodporuje generování stránek, ale můžete pořád používat generování uživatelského rozhraní s webovými formuláři tak, že přidáte k projektu závislosti MVC. Podpora pro generování stránky pro webové formuláře bude přidána v budoucí aktualizaci.

Při používání generování uživatelského rozhraní, zajišťujeme, že všechny požadované závislosti jsou nainstalovány v projektu. Například pokud začínat projekt webových formulářů ASP.NET a poté použijte generování uživatelského rozhraní pro přidání Kontroleru webového rozhraní API, požadované balíčky NuGet a odkazy jsou přidány do projektu automaticky.

Chcete-li přidat generování uživatelského rozhraní MVC do projektu webových formulářů, přidejte **novou vygenerovanou položku** a vyberte **závislosti MVC 5** v dialogovém okně. Existují dvě možnosti pro generování uživatelského rozhraní MVC; Minimální a úplné. Pokud vyberete minimální, pouze balíčky NuGet a odkazy pro architekturu ASP.NET MVC se přidají do vašeho projektu. Pokud vyberete možnost Úplná minimální závislosti jsou přidány, a také požadované soubory obsahu pro projekt MVC.

Podpora pro generování kontrolerů asynchronní používá nové asynchronní funkce z Entity Framework 6.

Další informace a podrobné pokyny najdete v tématu [generování uživatelského rozhraní ASP.NET: Přehled](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Browser Link – SignalR kanál mezi prohlížečem a sady Visual Studio

Nové [Browser Link](using-browser-link.md) funkce vám umožní připojení více prohlížečů k sadě Visual Studio a aktualizovat všechny kliknutím na tlačítko na panelu nástrojů. Můžete připojit více prohlížečů na váš web development, včetně mobilních emulátorů a klikněte na aktualizovat na Aktualizovat všechny prohlížeče všechny ve stejnou dobu. Browser Link také poskytuje rozhraní API umožňuje vývojářům umožňuje psát rozšíření odkazů prohlížeče.

![](release-notes/_static/image3.png)

Tím, že umožňuje vývojářům využívat rozhraní API odkazů prohlížeče, bude možné vytvořit velmi pokročilé scénáře, které překročí hranice mezi Visual Studio a v jakémkoli prohlížeči, který je připojený. Web Essentials využívá rozhraní API k vytvoření integrovaného prostředí mezi Visual Studio a prohlížeči vývojářských nástrojů, vzdálené řízení mobilních emulátorů a mnoho dalších.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Vylepšení webového editoru sady Visual Studio

Visual Studio 2013 obsahuje nové editoru HTML Razor soubory a soubory HTML ve webových aplikacích. Nový editor HTML obsahuje jedno jednotné schéma založených na HTML5. Má uživatelské rozhraní jQuery automatické uzavírání závorek a AngularJS atribut technologie IntelliSense, atribut seskupení technologie IntelliSense, ID a název třídy technologie Intellisense a dalších vylepšení včetně lepší výkon, formátování a inteligentní značky.

Následující snímek obrazovky ukazuje, jak pomocí Bootstrap atribut technologie IntelliSense v editoru jazyka HTML.

![Technologie IntelliSense v editoru HTML](release-notes/_static/image4.png)

Visual Studio 2013 také dodává s oběma CoffeeScript a méně editory součástí. LESS editor pochází z šablon stylů CSS editor se skvělými funkcemi a má napříč všemi méně dokumenty v technologii Intellisense pro konkrétní proměnné a objekty mixin @import řetězce.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Podpora aplikací webové služby Azure App Service v sadě Visual Studio

Ve Visual Studiu 2013 pomocí sady Azure SDK for .NET 2.2, můžete použít **Průzkumníka serveru** pracovat přímo s vzdálené webové aplikace. Můžete se přihlásit ke svému účtu Azure vytvářet nové webové aplikace, konfigurace aplikací, zobrazit protokoly v reálném čase a další. Vydáno odesílaných krátce po SDK 2.2, budete moct spustit v režimu ladění vzdáleně v Azure. Většina nových funkcí pro Azure App Service Web Apps pracovat i v sadě Visual Studio 2012 při instalaci v aktuální verzi sady Azure SDK pro .NET.

Další informace naleznete v následujících materiálech:

- [Vytvoření webové aplikace ASP.NET ve službě Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Řešení potíží s webovou aplikací ve službě Azure App Service pomocí sady Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Vylepšení publikování webu

Visual Studio 2013 obsahuje nové a vylepšené funkce publikování webu. Tady je několik z nich:

- Snadno [automatizovat šifrování souborů Web.config](https://go.microsoft.com/fwlink/?LinkId=325529). (Tento odkaz a následující dva přejděte na dokumentaci na webu MSDN, které nemusí být k dispozici, dokud v den v 10/17.)
- Snadno [automatizovat pořizování aplikace v režimu offline během nasazování](https://go.microsoft.com/fwlink/?LinkId=325530).
- Nakonfigurujete nástroj nasazení webu na [nahrazujícím datum poslední změny kontrolního součtu souboru](https://go.microsoft.com/fwlink/?LinkId=325531) pro zjištění souborů, které mají být zkopírovány do serveru.
- Pokud používáte FTP nebo systému souborů publikovat metody i s nasazením webu rychle publikujte jednotlivé vybrané soubory (včetně souboru Web.config).

Další informace o nasazení webu ASP.NET najdete v tématu [webu ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 zahrnuje bohatou sadu nových funkcí, které jsou popsány podrobně v [zpráva k vydání verze NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Tato verze NuGet také odstraňuje nutnost poskytovat explicitní souhlas pro funkce obnovení balíčků NuGet stáhnout balíčky. Souhlas (a související zaškrtávací políčko v dialogovém okně NuGet Předvolby) je nyní povoleno nainstalováním NuGet. Obnovení balíčku teď jednoduše funguje ve výchozím nastavení.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>ASP.NET – webové formuláře

### <a name="one-aspnet"></a>One ASP.NET

Šablony projektů webových formulářů bez problémů integrovat nové prostředí One ASP.NET. Můžete přidat MVC a webového rozhraní API podporují do projektu webové formuláře a nakonfigurujete ověřování pomocí Průvodce vytvořením projektu One ASP.NET. Další informace najdete v tématu [vytváření webových projektů ASP.NET v sadě Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Šablony projektů webových formulářů podporují novou architekturu identit technologie ASP.NET. Kromě toho šablony teď podporují vytváření intranetový projekt webových formulářů. Další informace najdete v tématu [metody ověřování](creating-web-projects-in-visual-studio.md#auth) v **vytváření webových projektů ASP.NET v sadě Visual Studio 2013**.

### <a name="bootstrap"></a>Bootstrap

Použití šablony webových formulářů [Bootstrap](http://twitter.github.io/bootstrap/) k poskytování elegantní a rychle reagující vzhled a chování, které můžete snadno přizpůsobit. Další informace najdete v tématu [Bootstrap v šablony webových projektů Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>One ASP.NET

Šablon projektu Web MVC bez problémů integrovat nové prostředí One ASP.NET. Můžete přizpůsobit projekt MVC a konfigurace ověřování pomocí Průvodce vytvořením projektu One ASP.NET. Úvodní kurz k ASP.NET MVC 5 najdete za [Začínáme s ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Informace o upgradu projektů MVC 4 do MVC 5 najdete v tématu [pokyny k upgradu ASP.NET MVC 4 a projektem webového rozhraní API technologie ASP.NET MVC 5 a webovým rozhraním API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Použití ASP.NET Identity pro ověřování a správě identit se aktualizovaly šablon projektu MVC. Kurz ověřování Facebooku nebo Googlu a členství v nové rozhraní API najdete tady [vytvoření aplikace ASP.NET MVC 5 pomocí Facebooku a Google OAuth2 nebo OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) a [vytvoření aplikace ASP.NET MVC pomocí ověření a Databáze SQL a nasazení do služby Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap

Šablona projektu MVC byl aktualizován na použití [Bootstrap](http://getbootstrap.com/) k poskytování elegantní a rychle reagující vzhled a chování, které můžete snadno přizpůsobit. Další informace najdete v tématu [Bootstrap v šablony webových projektů Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtry ověřování

Ověřovací filtry se nový typ filtru v architektuře ASP.NET MVC, která spustí před filtry autorizace v kanálu ASP.NET MVC a umožňují zadat ověřovací logiky akcích, na řadič, nebo globálně pro všechny řadiče. Filtry ověřování zpracovat přihlašovací údaje v požadavku a zadejte odpovídající objekt zabezpečení. Filtry ověřování můžete také přidat výzev ověřování v reakci na neoprávněné požadavky.

### <a name="filter-overrides"></a>Přepisy filtru

Nyní můžete přepsat, jaké filtry se použijí metody dané akce nebo kontroleru zadáním filtru přepsání. Přepsání filtry zadat sadu typy filtrů, které by se neměl spouštět pro daný obor (akce nebo kontroleru). To umožňuje konfigurovat filtry, které platí globálně, ale potom vyloučit určité globální filtry z použití na konkrétní akce nebo řadiče.

### <a name="attribute-routing"></a>Směrování atributů

ASP.NET MVC teď podporuje směrování atributů, díky příspěvek podle Tim McCall, autora [ http://attributerouting.net ](http://attributerouting.net). Se směrováním atributů můžete zadat trasy anotací akce a kontrolery.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>Rozhraní API pro ASP.NET Web 2

### <a name="attribute-routing"></a>Směrování atributů

Rozhraní ASP.NET Web API nyní podporuje směrování atributů, díky příspěvek podle Tim McCall, autora [ http://attributerouting.net ](http://attributerouting.net). Se směrováním atributů můžete zadat trasy rozhraní Web API anotací akce a kontrolery takto:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Směrování atributů vám dává větší kontrolu nad identifikátory URI webového rozhraní API. Můžete například snadno definovat prostředek hierarchii pomocí jediného kontroleru rozhraní API:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Směrování atributů také poskytuje pohodlné syntaxe pro zadání volitelné parametry, výchozí hodnoty a omezení trasy:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Další informace o směrování atributů najdete v tématu [směrováním atributů ve webovém rozhraní API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Šablony projektu webového rozhraní API a jednostránkové aplikace teď podporují ověřování pomocí OAuth 2.0. OAuth 2.0 je architektura určená k autorizaci přístupu klientů k chráněným prostředkům. Funguje pro celou řadu klientů včetně prohlížečů a mobilních zařízení.

Podpoře OAuth 2.0 je založená na nové middleware zabezpečení poskytované komponenty Microsoft OWIN pro ověřování nosiče a implementaci autorizace role serveru. Případně klienti se můžou autorizovat pomocí organizační autorizační server, jako je Azure Active Directory nebo služby AD FS ve Windows serveru 2012 R2.

### <a name="odata-improvements"></a>Vylepšení protokolu OData

**Podpora pro $select $rozbalte $batch, $value a**

ASP.NET Web API OData má teď plnou podporu pro $select, rozbalte položku $ a $value. Můžete také použít $batch pro žádost o dávkové zpracování a zpracování sad změn.

Možnosti $select a $$expand umožňují změnit tvar data, která je vrácena z koncového bodu OData. Další informace najdete v tématu [Introducing $select a $expand rozbalte podpory v Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Vylepšená rozšiřitelnost**

Formátovací moduly OData se teď rozšiřitelné. Můžete přidat položku metadat Atom, podporuje pojmenované datového proudu a media link položek, přidání anotací instance a přizpůsobit způsob generování odkazů.

**Typ bez podpory**

Teď můžete vytvářet služby OData, aniž byste museli definovat typy CLR pro vaše typy entit. Místo toho můžete své řadiče OData trvat nebo vrátit instanci **IEdmObject**, což jsou formátovací moduly OData můžeme serializovat a deserializovat.

**Znovu použít existující model**

Pokud již máte existující entity data model (EDM), můžete teď znovu použít ho přímo, namísto nutnosti vytvářet nové. Například pokud používáte Entity Framework, můžete použít EDM, který vytváří EF za vás.

### <a name="request-batching"></a>Žádost o dávkové zpracování

Žádost o dávkové zpracování kombinuje několik operací do jednoho požadavku HTTP POST, snížit síťový provoz a poskytovat plynulejší méně přetížení uživatelské rozhraní. Rozhraní ASP.NET Web API nyní podporuje několik strategií pro dávkové zpracování požadavku:

- Použijte koncový bod $batch služby OData.
- Balíček několika požadavků do jednoho požadavku vícedílné zprávy standardu MIME.
- Používejte vlastní formát dávkování.

Pokud chcete povolit žádosti o dávkové zpracování, jednoduše přidejte trasu s obslužnou rutinu dávkování do konfigurace webového rozhraní API:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

Můžete taky řídit, zda požadavky nebo provést postupně a v libovolném pořadí.

### <a name="portable-aspnet-web-api-client"></a>Přenosné ASP.NET Web API klienta

Klienta ASP.NET Web API nyní slouží k vytvoření knihovny tříd portable, které pracují mezi aplikacemi pro Windows Store a Windows Phone 8. Můžete také vytvořit přenosné formátovací moduly, které můžete sdílet mezi klientem a serverem.

### <a name="improved-testability"></a>Zlepšení testovatelnosti

Webové rozhraní API 2 je to mnohem jednodušší k jednotce pro vaše rozhraní API řadičů testu. Stačí vytvořit instanci váš kontroler API s zprávy s požadavkem a konfiguraci a poté zavolejte metodu akce, kterou chcete testovat. Je také snadné napodobení **UrlHelper** třídy pro metody akce, které provádějí generování odkazů.

### <a name="ihttpactionresult"></a>IHttpActionResult

Teď můžete implementovat IHttpActionResult k zapouzdření výsledku metody akce vašeho webového rozhraní API. IHttpActionResult vrátil z metody akce webového rozhraní API provádí modulem runtime ASP.NET Web API pro vytvoření zprávy výsledné odpovědi. IHttpActionResult mohou být vráceny všechny akce webového rozhraní API pro zjednodušení jednotky testování vaší implementace rozhraní Web API. Pro usnadnění práce, jsou k dispozici několik implementací IHttpActionResult úprav, včetně výsledků pro vrácení konkrétní stavové kódy formátovaný obsah nebo obsah vyjedná odpovědi.

### <a name="httprequestcontext"></a>HttpRequestContext

Nové **HttpRequestContext** sleduje jakýkoli stav, který se váže na žádost, ale není ihned k dispozici z požadavku. Například můžete použít **HttpRequestContext** zobrazíte data trasy, která objekt přidružený k požadavku, klientského certifikátu **UrlHelper** a kořenový adresář virtuální cesty. Můžete snadno vytvářet **HttpRequestContext** účely testování jednotky.

Protože objekt zabezpečení pro požadavek je naplněn s požadavkem, aniž byste museli spoléhat na **vlastnost Thread.CurrentPrincipal**, objekt zabezpečení je teď dostupná v průběhu životního cyklu požadavku i když je v kanálu webového rozhraní API.

### <a name="cors"></a>CORS

Díky Další skvělý příspěvek od společnosti Brock Allen ASP.NET teď plně podporuje různé žádost o sdílení zdroji (CORS).

Zabezpečení prohlížečů brání zasílání požadavků AJAX na jinou doménu na webové stránce. [CORS](http://www.w3.org/TR/cors/) je standard W3C, která umožňuje server zmírnit zásadu stejného zdroje. Pomocí CORS, server explicitně můžou některé požadavky cross-origin zatímco jiné odmítnout.

Webové rozhraní API 2 nyní podporuje CORS, včetně automatické zpracování předběžných požadavků. Další informace najdete v tématu [povolení žádostí nepůvodního zdroje v rozhraní ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtry ověřování

Ověřovací filtry se nový typ filtru v rozhraní ASP.NET Web API, která spustí před filtry autorizace v kanálu rozhraní ASP.NET Web API a umožňují zadat ověřovací logiky akcích, na řadič, nebo globálně pro všechny řadiče. Filtry ověřování zpracovat přihlašovací údaje v požadavku a zadejte odpovídající objekt zabezpečení. Filtry ověřování můžete také přidat výzev ověřování v reakci na neoprávněné požadavky.

### <a name="filter-overrides"></a>Přepsání filtru

Nyní můžete přepsat, jaké filtry se použijí metody dané akce nebo kontroleru, tak, že určíte filtru přepsání. Přepsání filtry zadat sadu filtr typy, které by neměl spouštět pro daný obor (akce nebo kontroleru). To umožňuje přidat globální filtry, ale pak vyloučit některé z určité akce nebo řadiče.

### <a name="owin-integration"></a>Integrace OWIN

Rozhraní ASP.NET Web API nyní plně podporuje OWIN a lze je spustit na žádném hostiteli podporující OWIN. Jsou zde také zahrnuty **HostAuthenticationFilter** , který poskytuje integraci se systémem ověřování OWIN.

Díky integraci OWIN mohou samoobslužné hostování webového rozhraní API ve vašem vlastním procesu společně s další middleware OWIN, jako je například SignalR. Další informace najdete v tématu [OWIN použití na webové rozhraní API ASP.NET Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>Funkce SignalR technologie ASP.NET 2.0

Následující části popisují funkce SignalR 2.0.

- [Základem OWIN](#builtonowin)
- [MapHubs a MapConnection jsou nyní MapSignalR](#MapSignalR)
- [Podpora víc domén](#crossdomain)
- [zařízení s iOS a Android podporují prostřednictvím MonoTouch a Monodroidu](#mobile)
- [Klient Portable .NET](#portable)
- [Nový balíček pro hostování na vlastním serveru](#selfhost)
- [Podpora serveru zpětně kompatibilní](#backwardcompat)
- [Odebrat server podpory pro rozhraní .NET 4.0](#remove40)
- [Odeslání zprávy do seznamu klienti a skupiny](#messagelist)
- [Odesílání zprávy pro konkrétního uživatele](#sendtouser)
- [Lepší podporu pro zpracování chyb](#errorhandling)
- [Jednodušší jednotky testování rozbočovače](#unittesting)
- [Zpracování chyb JavaScriptu](#javascripterror)

Příklad toho, jak upgradovat existující projekt 1.x do SignalR 2.0, naleznete v tématu [inovace knihovnou SignalR 1.x projektu](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Základem OWIN

Funkce SignalR 2.0 je sestavena zcela na [OWIN (Open Web Interface pro .NET)](http://owin.org/). Tato změna umožňuje procesu instalace pro funkci SignalR mnohem více konzistentní mezi aplikací SignalR webové hostována a v místním prostředí, ale má také požadovaný počet změn rozhraní API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs a MapConnection jsou nyní MapSignalR

Z důvodu kompatibility se standardy OWIN, tyto metody jsou přejmenované na `MapSignalR`. `MapSignalR` volá se, bez parametrů se namapuje všechny rozbočovače (jako `MapHubs` nemá ve verzi 1.x); Chcete-li namapovat jednotlivé **PersistentConnection** objektů, zadejte jako parametr typu a přípona adresy URL pro připojení jako typ připojení první argument.

`MapSignalR` Metoda je volána v třídy pro spuštění Owin. Visual Studio 2013 obsahuje nové šablony pro třídy pro spuštění Owin; Pokud chcete použít tuto šablonu, postupujte takto:

1. Klikněte pravým tlačítkem na projekt
2. Vyberte **přidat**, **novou položku...**
3. Vyberte **třída Owin Startup**. Pojmenujte novou třídu **Startup.cs**.

V **webovou aplikaci,** obsahující třídy Owin při spuštění `MapSignalR` metody se pak přidá do vaší Owin spouštění pomocí položky v uzlu nastavení aplikace v souboru Web.Config, jak je znázorněno níže.

V **aplikace v místním prostředí**, třída při spuštění se předá jako parametr typu `WebApp.Start` metody.

**Mapování centra a připojení v SignalR 1.x (ze souboru global aplikace ve webové aplikaci):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Mapování centra a připojení v SignalR 2.0 (ze souboru třída Owin Startup):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

V **aplikace v místním prostředí**, třída při spuštění se předá jako parametr typu pro `WebApp.Start` způsob, jak je znázorněno níže.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Podpora víc domén

V knihovně SignalR 1.x různé požadavky na doménu se řídí jediného příznaku EnableCrossDomain. Tento příznak řídí požadavky na JSONP a CORS. Pro větší flexibilitu a podporu všech CORS je odebraná ze součásti serveru pro funkci SignalR (lients JavaScriptu dál normálně ho používat CORS Pokud se zjistí, jestli prohlížeč podporuje), a nové middlewaru OWIN, který byl zpřístupněn pro podporu těchto scénářů.

V systému SignalR 2.0, pokud JSONP se vyžaduje na straně klienta (pro podporu požadavků mezi doménami ve starších prohlížečích), ji budou muset být explicitně povoluje nastavením `EnableJSONP` na `HubConfiguration` objektu `true`, jak je znázorněno níže. JSONP je standardně zakázaná, protože je to méně bezpečné než CORS.

Chcete-li přidat nový middlewarem CORS v SignalR 2.0, přidejte `Microsoft.Owin.Cors` knihovny do projektu a volejte `UseCors` před SignalR middleware, jak je znázorněno v následující části.

**Přidání do projektu Microsoft.Owin.Cors**: nainstalovat, spusťte následující příkaz v konzole Správce balíčků:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Tento příkaz přidá 2.0.0 verzi balíčku do projektu.

**Volání UseCors**

Následující fragmenty kódu ukazují, jak implementovat připojení mezi doménami v knihovně SignalR 1.x a 2.0.

**Implementace žádosti napříč doménami v knihovně SignalR 1.x (ze souboru global aplikace)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementace žádosti napříč doménami v SignalR 2.0 (ze souboru kódu C#)**

Následující kód ukazuje, jak povolit CORS a JSONP v projektu SignalR 2.0. Tento vzorový kód používá `Map` a `RunSignalR` místo `MapSignalR`, takže middlewarem CORS běží pouze pro funkci SignalR žádosti, které vyžadují podporu CORS (a nikoli pro veškerý provoz na cestě zadané v `MapSignalR`.) `Map` lze také použít pro veškerý middleware, který je potřeba spustit pro konkrétní předponu adresy URL, a nikoli pro celou aplikaci.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>zařízení s iOS a Android podporují prostřednictvím MonoTouch a Monodroidu

Byla přidána podpora pro iOS a Android klienty, kteří používají MonoTouch a Monodroidu součásti ze [knihovna pro Xamarin](https://xamarin.com/). Další informace o tom, jak je používat, naleznete v tématu [pomocí komponenty Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Tyto součásti vám bude k dispozici [Xamarin Store](https://store.xamarin.com/) při vydání verze SignalR RTW je k dispozici.

<a id="portable"></a> ### Přenosné klienta .NET

Pro lepší usnadňují vývoj pro různé platformy, Silverlight, WinRT a klienti Windows Phone byly nahrazeny jednoho přenosné .NET klienta, který podporuje tyto platformy:

- NET 4.5
- Silverlight 5
- WinRT (.NET pro aplikace Windows Store)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nový balíček pro hostování na vlastním serveru

Teď je k balíčku NuGet, aby bylo snazší, abyste mohli začít s hostování na vlastním serveru knihovnou SignalR (SignalR aplikace, které jsou hostované v procesu nebo jiné aplikace, nikoli hostované na webovém serveru). Upgrade projektu hostování na vlastním serveru integrované s knihovnou SignalR 1.x, odebrat balíček Microsoft.AspNet.SignalR.Owin a přidání Microsoft.AspNet.SignalR.SelfHost balíčku. Další informace o zahájení práce s balíček hostování na vlastním serveru najdete v tématu [kurz: SignalR v místním](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Podpora serveru zpětně kompatibilní

V předchozích verzích nástroje SignalR, verze balíčku SignalR v klientovi a serveru museli být identické. Aby bylo možné podporovat tlustá klientské aplikace, které by bylo obtížné aktualizovat, SignalR 2.0 teď podporuje pomocí staršího klienta na novější verzi serveru. **Poznámka: SignalR 2.0 nepodporuje serverů se staršími verzemi vytvořené pomocí nových klientů.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Odebrat server podpory pro rozhraní .NET 4.0

Funkce SignalR 2.0 klesla podporu pro server vzájemná funkční spolupráce s .NET 4.0. Rozhraní .NET 4.5 musí být použit s knihovnou SignalR 2.0 servery. Se stále vyžaduje .NET 4.0 klienta SignalR 2.0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Odeslání zprávy do seznamu klienti a skupiny

V systému SignalR 2.0 je možné odeslat zprávu pomocí seznamu klienta a ID skupin. Následující fragmenty kódu ukazují, jak to provést.

**Odeslání zprávy do seznamu klienti a skupiny pomocí PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Odeslání zprávy do seznamu klienti a skupiny pomocí centra**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Odesílání zprávy pro konkrétního uživatele

Tato funkce umožňuje uživatelům určit, co je ID uživatele IRequest přes nové rozhraní IUserIdProvider podle:

**Rozhraní IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Ve výchozím nastavení bude implementace, která se používá jako uživatelské jméno uživatele IPrincipal.Identity.Name.

Rozbočovače budete moct posílat zprávy k těmto uživatelům přes nové rozhraní API:

**Pomocí Clients.User rozhraní API**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Lepší podporu pro zpracování chyb

Uživatelé teď můžou vyvolat **HubException** z jakékoli volání rozbočovače. Konstruktor třídy **HubException** přijímá řetězcovou zprávu a objekt dodatečné údaje o chybě. SignalR bude automaticky serializovat výjimky a jejich odesílání do klienta ve kterém se použije k odmítnutí/převzetí služeb při volání metody rozbočovače.

**Zobrazit výjimkách centra podrobné** nastavení nemá žádný vliv na **HubException** odesílaných zpět do klienta, nebo Ne, jsou vždy odeslána.

**Ukázka, odesílání HubException klientovi kód na straně serveru**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Kód jazyka JavaScript klienta demonstrace reakce na HubException odeslaných ze serveru**

[!code-html[Main](release-notes/samples/sample16.html)]

**Klientský kód .NET demonstrace reakce na HubException odeslaných ze serveru**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Jednodušší jednotky testování rozbočovače

Funkce SignalR 2.0 obsahuje rozhraní volá `IHubCallerConnectionContext` v centrech, která usnadňuje vytvoření mock klienta u volání rozbočovače na straně. Následující fragmenty kódu ukazují, toto rozhraní pomocí oblíbených testovacích postroje [xUnit.net](https://github.com/xunit/xunit) a [moq](https://code.google.com/p/moq/).

**Testování částí SignalR pomocí xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Testování částí SignalR pomocí moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Zpracování chyb JavaScriptu

Všechny zpětná volání JavaScript zpracování chyb v SignalR 2.0 vrátí objekty jazyka JavaScript chyby namísto nezpracovaných řetězců. To umožňuje SignalR bohatší informace do vaší obslužné rutiny chyb. Vnitřní výjimka z můžete získat `source` vlastnost chyby.

**Klientský kód jazyka JavaScript, který zpracovává výjimku Start.Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Nový systém členství technologie ASP.NET

ASP.NET Identity je nový systém členství pro aplikace ASP.NET. ASP.NET Identity snadno integrovat data profilu uživatelská data aplikací. ASP.NET Identity můžete také zvolte model trvalosti pro profily uživatelů ve vaší aplikaci. Data můžete ukládat v databázi serveru SQL Server nebo jiného úložiště dat, včetně datových úložišť typu NoSQL, jako jsou úložiště tabulek Azure. Další informace najdete v tématu [jednotlivé uživatelské účty](creating-web-projects-in-visual-studio.md#indauth) v **vytváření webových projektů ASP.NET v sadě Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Ověřování na základě deklarací

Technologie ASP.NET nyní podporuje ověřování nezaloženého na deklaracích, kde je identitu uživatele reprezentována jako sada deklarací z důvěryhodného vystavitele. Uživatelé mohou být ověřené pomocí uživatelského jména a hesla, které jsou zachovány v databázi aplikace, nebo pomocí zprostředkovatelů sociálních identit (například: Accounts Microsoft, Facebook, Google, Twitter), nebo pomocí účty organizace prostřednictvím Azure Active Directory nebo Active Directory Federation Services (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integrace s Azure Active Directory a Windows Server Active Directory

Nyní můžete vytvořit projekty ASP.NET, které používají Azure Active Directory nebo Windows Server Active Directory (AD) pro ověřování. Další informace najdete v tématu [účty organizace](creating-web-projects-in-visual-studio.md#orgauth) v **vytváření webových projektů ASP.NET v sadě Visual Studio 2013**.

### <a name="owin-integration"></a>Integrace OWIN

Ověřování pomocí technologie ASP.NET je teď podle middleware OWIN, který jde použít na všechny hostitele na bázi OWIN. Další informace o OWIN, naleznete na následujícím [komponenty Microsoft OWIN](#TOC7) oddílu.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Komponenty Microsoft OWIN

[Otevřete Web Interface pro .NET](http://owin.org/) (OWIN) definuje abstrakce mezi .NET webové servery a webové aplikace. OWIN odpojí webové aplikace ze serveru, provedení hostitele nezávislá na webové aplikace. Můžete například hostování OWIN webové aplikace ve službě IIS nebo samoobslužné hostování ve vlastním procesu.

Změny zavedené v komponenty Microsoft OWIN (označované také jako projektu Katana) zahrnují nové součásti serveru a hostitele, nové pomocné knihovny a middleware a nový ověřovací middleware.

Další informace o OWIN a Katana najdete v tématu [co je nového v OWIN a Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Poznámka: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) aplikace nelze spustit v režimu classic služby IIS; musí být spuštěn v integrovaném režimu.**

**Poznámka: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) aplikace musí být spuštěn v režimu plné důvěryhodnosti.**

### <a name="new-servers-and-hosts"></a>Nové servery a hostitele

V této vydané verzi byly přidány nové komponenty aby se povolily scénáře hostování na vlastním serveru. Mezi tyto komponenty patří následující balíčky NuGet:

- **Microsoft.Owin.Host.HttpListener**. Poskytuje serveru OWIN, který používá **HttpListener** pro naslouchání požadavků protokolu HTTP a směrují je do kanálu OWIN.
- **Microsoft.Owin.Hosting** poskytuje knihovnu pro vývojáře, kteří chtějí samoobslužné hostování ve vlastním procesu, jako je například konzolové aplikace nebo služba Windows kanálu OWIN.
- **OwinHost**. Poskytuje samostatný spustitelný soubor, který obtéká `Microsoft.Owin.Hosting` a umožňuje hostování na vlastním serveru kanál OWIN bez nutnosti psát vlastní hostitelskou aplikaci.

Kromě toho `Microsoft.Owin.Host.SystemWeb` balíček nyní umožňuje middlewaru, který má poskytnout nápovědu pro **SystemWeb** server označující, že by měla být volána middleware fázi konkrétní kanálu ASP.NET. Tato funkce je zvláště užitečná pro middleware ověřování, které by měly být spuštěny již v rané fázi kanálu ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Pomocné rutiny knihovny a Middleware

I když můžete napsat komponenty OWIN používat pouze funkce a typ definice z specifikace OWIN nové `Microsoft.Owin` balíček poskytuje sadu abstrakce přívětivější. Tento balíček kombinuje několik starších balíčků (například `Owin.Extensions`, `Owin.Types`) do jednoho, strukturované objektový model, který může potom snadno využívat jiné komponenty OWIN. Ve skutečnosti většinou komponenty Microsoft OWIN nyní tento balíček použít.

> [!NOTE]
> [OWIN](http://www.owin.org) aplikace nelze spustit v režimu classic služby IIS; musí být spuštěn v integrovaném režimu.

> [!NOTE]
> [OWIN](http://www.owin.org) aplikace musí být spuštěn v režimu plné důvěryhodnosti.

Tato verze rovněž obsahuje Microsoft.Owin.Diagnostics balíček, který zahrnuje middleware pro ověření běžící aplikaci OWIN a middleware chybovou stránku umožňující vyšetřování chyb.

### <a name="authentication-components"></a>Ověření součásti

Následující komponenty ověřování jsou k dispozici.

- **Microsoft.Owin.Security.ActiveDirectory**. Umožňuje ověřování pomocí místní nebo cloudové adresářové služby.
- **Microsoft.Owin.Security.Cookies** umožňuje ověřování pomocí souborů cookie. Tento balíček se dříve nazýval `Microsoft.Owin.Security.Forms`.
- **Microsoft.Owin.Security.Facebook** umožňuje ověřování pomocí služeb na základě OAuth pro Facebook.
- **Microsoft.Owin.Security.Google** umožňuje ověřování pomocí služby Google OpenID založené.
- **Microsoft.Owin.Security.Jwt** umožňuje ověřování pomocí tokenů JWT.
- **Microsoft.Owin.Security.MicrosoftAccount** umožňuje ověřování používá účty Microsoft.
- **Microsoft.Owin.Security.OAuth**. Poskytuje serveru ověřování OAuth, jakož i middleware pro ověřování nosných tokenů.
- **Microsoft.Owin.Security.Twitter** umožňuje ověřování pomocí služeb na základě OAuth pro Twitter.

Tato verze rovněž obsahuje `Microsoft.Owin.Cors` balíček, který obsahuje middleware pro zpracování požadavků HTTP mezi zdroji.

> [!NOTE]
> V konečné verzi sady Visual Studio 2013 byla odebrána podpora k podepisování tokenů JWT.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Seznam nových funkcí a další změny v Entity Framework 6 najdete v tématu [historie verzí Entity Framework](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>Syntaxe Razor rozhraní ASP.NET 3

Syntaxe Razor rozhraní ASP.NET 3 obsahuje následující nové funkce:

- Podpora pro úpravy karty. Dříve **formátovat dokument** příkazu, Automatické odsazení a automatické formátování v sadě Visual Studio nefungovala správně při použití **zachovat tabulátory** možnost. Tato změna opravuje formátování kódu Razor pro kartu formátování pro Visual Studio.
- Podporu pravidel přepisování adres URL při generování odkazů.
- Odebrání transparentní atribut zabezpečení.
  > [!NOTE]
  > K zásadní změně a díky Razor 3 nekompatibilní pomocí MVC4 a starší, zatímco Razor 2 není kompatibilní s MVC5 nebo sestavení, proti MVC5.

Můžete najít Razor 3 opravených chybách v sadě Visual Studio 2013 z předběžných verzí [tady](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>Pozastavení aplikace technologie ASP.NET

Pozastavení aplikace technologie ASP.NET je funkce mění pravidla hry v rozhraní .NET Framework 4.5.1, podstatně změní uživatelské prostředí a ekonomický model pro hostování velký počet webů ASP.NET na jednom počítači. Další informace najdete v tématu [pozastavení aplikace technologie ASP.NET – responzivní sdílené hostování webů .NET](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a změny způsobující chyby

Tato část popisuje známé problémy a novinkách v ASP.NET and Web Tools pro Visual Studio 2013.

### <a name="nuget"></a>NuGet

- [Nový balíček obnovení nebude fungovat v Mono, při použití souboru SLN](https://nuget.codeplex.com/workitem/3596) – bude opravený v nadcházející nuget.exe stahování a [NuGet.CommandLine balíčku](http://www.nuget.org/packages/NuGet.CommandLine/) aktualizovat.
- [Nové obnovení balíčků nefunguje s projekty Wix](https://nuget.codeplex.com/workitem/3598) – bude opravený v nadcházející nuget.exe stahování a [NuGet.CommandLine balíčku](http://www.nuget.org/packages/NuGet.CommandLine/) aktualizovat.
- [Automatické obnovení balíčku nebude fungovat pro projekty v rámci složky řešení](https://nuget.codeplex.com/workitem/3625) – bude opravený v NuGet 2.8.

### <a name="aspnet-web-api"></a>Rozhraní API pro ASP.NET Web

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` nevrací `IQueryable<T>` vždy, protože jsme přidali podporu pro `$select` a `$expand`.

    Naše starší ukázky pro `ODataQueryOptions<T>` vždy zasazena návratová hodnota z `ApplyTo` k `IQueryable<T>`. To dříve pracoval, protože možnosti dotazu, pro které podporujeme dříve (`$filter`, `$orderby`, `$skip`, `$top`) nedojde ke změně tvaru dotazu. Teď podporujeme `$select` a `$expand` návratovou hodnotu z `ApplyTo` nebudou `IQueryable<T>` vždy.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Pokud použijete ukázkový kód z dříve, pokud klient nezasílal bude dál pracovní `$select` a `$expand`. Nicméně pokud chcete podporovat `$select` a `$expand` budete muset změnit na to, že kód.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url nebo RequestContext.Url má hodnotu null během žádosti o služby batch**

    Ve scénáři dávkování **UrlHelper** má hodnotu null při přístupu z **Request.Url** nebo **RequestContext.Url**.

    Tento problém momentálně sledovat tady: [BatchRequestContext.Url má hodnotu null pro dávkové zpracování požadavku](http://aspnetwebstack.codeplex.com/workitem/1301).

    Alternativní řešení je vytvořit novou instanci třídy **UrlHelper**, jako v následujícím příkladu:

    **Vytvoření nové instance UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Při použití MVC5 a OrgAuth, pokud máte zobrazení, které provést ověření AntiForgerToken, můžete narazit na následující chybu při odesílání dat do zobrazení:

    **Chyba**:

    *Chyba serveru v aplikaci '/'.*

    <em>Deklarace typu "<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'nebo'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>" není k dispozici na zadaný objekt ClaimsIdentity. Povolení podpory tokenů proti padělání pomocí ověřování nezaloženého na deklaracích, zkontrolujte, zda nakonfigurované zprostředkovatele je poskytování obě tyto deklarace identity ClaimsIdentity instancí, který generuje. Pokud zprostředkovatele deklarací identity nakonfigurovaný místo toho používá jiný typ deklarací jako jedinečný identifikátor, můžete nakonfigurovat tak, že nastavíte vlastnost statické AntiForgeryConfig.UniqueClaimTypeIdentifier.</em>

    **Alternativní řešení**:

    Přidejte následující řádek v souboru Global.asax to opravit:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Tato chyba bude opravena pro další verzi.
2. Po provedení upgradu aplikace MVC4 pro MVC5, sestavte řešení a spusťte jej. Zobrazí se následující chyba:

    [A] System.Web.WebPages.Razor.Configuration.HostSection nelze přetypovat na [B]System.Web.WebPages.Razor.Configuration.HostSection. Typu A mohou být "System.Web.WebPages.Razor, verze = 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35" v kontextu "Default" v umístění "C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ V4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll ". Typ B pochází z "System.Web.WebPages.Razor, verze = 3.0.0.0, jazyková verze = neutral, PublicKeyToken = 31bf3856ad364e35" v kontextu "Default" v umístění ' Files\root\6d05bbd0\ C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary technologie ASP.NET e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll ".

    Chcete-li výše uvedené chybu opravit, otevřete *všechny* souborů Web.config (včetně těch v zobrazení složky) v projektu a proveďte následující:

   1. Aktualizujte všechny výskyty "System.Web.Mvc" verze "4.0.0.0" "5.0.0.0".
   2. Aktualizovat všechny výskyty verzi "System.Web.Helpers", "2.0.0.0" &quot;System.Web.WebPages&quot; a &quot;System.Web.WebPages.Razor&quot; na "3.0.0.0"

      Například po provedení výše uvedené změny vazeb sestavení by měl vypadat takto:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Informace o upgradu projektů MVC 4 do MVC 5 najdete v tématu [pokyny k upgradu ASP.NET MVC 4 a projektem webového rozhraní API technologie ASP.NET MVC 5 a webovým rozhraním API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Při použití ověřování na straně klienta s jQuery Nerušivý ověření, je někdy zprávu ověření pro element input HTML s typem = 'number'. Chyba ověřování pro povinnou hodnotu ("The Age pole je povinné") se zobrazí kdy je zadáno neplatné číslo místo správnou zprávu, že je požadováno platné číslo.

    Tento problém se běžně vyskytují se automaticky generovaný kód pro model s celočíselnou vlastnost pro zobrazení vytvořit a upravit.

    Chcete-li tento problém obejít, změňte pomocný editor ze:

    `@Html.EditorFor(person => person.Age)`

    Do:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 se už nepodporuje částečným vztahem důvěryhodnosti. Projekty propojení na binární soubory MVC nebo WebAPI by měla být odstraněna [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) atribut a [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) atribut. Odebírání těchto atributů dojde k odstranění chyby kompilátoru, jako je následující.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Všimněte si, jak vedlejším účinkem této je nelze použít 4.0 a 5.0 sestavení ve stejné aplikaci. Budete muset aktualizovat všechny z nich 5.0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Jednostránková aplikace šablony s autorizací služby Facebook může způsobit nestabilitu v Internet Exploreru, zatímco je hostovaný na webu do zóny sítě intranet

Šablona SPA poskytuje externí Přihlaste se pomocí Facebooku. Když projekt vytvořen pomocí šablony běží místně, může způsobit podepisování se v Internet Exploreru k chybě.

Řešení:

1. Hostování webu v Internetové zóně; nebo

2. Otestování tohoto scénáře v prohlížeči než Internet Exploreru.

### <a name="web-forms-scaffolding"></a>Webové formuláře, generování uživatelského rozhraní

Webové formuláře generování uživatelského rozhraní se odebral z VS2013 a bude k dispozici v budoucí aktualizaci sady Visual Studio. Generování uživatelského rozhraní v rámci projektu webových formulářů však můžete použít přidáním závislosti MVC a generování generování uživatelského rozhraní pro MVC. Váš projekt bude obsahovat kombinaci webových formulářů a MVC.

Přidat do projektu webových formulářů MVC, přidejte nová vygenerovaná položka a vyberte **závislosti MVC 5**. Vyberte minimální nebo úplné v závislosti na tom, jestli budete potřebovat všechny soubory obsahu, jako jsou skripty. Potom přidání vygenerované položky pro MVC, který vytvoří zobrazení a kontroler ve vašem projektu.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC a generování rozhraní Web API – HTTP 404, nebyla nalezena chyba

Pokud dojde k chybě při přidání vygenerované položky do projektu, je možné, že váš projekt bude ponechána v nekonzistentním stavu. Některé změny se generování uživatelského rozhraní bude vrácena zpět, ale jiné změny, jako je například nainstalované balíčky NuGet, nebude vrácena zpět. Pokud se vrátí zpět změny konfigurace směrování, uživatelům se zobrazí chyba HTTP 404 při přechodu na automaticky generovaný položky.

Alternativní řešení:

- Chcete-li vyřešit tuto chybu pro MVC, přidejte nová vygenerovaná položka a vybrat závislosti MVC 5 (minimální nebo úplná). Tento proces se všechny požadované změny, přidejte do projektu.
- Chcete-li vyřešit tuto chybu pro webové rozhraní API:

  1. Přidání třídy WebApiConfig do projektu.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. V aplikaci nakonfigurovat WebApiConfig.Register\_začátek metody v souboru Global.asax následujícím způsobem:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
