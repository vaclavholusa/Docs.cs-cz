---
title: Zprostředkovatelé zásad autorizace v ASP.NET Core
author: mjrousos
description: Naučte se používat vlastní IAuthorizationPolicyProvider v aplikaci ASP.NET Core dynamicky generovat zásad autorizace.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 218d7a495655598046671093c0cfe7b9622aca5e
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077599"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="62e9c-103">Vlastní zprostředkovatelé zásad autorizace pomocí IAuthorizationPolicyProvider v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62e9c-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="62e9c-104">Podle [Rousos Jan](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="62e9c-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="62e9c-105">Obvykle při použití [na základě zásad autorizace](xref:security/authorization/policies), zásady, které jsou registrovány voláním `AuthorizationOptions.AddPolicy` jako součást konfigurace služby autorizace.</span><span class="sxs-lookup"><span data-stu-id="62e9c-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="62e9c-106">V některých případech nemusí být možné (nebo žádoucí) k registraci všech zásad autorizace tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="62e9c-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="62e9c-107">V takových případech můžete použít vlastní `IAuthorizationPolicyProvider` řídit, jak jsou zadány zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="62e9c-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="62e9c-108">Příklady situací, kde vlastní [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) můžou být užitečné, patří:</span><span class="sxs-lookup"><span data-stu-id="62e9c-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="62e9c-109">Pomocí služby externí zajistit vyhodnocení zásad.</span><span class="sxs-lookup"><span data-stu-id="62e9c-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="62e9c-110">Pomocí velkého rozsahu zásad (pro jiné místnosti čísla nebo stáří, např.), takže nemá smysl přidat každé jednotlivé autorizace zásadou `AuthorizationOptions.AddPolicy` volání.</span><span class="sxs-lookup"><span data-stu-id="62e9c-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="62e9c-111">Vytváření zásad za běhu na základě informací v externího zdroje dat (např. databáze) nebo určení požadavků na autorizaci dynamicky prostřednictvím jiný mechanismus.</span><span class="sxs-lookup"><span data-stu-id="62e9c-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

## <a name="customizing-policy-retrieval"></a><span data-ttu-id="62e9c-112">Přizpůsobení načtení zásad</span><span class="sxs-lookup"><span data-stu-id="62e9c-112">Customizing policy retrieval</span></span>

<span data-ttu-id="62e9c-113">Aplikace ASP.NET Core používat provádění `IAuthorizationPolicyProvider` rozhraní k načtení zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="62e9c-113">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="62e9c-114">Ve výchozím nastavení [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) je zaregistrován a použít.</span><span class="sxs-lookup"><span data-stu-id="62e9c-114">By default, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="62e9c-115">`DefaultAuthorizationPolicyProvider` Vrátí zásady z `AuthorizationOptions` součástí `IServiceCollection.AddAuthorization` volání.</span><span class="sxs-lookup"><span data-stu-id="62e9c-115">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="62e9c-116">Toto chování můžete přizpůsobit tak, že zaregistrujete jiné `IAuthorizationPolicyProvider` implementace v dané aplikaci [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru.</span><span class="sxs-lookup"><span data-stu-id="62e9c-116">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="62e9c-117">`IAuthorizationPolicyProvider` Rozhraní obsahuje dvě rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="62e9c-117">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="62e9c-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) vrátí zásad autorizace pro daného názvu.</span><span class="sxs-lookup"><span data-stu-id="62e9c-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="62e9c-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) vrátí výchozí zásady autorizace (zásady používané pro `[Authorize]` atributy bez zadanou zásadu).</span><span class="sxs-lookup"><span data-stu-id="62e9c-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="62e9c-120">Implementací tato dvě rozhraní API můžete přizpůsobit, jak jsou uvedeny zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="62e9c-120">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="62e9c-121">Parametry autorizovat příklad atribut</span><span class="sxs-lookup"><span data-stu-id="62e9c-121">Parameterized authorize attribute example</span></span>

<span data-ttu-id="62e9c-122">Jeden scénář kde `IAuthorizationPolicyProvider` je užitečné, je povolení vlastní `[Authorize]` atributy, jejichž požadavky závisí na parametr.</span><span class="sxs-lookup"><span data-stu-id="62e9c-122">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="62e9c-123">Například v [na základě zásad autorizace](xref:security/authorization/policies) dokumentace, základě stáří ("AtLeast21") zásad byla použita jako ukázka.</span><span class="sxs-lookup"><span data-stu-id="62e9c-123">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="62e9c-124">Pokud jiný řadič akce v aplikaci má být k dispozici pro uživatele *různých* stáří, může být užitečné používat mnoho různých zásad na základě věku.</span><span class="sxs-lookup"><span data-stu-id="62e9c-124">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="62e9c-125">Místo registrace všechny jiné na základě stáří zásadách, které aplikace bude nutné v `AuthorizationOptions`, můžete vygenerovat zásad dynamicky pomocí vlastní `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="62e9c-125">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="62e9c-126">Chcete-li pomocí zásady jednodušší, může opatřit poznámkami akce s atributem autorizace jako `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="62e9c-126">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="62e9c-127">Autorizace vlastní atributy</span><span class="sxs-lookup"><span data-stu-id="62e9c-127">Custom Authorization Attributes</span></span>

<span data-ttu-id="62e9c-128">Zásady autorizace jsou identifikovány jejich názvy.</span><span class="sxs-lookup"><span data-stu-id="62e9c-128">Authorization policies are identified by their names.</span></span> <span data-ttu-id="62e9c-129">Vlastní `MinimumAgeAuthorizeAttribute` popsané dříve musí mapování argumenty na řetězec, který slouží k načtení odpovídající zásada autorizace.</span><span class="sxs-lookup"><span data-stu-id="62e9c-129">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="62e9c-130">Můžete k tomu odvozené z `AuthorizeAttribute` a provádění `Age` vlastnost wrap `AuthorizeAttribute.Policy` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="62e9c-130">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="62e9c-131">Tento typ atributu je `Policy` řetězec založený na předponě pevně (`"MinimumAge"`) a celé předaná prostřednictvím konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="62e9c-131">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="62e9c-132">Můžete provést u akce v stejným způsobem jako ostatní `Authorize` atributy s tím rozdílem, že bude trvat celé jako parametr.</span><span class="sxs-lookup"><span data-stu-id="62e9c-132">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="62e9c-133">Vlastní IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="62e9c-133">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="62e9c-134">Vlastní `MinimumAgeAuthorizeAttribute` usnadňuje požadavek zásady autorizace pro všechny minimálním stářím potřeby.</span><span class="sxs-lookup"><span data-stu-id="62e9c-134">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="62e9c-135">Další problém vyřešit, je zajistit se, že zásady autorizace, jsou k dispozici pro všechny tyto různé stáří.</span><span class="sxs-lookup"><span data-stu-id="62e9c-135">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="62e9c-136">To je, kdy `IAuthorizationPolicyProvider` je užitečné.</span><span class="sxs-lookup"><span data-stu-id="62e9c-136">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="62e9c-137">Při použití `MinimumAgeAuthorizationAttribute`, názvy zásad autorizace bude vyhovovat vzoru `"MinimumAge" + Age`, takže vlastní `IAuthorizationPolicyProvider` měl generovat zásad autorizace podle:</span><span class="sxs-lookup"><span data-stu-id="62e9c-137">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="62e9c-138">Analýza stáří z název zásady.</span><span class="sxs-lookup"><span data-stu-id="62e9c-138">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="62e9c-139">Pomocí `AuthorizationPolicyBuilder` vytvořit nový `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="62e9c-139">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="62e9c-140">Přidávání požadavků zásad podle věk pomocí `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="62e9c-140">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="62e9c-141">V dalších scénářích, můžete použít `RequireClaim`, `RequireRole`, nebo `RequireUserName` místo.</span><span class="sxs-lookup"><span data-stu-id="62e9c-141">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="62e9c-142">Několik poskytovatelů zásad autorizace</span><span class="sxs-lookup"><span data-stu-id="62e9c-142">Multiple authorization policy providers</span></span>

<span data-ttu-id="62e9c-143">Při použití vlastní `IAuthorizationPolicyProvider` implementace, mějte na paměti, který ASP.NET Core používá jenom jednu instanci `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="62e9c-143">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="62e9c-144">Pokud vlastního zprostředkovatele není schopen poskytnout zásady autorizace pro názvy všech zásad, se musí vrátit k poskytovatele zálohování.</span><span class="sxs-lookup"><span data-stu-id="62e9c-144">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="62e9c-145">Názvy zásad mohou zahrnovat ty, které pocházejí z výchozí zásady pro `[Authorize]` atributy bez názvu.</span><span class="sxs-lookup"><span data-stu-id="62e9c-145">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="62e9c-146">Zvažte například, že aplikace zásady vlastní stáří i tradičnější načtení zásad na základě rolí.</span><span class="sxs-lookup"><span data-stu-id="62e9c-146">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="62e9c-147">Tyto aplikace použít poskytovatele zásad autorizace který:</span><span class="sxs-lookup"><span data-stu-id="62e9c-147">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="62e9c-148">Pokusí se analyzovat názvy zásad.</span><span class="sxs-lookup"><span data-stu-id="62e9c-148">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="62e9c-149">Volání do zprostředkovatele jinou zásadu (jako je `DefaultAuthorizationPolicyProvider`), pokud název zásady neobsahuje stáří.</span><span class="sxs-lookup"><span data-stu-id="62e9c-149">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="62e9c-150">Výchozí zásady</span><span class="sxs-lookup"><span data-stu-id="62e9c-150">Default policy</span></span>

<span data-ttu-id="62e9c-151">Kromě zásady s názvem autorizace, vlastní `IAuthorizationPolicyProvider` musí implementovat `GetDefaultPolicyAsync` zajistit zásad autorizace pro `[Authorize]` atributy bez zadaný název zásad.</span><span class="sxs-lookup"><span data-stu-id="62e9c-151">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="62e9c-152">V mnoha případech tento atribut autorizace pouze vyžaduje ověřeného uživatele, umožní vám provádět potřebné zásadou volání `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="62e9c-152">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="62e9c-153">Stejně jako u všech aspektů vlastní `IAuthorizationPolicyProvider`, tuto možnost můžete upravit, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="62e9c-153">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="62e9c-154">V některých případech:</span><span class="sxs-lookup"><span data-stu-id="62e9c-154">In some cases:</span></span>

* <span data-ttu-id="62e9c-155">Výchozí zásady autorizace se možná nepoužívají.</span><span class="sxs-lookup"><span data-stu-id="62e9c-155">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="62e9c-156">Načítání výchozí zásady se dají delegovat na zálohu `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="62e9c-156">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="using-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="62e9c-157">Pomocí vlastních IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="62e9c-157">Using a Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="62e9c-158">Použít vlastní zásady ze `IAuthorizationPolicyProvider`, musíte:</span><span class="sxs-lookup"><span data-stu-id="62e9c-158">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="62e9c-159">Zaregistrujte příslušný `AuthorizationHandler` typy pomocí vkládání závislostí (popsané v [na základě zásad autorizace](xref:security/authorization/policies#authorization-handlers)), stejně jako u všech scénářích na základě zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="62e9c-159">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="62e9c-160">Zaregistrovat vlastní `IAuthorizationPolicyProvider` typu v kolekci služby vkládání závislostí aplikace (v `Startup.ConfigureServices`) k nahrazení výchozího zásad zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="62e9c-160">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="62e9c-161">Dokončení vlastní `IAuthorizationPolicyProvider` ukázka je dostupná v [úložiště GitHub aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="62e9c-161">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span></span>
