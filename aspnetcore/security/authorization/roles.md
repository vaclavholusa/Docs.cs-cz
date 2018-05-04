---
title: Autorizace na základě rolí v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak omezit přístup kontroleru a akce ASP.NET Core předáním rolí do atribut Authorize.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/roles
ms.openlocfilehash: f1e7209cae1e2a58ad536547d655dd744ca0d3f7
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="role-based-authorization-in-aspnet-core"></a>Autorizace na základě rolí v ASP.NET Core

<a name="security-authorization-role-based"></a>

Při vytvoření identity může patřit do jedné nebo více rolí. Například Tracy může patřit do role správce a uživatele, a přitom Scott lze přiřadit pouze k roli uživatele. Jak jsou tyto role vytvořit a spravovat závisí na základní úložiště proces autorizace. Role jsou viditelné na vývojáře prostřednictvím [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) metodu [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) třídy.

## <a name="adding-role-checks"></a>Přidání role kontroly

Kontroly autorizace na základě rolí jsou deklarativní&mdash;vývojář vloží je v rámci svůj kód proti kontroler nebo akce v kontroleru, určení rolí, které má aktuální uživatel musí být členem skupiny k přístupu k požadovanému zdroji.

Například následující kód omezuje přístup na všechny akce na `AdministrationController` uživatelům, kteří jsou členy `Administrator` role:

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

Více rolí můžete zadat jako seznam oddělený čárkami:

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

Tento řadič pouze bude přístupný pro uživatele, kteří jsou členy z `HRManager` role nebo `Finance` role.

Pokud použijete více atributů uživatele s přístupem musí být členem všech rolí zadaný; Následující příklad vyžaduje, že uživatel musí být členem skupiny i `PowerUser` a `ControlPanelUser` role.

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

Můžete dále omezit přístup s použitím atributů autorizace další role na úrovni akce:

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

V předchozí kód fragment kódu členů `Administrator` role nebo `PowerUser` role přístup řadičem a `SetTime` akce, ale pouze členové `Administrator` role přístup `ShutDown` akce.

Můžete taky zablokovat řadič, ale povolit neověřená anonymní přístup k jednotlivým akcím.

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a>Kontroly rolí na základě zásad

Požadavky na role může být vyjádřený také pomocí nové zásady syntaxe, kde vývojář zaregistruje zásady při spuštění jako součást konfigurace služby autorizace. K tomu obvykle dochází v `ConfigureServices()` ve vaší *Startup.cs* souboru.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole", policy => policy.RequireRole("Administrator"));
    });
}
```

Zásady se použijí, pomocí `Policy` vlastnost na `AuthorizeAttribute` atribut:

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

Pokud chcete zadat víc povolených rolí v požadavek, pak je můžete zadat jako parametry, které `RequireRole` metoda:

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

Tento příklad autorizuje uživatele, kteří patří do `Administrator`, `PowerUser` nebo `BackupAdministrator` role.
