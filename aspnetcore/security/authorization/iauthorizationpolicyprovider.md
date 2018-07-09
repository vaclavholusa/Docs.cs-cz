---
title: Poskytovatelé zásad autorizace v ASP.NET Core
author: mjrousos
description: Další informace o použití vlastních IAuthorizationPolicyProvider v aplikaci ASP.NET Core pro dynamické generování zásad autorizace.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 6e46172ec8c5271ffcbad87e4ea5cc98465b78b0
ms.sourcegitcommit: 41d3c4b27309d56f567fd1ad443929aab6587fb1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/07/2018
ms.locfileid: "37910247"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Vlastní zprostředkovatelé zásad autorizace pomocí IAuthorizationPolicyProvider v ASP.NET Core 

Podle [Mike Rousos](https://github.com/mjrousos)

Obvykle při použití [autorizace na základě zásad](xref:security/authorization/policies), zásady jsou registrované pomocí volání `AuthorizationOptions.AddPolicy` jako součást konfigurace služby ověření. V některých případech nemusí být možné (nebo žádoucí) k registraci všech zásad autorizace tímto způsobem. V takových případech můžete použít vlastní `IAuthorizationPolicyProvider` řídit, jak jsou dodávány zásad autorizace.

Uvedené příklady situací, kdy vlastní [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) může být užitečná, patří:

* K poskytování hodnocení zásad pomocí externí služby.
* Pomocí velkého rozsahu zásad (pro jiné místnosti čísla nebo ve věku, například), proto nemá smysl pro přidání jednotlivých zásad jednotlivé autorizace pomocí `AuthorizationOptions.AddPolicy` volání.
* Vytváření zásad za běhu na základě informací v externího zdroje dat (třeba databáze) nebo určení požadavků na povolení dynamicky prostřednictvím jiného mechanismu.

## <a name="customizing-policy-retrieval"></a>Přizpůsobení načtení zásad

Aplikace ASP.NET Core pomocí implementace `IAuthorizationPolicyProvider` rozhraní k načtení zásad autorizace. Ve výchozím nastavení [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) je zaregistrované a použít. `DefaultAuthorizationPolicyProvider` Vrátí zásad `AuthorizationOptions` součástí `IServiceCollection.AddAuthorization` volání.

Toto chování můžete přizpůsobit tak, že zaregistrujete jiný `IAuthorizationPolicyProvider` implementace aplikace [injektáž závislostí](xref:fundamentals/dependency-injection) kontejneru. 

`IAuthorizationPolicyProvider` Rozhraní obsahuje dvě rozhraní API:

* [GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) vrátí zásad autorizace pro daný název.
* [GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) vrátí výchozí zásady autorizace (zásady používané pro `[Authorize]` atributy bez zásad zadaný). 

Implementací těchto dvou rozhraní API můžete přizpůsobit, jak jsou k dispozici zásad autorizace.

## <a name="parameterized-authorize-attribute-example"></a>Parametrizované autorizovat příkladu atribut

Jeden scénář kde `IAuthorizationPolicyProvider` je užitečné, je povolení vlastní `[Authorize]` atributy, jehož požadavky na závisí na parametru. Například v [autorizace na základě zásad](xref:security/authorization/policies) dokumentaci, založené na stáří ("AtLeast21") zásady byl použit jako ukázku. Pokud jiný kontroler akce v aplikaci by měla být k dispozici uživatelům *různých* ve věku, může být užitečné používat mnoho různých zásad na základě věku. Místo registrace všechny různé věkové zásady, které aplikace se musí v `AuthorizationOptions`, můžete vygenerovat zásad dynamicky pomocí vlastní `IAuthorizationPolicyProvider`. Chcete-li pomocí zásad jednodušší, může opatřit poznámkami akce s atributem vlastní autorizace jako `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Atributy vlastní autorizace

Zásady autorizace se identifikují podle svých názvů. Vlastní `MinimumAgeAuthorizeAttribute` popsané dříve je potřeba namapovat argumenty do řetězce, který slouží k načtení odpovídajících zásad autorizace. Uděláte to tak odvozený od `AuthorizeAttribute` a provádění `Age` wrap vlastnost `AuthorizeAttribute.Policy` vlastnost.

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

Tento typ atribut má `Policy` řetězce na základě pevně zakódované předpony (`"MinimumAge"`) a celé číslo předané prostřednictvím konstruktoru.

Můžete provést u akce stejným způsobem jako ostatní `Authorize` atributy s tím rozdílem, že přijímá jako parametr celé číslo.

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Vlastní IAuthorizationPolicyProvider

Vlastní `MinimumAgeAuthorizeAttribute` usnadňuje žádosti o zásady autorizace pro žádné minimální stáří požadovaného. Další problém vyřešit, je zajistit, že zásady ověřování jsou k dispozici pro všechny tyto různé věkové kategorie. Tady `IAuthorizationPolicyProvider` je užitečné.

Při použití `MinimumAgeAuthorizationAttribute`, názvy zásad autorizace se řídí vzorem `"MinimumAge" + Age`, takže vlastní `IAuthorizationPolicyProvider` by měl generovat zásad autorizace podle:

* Analýza věk z název zásady.
* Pomocí `AuthorizationPolicyBuilder` vytvořit nový `AuthorizationPolicy`
* Přidávání požadavků zásad založené na věk pomocí `AuthorizationPolicyBuilder.AddRequirements`. V jiných scénářích může používat `RequireClaim`, `RequireRole`, nebo `RequireUserName` místo.

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

## <a name="multiple-authorization-policy-providers"></a>Více poskytovatelů zásad autorizace

Při použití vlastní `IAuthorizationPolicyProvider` implementace, mějte na paměti, který ASP.NET Core využívá jenom jednu instanci `IAuthorizationPolicyProvider`. Pokud vlastního zprostředkovatele není schopna poskytnout zásady autorizace pro názvy všech zásad, ji by měl vrátit zpět k poskytovatelem zálohování. Názvy zásad může zahrnovat ty, které pocházejí z výchozí zásady pro `[Authorize]` atributy bez názvu.

Představte si třeba, že aplikace zásady vlastní stáří a tradičnější načtení zásad na základě rolí. Takové aplikace může používat poskytovatele zásad autorizace, které:

* Pokusí se analyzovat názvy zásad. 
* Volání do jiné zásady zprostředkovatele (jako je `DefaultAuthorizationPolicyProvider`), pokud neobsahuje název zásady stáří.

## <a name="default-policy"></a>Výchozí zásady

Kromě toho, že zásady s názvem autorizace, vlastní `IAuthorizationPolicyProvider` musí implementovat `GetDefaultPolicyAsync` poskytnout zásady autorizace pro `[Authorize]` atributy bez zadaný název zásad.

V mnoha případech se tento atribut autorizace vyžaduje pouze ověřeného uživatele, abyste měli nezbytné zásad voláním `RequireAuthenticatedUser`:

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Stejně jako u všech aspektů vlastní `IAuthorizationPolicyProvider`, si ho můžete přizpůsobit, podle potřeby. V některých případech:

* Výchozí autorizační zásady nemusí používat.
* Načítají se výchozí zásady se dají delegovat na záložní `IAuthorizationPolicyProvider`.

## <a name="using-a-custom-iauthorizationpolicyprovider"></a>Pomocí vlastních IAuthorizationPolicyProvider

Použití vlastních zásad ze `IAuthorizationPolicyProvider`, musíte:

* Zaregistrujte příslušný `AuthorizationHandler` typy s injektáž závislostí (popsané v [autorizace na základě zásad](xref:security/authorization/policies#authorization-handlers)), stejně jako u všech scénářích ověřování na základě zásad.
* Zaregistrovat vlastní `IAuthorizationPolicyProvider` typ v kolekci služby injektáž závislostí aplikace (v `Startup.ConfigureServices`) Chcete-li nahradit výchozí poskytovatel zásad.

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Kompletní vlastní `IAuthorizationPolicyProvider` ukázka je k dispozici v [úložiště GitHub aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider).
