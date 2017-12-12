---
title: "Jednoduché autorizace"
author: rick-anderson
description: "Tento dokument vysvětluje, jak použít atribut autorizovat k omezení přístupu k ASP.NET Core kontrolery a akce."
keywords: "ASP.NET Core, autorizace, třídy AuthorizeAttribute"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 391bcaad-205f-43e4-badc-fa592d6f79f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: f2dad58ffa17259412077d31f512b561e79ac595
ms.sourcegitcommit: b38796ea3806bf39b89806adfa681b2a33762907
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="simple-authorization"></a><span data-ttu-id="1322a-104">Jednoduché autorizace</span><span class="sxs-lookup"><span data-stu-id="1322a-104">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="1322a-105">Autorizace v MVC je řízen pomocí `AuthorizeAttribute` atribut a jeho různé parametry.</span><span class="sxs-lookup"><span data-stu-id="1322a-105">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="1322a-106">V nejjednodušším, použití `AuthorizeAttribute` atribut kontroler nebo akce omezíte přístup k kontroler nebo akce, které všem ověřeným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="1322a-106">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="1322a-107">Například následující kód omezuje přístup na `AccountController` všem ověřeným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="1322a-107">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="1322a-108">Pokud chcete se autorizace provádí akce, nikoli na řadič, použít `AuthorizeAttribute` atribut vlastní akci:</span><span class="sxs-lookup"><span data-stu-id="1322a-108">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="1322a-109">Teď můžete používat pouze ověření uživatelé `Logout` funkce.</span><span class="sxs-lookup"><span data-stu-id="1322a-109">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="1322a-110">Můžete také `AllowAnonymousAttribute` atribut pro povolení přístupu podle neověřené uživatele k jednotlivým akcím.</span><span class="sxs-lookup"><span data-stu-id="1322a-110">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="1322a-111">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1322a-111">For example:</span></span>

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

<span data-ttu-id="1322a-112">To by umožnilo pouze ověřeným uživatelům `AccountController`, s výjimkou `Login` akce, které je přístupné všem uživatelům, bez ohledu na jejich stav ověřené nebo neověřené / anonymní.</span><span class="sxs-lookup"><span data-stu-id="1322a-112">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="1322a-113">`[AllowAnonymous]`obchází všechny příkazy autorizace.</span><span class="sxs-lookup"><span data-stu-id="1322a-113">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="1322a-114">Pokud použijete zkombinujte `[AllowAnonymous]` a jakýkoli `[Authorize]` atribut pak autorizovat atributy budou vždy ignorovány.</span><span class="sxs-lookup"><span data-stu-id="1322a-114">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="1322a-115">Například pokud použijete `[AllowAnonymous]` řadiči úrovně žádné `[Authorize]` atributy na stejného řadiče nebo na všechny akce v něm budou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="1322a-115">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
