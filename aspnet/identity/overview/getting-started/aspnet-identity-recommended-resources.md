---
uid: identity/overview/getting-started/aspnet-identity-recommended-resources
title: "ASP.NET Identity doporučené prostředky | Microsoft Docs"
author: Rick-Anderson
description: "Toto téma obsahuje odkazy na zdroje informací k dokumentaci o tom, jak používat ASP.NET Identity. Pokud znáte skvělé blogu, vlákno stackoverflow nebo jiných připoj..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2015
ms.topic: article
ms.assetid: 0f78aec2-f509-46fa-b20f-d5208425d8ec
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-recommended-resources
msc.type: authoredcontent
ms.openlocfilehash: 8050d37cfea91701e1cae0413feb41ca9a68baff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-identity-recommended-resources"></a>ASP.NET Identity doporučené prostředky
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> Toto téma obsahuje odkazy na zdroje informací k dokumentaci o tom, jak používat ASP.NET Identity.
> 
> Pokud znáte skvělé blogu, [stackoverflow](http://stackoverflow.com) vlákno nebo jiných odkaz, který by být užitečné, [pošlete nám e-mail](mailto:aspnetue@microsoft.com?subject=Identity recommended resources) s odkazem nebo jednoduše nechte zprávu v dolní části této stránky.


- [Začínáme s ASP.NET Identity](#gettingstarted)
- [Nové zajímavé články musí pro čtení](#feat)
- [Zprostředkující ASP.NET Identity](#adv)
- [Videa](#video)
- [Kde klást otázky, požadavky na funkce, ohlásí chybu a noční sestavení](#samp)
- [Příspěvky blogu o Identity](#blog)
- [Poskytovatelé vlastní úložiště pro identitu ASP.NET](#cust)
- [Další Identity prostředky](#additional)
- [Q &amp; (otázek a odpovědí)](#qand)

<a id="gettingstarted"></a>
## <a name="getting-started-with-aspnet-identity"></a>Začínáme s ASP.NET Identity

- [Aplikace MVC 5 se službou Facebook, Twitter, LinkedIn a přihlašování Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) tohoto kurzu se dozvíte, jak psát aplikace ASP.NET MVC 5 s ověřování sítě Facebook a Google OAuth 2. Také ukazuje, jak přidat další data do databáze Identity.
- [Nasazení aplikace zabezpečené rozhraní ASP.NET MVC s členství, OAuth a SQL Database k Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). V tomto kurzu přidá nasazení Azure, jak zabezpečit vaše aplikace s rolemi, jak používat členské rozhraní API k přidání uživatelů a rolí a další funkce zabezpečení.
- [Úvod do ASP.NET Identity](introduction-to-aspnet-identity.md)
- [Vytvoření zabezpečeného webovou aplikaci ASP.NET MVC 5 s přihlášení resetovat heslo a potvrzení e-mailu](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)
- [Aplikace ASP.NET MVC 5 s SMS a e-mailu dvoufaktorové ověřování](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)

<a id="feat"></a>
## <a name="new-featured-must-read-articles"></a>Nové zajímavé články musí pro čtení

- [Návod: Rozhraní ASP.NET MVC Identity s ověřováním účtu Microsoft](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/) podle [Robert den](http://www.benday.com/about/)
- [Modely Identity rozšíření identitu ASP.NET 2.0 a pomocí klíče celé číslo namísto řetězců](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
- [AngularJS tokenu ověřování pomocí technologie ASP.NET Web API 2, Owin a Identity](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Thinktecture.IdentityManager k nahrazení WSAT](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [Identitu ASP.NET 2.0: Přizpůsobení uživatelů a rolí](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)

<a id="adv"></a>
## <a name="intermediate-aspnet-identity"></a>Zprostředkující ASP.NET Identity

- [Potvrzení účtu a heslo pro obnovení s ASP.NET Identity](../features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity](../features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Migrace existující web z členství SQL na identitě ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Přidání ASP.NET Identity pro prázdný nebo existující webových formulářů projektu](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md)
- Časopis MSDN [externího ověřování s ASP.NET Identity](https://msdn.microsoft.com/en-us/magazine/dn745860.aspx) podle Dino Esposito
- Časopis MSDN[první pohled na identitě ASP.NET Identity](https://msdn.microsoft.com/en-us/magazine/dn605872.aspx) podle Dino Esposito
- [ASP.NET Identity – uzamčení uživatelů](http://tech.trailmax.info/2014/06/asp-net-identity-user-lockout/)

<a id="samp"></a>
## <a name="where-to-ask-questions-request-features-report-a-bug-and-nightly-builds"></a>Kde klást otázky, požadavky na funkce, ohlásí chybu a noční sestavení

- Pro StackOverflow, použijte značky [aspnet-identity](http://stackoverflow.com/questions/tagged/asp.net-identity)
- Fóra ASP.NET účtovat [fórum o zabezpečení](https://forums.asp.net/25.aspx) a přidejte **ASP.NET Identity** k titulu.
- [ASP.NET Identity na Githubu](https://github.com/aspnet/AspNetIdentity) Get noční sestavení pro žádosti o funkce, otevřete chyby.

<a id="blog"></a>
## <a name="blog-posts-on-identity"></a>Příspěvky blogu o Identity

- [Co je SecurityStamp v identitě ASP.NET Identity?](http://stackoverflow.com/questions/19487322/what-is-asp-net-identitys-iusersecuritystampstoretuser-interface/19505060#19505060)
- Podle [Atten Jan](https://twitter.com/xivSolutions)

    - [Modely Identity rozšíření identitu ASP.NET 2.0 a pomocí klíče celé číslo namísto řetězců](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
    - [Identitu ASP.NET 2.0: Přizpůsobení uživatelů a rolí](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)
    - [ASP.NET MVC a Identity 2.0: Principy základy](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)
    - [Nastavení ověření účtu a dvoufaktorové ověřování](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)
    - [Konfigurace připojení databáze a migrace Code First pro účty Identity v architektuře ASP.NET MVC 5 a Visual Studio 2013](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)
- Podle [Taiseer Joudeh](http://bitoftech.net/taiseer-joudeh-blog/)

    - [Token ověřování pomocí technologie ASP.NET Web API 2, Owin middleware a ASP.NET Identity](http://bitoftech.net/2014/06/01/token-based-authentication-asp-net-web-api-2-owin-asp-net-identity/)
    - [AngularJS tokenu ověřování pomocí technologie ASP.NET Web API 2, Owin a Identity](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
    - [Povolte tokeny OAuth aktualizovat v aplikaci AngularJS použití prostředí ASP .NET webové rozhraní API 2 a Owin – část 3.](http://bitoftech.net/2014/07/16/enable-oauth-refresh-tokens-angularjs-app-using-asp-net-web-api-2-owin/)
- Podle [Anders opisek](https://twitter.com/anders_abel)

    - [Principy kanálu Owin externí ověřování](http://coding.abel.nu/2014/06/understanding-the-owin-external-authentication-pipeline/)
    - [Přehled Owin a ASP.NET Identity](http://coding.abel.nu/2014/06/asp-net-identity-and-owin-overview/)

 Podle [K. Scott Allen](https://twitter.com/OdeToCode) na funkce Ode ke kódu

    - [ASP.NET Core Identity](http://odetocode.com/blogs/scott/archive/2013/11/25/asp-net-core-identity.aspx) tomto blogu prozkoumá základní abstrakce, včetně IUser, IUserStore a I\*ukládání rozhraní.
    - [ASP.NET Identity s platformou Entity Framework](http://odetocode.com/blogs/scott/archive/2014/01/03/asp-net-identity-with-the-entity-framework.aspx) jednotlivé uživatelské účty v aplikacích MVC 5, webového rozhraní API a SPA, připojovací řetězce a správu kontexty
    - [Možnosti přizpůsobení s ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/09/customization-options-with-asp-net-identity.aspx)
    - [Implementace technologie ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- [Robert den](http://www.benday.com/about/)[návod: Identity rozhraní ASP.NET MVC s ověřováním účtu Microsoft](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/)
- [Společnosti Brock Allen](https://twitter.com/BrockLAllen)

    - [Úvod do na externí přihlášení zprostředkovatelé (sociální přihlášení) s middleware ověřování OWIN nebo Katana](http://brockallen.com/2014/01/09/a-primer-on-external-login-providers-social-logins-with-owinkatana-authentication-middleware/)
    - [Představení IdentityReboot](http://brockallen.com/2014/02/11/introducing-identityreboot/): sada rozšíření pro ASP.NET Identity, které implementují funkce hlavní chybí I jste podány stížnosti o.
- [Pranav Rastogi](https://twitter.com/rustd)

    - [Další informace získáte z sociálních zprostředkovatelů](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [@beabigrockstar](https://twitter.com/beabigrockstar)(Jerrie Pelser)

    - [2 Multi-Factor authentication](http://www.beabigrockstar.com/blog/2-factor-authentication-with-asp-net-identity-2-0-beta-1/)
    - [Pomocí ASP.NET Identity Google Authenticator](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)
    - [Provede ověření rozhraní ASP.NET MVC 5](http://www.beabigrockstar.com/)
- [Další informace získáte ze sociálních sítí, které se používají v rámci šablon projektu VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [Vytvoření jednoduché aplikace ToDo s ASP.NET Identity a přidružení uživatelů k ToDoes](https://blogs.msdn.com/b/webdev/archive/2013/10/20/building-a-simple-todo-application-with-asp-net-identity-and-associating-users-with-todoes.aspx)
- [Problémy s integrací služby Google OpenId s ASP.NET Identity](http://blog.technovert.com/2014/01/google-openid-integration-issues-asp-net-identity/) Pokud dojde k chybě: Chyba 404.15 HTTP – nebyl nalezen modul filtrování požadavků je nakonfigurován tak, aby odepřel požadavek, jehož řetězec dotazu je příliš dlouhý
- [Thinktecture.IdentityManager k nahrazení WSAT](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [AngularJS tokenu ověřování pomocí technologie ASP.NET Web API 2, Owin a Identity](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Jednoduché Asp.net Identity Core bez rozhraní Entity Framework](https://code.msdn.microsoft.com/Simple-Aspnet-Identiy-Core-7475a961)
- [Práce s role v identitě ASP.NET Identity pro MVC](http://www.dotnetfunda.com/articles/show/2898/working-with-roles-in-aspnet-identity-for-mvc) podle [Sheo Narayan](http://www.dotnetfunda.com/profile/sheonarayan.aspx)
- [Přechod k identitě ASP.NET Identity z členství technologie ASP.NET](http://webdojo.sharepoint.com/ajmatthews/_layouts/15/start.aspx#/Lists/Posts/Post.aspx?ID=2) podle Alistair Matthewsovi

<a id="video"></a>
## <a name="videos"></a>Videa

- Channel 9 [zabezpečení aplikací a služeb ASP.NET: Facelift zabezpečení pro moderní aplikace](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B421#fbid=PhVT9E1WRtr?hashlink=fbid) podle Ido Flatow
- Channel 9 [ASP.NET Identity ÚVOD](https://channel9.msdn.com/Events/dotnetConf/2014/ASP-NET-Identity-Security) podle Pranav Rastogi
- Channel 9 [ověřování ASP.NET pomocí ASP.NET Identity](https://channel9.msdn.com/Shows/Web+Camps+TV/Special-Movember-Episode-ASPNET-Authentication-Provider) podle Cory Fowler
- Channel 9 [vytváření moderní webové aplikace: ASP.NET Identity](https://channel9.msdn.com/Series/Building-Modern-Web-Apps/03) podle Jeff Koch
- Channel 9 [zabezpečení vašeho webu s ASP.NET Identity](https://channel9.msdn.com/Events/TechDays/Techdays-2014-the-Netherlands/Securing-your-website-with-ASP-NET-Identity) podle Alex Thissen
- [Použít identitu ASP.NET na existující DB – Model](https://www.youtube.com/watch?v=elfqejow5hM) podle Alexander Schmidt
- [Identitu ASP.NET jeden](https://www.youtube.com/watch?v=w8GD-QIusKk) podle Ivaylo Kenov z webu Telerik
- [Čeština ASP.NET Identity](https://www.youtube.com/watch?v=tVbZp5brcpY) v této přednáškový ukážeme, jak nasadit základní ověřování, jak přidat podporu pro externí identity zprostředkovatele například Twitteru nebo Facebooku a způsob použití jednorázového hesla (OTP). [ASP.NET Identity je nástupce členství zprostředkovatele rolí &#367; v technologii ASP.NET, tedy knihovna pro zajišt &#283; ní autentizace uživatel &#367;. V této p &#345; ednášce si ukážeme, jak nasad]

<a id="cust"></a>
## <a name="custom-storage-providers-for-aspnet-identity"></a>Poskytovatelé vlastní úložiště pro identitu ASP.NET

Pokud chcete napsat vlastního zprostředkovatele, přečtěte si [přehled z vlastních poskytovatelů úložiště pro ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md) a [implementace ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx) a potom si prohlédněte zdroj jeden z uvedených operačních systémů projektů níže.

- Kurz: [přehled zprostředkovatelů vlastní úložiště pro ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md) podle tní FitzMacken
- Blog: [implementace ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Kurz:[nastavení účtů pro základní Identity a jejich odkazující na externí DB](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Podle [ @xivSolutions ](https://twitter.com/xivSolutions).
- Kurz[: implementace poskytovatele úložiště ASP.NET Identity vlastní MySQL](../extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Entity CodeFluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) podle [SoftFluent](http://www.softfluent.com/)
- [Azure Table Storage](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) podle James Novák.
- Azure Table Storage: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) podle [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant podle Wertheim ADAM.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Elastické Hle[h: elastické Identity](https://github.com/bmbsqd/elastic-identity) podle Bombsquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) podle Jonathan Sheely Jonathan Sheely.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) podle Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) podle [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) podle [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Šablony T4 ke generování EF code pro úložiště uživatele "databáze nejprve": [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)

<a id="additional"></a>
## <a name="additional-aspnet-identity-resources"></a>Další ASP.NET Identity prostředky

- [Představení zprostředkovatele zabezpečení Yahoo a OAuth pro LinkedIn pro OWIN](http://blog.beabigrockstar.com/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) podle Jerrie Pelser pokyny Yahoo a LinkedIn.

<a id="qand"></a>
## <a name="qampa-questionanswer"></a>Q&amp;(otázek a odpovědí)

- Otázka: uzamčen uživatelů, kteří mají povoleno "Zapamatovat uživatele" (tak nebudou muset projít 2FA na tento počítač nebo prohlížeč) není uzamčen. Proč a jak zabránit který? Odpověď [zde](http://stackoverflow.com/questions/24312247/locked-out-users-can-login-if-they-have-auth-cookie).
- **Q**: jak můžete uložit vlastní deklarace identity, jako je například skutečné jméno uživatele, souboru cookie ASP.NET Identity tak, aby se zabránilo zbytečným databázové dotazy na každý požadavek. Odpověď [zde](http://stackoverflow.com/questions/23622047/identity-cookie-loses-custom-claim-information-after-a-period-of-time).
- **Hodnota Hash hesla Otázka: aktualizace AspNetUser**: je nutné 2 projekty. Jeden z nich používá ověřování pomocí technologie ASP.NET, druhé používá ověřování systému Windows, který je na straně správy. Chci, aby správce projekt, který má být schopné spravovat uživatelé dalších. Můžete změnit všechno kromě heslo [Odpovězte si zde](http://stackoverflow.com/questions/23880666/updating-aspnetuser-password-hash).
- **Q**: jak můžete resetovat heslo jako správce pro ostatní uživatele? Odpověď [zde](http://stackoverflow.com/questions/23783249/identity-2-0-reset-password-by-admin/24211766#24211766).
- **Q**: můžete změnit zobrazovaný název pole uživatelské jméno v ASP.NET MVC IdentityUser? Odpověď [zde](http://stackoverflow.com/questions/23256650/can-i-change-the-displayed-name-of-the-username-field-in-asp-net-mvc-identityuse).
- **Q**: jak se dá gran uživatelům oprávnění k přidání jiných uživatelů do určité role? Odpověď [zde](http://stackoverflow.com/questions/23695373/allow-users-to-grant-permissions-to-other-users-for-their-account-in-asp-net-ide).
- **Q**: ukládání informací o profilu v tabulce AspNetUsers oproti AspNetUserClaims tabulky. Odpověď [zde](http://stackoverflow.com/questions/23215727/is-there-any-benefit-to-storing-user-information-in-aspnetuserclaims-with-asp-ne).
- **Q**: Zapamatovat uživatele při použití externí zprostředkovatel ověřování. Odpověď [zde](http://stackoverflow.com/questions/23180896/how-to-remember-the-login-in-mvc5-when-an-external-provider-is-used).
- **Q**: Proč každou žádost vyžaduje ApplicationDBContext, který není příliš mnoho režii?. Odpověď, ne, režii je nízký.
- Otázka: jak lze získat seznam přihlášeného uživatele? Odpověď [zde](http://stackoverflow.com/questions/22995653/getting-a-list-of-logged-in-users-in-asp-net-identity/).
- Otázka: Jak mohu zjistit, kdy se uživatel přihlásí pomocí Microsoft.AspNet.Identity? Odpověď [zde](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698).
- Otázka: Jak získat lokalizované chybové zprávy pro identitu? Odpověď [zde](http://stackoverflow.com/questions/22835981/asp-net-identity-localization-publickeytoken/22845864#22845864).
- Otázka: jak lze nakonfigurovat CookieMiddleware získat čerstvou deklarací každých 30 minut? Odpověď [zde](http://stackoverflow.com/questions/22682663/how-to-hold-the-cookies-claims-updated-with-mcv5-owin/22796932#22796932).
- Otázka: jak změnit deklarace pro uživatele po jejich přihlášení? Odpověď [zde](http://stackoverflow.com/questions/22768836/can-i-modify-claims-in-asp-net-identity-with-owin-after-calling-signin/22769963#22769963).
- Otázka: jak zneplatnit tokeny zabezpečení? Odpověď [zde](http://stackoverflow.com/questions/22755700/revoke-token-generated-by-usertokenprovider-in-asp-net-identity-2-0/22767286#22767286).
- Otázka: jak se deklarace identity úložiště v middlewaru souboru cookie? Odpověď [zde](http://stackoverflow.com/questions/22320632/storing-retrieving-user-data-without-database-when-using-owin-cookie-authenticat/22541856#22541856).
- Otázka: Chci kód PIN nebo zabezpečení, zkontrolujte na každou metodu akce v mé aplikace MVC, ale chcete ukládat úspěch uživatelé tak nebudou muset zadání kódu PIN u každého požadavku k této metodě akce. Odpověď [zde](http://stackoverflow.com/questions/22479958/security-check-an-user-to-a-access-controller-action/22486075#22486075).
- Otázka: I chcete k uložení Internet Protocol vrácený e-mailu ze sociálních zprostředkovatele k databázi, jakým způsobem který? Odpověď [sem](http://stackoverflow.com/questions/22888397/save-claims-to-db-on-login/22970969#22970969):
- Otázka: Jak mohu zjistit, kdy se uživatel přihlásí v obou s nebo s out soubor cookie "Zapamatovat uživatele"? Odpověď [zde](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698).
- Otázka: je možné po volání metody přihlášení úpravám deklarací identity v identitě ASP.NET Identity s OWIN? Odpověď: Volání přihlášení je přesně je mají udělat, když chcete upravit deklarace pro uživatele. V podstatě způsobí, že objekt ClaimsIdentity k serializaci do souboru cookie, proto se zobrazí nových deklarací identity objeví na následné žádosti.
