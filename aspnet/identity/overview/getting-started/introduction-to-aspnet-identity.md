---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: "Úvod do ASP.NET Identity | Microsoft Docs"
author: jongalloway
description: "Systém členství technologie ASP.NET byla zavedena v systému technologii ASP.NET 2.0 zpět 2005 a od pak byly mnoho změny v typicall způsoby webové aplikace..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 7c7dcb7903b0d0772acc560161ff39c6869c599a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-aspnet-identity"></a>Úvod do ASP.NET Identity
====================
podle [Jon Galloway](https://github.com/jongalloway), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

> Systém členství technologie ASP.NET byla zavedená s prostředím ASP.NET 2.0 back 2005 a od pak byly provedeny změny v mnoha způsoby webové aplikace obvykle zpracovávat ověřování a autorizace. ASP.NET Identity je nový pohled na co systém členství musí být při vytváření moderní aplikace pro web, telefon nebo tablet.
> 
> Tento článek napsal Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Jon Galloway ([@jongalloway](https://twitter.com/jongalloway)), tní Dykstra a Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="background-membership-in-aspnet"></a>Pozadí: Členství technologie ASP.NET

### <a name="aspnet-membership"></a>Členství technologie ASP.NET

[Členství technologie ASP.NET](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) byl proto, aby vyřešila požadavky na členství lokality, které byly v 2005, které se podílejí ověřování pomocí formulářů a databázi systému SQL Server pro uživatelská jména, hesla a data profilu běžné. Dnes je mnohem širší pole možnosti úložiště dat pro webové aplikace a Většina vývojářů chcete povolit jejich lokality používat sociální identity zprostředkovatele pro funkci ověřování a autorizace. Omezení návrhu členství technologie ASP.NET velmi obtížné tento přechod:

- Schéma databáze je navržené pro SQL Server a nelze ji změnit. Můžete přidat informace o profilu, ale další data nachází v balení do jiné tabulky, což znesnadňuje pro přístup k jakýmkoli způsobem s výjimkou prostřednictvím rozhraní API poskytovatele profilu.
- Systému zprostředkovatele umožňuje změnit úložiště zálohování dat, ale v systému je uspořádaná kolem předpoklady, které jsou vhodné pro relační databázi. Můžete napsat zprostředkovatele uložit informace o členství v mechanismus nerelační úložiště, jako je například úložiště tabulek Azure, ale pak budete muset obejít relační návrhu pomocí zápisu velké množství kódu a spoustu `System.NotImplementedException` výjimky pro metody, které nejsou platí pro databáze NoSQL.
- Vzhledem k tomu, že funkce protokolu nebo odhlášení je založena na ověřování pomocí formulářů, nemůžete použít systém členství [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN obsahuje komponenty middlewaru pro ověřování, včetně podpory pro přihlašovací údaje pomocí poskytovatelů identit externí (např. Accounts Microsoft, Facebook, Google, Twitter) a přihlašovací údaje pomocí účty organizace z místní služby Active Directory nebo Azure Active Directory. OWIN zahrnuje taky podporu pro OAuth 2.0 JWT a CORS.

### <a name="aspnet-simple-membership"></a>ASP.NET jednoduché členství

[Jednoduché členství technologie ASP.NET](../../../web-pages/overview/security/16-adding-security-and-membership.md) byl vyvinut jako systém členství pro ASP.NET Web Pages. Byl vydán pomocí služby WebMatrix a Visual Studio 2010 SP1. Cílem jednoduché členství bylo snadné přidání funkcí členství do aplikace Web Pages.

Jednoduché členství snadněji přizpůsobit informace o profilu uživatele, ale stále sdílí jiné problémy s členství technologie ASP.NET a má určitá omezení:

- Bylo obtížné zachovat data systému členství v nerelační úložišti.
- Nemůžete ji použít s OWIN.
- Předchozí postup nebude fungovat správně s existující zprostředkovatele členství technologie ASP.NET a není rozšiřitelný.

### <a name="aspnet-universal-providers"></a>ASP.NET Universal Providers

[Balíček ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) byly vyvinuty, aby bylo možné zachovat informace o členství ve službě Microsoft Azure SQL Database a také pracovat se systém SQL Server Compact. Universal Providers byly založený na Entity Framework Code First, což znamená, že můžete uchovávat data v jakékoli úložiště nepodporuje EF použít Universal Providers. S Universal Providers schéma databáze byla vyčištěna poměrně velké také.

Universal Providers jsou postavené na infrastrukturu členství technologie ASP.NET, takže stále provádění stejná omezení jako zprostředkovatel SqlMembership. To znamená byly navrženy pro relačních databází a je těžké si přizpůsobit informace o profilu a uživatele. Tito zprostředkovatelé pořád použít ověřování pomocí formulářů pro funkci přihlášení a odhlášení.

## <a name="aspnet-identity"></a>ASP.NET Identity

Scénáře technologie ASP.NET má jako členství vyvinuly v průběhu let, týmem technologie ASP.NET má mnoho vytvořeným ze zpětné vazby od zákazníků.

Za předpokladu, že uživatelé budou přihlašovat tak, že zadáte uživatelské jméno a heslo, které mají registrované ve vaší vlastní aplikaci již není platný. Na webu se stal více sociálních. Uživatelé jsou v reálném čase prostřednictvím sociálních kanálů, jako je Facebook, Twitter a jiné sociální weby interakci mezi sebou. Vývojáři mají uživatelé moci přihlásit pomocí svých sociálních identit, aby měli bohaté prostředí na svých webech. Systém moderní členství musíte povolit na základě přesměrování přihlašovací údaje pro zprostředkovatele ověřování, jako je Facebook, Twitter a dalších.

Jako vyvinuly vývoj webů, takže se vzory vývoj webů. Jednotka testování kódu aplikace se aktivovala základní problém pro vývojáře aplikací. Technologie ASP.NET v 2008 přidat nové rozhraní na základě vzoru Model-View-Controller (MVC), v části usnadňuje vývojářům vytvářet jednotky možností intenzivního testování aplikací ASP.NET. Vývojáři, kteří chtěli jednotky otestovat jejich aplikační logiku také chtěli moci provést pomocí systému členství.

S ohledem na tyto změny v vývoj webových aplikací ASP.NET Identity vyvinula sledovat tyto cíle:

- **Jeden systém ASP.NET Identity**

    - ASP.NET Identity můžete použít se všemi rozhraní ASP.NET, jako je ASP.NET MVC, webových formulářů, webové stránky, webové rozhraní API a SignalR.
    - ASP.NET Identity lze použít při vytváření webové, telefon, úložiště nebo hybridní aplikace.
- **Snadné zapojení data profilu o uživateli**

    - Budete mít kontrolu nad schéma uživatele a informace o profilu. Například můžete snadno povolit systém k ukládání dat narození uživatele zadané při registraci účtu v aplikaci.

- **Trvalost ovládací prvek**

    - Ve výchozím nastavení ukládá systému ASP.NET Identity všechny informace o uživateli v databázi. ASP.NET Identity používá Entity Framework Code First k implementaci všech svůj mechanismus trvalosti.
    - Vzhledem k tomu, že můžete řídit schématu databáze, běžné úkoly, jako je například změna názvy tabulek nebo změna datový typ primárních klíčů je jednoduše provést.
    - Je snadné zařaďte jiného úložiště mechanismy, například SharePoint, služby Table úložiště Azure, databáze NoSQL, atd., aniž by museli throw `System.NotImplementedExceptions` výjimky.
- **Možnosti testování částí**

    - ASP.NET Identity vytvoří webovou aplikaci další jednotky možností intenzivního testování. Můžete napsat testy částí pro částí aplikace, které používají identitu ASP.NET.
- **Zprostředkovatele rolí**

    - Je role zprostředkovatele, který vám umožňuje omezit přístup částí aplikace tak, že role. Můžete snadno vytvořit role, jako je "Admin" a přidání uživatelů do rolí.
- **Na základě deklarace identity**

    - ASP.NET Identity podporuje ověřování na základě deklarace, kde je identita uživatele reprezentován jako sadu deklarací identity. Deklarace identity umožňují vývojářům být mnohem víc výrazovou v popisující identity uživatele, než dovolují role. Zatímco členství role je pouze logická hodnota (člen nebo třetí), deklarace identity může obsahovat bohaté informace o členství a identity uživatele.
- **Zprostředkovatelé přihlášení prostřednictvím sociální sítě**

    - Můžete snadno přidávat sociální plug-in protokolu například Account Microsoft, Facebook, Twitter, Google a dalších do vaší aplikace a ukládání dat specifických pro uživatele ve vaší aplikaci.
- **Azure Active Directory**

    - Můžete také přidat funkci přihlášení pomocí služby Azure Active Directory a ukládání dat specifických pro uživatele ve vaší aplikaci. Další informace najdete v tématu [účty organizace](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) v vytváření webové projekty ASP.NET v sadě Visual Studio 2013
- **Integrace OWIN**

    - Ověřování pomocí technologie ASP.NET je nyní podle middleware OWIN, který lze použít na všechny hostitele na základě OWIN. ASP.NET Identity nemá žádné závislosti na System.Web. Je plně kompatibilní rozhraní OWIN a lze použít v jakékoli aplikaci OWIN hostované.
    - ASP.NET Identity používá ověřování OWIN protokolu protokolu rezervace uživatelů na webu. To znamená, že místo použití ověřování pomocí formulářů pro generování souboru cookie, aplikace bude používat OWIN CookieAuthentication to provést.
- **Balíček NuGet**

    - ASP.NET Identity je znovu distribuován jako balíčku NuGet, který je nainstalován v šablony ASP.NET MVC, webových formulářů a webového rozhraní API, které dodávají spolu s Visual Studio 2013. Tento balíček NuGet můžete stáhnout z Galerie NuGet.
    - Uvolnění ASP.NET Identity jako NuGet balíček usnadňuje týmem ASP.NET k iteraci v nové funkce a opravy chyb a poskytovat tyto pro vývojáře pro agilní způsobem.

## <a name="getting-started-with-aspnet-identity"></a>Začínáme s ASP.NET Identity

ASP.NET Identity se používá v šablony projektů Visual Studio 2013 pro rozhraní ASP.NET MVC, webových formulářů, webového rozhraní API a SPA. V tomto návodu jsme budete znázorňují použití ASP.NET Identity šablony projektu k přidání funkcí registrace, přihlášení a odhlášení uživatele.

ASP.NET Identity je implementovaná pomocí následujícího postupu. Účelem tohoto článku je získáte nejvyšší úrovni přehled o ASP.NET Identity; můžete ji krok za krokem podle nebo právě přečtěte si podrobnosti. Podrobnější pokyny k vytváření aplikací pomocí ASP.NET Identity, včetně použití nového rozhraní API pro přidání uživatelů rolí a informace o profilu, najdete v části Další kroky na konci tohoto článku.

1. Vytvořte aplikaci ASP.NET MVC jednotlivé účty. Můžete použít ASP.NET Identity v architektuře ASP.NET MVC, webových formulářů, webového rozhraní API, SignalR atd. V tomto článku jsme se spustí se aplikace ASP.NET MVC.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. Vytvořený projekt obsahuje následující tři balíčky pro ASP.NET Identity.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
 Tento balíček obsahuje implementace Entity Framework ASP.NET Identity, který se uchová ASP.NET Identity dat a schématu na SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
 Tento balíček má základní rozhraní pro ASP.NET Identity. Tento balíček lze zapsat implementace pro ASP.NET Identity, že trvalost jiné cíle ukládá jako je například Azure Table Storage, NoSQL databáze atd.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
 Tento balíček obsahuje funkce, které se používá k zařaďte ověřování OWIN s ASP.NET Identity v aplikacích ASP.NET. Používá se při přidání protokolu do funkce aplikace a volání do middleware ověřování souborů Cookie OWIN ke generování souboru cookie.
3. Vytváření uživatele.  
 Spuštění aplikace a potom klikněte na **zaregistrovat** odkaz pro vytvoření uživatele. Následující obrázek ukazuje na stránku registrace, který shromažďuje uživatelské jméno a heslo.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
 Když uživatel klikne **zaregistrovat** tlačítko `Register` akce řadiče účtu vytvoří uživatele voláním rozhraní API ASP.NET Identity, jak je znázorněno níže:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Přihlásit se.  
 Pokud byl uživatel vytvořen úspěšně, uživatel přihlášen pomocí `SignInAsync` metoda.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

 Zvýrazněný výš v `SignInAsync` metoda generuje [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Vzhledem k tomu, že jsou založené na deklaracích identity systému ASP.NET Identity a ověřování souborů Cookie OWIN, rozhraní framework vyžaduje, aby aplikace ke generování ClaimsIdentity pro uživatele. ClaimsIdentity obsahuje informace o všech deklarací identity pro uživatele, například jaké role uživatel patří. V této fázi můžete také přidat další deklarace pro uživatele.  
  
 Zvýrazněný níže v `SignInAsync` metoda přihlášení uživatele pomocí třídě z OWIN a volání `SignIn` a předávání v ClaimsIdentity.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. Odhlaste se.  
 Kliknutím **Odhlásit** odkaz volá odhlášení akce v kontroleru účtu. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

 Zvýrazněných code výš ukazuje OWIN `AuthenticationManager.SignOut` metoda. Toto je obdobou [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) metodu používanou [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modulu ve webových formulářů.

## <a name="components-of-aspnet-identity"></a>Součástí ASP.NET Identity

Následující diagram znázorňuje komponenty ASP.NET Identity systému (klikněte na [to](introduction-to-aspnet-identity/_static/image3.png) nebo v diagramu a zvětšete je). Balíčky zeleně tvoří systém identit technologie ASP.NET. Všechny balíčky jsou závislosti, které jsou potřeba k použití systém identit technologie ASP.NET v aplikacích ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

Následuje stručný popis balíčků NuGet není již bylo zmíněno dříve:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Middleware, který umožňuje aplikaci používat soubor cookie na základě ověřování, podobné technologie ASP. Ověřování pomocí formulářů pro Asp.net.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Technologie přístup doporučené dat společnosti Microsoft pro relačních databází je rozhraní Entity Framework.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Migrace z členství na identitě ASP.NET Identity

Věříme, že brzy poskytovat pokyny k migraci vaší existující aplikace, které používají členství technologie ASP.NET nebo jednoduché členství na nový systém ASP.NET Identity.

## <a name="next-steps"></a>Další kroky

- [Vytvoření aplikace ASP.NET MVC 5 s použitím Facebook a Google OAuth2 a přihlašování OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 Tento kurz používá rozhraní API ASP.NET Identity k přidání informace o profilu k uživatelské databázi a jak k ověřování pro Google a sítě Facebook.
- [Vytvoření aplikace ASP.NET MVC pomocí ověřování a databázi SQL a nasazení do Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 Tento kurz ukazuje, jak použijete rozhraní API Identity k přidání uživatelů a rolí.
- [Jednotlivých uživatelských účtů](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth) při vytváření ASP.NET – webové projekty v sadě Visual Studio 2013
- [Účty organizace](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) při vytváření ASP.NET – webové projekty v sadě Visual Studio 2013
- [Přizpůsobení informací o profilu v identitě ASP.NET Identity šablony VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [Další informace získáte ze sociálních sítí, které se používají v rámci šablon projektu VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Ukázkovou aplikaci, která ukazuje, jak přidat základní role a podpora uživatelů a jak chcete role a správu uživatelů.
