---
title: Na základě zásad autorizace v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak vytvořit a používat obslužné rutiny zásad autorizace pro vynucení požadavky autorizace v aplikaci ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/policies
ms.openlocfilehash: 411fee90bdccfb45c33f5d4ccd7864c83c614e70
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="4ad83-103">Na základě zásad autorizace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4ad83-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="4ad83-104">Pod zahrnuje [autorizace na základě rolí](xref:security/authorization/roles) a [založené na deklaracích identity autorizace](xref:security/authorization/claims) použít požadavek, obslužnou rutinu požadavků a předem nakonfigurované zásad.</span><span class="sxs-lookup"><span data-stu-id="4ad83-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="4ad83-105">Tyto stavební bloky podporovat výraz hodnocení autorizace v kódu.</span><span class="sxs-lookup"><span data-stu-id="4ad83-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="4ad83-106">Výsledkem je strukturou autorizací bohatší, opakovaně použitelný, možností intenzivního testování.</span><span class="sxs-lookup"><span data-stu-id="4ad83-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="4ad83-107">Zásad autorizace se skládá z jednoho nebo více požadavků.</span><span class="sxs-lookup"><span data-stu-id="4ad83-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="4ad83-108">Jako součást konfigurace služby autorizace, je zaregistrován v `Startup.ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="4ad83-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="4ad83-109">V předchozím příkladu se vytvoří zásadu "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="4ad83-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="4ad83-110">Má jeden požadavek&mdash;s minimálním stářím, který je poskytnut jako parametr pro požadavek.</span><span class="sxs-lookup"><span data-stu-id="4ad83-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="4ad83-111">Zásady se aplikují pomocí `[Authorize]` atribut s názvem zásady.</span><span class="sxs-lookup"><span data-stu-id="4ad83-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="4ad83-112">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4ad83-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="4ad83-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4ad83-113">Requirements</span></span>

<span data-ttu-id="4ad83-114">Požadavek na autorizaci je kolekce dat parametry, které zásadu lze použít k vyhodnocení aktuální hlavní název uživatele.</span><span class="sxs-lookup"><span data-stu-id="4ad83-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="4ad83-115">V našem "AtLeast21" zásady, je požadavek na jeden parametr&mdash;minimálním stářím.</span><span class="sxs-lookup"><span data-stu-id="4ad83-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="4ad83-116">Implementuje požadavek [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), což je rozhraní prázdný značky.</span><span class="sxs-lookup"><span data-stu-id="4ad83-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="4ad83-117">Požadavek na minimální věk parametrizované může být implementováno takto:</span><span class="sxs-lookup"><span data-stu-id="4ad83-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="4ad83-118">Požadavek není nutné mít dat nebo vlastností.</span><span class="sxs-lookup"><span data-stu-id="4ad83-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="4ad83-119">Obslužné rutiny autorizace</span><span class="sxs-lookup"><span data-stu-id="4ad83-119">Authorization handlers</span></span>

<span data-ttu-id="4ad83-120">Obslužné rutiny autorizace je zodpovědná za vyhodnocení vlastnosti požadavek.</span><span class="sxs-lookup"><span data-stu-id="4ad83-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="4ad83-121">Obslužná rutina autorizace vyhodnotí požadavky proti zadaný [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) k určení, zda je povolen přístup.</span><span class="sxs-lookup"><span data-stu-id="4ad83-121">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="4ad83-122">Může mít požadavek [více obslužných rutin](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="4ad83-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="4ad83-123">Obslužná rutina může dědit [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), kde `TRequirement` je požadavek na zpracování.</span><span class="sxs-lookup"><span data-stu-id="4ad83-123">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="4ad83-124">Alternativně může implementovat obslužná rutina [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) pro zpracování více než jeden typ požadavku.</span><span class="sxs-lookup"><span data-stu-id="4ad83-124">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="4ad83-125">Použijte obslužnou rutinu pro jeden požadavek</span><span class="sxs-lookup"><span data-stu-id="4ad83-125">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="4ad83-126">Následuje příklad relace, ve kterém obslužnou rutinu minimálním stářím využívá jeden požadavek:</span><span class="sxs-lookup"><span data-stu-id="4ad83-126">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="4ad83-127">Předchozí kód určuje, zda má aktuální uživatel hlavní datum narození deklarace identity, který byl vydán vystavitelem známé a důvěryhodné.</span><span class="sxs-lookup"><span data-stu-id="4ad83-127">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="4ad83-128">Autorizace nelze provést v případě deklarace identity nebyl nalezen, v takovém případě je vrácená dokončené úlohy.</span><span class="sxs-lookup"><span data-stu-id="4ad83-128">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="4ad83-129">Pokud deklarace identity, se počítá stáří uživatele.</span><span class="sxs-lookup"><span data-stu-id="4ad83-129">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="4ad83-130">Pokud uživatel splňuje definované požadavek na minimální věk, autorizace se považuje za úspěšné.</span><span class="sxs-lookup"><span data-stu-id="4ad83-130">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="4ad83-131">Když je ověřování úspěšné, `context.Succeed` vyvolání splněna požadavek jako svůj jediný parametr.</span><span class="sxs-lookup"><span data-stu-id="4ad83-131">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="4ad83-132">Použijte obslužnou rutinu pro víc požadavků</span><span class="sxs-lookup"><span data-stu-id="4ad83-132">Use a handler for multiple requirements</span></span>

<span data-ttu-id="4ad83-133">Následuje příklad vztah jeden mnoho, ve kterém obslužnou rutinu oprávnění využívá tři požadavky:</span><span class="sxs-lookup"><span data-stu-id="4ad83-133">The following is an example of a one-to-many relationship in which a permission handler utilizes three requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="4ad83-134">Předchozí kód prochází [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;vlastnost obsahující požadavky není označena jako úspěšné.</span><span class="sxs-lookup"><span data-stu-id="4ad83-134">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="4ad83-135">Pokud uživatel, má oprávnění ke čtení, musí být potvrdí vlastníka nebo sponzor přístupu k požadovanému zdroji.</span><span class="sxs-lookup"><span data-stu-id="4ad83-135">If the user has read permission, he or she must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="4ad83-136">Pokud uživatel upravit nebo odstranit oprávnění, uživatel musí být vlastníkem k přístupu k požadovanému zdroji.</span><span class="sxs-lookup"><span data-stu-id="4ad83-136">If the user has edit or delete permission, he or she must be an owner to access the requested resource.</span></span> <span data-ttu-id="4ad83-137">Když je ověřování úspěšné, `context.Succeed` vyvolání splněna požadavek jako svůj jediný parametr.</span><span class="sxs-lookup"><span data-stu-id="4ad83-137">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="4ad83-138">Obslužná rutina registrace</span><span class="sxs-lookup"><span data-stu-id="4ad83-138">Handler registration</span></span>

<span data-ttu-id="4ad83-139">Obslužné rutiny jsou zaregistrované v kolekci služby během konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4ad83-139">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="4ad83-140">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4ad83-140">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="4ad83-141">Každý obslužná rutina se přidá do kolekce služeb vyvoláním `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="4ad83-141">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="4ad83-142">Co by měla vrátit obslužná rutina?</span><span class="sxs-lookup"><span data-stu-id="4ad83-142">What should a handler return?</span></span>

<span data-ttu-id="4ad83-143">Všimněte si, že `Handle` metoda v [příklad obslužná rutina](#security-authorization-handler-example) nevrací žádnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4ad83-143">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="4ad83-144">Jak je ve stavu úspěch nebo selhání uvedené?</span><span class="sxs-lookup"><span data-stu-id="4ad83-144">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="4ad83-145">Obslužná rutina označuje úspěch voláním `context.Succeed(IAuthorizationRequirement requirement)`, předávání požadavek, který byl úspěšně ověřen.</span><span class="sxs-lookup"><span data-stu-id="4ad83-145">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="4ad83-146">Obslužná rutina nepotřebuje řešit selhání obecně platí, jako ostatních obslužných rutin pro stejný požadavek může být úspěšné.</span><span class="sxs-lookup"><span data-stu-id="4ad83-146">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="4ad83-147">Chcete-li zaručit selhání, i v případě ostatních obslužných rutin požadavek úspěšné, volejte `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="4ad83-147">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="4ad83-148">Pokud nastavíte hodnotu `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) vlastnost (k dispozici v technologii ASP.NET Core 1.1 nebo novější) zkratům spuštění obslužné rutiny při `context.Fail` je volána.</span><span class="sxs-lookup"><span data-stu-id="4ad83-148">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="4ad83-149">`InvokeHandlersAfterFailure` použije se výchozí hodnota `true`, v takovém případě jsou volány všechny obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="4ad83-149">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="4ad83-150">To umožňuje požadavek na vytvoření vedlejší účinky, například protokolování, které vždy provádět i v případě `context.Fail` byla volána v jiná obslužná rutina.</span><span class="sxs-lookup"><span data-stu-id="4ad83-150">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="4ad83-151">Proč byste měli více obslužných rutin pro požadavek?</span><span class="sxs-lookup"><span data-stu-id="4ad83-151">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="4ad83-152">V případech, kde chcete vyhodnocení na **nebo** základ, implementovat několik obslužné rutiny pro jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="4ad83-152">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="4ad83-153">Například společnost Microsoft nemá dveří, které otevíraly jenom s kartami klíče.</span><span class="sxs-lookup"><span data-stu-id="4ad83-153">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="4ad83-154">Pokud necháte klíče karty v domácnostech, recepční vytiskne dočasné štítku a otevře dvířka pro vás.</span><span class="sxs-lookup"><span data-stu-id="4ad83-154">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="4ad83-155">V tomto scénáři by mít jeden požadavek, *BuildingEntry*, ale více obslužných rutin, každé z nich zkoumání jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="4ad83-155">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="4ad83-156">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="4ad83-156">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="4ad83-157">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="4ad83-157">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="4ad83-158">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="4ad83-158">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="4ad83-159">Zkontrolujte, zda jsou oba obslužné rutiny [zaregistrován](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="4ad83-159">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="4ad83-160">Je-li buď obslužná rutina úspěšné při zásady vyhodnotí `BuildingEntryRequirement`, úspěšné vyhodnocení zásad.</span><span class="sxs-lookup"><span data-stu-id="4ad83-160">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="4ad83-161">Pomocí funkci func ke splnění zásadu</span><span class="sxs-lookup"><span data-stu-id="4ad83-161">Using a func to fulfill a policy</span></span>

<span data-ttu-id="4ad83-162">Mohou nastat situace, ve které splnění zásady se snadno express v kódu.</span><span class="sxs-lookup"><span data-stu-id="4ad83-162">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="4ad83-163">Je možné poskytnout `Func<AuthorizationHandlerContext, bool>` při konfiguraci zásad služby s `RequireAssertion` tvůrce zásad.</span><span class="sxs-lookup"><span data-stu-id="4ad83-163">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="4ad83-164">Například předchozí `BadgeEntryHandler` by mohla být přepsána následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4ad83-164">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="4ad83-165">Přístup k kontext požadavku MVC v obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="4ad83-165">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="4ad83-166">`HandleRequirementAsync` Metoda implementovat v obslužnou rutinu autorizace má dva parametry: `AuthorizationHandlerContext` a `TRequirement` jsou zpracování.</span><span class="sxs-lookup"><span data-stu-id="4ad83-166">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="4ad83-167">Rozhraní, jako je například MVC nebo Jabbr jsou zdarma objekt, který chcete přidat `Resource` vlastnost `AuthorizationHandlerContext` předat doplňující informace.</span><span class="sxs-lookup"><span data-stu-id="4ad83-167">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="4ad83-168">Například MVC předá instance [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) v `Resource` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="4ad83-168">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="4ad83-169">Tato vlastnost poskytuje přístup k `HttpContext`, `RouteData`a všechno jinak poskytované MVC a stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="4ad83-169">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="4ad83-170">Použití `Resource` vlastnost je konkrétní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="4ad83-170">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="4ad83-171">Pomocí informací v `Resource` vlastnost omezení zásad autorizace pro konkrétní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="4ad83-171">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="4ad83-172">Měli přetypovat `Resource` pomocí vlastnosti `as` – klíčové slovo a pak potvrďte přetypování má být úspěšné, aby váš kód není havárií s `InvalidCastException` spuštění na ostatní platformy:</span><span class="sxs-lookup"><span data-stu-id="4ad83-172">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
