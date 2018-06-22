---
title: Migrace ověřování a identita na jádro ASP.NET
author: ardalis
description: Zjistěte, jak migrovat do projektu ASP.NET MVC základní ověřování a identita z projektu aplikace ASP.NET MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: e05d72ca78c7b8191a47f78cda31ee40e04d0706
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275694"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="54b05-103">Migrace ověřování a identita na jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="54b05-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="54b05-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="54b05-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="54b05-105">V předchozím článku jsme [migrovat konfigurace z projektu aplikace ASP.NET MVC rozhraní ASP.NET MVC základní](xref:migration/configuration).</span><span class="sxs-lookup"><span data-stu-id="54b05-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="54b05-106">V tomto článku jsme migrovat funkce správy registrace, přihlášení a uživatele.</span><span class="sxs-lookup"><span data-stu-id="54b05-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="54b05-107">Nakonfigurujte identitu a členství</span><span class="sxs-lookup"><span data-stu-id="54b05-107">Configure Identity and Membership</span></span>

<span data-ttu-id="54b05-108">V architektuře ASP.NET MVC, ověřování a identita funkcí je nakonfigurována pomocí ASP.NET Identity v *Startup.Auth.cs* a *IdentityConfig.cs*, který je umístěn v *App_Start* složka.</span><span class="sxs-lookup"><span data-stu-id="54b05-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="54b05-109">V aplikaci ASP.NET MVC jádra, tyto funkce jsou nakonfigurované v *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="54b05-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="54b05-110">Nainstalujte `Microsoft.AspNetCore.Identity.EntityFrameworkCore` a `Microsoft.AspNetCore.Authentication.Cookies` balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="54b05-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="54b05-111">Potom otevřete *Startup.cs* a aktualizovat `Startup.ConfigureServices` metoda použít službu pro rozhraní Entity Framework a Identity:</span><span class="sxs-lookup"><span data-stu-id="54b05-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add EF services to the services container.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

     services.AddMvc();
}
```

<span data-ttu-id="54b05-112">V tomto okamžiku existují dva typy odkazovaný ve výše uvedený kód, který jsme nebyly migrovány z projektu ASP.NET MVC: `ApplicationDbContext` a `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="54b05-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="54b05-113">Vytvořte novou *modely* složky v ASP.NET Core projektu a přidejte do ní odpovídající tyto typy dvou tříd.</span><span class="sxs-lookup"><span data-stu-id="54b05-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="54b05-114">Zjistíte, ASP.NET MVC verzích tyto třídy ve */Models/IdentityModels.cs*, ale budeme používat jeden soubor pro třídy migrované projektu, protože se trochu objasnit.</span><span class="sxs-lookup"><span data-stu-id="54b05-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="54b05-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="54b05-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="54b05-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="54b05-116">*ApplicationDbContext.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvcProject.Models
{
    public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder builder)
        {
            base.OnModelCreating(builder);
            // Customize the ASP.NET Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

<span data-ttu-id="54b05-117">ASP.NET Core MVC Starter webového projektu neobsahuje mnohem přizpůsobení uživatelů, nebo `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="54b05-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="54b05-118">Při migraci skutečné aplikace, musíte taky migrovat všechny vlastní vlastnosti a metody uživatele vaší aplikace a `DbContext` třídy a také další třídy modelu, vaše aplikace využívá.</span><span class="sxs-lookup"><span data-stu-id="54b05-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="54b05-119">Například pokud vaše `DbContext` má `DbSet<Album>`, budete muset migrovat `Album` třídy.</span><span class="sxs-lookup"><span data-stu-id="54b05-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="54b05-120">S těmito soubory na místě *Startup.cs* souboru můžete provedeny zkompilovat aktualizací jeho `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="54b05-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="54b05-121">Naše aplikace je teď připravený na podporu ověřování a identita služby.</span><span class="sxs-lookup"><span data-stu-id="54b05-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="54b05-122">Pouze musí mít tyto funkce dostupná uživatelům.</span><span class="sxs-lookup"><span data-stu-id="54b05-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="54b05-123">Migrace logiku registrace a přihlášení</span><span class="sxs-lookup"><span data-stu-id="54b05-123">Migrate registration and login logic</span></span>

<span data-ttu-id="54b05-124">Nakonfigurované pro aplikaci služby Identita a přístup k datům nakonfigurovat pomocí rozhraní Entity Framework a SQL Server jsme připraveni přidat podporu registrace a přihlášení do aplikace.</span><span class="sxs-lookup"><span data-stu-id="54b05-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="54b05-125">Odvolat, [výše v procesu migrace](xref:migration/mvc#migrate-the-layout-file) jsme označeno jako komentář odkaz na *_LoginPartial* v *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="54b05-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="54b05-126">Nyní je čas vrátit kódu, Odkomentujte ho a přidejte v zobrazení pro podporu funkcí přihlášení a potřeby řadiče.</span><span class="sxs-lookup"><span data-stu-id="54b05-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="54b05-127">Zrušením komentáře u `@Html.Partial` řádek v *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="54b05-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="54b05-128">Nyní, přidat nové zobrazení syntaxe Razor s názvem *_LoginPartial* k *zobrazení a sdílených* složky:</span><span class="sxs-lookup"><span data-stu-id="54b05-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="54b05-129">Aktualizace *_LoginPartial.cshtml* následujícím kódem (Nahraďte veškerý jeho obsah):</span><span class="sxs-lookup"><span data-stu-id="54b05-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

```cshtml
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="Logout" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log out</button>
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

<span data-ttu-id="54b05-130">V tomto okamžiku byste měli být schopni obnovit stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="54b05-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="54b05-131">Souhrn</span><span class="sxs-lookup"><span data-stu-id="54b05-131">Summary</span></span>

<span data-ttu-id="54b05-132">ASP.NET Core zavádí změny funkce ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="54b05-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="54b05-133">V tomto článku jste viděli, jak migrovat funkce správy ověřování a uživatele v identitě ASP.NET Identity do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="54b05-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
