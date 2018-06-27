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

> [!WARNING]
> `[AllowAnonymous]` obchází všechny příkazy autorizace. Pokud kombinujete `[AllowAnonymous]` a jakýkoli `[Authorize]` atribut, `[Authorize]` atributy jsou ignorovány. Například pokud použijete `[AllowAnonymous]` na úrovni kontroleru, všechny `[Authorize]` atributy ve stejném řadiči (nebo na všechny akce v něm) je ignorována.
