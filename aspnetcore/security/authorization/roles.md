---
title: "Autorizace podle rolí"
author: rick-anderson
description: "Tento dokument ukazuje, jak omezit přístup kontroleru a akce ASP.NET Core předáním rolí do atribut Authorize."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/roles
ms.openlocfilehash: 764d1fcc384fc8370d1a536f9609333de6bd4357
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="role-based-authorization"></a><span data-ttu-id="ec8de-103">Autorizace podle rolí</span><span class="sxs-lookup"><span data-stu-id="ec8de-103">Role based Authorization</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="ec8de-104">Při vytvoření identity může patřit do jedné nebo více rolí.</span><span class="sxs-lookup"><span data-stu-id="ec8de-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="ec8de-105">Například Tracy může patřit do role správce a uživatele, a přitom Scott lze přiřadit pouze k roli uživatele.</span><span class="sxs-lookup"><span data-stu-id="ec8de-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="ec8de-106">Jak jsou tyto role vytvořit a spravovat závisí na základní úložiště proces autorizace.</span><span class="sxs-lookup"><span data-stu-id="ec8de-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="ec8de-107">Role jsou viditelné na vývojáře prostřednictvím [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) metodu [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) třídy.</span><span class="sxs-lookup"><span data-stu-id="ec8de-107">Roles are exposed to the developer through the [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="ec8de-108">Přidání role kontroly</span><span class="sxs-lookup"><span data-stu-id="ec8de-108">Adding role checks</span></span>

<span data-ttu-id="ec8de-109">Kontroly autorizace na základě rolí jsou deklarativní&mdash;vývojář vloží je v rámci svůj kód proti kontroler nebo akce v kontroleru, určení rolí, které má aktuální uživatel musí být členem skupiny k přístupu k požadovanému zdroji.</span><span class="sxs-lookup"><span data-stu-id="ec8de-109">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="ec8de-110">Například následující kód omezuje přístup na všechny akce na `AdministrationController` uživatelům, kteří jsou členy `Administrator` role:</span><span class="sxs-lookup"><span data-stu-id="ec8de-110">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="ec8de-111">Více rolí můžete zadat jako seznam oddělený čárkami:</span><span class="sxs-lookup"><span data-stu-id="ec8de-111">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="ec8de-112">Tento řadič pouze bude přístupný pro uživatele, kteří jsou členy z `HRManager` role nebo `Finance` role.</span><span class="sxs-lookup"><span data-stu-id="ec8de-112">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="ec8de-113">Pokud použijete více atributů uživatele s přístupem musí být členem všech rolí zadaný; Následující příklad vyžaduje, že uživatel musí být členem skupiny i `PowerUser` a `ControlPanelUser` role.</span><span class="sxs-lookup"><span data-stu-id="ec8de-113">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="ec8de-114">Můžete dále omezit přístup s použitím atributů autorizace další role na úrovni akce:</span><span class="sxs-lookup"><span data-stu-id="ec8de-114">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

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

<span data-ttu-id="ec8de-115">V předchozí kód fragment kódu členů `Administrator` role nebo `PowerUser` role přístup řadičem a `SetTime` akce, ale pouze členové `Administrator` role přístup `ShutDown` akce.</span><span class="sxs-lookup"><span data-stu-id="ec8de-115">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="ec8de-116">Můžete taky zablokovat řadič, ale povolit neověřená anonymní přístup k jednotlivým akcím.</span><span class="sxs-lookup"><span data-stu-id="ec8de-116">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

## <a name="policy-based-role-checks"></a><span data-ttu-id="ec8de-117">Kontroly rolí na základě zásad</span><span class="sxs-lookup"><span data-stu-id="ec8de-117">Policy based role checks</span></span>

<span data-ttu-id="ec8de-118">Požadavky na role může být vyjádřený také pomocí nové zásady syntaxe, kde vývojář zaregistruje zásady při spuštění jako součást konfigurace služby autorizace.</span><span class="sxs-lookup"><span data-stu-id="ec8de-118">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="ec8de-119">K tomu obvykle dochází v `ConfigureServices()` ve vaší *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="ec8de-119">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="ec8de-120">Zásady se použijí, pomocí `Policy` vlastnost na `AuthorizeAttribute` atribut:</span><span class="sxs-lookup"><span data-stu-id="ec8de-120">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="ec8de-121">Pokud chcete zadat víc povolených rolí v požadavek, pak je můžete zadat jako parametry, které `RequireRole` metoda:</span><span class="sxs-lookup"><span data-stu-id="ec8de-121">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="ec8de-122">Tento příklad autorizuje uživatele, kteří patří do `Administrator`, `PowerUser` nebo `BackupAdministrator` role.</span><span class="sxs-lookup"><span data-stu-id="ec8de-122">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
