---
title: "Ověření na základě deklarace identity"
author: rick-anderson
description: "Tento dokument vysvětluje, jak přidat kontroluje deklarací autorizace v aplikaci ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/claims
ms.openlocfilehash: 870bdf79abc2c94745ab5da6997a37ed0e4ea4e2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="claims-based-authorization"></a>Ověření na základě deklarace identity

<a name="security-authorization-claims-based"></a>

Při vytvoření identity může přiřadit jeden nebo více deklarace identity vystaveny důvěryhodná strana. Deklarace identity je název hodnota je pár, který představuje jaké předmět, není co předmět můžete provést. Například můžete mít licenci ovladače, vydaný místní řízení autoritou licence. Bance má vaše datum narození na. V takovém případě by být název deklarací `DateOfBirth`, hodnoty deklarace identity by vaše datum narození, například `8th June 1970` a vystavitele bude podporovat jeho autority licence. Autorizaci deklarací identity na základě, v nejjednodušším kontroluje hodnotu deklarace identity a umožňuje přístup k prostředku na základě této hodnotě. Pokud chcete přístup k křížovou kartou noci proces autorizace. například může být:

Úředník zabezpečení dveře by vyhodnocovaly hodnotu vaše datum narození deklarace identity a jestli důvěřují vystavitele (řízení oprávnění licencí) před udělením přístupu.

Identity může obsahovat více deklarací identity s více hodnotami a může obsahovat více deklarací identity stejného typu.

## <a name="adding-claims-checks"></a>Přidání deklarace identity kontroly

Deklarace identity jsou deklarativní kontroly autorizace na základě – vývojář vloží je v rámci svůj kód proti kontroler nebo akce v kontroleru, deklarace identity, které musí mít aktuální uživatel a volitelně musí obsahovat hodnotu deklarace identity pro přístup k určení požadovaný prostředek. Deklarace identity, že jsou požadavky na základě zásad a vývojář musí sestavení a zaregistrujte zásadu vyjadřující požadavky deklarací identity.

Nejjednodušší typ deklarace identity zásad hledá přítomnost deklarace identity a neohlásí hodnota.

Nejdřív je potřeba vytvořit a zaregistrovat zásady. To probíhá v rámci konfigurace služby ověřování, což obvykle trvá část v `ConfigureServices()` ve vaší *Startup.cs* souboru.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

V takovém případě `EmployeeOnly` zásada kontroluje přítomnost `EmployeeNumber` deklarace identity na aktuální identitu.

Pak aplikovat zásady použití `Policy` vlastnost `AuthorizeAttribute` atribut zadat název zásad;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

`AuthorizeAttribute` Atribut lze použít k celé řadiči, v této instanci jen identity odpovídající zásady budou mít povolený přístup k žádné akci na řadiči.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Pokud se jedná o zařízení, který je chráněn `AuthorizeAttribute` atribut, ale chcete povolit anonymní přístup k určité akce, které použijete `AllowAnonymousAttribute` atribut.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

Většina deklarace identity se dodávají s hodnotou. Při vytváření zásady, můžete zadat seznam povolených hodnot. V následujícím příkladu by pouze úspěšné u zaměstnanců, jejichž číslo zaměstnance byl 1, 2, 3, 4 nebo 5.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

## <a name="multiple-policy-evaluation"></a>Více hodnocení zásad

Pokud použijete různé zásady pro kontroler nebo akce, musí splnit všechny zásady, před udělením přístupu. Příklad:

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

V předchozím příkladu všechny identity, které splní `EmployeeOnly` přístup zásad `Payslip` akci, jako tyto zásady se vynucuje na řadiči. Ale k volání `UpdateSalary` akce musíte splnit identitu *i* `EmployeeOnly` zásad a `HumanResources` zásad.

Pokud chcete složitější zásady, jako je například trvá datum narození deklarace identity, Výpočet stáří z něho pak kontrola stáří je 21 nebo starší, pak budete muset napsat [obslužné rutiny vlastní zásady](policies.md).
