---
title: Autorizace na základě prostředků v ASP.NET Core
author: scottaddie
description: Zjistěte, jak implementovat ověřování na základě prostředků v aplikaci ASP.NET Core atribut Authorize nestačí.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
uid: security/authorization/resourcebased
ms.openlocfilehash: 6a110a69c58d5e20a15198378510486daec3d452
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342286"
---
# <a name="resource-based-authorization-in-aspnet-core"></a>Autorizace na základě prostředků v ASP.NET Core

Strategii autorizace závisí na prostředek, ke kterému přistupujete. Vezměte v úvahu dokument, který má vlastnost Autor. Pouze autor můžou aktualizovat dokument. V důsledku toho dokumentu musí načíst z úložiště dat, než dojde k vyhodnocení autorizace.

Před datové vazby a před spuštěním obslužná rutina stránky nebo akci, která načte dokument dojde k vyhodnocení atribut. Z těchto důvodů, deklarativní autorizace pomocí `[Authorize]` atribut nestačí. Místo toho můžete vyvolat vlastní autorizační metoda&mdash;styl označované jako imperativní autorizace.

Použití [ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample)) a prozkoumejte funkce popsané v tomto tématu.

[Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněnými autorizací](xref:security/authorization/secure-data) obsahuje ukázkovou aplikaci, která používá ověřování založené na resource.

## <a name="use-imperative-authorization"></a>Použití imperativní autorizace

Autorizace je implementovaný jako [služby IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) služby a je registrován v kolekci služby v rámci `Startup` třídy. Služba je k dispozici prostřednictvím [injektáž závislostí](xref:fundamentals/dependency-injection) obslužných rutin stránky nebo provádět akce.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` má dva `AuthorizeAsync` přetížení metody: jedna přijímá prostředku a název zásady a druhý přijímá prostředku a seznam všech požadavků k vyhodnocení.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

---

<a name="security-authorization-resource-based-imperative"></a>

V následujícím příkladu je prostředek, který má být zabezpečeny načten do vlastní `Document` objektu. `AuthorizeAsync` Přetížení je vyvoláno pro zjištění, zda má aktuální uživatel může upravit zadaný dokument. Vlastní zásady autorizace "EditPolicy" je rozdělen do rozhodnutí. Zobrazit [vlastní autorizace na základě zásad](xref:security/authorization/policies) pro další informace o vytváření zásad autorizace.

> [!NOTE]
> Následující kód ukázky předpokládají bylo spuštěno ověřování a nastavení `User` vlastnost.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>Zápis obslužné rutiny založené na prostředcích

Zápis obslužné rutiny pro ověřování na základě prostředku se výrazně liší od [zápis obslužné rutiny jednoduché požadavky](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Vytvořit vlastní požadavek třídy a implementovat třídu obslužné rutiny požadavku. Třída obslužné rutiny určuje požadavky a typ prostředku. Například obslužnou rutinu využitím `SameAuthorRequirement` požadavek a `Document` prostředků vypadá takto:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

Registrovat požadavku a obslužné rutiny v `Startup.ConfigureServices` metody:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Provozní požadavky

Pokud jste už rozhodování na základě výsledků operace CRUD (vytváření, čtení, Update, Delete), použijte [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) pomocná třída. Tato třída umožňuje zapisovat jedna obslužná rutina namísto jednotlivých tříd pro každý typ operace. Pro použití je třeba zadejte názvy některé operace:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

Obslužná rutina je implementován takto, pomocí `OperationAuthorizationRequirement` požadavek a `Document` prostředků:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

Předchozí obslužná rutina ověří pomocí prostředku, identitu uživatele a požadavek na operaci `Name` vlastnost.

Volání obslužné rutiny provozní prostředků, zadejte operaci při vyvolání `AuthorizeAsync` v obslužné rutině stránky nebo akce. Následující příklad určuje, zda chcete-li zobrazit poskytnutý dokument smí obsahovat ověřeného uživatele.

> [!NOTE]
> Následující kód ukázky předpokládají bylo spuštěno ověřování a nastavení `User` vlastnost.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Pokud autorizace úspěšná, vrátí se stránka pro zobrazení dokumentu. Pokud autorizace selže, ale uživatel je ověřen, vrací `ForbidResult` informuje veškerý middleware ověřování, který autorizace se nezdařila. A `ChallengeResult` je vrácena při ověřování musí být provedeno. Pro klienty prohlížeče interaktivní může být vhodné přesměruje uživatele na přihlašovací stránku.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Pokud autorizace úspěšná, vrátí se zobrazení pro dokument. Pokud se ověření nezdaří, vrátí `ChallengeResult` informuje veškerý middleware ověřování, autorizace se nezdařila a middleware může trvat odpovídající odpověď. Stavový kód 401 nebo 403 může vrací odpovídající odpověď. Pro klienty prohlížeče interaktivní může to znamenat přesměruje uživatele na přihlašovací stránku.

---
