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
ms.openlocfilehash: 723e371e0d0b4877f96898c68cd59b433fa97dc1
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2018
---
# <a name="resource-based-authorization"></a><span data-ttu-id="30080-103">Ověření na základě prostředků</span><span class="sxs-lookup"><span data-stu-id="30080-103">Resource-based authorization</span></span>

<span data-ttu-id="30080-104">Strategie autorizace závisí na prostředek přistupuje.</span><span class="sxs-lookup"><span data-stu-id="30080-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="30080-105">Vezměte v úvahu dokument, který má vlastnost autora.</span><span class="sxs-lookup"><span data-stu-id="30080-105">Consider a document which has an author property.</span></span> <span data-ttu-id="30080-106">K aktualizaci dokumentu je povoleno pouze autora.</span><span class="sxs-lookup"><span data-stu-id="30080-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="30080-107">V důsledku toho dokumentu musí načíst z úložiště dat, než dojde k vyhodnocení autorizace.</span><span class="sxs-lookup"><span data-stu-id="30080-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="30080-108">Před datové vazby a před spuštěním obslužná rutina stránky nebo akce, který načte dokumentu dojde k vyhodnocení atribut.</span><span class="sxs-lookup"><span data-stu-id="30080-108">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="30080-109">Z těchto důvodů, deklarativní autorizace pomocí `[Authorize]` atribut nestačí.</span><span class="sxs-lookup"><span data-stu-id="30080-109">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="30080-110">Místo toho můžete vyvolat metodu vlastní autorizace&mdash;styl známé jako imperativní autorizace.</span><span class="sxs-lookup"><span data-stu-id="30080-110">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="30080-111">Použití [ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample)) prozkoumat funkce popsané v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="30080-111">Use the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

<span data-ttu-id="30080-112">[Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněn autorizace](xref:security/authorization/secure-data) obsahuje ukázkovou aplikaci, která používá ověřování založené na prostředcích.</span><span class="sxs-lookup"><span data-stu-id="30080-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="30080-113">Použít imperativní autorizace</span><span class="sxs-lookup"><span data-stu-id="30080-113">Use imperative authorization</span></span>

<span data-ttu-id="30080-114">Autorizace je implementovaný jako [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) služby a je zaregistrován v kolekci služby v rámci `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="30080-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="30080-115">Služba je k dispozici prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) obslužné rutiny stránky nebo akce.</span><span class="sxs-lookup"><span data-stu-id="30080-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="30080-116">`IAuthorizationService`má dva `AuthorizeAsync` přetížení metody: přijímá jeden prostředek a název zásady a dalších přijetí prostředek a seznam požadavků pro vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="30080-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="30080-117">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="30080-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="30080-118">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="30080-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="30080-119">V následujícím příkladu je prostředek, který nelze zabezpečit načten do vlastní `Document` objektu.</span><span class="sxs-lookup"><span data-stu-id="30080-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="30080-120">`AuthorizeAsync` Přetížení je volána k určení, zda má aktuální uživatel může zadaný dokument upravovat.</span><span class="sxs-lookup"><span data-stu-id="30080-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="30080-121">Vlastní zásady autorizace "EditPolicy" je rozdělen do rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="30080-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="30080-122">V tématu [vlastní na základě zásad autorizace](xref:security/authorization/policies) Další informace o vytváření zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="30080-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="30080-123">Následující kód ukázky předpokládá, že byla spuštěna ověřování a sadu `User` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="30080-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="30080-124">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="30080-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="30080-125">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="30080-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="30080-126">Zápis obslužné rutiny založené na prostředcích</span><span class="sxs-lookup"><span data-stu-id="30080-126">Write a resource-based handler</span></span>

<span data-ttu-id="30080-127">Zápis obslužné rutiny pro ověření na základě prostředků není výrazně liší od [zápis obslužné rutiny prostý požadavky](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="30080-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="30080-128">Vytvořte třídu vlastní požadavek a implementovat třídu obslužné rutiny požadavku.</span><span class="sxs-lookup"><span data-stu-id="30080-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="30080-129">Třídu obslužné rutiny určuje požadavek i typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="30080-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="30080-130">Například obslužnou rutinu využitím `SameAuthorRequirement` požadavek a `Document` prostředků vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="30080-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="30080-131">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="30080-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="30080-132">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="30080-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="30080-133">Požadavek a obslužné rutiny v registru `Startup.ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="30080-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="30080-134">Provozní požadavky</span><span class="sxs-lookup"><span data-stu-id="30080-134">Operational requirements</span></span>

<span data-ttu-id="30080-135">Pokud provádíte rozhodnutí podle výstupy CRUD (**C**tvořit, **R**rohlášení, **U**ktualizovat, **D**dstranit) operace, použijte [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) pomocná třída.</span><span class="sxs-lookup"><span data-stu-id="30080-135">If you're making decisions based on the outcomes of CRUD (**C**reate, **R**ead, **U**pdate, **D**elete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="30080-136">Tato třída umožňuje zapisovat jeden obslužné rutině místo jednotlivých tříd pro každý typ operace.</span><span class="sxs-lookup"><span data-stu-id="30080-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="30080-137">Pokud chcete použít, zadejte některé názvy operace:</span><span class="sxs-lookup"><span data-stu-id="30080-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="30080-138">Obslužná rutina je implementovaná následujícím způsobem pomocí `OperationAuthorizationRequirement` požadavek a `Document` prostředků:</span><span class="sxs-lookup"><span data-stu-id="30080-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="30080-139">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="30080-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="30080-140">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="30080-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="30080-141">Obslužná rutina předchozí ověří pomocí prostředku, identitu uživatele a požadavek na operaci `Name` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="30080-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="30080-142">Chcete-li volání obslužné rutiny provozní prostředků, zadejte operaci při vyvolání `AuthorizeAsync` v obslužné rutině stránky nebo akce.</span><span class="sxs-lookup"><span data-stu-id="30080-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="30080-143">Následující příklad určuje, zda ověřený uživatel může zadaný dokument zobrazit.</span><span class="sxs-lookup"><span data-stu-id="30080-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="30080-144">Následující kód ukázky předpokládá, že byla spuštěna ověřování a sadu `User` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="30080-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="30080-145">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="30080-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="30080-146">Pokud autorizace úspěšné, vrátí se na stránce pro zobrazení dokumentu.</span><span class="sxs-lookup"><span data-stu-id="30080-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="30080-147">Pokud autorizace selže, ale uživatel ověřen, vrácení `ForbidResult` informuje veškerý middleware ověřování, který autorizace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="30080-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="30080-148">A `ChallengeResult` se vám při ověřování musí být provedeno.</span><span class="sxs-lookup"><span data-stu-id="30080-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="30080-149">Pro klienty interaktivní prohlížeče může být vhodné přesměruje uživatele na přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="30080-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="30080-150">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="30080-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="30080-151">Pokud autorizace úspěšné, vrátí se zobrazení pro dokument.</span><span class="sxs-lookup"><span data-stu-id="30080-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="30080-152">Pokud autorizace selže, vrácení `ChallengeResult` informuje veškerý middleware ověřování, autorizace se nezdařila, a middleware může trvat odpovídající odpověď.</span><span class="sxs-lookup"><span data-stu-id="30080-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="30080-153">Odpovídající odpověď může být vrátí stavový kód 401 nebo 403.</span><span class="sxs-lookup"><span data-stu-id="30080-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="30080-154">Pro klienty interaktivní prohlížeče to může znamenat přesměrovat uživatele na přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="30080-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
