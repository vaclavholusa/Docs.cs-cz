---
title: "Autorizace uživatele na základě zásad"
author: rick-anderson
description: "Tento dokument vysvětluje, jak vytvořit a použít vlastní autorizační zásady obslužné rutiny v aplikaci ASP.NET Core."
keywords: "ASP.NET Core, ověřování, vlastní zásady, zásady autorizace"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 0281d054204a11acc2cf11cf5fca23a8f70aad8e
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="custom-policy-based-authorization"></a>Autorizace uživatele na základě zásad

<a name="security-authorization-policies-based"></a>

Pod zahrnuje [role autorizace](roles.md) a [deklarací autorizace](claims.md) Zkontrolujte použití požadavku, obslužné rutiny pro požadavek a předem nakonfigurované zásady. Tyto stavební bloky umožňují express hodnocení autorizační kód, aby vám umožnil bohatší, opakovaně využívat, a snadno testovat autorizace strukturou.

Zásad autorizace se skládá z jednoho nebo více požadavků a zaregistrován při spuštění aplikace v rámci daná konfigurace služby ověřování v `ConfigureServices` v *Startup.cs* souboru.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });
}
```

Zde vidíte, že u jednoho požadavku, že o minimálním stářím, který byl předán jako parametr požadavek na vytvoření zásadu "Over21".

Zásady se použijí, pomocí `Authorize` atribut tak, že zadáte název zásady, například;

```csharp
[Authorize(Policy="Over21")]
public class AlcoholPurchaseRequirementsController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

## <a name="requirements"></a>Požadavky

Požadavek na autorizaci je kolekce dat parametry, které zásadu lze použít k vyhodnocení aktuální hlavní název uživatele. V našich minimálním stářím zásad je požadavek na našich jeden parametr minimálním stářím. Požadavek musí implementovat `IAuthorizationRequirement`. Toto je prázdná, rozhraní značky. Parametrizované minimálním stářím požadavek může být implementováno takto;

```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; private set; }
    
    public MinimumAgeRequirement(int minimumAge)
    {
        MinimumAge = minimumAge;
    }
}
```

Požadavek není nutné mít dat nebo vlastností.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Obslužné rutiny autorizace

Obslužné rutiny autorizace je zodpovědná za vyhodnocení všechny vlastnosti požadavek. Obslužná rutina ověřování musí vyhodnocuje je s zadaný `AuthorizationHandlerContext` rozhodnout, jestli je povolené ověřování. Může mít požadavek [více obslužných rutin](policies.md#security-authorization-policies-based-multiple-handlers). Obslužné rutiny musí dědit `AuthorizationHandler<T>` tam, kde T představuje požadavek zpracovává.

<a name="security-authorization-handler-example"></a>

Obslužná rutina minimálním stářím může vypadat například takto:

```csharp
public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        if (!context.User.HasClaim(c => c.Type == ClaimTypes.DateOfBirth &&
                                   c.Issuer == "http://contoso.com"))
        {
            // .NET 4.x -> return Task.FromResult(0);
            return Task.CompletedTask;
        }

        var dateOfBirth = Convert.ToDateTime(context.User.FindFirst(
            c => c.Type == ClaimTypes.DateOfBirth && c.Issuer == "http://contoso.com").Value);

        int calculatedAge = DateTime.Today.Year - dateOfBirth.Year;
        if (dateOfBirth > DateTime.Today.AddYears(-calculatedAge))
        {
            calculatedAge--;
        }

        if (calculatedAge >= requirement.MinimumAge)
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

Ve výše uvedeném kódu nejprve podíváme pro případ, že má aktuální uživatel hlavní datum narození deklarace identity, který byl vydán vystavitele víme a vztah důvěryhodnosti. Pokud chybí deklarace jsme nejde autorizovat, proto vrátíme. Pokud budeme mít deklaraci identity, jsme zjistit, kolik je uživatel, a pokud nebudou splňovat předaná v požadavek na minimální věk pak autorizace proběhlo úspěšně. Po úspěšné autorizace říkáme `context.Succeed()` předávání v požadavku, že proběhlo úspěšně jako parametr.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Obslužná rutina registrace
Obslužné rutiny musí být zaregistrovaný v kolekci služby během konfigurace, například;

```csharp

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });

    services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
}
```

Každý obslužná rutina se přidá do kolekce služeb pomocí `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` předávání v třídě obslužné rutiny.

## <a name="what-should-a-handler-return"></a>Co by měla vrátit obslužná rutina?

Zobrazí v našich [příklad obslužná rutina](policies.md#security-authorization-handler-example) , `Handle()` metoda nemá žádnou návratovou hodnotu, tak jak se jsme indikující úspěch nebo selhání?

* Obslužná rutina označuje úspěch voláním `context.Succeed(IAuthorizationRequirement requirement)`, předávání požadavek, který byl úspěšně ověřen.

* Obslužná rutina pro zpracování chyby obecně platí, jako ostatní obslužné rutiny pro požadavek na stejné může být úspěšné, nemusí.

* Chcete-li zaručit selhání i v případě ostatních obslužných rutin pro požadavek úspěšné, volejte `context.Fail`.

Bez ohledu na to, co bude volat uvnitř vaší obslužné rutiny budou všechny obslužné rutiny pro požadavek volat, pokud zásady vyžaduje požadavek. To umožňuje požadavky tak, aby měl vedlejší účinky, například protokolování, který bude mít místní i v případě `context.Fail()` byla volána v jiná obslužná rutina.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Proč byste měli více obslužných rutin pro požadavek?

V případech, kde chcete vyhodnocení na **nebo** základ implementovat několik obslužné rutiny pro jeden požadavek. Například společnost Microsoft nemá dveří, které otevíraly jenom s kartami klíče. Pokud necháte klíče karty doma recepční vytiskne dočasné štítku a otevře dvířka pro vás. V tomto scénáři by mít jeden požadavek, *EnterBuilding*, ale více obslužných rutin, každé z nich zkoumání jeden požadavek.

```csharp
public class EnterBuildingRequirement : IAuthorizationRequirement
{
}

public class BadgeEntryHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.BadgeId &&
                                       c.Issuer == "http://microsoftsecurity"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}

public class HasTemporaryStickerHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.TemporaryBadgeId &&
                                       c.Issuer == "https://microsoftsecurity"))
        {
            // We'd also check the expiration date on the sticker.
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

Nyní, za předpokladu, že oba obslužné rutiny jsou [zaregistrován](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) při vyhodnotí zásadu `EnterBuildingRequirement` Pokud se podaří buď obslužná rutina bude úspěšné vyhodnocení zásad.

## <a name="using-a-func-to-fulfill-a-policy"></a>Pomocí funkci func ke splnění zásadu

Mohou nastat situace kde splnění zásady je jednoduše express v kódu. Je možné jednoduše zadat `Func<AuthorizationHandlerContext, bool>` při konfiguraci zásad služby s `RequireAssertion` tvůrce zásad.

Například předchozí `BadgeEntryHandler` by mohla být přepsána následujícím způsobem:

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("BadgeEntry",
                          policy => policy.RequireAssertion(context =>
                                  context.User.HasClaim(c =>
                                     (c.Type == ClaimTypes.BadgeId ||
                                      c.Type == ClaimTypes.TemporaryBadgeId)
                                      && c.Issuer == "https://microsoftsecurity"));
                          }));
    }
 }
```

## <a name="accessing-mvc-request-context-in-handlers"></a>Přístup k kontext požadavku MVC v obslužné rutiny

`Handle` Metoda musí implementovat v obslužné rutině autorizace má dva parametry `AuthorizationContext` a `Requirement` jsou zpracování. Rozhraní, jako je například MVC nebo Jabbr jsou zdarma objekt, který chcete přidat `Resource` vlastnost `AuthorizationContext` předávat doplňující informace.

Například MVC předá instance `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` v vlastnost prostředku, který se používá pro přístup k HttpContext, RouteData a všechno else MVC poskytuje.

Použití `Resource` vlastnost je konkrétní rozhraní. Pomocí informací v `Resource` vlastnost omezí zásad autorizace pro konkrétní rozhraní. Měli přetypovat `Resource` pomocí vlastnosti `as` – klíčové slovo a zkontrolujte přetypování má být úspěšné, aby váš kód není havárií s `InvalidCastExceptions` spuštění na ostatní platformy;

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
