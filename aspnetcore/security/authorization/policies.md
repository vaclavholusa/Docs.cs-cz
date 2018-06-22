---
title: Na základě zásad autorizace v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak vytvořit a používat obslužné rutiny zásad autorizace pro vynucení požadavky autorizace v aplikaci ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: 81ca6ad9ddba3de094762f5608bb6a5719bca7a1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277979"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>Na základě zásad autorizace v ASP.NET Core

Pod zahrnuje [autorizace na základě rolí](xref:security/authorization/roles) a [založené na deklaracích identity autorizace](xref:security/authorization/claims) použít požadavek, obslužnou rutinu požadavků a předem nakonfigurované zásad. Tyto stavební bloky podporovat výraz hodnocení autorizace v kódu. Výsledkem je strukturou autorizací bohatší, opakovaně použitelný, možností intenzivního testování.

Zásad autorizace se skládá z jednoho nebo více požadavků. Jako součást konfigurace služby autorizace, je zaregistrován v `Startup.ConfigureServices` metoda:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

V předchozím příkladu se vytvoří zásadu "AtLeast21". Má jeden požadavek&mdash;s minimálním stářím, který je poskytnut jako parametr pro požadavek.

Zásady se aplikují pomocí `[Authorize]` atribut s názvem zásady. Příklad:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>Požadavky

Požadavek na autorizaci je kolekce dat parametry, které zásadu lze použít k vyhodnocení aktuální hlavní název uživatele. V našem "AtLeast21" zásady, je požadavek na jeden parametr&mdash;minimálním stářím. Implementuje požadavek [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), což je rozhraní prázdný značky. Požadavek na minimální věk parametrizované může být implementováno takto:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> Požadavek není nutné mít dat nebo vlastností.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Obslužné rutiny autorizace

Obslužné rutiny autorizace je zodpovědná za vyhodnocení vlastnosti požadavek. Obslužná rutina autorizace vyhodnotí požadavky proti zadaný [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) k určení, zda je povolen přístup.

Může mít požadavek [více obslužných rutin](#security-authorization-policies-based-multiple-handlers). Obslužná rutina může dědit [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), kde `TRequirement` je požadavek na zpracování. Alternativně může implementovat obslužná rutina [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) pro zpracování více než jeden typ požadavku.

### <a name="use-a-handler-for-one-requirement"></a>Použijte obslužnou rutinu pro jeden požadavek

<a name="security-authorization-handler-example"></a>

Následuje příklad relace, ve kterém obslužnou rutinu minimálním stářím využívá jeden požadavek:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Předchozí kód určuje, zda má aktuální uživatel hlavní datum narození deklarace identity, který byl vydán vystavitelem známé a důvěryhodné. Autorizace nelze provést v případě deklarace identity nebyl nalezen, v takovém případě je vrácená dokončené úlohy. Pokud deklarace identity, se počítá stáří uživatele. Pokud uživatel splňuje definované požadavek na minimální věk, autorizace se považuje za úspěšné. Když je ověřování úspěšné, `context.Succeed` vyvolání splněna požadavek jako svůj jediný parametr.

### <a name="use-a-handler-for-multiple-requirements"></a>Použijte obslužnou rutinu pro víc požadavků

Následuje příklad vztah jeden mnoho, ve kterém obslužnou rutinu oprávnění využívá tři požadavky:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Předchozí kód prochází [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;vlastnost obsahující požadavky není označena jako úspěšné. Pokud uživatel, má oprávnění ke čtení, musí být potvrdí vlastníka nebo sponzor přístupu k požadovanému zdroji. Pokud uživatel upravit nebo odstranit oprávnění, uživatel musí být vlastníkem k přístupu k požadovanému zdroji. Když je ověřování úspěšné, `context.Succeed` vyvolání splněna požadavek jako svůj jediný parametr.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Obslužná rutina registrace

Obslužné rutiny jsou zaregistrované v kolekci služby během konfigurace. Příklad:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

Každý obslužná rutina se přidá do kolekce služeb vyvoláním `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.

## <a name="what-should-a-handler-return"></a>Co by měla vrátit obslužná rutina?

Všimněte si, že `Handle` metoda v [příklad obslužná rutina](#security-authorization-handler-example) nevrací žádnou hodnotu. Jak je ve stavu úspěch nebo selhání uvedené?

* Obslužná rutina označuje úspěch voláním `context.Succeed(IAuthorizationRequirement requirement)`, předávání požadavek, který byl úspěšně ověřen.

* Obslužná rutina nepotřebuje řešit selhání obecně platí, jako ostatních obslužných rutin pro stejný požadavek může být úspěšné.

* Chcete-li zaručit selhání, i v případě ostatních obslužných rutin požadavek úspěšné, volejte `context.Fail`.

Pokud nastavíte hodnotu `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) vlastnost (k dispozici v technologii ASP.NET Core 1.1 nebo novější) zkratům spuštění obslužné rutiny při `context.Fail` je volána. `InvokeHandlersAfterFailure` použije se výchozí hodnota `true`, v takovém případě jsou volány všechny obslužné rutiny. To umožňuje požadavek na vytvoření vedlejší účinky, například protokolování, které vždy provádět i v případě `context.Fail` byla volána v jiná obslužná rutina.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Proč byste měli více obslužných rutin pro požadavek?

V případech, kde chcete vyhodnocení na **nebo** základ, implementovat několik obslužné rutiny pro jeden požadavek. Například společnost Microsoft nemá dveří, které otevíraly jenom s kartami klíče. Pokud necháte klíče karty v domácnostech, recepční vytiskne dočasné štítku a otevře dvířka pro vás. V tomto scénáři by mít jeden požadavek, *BuildingEntry*, ale více obslužných rutin, každé z nich zkoumání jeden požadavek.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Zkontrolujte, zda jsou oba obslužné rutiny [zaregistrován](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Je-li buď obslužná rutina úspěšné při zásady vyhodnotí `BuildingEntryRequirement`, úspěšné vyhodnocení zásad.

## <a name="using-a-func-to-fulfill-a-policy"></a>Pomocí funkci func ke splnění zásadu

Mohou nastat situace, ve které splnění zásady se snadno express v kódu. Je možné poskytnout `Func<AuthorizationHandlerContext, bool>` při konfiguraci zásad služby s `RequireAssertion` tvůrce zásad.

Například předchozí `BadgeEntryHandler` by mohla být přepsána následujícím způsobem:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Přístup k kontext požadavku MVC v obslužné rutiny

`HandleRequirementAsync` Metoda implementovat v obslužnou rutinu autorizace má dva parametry: `AuthorizationHandlerContext` a `TRequirement` jsou zpracování. Rozhraní, jako je například MVC nebo Jabbr jsou zdarma objekt, který chcete přidat `Resource` vlastnost `AuthorizationHandlerContext` předat doplňující informace.

Například MVC předá instance [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) v `Resource` vlastnost. Tato vlastnost poskytuje přístup k `HttpContext`, `RouteData`a všechno jinak poskytované MVC a stránky Razor.

Použití `Resource` vlastnost je konkrétní rozhraní. Pomocí informací v `Resource` vlastnost omezení zásad autorizace pro konkrétní rozhraní. Měli přetypovat `Resource` pomocí vlastnosti `as` – klíčové slovo a pak potvrďte přetypování má být úspěšné, aby váš kód není havárií s `InvalidCastException` spuštění na ostatní platformy:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
