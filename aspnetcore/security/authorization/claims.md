---
title: Na základě deklarace autorizace v ASP.NET Core
author: rick-anderson
description: Informace o postupu přidání deklarace identity kontroluje autorizace v aplikaci ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/claims
ms.openlocfilehash: 2464f8cac720dcf5de02f2679e9450e8b77de3ee
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/20/2018
---
# <a name="claims-based-authorization-in-aspnet-core"></a>Na základě deklarace autorizace v ASP.NET Core

<a name="security-authorization-claims-based"></a>

Při vytvoření identity může přiřadit jeden nebo více deklarace identity vystaveny důvěryhodná strana. Deklarace identity je, že je dvojice název-hodnota reprezentující jaké předmět, můžete provést není co předmět. Například můžete mít licenci ovladače, vydaný místní řízení autoritou licence. Bance má vaše datum narození na. V takovém případě by být název deklarací `DateOfBirth`, hodnoty deklarace identity by vaše datum narození, například `8th June 1970` a vystavitele bude podporovat jeho autority licence. Autorizaci deklarací identity na základě, v nejjednodušším kontroluje hodnotu deklarace identity a umožňuje přístup k prostředku na základě této hodnotě. Pokud chcete přístup k křížovou kartou noci proces autorizace. například může být:

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

### <a name="add-a-generic-claim-check"></a>Přidání deklarace identity obecné kontroly

Pokud není hodnota deklarace identity jednu hodnotu nebo transformace se vyžaduje, použijte [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion). Další informace najdete v tématu [pomocí funkci func ke splnění zásadu](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).

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

Pokud chcete složitější zásady, jako je například trvá datum narození deklarace identity, Výpočet stáří z něho pak kontrola stáří je 21 nebo starší, pak budete muset napsat [obslužné rutiny vlastní zásady](xref:security/authorization/policies).
