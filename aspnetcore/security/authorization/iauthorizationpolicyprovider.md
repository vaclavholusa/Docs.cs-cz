---
title: Zprostředkovatelé zásad autorizace v ASP.NET Core
author: mjrousos
description: Naučte se používat vlastní IAuthorizationPolicyProvider v aplikaci ASP.NET Core dynamicky generovat zásad autorizace.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: a5bad88b37d38b819b960b1eb27808d891268c01
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/17/2018
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Vlastní zprostředkovatelé zásad autorizace pomocí IAuthorizationPolicyProvider v ASP.NET Core 

Podle [Rousos Jan](https://github.com/mjrousos)

Obvykle při použití [na základě zásad autorizace](xref:security/authorization/policies), zásady, které jsou registrovány voláním `AuthorizationOptions.AddPolicy` jako součást konfigurace služby autorizace. V některých případech nemusí být možné (nebo žádoucí) k registraci všech zásad autorizace tímto způsobem. V takových případech můžete použít vlastní `IAuthorizationPolicyProvider` řídit, jak jsou zadány zásad autorizace.

Příklady situací, kde vlastní [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) můžou být užitečné, patří:

* Pomocí služby externí zajistit vyhodnocení zásad.
* Pomocí velkého rozsahu zásad (pro jiné místnosti čísla nebo stáří, např.), takže nemá smysl přidat každé jednotlivé autorizace zásadou `AuthorizationOptions.AddPolicy` volání.
* Vytváření zásad za běhu na základě informací v externího zdroje dat (např. databáze) nebo určení požadavků na autorizaci dynamicky prostřednictvím jiný mechanismus.

## <a name="customizing-policy-retrieval"></a>Přizpůsobení načtení zásad

Aplikace ASP.NET Core používat provádění `IAuthorizationPolicyProvider` rozhraní k načtení zásad autorizace. Ve výchozím nastavení [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) je zaregistrován a použít. `DefaultAuthorizationPolicyProvider` Vrátí zásady z `AuthorizationOptions` součástí `IServiceCollection.AddAuthorization` volání.

Toto chování můžete přizpůsobit tak, že zaregistrujete jiné `IAuthorizationPolicyProvider` implementace v dané aplikaci [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru. 

`IAuthorizationPolicyProvider` Rozhraní obsahuje dvě rozhraní API:

* [GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) vrátí zásad autorizace pro daného názvu.
* [GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) vrátí výchozí zásady autorizace (zásady používané pro `[Authorize]` atributy bez zadanou zásadu). 

Implementací tato dvě rozhraní API můžete přizpůsobit, jak jsou uvedeny zásad autorizace.

## <a name="parameterized-authorize-attribute-example"></a>Parametry autorizovat příklad atribut

Jeden scénář kde `IAuthorizationPolicyProvider` je užitečné, je povolení vlastní `[Authorize]` atributy, jejichž požadavky závisí na parametr. Například v [na základě zásad autorizace](xref:security/authorization/policies) dokumentace, základě stáří ("AtLeast21") zásad byla použita jako ukázka. Pokud jiný řadič akce v aplikaci má být k dispozici pro uživatele *různých* stáří, může být užitečné používat mnoho různých zásad na základě věku. Místo registrace všechny jiné na základě stáří zásadách, které aplikace bude nutné v `AuthorizationOptions`, můžete vygenerovat zásad dynamicky pomocí vlastní `IAuthorizationPolicyProvider`. Chcete-li pomocí zásady jednodušší, může opatřit poznámkami akce s atributem autorizace jako `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Autorizace vlastní atributy

Zásady autorizace jsou identifikovány jejich názvy. Vlastní `MinimumAgeAuthorizeAttribute` popsané dříve musí mapování argumenty na řetězec, který slouží k načtení odpovídající zásada autorizace. Můžete k tomu odvozené z `AuthorizeAttribute` a provádění `Age` vlastnost wrap `AuthorizeAttribute.Policy` vlastnost.

```CSharp
internal class MinimumAgeAuthorizeAttribute : AuthorizeAttribute
{
    const string POLICY_PREFIX = "MinimumAge";

    public MinimumAgeAuthorizeAttribute(int age) => Age = age;

    // Get or set the Age property by manipulating the underlying Policy property
    public int Age
    {
        get
        {
            if (int.TryParse(Policy.Substring(POLICY_PREFIX.Length), out var age))
            {
                return age;
            }
            return default(int);
        }
        set
        {
            Policy = $"{POLICY_PREFIX}{value.ToString()}";
        }
    }
}
```

Tento typ atributu je `Policy` řetězec založený na předponě pevně (`"MinimumAge"`) a celé předaná prostřednictvím konstruktoru.

Můžete provést u akce v stejným způsobem jako ostatní `Authorize` atributy s tím rozdílem, že bude trvat celé jako parametr.

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Vlastní IAuthorizationPolicyProvider

Vlastní `MinimumAgeAuthorizeAttribute` usnadňuje požadavek zásady autorizace pro všechny minimálním stářím potřeby. Další problém vyřešit, je zajistit se, že zásady autorizace, jsou k dispozici pro všechny tyto různé stáří. To je, kdy `IAuthorizationPolicyProvider` je užitečné.

Při použití `MinimumAgeAuthorizationAttribute`, názvy zásad autorizace bude vyhovovat vzoru `"MinimumAge" + Age`, takže vlastní `IAuthorizationPolicyProvider` měl generovat zásad autorizace podle:

* Analýza stáří z název zásady.
* Pomocí `AuthorizationPolicyBuiler` vytvořit nový `AuthorizationPolicy`
* Přidávání požadavků zásad podle věk pomocí `AuthorizationPolicyBuilder.AddRequirements`. V dalších scénářích, můžete použít `RequireClaim`, `RequireRole`, nebo `RequireUserName` místo.

```CSharp
internal class MinimumAgePolicyProvider : IAuthorizationPolicyProvider
{
    const string POLICY_PREFIX = "MinimumAge";

    // Policies are looked up by string name, so expect 'parameters' (like age)
    // to be embedded in the policy names. This is abstracted away from developers
    // by the more strongly-typed attributes derived from AuthorizeAttribute
    // (like [MinimumAgeAuthorize()] in this sample)
    public Task<AuthorizationPolicy> GetPolicyAsync(string policyName)
    {
        if (policyName.StartsWith(POLICY_PREFIX, StringComparison.OrdinalIgnoreCase) &&
            int.TryParse(policyName.Substring(POLICY_PREFIX.Length), out var age))
        {
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a>Několik poskytovatelů zásad autorizace

Při použití vlastní `IAuthorizationPolicyProvider` implementace, mějte na paměti, který ASP.NET Core používá jenom jednu instanci `IAuthorizationPolicyProvider`. Pokud vlastního zprostředkovatele není schopen poskytnout zásady autorizace pro názvy všech zásad, se musí vrátit k poskytovatele zálohování. Názvy zásad mohou zahrnovat ty, které pocházejí z výchozí zásady pro `[Authorize]` atributy bez názvu.

Zvažte například, že aplikace zásady vlastní stáří i tradičnější načtení zásad na základě rolí. Tyto aplikace použít poskytovatele zásad autorizace který:

* Pokusí se analyzovat názvy zásad. 
* Volání do zprostředkovatele jinou zásadu (jako je `DefaultAuthorizationPolicyProvider`), pokud název zásady neobsahuje stáří.

## <a name="default-policy"></a>Výchozí zásady

Kromě zásady s názvem autorizace, vlastní `IAuthorizationPolicyProvider` musí implementovat `GetDefaultPolicyAsync` zajistit zásad autorizace pro `[Authorize]` atributy bez zadaný název zásad.

V mnoha případech tento atribut autorizace pouze vyžaduje ověřeného uživatele, umožní vám provádět potřebné zásadou volání `RequireAuthenticatedUser`:

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Stejně jako u všech aspektů vlastní `IAuthorizationPolicyProvider`, tuto možnost můžete upravit, podle potřeby. V některých případech:

* Výchozí zásady autorizace se možná nepoužívají.
* Načítání výchozí zásady se dají delegovat na zálohu `IAuthorizationPolicyProvider`.

## <a name="using-a-custom-iauthorizationpolicyprovider"></a>Pomocí vlastních IAuthorizationPolicyProvider

Použít vlastní zásady ze `IAuthorizationPolicyProvider`, musíte:

* Zaregistrujte příslušný `AuthorizationHandler` typy pomocí vkládání závislostí (popsané v [na základě zásad autorizace](xref:security/authorization/policies#authorization-handlers)), stejně jako u všech scénářích na základě zásad autorizace.
* Zaregistrovat vlastní `IAuthorizationPolicyProvider` typu v kolekci služby vkládání závislostí aplikace (v `Startup.ConfigureServices`) k nahrazení výchozího zásad zprostředkovatele.

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Dokončení vlastní `IAuthorizationPolicyProvider` ukázka je dostupná v [úložiště GitHub aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).
