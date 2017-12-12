---
uid: web-api/overview/security/individual-accounts-in-web-api
title: "Zabezpečení webového rozhraní API s jednotlivých účtů a místní přihlášení v rozhraní ASP.NET Web API 2.2 | Microsoft Docs"
author: MikeWasson
description: "Toto téma ukazuje, jak zabezpečit webové rozhraní API pomocí OAuth2 k ověřování na základě databáze členství. Použité v kurzu Visual Studio 201 verze softwaru..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2014
ms.topic: article
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 8207df79c1e915b33a0ba095d917a6dc69550173
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Zabezpečení webového rozhraní API s jednotlivých účtů a místní přihlášení v rozhraní ASP.NET Web API 2.2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhnout ukázkové aplikace](https://github.com/MikeWasson/LocalAccountsApp)

> Toto téma ukazuje, jak zabezpečit webové rozhraní API pomocí OAuth2 k ověřování na základě databáze členství.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Webové rozhraní API 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


Ve Visual Studiu 2013 se šablona projektu webového rozhraní API jsou tři možnosti pro ověřování:

- **Jednotlivé účty.** Aplikace používá databázi členství.
- **Účty organizace.** Uživatelé přihlásit pomocí svých Azure Active Directory, Office 365 nebo pověření na místní služby Active Directory.
- **Ověřování systému Windows.** Tato možnost je určena pro intranetové aplikace a používá modul IIS ověřování systému Windows.

Další podrobnosti o těchto možnostech najdete v tématu [vytváření webové projekty ASP.NET v sadě Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Samostatné účty obsahují dva způsoby pro uživatele k přihlášení:

- **Místní přihlášení**. Uživatel zaregistruje v lokalitě, zadáte uživatelské jméno a heslo. Aplikace ukládá hodnotu hash hesla v databázi členství. Když se uživatel přihlásí, systém ASP.NET Identity ověří heslo.
- **Přihlášení prostřednictvím sociální sítě**. Přihlášení uživatele pomocí externí služby, jako je Facebook, Microsoft nebo Google. Aplikace stále vytvoří položka pro uživatele v databázi členství, ale nejsou uložené žádné přihlašovací údaje. Uživatel se ověřuje po přihlášení k externí služby.

Tento článek vypadá na scénář místní přihlášení. Pro přihlášení k místním i sociálních webového rozhraní API používá OAuth2 k ověřování žádostí. Toky přihlašovacích údajů se ale liší pro místní a sociálních přihlášení.

V tomto článku I budete ukazují jednoduchou aplikaci, která umožňuje uživateli přihlásit a odesílat ověřených volání AJAX do webového rozhraní API. Si můžete stáhnout ukázkový kód [zde](https://github.com/MikeWasson/LocalAccountsApp). V souboru readme popisuje postup vytvoření vzorku od začátku je nutné v sadě Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

Ukázková aplikace používá Knockout.js pro datové vazby a jQuery pro odesílání požadavků AJAX. I budete se zaměřením na volání AJAX, takže není nutné znát Knockout.js k tomuto článku.

Na této cestě I budete popisují:

- Jaké aplikace se provádí na straně klienta.
- Co se děje na serveru.
- Přenos HTTP uprostřed.

Nejprve musíme definovat některé terminologie OAuth2.

- *Prostředek*. Některá část dat, jež lze chránit.
- *Server prostředků*. Server, který je hostitelem prostředku.
- *Vlastník prostředku*. Typ entity, která můžete udělit oprávnění pro přístup k prostředkům. (Obvykle uživatel.)
- *Klient*: aplikace, která chce získat přístup k prostředku. V tomto článku klient je webový prohlížeč.
- *Přístupový token*. Token, který uděluje přístup k prostředku.
- *Token nosiče*. Konkrétní typ přístupový token, s vlastností, že každý, kdo použití tokenu. Jinými slovy klient nevyžaduje kryptografického klíče nebo další tajný klíč pro použití tokenu nosiče. Z tohoto důvodu nosné tokeny musí být použit pouze HTTPS a musí mít vypršení platnosti poměrně krátké doby.
- *Autorizace serveru*. Server, který poskytuje se přístupové tokeny.

Aplikace může fungovat jako server ověřování a server prostředků. Šablona projektu webového rozhraní API následuje tohoto vzoru.

## <a name="local-login-credential-flow"></a>Tok přihlašovacích údajů místního přihlášení

Pro místní přihlášení, webového rozhraní API používá [toku heslo vlastníka prostředku](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) definované v OAuth2.

1. Uživatel zadá své jméno a heslo do klienta.
2. Klient odešle tyto přihlašovací údaje serveru ověřování.
3. Server ověřování ověřuje přihlašovací údaje a vrátí přístupový token.
4. Pro přístup k chráněnému prostředku, klient zahrne přístupový token v hlavičce autorizace požadavku HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

Když vyberete **jednotlivých účtů** v šabloně projektu webového rozhraní API, tento projekt zahrnuje autorizaci serveru, který ověří přihlašovací údaje uživatele a vydává tokeny. Následující diagram znázorňuje stejného toku přihlašovacích údajů z hlediska součásti webového rozhraní API.

![](individual-accounts-in-web-api/_static/image4.png)

V tomto scénáři webového rozhraní API řadiče fungují jako servery prostředků. Filtr ověřování ověří přístupových tokenů a **[Authorize]** atribut slouží k ochraně prostředku. Pokud má kontroler nebo akce **[Authorize]** atribut, všechny požadavky na tento kontroler nebo akce musí být ověřeny. Jinak je Autorizace odepřena a webového rozhraní API vrátí chyba 401 (Neautorizováno).

Server ověřování a ověřování filtr, jak provádět volání do [OWIN middleware](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) komponenty, která zpracovává podrobnosti o OAuth2. Později v tomto kurzu I budete popisují návrhu podrobněji.

## <a name="sending-an-unauthorized-request"></a>Odesílání neoprávněného požadavku

Abyste mohli začít, spusťte aplikaci a klikněte na tlačítko **volání rozhraní API** tlačítko. Po dokončení žádosti se měl zobrazit chybovou zprávu ve **výsledek** pole. Je to způsobeno žádost neobsahuje přístupový token, takže požadavek není autorizovaný.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

**Volání rozhraní API** tlačítko odešle požadavek AJAX ~/api nebo hodnoty, které vyvolá akce kontroleru webového rozhraní API. Zde je v části kódu jazyka JavaScript, který odesílá požadavek AJAX. V ukázkové aplikace všechny aplikace kód jazyka JavaScript se nachází v souboru Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Dokud se uživatel přihlásí, neexistuje žádný token nosiče a proto žádné autorizační hlavičky v požadavku. To způsobí, že žádost o vrátit chyba 401.

Zde je požadavek HTTP. (Použití [Fiddler](http://www.telerik.com/fiddler) pro zachycení přenosu dat protokolu HTTP.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Odpověď HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Všimněte si, že odpověď obsahuje hlavičku Www-Authenticate s otázkou, nastavte na nosiče. Označuje, že server očekává token nosiče.

## <a name="register-a-user"></a>Registrace uživatele

V **zaregistrovat** část aplikace, zadejte e-mailu a heslo a klikněte na tlačítko **zaregistrovat** tlačítko.

Nemusíte používat platnou e-mailovou adresu pro tuto ukázku, ale skutečná aplikace by potvrďte adresu. (Viz [vytvořit zabezpečený webovou aplikaci ASP.NET MVC 5 s protokolu v e-mailu potvrzení a heslo resetovat](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Pro heslo použijte něco jako "Heslo1!", velké písmeno, malé písmeno, číslo a jiné než alfanumerické znaky. Pro zjednodušení aplikace I vynecháno ověřování na straně klienta, tak, pokud dojde k problému s formátem heslo, abyste dosáhli Chyba 400 (Chybný požadavek).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

**Zaregistrovat** tlačítko odešle požadavek POST do ~/api/Account/Register /. Text žádosti je objekt JSON, která obsahuje jméno a heslo. Tady je kód JavaScript, který odešle žádost:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Požadavek HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Odpověď HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Tento požadavek se zpracovává souborem `AccountController` třídy. Interně `AccountController` používá ASP.NET Identity ke správě této databáze členství.

Pokud aplikaci spouštíte místně ze sady Visual Studio, uživatelské účty jsou uloženy v LocalDB, v tabulce AspNetUsers. Chcete-li zobrazit tabulky v sadě Visual Studio, klikněte na tlačítko **zobrazení** nabídce vyberte možnost **Průzkumníka serveru**, pak rozbalte **datová připojení**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Získání tokenu přístupu

Zatím jsme neudělali žádné OAuth, ale nyní jsme zobrazí serveru ověřování OAuth v akci, žádosti o token přístupu. V **protokolu v** oblasti ukázková aplikace, zadejte e-mailu a heslo a klikněte na tlačítko **protokolu v**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

**Protokolu v** tlačítko odešle požadavek na koncový bod tokenu. Text žádosti obsahuje následující data formuláře kódovaná adresy url:

- udělit\_typu: "password"
- uživatelské jméno: &lt;e-mail uživatele&gt;
- heslo: &lt;heslo&gt;

Tady je kód jazyka JavaScript, který odesílá požadavek AJAX:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Pokud neproběhne úspěšně, server ověřování vrátí přístupový token v textu odpovědi. Všimněte si, uložíme token v relaci úložiště pro pozdější použití při odesílání požadavků na rozhraní API. Na rozdíl od některých způsoby ověřování (například ověřování na základě souborů cookie) v prohlížeči nebudou obsahovat automaticky přístupový token v následných žádostí. Aplikace musí učinit explicitně. Který je dobrá vlastnost, protože se omezuje [ohrožení zabezpečení proti útokům CSRF](preventing-cross-site-request-forgery-csrf-attacks.md).

Požadavek HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Uvidíte, že požadavek obsahuje přihlašovacích údajů uživatele. Můžete *musí* zabezpečení transportní vrstvy pomocí protokolu HTTPS.

Odpověď HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Čitelnější I odsazeny JSON a zkrácen přístupový token, který je poměrně dlouho.

`access_token`, `token_type`, A `expires_in` vlastnosti jsou definované specifikací OAuth2. Ostatní vlastnosti (`userName`, `.issued`, a `.expires`) jsou jenom pro informační účely. Kód, který přidává tyto další vlastnosti v najdete `TokenEndpoint` metoda v souboru /Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Odeslat požadavek na ověřeného

Teď, když máme token nosiče, bychom mohli požadavek na ověřeného rozhraní API. To se provádí nastavením autorizační hlavičky v požadavku. Klikněte **volání rozhraní API** tlačítko vidět.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Požadavek HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Odpověď HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Odhlášení

Protože prohlížeče neukládá do mezipaměti přihlašovací údaje nebo přístupový token, protokolování je pouze záležitost "zapomenutí" token, odebráním z relace úložiště:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Vysvětlení jednotlivých účtů v šabloně projektů

Když vyberete **jednotlivých účtů** v šabloně projektů webové aplikace ASP.NET, tento projekt zahrnuje:

- Serveru ověřování OAuth2.
- Koncový bod webové rozhraní API pro správu uživatelských účtů
- Model EF pro ukládání uživatelských účtů.

Zde jsou hlavní aplikace třídy, které implementují tyto funkce:

- `AccountController`. Obsahuje koncový bod webové rozhraní API pro správu uživatelských účtů. `Register` Akce je pouze ten, který jsme použili v tomto kurzu. Další metody pro třídu podporují resetování hesla, sociálních přihlášení a další funkce.
- `ApplicationUser`, definované v /Models/IdentityModels.cs. Tato třída je model EF pro uživatelské účty v databázi členství.
- `ApplicationUserManager`, definované v /App\_Start/IdentityConfig.cs Tato třída odvozená z [objekt UserManager](https://msdn.microsoft.com/en-us/library/dn613290.aspx) a provádí operace v uživatelských účtech, jako je například vytváření nového uživatele, ověření hesla a tak dále a automaticky přetrvává. změny databáze.
- `ApplicationOAuthProvider`. Tento objekt zapojení do middlewaru OWIN a zpracovává událostí vyvolaných middlewarem. Je odvozena z [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/en-us/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Konfigurace serveru ověřování

Následující kód v StartupAuth.cs, konfiguruje serveru ověřování OAuth2.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

`TokenEndpointPath` Vlastnost je cesta URL k serveru koncového bodu autorizace. Je adresa URL, která používá tuto aplikaci se získat token nosiče.

`Provider` Vlastnost určuje zprostředkovatele, který se připojuje k middlewaru OWIN a zpracuje událostí vyvolaných middlewarem.

Tady je základní postup, když aplikace chce získat token:

1. Chcete-li získat přístupový token, aplikace odešle požadavek na ~ / Token.
2. Volání middleware OAuth `GrantResourceOwnerCredentials` na poskytovateli.
3. Volání zprostředkovatele `ApplicationUserManager` ověřte pověření a vytvoření identity deklarací.
4. Pokud tato operace úspěšná, vytvoří zprostředkovatele lístek ověřování, který se používá k vygenerování tokenu.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

OAuth middleware neví nic o uživatelské účty. Zprostředkovatel komunikováním se mezi middleware a ASP.NET Identity. Další informace o implementaci serveru ověřování najdete v tématu [serveru OWIN OAuth 2.0 ověřování](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Konfigurace webového rozhraní API, aby používala tokeny nosičů

V `WebApiConfig.Register` metoda, následující kód nastaví ověřování pro kanál rozhraní Web API:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

**HostAuthenticationFilter** třída umožňuje ověřování pomocí nosných tokenů.

**SuppressDefaultHostAuthentication** metoda informuje webového rozhraní API ignorovat jakékoli ověřování, který se stane, než požadavek dosáhne kanál rozhraní Web API pomocí služby IIS nebo pomocí middlewaru OWIN. Tímto způsobem jsme můžete omezit webového rozhraní API k ověřování jenom pomocí nosných tokenů.

> [!NOTE]
> Na konkrétní části MVC aplikace může použít ověřování pomocí formulářů, která ukládá přihlašovací údaje do souboru cookie. Ověřování na základě souboru cookie vyžaduje použití tokenů proti padělání, aby se zabránilo útokům proti útokům CSRF. Problém pro webové rozhraní API, který je, protože neexistuje žádný pohodlný způsob pro webové rozhraní API k odeslání token proti padělání do klienta. (Pro další informace o tomto problému, viz [prevence proti útokům CSRF útoků v rozhraní Web API](preventing-cross-site-request-forgery-csrf-attacks.md).) Volání metody **SuppressDefaultHostAuthentication** zajistí, že webového rozhraní API není bude zranitelný vůči útoku proti útokům CSRF z přihlašovací údaje uložené v souborech cookie.


Když klient požádá o chráněný prostředek, stane se v kanálu webového rozhraní API:

1. **HostAuthentication** filtru volá OAuth middleware pro ověření tokenu.
2. Middleware převede tokenu deklarací identity.
3. V tomto okamžiku se jedná o žádost *ověřený* ale ne *oprávnění*.
4. Filtr autorizace prověří identitu deklarací. Pokud deklarace identity autorizovat uživatele pro tento prostředek, je požadavek autorizován. Ve výchozím nastavení **[Authorize]** atribut ověření jakékoli požadavky, které je ověřen. Nicméně můžete povolit roli nebo další deklarace identity. Další informace najdete v tématu [ověřování a autorizace v rozhraní Web API](authentication-and-authorization-in-aspnet-web-api.md).
5. Pokud předchozí kroky úspěšné, vrátí kontroleru k chráněnému prostředku. Klient obdrží, jinak chyba 401 (Neautorizováno).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Další prostředky

- [ASP.NET Identity](../../../identity/index.md)
- [Princip funkce zabezpečení v šabloně SPA pro VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). MSDN blog o Hongye post Sun.
- [Rozvěrače webové rozhraní API jednotlivých účtů šablony – část 2: místní účty](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Příspěvek blogu podle Dominick Baier.
- [Hostování ověřování a webového rozhraní API s OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Dobrý vysvětlení `SuppressDefaultHostAuthentication` a `HostAuthenticationFilter` ve společnosti Brock Allen.
- [Přizpůsobení informací o profilu v identitě ASP.NET Identity VS 2013 šablony](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). MSDN příspěvku na blogu podle Pranav Rastogi.
- [Za správu životního cyklu požadavek pro objekt UserManager třídu v identitě ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). Příspěvek blogu MSDN podle Suhas Joshi s funkčním vysvětlení `UserManager` třídy.
