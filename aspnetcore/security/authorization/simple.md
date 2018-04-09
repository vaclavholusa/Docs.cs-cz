---
title: Jednoduché autorizace v ASP.NET Core
author: rick-anderson
description: Naučte se používat atribut autorizovat k omezení přístupu k ASP.NET Core kontrolery a akce.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/simple
ms.openlocfilehash: cef5cb146c6c1ff052430748a9a64c6a822d6fa3
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="4ceab-103">Jednoduché autorizace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4ceab-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="4ceab-104">Autorizace v MVC je řízen pomocí `AuthorizeAttribute` atribut a jeho různé parametry.</span><span class="sxs-lookup"><span data-stu-id="4ceab-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="4ceab-105">V nejjednodušším, použití `AuthorizeAttribute` atribut kontroler nebo akce omezíte přístup k kontroler nebo akce, které všem ověřeným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="4ceab-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="4ceab-106">Například následující kód omezuje přístup na `AccountController` všem ověřeným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="4ceab-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="4ceab-107">Pokud chcete se autorizace provádí akce, nikoli na řadič, použít `AuthorizeAttribute` atribut vlastní akci:</span><span class="sxs-lookup"><span data-stu-id="4ceab-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="4ceab-108">Teď můžete používat pouze ověření uživatelé `Logout` funkce.</span><span class="sxs-lookup"><span data-stu-id="4ceab-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="4ceab-109">Můžete také `AllowAnonymous` atribut pro povolení přístupu podle neověřené uživatele k jednotlivým akcím.</span><span class="sxs-lookup"><span data-stu-id="4ceab-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="4ceab-110">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4ceab-110">For example:</span></span>

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

<span data-ttu-id="4ceab-111">To by umožnilo pouze ověřeným uživatelům `AccountController`, s výjimkou `Login` akce, které je přístupné všem uživatelům, bez ohledu na jejich stav ověřené nebo neověřené / anonymní.</span><span class="sxs-lookup"><span data-stu-id="4ceab-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="4ceab-112">`[AllowAnonymous]` obchází všechny příkazy autorizace.</span><span class="sxs-lookup"><span data-stu-id="4ceab-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="4ceab-113">Pokud použijete zkombinujte `[AllowAnonymous]` a jakýkoli `[Authorize]` atribut pak autorizovat atributy budou vždy ignorovány.</span><span class="sxs-lookup"><span data-stu-id="4ceab-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="4ceab-114">Například pokud použijete `[AllowAnonymous]` řadiči úrovně žádné `[Authorize]` atributy na stejného řadiče nebo na všechny akce v něm budou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="4ceab-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
