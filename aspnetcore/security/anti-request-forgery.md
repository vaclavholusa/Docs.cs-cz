---
title: "Prevence útoků (XSRF/proti útokům CSRF) padělání požadavku posílaného mezi weby v ASP.NET Core"
author: steve-smith
description: "Prevence útoků (XSRF/proti útokům CSRF) padělání požadavku posílaného mezi weby v ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 079c36535b8c9e7229952a2f7bcd53174effa6af
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Prevence útoků (XSRF/proti útokům CSRF) padělání požadavku posílaného mezi weby v ASP.NET Core

[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), a [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-attack-does-anti-forgery-prevent"></a>Jaké útoku proti padělání zabránit?

Padělání požadavku posílaného mezi weby (také označované jako XSRF nebo proti útokům CSRF, vyslovováno *najdete procházení*) je útok na hostované webové aplikace, které můžete vytvořit web ovlivnit interakce mezi prohlížeče klienta a webová stránka, která důvěřuje Tento prohlížeč. Tyto útoky jsou možné, protože webových prohlížečů odesílat některé typy ověřování tokenů automaticky s každou žádost na web. Tato forma zneužití je taky říká *jedním kliknutím útoku* nebo jako *relace jízda*, protože je útok trvá výhod uživatel ověřený relace.

Příklad útoku proti útokům CSRF:

1. Přihlášení uživatele do `www.example.com`, ověřování pomocí formulářů.
2. Server ověřuje uživatele a vydá odpovědi, která obsahuje soubor cookie ověřování.
3. Uživatel navštíví škodlivé weby.

   Škodlivé weby obsahuje formuláře HTML, který je podobný následujícímu:

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

Všimněte si, že akce formuláře požadavky na server, snadno napadnutelný, aby škodlivé weby. Toto je část "webů" proti útokům CSRF.

4. Uživatel kliknutím na tlačítko pro odeslání. V prohlížeči automaticky zahrne ověřovacího souboru cookie pro požadovanou doménu (citlivé lokality v tomto případě) s požadavkem.
5. Žádost se spustí na serveru s kontext ověřování uživatele a můžete provádět žádnou akci, která ověřený uživatel může provádět.

Tento příklad vyžaduje, aby uživatel klepnutím na tlačítko formuláře. Na stránce škodlivého může:

* Spusťte skript, který automaticky odešle formulář.
* Odešle odeslání formuláře jako požadavek AJAX. 
* Skrytá formuláře pomocí šablon stylů CSS. 

Použití protokolu SSL nezabrání útoku proti útokům CSRF, může odesílat škodlivé weby `https://` požadavku. 

Některými útoky cílové lokality koncové body, které reagují na `GET` požadavků, ve kterých případ značka obrázku lze použít k provedení akce (Tato forma útoku je běžné v lokalitách fórum, které umožňují bitové kopie, ale blokovat JavaScript). Aplikace, které změní stav s `GET` požadavky se stát terčem před nebezpečnými útoky.

Útoky proti útokům CSRF je možné proti webové stránky, které používají soubory cookie pro ověřování, protože prohlížeče odesílají všechny relevantní soubory cookie k cílovému webu. Útoky proti útokům CSRF však nejsou omezeny na zneužitím soubory cookie. Ověřování Basic a Digest jsou například také snadno napadnutelný. Po přihlášení uživatele pomocí ověřování základní nebo Digest, v prohlížeči automaticky odesílá přihlašovací údaje až do ukončení relace.

Poznámka: V tomto kontextu *relace* odkazuje na straně klienta relace, během které je uživatel ověřený. Je relace na straně serveru, které nejsou nebo [relace middleware](xref:fundamentals/app-state).

Uživatelé můžou chránit proti ohrožení zabezpečení proti útokům CSRF pomocí:
* Protokolování z webových stránek, při jejich dokončení jejich používání.
* Pravidelně vymazání svého prohlížeče soubory cookie.

Ohrožení zabezpečení proti útokům CSRF jsou však zásadně problém s webovou aplikaci, ne koncový uživatel.

## <a name="how-does-aspnet-core-mvc-address-csrf"></a>Jak ASP.NET MVC základní adresa proti útokům CSRF?

> [!WARNING]
> ASP.NET Core implementuje pomocí proti request padělání [zásobníku ochrany dat ASP.NET Core](xref:security/data-protection/introduction). Ochrana dat ASP.NET Core musí být nakonfigurováno pro práci v serverové farmě. V tématu [konfiguraci ochrany dat](xref:security/data-protection/configuration/overview) Další informace.

Konfigurace ochrany dat proti request padělání výchozí ASP.NET Core 

V technologii ASP.NET 2.0 MVC jádra [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) vloží tokeny proti zfalšování pro prvků formuláře HTML. Například následující kód v souboru nástroje Razor bude automaticky generovat tokeny proti zfalšování:

```html
<form method="post">
  <!-- form markup -->
</form>
```

Automatické generování tokenů proti padělání prvků formuláře HTML se stane, když:

* `form` Značka obsahuje `method="post"` atribut a

  * Atribut akce je prázdný. ( `action=""`) NEBO
  * Není zadána atributu akce. (`<form method="post">`)

Můžete zakázat automatické generování tokenů proti padělání pro prvků formuláře HTML pomocí:

* Výslovně zakazuje `asp-antiforgery`. Příklad

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* OPT element form mimo značku Pomocníci pomocí pomocné rutiny značka [! výslovný nesouhlas s symbol](xref:mvc/views/tag-helpers/intro#opt-out).

 ```html
  <!form method="post">
  </!form>
  ```

* Odeberte `FormTagHelper` ze zobrazení. Můžete odebrat `FormTagHelper` ze zobrazení přidáním následující direktivu Razor zobrazení:

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Stránky Razor](xref:mvc/razor-pages/index) jsou automaticky chráněny před XSRF nebo proti útokům CSRF. Nemáte k zápisu žádný další kód. V tématu [XSRF/proti útokům CSRF a stránky Razor](xref:mvc/razor-pages/index#xsrf) Další informace.

Nejběžnější přístup k obraně proti útokům proti útokům CSRF je token vzor synchronizátor (STP). STP je technika používá, když uživatel požádá o stránku s dat formuláře. Server odešle token spojené s identitou aktuálního uživatele do klienta. Klient odešle zpět token k serveru pro ověření. Pokud server přijme token, který neodpovídá identitu ověřeného uživatele, požadavek byl odmítnut. Token je jedinečný a nepředvídatelným. Token mohou sloužit také zajistit správné pořadí řady požadavků (stránka 2, která předchází stránka 3 předchází zajistit stránka 1). Všechny formuláře v šablony ASP.NET Core MVC generování antiforgery tokenů. Následující dva příklady zobrazení logiky generování antiforgery tokenů:

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

Můžete explicitně přidat antiforgery token ``<form>`` element bez použití značky Pomocníci s pomocné rutiny HTML ``@Html.AntiForgeryToken``:


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

V každém z předchozích případech se přidá ASP.NET Core skryté pole formuláře podobný následujícímu:
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

ASP.NET Core obsahuje tři [filtry](xref:mvc/controllers/filters) pro práci s antiforgery tokeny: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, a ``IgnoreAntiforgeryToken``.

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a>ValidateAntiForgeryToken

``ValidateAntiForgeryToken`` Je filtr akce, který lze použít na jednotlivé akci, kontroler, nebo globálně. Požadavky na akce, které tento filtr použít se zablokuje, pokud požadavek obsahuje platný antiforgery token.

```c#
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

``ValidateAntiForgeryToken`` Atribut vyžaduje, aby token pro požadavky na metody akce upraví, včetně `HTTP GET` požadavky. Pokud použijete ho široce, můžete přepsat její ``IgnoreAntiforgeryToken`` atribut.

### <a name="autovalidateantiforgerytoken"></a>AutoValidateAntiforgeryToken

Aplikace ASP.NET Core obecně negenerovat antiforgery tokeny pro bezpečné metody HTTP (GET, HEAD, možnosti a trasování). Místo použití široce ``ValidateAntiForgeryToken`` atribut a pak jeho pomocí přepsání ``IgnoreAntiforgeryToken`` atributy, můžete použít ``AutoValidateAntiforgeryToken`` atribut. Tento atribut funguje stejně jako na ``ValidateAntiForgeryToken`` atribut, s tím rozdílem, že není třeba tokeny na požadavky vytvořené pomocí následujících metod HTTP:

* GET
* HEAD
* MOŽNOSTI
* TRACE

Doporučujeme použít ``AutoValidateAntiforgeryToken`` široce pro scénáře – rozhraní API. Tím se zajistí, že jsou ve výchozím nastavení chráněna vaše akce POST. Alternativou je ignorovat antiforgery tokeny ve výchozím nastavení, pokud ``ValidateAntiForgeryToken`` se použije k metodě jednotlivé akce. Je pravděpodobnější v tomto scénáři pro metodu POST akce na levé straně nechráněné, a aplikace bude zranitelný vůči útoku proti útokům CSRF. I anonymní PŘÍSPĚVCÍCH by měli poslat antiforgery token.

Poznámka: Rozhraní API nemáte automatický mechanismus pro odesílání bez souborů cookie součást tokenu; implementace pravděpodobně závisí na implementaci kódu klienta. Níže jsou uvedeny některé příklady.


Příklad (třída úroveň):

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Příklad (globální):

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a>IgnoreAntiforgeryToken

``IgnoreAntiforgeryToken`` Filtru se používá k vyloučení potřeby token antiforgery být pro danou akci (nebo řadič). Při použití, přepíše tento filtr ``ValidateAntiForgeryToken`` nebo ``AutoValidateAntiforgeryToken`` filtry zadané na vyšší úrovni (globálně nebo na řadič).

```c#
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

## <a name="javascript-ajax-and-spas"></a>JavaScript AJAX a SPA

V tradiční aplikace založené na HTML antiforgery tokeny jsou předána serveru pomocí skrytého pole. Moderní aplikace založené na jazyce JavaScript a jednostránkové aplikace (SPA) jsou vytvářeny požadavky mnoho prostřednictvím kódu programu. Tyto požadavky AJAX může používat další metody (například hlavičky požadavku nebo soubory cookie) k odesílání token. Pokud se soubory cookie používají k ukládání tokeny ověřování a k ověřování žádostí o rozhraní API na serveru, proti útokům CSRF bude na potenciální problém. Ale pokud místní úložiště se používá k ukládání tokenu, ohrožení zabezpečení proti útokům CSRF může být omezeny, vzhledem k tomu, že hodnoty z místního úložiště nejsou automaticky odesílá na server s každou novou žádost. Proto používá místní úložiště k uložení antiforgery token do klienta a odeslání tokenu, protože hlavička požadavku je doporučený postup.

### <a name="angularjs"></a>AngularJS

AngularJS používá vytváření adresu proti útokům CSRF. Pokud server odešle soubor cookie s názvem ``XSRF-TOKEN``, úhlová ``$http`` služba přidá hodnotu z tento soubor cookie do záhlaví při odesílání požadavku na tento server. Tento proces je automatické; nemusíte explicitně nastaven záhlaví. Název hlavičky je ``X-XSRF-TOKEN``. Server by měl zjistit tuto hlavičku a ověřit jeho obsah.

Pro rozhraní API ASP.NET Core ve spolupráci s touto konvencí:

* Konfigurace aplikace k poskytování token v souboru cookie s názvem``XSRF-TOKEN``
* Konfigurovat službu antiforgery hledání hlavičku s názvem``X-XSRF-TOKEN``

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Ukázkové zobrazení](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).

### <a name="javascript"></a>JavaScript

Pomocí jazyka JavaScript se zobrazeními, můžete vytvořit token pomocí služby z v rámci zobrazení. Uděláte to tak, Vložit `Microsoft.AspNetCore.Antiforgery.IAntiforgery` služby do zobrazení a volání `GetAndStoreTokens`, jak je znázorněno:

[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]

Tento přístup se eliminuje potřeba k řešení přímo nastavení souborů cookie ze serveru nebo jejich čtení z klienta.

JavaScript můžete také přístup k tokeny, které jsou součástí soubory cookie a pak použít obsah souboru cookie pro vytvoření záhlaví s hodnotou tokenu, jak je uvedeno níže.

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Pak za předpokladu, že vytvoříte svůj skript požadavky odeslat token v hlavičce názvem ``X-CSRF-TOKEN``, konfigurovat službu antiforgery hledání ``X-CSRF-TOKEN`` hlavičky:

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

Následující příklad používá jQuery požadavek AJAX s příslušnou hlavičky:

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a>Konfigurace Antiforgery

`IAntiforgery`poskytuje rozhraní API pro konfiguraci antiforgery systému. Může být požadována v `Configure` metodu `Startup` třídy. Následující příklad používá middleware z aplikace domovské stránky k vygenerování tokenu antiforgery a jeho odeslání v odpovědi jako soubor cookie (pomocí výchozí úhlová konvence popsané výše):


```c#
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a>Možnosti

Můžete přizpůsobit [antiforgery možnosti](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) v `ConfigureServices`:

```c#
services.AddAntiforgery(options => 
{
  options.CookieDomain = "mydomain.com";
  options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
  options.CookiePath = "Path";
  options.FormFieldName = "AntiforgeryFieldname";
  options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
  options.RequireSsl = false;
  options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|Možnost        | Popis |
|------------- | ----------- |
|CookieDomain  | Doména souboru cookie. Použije se výchozí hodnota `null`. |
|CookieName    | Název souboru cookie. Pokud není nastavena, systém bude generovat jedinečný název začíná `DefaultCookiePrefix` (". AspNetCore.Antiforgery."). |
|CookiePath    | Cesta, nastavte v souboru cookie. |
|FormFieldName | Název pole Skrytá formuláře antiforgery systém použije k vykreslení antiforgery tokeny v zobrazeních. |
|HeaderName    | Název hlavičky, která používá antiforgery systém. Pokud `null`, systém bude považovat jenom data formuláře. |
|requireSsl    | Určuje, zda je požadován protokol SSL antiforgery systémem. Použije se výchozí hodnota `false`. Pokud `true`, bez SSL požadavky nezdaří. |
|SuppressXFrameOptionsHeader  | Určuje, jestli se má potlačit generování `X-Frame-Options` záhlaví. Ve výchozím nastavení je generována záhlaví s hodnotou "SAMEORIGIN". Použije se výchozí hodnota `false`. |

V tématu https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions Další informace.

### <a name="extending-antiforgery"></a>Rozšíření Antiforgery

[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) typ mohou vývojáři rozšířit chování systému anti-XSRF odezvy další data v každý token. [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) metoda je volána pokaždé, když se vygeneruje token pole a vložené návratovou hodnotu v rámci vygenerovaný token. Implementátor může vrátit časové razítko, hodnotu nonce nebo jakoukoli jinou hodnotu a pak zavolají [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) ověřit tato data při ověření tokenu. Uživatelské jméno klienta je již vložených do generovaných tokenů, takže není nutné zahrnout tyto informace. Pokud token obsahuje dodatečná data, ale ne `IAntiForgeryAdditionalDataProvider` byl nakonfigurován, nejsou dodatečná data ověřena.

## <a name="fundamentals"></a>Základy

Útoky proti útokům CSRF spoléhají na výchozí chování prohlížeče odeslat soubory cookie přidružené k doméně s každou žádost odeslanou do této domény. Tyto soubory cookie jsou uloženy v prohlížeči. Často obsahují soubory cookie relace pro ověřené uživatele. Ověřování na základě souborů cookie je Oblíbené formu ověřování. Ověřování na základě tokenu systémy mají byla popularita, hlavně pro SPA a v dalších scénářích "inteligentní klient".

### <a name="cookie-based-authentication"></a>Ověřování na základě souborů cookie

Jakmile uživatel byl ověřen pomocí uživatelského jména a hesla, budou se vystaví token, který slouží k jejich identifikaci a ověření, které byly ověřeny. Token je uloženo jako soubor cookie, který doprovází každý požadavek klienta umožňuje. Generování a ověřování tento soubor cookie je potřeba middleware ověřování souborů cookie. Základní technologie ASP.NET poskytuje soubor cookie [middleware](xref:fundamentals/middleware/index) který serializuje hlavní název uživatele do šifrovaného souboru cookie a pak na následné žádosti, ověří souboru cookie, znovu vytvoří objekt a přiřadí ji k `User` vlastnost `HttpContext`.

Pokud soubor cookie se používá, ověřovacího souboru cookie je kontejner pro ověřovací lístek. Lístek se předá jako hodnota souboru cookie pro ověřování formulářů s každou žádostí a ověřování pomocí formulářů, na serveru, používá k identifikaci ověřeného uživatele.

Když je uživatel přihlášen do systému, uživatelské relace je vytvořena na straně serveru a je uložen v databázi nebo jiných trvalého úložiště. Systém vygeneruje klíč relace, která odkazuje na skutečnou relace v úložišti dat a pak se odešle jako soubor cookie straně klienta. Webový server zkontroluje tento klíč relace vždy, když uživatel požádá o prostředek, který vyžaduje ověření. Systém kontroluje, zda relace uživatele má oprávnění k přístupu k požadovanému zdroji. Pokud ano, pokračuje v požadavku. Žádost, jinak vrátí jako není autorizovaný. V tento postup soubory cookie se použijí zpřístupnění aplikace se zdají být stavová, vzhledem k tomu, že je schopna "nezapomeňte", má uživatel ověřený se serverem.

### <a name="user-tokens"></a>Tokeny uživatele

Ověřování na základě tokenu není relace uložit na server. Když je uživatel přihlášen, že se vystaví token, (ne antiforgery token). Tento token obsahuje data, která je nutné k ověření tokenu. Také obsahuje informace o uživateli ve formě [deklarace identity](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model). Pokud chce uživatel pro přístup k prostředkům serveru, které vyžadují ověřování, token je odeslána na server s hlavičku další ověřování ve formuláři nosiče {token}. Díky aplikaci bezstavové vzhledem k tomu, že v každé následné žádosti o token je předán v požadavku pro ověřování na straně serveru. Tento token není *šifrované*; je spíše *kódovaný*. Na straně serveru může dekódovat tokenu pro přístup k nezpracované informace v tokenu. Odeslat token v následných žádostí, buď uložit v místním úložišti v prohlížeči nebo v souboru cookie. Nemusíte si dělat starosti o ohrožení zabezpečení XSRF Pokud token je uložený v místním úložišti, ale je problém, pokud je token je uložen v souboru cookie.

### <a name="multiple-applications-are-hosted-in-one-domain"></a>Více aplikací, které jsou hostované v jedné doméně

I když `example1.cloudapp.net` a `example2.cloudapp.net` jsou různých hostitelích, je vztah implicitní vztah důvěryhodnosti mezi hostiteli v rámci `*.cloudapp.net` domény. Tento vztah důvěryhodnosti implicitní umožňuje potenciálně nedůvěryhodní hostitelé ovlivnit vzájemně soubory cookie (stejného původu zásady, které řídí požadavky AJAX nemáte platit pro soubory cookie HTTP). Modul runtime ASP.NET Core poskytuje některé zmírnění v tom, že uživatelské jméno se vloží do tokenu pole. I když je možné přepsat token relace škodlivý subdomény, nelze vygenerovat token pro daného uživatele platné pole. Když jsou hostované v takovém prostředí, rutiny předdefinované anti-XSRF stále nelze bránit proti zneužití relace nebo přihlášení proti útokům CSRF útoky. Sdílené hostitelská prostředí jsou vunerable zneužití relace, přihlášení proti útokům CSRF a jiným útokům.

### <a name="additional-resources"></a>Další zdroje

* [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [otevřete projekt webové aplikace zabezpečení](https://www.owasp.org/index.php/Main_Page) (OWASP).
