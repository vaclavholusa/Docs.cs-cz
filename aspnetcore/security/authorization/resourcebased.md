---
title: "Ověření na základě prostředků v ASP.NET Core"
author: scottaddie
description: "Zjistěte, jak implementovat autorizace na základě prostředků v aplikaci ASP.NET Core při atribut autorizovat nestačí."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/resourcebased
ms.openlocfilehash: 708f306da740870b106cbeeb96879480f8745439
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="resource-based-authorization"></a>Ověření na základě prostředků

Podle [Scott Addie](https://twitter.com/Scott_Addie)

Strategie autorizace závisí na prostředek přistupuje. Vezměte v úvahu dokument, který má vlastnost autora. K aktualizaci dokumentu je povoleno pouze autora. V důsledku toho dokumentu musí načíst z úložiště dat, než dojde k vyhodnocení autorizace.

Před datové vazby a před spuštěním obslužná rutina stránky nebo akce, který načte dokumentu dojde k vyhodnocení atribut. Z těchto důvodů, deklarativní autorizace pomocí `[Authorize]` atribut nestačí. Místo toho můžete vyvolat metodu vlastní autorizace&mdash;styl známé jako imperativní autorizace.

Použití [ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample)) prozkoumat funkce popsané v tomto tématu.

## <a name="use-imperative-authorization"></a>Použít imperativní autorizace

Autorizace je implementovaný jako [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) služby a je zaregistrován v kolekci služby v rámci `Startup` třídy. Služba je k dispozici prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) obslužné rutiny stránky nebo akce.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService`má dva `AuthorizeAsync` přetížení metody: přijímá jeden prostředek a název zásady a dalších přijetí prostředek a seznam požadavků pro vyhodnocení.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

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

V následujícím příkladu je prostředek, který nelze zabezpečit načten do vlastní `Document` objektu. `AuthorizeAsync` Přetížení je volána k určení, zda má aktuální uživatel může zadaný dokument upravovat. Vlastní zásady autorizace "EditPolicy" je rozdělen do rozhodnutí. V tématu [vlastní na základě zásad autorizace](xref:security/authorization/policies) Další informace o vytváření zásad autorizace.

> [!NOTE]
> Následující kód ukázky předpokládá, že byla spuštěna ověřování a sadu `User` vlastnost.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>Zápis obslužné rutiny založené na prostředcích

Zápis obslužné rutiny pro ověření na základě prostředků není výrazně liší od [zápis obslužné rutiny prostý požadavky](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Vytvořte třídu vlastní požadavek a implementovat třídu obslužné rutiny požadavku. Třídu obslužné rutiny určuje požadavek i typ prostředku. Například obslužnou rutinu využitím `SameAuthorRequirement` požadavek a `Document` prostředků vypadá takto:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

Požadavek a obslužné rutiny v registru `Startup.ConfigureServices` metoda:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Provozní požadavky

Pokud provádíte rozhodnutí podle výstupy CRUD (**C**tvořit, **R**rohlášení, **U**ktualizovat, **D**dstranit) operace, použijte [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) pomocná třída. Tato třída umožňuje zapisovat jeden obslužné rutině místo jednotlivých tříd pro každý typ operace. Pokud chcete použít, zadejte některé názvy operace:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

Obslužná rutina je implementovaná následujícím způsobem pomocí `OperationAuthorizationRequirement` požadavek a `Document` prostředků:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

Obslužná rutina předchozí ověří pomocí prostředku, identitu uživatele a požadavek na operaci `Name` vlastnost.

Chcete-li volání obslužné rutiny provozní prostředků, zadejte operaci při vyvolání `AuthorizeAsync` v obslužné rutině stránky nebo akce. Následující příklad určuje, zda ověřený uživatel může zadaný dokument zobrazit.

> [!NOTE]
> Následující kód ukázky předpokládá, že byla spuštěna ověřování a sadu `User` vlastnost.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Pokud autorizace úspěšné, vrátí se na stránce pro zobrazení dokumentu. Pokud autorizace selže, ale uživatel ověřen, vrácení `ForbidResult` informuje veškerý middleware ověřování, který autorizace se nezdařila. A `ChallengeResult` se vám při ověřování musí být provedeno. Pro klienty interaktivní prohlížeče může být vhodné přesměruje uživatele na přihlašovací stránku.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Pokud autorizace úspěšné, vrátí se zobrazení pro dokument. Pokud autorizace selže, vrácení `ChallengeResult` informuje veškerý middleware ověřování, autorizace se nezdařila, a middleware může trvat odpovídající odpověď. Odpovídající odpověď může být vrátí stavový kód 401 nebo 403. Pro klienty interaktivní prohlížeče to může znamenat přesměrovat uživatele na přihlašovací stránku.

---
