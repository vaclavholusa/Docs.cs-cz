---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Zabezpečení webového rozhraní API s jednotlivých účtů a místní přihlášení v rozhraní ASP.NET Web API 2.2 | Dokumentace Microsoftu
author: MikeWasson
description: Toto téma ukazuje, jak zabezpečit webové rozhraní API pomocí OAuth2 k ověřování na základě databáze členství. Verze softwaru používaných pro kurz 201 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2014
ms.topic: article
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: f37cb206b78fd46835bb59bede5920f8e5acd3fd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399935"
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Zabezpečení webového rozhraní API s jednotlivých účtů a místní přihlášení v rozhraní ASP.NET Web API 2.2
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhněte si ukázkovou aplikaci](https://github.com/MikeWasson/LocalAccountsApp)

> Toto téma ukazuje, jak zabezpečit webové rozhraní API pomocí OAuth2 k ověřování na základě databáze členství.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Web API 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


V sadě Visual Studio 2013 šablonu projektu webového rozhraní API nabízí tři možnosti pro ověřování:

- **Samostatné účty.** Aplikace používá databáze členství.
- **Účty organizace.** Uživatelé, přihlaste se pomocí jejich Azure Active Directory, Office 365 nebo přihlašovací údaje místní služby Active Directory.
- **Ověřování Windows.** Tato možnost je určená pro intranetové aplikace a používá modul IIS ověřování Windows.

Další podrobnosti o těchto možnostech najdete v tématu [vytváření webových projektů ASP.NET v sadě Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Jednotlivé účty poskytují dva způsoby, jak uživatele k přihlášení:

- **Místní přihlášení**. Uživatel zaregistruje v lokalitě, zadáním uživatelského jména a hesla. Aplikace ukládá hodnota hash hesla v databázi členství. Když se uživatel přihlásí, systém ASP.NET Identity ověří heslo.
- **Přihlášení prostřednictvím sociální sítě**. Přihlášení uživatele pomocí externí služby, jako je Facebook, Microsoft nebo Google. Aplikace se stále vytvoří záznam pro uživatele v databázi členství, ale nejsou uložené žádné přihlašovací údaje. Uživatel se ověřuje pomocí přihlášení k externí službě.

Tento článek ukazuje scénář místní přihlášení. Místní a sociální přihlášení webové rozhraní API používá OAuth2 k ověření požadavků. Toky přihlašovacích údajů se však liší pro sociálních sítí a místní přihlášení.

V tomto článku vám předvedu jednoduchou aplikaci, která umožňuje uživatelům přihlášení a odesílání prováděl ověřená volání AJAX do webového rozhraní API. Můžete stáhnout ukázkový kód [tady](https://github.com/MikeWasson/LocalAccountsApp). Soubor readme popisuje postup vytvoření ukázky od začátku v sadě Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

Ukázková aplikace používá rozhraní Knockout.js pro vázání dat a jQuery pro odesílání požadavků AJAX. Můžu budete se zaměřit na volání AJAX, takže není nutné znát rozhraní Knockout.js pro účely tohoto článku.

Na cestě můžu popisu použijeme:

- Co si aplikace vede na straně klienta.
- Co se děje na serveru.
- Přenos pomocí protokolu HTTP uprostřed.

Nejprve musíme definovat některé terminologie OAuth2.

- *Prostředek*. Některá část dat, která se dají chránit.
- *Server prostředků*. Server, který je hostitelem prostředku.
- *Vlastník prostředku*. Entita, která můžete udělit oprávnění pro přístup k prostředku. (Obvykle uživatel.)
- *Klient*: aplikace, která požaduje přístup k prostředku. V tomto článku klient je webový prohlížeč.
- *Přístupový token*. Token, který uděluje přístup k prostředku.
- *Nosný token*. Konkrétního typu přístupový token s vlastností, kdo použít token. Jinými slovy klient nemusí kryptografického klíče nebo další tajný kód k použití nosného tokenu. Z tohoto důvodu nosné tokeny by měla sloužit pouze prostřednictvím protokolu HTTPS a musí mít vypršení platnosti poměrně krátké doby.
- *Autorizační server*. Server, který poskytuje si přístupové tokeny.

Aplikace může fungovat jako autorizační server a server prostředků. Šablona projektu webového rozhraní API používá tento vzor.

## <a name="local-login-credential-flow"></a>Tok přihlašovacích údajů místního přihlášení

Místní přihlášení, webové rozhraní API používá [tok heslo vlastníka prostředku](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) definované v OAuth2.

1. Uživatel zadá své jméno a heslo do klienta.
2. Klient odešle tyto přihlašovací údaje k autorizačnímu serveru.
3. Autorizační server ověří přihlašovací údaje a vrátí přístupový token.
4. Klient pro přístup k chráněnému prostředku, zahrne přístupový token v hlavičce autorizace požadavku HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

Když vyberete **jednotlivých účtů** v šabloně projektu webového rozhraní API projekt obsahuje autorizační server, který ověří přihlašovací údaje uživatele a vystavuje tokeny. Následující diagram znázorňuje stejný tok přihlašovacích údajů z hlediska součásti webového rozhraní API.

![](individual-accounts-in-web-api/_static/image4.png)

V tomto scénáři kontrolerů rozhraní Web API fungují jako servery prostředků. Filtr ověřování ověří přístupové tokeny a **[Authorize]** atribut se používá pro ochranu prostředku. Pokud má kontroler nebo akce **[Authorize]** atribut, všechny požadavky na tento kontroler nebo akce musí být ověřené. V opačném případě je Autorizace odepřena, a webového rozhraní API vrací chybu 401 (Neautorizováno).

Autorizační server a filtr ověřování, jak provádět volání do [OWIN middleware](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) komponenta, která zpracovává podrobnosti protokolu oauth2. Můžu budete popisují návrhu podrobněji dále v tomto kurzu.

## <a name="sending-an-unauthorized-request"></a>Odesílání neoprávněného požadavku

Abyste mohli začít, spusťte aplikaci a klikněte na tlačítko **volání rozhraní API** tlačítko. Po dokončení žádosti zobrazí chybovou zprávu ve **výsledek** pole. Důvodem je, žádost neobsahuje přístupový token, tak, že žádost neoprávněná.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

**Volání rozhraní API** tlačítko odešle požadavek AJAX ~/api/hodnota, která vyvolá akci kontroleru webového rozhraní API. Tady je část kódu jazyka JavaScript, který odešle požadavek AJAX. V ukázkové aplikaci veškerý kód JavaScript aplikace se nachází v souboru Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Dokud se uživatel přihlásí, není žádný nosný token a proto žádné autorizační hlavičky v požadavku. To způsobí, že mohl požadavek vrátit chyba 401.

Tady je požadavek HTTP. (Jsem [Fiddler](http://www.telerik.com/fiddler) pro zachycení provozu HTTP.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Odpověď HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Všimněte si, že odpověď obsahuje hlavičku Www-Authenticate s otázkou, nastavte na nosiče. Označuje, že server očekává nosný token.

## <a name="register-a-user"></a>Registrace uživatele

V **zaregistrovat** části aplikace, zadejte e-mailu a heslo a klikněte na tlačítko **zaregistrovat** tlačítko.

Není nutné používat platnou e-mailovou adresu pro tuto ukázku, ale skutečná aplikace by potvrďte adresu. (Viz [vytvořit zabezpečenou webovou aplikaci ASP.NET MVC 5 s přihlášením, resetovat heslo a potvrzení e-mailu](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Zadání hesla pomocí něčeho jako "Heslo1!", velké písmeno, malé písmeno, číslo a jiné než alfanumerické znaky. Pro zjednodušení aplikace mi zbývá si ověřování na straně klienta, takže pokud je nějaký problém s formát hesla, zobrazí se vám Chyba 400 (Chybný požadavek).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

**Zaregistrovat** tlačítka se odešle požadavek POST do ~/api/Account/Register /. Text požadavku je objekt JSON, který obsahuje uživatelské jméno a heslo. Tady je kód jazyka JavaScript, která odešle žádost:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Požadavek protokolu HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Odpověď HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Tento požadavek zařizuje služba `AccountController` třídy. Interně `AccountController` ASP.NET Identity používá ke správě databáze členství.

Pokud aplikaci spouštíte místně v sadě Visual Studio, uživatelské účty jsou uloženy v LocalDB, v tabulce AspNetUsers. Mají tabulky zobrazit v sadě Visual Studio, klikněte na tlačítko **zobrazení** nabídce vyberte možnost **Průzkumníka serveru**, potom rozbalte **datová připojení**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Získání přístupového tokenu

Zatím jsme neprovedli jakékoli OAuth, ale teď jsme zobrazí autorizační server OAuth v akci, když jsme žádost o přístupový token. V **přihlásit** oblasti v ukázkové aplikaci, zadejte e-mailu a heslo a klikněte na tlačítko **přihlásit**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

**Přihlásit** tlačítko odešle požadavek na koncový bod tokenu. Text žádosti obsahuje následující data formuláře kódovaná adresy url:

- udělit\_typ: "password"
- uživatelské jméno: &lt;e-mail uživatele&gt;
- heslo: &lt;heslo&gt;

Tady je kód jazyka JavaScript, který odešle požadavek AJAX:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Pokud je žádost úspěšná, vrátí autorizační server přístupový token v textu odpovědi. Všimněte si, že uložíme tokenu relace úložiště pro pozdější použití při odesílání požadavků na rozhraní API. Na rozdíl od některé typy ověřování (jako je například ověřování na základě souborů cookie) v prohlížeči nezahrnuje automaticky přístupového tokenu v následných žádostí. Aplikaci musíte to udělat explicitně. To je dobrá věc, protože omezuje [ohrožení zabezpečení CSRF](preventing-cross-site-request-forgery-csrf-attacks.md).

Požadavek protokolu HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Uvidíte, že žádost obsahuje přihlašovacích údajů uživatele. Můžete *musí* poskytuje vrstvu zabezpečení přenosu pomocí protokolu HTTPS.

Odpověď HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Pro lepší čitelnost můžu odsazena ve formátu JSON a přístupový token, který je poměrně dlouhých zkrácena.

`access_token`, `token_type`, A `expires_in` vlastnosti jsou definované specifikací OAuth2. Ostatní vlastnosti (`userName`, `.issued`, a `.expires`) jsou pouze k informačním účelům. Kód, který přidá tyto další vlastnosti v můžete najít `TokenEndpoint` metody v souboru /Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Poslat žádost o ověření

Když teď máme nosný token, bychom mohli ověřeného požadavku rozhraní API. To se provádí nastavením hlavičku autorizace v požadavku. Klikněte na tlačítko **volání rozhraní API** znovu, abyste viděli toto tlačítko.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Požadavek protokolu HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Odpověď HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Odhlásit se

Vzhledem k tomu, že v prohlížeči neukládá do mezipaměti přihlašovací údaje nebo přístupového tokenu, odhlášení je jednoduše otázkou "zapomínání" token, tak, že odeberete ze služby storage relace:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Vysvětlení jednotlivých účtů šablony projektu

Když vyberete **jednotlivých účtů** v šabloně projektu webové aplikace technologie ASP.NET obsahuje projekt:

- Serveru ověřování OAuth2.
- Koncový bod webové rozhraní API pro správu uživatelských účtů
- Modelu EF pro ukládání uživatelských účtů.

Tady jsou hlavní aplikace třídy, které implementují tyto funkce:

- `AccountController`. Poskytuje koncový bod webové rozhraní API pro správu uživatelských účtů. `Register` Akce je pouze jeden, který jsme použili v tomto kurzu. Ostatní metody ve třídě podporu resetování hesla, přihlašování přes sociální sítě a další funkce.
- `ApplicationUser`, definované v /Models/IdentityModels.cs. Tato třída je EF model pro uživatelské účty v databázi členství.
- `ApplicationUserManager`, definované v /App\_Start/IdentityConfig.cs Tato třída je odvozena z [objektu UserManager](https://msdn.microsoft.com/library/dn613290.aspx) a provádí operace u uživatelských účtů, jako je vytvoření nového uživatele, ověření hesla a tak dále a automaticky opakuje. změny v databázi.
- `ApplicationOAuthProvider`. Tento objekt zpřístupní OWIN middleware a zpracování událostí vyvolaných middlewarem. Se odvozuje od [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Konfigurace serveru ověřování

V StartupAuth.cs následující kód konfiguruje serveru ověřování OAuth2.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

`TokenEndpointPath` Vlastnost je cesta URL pro koncový bod autorizace serveru. Adresa URL, které je tato aplikace používá k získání nosné tokeny.

`Provider` Vlastnost určuje zprostředkovatele, který zpřístupní OWIN middleware a zpracování událostí vyvolaných middlewarem.

Tady je základní tok, když aplikace chce získat token:

1. Chcete-li získat přístupový token aplikace odešle požadavek na ~ / Token.
2. Middleware volá OAuth `GrantResourceOwnerCredentials` na poskytovateli.
3. Volání zprostředkovatele `ApplicationUserManager` ověřit přihlašovací údaje a vytvoření identity deklarací.
4. Pokud tato operace úspěšná, poskytovatel vytvoří lístek ověřování, který se používá k vygenerování tokenu.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

OAuth middleware neví nic o uživatelské účty. Zprostředkovatel komunikováním se mezi middleware a ASP.NET Identity. Další informace o implementaci autorizační server najdete v tématu [OWIN OAuth 2.0 autorizační Server](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Konfigurace webového rozhraní API, aby používala tokeny nosičů

V `WebApiConfig.Register` metody následující kód nastaví ověřování pro kanál rozhraní Web API:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

**HostAuthenticationFilter** třída umožňuje ověřování pomocí nosných tokenů.

**SuppressDefaultHostAuthentication** metoda říká webového rozhraní API pro ignorování ověřování, který se stane, než požadavek dosáhne kanál rozhraní Web API, služby IIS nebo OWIN middleware. Tímto způsobem můžeme omezit webové rozhraní API ověřování jenom pomocí nosných tokenů.

> [!NOTE]
> Konkrétně se část aplikace MVC použít ověřování pomocí formulářů, které ukládá přihlašovací údaje do souboru cookie. Ověřování na základě souboru cookie vyžaduje použití tokenů proti padělání, prevenci proti útokům CSRF. To je problém pro webová rozhraní API, protože neexistuje žádný pohodlný způsob pro webové rozhraní API k odeslání do tokenu proti zfalšování klientovi. (Další informace o tomto problému naleznete v tématu [prevence útoků CSRF v rozhraní Web API](preventing-cross-site-request-forgery-csrf-attacks.md).) Volání **SuppressDefaultHostAuthentication** zajistí, že webové rozhraní API není zranitelný vůči útokům CSRF z přihlašovací údaje uložené v souborech cookie.


Když klient požádá o chráněný prostředek, stane se v kanálu webového rozhraní API:

1. **HostAuthentication** filtr volá OAuth middleware pro ověření tokenu.
2. Middleware převede tokenu deklarací identity.
3. V tomto okamžiku je požadavek *ověření* , ale ne *oprávnění*.
4. Filtr autorizace prozkoumá deklarací identity. Pokud deklarace identity autorizace uživatelů pro daný prostředek, je požadavek autorizován. Ve výchozím nastavení **[Authorize]** atribut povolí všechny požadavky, které je ověřený. Můžete ale povolit podle role nebo další deklarace identity. Další informace najdete v tématu [ověřování a autorizace v rozhraní Web API](authentication-and-authorization-in-aspnet-web-api.md).
5. Pokud předchozí kroky jsou úspěšné, kontroler vrací chráněnému prostředku. V opačném případě klient obdrží chyba 401 (Neautorizováno).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Další prostředky

- [ASP.NET Identity](../../../identity/index.md)
- [Princip funkce zabezpečení v šabloně SPA for VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). MSDN blogovém příspěvku o Hongye Sun.
- [Rozbor webové rozhraní API jednotlivých účtů šablony – část 2: místní účty](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Příspěvek na blogu od Dominick Baier.
- [Hostování ověřování a webové rozhraní API s OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Dobrý popis `SuppressDefaultHostAuthentication` a `HostAuthenticationFilter` ve společnosti Brock Allen.
- [Přizpůsobení informací o profilu ASP.NET Identity v šablonách VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). Příspěvek blogu na webu MSDN napsal Pranav Rastogi.
- [Za správu životního cyklu požadavku pro třídu objektu UserManager. v ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). Příspěvek na blogu MSDN od Suhas Joshi dobré vysvětlením, `UserManager` třídy.
