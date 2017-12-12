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
# <a name="simple-authorization"></a>Jednoduché autorizace

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

Můžete také `AllowAnonymousAttribute` atribut pro povolení přístupu podle neověřené uživatele k jednotlivým akcím. Příklad:

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
> `[AllowAnonymous]`obchází všechny příkazy autorizace. Pokud použijete zkombinujte `[AllowAnonymous]` a jakýkoli `[Authorize]` atribut pak autorizovat atributy budou vždy ignorovány. Například pokud použijete `[AllowAnonymous]` řadiči úrovně žádné `[Authorize]` atributy na stejného řadiče nebo na všechny akce v něm budou ignorovány.
