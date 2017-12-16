---
title: "Na základě zásad autorizace v ASP.NET Core"
author: rick-anderson
description: "Zjistěte, jak vytvořit a použít vlastní autorizační zásady obslužné rutiny pro vynucení požadavky autorizace v aplikaci ASP.NET Core."
keywords: "ASP.NET Core, ověřování, vlastní zásady, zásady autorizace"
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 280dd72b75e39546061d8455931f597f50c829fe
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="44fb0-104">Autorizace uživatele na základě zásad</span><span class="sxs-lookup"><span data-stu-id="44fb0-104">Custom policy-based authorization</span></span>

<span data-ttu-id="44fb0-105">Pod zahrnuje [autorizace na základě rolí](xref:security/authorization/roles) a [založené na deklaracích identity autorizace](xref:security/authorization/claims) použít požadavek, obslužnou rutinu požadavků a předem nakonfigurované zásad.</span><span class="sxs-lookup"><span data-stu-id="44fb0-105">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="44fb0-106">Tyto stavební bloky podporovat výraz hodnocení autorizace v kódu.</span><span class="sxs-lookup"><span data-stu-id="44fb0-106">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="44fb0-107">Výsledkem je strukturou autorizací bohatší, opakovaně použitelný, možností intenzivního testování.</span><span class="sxs-lookup"><span data-stu-id="44fb0-107">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="44fb0-108">Zásad autorizace se skládá z jednoho nebo více požadavků.</span><span class="sxs-lookup"><span data-stu-id="44fb0-108">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="44fb0-109">Jako součást konfigurace služby autorizace, je zaregistrován v `ConfigureServices` metodu `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="44fb0-109">It's registered as part of the authorization service configuration, in the `ConfigureServices` method of the `Startup` class:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="44fb0-110">V předchozím příkladu se vytvoří zásadu "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="44fb0-110">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="44fb0-111">Má jednoho požadavku,., o minimálním stářím, které jsou dodané jako parametr pro požadavek.</span><span class="sxs-lookup"><span data-stu-id="44fb0-111">It has a single requirement, that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="44fb0-112">Zásady se aplikují pomocí `[Authorize]` atribut s názvem zásady.</span><span class="sxs-lookup"><span data-stu-id="44fb0-112">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="44fb0-113">Příklad:</span><span class="sxs-lookup"><span data-stu-id="44fb0-113">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="44fb0-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="44fb0-114">Requirements</span></span>

<span data-ttu-id="44fb0-115">Požadavek na autorizaci je kolekce dat parametry, které zásadu lze použít k vyhodnocení aktuální hlavní název uživatele.</span><span class="sxs-lookup"><span data-stu-id="44fb0-115">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="44fb0-116">V našem "AtLeast21" zásady, je požadavek na jeden parametr&mdash;minimálním stářím.</span><span class="sxs-lookup"><span data-stu-id="44fb0-116">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="44fb0-117">Implementuje požadavek `IAuthorizationRequirement`, což je rozhraní prázdný značky.</span><span class="sxs-lookup"><span data-stu-id="44fb0-117">A requirement implements `IAuthorizationRequirement`, which is an empty marker interface.</span></span> <span data-ttu-id="44fb0-118">Požadavek na minimální věk parametrizované může být implementováno takto:</span><span class="sxs-lookup"><span data-stu-id="44fb0-118">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="44fb0-119">Požadavek není nutné mít dat nebo vlastností.</span><span class="sxs-lookup"><span data-stu-id="44fb0-119">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="44fb0-120">Obslužné rutiny autorizace</span><span class="sxs-lookup"><span data-stu-id="44fb0-120">Authorization handlers</span></span>

<span data-ttu-id="44fb0-121">Obslužné rutiny autorizace je zodpovědná za vyhodnocení vlastnosti požadavek.</span><span class="sxs-lookup"><span data-stu-id="44fb0-121">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="44fb0-122">Obslužná rutina autorizace vyhodnotí požadavky proti zadaný `AuthorizationHandlerContext` k určení, zda je povolen přístup.</span><span class="sxs-lookup"><span data-stu-id="44fb0-122">The authorization handler evaluates the requirements against a provided `AuthorizationHandlerContext` to determine if access is allowed.</span></span> <span data-ttu-id="44fb0-123">Může mít požadavek [více obslužných rutin](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="44fb0-123">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="44fb0-124">Obslužné rutiny dědění `AuthorizationHandler<T>`, kde `T` je požadavek na zpracování.</span><span class="sxs-lookup"><span data-stu-id="44fb0-124">Handlers inherit `AuthorizationHandler<T>`, where `T` is the requirement to be handled.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="44fb0-125">Obslužná rutina minimálním stářím může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="44fb0-125">The minimum age handler might look like this:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="44fb0-126">Předchozí kód určuje, zda má aktuální uživatel hlavní datum narození deklarace identity, který byl vydán vystavitelem známé a důvěryhodné.</span><span class="sxs-lookup"><span data-stu-id="44fb0-126">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="44fb0-127">Autorizace nelze provést v případě deklarace identity nebyl nalezen, v takovém případě je vrácená dokončené úlohy.</span><span class="sxs-lookup"><span data-stu-id="44fb0-127">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="44fb0-128">Pokud deklarace identity, se počítá stáří uživatele.</span><span class="sxs-lookup"><span data-stu-id="44fb0-128">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="44fb0-129">Pokud uživatel splňuje definované požadavek na minimální věk, autorizace se považuje za úspěšné.</span><span class="sxs-lookup"><span data-stu-id="44fb0-129">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="44fb0-130">Když je ověřování úspěšné, `context.Succeed` vyvolání splněna požadavek jako parametr.</span><span class="sxs-lookup"><span data-stu-id="44fb0-130">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="44fb0-131">Obslužná rutina registrace</span><span class="sxs-lookup"><span data-stu-id="44fb0-131">Handler registration</span></span>

<span data-ttu-id="44fb0-132">Obslužné rutiny jsou zaregistrované v kolekci služby během konfigurace.</span><span class="sxs-lookup"><span data-stu-id="44fb0-132">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="44fb0-133">Příklad:</span><span class="sxs-lookup"><span data-stu-id="44fb0-133">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="44fb0-134">Každý obslužná rutina se přidá do kolekce služeb vyvoláním `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="44fb0-134">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="44fb0-135">Co by měla vrátit obslužná rutina?</span><span class="sxs-lookup"><span data-stu-id="44fb0-135">What should a handler return?</span></span>

<span data-ttu-id="44fb0-136">Všimněte si, že `Handle` metoda v [příklad obslužná rutina](#security-authorization-handler-example) nevrací žádnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="44fb0-136">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="44fb0-137">Jak je ve stavu úspěch nebo selhání uvedené?</span><span class="sxs-lookup"><span data-stu-id="44fb0-137">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="44fb0-138">Obslužná rutina označuje úspěch voláním `context.Succeed(IAuthorizationRequirement requirement)`, předávání požadavek, který byl úspěšně ověřen.</span><span class="sxs-lookup"><span data-stu-id="44fb0-138">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="44fb0-139">Obslužná rutina pro zpracování chyby obecně platí, jako ostatní obslužné rutiny pro požadavek na stejné může být úspěšné, nemusí.</span><span class="sxs-lookup"><span data-stu-id="44fb0-139">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="44fb0-140">Chcete-li zaručit selhání, i v případě ostatních obslužných rutin požadavek úspěšné, volejte `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="44fb0-140">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="44fb0-141">Bez ohledu na to, co bude volat uvnitř vaší obslužné rutiny budou všechny obslužné rutiny pro požadavek volat, pokud zásady vyžaduje požadavek.</span><span class="sxs-lookup"><span data-stu-id="44fb0-141">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="44fb0-142">To umožňuje požadavky tak, aby měl vedlejší účinky, například protokolování, který bude mít místní i v případě `context.Fail()` byla volána v jiná obslužná rutina.</span><span class="sxs-lookup"><span data-stu-id="44fb0-142">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="44fb0-143">Proč byste měli více obslužných rutin pro požadavek?</span><span class="sxs-lookup"><span data-stu-id="44fb0-143">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="44fb0-144">V případech, kde chcete vyhodnocení na **nebo** základ, implementovat několik obslužné rutiny pro jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="44fb0-144">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="44fb0-145">Například společnost Microsoft nemá dveří, které otevíraly jenom s kartami klíče.</span><span class="sxs-lookup"><span data-stu-id="44fb0-145">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="44fb0-146">Pokud necháte klíče karty v domácnostech, recepční vytiskne dočasné štítku a otevře dvířka pro vás.</span><span class="sxs-lookup"><span data-stu-id="44fb0-146">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="44fb0-147">V tomto scénáři by mít jeden požadavek, *BuildingEntry*, ale více obslužných rutin, každé z nich zkoumání jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="44fb0-147">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="44fb0-148">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="44fb0-148">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="44fb0-149">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="44fb0-149">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="44fb0-150">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="44fb0-150">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="44fb0-151">Zkontrolujte, zda jsou oba obslužné rutiny [zaregistrován](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="44fb0-151">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="44fb0-152">Je-li buď obslužná rutina úspěšné při zásady vyhodnotí `BuildingEntryRequirement`, úspěšné vyhodnocení zásad.</span><span class="sxs-lookup"><span data-stu-id="44fb0-152">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="44fb0-153">Pomocí funkci func ke splnění zásadu</span><span class="sxs-lookup"><span data-stu-id="44fb0-153">Using a func to fulfill a policy</span></span>

<span data-ttu-id="44fb0-154">Mohou nastat situace, ve které splnění zásady se snadno express v kódu.</span><span class="sxs-lookup"><span data-stu-id="44fb0-154">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="44fb0-155">Je možné poskytnout `Func<AuthorizationHandlerContext, bool>` při konfiguraci zásad služby s `RequireAssertion` tvůrce zásad.</span><span class="sxs-lookup"><span data-stu-id="44fb0-155">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="44fb0-156">Například předchozí `BadgeEntryHandler` by mohla být přepsána následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="44fb0-156">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="44fb0-157">Přístup k kontext požadavku MVC v obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="44fb0-157">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="44fb0-158">`HandleRequirementAsync` Metoda implementovat v obslužnou rutinu autorizace má dva parametry: `AuthorizationHandlerContext` a `TRequirement` jsou zpracování.</span><span class="sxs-lookup"><span data-stu-id="44fb0-158">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="44fb0-159">Rozhraní, jako je například MVC nebo Jabbr jsou zdarma objekt, který chcete přidat `Resource` vlastnost `AuthorizationHandlerContext` předat doplňující informace.</span><span class="sxs-lookup"><span data-stu-id="44fb0-159">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="44fb0-160">Například MVC předá instance [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) v `Resource` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="44fb0-160">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="44fb0-161">Tato vlastnost poskytuje přístup k `HttpContext`, `RouteData`a všechno jinak poskytované MVC a stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="44fb0-161">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="44fb0-162">Použití `Resource` vlastnost je konkrétní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="44fb0-162">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="44fb0-163">Pomocí informací v `Resource` vlastnost omezení zásad autorizace pro konkrétní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="44fb0-163">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="44fb0-164">Měli přetypovat `Resource` pomocí vlastnosti `as` – klíčové slovo a pak potvrďte přetypování má být úspěšné, aby váš kód není havárií s `InvalidCastException` spuštění na ostatní platformy:</span><span class="sxs-lookup"><span data-stu-id="44fb0-164">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
