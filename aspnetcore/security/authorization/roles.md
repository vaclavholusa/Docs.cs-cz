---
title: "Autorizace podle rolí"
author: rick-anderson
description: "Tento dokument ukazuje, jak omezit přístup kontroleru a akce ASP.NET Core předáním rolí do atribut Authorize."
keywords: "ASP.NET Core, autorizace, rolí"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5e014da1-8bc0-409b-951a-88b92c661fdf
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/roles
ms.openlocfilehash: 649b21d99c742843534748b0ba9d7b7b22483a62
ms.sourcegitcommit: 703593d5fd14076e79be2ba75a5b8da12a60ab15
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="role-based-authorization"></a><span data-ttu-id="19f05-104">Autorizace podle rolí</span><span class="sxs-lookup"><span data-stu-id="19f05-104">Role based Authorization</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="19f05-105">Při vytvoření identity může patřit do jedné nebo více rolí.</span><span class="sxs-lookup"><span data-stu-id="19f05-105">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="19f05-106">Například Tracy může patřit do role správce a uživatele, a přitom Scott lze přiřadit pouze k roli uživatele.</span><span class="sxs-lookup"><span data-stu-id="19f05-106">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="19f05-107">Jak jsou tyto role vytvořit a spravovat závisí na základní úložiště proces autorizace.</span><span class="sxs-lookup"><span data-stu-id="19f05-107">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="19f05-108">Role jsou viditelné na vývojáře prostřednictvím [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) metodu [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) třídy.</span><span class="sxs-lookup"><span data-stu-id="19f05-108">Roles are exposed to the developer through the [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="19f05-109">Přidání role kontroly</span><span class="sxs-lookup"><span data-stu-id="19f05-109">Adding role checks</span></span>

<span data-ttu-id="19f05-110">Kontroly autorizace na základě rolí jsou deklarativní&mdash;vývojář vloží je v rámci svůj kód proti kontroler nebo akce v kontroleru, určení rolí, které má aktuální uživatel musí být členem skupiny k přístupu k požadovanému zdroji.</span><span class="sxs-lookup"><span data-stu-id="19f05-110">Role based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="19f05-111">Například následující kód omezí přístup pro všechny akce na `AdministrationController` uživatelům, kteří jsou členy `Administrator` skupiny.</span><span class="sxs-lookup"><span data-stu-id="19f05-111">For example, the following code would limit access to any actions on the `AdministrationController` to users who are a member of the `Administrator` group.</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="19f05-112">Více rolí můžete zadat jako seznam oddělený čárkami:</span><span class="sxs-lookup"><span data-stu-id="19f05-112">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="19f05-113">Tento řadič pouze bude přístupný pro uživatele, kteří jsou členy z `HRManager` role nebo `Finance` role.</span><span class="sxs-lookup"><span data-stu-id="19f05-113">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="19f05-114">Pokud použijete více atributů uživatele s přístupem musí být členem všech rolí zadaný; Následující příklad vyžaduje, že uživatel musí být členem skupiny i `PowerUser` a `ControlPanelUser` role.</span><span class="sxs-lookup"><span data-stu-id="19f05-114">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="19f05-115">Můžete dále omezit přístup s použitím atributů autorizace další role na úrovni akce:</span><span class="sxs-lookup"><span data-stu-id="19f05-115">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

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

<span data-ttu-id="19f05-116">V předchozí kód fragment kódu členů `Administrator` role nebo `PowerUser` role přístup řadičem a `SetTime` akce, ale pouze členové `Administrator` role přístup `ShutDown` akce.</span><span class="sxs-lookup"><span data-stu-id="19f05-116">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="19f05-117">Můžete taky zablokovat řadič, ale povolit neověřená anonymní přístup k jednotlivým akcím.</span><span class="sxs-lookup"><span data-stu-id="19f05-117">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

## <a name="policy-based-role-checks"></a><span data-ttu-id="19f05-118">Kontroly rolí na základě zásad</span><span class="sxs-lookup"><span data-stu-id="19f05-118">Policy based role checks</span></span>

<span data-ttu-id="19f05-119">Požadavky na role může být vyjádřený také pomocí nové zásady syntaxe, kde vývojář zaregistruje zásady při spuštění jako součást konfigurace služby autorizace.</span><span class="sxs-lookup"><span data-stu-id="19f05-119">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="19f05-120">K tomu obvykle dochází v `ConfigureServices()` ve vaší *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="19f05-120">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="19f05-121">Zásady se použijí, pomocí `Policy` vlastnost na `AuthorizeAttribute` atribut:</span><span class="sxs-lookup"><span data-stu-id="19f05-121">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="19f05-122">Pokud chcete zadat víc povolených rolí v požadavek, pak je můžete zadat jako parametry, které `RequireRole` metoda:</span><span class="sxs-lookup"><span data-stu-id="19f05-122">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="19f05-123">Tento příklad autorizuje uživatele, kteří patří do `Administrator`, `PowerUser` nebo `BackupAdministrator` role.</span><span class="sxs-lookup"><span data-stu-id="19f05-123">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
