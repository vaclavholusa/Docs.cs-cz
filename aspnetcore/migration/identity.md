---
title: "Migrace ověřování a identita"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/identity
ms.openlocfilehash: a3aa976578d8db089c69bf888f492f9cd7df824b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-authentication-and-identity"></a>Migrace ověřování a identita

<a name="migration-identity"></a>

Podle [Steve Smith](https://ardalis.com/)

V předchozím článku jsme [migrovat konfigurace z projektu aplikace ASP.NET MVC rozhraní ASP.NET MVC základní](configuration.md). V tomto článku jsme migrovat funkce správy registrace, přihlášení a uživatele.

## <a name="configure-identity-and-membership"></a>Nakonfigurujte identitu a členství

Ověřování a identita funkcí v architektuře ASP.NET MVC je nakonfigurována pomocí ASP.NET Identity v Startup.Auth.cs a IdentityConfig.cs, umístěný ve složce App_Start. V aplikaci ASP.NET MVC jádra, tyto funkce jsou nakonfigurované v *Startup.cs*.

Nainstalujte `Microsoft.AspNetCore.Identity.EntityFrameworkCore` a `Microsoft.AspNetCore.Authentication.Cookies` balíčky NuGet.

Potom otevřete Startup.cs a aktualizací `ConfigureServices()` metoda použít službu pro rozhraní Entity Framework a Identity:

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

V tomto okamžiku existují dva typy odkazovaný ve výše uvedený kód, který jsme nebyly migrovány z projektu ASP.NET MVC: `ApplicationDbContext` a `ApplicationUser`. Vytvořte novou *modely* složky v ASP.NET Core projektu a přidejte do ní odpovídající tyto typy dvou tříd. Zjistíte, ASP.NET MVC verzích tyto třídy ve `/Models/IdentityModels.cs`, ale budeme používat jeden soubor pro třídy migrované projektu, protože se trochu objasnit.

ApplicationUser.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

ApplicationDbContext.cs:

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

ASP.NET Core MVC Starter webového projektu neobsahuje mnohem přizpůsobení uživatelů nebo ApplicationDbContext. Při migraci reálné aplikaci, budete také muset migrovat všechny vlastní vlastnosti a metody uživatelů vaší aplikace a třídy DbContext, jakož i další, které vaše aplikace využívá (například pokud má vaše DbContext DbSettřídymodelu<Album>, samozřejmě je nutné migrovat třídě alb).

S těmito soubory na místě, můžete provést souboru Startup.cs zkompilovat aktualizací jeho pomocí příkazů:

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

Naše aplikace je teď připravený na podporu ověřování a identita služby – pouze musí mít tyto funkce dostupná uživatelům.

## <a name="migrate-registration-and-login-logic"></a>Migrace registrace a přihlášení logiky

Pomocí identity služby nakonfigurovat pro aplikaci a nakonfigurovat pomocí rozhraní Entity Framework a SQL Server přístup k datům jsme jste připraveni přidat podporu registrace a přihlášení do aplikace. Odvolat, [výše v procesu migrace](mvc.md#migrate-layout-file) jsme označeno jako komentář odkaz na _LoginPartial v _Layout.cshtml. Nyní je čas vrátit kódu, Odkomentujte ho a přidejte v zobrazení pro podporu funkcí přihlášení a potřeby řadiče.

Aktualizace _Layout.cshtml; Zrušením komentáře u @Html.Partial řádku:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Nyní přidejte novou stránku zobrazení MVC s názvem _LoginPartial do zobrazení nebo sdílené složky:

Aktualizovat _LoginPartial.cshtml následujícím kódem (Nahraďte veškerý jeho obsah):

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

V tomto okamžiku byste měli být schopni obnovit stránku v prohlížeči.

## <a name="summary"></a>Souhrn

ASP.NET Core zavádí změny funkce ASP.NET Identity. V tomto článku jste viděli, jak migrovat funkcím pro správu ověřování a uživatel ASP.NET identity do ASP.NET Core.
