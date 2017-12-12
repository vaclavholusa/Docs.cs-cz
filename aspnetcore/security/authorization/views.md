---
title: "Ověření na základě zobrazení v rozhraní ASP.NET MVC jádra"
author: rick-anderson
description: "Tento dokument ukazuje, jak využívat službu autorizace uvnitř zobrazení syntaxe Razor rozhraní ASP.NET Core a vložit."
keywords: ASP.NET Core, autorizace, IAuthorizationService, Razor autorizace
ms.author: riande
manager: wpickett
ms.date: 10/30/2017
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 756431f398c29376ab0ecd6c4f4d1db4f8022b0b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="view-based-authorization"></a><span data-ttu-id="7e850-104">Ověření na základě zobrazení</span><span class="sxs-lookup"><span data-stu-id="7e850-104">View-based authorization</span></span>

<span data-ttu-id="7e850-105">Vývojář často chce zobrazení, skrytí nebo v opačném případě upravte uživatelského rozhraní na základě aktuální identity uživatele.</span><span class="sxs-lookup"><span data-stu-id="7e850-105">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="7e850-106">Můžete přístup ke službě ověřování v rámci zobrazení MVC prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7e850-106">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="7e850-107">Chcete-li vložit služby autorizace do zobrazení Razor, použijte `@inject` – direktiva:</span><span class="sxs-lookup"><span data-stu-id="7e850-107">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="7e850-108">Pokud chcete službu ověřování v každé zobrazení, umístit `@inject` direktivy do *_ViewImports.cshtml* soubor *zobrazení* adresáře.</span><span class="sxs-lookup"><span data-stu-id="7e850-108">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="7e850-109">Další informace najdete v tématu [vkládání závislostí do zobrazení](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7e850-109">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="7e850-110">Používat službu vloženého autorizace k vyvolání `AuthorizeAsync` v úplně stejně, jako by kontrolovat při [autorizace na základě prostředků](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="7e850-110">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7e850-111">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="7e850-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7e850-112">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="7e850-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="7e850-113">V některých případech bude prostředek zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="7e850-113">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="7e850-114">Vyvolání `AuthorizeAsync` v úplně stejně, jako by kontrolovat při [autorizace na základě prostředků](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="7e850-114">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7e850-115">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="7e850-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7e850-116">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="7e850-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="7e850-117">V předchozí kód model předán jako prostředek, kterou má provést vyhodnocení zásad v úvahu.</span><span class="sxs-lookup"><span data-stu-id="7e850-117">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="7e850-118">Nemusíte spoléhat na přepnutím viditelnost prvků uživatelského rozhraní aplikace jako jedinou autorizace kontroly.</span><span class="sxs-lookup"><span data-stu-id="7e850-118">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="7e850-119">Skrytí elementu uživatelského rozhraní nemusí úplně zamezit přístupu k jeho přidruženému kontroleru akce.</span><span class="sxs-lookup"><span data-stu-id="7e850-119">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="7e850-120">Představte si třeba tlačítko v předchozím fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="7e850-120">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="7e850-121">Uživatele můžete vyvolat `Edit` metody akce, pokud uživatel zná prostředků relativní adresa URL je */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="7e850-121">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="7e850-122">Z tohoto důvodu `Edit` metoda akce proveďte vlastní kontroly autorizace.</span><span class="sxs-lookup"><span data-stu-id="7e850-122">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
