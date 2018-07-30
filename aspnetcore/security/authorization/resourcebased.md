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
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="6c977-103">Autorizace na základě prostředků v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6c977-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="6c977-104">Strategii autorizace závisí na prostředek, ke kterému přistupujete.</span><span class="sxs-lookup"><span data-stu-id="6c977-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="6c977-105">Vezměte v úvahu dokument, který má vlastnost Autor.</span><span class="sxs-lookup"><span data-stu-id="6c977-105">Consider a document which has an author property.</span></span> <span data-ttu-id="6c977-106">Pouze autor můžou aktualizovat dokument.</span><span class="sxs-lookup"><span data-stu-id="6c977-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="6c977-107">V důsledku toho dokumentu musí načíst z úložiště dat, než dojde k vyhodnocení autorizace.</span><span class="sxs-lookup"><span data-stu-id="6c977-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="6c977-108">Před datové vazby a před spuštěním obslužná rutina stránky nebo akci, která načte dokument dojde k vyhodnocení atribut.</span><span class="sxs-lookup"><span data-stu-id="6c977-108">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="6c977-109">Z těchto důvodů, deklarativní autorizace pomocí `[Authorize]` atribut nestačí.</span><span class="sxs-lookup"><span data-stu-id="6c977-109">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="6c977-110">Místo toho můžete vyvolat vlastní autorizační metoda&mdash;styl označované jako imperativní autorizace.</span><span class="sxs-lookup"><span data-stu-id="6c977-110">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="6c977-111">Použití [ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample)) a prozkoumejte funkce popsané v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="6c977-111">Use the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

<span data-ttu-id="6c977-112">[Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněnými autorizací](xref:security/authorization/secure-data) obsahuje ukázkovou aplikaci, která používá ověřování založené na resource.</span><span class="sxs-lookup"><span data-stu-id="6c977-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="6c977-113">Použití imperativní autorizace</span><span class="sxs-lookup"><span data-stu-id="6c977-113">Use imperative authorization</span></span>

<span data-ttu-id="6c977-114">Autorizace je implementovaný jako [služby IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) služby a je registrován v kolekci služby v rámci `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="6c977-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="6c977-115">Služba je k dispozici prostřednictvím [injektáž závislostí](xref:fundamentals/dependency-injection) obslužných rutin stránky nebo provádět akce.</span><span class="sxs-lookup"><span data-stu-id="6c977-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="6c977-116">`IAuthorizationService` má dva `AuthorizeAsync` přetížení metody: jedna přijímá prostředku a název zásady a druhý přijímá prostředku a seznam všech požadavků k vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="6c977-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6c977-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6c977-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6c977-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6c977-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="6c977-119">V následujícím příkladu je prostředek, který má být zabezpečeny načten do vlastní `Document` objektu.</span><span class="sxs-lookup"><span data-stu-id="6c977-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="6c977-120">`AuthorizeAsync` Přetížení je vyvoláno pro zjištění, zda má aktuální uživatel může upravit zadaný dokument.</span><span class="sxs-lookup"><span data-stu-id="6c977-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="6c977-121">Vlastní zásady autorizace "EditPolicy" je rozdělen do rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="6c977-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="6c977-122">Zobrazit [vlastní autorizace na základě zásad](xref:security/authorization/policies) pro další informace o vytváření zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="6c977-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="6c977-123">Následující kód ukázky předpokládají bylo spuštěno ověřování a nastavení `User` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6c977-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6c977-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6c977-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6c977-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6c977-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="6c977-126">Zápis obslužné rutiny založené na prostředcích</span><span class="sxs-lookup"><span data-stu-id="6c977-126">Write a resource-based handler</span></span>

<span data-ttu-id="6c977-127">Zápis obslužné rutiny pro ověřování na základě prostředku se výrazně liší od [zápis obslužné rutiny jednoduché požadavky](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="6c977-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="6c977-128">Vytvořit vlastní požadavek třídy a implementovat třídu obslužné rutiny požadavku.</span><span class="sxs-lookup"><span data-stu-id="6c977-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="6c977-129">Třída obslužné rutiny určuje požadavky a typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="6c977-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="6c977-130">Například obslužnou rutinu využitím `SameAuthorRequirement` požadavek a `Document` prostředků vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6c977-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6c977-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6c977-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6c977-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6c977-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="6c977-133">Registrovat požadavku a obslužné rutiny v `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="6c977-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="6c977-134">Provozní požadavky</span><span class="sxs-lookup"><span data-stu-id="6c977-134">Operational requirements</span></span>

<span data-ttu-id="6c977-135">Pokud jste už rozhodování na základě výsledků operace CRUD (vytváření, čtení, Update, Delete), použijte [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) pomocná třída.</span><span class="sxs-lookup"><span data-stu-id="6c977-135">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="6c977-136">Tato třída umožňuje zapisovat jedna obslužná rutina namísto jednotlivých tříd pro každý typ operace.</span><span class="sxs-lookup"><span data-stu-id="6c977-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="6c977-137">Pro použití je třeba zadejte názvy některé operace:</span><span class="sxs-lookup"><span data-stu-id="6c977-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="6c977-138">Obslužná rutina je implementován takto, pomocí `OperationAuthorizationRequirement` požadavek a `Document` prostředků:</span><span class="sxs-lookup"><span data-stu-id="6c977-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6c977-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6c977-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6c977-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6c977-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="6c977-141">Předchozí obslužná rutina ověří pomocí prostředku, identitu uživatele a požadavek na operaci `Name` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6c977-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="6c977-142">Volání obslužné rutiny provozní prostředků, zadejte operaci při vyvolání `AuthorizeAsync` v obslužné rutině stránky nebo akce.</span><span class="sxs-lookup"><span data-stu-id="6c977-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="6c977-143">Následující příklad určuje, zda chcete-li zobrazit poskytnutý dokument smí obsahovat ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="6c977-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="6c977-144">Následující kód ukázky předpokládají bylo spuštěno ověřování a nastavení `User` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6c977-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6c977-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6c977-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="6c977-146">Pokud autorizace úspěšná, vrátí se stránka pro zobrazení dokumentu.</span><span class="sxs-lookup"><span data-stu-id="6c977-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="6c977-147">Pokud autorizace selže, ale uživatel je ověřen, vrací `ForbidResult` informuje veškerý middleware ověřování, který autorizace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="6c977-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="6c977-148">A `ChallengeResult` je vrácena při ověřování musí být provedeno.</span><span class="sxs-lookup"><span data-stu-id="6c977-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="6c977-149">Pro klienty prohlížeče interaktivní může být vhodné přesměruje uživatele na přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="6c977-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6c977-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6c977-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="6c977-151">Pokud autorizace úspěšná, vrátí se zobrazení pro dokument.</span><span class="sxs-lookup"><span data-stu-id="6c977-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="6c977-152">Pokud se ověření nezdaří, vrátí `ChallengeResult` informuje veškerý middleware ověřování, autorizace se nezdařila a middleware může trvat odpovídající odpověď.</span><span class="sxs-lookup"><span data-stu-id="6c977-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="6c977-153">Stavový kód 401 nebo 403 může vrací odpovídající odpověď.</span><span class="sxs-lookup"><span data-stu-id="6c977-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="6c977-154">Pro klienty prohlížeče interaktivní může to znamenat přesměruje uživatele na přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="6c977-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
