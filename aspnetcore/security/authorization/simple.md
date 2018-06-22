---
title: Jednoduché autorizace v ASP.NET Core
author: rick-anderson
description: Naučte se používat atribut autorizovat k omezení přístupu k ASP.NET Core kontrolery a akce.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 3c5e9d5dfd65ded40c9828a666143c1868f5562f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272062"
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="64593-103">Jednoduché autorizace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64593-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="64593-104">Autorizace v MVC je řízen pomocí `AuthorizeAttribute` atribut a jeho různé parametry.</span><span class="sxs-lookup"><span data-stu-id="64593-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="64593-105">V nejjednodušším, použití `AuthorizeAttribute` atribut kontroler nebo akce omezíte přístup k kontroler nebo akce, které všem ověřeným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="64593-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="64593-106">Například následující kód omezuje přístup na `AccountController` všem ověřeným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="64593-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="64593-107">Pokud chcete se autorizace provádí akce, nikoli na řadič, použít `AuthorizeAttribute` atribut vlastní akci:</span><span class="sxs-lookup"><span data-stu-id="64593-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="64593-108">Teď můžete používat pouze ověření uživatelé `Logout` funkce.</span><span class="sxs-lookup"><span data-stu-id="64593-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="64593-109">Můžete také `AllowAnonymous` atribut pro povolení přístupu podle neověřené uživatele k jednotlivým akcím.</span><span class="sxs-lookup"><span data-stu-id="64593-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="64593-110">Příklad:</span><span class="sxs-lookup"><span data-stu-id="64593-110">For example:</span></span>

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

<span data-ttu-id="64593-111">To by umožnilo pouze ověřeným uživatelům `AccountController`, s výjimkou `Login` akce, které je přístupné všem uživatelům, bez ohledu na jejich stav ověřené nebo neověřené / anonymní.</span><span class="sxs-lookup"><span data-stu-id="64593-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="64593-112">`[AllowAnonymous]` obchází všechny příkazy autorizace.</span><span class="sxs-lookup"><span data-stu-id="64593-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="64593-113">Pokud použijete zkombinujte `[AllowAnonymous]` a jakýkoli `[Authorize]` atribut pak autorizovat atributy budou vždy ignorovány.</span><span class="sxs-lookup"><span data-stu-id="64593-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="64593-114">Například pokud použijete `[AllowAnonymous]` řadiči úrovně žádné `[Authorize]` atributy na stejného řadiče nebo na všechny akce v něm budou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="64593-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
