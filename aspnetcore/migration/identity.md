---
title: "Migrace ověřování a identita na jádro ASP.NET"
author: ardalis
description: "Zjistěte, jak migrovat do projektu ASP.NET MVC základní ověřování a identita z projektu aplikace ASP.NET MVC."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: bf452ad3969863f8f058b29a31f19af13cb2fc6b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="migrating-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="0eb93-103">Migrace ověřování a identita na jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0eb93-103">Migrating Authentication and Identity to ASP.NET Core</span></span>

<a name="migration-identity"></a>

<span data-ttu-id="0eb93-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0eb93-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0eb93-105">V předchozím článku jsme [migrovat konfigurace z projektu aplikace ASP.NET MVC rozhraní ASP.NET MVC základní](configuration.md).</span><span class="sxs-lookup"><span data-stu-id="0eb93-105">In the previous article we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](configuration.md).</span></span> <span data-ttu-id="0eb93-106">V tomto článku jsme migrovat funkce správy registrace, přihlášení a uživatele.</span><span class="sxs-lookup"><span data-stu-id="0eb93-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="0eb93-107">Nakonfigurujte identitu a členství</span><span class="sxs-lookup"><span data-stu-id="0eb93-107">Configure Identity and Membership</span></span>

<span data-ttu-id="0eb93-108">Ověřování a identita funkcí v architektuře ASP.NET MVC je nakonfigurována pomocí ASP.NET Identity v Startup.Auth.cs a IdentityConfig.cs, umístěný ve složce App_Start.</span><span class="sxs-lookup"><span data-stu-id="0eb93-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in Startup.Auth.cs and IdentityConfig.cs, located in the App_Start folder.</span></span> <span data-ttu-id="0eb93-109">V aplikaci ASP.NET MVC jádra, tyto funkce jsou nakonfigurované v *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="0eb93-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="0eb93-110">Nainstalujte `Microsoft.AspNetCore.Identity.EntityFrameworkCore` a `Microsoft.AspNetCore.Authentication.Cookies` balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="0eb93-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="0eb93-111">Potom otevřete Startup.cs a aktualizací `ConfigureServices()` metoda použít službu pro rozhraní Entity Framework a Identity:</span><span class="sxs-lookup"><span data-stu-id="0eb93-111">Then, open Startup.cs and update the `ConfigureServices()` method to use Entity Framework and Identity services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

<span data-ttu-id="0eb93-112">V tomto okamžiku existují dva typy odkazovaný ve výše uvedený kód, který jsme nebyly migrovány z projektu ASP.NET MVC: `ApplicationDbContext` a `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="0eb93-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="0eb93-113">Vytvořte novou *modely* složky v ASP.NET Core projektu a přidejte do ní odpovídající tyto typy dvou tříd.</span><span class="sxs-lookup"><span data-stu-id="0eb93-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="0eb93-114">Zjistíte, ASP.NET MVC verzích tyto třídy ve `/Models/IdentityModels.cs`, ale budeme používat jeden soubor pro třídy migrované projektu, protože se trochu objasnit.</span><span class="sxs-lookup"><span data-stu-id="0eb93-114">You will find the ASP.NET MVC versions of these classes in `/Models/IdentityModels.cs`, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="0eb93-115">ApplicationUser.cs:</span><span class="sxs-lookup"><span data-stu-id="0eb93-115">ApplicationUser.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="0eb93-116">ApplicationDbContext.cs:</span><span class="sxs-lookup"><span data-stu-id="0eb93-116">ApplicationDbContext.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

<span data-ttu-id="0eb93-117">ASP.NET Core MVC Starter webového projektu neobsahuje mnohem přizpůsobení uživatelů nebo ApplicationDbContext.</span><span class="sxs-lookup"><span data-stu-id="0eb93-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the ApplicationDbContext.</span></span> <span data-ttu-id="0eb93-118">Při migraci reálné aplikaci, budete také muset migrovat všechny vlastní vlastnosti a metody uživatelů vaší aplikace a třídy DbContext, jakož i další, které vaše aplikace využívá (například pokud má vaše DbContext DbSettřídymodelu<Album>, samozřejmě je nutné migrovat třídě alb).</span><span class="sxs-lookup"><span data-stu-id="0eb93-118">When migrating a real application, you will also need to migrate all of the custom properties and methods of your application's user and DbContext classes, as well as any other Model classes your application utilizes (for example, if your DbContext has a DbSet<Album>, you will of course need to migrate the Album class).</span></span>

<span data-ttu-id="0eb93-119">S těmito soubory na místě, můžete provést souboru Startup.cs zkompilovat aktualizací jeho pomocí příkazů:</span><span class="sxs-lookup"><span data-stu-id="0eb93-119">With these files in place, the Startup.cs file can be made to compile by updating its using statements:</span></span>

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

<span data-ttu-id="0eb93-120">Naše aplikace je teď připravený na podporu ověřování a identita služby – pouze musí mít tyto funkce dostupná uživatelům.</span><span class="sxs-lookup"><span data-stu-id="0eb93-120">Our application is now ready to support authentication and identity services - it just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="0eb93-121">Migrace registrace a přihlášení logiky</span><span class="sxs-lookup"><span data-stu-id="0eb93-121">Migrate Registration and Login Logic</span></span>

<span data-ttu-id="0eb93-122">Pomocí identity služby nakonfigurovat pro aplikaci a nakonfigurovat pomocí rozhraní Entity Framework a SQL Server přístup k datům jsme jste připraveni přidat podporu registrace a přihlášení do aplikace.</span><span class="sxs-lookup"><span data-stu-id="0eb93-122">With identity services configured for the application and data access configured using Entity Framework and SQL Server, we are now ready to add support for registration and login to the application.</span></span> <span data-ttu-id="0eb93-123">Odvolat, [výše v procesu migrace](mvc.md#migrate-layout-file) jsme označeno jako komentář odkaz na _LoginPartial v _Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="0eb93-123">Recall that [earlier in the migration process](mvc.md#migrate-layout-file) we commented out a reference to _LoginPartial in _Layout.cshtml.</span></span> <span data-ttu-id="0eb93-124">Nyní je čas vrátit kódu, Odkomentujte ho a přidejte v zobrazení pro podporu funkcí přihlášení a potřeby řadiče.</span><span class="sxs-lookup"><span data-stu-id="0eb93-124">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="0eb93-125">Aktualizace _Layout.cshtml; Zrušením komentáře u @Html.Partial řádku:</span><span class="sxs-lookup"><span data-stu-id="0eb93-125">Update _Layout.cshtml; uncomment the @Html.Partial line:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="0eb93-126">Nyní přidejte novou stránku zobrazení MVC s názvem _LoginPartial do zobrazení nebo sdílené složky:</span><span class="sxs-lookup"><span data-stu-id="0eb93-126">Now, add a new MVC View Page called _LoginPartial to the Views/Shared folder:</span></span>

<span data-ttu-id="0eb93-127">Aktualizovat _LoginPartial.cshtml následujícím kódem (Nahraďte veškerý jeho obsah):</span><span class="sxs-lookup"><span data-stu-id="0eb93-127">Update _LoginPartial.cshtml with the following code (replace all of its contents):</span></span>

```cshtml
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

<span data-ttu-id="0eb93-128">V tomto okamžiku byste měli být schopni obnovit stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="0eb93-128">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="0eb93-129">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0eb93-129">Summary</span></span>

<span data-ttu-id="0eb93-130">ASP.NET Core zavádí změny funkce ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="0eb93-130">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="0eb93-131">V tomto článku jste viděli, jak migrovat funkcím pro správu ověřování a uživatel ASP.NET identity do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0eb93-131">In this article, you have seen how to migrate the authentication and user management features of an ASP.NET Identity to ASP.NET Core.</span></span>
