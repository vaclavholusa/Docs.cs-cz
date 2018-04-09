---
title: Zabránit webů požadavku padělání (XSRF/proti útokům CSRF) před útoky v ASP.NET Core
author: steve-smith
description: Zjistit, jak zabránit útoky na webové aplikace, kde můžete škodlivou webovou stránku ovlivnit interakce mezi prohlížeče klienta a aplikace.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: ad50f8b261447d40ccc24c0ee006239aa976bf20
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/03/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Zabránit webů požadavku padělání (XSRF/proti útokům CSRF) před útoky v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), a [Rick Anderson](https://twitter.com/RickAndMSFT)

Padělání požadavku posílaného mezi weby (také označované jako XSRF nebo proti útokům CSRF, vyslovováno *najdete procházení*) je útok na hostované webové aplikace, které škodlivý webové aplikace mohou mít vliv na interakce mezi prohlížeče klienta a webové aplikace, která důvěřuje, prohlížeč. Tyto útoky nejsou možné, protože webových prohlížečů odesílat některé typy ověřování tokenů automaticky s každou žádost na web. Tato forma zneužití je také označován jako *jedním kliknutím útoku* nebo *relace jízda* protože útoku využívá uživatel je ověřen dříve relace.

Příklad útoku proti útokům CSRF:

1. Uživatel se přihlásí do `www.good-banking-site.com` ověřování pomocí formulářů. Server ověřuje uživatele a vydá odpovědi, která obsahuje soubor cookie ověřování. Web není bude zranitelný vůči útoku, protože důvěřovat jakékoli požadavky, které obdrží s platnou ověřovacího souboru cookie.
1. Uživatel navštíví škodlivé weby `www.bad-crook-site.com`.

   Škodlivé weby, `www.bad-crook-site.com`, obsahuje formuláře HTML, který je podobný následujícímu:

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   Všimněte si, že formuláře `action` příspěvky k větší zranitelnosti lokality, nikoli k škodlivé weby. Toto je část "webů" proti útokům CSRF.

1. Uživatel vybere tlačítko pro odeslání. Prohlížeč provede požadavek a automaticky zahrne ověřovacího souboru cookie pro požadovanou doménu `www.good-banking-site.com`.
1. Se spouští požadavek `www.good-banking-site.com` server s kontext ověřování uživatele a mohou provádět žádnou akci, kterou ověřený uživatel může provádět.

Když uživatel vybere tlačítko pro odeslání formuláře, může škodlivé weby:

* Spusťte skript, který automaticky odešle formulář.
* Odešle odeslání formuláře jako požadavek AJAX. 
* Skrytá formuláře pomocí šablon stylů CSS. 

Pomocí protokolu HTTPS nezabrání útoku proti útokům CSRF. Může odesílat škodlivé weby `https://www.good-banking-site.com/` stejným způsobem jako ho můžete odeslat požadavek nezabezpečené požadavku.

Některými útoky cílové koncové body, které reagují na požadavky GET, v takovém případě značka obrázku lze provést akci. Tato forma útoku je běžné v lokalitách fórum, které umožňují bitové kopie, ale blokovat JavaScript. Aplikace, které změní stav pro požadavky GET, kde jsou změny proměnné nebo prostředky, jsou ohroženy útoky se zlými úmysly. **Jsou nezabezpečené požadavky GET, které změní stav. Osvědčeným postupem je nikdy změny stavu na požadavek GET.**

Útoky proti útokům CSRF jsou možné u webových aplikací, které používají soubory cookie pro ověřování, protože:

* Prohlížeče ukládají soubory cookie vydané webové aplikace.
* Uložené soubory cookie obsahují soubory cookie relace pro ověřené uživatele.
* Prohlížeče odeslat všechny soubory cookie přiřazené doméně do webové aplikace každou žádost bez ohledu na to, jak byl vygenerován požadavek na aplikaci v prohlížeči.

Nicméně nejsou proti útokům CSRF útoky omezené na zneužitím soubory cookie. Ověřování Basic a Digest jsou například také snadno napadnutelný. Jakmile se uživatel přihlásí pomocí ověřování základní nebo Digest, v prohlížeči automaticky odesílá přihlašovací údaje dokud relace&dagger; ukončí.

&dagger;V tomto kontextu *relace* odkazuje na straně klienta relace, během které je uživatel ověřený. Je relace na straně serveru, které nejsou nebo [ASP.NET Core relace Middleware](xref:fundamentals/app-state).

Uživatelé mohou chránit proti ohrožení zabezpečení proti útokům CSRF provedením opatření:

* Přihlášení z webové aplikace po dokončení jejich používání.
* Vymazat prohlížeče soubory cookie pravidelně.

Ohrožení zabezpečení proti útokům CSRF jsou však zásadně problém s webovou aplikaci, ne koncový uživatel.

## <a name="authentication-fundamentals"></a>Základy ověřování

Ověřování na základě souborů cookie je Oblíbené formu ověřování. Systémy ověřování na základě tokenu se popularita, hlavně pro jednostránkové aplikace (SPA).

### <a name="cookie-based-authentication"></a>Ověřování na základě souborů cookie

Při ověření uživatele pomocí uživatelského jména a hesla, že se vystaví token, obsahující lístek ověřování, který slouží k ověřování a autorizaci. Token je uloženo jako soubor cookie, který doprovází každý požadavek klienta umožňuje. Generování a ověřování tento soubor cookie provádí Middleware ověřování souborů Cookie. [Middleware](xref:fundamentals/middleware/index) serializuje hlavní název uživatele do šifrovaného souboru cookie. Na následné žádosti, middleware ověří, že soubor cookie, znovu vytvoří objekt a přiřadí objekt k [uživatele](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) vlastnost [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

### <a name="token-based-authentication"></a>Ověřování na základě tokenu

Pokud je uživatel ověřen, budou se vystaví token, (ne antiforgery token). Token obsahuje uživatelské informace ve formě [deklarace identity](/dotnet/framework/security/claims-based-identity-model) nebo odkaz na token, který odkazuje na stav uživatele v aplikaci udržuje aplikace. Když se uživatel pokusí o přístup k prostředku, které vyžadují ověřování, je token odesílají do aplikace s hlavičku další ověřování ve formuláři tokenu nosiče. Díky tomu bezstavové aplikace. V každé následné žádosti o token je předán v požadavku pro ověřování na straně serveru. Tento token není *šifrované*; má *kódovaný*. Na serveru je token dekódovat pro přístup k jeho informace. Odeslat token na následné žádosti, ukládat do místního úložiště prohlížeče token. Nemusíte dělat starosti o ohrožení zabezpečení proti útokům CSRF, pokud token je uložený v místním úložišti v prohlížeči. Proti útokům CSRF není vážný, pokud token je uložen v souboru cookie.

### <a name="multiple-apps-hosted-at-one-domain"></a>Více aplikací, které jsou hostované v jedné doméně

Sdílené hostitelská prostředí jsou citlivé na zneužití relace přihlášení proti útokům CSRF a jiným útokům.

I když `example1.contoso.net` a `example2.contoso.net` jsou různých hostitelích, je vztah implicitní vztah důvěryhodnosti mezi hostiteli v rámci `*.contoso.net` domény. Tento vztah důvěryhodnosti implicitní umožňuje potenciálně nedůvěryhodní hostitelé ovlivnit vzájemně soubory cookie (stejného původu zásady, které řídí požadavky AJAX nemáte platit pro soubory cookie HTTP).

Není sdílením domén se dá zabránit útokům, které využívají důvěryhodné soubory cookie mezi aplikacemi, které jsou hostované ve stejné doméně. Při každé aplikace je hostované na vlastní domény, neexistuje žádný vztah důvěry implicitní souboru cookie zneužít.

## <a name="aspnet-core-antiforgery-configuration"></a>Konfigurace antiforgery ASP.NET Core

> [!WARNING]
> ASP.NET Core implementuje antiforgery pomocí [ochranu dat ASP.NET Core](xref:security/data-protection/introduction). Zásobník ochrany dat musí být nakonfigurováno pro práci v serverové farmě. V tématu [konfiguraci ochrany dat](xref:security/data-protection/configuration/overview) Další informace.

V technologii ASP.NET Core 2.0 nebo novější [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) vloží antiforgery tokeny do elementů formuláře HTML. Následující kód v souboru nástroje Razor automaticky vygeneruje antiforgery tokenů:

```cshtml
<form method="post">
    ...
</form>
```

Wijsova, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generuje tokeny antiforgery ve výchozím nastavení, pokud není formuláře metoda GET.

Automatické generování antiforgery tokeny pro prvků formuláře HTML se stane, když `<form>` značka obsahuje `method="post"` platí atribut a z následujících:

  * Atribut akce je prázdná (`action=""`).
  * Není zadaný atribut akce (`<form method="post">`).

Je možné zakázat automatické generování antiforgery tokeny pro prvků formuláře HTML:

* Explicitně zakázat antiforgery tokeny se `asp-antiforgery` atribut:

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* Form element je vyjádřit výslovný souhlas mimo značku Pomocníci pomocí pomocné rutiny značky [! výslovný nesouhlas s symbol](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* Odeberte `FormTagHelper` ze zobrazení. `FormTagHelper` Můžete odstranit ze zobrazení přidáním následující direktiva do zobrazení Razor:

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Stránky Razor](xref:mvc/razor-pages/index) jsou automaticky chráněny před XSRF nebo proti útokům CSRF. Další informace najdete v tématu [XSRF/proti útokům CSRF a stránky Razor](xref:mvc/razor-pages/index#xsrf).

Nejběžnější přístupem k obraně proti útokům proti útokům CSRF je použití *tokenu vzor synchronizátor* (STP). STP se používá, když uživatel požádá o stránku s dat formuláře:

1. Server odešle token spojené s identitou aktuálního uživatele do klienta.
1. Klient odešle zpět token k serveru pro ověření.
1. Pokud server přijme token, který neodpovídá identitu ověřeného uživatele, požadavek byl odmítnut.

Token je jedinečný a nepředvídatelným. Token lze také zajistit správné pořadí řady požadavků (například zajistíte pořadí požadavek: stránka 1 &ndash; stránka 2 &ndash; stránka 3). Všechny formuláře v ASP.NET MVC jádra a stránky Razor šablony generování antiforgery tokenů. Následující pár příkladů, zobrazení generování antiforgery tokenů:

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

Explicitně přidat antiforgery token `<form>` element bez použití značky Pomocníci s pomocné rutiny HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

V každém z předchozích případech se přidá ASP.NET Core skryté pole formuláře podobný následujícímu:

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core obsahuje tři [filtry](xref:mvc/controllers/filters) pro práci s antiforgery tokenů:

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Antiforgery možnosti

Přizpůsobení [antiforgery možnosti](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) v `Startup.ConfigureServices`:

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| Možnost | Popis |
| ------ | ----------- |
| [Soubor cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Určuje nastavení používaná k vytvoření antiforgery soubory cookie. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Doména souboru cookie. Použije se výchozí hodnota `null`. Tato vlastnost je zastaralá a bude v budoucí verzi odebrána. Doporučená alternativa je Cookie.Domain. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Název souboru cookie. Pokud není nastavena, systém generuje jedinečný název začíná [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery."). Tato vlastnost je zastaralá a bude v budoucí verzi odebrána. Doporučená alternativa je Cookie.Name. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Cesta, nastavte v souboru cookie. Tato vlastnost je zastaralá a bude v budoucí verzi odebrána. Doporučená alternativa je Cookie.Path. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Název pole Skrytá formuláře antiforgery systém použije k vykreslení antiforgery tokeny v zobrazeních. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Název hlavičky, která používá antiforgery systém. Pokud `null`, je za pouze data formuláře. |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | Určuje, zda je požadován protokol SSL antiforgery systémem. Pokud `true`, požadavky bez SSL selžou. Použije se výchozí hodnota `false`. Tato vlastnost je zastaralá a bude v budoucí verzi odebrána. Doporučená alternativa je nastavit Cookie.SecurePolicy. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Určuje, jestli se má potlačit generování `X-Frame-Options` záhlaví. Ve výchozím nastavení je generována záhlaví s hodnotou "SAMEORIGIN". Použije se výchozí hodnota `false`. |

Další informace najdete v tématu [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).

## <a name="configure-antiforgery-features-with-iantiforgery"></a>Konfigurace funkcí antiforgery s IAntiforgery

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) poskytuje rozhraní API pro konfiguraci antiforgery funkcí. `IAntiforgery` mohou být vyžádány v `Configure` metodu `Startup` třídy. Následující příklad používá middleware z aplikace domovské stránky k vygenerování tokenu antiforgery a jeho odeslání v odpovědi jako soubor cookie (pomocí výchozí úhlová konvence popsané dál v tomto tématu):

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a>Požadovat antiforgery ověření

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) je filtr akce, který lze použít na jednotlivé akci, kontroler, nebo globálně. Požadavky na akce, které tento filtr použít jsou zablokovány, pokud požadavek obsahuje platný antiforgery token.

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

`ValidateAntiForgeryToken` Atribut vyžaduje, aby token pro požadavky na metody akce upraví, včetně požadavky HTTP GET. Pokud `ValidateAntiForgeryToken` atribut se používá v rámci řadičů aplikace, je možné přepsat pomocí `IgnoreAntiforgeryToken` atribut.

> [!NOTE]
> ASP.NET Core nepodporuje přidání antiforgery tokenů pro požadavky GET automaticky.

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>Automaticky ověření antiforgery tokeny pro pouze unsafe metody HTTP

Aplikace ASP.NET Core negenerovat zprávy o antiforgery tokeny pro bezpečné metody HTTP (GET, HEAD, možnosti a trasování). Místo použití široce `ValidateAntiForgeryToken` atribut a pak jeho pomocí přepsání `IgnoreAntiforgeryToken` atributy, [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) atribut je možné použít. Tento atribut funguje stejně jako na `ValidateAntiForgeryToken` atribut, s tím rozdílem, že není třeba tokeny na požadavky vytvořené pomocí následujících metod HTTP:

* GET
* HEAD
* MOŽNOSTI
* TRACE

Doporučujeme používat `AutoValidateAntiforgeryToken` široce pro scénáře – rozhraní API. Tím se zajistí, že jsou ve výchozím nastavení chráněna POST akce. Alternativou je ignorovat antiforgery tokeny ve výchozím nastavení, pokud `ValidateAntiForgeryToken` se použije u jednotlivých metod akcí. Ho má více pravděpodobně v tomto scénáři pro metodu POST akce pro být ponecháno nechráněné omylem, a aplikace bude zranitelný vůči útoku proti útokům CSRF. Všechny příspěvky by měli poslat antiforgery token.

Rozhraní API nemají automatický mechanismus pro odesílání bez souborů cookie součást tokenu. Implementace pravděpodobně závisí na implementaci kódu klienta. Níže jsou uvedeny některé příklady:

Příklad úrovni třídy:

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Globální příklad:

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a>Přepsání globální nebo antiforgery atributů kontroleru

[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtru se používá k vyloučení potřeby antiforgery token pro danou akci (nebo řadič). Při použití, přepíše tento filtr `ValidateAntiForgeryToken` a `AutoValidateAntiforgeryToken` filtry zadané na vyšší úrovni (globálně nebo na řadič).

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a>Po ověření obnovovacích tokenů

Tokeny musí aktualizovat po ověření uživatele přesměrováním uživatele na zobrazení nebo stránku stránky Razor.

## <a name="javascript-ajax-and-spas"></a>JavaScript AJAX a SPA

V tradiční aplikace založené na HTML antiforgery tokeny jsou předána serveru pomocí skrytého pole. Moderní aplikace založené na jazyce JavaScript a SPA mnoho požadavků probíhají prostřednictvím kódu programu. Tyto požadavky AJAX může používat další metody (například hlavičky požadavku nebo soubory cookie) k odesílání token.

Pokud se soubory cookie používají k ukládání tokeny ověřování a k ověřování žádostí o rozhraní API na serveru, proti útokům CSRF je na potenciální problém. Pokud místní úložiště se používá k ukládání tokenu, může ohrožení zabezpečení proti útokům CSRF omezeny, protože hodnoty z místního úložiště nejsou automaticky odesílá na server s každou žádostí. Proto používá místní úložiště k uložení antiforgery token do klienta a odeslání tokenu, protože hlavička požadavku je doporučený postup.

### <a name="javascript"></a>JavaScript

Pomocí jazyka JavaScript se zobrazeními, token lze vytvořit pomocí služby z v rámci zobrazení. Vložit [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) služby do zobrazení a volání [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

Tento přístup se eliminuje potřeba k řešení přímo nastavení souborů cookie ze serveru nebo jejich čtení z klienta.

V předchozím příkladu používá JavaScript načíst hodnotu skryté pole hlavičky AJAX POST.

JavaScript můžete také přístup k tokeny v souborech cookie a použít k vytvoření záhlaví s hodnotou tokenu souboru cookie obsah.

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Za předpokladu, že skript požadavky odeslat token v hlavičce názvem `X-CSRF-TOKEN`, konfigurovat službu antiforgery hledání `X-CSRF-TOKEN` hlavičky:

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

Následující příklad používá JavaScript aby požadavek AJAX s příslušnou hlavičky:

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a>AngularJS

AngularJS používá vytváření adresu proti útokům CSRF. Pokud server odešle soubor cookie s názvem `XSRF-TOKEN`, AngularJS `$http` služby přidá hodnota souboru cookie záhlaví při odesílání požadavku na server. Tento proces je automatické. Záhlaví nemusí být explicitně nastaveno. Název hlavičky je `X-XSRF-TOKEN`. Server by měl zjistit tuto hlavičku a ověřit jeho obsah.

Pro rozhraní API ASP.NET Core ve spolupráci s touto konvencí:

* Konfigurace aplikace k poskytování token v souboru cookie s názvem `XSRF-TOKEN`.
* Konfigurovat službu antiforgery hledání hlavičku s názvem `X-XSRF-TOKEN`.

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>Rozšíření antiforgery

[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) typ mohou vývojáři rozšířit chování systému anti-proti útokům CSRF odezvy další data v každý token. [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) metoda je volána pokaždé, když se vygeneruje token pole a vložené návratovou hodnotu v rámci vygenerovaný token. Implementátor může vrátit časové razítko, hodnotu nonce nebo jakoukoli jinou hodnotu a pak zavolají [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) ověřit tato data při ověření tokenu. Uživatelské jméno klienta je již vložených do generovaných tokenů, takže není nutné zahrnout tyto informace. Pokud token obsahuje dodatečná data, ale ne `IAntiForgeryAdditionalDataProvider` je nakonfigurován, nejsou dodatečná data ověřena.

## <a name="additional-resources"></a>Další zdroje

* [Proti útokům CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [otevřete projekt webové aplikace zabezpečení](https://www.owasp.org/index.php/Main_Page) (OWASP).
