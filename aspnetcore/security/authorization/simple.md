---
title: Jednoduché autorizace v ASP.NET Core
author: rick-anderson
description: Naučte se používat atribut autorizovat k omezení přístupu k ASP.NET Core kontrolery a akce.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961120"
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="4d13a-103">Jednoduché autorizace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d13a-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="4d13a-104">Autorizace v MVC je řízen pomocí `AuthorizeAttribute` atribut a jeho různé parametry.</span><span class="sxs-lookup"><span data-stu-id="4d13a-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="4d13a-105">V nejjednodušším, použití `AuthorizeAttribute` atribut kontroler nebo akce omezíte přístup k kontroler nebo akce, které všem ověřeným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="4d13a-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="4d13a-106">Například následující kód omezuje přístup na `AccountController` všem ověřeným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="4d13a-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="4d13a-107">Pokud chcete se autorizace provádí akce, nikoli na řadič, použít `AuthorizeAttribute` atribut vlastní akci:</span><span class="sxs-lookup"><span data-stu-id="4d13a-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

<span data-ttu-id="4d13a-108">Teď můžete používat pouze ověření uživatelé `Logout` funkce.</span><span class="sxs-lookup"><span data-stu-id="4d13a-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="4d13a-109">Můžete také `AllowAnonymous` atribut pro povolení přístupu podle neověřené uživatele k jednotlivým akcím.</span><span class="sxs-lookup"><span data-stu-id="4d13a-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="4d13a-110">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4d13a-110">For example:</span></span>

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="4d13a-111">To by umožnilo pouze ověřeným uživatelům `AccountController`, s výjimkou `Login` akce, které je přístupné všem uživatelům, bez ohledu na jejich stav ověřené nebo neověřené / anonymní.</span><span class="sxs-lookup"><span data-stu-id="4d13a-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="4d13a-112">`[AllowAnonymous]` obchází všechny příkazy autorizace.</span><span class="sxs-lookup"><span data-stu-id="4d13a-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="4d13a-113">Pokud kombinujete `[AllowAnonymous]` a jakýkoli `[Authorize]` atribut, `[Authorize]` atributy jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="4d13a-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="4d13a-114">Například pokud použijete `[AllowAnonymous]` na úrovni kontroleru, všechny `[Authorize]` atributy ve stejném řadiči (nebo na všechny akce v něm) je ignorována.</span><span class="sxs-lookup"><span data-stu-id="4d13a-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
