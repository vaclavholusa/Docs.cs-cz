---
uid: identity/overview/getting-started/aspnet-identity-recommended-resources
title: ASP.NET Identity – doporučené zdroje | Dokumentace Microsoftu
author: Rick-Anderson
description: Toto téma obsahuje odkazy na dokumentaci o tom, jak používat ASP.NET Identity. Pokud znáte skvělé blogový příspěvek, vlákno na stackoverflow nebo jiných ext odkazu...
ms.author: riande
ms.date: 04/09/2015
ms.assetid: 0f78aec2-f509-46fa-b20f-d5208425d8ec
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-recommended-resources
msc.type: authoredcontent
ms.openlocfilehash: 9937003a23d018ffd7fb4a837fca9d39dcc2a89f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753018"
---
<a name="aspnet-identity-recommended-resources"></a>ASP.NET Identity – doporučené prostředky
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> Toto téma obsahuje odkazy na dokumentaci o tom, jak používat ASP.NET Identity.
> 
> Pokud znáte skvělé blogový příspěvek, [stackoverflow](http://stackoverflow.com) vlákna nebo odkaz, který může být užitečné, [nám pošlete e-mailu](mailto:aspnetue@microsoft.com?subject=Identity recommended resources) s odkazem nebo jen opustit zprávu v dolní části této stránky.


- [Začínáme s ASP.NET Identity](#gettingstarted)
- [Nové Doporučené články musí pro čtení](#feat)
- [Zprostředkující ASP.NET Identity](#adv)
- [Videa](#video)
- [Pokud chcete klást otázky, požádat o funkce, nahlaste jim chybu a denně automatizovaných buildů](#samp)
- [Blogové příspěvky o Identity](#blog)
- [Poskytovatelé vlastního úložiště pro ASP.NET Identity](#cust)
- [Další Identity prostředky](#additional)
- [Q &amp; (otázek a odpovědí)](#qand)

<a id="gettingstarted"></a>
## <a name="getting-started-with-aspnet-identity"></a>Začínáme s ASP.NET Identity

- [Aplikace MVC 5 s Facebook, Twitter, LinkedIn a Google OAuth2 přihlašování](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) v tomto kurzu se dozvíte, jak psát aplikace ASP.NET MVC 5 s autorizací Facebook nebo Google OAuth 2. Také ukazuje, jak přidat další data do databáze nástroje Identity.
- [Nasaďte zabezpečené aplikace ASP.NET MVC pomocí členství, OAuth a SQL Database na Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). V tomto kurzu přidá nasazení v Azure, jak zabezpečit vaši aplikaci s rolemi, jak můžete členské rozhraní API přidávat uživatele a role a další funkce zabezpečení.
- [Úvod do ASP.NET Identity](introduction-to-aspnet-identity.md)
- [Vytvoření zabezpečené webové aplikace ASP.NET MVC 5 s přihlášením, resetovat heslo a potvrzení e-mailu](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)
- [Aplikace ASP.NET MVC 5 s dvoufaktorovým ověřováním pomocí SMS a e-mailu](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)

<a id="feat"></a>
## <a name="new-featured-must-read-articles"></a>Nové Doporučené články musí pro čtení

- [Návod: Identitu ASP.NET MVC pomocí ověřování pomocí účtu Microsoft](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/) podle [Benjamin den](http://www.benday.com/about/)
- [ASP.NET Identity 2.0 rozšíření Identity modely a pomocí klíče celého čísla namísto řetězců](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
- [Ověřování pomocí tokenu AngularJS pomocí technologie ASP.NET Web API 2, Owin a Identity](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Thinktecture.IdentityManager jako náhrada WSAT](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [Identit technologie ASP.NET 2.0: Přizpůsobení uživatelů a rolí](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)

<a id="adv"></a>
## <a name="intermediate-aspnet-identity"></a>Zprostředkující ASP.NET Identity

- [Potvrzení účtu a obnovení hesla s ASP.NET Identity](../features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity](../features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Migrace stávajícího webu z členství SQL na ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Přidání ASP.NET Identity do prázdného nebo stávajícího projektu webových formulářů](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md)
- Zpravodaj MSDN Magazine [externího ověřování s ASP.NET Identity](https://msdn.microsoft.com/magazine/dn745860.aspx) podle Dino Esposito
- Zpravodaj MSDN Magazine[první pohled na ASP.NET Identity](https://msdn.microsoft.com/magazine/dn605872.aspx) podle Dino Esposito
- [ASP.NET Identity – uzamčení uživatelů](http://tech.trailmax.info/2014/06/asp-net-identity-user-lockout/)

<a id="samp"></a>
## <a name="where-to-ask-questions-request-features-report-a-bug-and-nightly-builds"></a>Pokud chcete klást otázky, požádat o funkce, nahlaste jim chybu a denně automatizovaných buildů

- StackOverflow, použijte značku [aspnet identity](http://stackoverflow.com/questions/tagged/asp.net-identity)
- Fóra ASP.NET účtovat [fórum o zabezpečení](https://forums.asp.net/25.aspx) a přidejte **ASP.NET Identity** na titulek.
- [ASP.NET Identity na Githubu](https://github.com/aspnet/AspNetIdentity) získání denně automatizovaných buildů, žádosti o funkce, otevřete chyby.

<a id="blog"></a>
## <a name="blog-posts-on-identity"></a>Blogové příspěvky o Identity

- [Co je SecurityStamp v ASP.NET Identity?](http://stackoverflow.com/questions/19487322/what-is-asp-net-identitys-iusersecuritystampstoretuser-interface/19505060#19505060)
- Podle [Atten Jan](https://twitter.com/xivSolutions)

    - [ASP.NET Identity 2.0 rozšíření Identity modely a pomocí klíče celého čísla namísto řetězců](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
    - [Identit technologie ASP.NET 2.0: Přizpůsobení uživatelů a rolí](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)
    - [ASP.NET MVC a Identity 2.0: Základy práce s](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)
    - [Nastavení ověření účtu a dvoufaktorového ověřování](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)
    - [Konfigurace připojení databáze a migrace Code First pro účty Identity v architektuře ASP.NET MVC 5 a Visual Studio 2013](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)
- Podle [Taiseer Joudeh](http://bitoftech.net/taiseer-joudeh-blog/)

    - [Token na základě ověřování pomocí technologie ASP.NET Web API 2, Owin middleware a ASP.NET Identity](http://bitoftech.net/2014/06/01/token-based-authentication-asp-net-web-api-2-owin-asp-net-identity/)
    - [Ověřování pomocí tokenu AngularJS pomocí technologie ASP.NET Web API 2, Owin a Identity](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
    - [Povolte aktualizace tokenů OAuth do aplikace AngularJS s použitím rozhraní ASP .NET Web API 2 a Owin – část 3.](http://bitoftech.net/2014/07/16/enable-oauth-refresh-tokens-angularjs-app-using-asp-net-web-api-2-owin/)
- Podle [Anders Abel](https://twitter.com/anders_abel)

    - [Vysvětlení kanálu Owin externí ověřování](http://coding.abel.nu/2014/06/understanding-the-owin-external-authentication-pipeline/)
    - [ASP.NET Identity a přehled Owin](http://coding.abel.nu/2014/06/asp-net-identity-and-owin-overview/)

  Podle [K. Scott Allen](https://twitter.com/OdeToCode) na kód do kódu

    - [ASP.NET Core Identity](http://odetocode.com/blogs/scott/archive/2013/11/25/asp-net-core-identity.aspx) tento blog prozkoumá základní abstrakce, včetně IUser, úložiště IUserStore a I\*Store rozhraní.
    - [ASP.NET Identity s rozhraním Entity Framework](http://odetocode.com/blogs/scott/archive/2014/01/03/asp-net-identity-with-the-entity-framework.aspx) jednotlivých uživatelských účtů v aplikacích MVC 5, webové rozhraní API a jednostránková aplikace, připojovacích řetězců a správu kontextů
    - [Možnosti přizpůsobení s ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/09/customization-options-with-asp-net-identity.aspx)
    - [Implementace ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- [Den Benjamin](http://www.benday.com/about/)[názorný postup: identitu ASP.NET MVC pomocí ověření účtu Microsoft](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/)
- [Společnosti Brock Allen](https://twitter.com/BrockLAllen)

    - [O externí přihlášení zprostředkovatele (přihlašování přes sociální sítě) s ověřovacím middlewarem OWIN a Katana](http://brockallen.com/2014/01/09/a-primer-on-external-login-providers-social-logins-with-owinkatana-authentication-middleware/)
    - [Představujeme IdentityReboot](http://brockallen.com/2014/02/11/introducing-identityreboot/): sada rozšíření ASP.NET Identity, které implementují hlavní chybějící funkce jsem jste podány stížnosti o.
- [Pranav Rastogi](https://twitter.com/rustd)

    - [Získat více informací ze sociálních sítí poskytovatele](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [@beabigrockstar](https://twitter.com/beabigrockstar) (Jerrie Pelser)

    - [2 Multi-Factor authentication](http://www.beabigrockstar.com/blog/2-factor-authentication-with-asp-net-identity-2-0-beta-1/)
    - [Pomocí služby Google Authenticator s ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)
    - [Provede ověření ASP.NET MVC 5](http://www.beabigrockstar.com/)
- [Další informace získáte z poskytovatelé služeb sociálních sítí použít v šablonách projektů VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [Vytváření jednoduchou aplikaci seznamu úkolů s ASP.NET Identity a uživatelům přidružení ToDoes](https://blogs.msdn.com/b/webdev/archive/2013/10/20/building-a-simple-todo-application-with-asp-net-identity-and-associating-users-with-todoes.aspx)
- [Problémy s integrací služby Google OpenId s ASP.NET Identity](http://blog.technovert.com/2014/01/google-openid-integration-issues-asp-net-identity/) Pokud dojde k chybě: Chyba 404.15 HTTP – nebyl nalezen modul filtrování žádostí je konfigurován k odepření požadavku, ve kterém je moc dlouhý řetězec dotazu
- [Thinktecture.IdentityManager jako náhrada WSAT](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [Ověřování pomocí tokenu AngularJS pomocí technologie ASP.NET Web API 2, Owin a Identity](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Jednoduché identitu Asp.net Core bez rozhraní Entity Framework](https://code.msdn.microsoft.com/Simple-Aspnet-Identiy-Core-7475a961)
- [Práce s rolemi v ASP.NET Identity pro MVC](http://www.dotnetfunda.com/articles/show/2898/working-with-roles-in-aspnet-identity-for-mvc) podle [Sheo Narayan](http://www.dotnetfunda.com/profile/sheonarayan.aspx)
- [Přesun z členství technologie ASP.NET na ASP.NET Identity](http://webdojo.sharepoint.com/ajmatthews/_layouts/15/start.aspx#/Lists/Posts/Post.aspx?ID=2) podle Alistair Matthews

<a id="video"></a>
## <a name="videos"></a>Videa

- Na webu Channel 9 [zabezpečení aplikací ASP.NET a služby: Facelift zabezpečení pro moderní aplikace](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B421#fbid=PhVT9E1WRtr?hashlink=fbid) podle Ido Flatow
- Na webu Channel 9 [ASP.NET Identity ÚVOD](https://channel9.msdn.com/Events/dotnetConf/2014/ASP-NET-Identity-Security) napsal Pranav Rastogi
- Channel 9 [ověřování ASP.NET pomocí technologie ASP.NET Identity](https://channel9.msdn.com/Shows/Web+Camps+TV/Special-Movember-Episode-ASPNET-Authentication-Provider) podle Cory fowler vám
- Na webu Channel 9 [vytváření moderních webových aplikací: ASP.NET Identity](https://channel9.msdn.com/Series/Building-Modern-Web-Apps/03) podle Koch Jan
- Channel 9 [zabezpečení svůj web s ASP.NET Identity](https://channel9.msdn.com/Events/TechDays/Techdays-2014-the-Netherlands/Securing-your-website-with-ASP-NET-Identity) podle Alex Thissen
- [Použití ASP.NET Identity na existující DB – Model](https://www.youtube.com/watch?v=elfqejow5hM) podle Alexander Schmidt
- [ASP.NET jedné Identity](https://www.youtube.com/watch?v=w8GD-QIusKk) podle Ivaylo Kenov Telerik
- [Čeština ASP.NET Identity](https://www.youtube.com/watch?v=tVbZp5brcpY) v této přednášce vám ukážeme postup nasazení základního ověřování, jak přidat podporu pro externí identity poskytovatelů, jako je Twitter nebo Facebook a použití jednorázového hesla (OTP). [ASP.NET Identity je nástupce členství v roli zprostředkovatele&#367; v technologii ASP.NET, tedy knihovna pro zajišt&#283;ní autentizace uživatel&#367;. V této p&#345;ednášce si ukážeme, jak nasad]

<a id="cust"></a>
## <a name="custom-storage-providers-for-aspnet-identity"></a>Poskytovatelé vlastního úložiště pro ASP.NET Identity

Pokud chcete napsat vlastního zprostředkovatele, přečtěte si [přehled o vlastní poskytovatelé úložiště pro ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md) a [implementace ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx) a pak zkontrolujte zdroj uvedené projekty OSS níže.

- Kurz: [přehled poskytovatelů vlastního úložiště pro ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md) podle Tom FitzMacken
- Blog: [implementace ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Kurz:[nastavením základní Identity účtů a odkazující na externí DB](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Podle [ @xivSolutions ](https://twitter.com/xivSolutions).
- Kurz[: implementace poskytovatele úložiště vlastní MySQL ASP.NET Identity](../extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Entity CodeFluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) podle [SoftFluent](http://www.softfluent.com/)
- [Azure Table Storage](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) podle James Randall.
- Azure Table Storage: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) podle [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant podle Wertheim ADAM.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Elastické ledat[h: elastické Identity](https://github.com/bmbsqd/elastic-identity) podle Bombsquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) podle Jonathan Sheely Jonathan Sheely.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) podle Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) podle [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) podle [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Šablony T4 pro generování EF code pro úložiště "databáze nejprve" uživatele: [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)

<a id="additional"></a>
## <a name="additional-aspnet-identity-resources"></a>Zdroje dalších ASP.NET Identity

- [Představení zprostředkovatele zabezpečení Yahoo a OAuth pro LinkedIn pro OWIN](http://blog.beabigrockstar.com/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) podle Jerrie Pelser pokyny Yahoo a LinkedIn.

<a id="qand"></a>
## <a name="qampa-questionanswer"></a>Q&amp;(otázek a odpovědí)

- D: uzamčené si uživatelé, kteří mají povolené "pamatovat si mě" (aby se nemuseli 2FA a absolvovat na počítači a prohlížeče) není uzamčen. Proč a jak zabráním, který? Odpověď [tady](http://stackoverflow.com/questions/24312247/locked-out-users-can-login-if-they-have-auth-cookie).
- **Q**: jak lze ukládat vlastní deklarace identity, jako je například skutečné jméno uživatele, v souboru cookie ASP.NET Identity zabránit zbytečných databázové dotazy u každého požadavku. Odpověď [tady](http://stackoverflow.com/questions/23622047/identity-cookie-loses-custom-claim-information-after-a-period-of-time).
- **Hodnota Hash hesla d: aktualizace AspNetUser**: je nutné 2 projekty. Jeden z nich používá ověřování pomocí technologie ASP.NET, druhé používá ověřování Windows, což je Správa na straně. Chci, aby správce projekt tak, aby mohli spravovat uživatele z druhé. Můžete upravit všechno kromě hesla [Odpovědět zde](http://stackoverflow.com/questions/23880666/updating-aspnetuser-password-hash).
- **Q**: jak můžete resetovat heslo správce pro ostatní uživatele? Odpověď [tady](http://stackoverflow.com/questions/23783249/identity-2-0-reset-password-by-admin/24211766#24211766).
- **Q**: můžete změnit zobrazovaný název pole uživatelské jméno v ASP.NET MVC IdentityUser? Odpověď [tady](http://stackoverflow.com/questions/23256650/can-i-change-the-displayed-name-of-the-username-field-in-asp-net-mvc-identityuse).
- **Q**: Jak můžu gran uživatelům oprávnění k přidání dalších uživatelů do určité role? Odpověď [tady](http://stackoverflow.com/questions/23695373/allow-users-to-grant-permissions-to-other-users-for-their-account-in-asp-net-ide).
- **Q**: ukládání informací o profilu v tabulce AspNetUsers vs. AspNetUserClaims tabulky. Odpověď [tady](http://stackoverflow.com/questions/23215727/is-there-any-benefit-to-storing-user-information-in-aspnetuserclaims-with-asp-ne).
- **Q**: Zapamatovat uživatele při použití zprostředkovatele externího ověřování. Odpověď [tady](http://stackoverflow.com/questions/23180896/how-to-remember-the-login-in-mvc5-when-an-external-provider-is-used).
- **Q**: Proč každý požadavek vyžaduje ApplicationDBContext, který není příliš mnoho režie?. Odpověď, ne, režie je nízký.
- Otázka: Jak získat seznam přihlášeným uživatelům? Odpověď [tady](http://stackoverflow.com/questions/22995653/getting-a-list-of-logged-in-users-in-asp-net-identity/).
- Dotaz: Jak mohu zjistit, když se uživatel přihlásí pomocí Microsoft.AspNet.Identity? Odpověď [tady](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698).
- Otázka: Jak získat lokalizované chybové zprávy pro identitu? Odpověď [tady](http://stackoverflow.com/questions/22835981/asp-net-identity-localization-publickeytoken/22845864#22845864).
- Otázka: Jak můžu nakonfigurovat CookieMiddleware získat novou deklarací každých 30 minut? Odpověď [tady](http://stackoverflow.com/questions/22682663/how-to-hold-the-cookies-claims-updated-with-mcv5-owin/22796932#22796932).
- Otázka: jak změnit deklarace pro uživatele po jejich přihlášení? Odpověď [tady](http://stackoverflow.com/questions/22768836/can-i-modify-claims-in-asp-net-identity-with-owin-after-calling-signin/22769963#22769963).
- Otázka: jak se platnost tokenů zabezpečení? Odpověď [tady](http://stackoverflow.com/questions/22755700/revoke-token-generated-by-usertokenprovider-in-asp-net-identity-2-0/22767286#22767286).
- Otázka: jak je úložiště deklarací identity v middlewaru souboru cookie? Odpověď [tady](http://stackoverflow.com/questions/22320632/storing-retrieving-user-data-without-database-when-using-owin-cookie-authenticat/22541856#22541856).
- Dotaz: Chci kódu PIN nebo zabezpečení, podívejte se na každou metodu akce v aplikaci MVC, ale rád bych k ukládání úspěšných uživatelů, tak si uživatelé nebudou muset zadat kód PIN u každého požadavku k této metodě akce. Odpověď [tady](http://stackoverflow.com/questions/22479958/security-check-an-user-to-a-access-controller-action/22486075#22486075).
- Otázka: nemůžu chtěli do uložit adresu vrácených e-mailu z poskytovatele sociálních sítí do databáze, jak to mám udělat? Odpověď [tady](http://stackoverflow.com/questions/22888397/save-claims-to-db-on-login/22970969#22970969):
- Otázka: jak lze zjistit když se uživatel přihlásí v obou s s výstupech soubor cookie "pamatovat si mě"? Odpověď [tady](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698).
- Dotaz: lze změnit deklarace identity v ASP.NET Identity s OWIN po volání metody přihlašování? Odpověď: Voláním SignIn je přesně to co mají dělat, když chcete změnit deklarace pro uživatele. V podstatě způsobí, že objekt ClaimsIdentity k serializaci do souboru cookie, což je důvod, proč se zobrazí nová deklarace identity, zobrazí na následné žádosti.
