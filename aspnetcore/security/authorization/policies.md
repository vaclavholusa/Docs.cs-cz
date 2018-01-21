---
title: "Na základě zásad autorizace v ASP.NET Core"
author: rick-anderson
description: "Zjistěte, jak vytvořit a použít vlastní autorizační zásady obslužné rutiny pro vynucení požadavky autorizace v aplikaci ASP.NET Core."
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 1067e97dd6e71021929aa3690b0c3f5bfc6c9724
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="custom-policy-based-authorization"></a>Autorizace uživatele na základě zásad

Pod zahrnuje [autorizace na základě rolí](xref:security/authorization/roles) a [založené na deklaracích identity autorizace](xref:security/authorization/claims) použít požadavek, obslužnou rutinu požadavků a předem nakonfigurované zásad. Tyto stavební bloky podporovat výraz hodnocení autorizace v kódu. Výsledkem je strukturou autorizací bohatší, opakovaně použitelný, možností intenzivního testování.

Zásad autorizace se skládá z jednoho nebo více požadavků. Jako součást konfigurace služby autorizace, je zaregistrován v `ConfigureServices` metodu `Startup` třídy:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

V předchozím příkladu se vytvoří zásadu "AtLeast21". Má jednoho požadavku,., o minimálním stářím, které jsou dodané jako parametr pro požadavek.

Zásady se aplikují pomocí `[Authorize]` atribut s názvem zásady. Příklad:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>Požadavky

Požadavek na autorizaci je kolekce dat parametry, které zásadu lze použít k vyhodnocení aktuální hlavní název uživatele. V našem "AtLeast21" zásady, je požadavek na jeden parametr&mdash;minimálním stářím. Implementuje požadavek `IAuthorizationRequirement`, což je rozhraní prázdný značky. Požadavek na minimální věk parametrizované může být implementováno takto:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> Požadavek není nutné mít dat nebo vlastností.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Obslužné rutiny autorizace

Obslužné rutiny autorizace je zodpovědná za vyhodnocení vlastnosti požadavek. Obslužná rutina autorizace vyhodnotí požadavky proti zadaný `AuthorizationHandlerContext` k určení, zda je povolen přístup. Může mít požadavek [více obslužných rutin](#security-authorization-policies-based-multiple-handlers). Obslužné rutiny dědění `AuthorizationHandler<T>`, kde `T` je požadavek na zpracování.

<a name="security-authorization-handler-example"></a>

Obslužná rutina minimálním stářím může vypadat například takto:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Předchozí kód určuje, zda má aktuální uživatel hlavní datum narození deklarace identity, který byl vydán vystavitelem známé a důvěryhodné. Autorizace nelze provést v případě deklarace identity nebyl nalezen, v takovém případě je vrácená dokončené úlohy. Pokud deklarace identity, se počítá stáří uživatele. Pokud uživatel splňuje definované požadavek na minimální věk, autorizace se považuje za úspěšné. Když je ověřování úspěšné, `context.Succeed` vyvolání splněna požadavek jako parametr.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Obslužná rutina registrace

Obslužné rutiny jsou zaregistrované v kolekci služby během konfigurace. Příklad:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

Každý obslužná rutina se přidá do kolekce služeb vyvoláním `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.

## <a name="what-should-a-handler-return"></a>Co by měla vrátit obslužná rutina?

Všimněte si, že `Handle` metoda v [příklad obslužná rutina](#security-authorization-handler-example) nevrací žádnou hodnotu. Jak je ve stavu úspěch nebo selhání uvedené?

* Obslužná rutina označuje úspěch voláním `context.Succeed(IAuthorizationRequirement requirement)`, předávání požadavek, který byl úspěšně ověřen.

* Obslužná rutina pro zpracování chyby obecně platí, jako ostatní obslužné rutiny pro požadavek na stejné může být úspěšné, nemusí.

* Chcete-li zaručit selhání, i v případě ostatních obslužných rutin požadavek úspěšné, volejte `context.Fail`.

Bez ohledu na to, co bude volat uvnitř vaší obslužné rutiny budou všechny obslužné rutiny pro požadavek volat, pokud zásady vyžaduje požadavek. To umožňuje požadavky tak, aby měl vedlejší účinky, například protokolování, který bude mít místní i v případě `context.Fail()` byla volána v jiná obslužná rutina.

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
