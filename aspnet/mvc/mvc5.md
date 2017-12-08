---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: "ASP.NET MVC 5 ASP.NET MVC 5 je rozhraní pro vytváření škálovatelných standardy webových aplikací pomocí vzory návrhu zavedené a výkon AS...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: e57163469ae4606df0fc17e3e054b7696782a084
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>Co je nového v architektuře ASP.NET MVC 5

### <a name="one-aspnet"></a>Jeden ASP.NET

Šablon projektu MVC webové bezproblémově integrovat nové prostředí jeden ASP.NET. Můžete přizpůsobit projektu MVC a nakonfigurovat ověřování pomocí Průvodce vytvořením projektu jeden ASP.NET. Úvodní kurz k ASP.NET MVC 5 naleznete na adrese [Začínáme s ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md).

Informace týkající se upgradu projekty MVC 4 až MVC 5 najdete v tématu [postup upgradu služby ASP.NET MVC 4 a projekt webového rozhraní API ASP.NET MVC 5 a webovém rozhraní API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Šablon projektu MVC byly aktualizovány na používání technologie ASP.NET Identity pro ověřování a správu identit. Kurz poskytuje funkci ověřování sítě Facebook a Google a členství v nové rozhraní API naleznete na adrese [vytvořit aplikaci ASP.NET MVC 5 s použitím Facebook a Google OAuth2 a přihlašování OpenID](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) a [nasazení aplikace ASP.NET MVC zabezpečení s Členství, OAuth a webu systému Windows Azure SQL Database](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>Bootstrap

Šablona projektu MVC je aktualizovaná tak, aby použít [Bootstrap](http://getbootstrap.com/) k poskytování elegantní a dobře reagovaly vzhled a chování, můžete snadno přizpůsobit. Další informace najdete v tématu [Bootstrap v šablony webových projektů Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtry ověřování

[Filtry ověřování](http://www.dotnetcurry.com/showarticle.aspx?ID=957) jsou nový typ filtru v architektuře ASP.NET MVC, která spustí před filtry autorizace v architektuře ASP.NET MVC kanálu a umožňují určit ověřování logiku za akce, kontroleru, nebo globálně pro všechny řadiče. Filtry ověřování zpracovat přihlašovací údaje v požadavku a zadejte odpovídající objekt zabezpečení. Filtry ověřování můžete také přidat výzev ověřování v reakci na neoprávněné žádosti. V tématu [filtry ověřování aplikace ASP.NET MVC 5](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [filtry ověřování v ASP.NET MVC 5](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/) a [nakonec nové technologie ASP.NET MVC 5 filtry ověřování!](http://hackwebwith.net/finally-the-new-asp-net-mvc-5-authentication-filters/).

### <a name="filter-overrides"></a>Přepsání filtru

Nyní můžete přepsat filtry, které platí pro danou akci metodu nebo řadiče zadáním [přepsání filtru](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). Zadejte filtry přepsání typy filtrů, které by neměl být spuštěn pro daný obor (akce nebo kontroleru). To umožňuje nastavit filtry, které platí globálně, ale pak vyloučit určité globální filtry z použití řadiče nebo konkrétní akce. V tématu [nové přepsání filtru funkce v ASP.NET MVC 5 a ASP.NET Web API 2](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [jak používat funkce technologie ASP.NET MVC 5 filtru přepsání](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), a [filtru přepsání v ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Atribut směrování

ASP.NET MVC nyní podporuje [směrováním atributů](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), díky příspěvek ve Tim McCall, Autor [http://attributerouting.net](http://attributerouting.net). Se směrováním atributů můžete zadat trasy zadávání poznámek k akce a kontrolery.

## <a name="new-web-project-experience"></a>Nové prostředí webového projektu

Budeme mít rozšířené možnosti vytvoření nové webové projekty v sadě Visual Studio 2013. V **nový webový projekt ASP.NET** dialogovém okně můžete vybrat typ projektu, nakonfigurovat libovolnou kombinaci technologie (webových formulářů, MVC, Web API), nakonfigurujte možnosti ověřování a přidejte projektu testování částí.

![Nový projekt ASP.NET](mvc5/_static/image1.png)

Dialogové okno Nový umožňuje změnit výchozí možnosti ověřování pro řadu šablony. Například při vytváření projektu webových formulářů ASP.NET můžete vybrat některý z následujících možností:

- Bez ověřování
- Jednotlivých uživatelských účtů (členství technologie ASP.NET nebo sociální zprostředkovatele přihlášení)
- Účty organizace (Active Directory v aplikaci internet)
- Ověřování systému Windows (Active Directory v intranetu aplikace)

![Možnosti ověřování](mvc5/_static/image2.png)

Další informace o nový proces pro vytváření webových projektů najdete v tématu [vytváření webové projekty ASP.NET v sadě Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Další informace o nové možnosti ověřování najdete v tématu [ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET generování uživatelského rozhraní

Generování uživatelského rozhraní ASP.NET je architektura generování kódu pro webové aplikace ASP.NET. Usnadňuje přidat často používaný kód do projektu, který komunikuje s datovým modelem.

V předchozích verzích sady Visual Studio generování uživatelského rozhraní omezovala na projekty ASP.NET MVC. Visual Studio 2013 můžete nyní pomocí generování uživatelského rozhraní pro všechny projekt ASP.NET, včetně webových formulářů. Visual Studio 2013 pro projekt webové formuláře aktuálně nepodporuje generování stránek, ale když můžete nadále používat generování uživatelského rozhraní s webovými formuláři tak, že přidáte do projektu závislosti MVC. Podpora pro generování stránky pro webové formuláře bude přidána v budoucí aktualizaci.

Při použití generování uživatelského rozhraní, je zajištěno, že všechny požadované závislosti jsou nainstalované v projektu. Například když začít s projektem webových formulářů ASP.NET a přidání Kontroleru webového rozhraní API pomocí generování uživatelského rozhraní, požadované balíčky NuGet a odkazy se přidají do projektu automaticky.

Chcete-li přidat generování uživatelského rozhraní MVC do projektu webových formulářů, přidejte **novou vygenerovanou položku** a vyberte **závislosti MVC 5** v dialogovém okně. Existují dvě možnosti pro generování uživatelského rozhraní MVC; Minimální a úplné. Pokud vyberete minimální, jenom balíčky NuGet a odkazy pro architekturu ASP.NET MVC se přidají do projektu. Pokud vyberete možnost Úplná minimální závislosti přidají, a také požadované soubory obsahu pro projekt MVC.

Podpora pro generování uživatelského rozhraní asynchronní řadiče využívá nové funkce asynchronní z Entity Framework 6.

Další informace a podrobné pokyny najdete v tématu [generování uživatelského rozhraní ASP.NET: Přehled](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="getting-help-and-reporting-issues"></a>Získání nápovědy a hlášení problémů

- [Známé problémy a narušující změny seznamu](../visual-studio/overview/2013/release-notes.md#knownissues)
- Získání nápovědy a popisují ASP.NET MVC 5 v [fóra](https://forums.asp.net/1146.aspx)
- [Sestavy chyb v ASP.NET MVC 5](https://github.com/aspnet/AspNetWebStack/issues)
- [Vytvoření žádosti o funkce](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>Upgrade z architektury ASP.NET MVC 4

V tématu [postup upgradu ASP.NET MVC 4 a webového projektu rozhraní API a rozhraní Web API 2 ASP.NET MVC 5](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
