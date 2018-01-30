---
title: "Ověření na základě deklarace identity"
author: rick-anderson
description: "Tento dokument vysvětluje, jak přidat kontroluje deklarací autorizace v aplikaci ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/claims
ms.openlocfilehash: 76b6566df4a427836eb5060f7d80e1039e479884
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="claims-based-authorization"></a><span data-ttu-id="6b34c-103">Ověření na základě deklarace identity</span><span class="sxs-lookup"><span data-stu-id="6b34c-103">Claims-Based Authorization</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="6b34c-104">Při vytvoření identity může přiřadit jeden nebo více deklarace identity vystaveny důvěryhodná strana.</span><span class="sxs-lookup"><span data-stu-id="6b34c-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="6b34c-105">Deklarace identity je název hodnota je pár, který představuje jaké předmět, není co předmět můžete provést.</span><span class="sxs-lookup"><span data-stu-id="6b34c-105">A claim is name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="6b34c-106">Například můžete mít licenci ovladače, vydaný místní řízení autoritou licence.</span><span class="sxs-lookup"><span data-stu-id="6b34c-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="6b34c-107">Bance má vaše datum narození na.</span><span class="sxs-lookup"><span data-stu-id="6b34c-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="6b34c-108">V takovém případě by být název deklarací `DateOfBirth`, hodnoty deklarace identity by vaše datum narození, například `8th June 1970` a vystavitele bude podporovat jeho autority licence.</span><span class="sxs-lookup"><span data-stu-id="6b34c-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="6b34c-109">Autorizaci deklarací identity na základě, v nejjednodušším kontroluje hodnotu deklarace identity a umožňuje přístup k prostředku na základě této hodnotě.</span><span class="sxs-lookup"><span data-stu-id="6b34c-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="6b34c-110">Pokud chcete přístup k křížovou kartou noci proces autorizace. například může být:</span><span class="sxs-lookup"><span data-stu-id="6b34c-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="6b34c-111">Úředník zabezpečení dveře by vyhodnocovaly hodnotu vaše datum narození deklarace identity a jestli důvěřují vystavitele (řízení oprávnění licencí) před udělením přístupu.</span><span class="sxs-lookup"><span data-stu-id="6b34c-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="6b34c-112">Identity může obsahovat více deklarací identity s více hodnotami a může obsahovat více deklarací identity stejného typu.</span><span class="sxs-lookup"><span data-stu-id="6b34c-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="6b34c-113">Přidání deklarace identity kontroly</span><span class="sxs-lookup"><span data-stu-id="6b34c-113">Adding claims checks</span></span>

<span data-ttu-id="6b34c-114">Deklarace identity jsou deklarativní kontroly autorizace na základě – vývojář vloží je v rámci svůj kód proti kontroler nebo akce v kontroleru, deklarace identity, které musí mít aktuální uživatel a volitelně musí obsahovat hodnotu deklarace identity pro přístup k určení požadovaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="6b34c-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="6b34c-115">Deklarace identity, že jsou požadavky na základě zásad a vývojář musí sestavení a zaregistrujte zásadu vyjadřující požadavky deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="6b34c-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="6b34c-116">Nejjednodušší typ deklarace identity zásad hledá přítomnost deklarace identity a neohlásí hodnota.</span><span class="sxs-lookup"><span data-stu-id="6b34c-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="6b34c-117">Nejdřív je potřeba vytvořit a zaregistrovat zásady.</span><span class="sxs-lookup"><span data-stu-id="6b34c-117">First you need to build and register the policy.</span></span> <span data-ttu-id="6b34c-118">To probíhá v rámci konfigurace služby ověřování, což obvykle trvá část v `ConfigureServices()` ve vaší *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="6b34c-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="6b34c-119">V takovém případě `EmployeeOnly` zásada kontroluje přítomnost `EmployeeNumber` deklarace identity na aktuální identitu.</span><span class="sxs-lookup"><span data-stu-id="6b34c-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="6b34c-120">Pak aplikovat zásady použití `Policy` vlastnost `AuthorizeAttribute` atribut zadat název zásad;</span><span class="sxs-lookup"><span data-stu-id="6b34c-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="6b34c-121">`AuthorizeAttribute` Atribut lze použít k celé řadiči, v této instanci jen identity odpovídající zásady budou mít povolený přístup k žádné akci na řadiči.</span><span class="sxs-lookup"><span data-stu-id="6b34c-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="6b34c-122">Pokud se jedná o zařízení, který je chráněn `AuthorizeAttribute` atribut, ale chcete povolit anonymní přístup k určité akce, které použijete `AllowAnonymousAttribute` atribut.</span><span class="sxs-lookup"><span data-stu-id="6b34c-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

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

<span data-ttu-id="6b34c-123">Většina deklarace identity se dodávají s hodnotou.</span><span class="sxs-lookup"><span data-stu-id="6b34c-123">Most claims come with a value.</span></span> <span data-ttu-id="6b34c-124">Při vytváření zásady, můžete zadat seznam povolených hodnot.</span><span class="sxs-lookup"><span data-stu-id="6b34c-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="6b34c-125">V následujícím příkladu by pouze úspěšné u zaměstnanců, jejichž číslo zaměstnance byl 1, 2, 3, 4 nebo 5.</span><span class="sxs-lookup"><span data-stu-id="6b34c-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

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

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="6b34c-126">Více hodnocení zásad</span><span class="sxs-lookup"><span data-stu-id="6b34c-126">Multiple Policy Evaluation</span></span>

<span data-ttu-id="6b34c-127">Pokud použijete různé zásady pro kontroler nebo akce, musí splnit všechny zásady, před udělením přístupu.</span><span class="sxs-lookup"><span data-stu-id="6b34c-127">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="6b34c-128">Příklad:</span><span class="sxs-lookup"><span data-stu-id="6b34c-128">For example:</span></span>

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

<span data-ttu-id="6b34c-129">V předchozím příkladu všechny identity, které splní `EmployeeOnly` přístup zásad `Payslip` akci, jako tyto zásady se vynucuje na řadiči.</span><span class="sxs-lookup"><span data-stu-id="6b34c-129">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="6b34c-130">Ale k volání `UpdateSalary` akce musíte splnit identitu *i* `EmployeeOnly` zásad a `HumanResources` zásad.</span><span class="sxs-lookup"><span data-stu-id="6b34c-130">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="6b34c-131">Pokud chcete složitější zásady, jako je například trvá datum narození deklarace identity, Výpočet stáří z něho pak kontrola stáří je 21 nebo starší, pak budete muset napsat [obslužné rutiny vlastní zásady](policies.md).</span><span class="sxs-lookup"><span data-stu-id="6b34c-131">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](policies.md).</span></span>
