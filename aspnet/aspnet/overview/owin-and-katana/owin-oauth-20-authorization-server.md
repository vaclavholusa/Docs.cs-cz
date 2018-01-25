---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: "Serveru ověřování OAuth 2.0 OWIN | Microsoft Docs"
author: hongyes
description: "V tomto kurzu pokyny o tom, jak implementovat serveru autorizace OAuth 2.0 pomocí middlewaru OWIN OAuth. To je pokročilé kurzu této pouze Nastav..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/20/2014
ms.topic: article
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: e5968f8d19191c3f44e9bd58f8e22a39d8d8faff
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="owin-oauth-20-authorization-server"></a>Server ověřování OWIN OAuth 2.0
====================
podle [Hongye Sun](https://github.com/hongyes), [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu pokyny o tom, jak implementovat serveru autorizace OAuth 2.0 pomocí middlewaru OWIN OAuth. Toto je pokročilé kurz, který pouze popisuje postup vytvoření serveru OWIN OAuth 2.0 autorizace. Toto není návod krok za krokem. [Stažení ukázkového kódu](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
> 
> > [!NOTE]
> > Tato osnova nesmí určena k použití pro vytvoření zabezpečeného produkční aplikace. V tomto kurzu slouží jako pouze přehled o tom, jak implementovat serveru autorizace OAuth 2.0 pomocí middlewaru OWIN OAuth.
> 
> 
> ## <a name="software-versions"></a>Verze softwaru
> 
> | **Zobrazí v tomto kurzu** | **Taky spolupracuje se službou** |
> | --- | --- |
> | Windows 8.1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Visual Studio 2013 Express pro plochu](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express). Visual Studio 2012 s nejnovější aktualizace by měla fungovat, ale kurzu nebyl testován s ním a některé možnosti nabídky a dialogových oknech se liší. |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je v uvést [Katana projektu na Githubu](https://github.com/aspnet/AspNetKatana/). Pro dotazy a připomínky týkající se tohoto kurzu, samotné najdete v části komentáře v dolní části stránky.


[OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) umožňuje aplikaci třetí strany získat omezený přístup k službě protokolu HTTP. Místo použití pověření vlastníka prostředku pro přístup k chráněnému prostředku, klient získá přístupový token (což je řetězec představující konkrétní obor, doba platnosti a další atributy přístupu). Přístupové tokeny jsou vystavené klientům třetích stran pomocí serveru autorizace schválení vlastníka prostředku.

V tomto kurzu bude zahrnovat:

- Postup vytvoření serveru autorizace pro podporu ověřování čtyři typy udělení a obnovovacích tokenů:
    - Udělení autorizačního kódu
    - Implicitní Grant
    - Heslo vlastníka prostředku pověření Grant
    - Udělení pověření klienta
- Vytváření prostředků serveru, který je chráněn přístupový token.
- Vytváření klientů OAuth 2.0.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Požadavky

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) nebo bezplatnou [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express), jak je uvedeno v **verze softwaru** v horní části stránky.
- Znalost OWIN. V tématu [Začínáme s projektem Katana](https://msdn.microsoft.com/magazine/dn451439.aspx) a [co je nového v OWIN a Katana](index.md).
- Znalost [OAuth](http://tools.ietf.org/html/rfc6749) terminologie, včetně [role](http://tools.ietf.org/html/rfc6749#section-1.1), [toku protokol](http://tools.ietf.org/html/rfc6749#section-1.2), a [Authorization Grant](http://tools.ietf.org/html/rfc6749#section-1.3). [Úvod OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-1) úvodem funkční.

## <a name="create-an-authorization-server"></a>Vytvoření serveru autorizace

V tomto kurzu jsme se zhruba načrtnout použití [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) a architektura ASP.NET MVC k vytvoření serveru autorizace. Věříme, že brzy zajistit stahování pro je hotová ukázka jako tento kurz neobsahuje každý krok. Nejprve vytvořte prázdnou webovou aplikaci s názvem *AuthorizationServer* a nainstalujte následující balíčky:

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (nebo jiné sociálních přihlášení balíček například Microsoft.Owin.Security.Facebook)

Přidat [třídy pro spuštění OWIN](owin-startup-class-detection.md) v kořenové složce projektu s názvem *spuštění*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Vytvoření *aplikace\_spustit* složky. Vyberte *aplikace\_spustit* složku a použití Shift + Alt + A přidat stažené verze *AuthorizationServer\App\_Start\Startup.Auth.cs* souboru.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

Výše uvedený kód umožňuje přihlášení aplikace/externí soubory cookie a ověřování Google, které jsou používány autorizace samotný server Pokud chcete spravovat účty.

`UseOAuthAuthorizationServer` Metoda rozšíření má instalační program serveru ověřování. Instalační program možnosti jsou:

- `AuthorizeEndpointPath`: Cesta požadavku, kam klientské aplikace přesměrují uživatelského agenta za účelem získání uživatele souhlas k vydání tokenu nebo kódu. Musí začínat úvodním lomítkem, například "`/Authorize`".
- `TokenEndpointPath`: Požadavek cesta klientské aplikace přímo komunikují se získat přístupový token. Musí začínat úvodním lomítkem, jako je "/ Token". Pokud se objeví klienta [klienta\_tajný klíč](http://tools.ietf.org/html/rfc6749#appendix-A.2), musí být poskytnut k tomuto koncovému bodu.
- `ApplicationCanDisplayErrors`: Nastavte na `true` Pokud webová aplikace chce generovat vlastní chybovou stránku pro chyby ověření klienta na `/Authorize` koncový bod. Je to potřeba jenom pro případy, kdy prohlížeč není přesměrován zpět do klientské aplikace, například když `client_id` nebo `redirect_uri` jsou nesprávné. `/Authorize` Koncový bod by měl očekávat "oauth. Chyba","oauth. ErrorDescription"a"oauth. Vlastnosti ErrorUri"se přidají do prostředí OWIN. 

    > [!NOTE]
    > Pokud není nastavena hodnota true, bude server ověřování vrátit výchozí chybovou stránku s podrobnosti o chybě.
- `AllowInsecureHttp`: Hodnota True pro povolení požadavků ověřování a tokenů příchod na adresy HTTP URI a chcete povolit příchozí `redirect_uri` autorizovat parametry žádosti do mají adresy HTTP URI. 

    > [!WARNING]
    > Zabezpečení – jde o vývoj jenom.
- `Provider`: Objekt Poskytnutý aplikací ke zpracování událostí vyvolaných middlewarem serveru ověřování. Aplikace může toto rozhraní implementovat plně nebo může vytvořit instanci `OAuthAuthorizationServerProvider` a přiřadit delegáty potřebné pro toky OAuth tento server podporuje.
- `AuthorizationCodeProvider`: Vytvoří jednorázový autorizační kód se vraťte do klientské aplikace. Pro server OAuth se zabezpečit aplikace **musí** poskytovat instanci pro `AuthorizationCodeProvider` kde token vyprodukované `OnCreate/OnCreateAsync` událostí je považován za platný pouze pro jedno volání do `OnReceive/OnReceiveAsync`.
- `RefreshTokenProvider`: Vytvoří token obnovení, který může být použit k vytvoření nového tokenu přístupu v případě potřeby. Pokud není zadaný server ověřování nevrátí tokeny obnovení z `/Token` koncový bod.

## <a name="account-management"></a>Správa účtů

OAuth nebude vědět, kde nebo jak můžete spravovat informace o účtu uživatele. Má [ASP.NET Identity](../../../identity/index.md) který zodpovídá za ho. V tomto kurzu jsme zjednodušit správu kód účtu a ujistěte se, že tento uživatel se může přihlásit pomocí middlewaru OWIN souboru cookie. Tady je zjednodušená ukázkový kód pro `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri`slouží k ověření klienta se jeho adresa URL přesměrování registrované. `ValidateClientAuthentication`kontroluje hlavičky základní schéma a text formuláře získat pověření klienta.

Přihlašovací stránky je zobrazena níže:

![](owin-oauth-20-authorization-server/_static/image1.png)

Zkontrolujte IETF OAuth 2 [udělení autorizačního kódu](http://tools.ietf.org/html/rfc6749#section-4.1) části nyní. 

**Zprostředkovatel** (v následující tabulce) je [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Zprostředkovatel, který je typu `OAuthAuthorizationServerProvider`, která obsahuje všechny události serveru OAuth. 

| Tok postup z části udělení autorizačního kódu | Stažení ukázkové provádí tyto kroky: |
| --- | --- |
|  |  |
| (A) klient zahájí toku přesměrováním vlastníka prostředku uživatelského agenta pro koncový bod autorizace. Klient zahrne jeho identifikátoru klienta, požadovaném oboru, místní stavu a identifikátor URI přesměrování, ke kterému serveru ověřování odešle uživatelského agenta zpět, když je přístup povolen (nebo je odepřen). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) serveru ověřování ověřuje vlastníka prostředku (prostřednictvím uživatelského agenta) a určuje, zda uděluje vlastníka prostředku, nebo odmítne žádost o přístup klienta. | **&lt;Pokud uživatel povolí přístup&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) za předpokladu, že vlastník prostředku uděluje přístup, server ověřování přesměruje uživatelského agenta zpět do klienta pomocí zadaný identifikátor URI přesměrování starší (v požadavku nebo během registrace klienta). ... |  |
|  |  |
| (D) klient požádá o token přístupu ze serveru ověřování koncový bod tokenu zahrnutím autorizační kód přijaté v předchozím kroku. Při vytváření požadavku, klient se ověří pomocí serveru ověřování. Klient zahrne přesměrování, které URI používají k získání autorizační kód pro ověření. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Ukázka implementace pro `AuthorizationCodeProvider.CreateAsync` a `ReceiveAsync` k řízení vytvoření a ověření autorizační kód je uveden níže.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

Výše uvedený kód používá v paměti ConcurrentDictionary uložit lístek pro kód a identity a obnovit identitu po přijetí kód. V reálné aplikaci byste nahradit odpovídajícími trvalé úložišti. Koncový bod autorizace je pro vlastníka prostředku k udělení přístupu k klienta. Obvykle je uživatelské rozhraní pro uživatele a klikněte na tlačítko potvrďte udělení povolit. Middleware OWIN OAuth umožňuje kódu aplikace za účelem zpracování koncového bodu autorizace. V našem ukázkovou aplikaci používáme kontroler MVC volá `OAuthController` se nezdařilo. Zde je ukázka implementace:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

`Authorize` Akce nejdřív zkontroluje, pokud je uživatel přihlášen k autorizaci serveru. Pokud ne, že ověřovací middleware vyzve volajícího k ověření pomocí souboru cookie "Aplikace" a přesměruje na přihlašovací stránku. (Viz výše uvedený zvýrazněný kód.) Pokud je uživatel přihlášen, se budou vykreslovat autorizovat zobrazení, jak je uvedeno níže:

![](owin-oauth-20-authorization-server/_static/image2.png)

Pokud **Grant** výběru tlačítka `Authorize` akce vytvoří nové identity "Nosiče" a přihlaste se pomocí ho. Spustí serveru ověřování k vygenerování tokenu nosiče a jeho odeslání zpět do klienta se datová část JSON. 

### <a name="implicit-grant"></a>Implicitní Grant

Najdete IETF OAuth 2 [implicitní Grant](http://tools.ietf.org/html/rfc6749#section-4.2) části nyní.

 [Implicitní Grant](http://tools.ietf.org/html/rfc6749#section-4.2) toku znázorněno na obrázku 4 je toku a které mapování OWIN OAuth odpovídá middleware.  

| Tok postup z části implicitní Grant | Stažení ukázkové provádí tyto kroky: |
| --- | --- |
|  |  |
| (A) klient zahájí toku přesměrováním vlastníka prostředku uživatelského agenta pro koncový bod autorizace. Klient zahrne jeho identifikátoru klienta, požadovaném oboru, místní stavu a identifikátor URI přesměrování, ke kterému serveru ověřování odešle uživatelského agenta zpět, když je přístup povolen (nebo je odepřen). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) serveru ověřování ověřuje vlastníka prostředku (prostřednictvím uživatelského agenta) a určuje, zda uděluje vlastníka prostředku, nebo odmítne žádost o přístup klienta. | **&lt;Pokud uživatel povolí přístup&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) za předpokladu, že vlastník prostředku uděluje přístup, server ověřování přesměruje uživatelského agenta zpět do klienta pomocí zadaný identifikátor URI přesměrování starší (v požadavku nebo během registrace klienta). ... |  |
|  |  |
| (D) klient požádá o token přístupu ze serveru ověřování koncový bod tokenu zahrnutím autorizační kód přijaté v předchozím kroku. Při vytváření požadavku, klient se ověří pomocí serveru ověřování. Klient zahrne přesměrování, které URI používají k získání autorizační kód pro ověření. |  |

Vzhledem k tomu, že implementovali jsme již koncového bodu autorizace (`OAuthController.Authorize` akce) pro udělení autorizačního kódu umožňuje automaticky také implicitního toku. Poznámka: `Provider.ValidateClientRedirectUri` slouží k ověření s jeho přesměrování adresu URL, která chrání toku implicitní udělit přístup odesílání tokenu k škodlivý klienty z ID klienta ([útok Man-in-the-middle](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Heslo vlastníka prostředku pověření Grant

Najdete IETF OAuth 2 [udělení pověření heslo vlastníka prostředku](http://tools.ietf.org/html/rfc6749#section-4.3) části nyní.

 [Udělení pověření heslo vlastníka prostředku](http://tools.ietf.org/html/rfc6749#section-4.3) toku vidět na obrázku 5 má toku a které mapování OWIN OAuth odpovídá middleware.  

| Tok postup z části udělení pověření heslo vlastníka prostředku | Stažení ukázkové provádí tyto kroky: |
| --- | --- |
|  |  |
| (A vlastník) prostředku poskytuje klientovi jeho uživatelské jméno a heslo. |  |
|  |  |
| (B) klient požádá o token přístupu ze serveru ověřování koncový bod tokenu zahrnutím přihlašovací údaje přijaté z vlastníka prostředku. Při vytváření požadavku, klient se ověří pomocí serveru ověřování. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) serveru ověřování ověřuje klienta a ověří pověření vlastníka prostředku a pokud je platná, vydá přístupový token. |  |

Zde je ukázka implementace pro `Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> Výše uvedený kód je určen k vysvětlení této části kurzu a nesmí být použita v zabezpečené nebo produkční aplikace. Nekontroluje pověření vlastníky prostředků. Předpokládá, že se každý pověření je platný a vytvoří novou identitu pro ni. Novou identitu, se použije k vygenerování přístupového tokenu a aktualizovat token. Nahraďte kód vlastní kód správu zabezpečení účtu.


### <a name="client-credentials-grant"></a>Udělení pověření klienta

Najdete IETF OAuth 2 [udělení pověření klienta](http://tools.ietf.org/html/rfc6749#section-4.4) části nyní.

 [Udělení pověření klienta](http://tools.ietf.org/html/rfc6749#section-4.4) toku vidíte na obrázku 6 má toku a které mapování OWIN OAuth odpovídá middleware.  

| Tok postup z části udělení pověření klienta | Stažení ukázkové provádí tyto kroky: |
| --- | --- |
|  |  |
| (A) klient ověří server ověřování a požadavky přístupového tokenu z koncového bodu token. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B) serveru ověřování ověřuje klienta a pokud je platná, vydá přístupový token. |  |

Zde je ukázka implementace pro `Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> Výše uvedený kód je určen k vysvětlení této části kurzu a nesmí být použita v zabezpečené nebo produkční aplikace. Nahraďte kód vlastního kódu správy klientů.


### <a name="refresh-token"></a>Aktualizovat Token

Najdete IETF OAuth 2 [aktualizovat Token](http://tools.ietf.org/html/rfc6749#section-1.5) části nyní.

 [Aktualizovat Token](http://tools.ietf.org/html/rfc6749#section-1.5) toku znázorněno na obrázku 2 má toku a které mapování OWIN OAuth odpovídá middleware.  

| Tok postup z části udělení pověření klienta | Stažení ukázkové provádí tyto kroky: |
| --- | --- |
|  |  |
| (G) klient požádá o nový přístupový token ověřování pomocí serveru ověřování a prezentování token obnovení. Požadavky na ověřování klienta jsou založené na typ klienta a na serveru zásad autorizace. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (H) serveru ověřování ověřuje klienta a ověří token obnovení a pokud je platná, vydá nový přístupový token (a volitelně nový token obnovení). |  |

Zde je ukázka implementace pro `Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Vytvořit prostředek serveru, který je chráněn tokenu přístupu

Vytvoření projektu prázdnou webovou aplikaci a nainstalujte následující balíčky v projektu:

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Vytvořte třídu, spuštění a konfigurovat ověřování a webového rozhraní API. V tématu *AuthorizationServer\ResourceServer\Startup.cs* v ukázkové stahování.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

V tématu *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* v ukázkové stahování.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

V tématu *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* v ukázkové stahování.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors`metoda umožňuje CORS pro všechny domény.
- `UseOAuthBearerAuthentication`metoda umožňuje middleware nosiče tokenu ověřování OAuth, který bude přijímat a ověření tokenu nosiče z autorizační hlavičky v požadavku.
- `Config.SuppressDefaultHostAuthenticaiton`potlačí výchozí hostitel ověřený objekt zabezpečení z aplikace, proto všechny požadavky budou anonymní po toto volání.
- `HostAuthenticationFilter`Umožňuje ověřování jenom pro zadaný typ ověřování. V takovém případě je typ ověřování nosiče.

Aby bylo možné prokázat ověřenou identitu, vytvoříme objektu ApiController výstup deklarací identity aktuálního uživatele.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Pokud server ověřování a server prostředků nejsou ve stejném počítači, OAuth middleware použije jiný počítač klíče k šifrování a dešifrování přístupový token nosiče. Aby bylo možné sdílet stejný soukromý klíč mezi obou projektů, stejné přidáme `machinekey` nastavení v obou *web.config* soubory.

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>Vytvoření klientů OAuth 2.0

 Používáme [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) balíček NuGet můžete zjednodušit kód klienta.

### <a name="authorization-code-grant-client"></a>Autorizační kód Grant klienta

 Tento klient je aplikace MVC. Spustí tok autorizační kód grant k získání tokenu přístupu z back-end. Má jednu stránku, jak je uvedeno níže:

![](owin-oauth-20-authorization-server/_static/image3.png)

- **Autorizovat** tlačítko přesměruje prohlížeče serveru ověřování oznámit vlastníkovi prostředku k udělení přístupu k tohoto klienta.
- **Aktualizovat** tlačítko bude získání nového tokenu přístupu a obnovovací token pomocí této aktualizace tokenu.
- **Přístup chráněný prostředek API** tlačítko zavolá server prostředků a získat data deklarace identity aktuální uživatel je zobrazit na stránce.

Zde je ukázka kódu `HomeController` klienta.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth`vyžaduje SSL ve výchozím nastavení. Vzhledem k tomu, že naše Ukázka používá HTTP, je nutné přidat následující nastavení v konfiguračním souboru:

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Zabezpečení – nikdy zakázat protokol SSL v produkční aplikaci. Přihlašovací údaje jsou nyní odesílány ve formátu prostého textu v drátové síti. Výše uvedený kód je jen pro místní ukázka ladění a zkoumání.


### <a name="implicit-grant-client"></a>Implicitní Grant klienta

Tento klient je pomocí jazyka JavaScript na:

1. Otevřete nové okno a přesměrovat na koncový bod authorize serveru ověřování.
2. Získání tokenu přístupu z adresy URL fragmenty při přesměrování zpět.

Tento proces je znázorněn na následujícím obrázku:

![](owin-oauth-20-authorization-server/_static/image4.png)

Klient by měl mít dvě stránky: jeden pro domovskou stránku a druhou pro zpětné volání. Tady je ukázkový kód nalézt v JavaScript *Index.cshtml* souboru:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Zde je kód v zpracování zpětného volání *SignIn.cshtml* souboru:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> Osvědčeným postupem je přesunout na externí soubor JavaScript a není ji vložit se značkami Razor. Pro zjednodušení této ukázku byla kombinaci.


### <a name="resource-owner-password-credentials-grant-client"></a>Heslo vlastníka prostředku pověření Grant klienta

Jsme ukázku tohoto klienta pomocí konzolové aplikace. Zde je kód:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>Klient udělení pověření klienta

Podobně jako u udělení pověření heslo vlastníka prostředku, zde je kód aplikace konzoly:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
