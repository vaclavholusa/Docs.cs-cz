---
title: Ověření na základě zobrazení v rozhraní ASP.NET MVC jádra
author: rick-anderson
description: Tento dokument ukazuje, jak využívat službu autorizace uvnitř zobrazení syntaxe Razor rozhraní ASP.NET Core a vložit.
manager: wpickett
ms.author: riande
ms.date: 10/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/views
ms.openlocfilehash: dad59a297efb4648755436fbd07742f95af97fb2
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="4660f-103">Ověření na základě zobrazení v rozhraní ASP.NET MVC jádra</span><span class="sxs-lookup"><span data-stu-id="4660f-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="4660f-104">Vývojář často chce zobrazení, skrytí nebo v opačném případě upravte uživatelského rozhraní na základě aktuální identity uživatele.</span><span class="sxs-lookup"><span data-stu-id="4660f-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="4660f-105">Můžete přístup ke službě ověřování v rámci zobrazení MVC prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4660f-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="4660f-106">Chcete-li vložit služby autorizace do zobrazení Razor, použijte `@inject` – direktiva:</span><span class="sxs-lookup"><span data-stu-id="4660f-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="4660f-107">Pokud chcete službu ověřování v každé zobrazení, umístit `@inject` direktivy do *_ViewImports.cshtml* soubor *zobrazení* adresáře.</span><span class="sxs-lookup"><span data-stu-id="4660f-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="4660f-108">Další informace najdete v tématu [vkládání závislostí do zobrazení](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4660f-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="4660f-109">Používat službu vloženého autorizace k vyvolání `AuthorizeAsync` v úplně stejně, jako by kontrolovat při [autorizace na základě prostředků](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="4660f-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4660f-110">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="4660f-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4660f-111">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="4660f-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="4660f-112">V některých případech bude prostředek zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="4660f-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="4660f-113">Vyvolání `AuthorizeAsync` v úplně stejně, jako by kontrolovat při [autorizace na základě prostředků](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="4660f-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4660f-114">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="4660f-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4660f-115">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="4660f-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="4660f-116">V předchozí kód model předán jako prostředek, kterou má provést vyhodnocení zásad v úvahu.</span><span class="sxs-lookup"><span data-stu-id="4660f-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="4660f-117">Nemusíte spoléhat na přepnutím viditelnost prvků uživatelského rozhraní aplikace jako jedinou autorizace kontroly.</span><span class="sxs-lookup"><span data-stu-id="4660f-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="4660f-118">Skrytí elementu uživatelského rozhraní nemusí úplně zamezit přístupu k jeho přidruženému kontroleru akce.</span><span class="sxs-lookup"><span data-stu-id="4660f-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="4660f-119">Představte si třeba tlačítko v předchozím fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="4660f-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="4660f-120">Uživatele můžete vyvolat `Edit` metody akce, pokud uživatel zná prostředků relativní adresa URL je */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="4660f-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="4660f-121">Z tohoto důvodu `Edit` metoda akce proveďte vlastní kontroly autorizace.</span><span class="sxs-lookup"><span data-stu-id="4660f-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
