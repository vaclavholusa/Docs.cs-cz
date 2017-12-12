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
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="7dd4d-104">Autorizace uživatele na základě zásad</span><span class="sxs-lookup"><span data-stu-id="7dd4d-104">Custom policy-based authorization</span></span>

<a name="security-authorization-policies-based"></a>

<span data-ttu-id="7dd4d-105">Pod zahrnuje [role autorizace](roles.md) a [deklarací autorizace](claims.md) Zkontrolujte použití požadavku, obslužné rutiny pro požadavek a předem nakonfigurované zásady.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-105">Underneath the covers, the [role authorization](roles.md) and [claims authorization](claims.md) make use of a requirement, a handler for the requirement, and a pre-configured policy.</span></span> <span data-ttu-id="7dd4d-106">Tyto stavební bloky umožňují express hodnocení autorizační kód, aby vám umožnil bohatší, opakovaně využívat, a snadno testovat autorizace strukturou.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-106">These building blocks allow you to express authorization evaluations in code, allowing for a richer, reusable, and easily testable authorization structure.</span></span>

<span data-ttu-id="7dd4d-107">Zásad autorizace se skládá z jednoho nebo více požadavků a zaregistrován při spuštění aplikace v rámci daná konfigurace služby ověřování v `ConfigureServices` v *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-107">An authorization policy is made up of one or more requirements and registered at application startup as part of the Authorization service configuration, in `ConfigureServices` in the *Startup.cs* file.</span></span>

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

<span data-ttu-id="7dd4d-108">Zde vidíte, že u jednoho požadavku, že o minimálním stářím, který byl předán jako parametr požadavek na vytvoření zásadu "Over21".</span><span class="sxs-lookup"><span data-stu-id="7dd4d-108">Here you can see an "Over21" policy is created with a single requirement, that of a minimum age, which is passed as a parameter to the requirement.</span></span>

<span data-ttu-id="7dd4d-109">Zásady se použijí, pomocí `Authorize` atribut tak, že zadáte název zásady, například;</span><span class="sxs-lookup"><span data-stu-id="7dd4d-109">Policies are applied using the `Authorize` attribute by specifying the policy name, for example;</span></span>

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

## <a name="requirements"></a><span data-ttu-id="7dd4d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7dd4d-110">Requirements</span></span>

<span data-ttu-id="7dd4d-111">Požadavek na autorizaci je kolekce dat parametry, které zásadu lze použít k vyhodnocení aktuální hlavní název uživatele.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-111">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="7dd4d-112">V našich minimálním stářím zásad je požadavek na našich jeden parametr minimálním stářím.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-112">In our Minimum Age policy, the requirement we have is a single parameter, the minimum age.</span></span> <span data-ttu-id="7dd4d-113">Požadavek musí implementovat `IAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-113">A requirement must implement `IAuthorizationRequirement`.</span></span> <span data-ttu-id="7dd4d-114">Toto je prázdná, rozhraní značky.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-114">This is an empty, marker interface.</span></span> <span data-ttu-id="7dd4d-115">Parametrizované minimálním stářím požadavek může být implementováno takto;</span><span class="sxs-lookup"><span data-stu-id="7dd4d-115">A parameterized minimum age requirement might be implemented as follows;</span></span>

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

<span data-ttu-id="7dd4d-116">Požadavek není nutné mít dat nebo vlastností.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-116">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="7dd4d-117">Obslužné rutiny autorizace</span><span class="sxs-lookup"><span data-stu-id="7dd4d-117">Authorization handlers</span></span>

<span data-ttu-id="7dd4d-118">Obslužné rutiny autorizace je zodpovědná za vyhodnocení všechny vlastnosti požadavek.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-118">An authorization handler is responsible for the evaluation of any properties of a requirement.</span></span> <span data-ttu-id="7dd4d-119">Obslužná rutina ověřování musí vyhodnocuje je s zadaný `AuthorizationHandlerContext` rozhodnout, jestli je povolené ověřování.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-119">The  authorization handler must evaluate them against a provided `AuthorizationHandlerContext` to decide if authorization is allowed.</span></span> <span data-ttu-id="7dd4d-120">Může mít požadavek [více obslužných rutin](policies.md#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="7dd4d-120">A requirement can have [multiple handlers](policies.md#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="7dd4d-121">Obslužné rutiny musí dědit `AuthorizationHandler<T>` tam, kde T představuje požadavek zpracovává.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-121">Handlers must inherit `AuthorizationHandler<T>` where T is the requirement it handles.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="7dd4d-122">Obslužná rutina minimálním stářím může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="7dd4d-122">The minimum age handler might look like this:</span></span>

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

<span data-ttu-id="7dd4d-123">Ve výše uvedeném kódu nejprve podíváme pro případ, že má aktuální uživatel hlavní datum narození deklarace identity, který byl vydán vystavitele víme a vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-123">In the code above, we first look to see if the current user principal has a date of birth claim which has been issued by an Issuer we know and trust.</span></span> <span data-ttu-id="7dd4d-124">Pokud chybí deklarace jsme nejde autorizovat, proto vrátíme.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-124">If the claim is missing we can't authorize so we return.</span></span> <span data-ttu-id="7dd4d-125">Pokud budeme mít deklaraci identity, jsme zjistit, kolik je uživatel, a pokud nebudou splňovat předaná v požadavek na minimální věk pak autorizace proběhlo úspěšně.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-125">If we have a claim, we figure out how old the user is, and if they meet the minimum age passed in by the requirement then authorization has been successful.</span></span> <span data-ttu-id="7dd4d-126">Po úspěšné autorizace říkáme `context.Succeed()` předávání v požadavku, že proběhlo úspěšně jako parametr.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-126">Once authorization is successful we call `context.Succeed()` passing in the requirement that has been successful as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="7dd4d-127">Obslužná rutina registrace</span><span class="sxs-lookup"><span data-stu-id="7dd4d-127">Handler registration</span></span>
<span data-ttu-id="7dd4d-128">Obslužné rutiny musí být zaregistrovaný v kolekci služby během konfigurace, například;</span><span class="sxs-lookup"><span data-stu-id="7dd4d-128">Handlers must be registered in the services collection during configuration, for example;</span></span>

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

<span data-ttu-id="7dd4d-129">Každý obslužná rutina se přidá do kolekce služeb pomocí `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` předávání v třídě obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-129">Each handler is added to the services collection by using `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passing in your handler class.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="7dd4d-130">Co by měla vrátit obslužná rutina?</span><span class="sxs-lookup"><span data-stu-id="7dd4d-130">What should a handler return?</span></span>

<span data-ttu-id="7dd4d-131">Zobrazí v našich [příklad obslužná rutina](policies.md#security-authorization-handler-example) , `Handle()` metoda nemá žádnou návratovou hodnotu, tak jak se jsme indikující úspěch nebo selhání?</span><span class="sxs-lookup"><span data-stu-id="7dd4d-131">You can see in our [handler example](policies.md#security-authorization-handler-example) that the `Handle()` method has no return value, so how do we indicate success or failure?</span></span>

* <span data-ttu-id="7dd4d-132">Obslužná rutina označuje úspěch voláním `context.Succeed(IAuthorizationRequirement requirement)`, předávání požadavek, který byl úspěšně ověřen.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-132">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="7dd4d-133">Obslužná rutina pro zpracování chyby obecně platí, jako ostatní obslužné rutiny pro požadavek na stejné může být úspěšné, nemusí.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-133">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="7dd4d-134">Chcete-li zaručit selhání i v případě ostatních obslužných rutin pro požadavek úspěšné, volejte `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-134">To guarantee failure even if other handlers for a requirement succeed, call `context.Fail`.</span></span>

<span data-ttu-id="7dd4d-135">Bez ohledu na to, co bude volat uvnitř vaší obslužné rutiny budou všechny obslužné rutiny pro požadavek volat, pokud zásady vyžaduje požadavek.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-135">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="7dd4d-136">To umožňuje požadavky tak, aby měl vedlejší účinky, například protokolování, který bude mít místní i v případě `context.Fail()` byla volána v jiná obslužná rutina.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-136">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="7dd4d-137">Proč byste měli více obslužných rutin pro požadavek?</span><span class="sxs-lookup"><span data-stu-id="7dd4d-137">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="7dd4d-138">V případech, kde chcete vyhodnocení na **nebo** základ implementovat několik obslužné rutiny pro jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-138">In cases where you want evaluation to be on an **OR** basis you implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="7dd4d-139">Například společnost Microsoft nemá dveří, které otevíraly jenom s kartami klíče.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-139">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="7dd4d-140">Pokud necháte klíče karty doma recepční vytiskne dočasné štítku a otevře dvířka pro vás.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-140">If you leave your key card at home the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="7dd4d-141">V tomto scénáři by mít jeden požadavek, *EnterBuilding*, ale více obslužných rutin, každé z nich zkoumání jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-141">In this scenario you'd have a single requirement, *EnterBuilding*, but multiple handlers, each one examining a single requirement.</span></span>

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

<span data-ttu-id="7dd4d-142">Nyní, za předpokladu, že oba obslužné rutiny jsou [zaregistrován](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) při vyhodnotí zásadu `EnterBuildingRequirement` Pokud se podaří buď obslužná rutina bude úspěšné vyhodnocení zásad.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-142">Now, assuming both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) when a policy evaluates the `EnterBuildingRequirement` if either handler succeeds the policy evaluation will succeed.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="7dd4d-143">Pomocí funkci func ke splnění zásadu</span><span class="sxs-lookup"><span data-stu-id="7dd4d-143">Using a func to fulfill a policy</span></span>

<span data-ttu-id="7dd4d-144">Mohou nastat situace kde splnění zásady je jednoduše express v kódu.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-144">There may be occasions where fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="7dd4d-145">Je možné jednoduše zadat `Func<AuthorizationHandlerContext, bool>` při konfiguraci zásad služby s `RequireAssertion` tvůrce zásad.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-145">It is possible to simply supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="7dd4d-146">Například předchozí `BadgeEntryHandler` by mohla být přepsána následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="7dd4d-146">For example the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

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

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="7dd4d-147">Přístup k kontext požadavku MVC v obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="7dd4d-147">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="7dd4d-148">`Handle` Metoda musí implementovat v obslužné rutině autorizace má dva parametry `AuthorizationContext` a `Requirement` jsou zpracování.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-148">The `Handle` method you must implement in an authorization handler has two parameters, an `AuthorizationContext` and the `Requirement` you are handling.</span></span> <span data-ttu-id="7dd4d-149">Rozhraní, jako je například MVC nebo Jabbr jsou zdarma objekt, který chcete přidat `Resource` vlastnost `AuthorizationContext` předávat doplňující informace.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-149">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationContext` to pass through extra information.</span></span>

<span data-ttu-id="7dd4d-150">Například MVC předá instance `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` v vlastnost prostředku, který se používá pro přístup k HttpContext, RouteData a všechno else MVC poskytuje.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-150">For example, MVC passes an instance of `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` in the resource property which is used to access HttpContext, RouteData and everything else MVC provides.</span></span>

<span data-ttu-id="7dd4d-151">Použití `Resource` vlastnost je konkrétní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-151">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="7dd4d-152">Pomocí informací v `Resource` vlastnost omezí zásad autorizace pro konkrétní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="7dd4d-152">Using information in the `Resource` property will limit your authorization policies to particular frameworks.</span></span> <span data-ttu-id="7dd4d-153">Měli přetypovat `Resource` pomocí vlastnosti `as` – klíčové slovo a zkontrolujte přetypování má být úspěšné, aby váš kód není havárií s `InvalidCastExceptions` spuštění na ostatní platformy;</span><span class="sxs-lookup"><span data-stu-id="7dd4d-153">You should cast the `Resource` property using the `as` keyword, and then check the cast has succeed to ensure your code doesn't crash with `InvalidCastExceptions` when run on other frameworks;</span></span>

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
