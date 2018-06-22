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
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Migrace ověřování a identita na jádro ASP.NET

Podle [Steve Smith](https://ardalis.com/)

V předchozím článku jsme [migrovat konfigurace z projektu aplikace ASP.NET MVC rozhraní ASP.NET MVC základní](xref:migration/configuration). V tomto článku jsme migrovat funkce správy registrace, přihlášení a uživatele.

## <a name="configure-identity-and-membership"></a>Nakonfigurujte identitu a členství

V architektuře ASP.NET MVC, ověřování a identita funkcí je nakonfigurována pomocí ASP.NET Identity v *Startup.Auth.cs* a *IdentityConfig.cs*, který je umístěn v *App_Start* složka. V aplikaci ASP.NET MVC jádra, tyto funkce jsou nakonfigurované v *Startup.cs*.

Nainstalujte `Microsoft.AspNetCore.Identity.EntityFrameworkCore` a `Microsoft.AspNetCore.Authentication.Cookies` balíčky NuGet.

Potom otevřete *Startup.cs* a aktualizovat `Startup.ConfigureServices` metoda použít službu pro rozhraní Entity Framework a Identity:

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

V tomto okamžiku existují dva typy odkazovaný ve výše uvedený kód, který jsme nebyly migrovány z projektu ASP.NET MVC: `ApplicationDbContext` a `ApplicationUser`. Vytvořte novou *modely* složky v ASP.NET Core projektu a přidejte do ní odpovídající tyto typy dvou tříd. Zjistíte, ASP.NET MVC verzích tyto třídy ve */Models/IdentityModels.cs*, ale budeme používat jeden soubor pro třídy migrované projektu, protože se trochu objasnit.

*ApplicationUser.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

*ApplicationDbContext.cs*:

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

ASP.NET Core MVC Starter webového projektu neobsahuje mnohem přizpůsobení uživatelů, nebo `ApplicationDbContext`. Při migraci skutečné aplikace, musíte taky migrovat všechny vlastní vlastnosti a metody uživatele vaší aplikace a `DbContext` třídy a také další třídy modelu, vaše aplikace využívá. Například pokud vaše `DbContext` má `DbSet<Album>`, budete muset migrovat `Album` třídy.

S těmito soubory na místě *Startup.cs* souboru můžete provedeny zkompilovat aktualizací jeho `using` příkazy:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Naše aplikace je teď připravený na podporu ověřování a identita služby. Pouze musí mít tyto funkce dostupná uživatelům.

## <a name="migrate-registration-and-login-logic"></a>Migrace logiku registrace a přihlášení

Nakonfigurované pro aplikaci služby Identita a přístup k datům nakonfigurovat pomocí rozhraní Entity Framework a SQL Server jsme připraveni přidat podporu registrace a přihlášení do aplikace. Odvolat, [výše v procesu migrace](xref:migration/mvc#migrate-the-layout-file) jsme označeno jako komentář odkaz na *_LoginPartial* v *_Layout.cshtml*. Nyní je čas vrátit kódu, Odkomentujte ho a přidejte v zobrazení pro podporu funkcí přihlášení a potřeby řadiče.

Zrušením komentáře u `@Html.Partial` řádek v *_Layout.cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Nyní, přidat nové zobrazení syntaxe Razor s názvem *_LoginPartial* k *zobrazení a sdílených* složky:

Aktualizace *_LoginPartial.cshtml* následujícím kódem (Nahraďte veškerý jeho obsah):

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

V tomto okamžiku byste měli být schopni obnovit stránku v prohlížeči.

## <a name="summary"></a>Souhrn

ASP.NET Core zavádí změny funkce ASP.NET Identity. V tomto článku jste viděli, jak migrovat funkce správy ověřování a uživatele v identitě ASP.NET Identity do ASP.NET Core.
