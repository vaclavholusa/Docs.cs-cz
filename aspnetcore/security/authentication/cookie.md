---
title: "Pomocí souboru Cookie ověřování bez ASP.NET Core Identity"
author: rick-anderson
description: "Vysvětlení pomocí souboru cookie ověřování bez ASP.NET Core Identity"
manager: wpickett
ms.author: riande
ms.date: 10/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/cookie
ms.openlocfilehash: 2c08c4810a1952cc4890d46593d55f558b6ed8e9
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>Pomocí souboru Cookie ověřování bez ASP.NET Core Identity

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Luke Latham](https://github.com/guardrex)

Protože jste viděli v dřívější ověřování témata, [ASP.NET Core Identity](xref:security/authentication/identity) je poskytovatel dokončení, plně vybavený ověřování pro vytváření a údržbu přihlášení. Můžete však použít logika vlastní ověřování na základě souboru cookie ověřování v některých případech. Ověřování na základě souborů cookie slouží jako zprostředkovatel ověřování samostatné bez ASP.NET Core Identity.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Informace o migraci na základě souboru cookie ověřování z ASP.NET Core 1.x k 2.0, najdete v části [migrace ověřování a identita na technologii ASP.NET 2.0 základní téma (ověřování na základě souborů Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).

## <a name="configuration"></a>Konfigurace

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Pokud nepoužíváte [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), instalace verze 2.0 + [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) balíček NuGet.

V `ConfigureServices` metoda, vytvořit službu Middleware ověřování s `AddAuthentication` a `AddCookie` metody:

[!code-csharp[](cookie/sample/Startup.cs?name=snippet1)]

`AuthenticationScheme` Předaný `AddAuthentication` nastaví výchozí schéma ověřování pro aplikaci. `AuthenticationScheme` je užitečné, když je spuštěno více instancí ověřování souborů cookie a vy chcete [autorizaci s konkrétní schéma](xref:security/authorization/limitingidentitybyscheme). Nastavení `AuthenticationScheme` k `CookieAuthenticationDefaults.AuthenticationScheme` poskytuje hodnotu "Soubory cookie" pro schéma. Můžete zadat libovolnou hodnotu řetězce, která rozlišuje schéma.

V `Configure` metoda, použijte `UseAuthentication` metoda k vyvolání Middleware ověřování, který nastaví `HttpContext.User` vlastnost. Volání `UseAuthentication` metoda před voláním `UseMvcWithDefaultRoute` nebo `UseMvc`:

[!code-csharp[](cookie/sample/Startup.cs?name=snippet2)]

**Možnosti AddCookie**

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) třída se používá ke konfiguraci možností zprostředkovatele ověřování.

| Možnost | Popis |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | Určuje cestu k poskytování s 302 Found (adresa URL přesměrování) při aktivaci pomocí `HttpContext.ForbidAsync`. Výchozí hodnota je `/Account/AccessDenied`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | Vystavitel, který má používat pro [vystavitele](/dotnet/api/system.security.claims.claim.issuer) vlastnost deklarace identity, vytvořených službou souboru cookie ověřování. |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | Název domény, kde je soubor cookie zpracovat. Ve výchozím nastavení to je název hostitele žádosti. Prohlížeč odesílá pouze soubor cookie v požadavcích na odpovídající název hostitele. Můžete upravit tak, aby soubory cookie, které jsou k dispozici na každém hostiteli ve vaší doméně. Například nastavení domény souboru cookie na `.contoso.com` ji zpřístupní k `contoso.com`, `www.contoso.com`, a `staging.www.contoso.com`. |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | Získá nebo nastaví životnost souboru cookie. V současné době to možnost Ne ops a bude v ASP.NET Core 2.1 + zastaralé. Použití `ExpireTimeSpan` nastavit vypršení platnosti souboru cookie. Další informace najdete v tématu [vysvětlení chování CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293). |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | Příznak, který udává, pokud je soubor cookie musí být přístupný jenom pro servery. Změnou této hodnoty `false` povoluje skripty na straně klienta pro přístup k souboru cookie a může zobrazit v aplikaci krádež souborů cookie musí mít vaše aplikace [skriptování (XSS)](xref:security/cross-site-scripting) ohrožení zabezpečení. Výchozí hodnota je `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | Nastaví název souboru cookie. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | Použít k izolování aplikací běžících na stejný název hostitele. Pokud máte aplikace spuštěné v `/app1` a chcete omezit soubory cookie pro tuto aplikaci, nastavte `CookiePath` vlastnost `/app1`. Díky tomu souboru cookie, který je k dispozici pouze u požadavků na `/app1` a všechny aplikace pod ním. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | Určuje, zda má prohlížeč umožnit soubor cookie na připojit ke stejné lokalitě požadavky jenom (`SameSiteMode.Strict`) nebo požadavků webů pomocí bezpečné metod HTTP a žádostí stejné lokalitě (`SameSiteMode.Lax`). Pokud nastavíte hodnotu `SameSiteMode.None`, není nastaven hodnotu hlavičky souborů cookie. Všimněte si, že [Middleware zásady souborů Cookie](#cookie-policy-middleware) přepsat hodnotu, která zadáte. Pro podporu ověřování OAuth, výchozí hodnota je `SameSiteMode.Lax`. Další informace najdete v tématu [poškozený z důvodu zásad SameSite souboru cookie ověřování OAuth](https://github.com/aspnet/Security/issues/1231). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | Příznak, který udává, pokud je třeba vytvořit soubor cookie omezit na HTTPS (`CookieSecurePolicy.Always`), HTTP nebo HTTPS (`CookieSecurePolicy.None`), nebo stejný protokol jako požadavku (`CookieSecurePolicy.SameAsRequest`). Výchozí hodnota je `CookieSecurePolicy.SameAsRequest`. |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | Nastaví `DataProtectionProvider` sloužící k vytvoření výchozích `TicketDataFormat`. Pokud `TicketDataFormat` je vlastnost nastavena, `DataProtectionProvider` možnost nepoužívá. Pokud není zadaná, se používá výchozí zprostředkovatele ochrany data aplikace. |
| [Události](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | Obslužná rutina volá metody zprostředkovatele, která poskytnou aplikaci kontrolu v určitých bodech zpracování. Pokud `Events` není zadáno, je zadaný výchozí instance, který nic neprovádí, pokud jsou volány metody. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | Použít jako typ služby k získání `Events` instanci místo vlastnost. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | `TimeSpan` Po kterém vyprší platnost lístku ověřování, který je uložený v souboru cookie. `ExpireTimeSpan` se přidá do aktuální čas vytvoření dobu vypršení platnosti lístku. `ExpiredTimeSpan` Hodnotu vždy přejde do šifrované AuthTicket ověřit server. Může také přejít do [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) záhlaví, ale jenom v případě `IsPersistent` nastavena. Chcete-li nastavit `IsPersistent` k `true`, nakonfigurujte [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) předaný `SignInAsync`. Výchozí hodnota `ExpireTimeSpan` je 14 dnů. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | Určuje cestu k poskytování s 302 Found (adresa URL přesměrování) při aktivaci pomocí `HttpContext.ChallengeAsync`. Aktuální adresa URL, která generovala kód 401 je přidána do `LoginPath` jako parametr řetězce dotazu s názvem podle `ReturnUrlParameter`. Jednou požadavek na `LoginPath` udělí novou identitu přihlášení, `ReturnUrlParameter` hodnota se používá k přesměrování prohlížeče zpět na adresu URL, která způsobila původní kód neautorizovaného stavu. Výchozí hodnota je `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | Pokud `LogoutPath` je poskytnuta obslužnou rutinu, pak přesměruje požadavek na tuto cestu na základě hodnoty z `ReturnUrlParameter`. Výchozí hodnota je `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | Určuje název parametru řetězce dotazu, který se připojí obslužnou rutinou pro odpovědi 302 Found (adresa URL přesměrování). `ReturnUrlParameter` se používá, pokud dorazí požadavek na `LoginPath` nebo `LogoutPath` po provedení akce přihlašovací nebo odhlašovací vrátit v prohlížeči na původní adresu URL. Výchozí hodnota je `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | Volitelný kontejner používá k ukládání identit napříč požadavky. Pokud se používá, jenom identifikátor relace je odeslat klientovi. `SessionStore` slouží k zmírnit potenciální potíže s velkými identitami. |
| [parametr slidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | Příznak, který udává, pokud se nový soubor cookie s času aktualizované vypršení platnosti musí být vydávány dynamicky. To se může stát při všechny žádosti, kde je aktuální doba vypršení platnosti souboru cookie více než 50 % vypršela. Nové datum vypršení platnosti je dál přesunout na aktuální datum a `ExpireTimespan`. [Čas vypršení platnosti souboru cookie absolutní](xref:security/authentication/cookie#absolute-cookie-expiration) lze nastavit pomocí `AuthenticationProperties` třídy při volání metody `SignInAsync`. Čas vypršení platnosti absolutní lze vylepšit zabezpečení vaší aplikace omezení množství času, který je platný soubor cookie ověřování. Výchozí hodnota je `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | `TicketDataFormat` Se používá k ochraně a zrušení identity a dalších vlastností, které jsou uložené v hodnotě souboru cookie. Pokud není zadaná, `TicketDataFormat` je vytvořený pomocí [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0). |
| [Ověření](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | Metoda, která kontroluje, zda jsou platné možnosti. |

Nastavit `CookieAuthenticationOptions` v konfiguraci služby pro ověřování v `ConfigureServices` metoda:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Soubor cookie používá ASP.NET Core 1.x [middleware](xref:fundamentals/middleware/index) který serializuje hlavní název uživatele do šifrovaného souboru cookie. Na následné žádosti, se ověří souboru cookie a znovu vytvořit a přiřadit k objektu zabezpečení `HttpContext.User` vlastnost.

Nainstalujte [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) balíček NuGet do projektu. Tento balíček obsahuje middlewaru souboru cookie.

Použití `UseCookieAuthentication` metoda v `Configure` metoda v vaší *Startup.cs* soubor před `UseMvc` nebo `UseMvcWithDefaultRoute`:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

**Možnosti CookieAuthenticationOptions**

[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) třída se používá ke konfiguraci možností zprostředkovatele ověřování.

| Možnost | Popis |
| ------ | ----------- |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | Nastaví schéma ověřování. `AuthenticationScheme` je užitečné, když je spuštěno více instancí ověřování a vy chcete [autorizaci s konkrétní schéma](xref:security/authorization/limitingidentitybyscheme). Nastavení `AuthenticationScheme` k `CookieAuthenticationDefaults.AuthenticationScheme` poskytuje hodnotu "Soubory cookie" pro schéma. Můžete zadat libovolnou hodnotu řetězce, která rozlišuje schéma. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | Nastaví hodnotu, znamená, že ověřování souborů cookie by měl spustit u každého požadavku a pokus o ověření a proveďte rekonstrukci serializovaný objekt zabezpečení, vytvořen. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | V případě hodnoty true zpracovává middleware ověřování automatické problémy. Pokud hodnotu false, middleware ověřování jenom mění odpovědi, když explicitně uvedené `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | Vystavitel, který má používat pro [vystavitele](/dotnet/api/system.security.claims.claim.issuer) vlastnost deklarace identity, vytvořené middleware ověřování souborů cookie. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | Název domény, kde je soubor cookie zpracovat. Ve výchozím nastavení to je název hostitele žádosti. V prohlížeči slouží pouze soubor cookie na odpovídající název hostitele. Můžete upravit tak, aby soubory cookie, které jsou k dispozici na každém hostiteli ve vaší doméně. Například nastavení domény souboru cookie na `.contoso.com` ji zpřístupní k `contoso.com`, `www.contoso.com`, a `staging.www.contoso.com`. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | Příznak, který udává, pokud je soubor cookie musí být přístupný jenom pro servery. Změnou této hodnoty `false` povoluje skripty na straně klienta pro přístup k souboru cookie a může zobrazit v aplikaci krádež souborů cookie musí mít vaše aplikace [skriptování (XSS)](xref:security/cross-site-scripting) ohrožení zabezpečení. Výchozí hodnota je `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | Použít k izolování aplikací běžících na stejný název hostitele. Pokud máte aplikace spuštěné v `/app1` a chcete omezit soubory cookie pro tuto aplikaci, nastavte `CookiePath` vlastnost `/app1`. Díky tomu souboru cookie, který je k dispozici pouze u požadavků na `/app1` a všechny aplikace pod ním. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | Příznak, který udává, pokud je třeba vytvořit soubor cookie omezit na HTTPS (`CookieSecurePolicy.Always`), HTTP nebo HTTPS (`CookieSecurePolicy.None`), nebo stejný protokol jako požadavku (`CookieSecurePolicy.SameAsRequest`). Výchozí hodnota je `CookieSecurePolicy.SameAsRequest`. |
| [Popis](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | Další informace o typu ověřování, který je k dispozici na aplikaci. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | `TimeSpan` Po kterém vyprší platnost lístku ověřování. Přidá na aktuální čas vytvoření dobu vypršení platnosti lístku. Použít `ExpireTimeSpan`, je nutné nastavit `IsPersistent` k `true` v `AuthenticationProperties` předaný `SignInAsync`. Výchozí hodnota je 14 dnů. |
| [parametr slidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | Příznak, který udává, zda datum vypršení platnosti souboru cookie resetuje při více než polovinu `ExpireTimeSpan` uplynutí intervalu. Nový čas exipiration se dál přesune na aktuální datum a `ExpireTimespan`. [Čas vypršení platnosti souboru cookie absolutní](xref:security/authentication/cookie#absolute-cookie-expiration) lze nastavit pomocí `AuthenticationProperties` třídy při volání metody `SignInAsync`. Čas vypršení platnosti absolutní lze vylepšit zabezpečení vaší aplikace omezení množství času, který je platný soubor cookie ověřování. Výchozí hodnota je `true`. |

Nastavit `CookieAuthenticationOptions` pro Middleware ověřování souborů Cookie v `Configure` metoda:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a>Middlewaru souboru cookie zásad.

[Middleware zásady souborů cookie](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) povoluje možnosti zásad souborů cookie v aplikaci. Přidání middleware aplikace zpracování kanálu je pořadí citlivé; ovlivňuje pouze komponenty registrované po ho v kanálu.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) poskytované middlewaru souboru Cookie zásady umožňují řídit globální vlastnosti souboru cookie zpracování a háku do obslužné rutiny zpracování souboru cookie, když jsou soubory cookie připojí nebo odstranit.

| Vlastnost | Popis |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | Ovlivňuje, jestli HttpOnly musí být soubory cookie, který je příznak označující, pokud soubor cookie musí být přístupný jenom pro servery. Výchozí hodnota je `HttpOnlyPolicy.None`. |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | Ovlivňuje stejné lokalitě atribut souboru cookie (viz níže). Výchozí hodnota je `SameSiteMode.Lax`. Tato možnost je k dispozici pro jádro ASP.NET 2.0 +. |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Voláno, když se připojí do souboru cookie. |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | Volá se při odstranění souboru cookie. |
| [Zabezpečení](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | Ovlivňuje, jestli soubory cookie musí být zabezpečené. Výchozí hodnota je `CookieSecurePolicy.None`. |

**MinimumSameSitePolicy** (ASP.NET Core 2.0 + pouze)

Výchozí hodnota `MinimumSameSitePolicy` hodnota je `SameSiteMode.Lax` pro povolení ověřování OAuth2. Výhradně vynutit zásady stejné lokalitě `SameSiteMode.Strict`, nastavte `MinimumSameSitePolicy`. I když toto nastavení dělí OAuth2 a jiná schémata ověřování cross-origin, zvyšuje úroveň zabezpečení souboru cookie pro ostatní typy aplikací, které nejsou závisí na zpracování požadavků mezi zdroji.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Nastavení zásad middlewaru souboru Cookie pro `MinimumSameSitePolicy` může mít vliv na vaše nastavení `Cookie.SameSite` v `CookieAuthenticationOptions` nastavení podle níže uvedené tabulky.

| MinimumSameSitePolicy | Cookie.SameSite | Výsledná nastavení Cookie.SameSite |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="creating-an-authentication-cookie"></a>Vytvoření souboru cookie pro ověřování

Pokud chcete vytvořit soubor cookie, která uchovává informace o uživateli, je nutné vytvořit [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal). Informace o uživateli se serializovat a uloží do souboru cookie. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Vytvoření [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) s jakékoli požadované [deklarace identity](/dotnet/api/system.security.claims.claim)s a volání [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) k přihlášení uživatele:

[!code-csharp[](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Volání [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) k přihlášení uživatele:

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

`SignInAsync` Vytvoří šifrovaného souboru cookie a přidává ji k aktuální odpovědi. Pokud neurčíte `AuthenticationScheme`, se používá výchozí schéma.

V pozadí, je šifrování, které používá ASP.NET Core [ochrany dat](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) systému. Pokud jste hostování aplikace na více počítačů, Vyrovnávání zatížení napříč aplikací nebo pomocí webové farmy, pak musíte [konfigurovat ochranu dat](xref:security/data-protection/configuration/overview) pro stejný klíč prstenec a identifikátor aplikace.

## <a name="signing-out"></a>Odhlásit

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Odhlásit se aktuálního uživatele a odstranit jejich souboru cookie, volání [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

[!code-csharp[](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Odhlásit se aktuálního uživatele a odstranit jejich souboru cookie, volání [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

Pokud nepoužíváte `CookieAuthenticationDefaults.AuthenticationScheme` (nebo "soubory cookie.) jako schéma (například" ContosoCookie"), zadejte schéma, které jste použili při konfiguraci zprostředkovatele ověřování. Jinak je použit výchozí schéma.

## <a name="reacting-to-back-end-changes"></a>Reaguje na změny back-end

Po vytvoření souboru cookie bude jediným zdrojem identity. I v případě, že zakážete uživatele v systémech back-end, nemá žádné informace o tento soubor cookie ověřování systému a uživatel zůstane přihlášený tak dlouho, dokud jejich soubor cookie je neplatný.

[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) událost v ASP.NET Core 2.x nebo [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) metoda v ASP.NET Core 1.x lze použít pro zachycení a přepsat ověření souboru cookie identity. Tento přístup snižuje riziko odvolané uživatele, kteří používají aplikaci.

Jeden ze způsobů ověření souboru cookie je založena na udržování přehledu o při změně databáze uživatelů. Pokud databáze nebyla změněna od uživatele soubor cookie vydala, není nutné opakované ověření uživatele, pokud jejich souborů cookie je stále platné. K implementaci databázi, která je implementovaná v tomto scénáři `IUserRepository` v tomto příkladu ukládá `LastChanged` hodnotu. Při aktualizaci všechny uživatele v databázi, `LastChanged` hodnota nastavena na aktuální čas.

Chcete-li zrušit platnost souboru cookie, pokud změny databáze v závislosti na `LastChanged` hodnotu, vytvořte soubor cookie s `LastChanged` deklarace identity, který obsahuje aktuální `LastChanged` hodnotu z databáze:

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

K přepsání pro implementaci `ValidatePrincipal` událostí, zápisu a metoda podpisem následující třídy, které pocházejí z [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Příklad vypadá takto:

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Registrace instance události při registraci služby souborů cookie v `ConfigureServices` metoda. Zadejte registraci vymezené služby pro vaše `CustomCookieAuthenticationEvents` třídy:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

K přepsání pro implementaci `ValidateAsync` událostí, zápisu a metoda podpisem následující:

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

ASP.NET Core Identity implementuje tato kontrola v rámci jeho [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync). Příklad vypadá takto:

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Během konfigurace ověřování souborů cookie v registraci události `Configure` metoda:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

Představte si situaci, ve kterém se aktualizuje uživatelské jméno &mdash; rozhodnutí, které nemají vliv na zabezpečení žádným způsobem. Pokud chcete aktualizovat nedestruktivně hlavní název uživatele, zavolejte `context.ReplacePrincipal` a nastavte `context.ShouldRenew` vlastnost `true`.

> [!WARNING]
> Přístup v rámci popsaného se aktivuje u každého požadavku. To může způsobit snížení výkonu velký pro aplikaci.

## <a name="persistent-cookies"></a>Trvalé soubory cookie

Můžete chtít soubor cookie k uchování v rámci relací prohlížeče. Tato trvalosti by měl povolit pouze s souhlas explicitní uživatele s "Zapamatovat uživatele" zaškrtávací políčko je na přihlášení nebo podobné mechanismus. 

Následující fragment kódu vytvoří identitu a odpovídající soubor cookie, který odolává prostřednictvím prohlížeče uzavření. Jsou dodržení nastavení klouzavé vypršení platnosti předtím nakonfigurovali. Pokud při zavření prohlížeče vyprší platnost souboru cookie, prohlížeč vymaže soubor cookie po jeho restartování.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) třída se nachází v `Microsoft.AspNetCore.Authentication` oboru názvů.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) třída se nachází v `Microsoft.AspNetCore.Http.Authentication` oboru názvů.

---

## <a name="absolute-cookie-expiration"></a>Platnost absolutní souborů cookie

Můžete nastavit čas vypršení platnosti absolutní s `ExpiresUtc`. Musíte taky nastavit `IsPersistent`, jinak hodnota `ExpiresUtc` je ignorován a k vytvoření souboru cookie jedné relace. Když `ExpiresUtc` je nastavený na `SignInAsync`, přepíše hodnotu `ExpireTimeSpan` možnost `CookieAuthenticationOptions`, pokud nastavení.

Následující fragment kódu vytvoří identitu a odpovídající soubor cookie, který trvá po dobu 20 minut. To bude ignorovat nastavení klouzavé vypršení platnosti předtím nakonfigurovali.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="see-also"></a>Viz také

* [Změny auth 2.0 nebo migrace oznámení](https://github.com/aspnet/Announcements/issues/262)
* [Omezení identity schématem](xref:security/authorization/limitingidentitybyscheme)
