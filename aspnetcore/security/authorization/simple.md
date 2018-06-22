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
# <a name="simple-authorization-in-aspnet-core"></a>Jednoduché autorizace v ASP.NET Core

<a name="security-authorization-simple"></a>

Autorizace v MVC je řízen pomocí `AuthorizeAttribute` atribut a jeho různé parametry. V nejjednodušším, použití `AuthorizeAttribute` atribut kontroler nebo akce omezíte přístup k kontroler nebo akce, které všem ověřeným uživatelům.

Například následující kód omezuje přístup na `AccountController` všem ověřeným uživatelům.

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

Pokud chcete se autorizace provádí akce, nikoli na řadič, použít `AuthorizeAttribute` atribut vlastní akci:

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

Teď můžete používat pouze ověření uživatelé `Logout` funkce.

Můžete také `AllowAnonymous` atribut pro povolení přístupu podle neověřené uživatele k jednotlivým akcím. Příklad:

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

To by umožnilo pouze ověřeným uživatelům `AccountController`, s výjimkou `Login` akce, které je přístupné všem uživatelům, bez ohledu na jejich stav ověřené nebo neověřené / anonymní.

>[!WARNING]
> `[AllowAnonymous]` obchází všechny příkazy autorizace. Pokud použijete zkombinujte `[AllowAnonymous]` a jakýkoli `[Authorize]` atribut pak autorizovat atributy budou vždy ignorovány. Například pokud použijete `[AllowAnonymous]` řadiči úrovně žádné `[Authorize]` atributy na stejného řadiče nebo na všechny akce v něm budou ignorovány.
