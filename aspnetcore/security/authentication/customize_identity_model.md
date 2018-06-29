---
title: Přizpůsobení modelu identity
author: ajcvickers
description: Tento článek popisuje, jak přizpůsobit základní datový model Entity Framework Core pro ASP.NET Core Identity.
monikerRange: '>= aspnetcore-2.1'
ms.author: avickers
ms.date: 04/12/2018
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 41ea125414c5997ee36f4e312beba4ff318a4a8d
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077673"
---
# <a name="identity-model-customization"></a><span data-ttu-id="9af73-103">Přizpůsobení modelu identity</span><span class="sxs-lookup"><span data-stu-id="9af73-103">Identity model customization</span></span>

<span data-ttu-id="9af73-104">Podle [podle Arthur Vickerse](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="9af73-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="9af73-105">Jádro ASP.NET Identity poskytuje rozhraní pro správu a ukládání uživatelských účtů v aplikacích ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9af73-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core applications.</span></span> <span data-ttu-id="9af73-106">Identity se přidá do projektu při "Jednotlivé uživatelské účty" je vybrán jako ověřovací mechanismus.</span><span class="sxs-lookup"><span data-stu-id="9af73-106">Identity is added to your project when "Individual User Accounts" is selected as the authentication mechanism.</span></span> <span data-ttu-id="9af73-107">Ve výchozím nastavení, Identity využívá z Entity Framework (EF) základní datový model.</span><span class="sxs-lookup"><span data-stu-id="9af73-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="9af73-108">Tento článek popisuje postup přizpůsobení modelu Identity.</span><span class="sxs-lookup"><span data-stu-id="9af73-108">This article describes how to customize the Identity model.</span></span>

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="9af73-109">Migrace základní EF a identity</span><span class="sxs-lookup"><span data-stu-id="9af73-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="9af73-110">Před zkoumání modelu, je vhodné pochopit, jak Identity funguje s [EF základní migrace](/ef/core/managing-schemas/migrations/) k vytváření a aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="9af73-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="9af73-111">Na nejvyšší úrovni je proces:</span><span class="sxs-lookup"><span data-stu-id="9af73-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="9af73-112">Definování nebo aktualizovat [datového modelu v kódu](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="9af73-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="9af73-113">Přidejte migrace převede tento model na změny, které lze použít k databázi.</span><span class="sxs-lookup"><span data-stu-id="9af73-113">Add a migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="9af73-114">Zkontrolujte, že migrace správně představuje vaše záměry.</span><span class="sxs-lookup"><span data-stu-id="9af73-114">Check that the migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="9af73-115">Použijte migraci tak, aby byly synchronizované s modelem má databáze aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="9af73-115">Apply the migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="9af73-116">Opakováním kroků 1 – 4 dál Upřesnit modelu a synchronizujte databázi.</span><span class="sxs-lookup"><span data-stu-id="9af73-116">Repeat steps 1-4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="9af73-117">Migrace jsou přidány a použije pomocí:</span><span class="sxs-lookup"><span data-stu-id="9af73-117">Migrations are added and applied using:</span></span>

* <span data-ttu-id="9af73-118">[Konzola správce balíčků](/ef/core/miscellaneous/cli/powershell) (PMC), pokud používáte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9af73-118">The [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) if you are using Visual Studio.</span></span>
* <span data-ttu-id="9af73-119">[Dotnet příkazy](/ef/core/miscellaneous/cli/dotnet) na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="9af73-119">The [dotnet commands](/ef/core/miscellaneous/cli/dotnet) on the command line.</span></span>
* <span data-ttu-id="9af73-120">Při spuštění aplikace, vyberte odkaz migrace na chybové stránce.</span><span class="sxs-lookup"><span data-stu-id="9af73-120">Selecting the migrations link on the error page when the application is run.</span></span>

<span data-ttu-id="9af73-121">ASP.NET Core má stránky vývoj chyba obslužné rutiny, které je možné použít migrace při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="9af73-121">ASP.NET Core has a development-time error page handler that can be used to apply migrations when the application is run.</span></span> <span data-ttu-id="9af73-122">Aplikacích v produkčním prostředí, je často vhodné pro generování skriptů SQL z migrace a nasadit je do databáze v rámci řízené nasazení aplikace a databáze.</span><span class="sxs-lookup"><span data-stu-id="9af73-122">For production applications, it is often more appropriate to generate SQL scripts from the migrations and deploy these to the database as part of a controlled application and database deployment.</span></span>

<span data-ttu-id="9af73-123">Při vytváření nové aplikace pomocí Identity kroky 1 a 2 výše již byly dokončeny.</span><span class="sxs-lookup"><span data-stu-id="9af73-123">When a new application using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="9af73-124">To znamená počáteční datového modelu již existuje, a počáteční migrace byla přidána do projektu.</span><span class="sxs-lookup"><span data-stu-id="9af73-124">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="9af73-125">Počáteční migraci je ještě potřeba použijí k databázi.</span><span class="sxs-lookup"><span data-stu-id="9af73-125">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="9af73-126">Počáteční migrace můžete provést spuštěním Update-Database (pomocí PMC), příkaz dotnet ef databáze aktualizace (rozhraní .NET Core CLI) nebo pomocí chybovou stránku, pokud je aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="9af73-126">The initial migration can be done by running the Update-Database (PMC), the dotnet ef database update (.NET Core CLI) command, or by using the error page when the application is run.</span></span>

<span data-ttu-id="9af73-127">Předchozí kroky musí zopakovat, jakmile jsou změny modelu.</span><span class="sxs-lookup"><span data-stu-id="9af73-127">The preceding steps need to be repeated as changes are made to the model.</span></span>

<a name="identity-model"></a>

## <a name="the-identity-model"></a><span data-ttu-id="9af73-128">Modelem Identity</span><span class="sxs-lookup"><span data-stu-id="9af73-128">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="9af73-129">Typy entit</span><span class="sxs-lookup"><span data-stu-id="9af73-129">Entity types</span></span>

<span data-ttu-id="9af73-130">Modelem Identity se skládá ze sedmi typy entit:</span><span class="sxs-lookup"><span data-stu-id="9af73-130">The Identity model consists of seven entity types:</span></span>

* <span data-ttu-id="9af73-131">`User` -představuje uživatele</span><span class="sxs-lookup"><span data-stu-id="9af73-131">`User` - represents the user</span></span>
* <span data-ttu-id="9af73-132">`Role` -představuje roli</span><span class="sxs-lookup"><span data-stu-id="9af73-132">`Role` - represents a role</span></span>
* <span data-ttu-id="9af73-133">`UserClaim` -představuje požadavek, aby měl uživatel</span><span class="sxs-lookup"><span data-stu-id="9af73-133">`UserClaim` - represents a claim that a user possess</span></span>
* <span data-ttu-id="9af73-134">`UserToken` -představuje ověřovací token pro uživatele</span><span class="sxs-lookup"><span data-stu-id="9af73-134">`UserToken` - represents an authentication token for a user</span></span>
* <span data-ttu-id="9af73-135">`UserLogin` -propojuje uživatele s přihlašovacími údaji</span><span class="sxs-lookup"><span data-stu-id="9af73-135">`UserLogin` - associates a user with a login</span></span>
* <span data-ttu-id="9af73-136">`RoleClaim` -představuje deklaraci, která jsou udělena pro všechny uživatele v rámci role</span><span class="sxs-lookup"><span data-stu-id="9af73-136">`RoleClaim` - represents a claim that is granted to all users within a role</span></span>
* <span data-ttu-id="9af73-137">`UserRole` -připojení entita, která přidruží uživatelů a rolí</span><span class="sxs-lookup"><span data-stu-id="9af73-137">`UserRole` - join entity that associates users and roles</span></span>

### <a name="entity-type-relationships"></a><span data-ttu-id="9af73-138">Vztahy mezi typy entit</span><span class="sxs-lookup"><span data-stu-id="9af73-138">Entity type relationships</span></span>

<span data-ttu-id="9af73-139">Tyto typy entit se vztahují k sobě navzájem následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="9af73-139">These entity types are related to each other in the following ways:</span></span>

* <span data-ttu-id="9af73-140">Každý `User` může mít mnoho `UserClaims`</span><span class="sxs-lookup"><span data-stu-id="9af73-140">Each `User` can have many `UserClaims`</span></span>
* <span data-ttu-id="9af73-141">Každý `User` může mít mnoho `UserLogins`</span><span class="sxs-lookup"><span data-stu-id="9af73-141">Each `User` can have many `UserLogins`</span></span>
* <span data-ttu-id="9af73-142">Každý `User` může mít mnoho `UserTokens`</span><span class="sxs-lookup"><span data-stu-id="9af73-142">Each `User` can have many `UserTokens`</span></span>
* <span data-ttu-id="9af73-143">Každý `Role` může mít mnoho přidružené `RoleClaims`</span><span class="sxs-lookup"><span data-stu-id="9af73-143">Each `Role` can have many associated `RoleClaims`</span></span>
* <span data-ttu-id="9af73-144">Každý `User` může mít mnoho přidružené `Roles`a každou `Role` může být spojen s mnoha uživateli</span><span class="sxs-lookup"><span data-stu-id="9af73-144">Each `User` can have many associated `Roles`, and each `Role` can be associated with many Users</span></span>
  * <span data-ttu-id="9af73-145">Toto je vztah m: n, která vyžaduje připojení k tabulku v databázi.</span><span class="sxs-lookup"><span data-stu-id="9af73-145">This is a many-to-many relationship, which requires a join table in the database.</span></span> <span data-ttu-id="9af73-146">V tabulce spojení je reprezentována `UserRole` entity.</span><span class="sxs-lookup"><span data-stu-id="9af73-146">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="9af73-147">Výchozí konfigurace modelu</span><span class="sxs-lookup"><span data-stu-id="9af73-147">Default model configuration</span></span>

<span data-ttu-id="9af73-148">Identity definuje celou řadu "třídy kontextu", které dědí [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) konfigurace a použití modelu.</span><span class="sxs-lookup"><span data-stu-id="9af73-148">Identity defines a variety of "context classes" which inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="9af73-149">Tato konfigurace se provádí pomocí [EF základní kódu první rozhraní Fluent API](/ef/core/modeling/) v [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) metoda třídy kontextu.</span><span class="sxs-lookup"><span data-stu-id="9af73-149">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) method of the context class.</span></span> <span data-ttu-id="9af73-150">Výchozí konfigurace je:</span><span class="sxs-lookup"><span data-stu-id="9af73-150">The default configuration is:</span></span>

```CSharp
builder.Entity<TUser>(b =>
{
    // Primary key
    b.HasKey(u => u.Id);

    // Indexes for "normalized" username and email, to allow efficient lookups
    b.HasIndex(u => u.NormalizedUserName).HasName("UserNameIndex").IsUnique();
    b.HasIndex(u => u.NormalizedEmail).HasName("EmailIndex");

    // Maps to the AspNetUsers table
    b.ToTable("AspNetUsers");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(u => u.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.UserName).HasMaxLength(256);
    b.Property(u => u.NormalizedUserName).HasMaxLength(256);
    b.Property(u => u.Email).HasMaxLength(256);
    b.Property(u => u.NormalizedEmail).HasMaxLength(256);

    // The relationships between User and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each User can have many UserClaims
    b.HasMany<TUserClaim>().WithOne().HasForeignKey(uc => uc.UserId).IsRequired();

    // Each User can have many UserLogins
    b.HasMany<TUserLogin>().WithOne().HasForeignKey(ul => ul.UserId).IsRequired();

    // Each User can have many UserTokens
    b.HasMany<TUserToken>().WithOne().HasForeignKey(ut => ut.UserId).IsRequired();

    // Each User can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.UserId).IsRequired();
});

builder.Entity<TUserClaim>(b =>
{
    // Primary key
    b.HasKey(uc => uc.Id);

    // Maps to the AspNetUserClaims table
    b.ToTable("AspNetUserClaims");
});

builder.Entity<TUserLogin>(b =>
{
    // Composite primary key consisting of the LoginProvider and the key to use
    // with that provider
    b.HasKey(l => new { l.LoginProvider, l.ProviderKey });

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(t => t.LoginProvider).HasMaxLength(maxKeyLength);
    b.Property(t => t.Name).HasMaxLength(maxKeyLength);

    // Maps to the AspNetUserTokens table
    b.ToTable("AspNetUserTokens");
});

builder.Entity<TRole>(b =>
{
    // Primary key
    b.HasKey(r => r.Id);

    // Index for "normalized" role name to allow efficient lookups
    b.HasIndex(r => r.NormalizedName).HasName("RoleNameIndex").IsUnique();

    // Maps to the AspNetRoles table
    b.ToTable("AspNetRoles");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(r => r.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.Name).HasMaxLength(256);
    b.Property(u => u.NormalizedName).HasMaxLength(256);

    // The relationships between Role and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each Role can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.RoleId).IsRequired();

    // Each Role can have many associated RoleClaims
    b.HasMany<TRoleClaim>().WithOne().HasForeignKey(rc => rc.RoleId).IsRequired();
});

builder.Entity<TRoleClaim>(b =>
{
    // Primary key
    b.HasKey(rc => rc.Id);

    // Maps to the AspNetRoleClaims table
    b.ToTable("AspNetRoleClaims");
});

builder.Entity<TUserRole>(b =>
{
    // Primary key
    b.HasKey(r => new { r.UserId, r.RoleId });

    // Maps to the AspNetUserRoles table
    b.ToTable("AspNetUserRoles");
});
```

### <a name="model-generic-types"></a><span data-ttu-id="9af73-151">Obecné typy v modelu</span><span class="sxs-lookup"><span data-stu-id="9af73-151">Model generic types</span></span>

<span data-ttu-id="9af73-152">Identity definuje výchozí typy CLR pro každou z výše uvedené typy entit.</span><span class="sxs-lookup"><span data-stu-id="9af73-152">Identity defines default CLR types for each of the entity types listed above.</span></span> <span data-ttu-id="9af73-153">Tyto typy jsou všechny s předponou "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, a `IdentityUserRole`.</span><span class="sxs-lookup"><span data-stu-id="9af73-153">These types are all prefixed with "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, and `IdentityUserRole`.</span></span>

<span data-ttu-id="9af73-154">Místo přímo pomocí tyto typy, typy slouží jako základní třídy pro aplikace pro vlastní typy.</span><span class="sxs-lookup"><span data-stu-id="9af73-154">Rather than using these types directly, the types can be used as base classes for the application's own types.</span></span> <span data-ttu-id="9af73-155">`DbContext` Tříd definovaných výčtem Identity jsou obecné tak, že různé typy CLR lze použít pro jeden nebo více typů entit v modelu.</span><span class="sxs-lookup"><span data-stu-id="9af73-155">The `DbContext` classes defined by Identity are generic such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="9af73-156">Tyto obecné typy také umožňují typ uživatele primární klíč se musí změnit.</span><span class="sxs-lookup"><span data-stu-id="9af73-156">These generic types also allow for the type of the User primary key to be changed.</span></span>

<span data-ttu-id="9af73-157">Při použití Identity s podporou pro role, [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) má být použita třída:</span><span class="sxs-lookup"><span data-stu-id="9af73-157">When using Identity with support for roles, an [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) class should be used:</span></span>

```CSharp
// Uses all the built-in Identity types
// Uses `string` as the key type
public class IdentityDbContext
    : IdentityDbContext<IdentityUser, IdentityRole, string>
{
}

// Uses the built-in Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityDbContext<TUser>
    : IdentityDbContext<TUser, IdentityRole, string>
        where TUser : IdentityUser
{
}

// Uses the built-in Identity types except with custom User and Role types
// The key type is defined by TKey
public class IdentityDbContext<TUser, TRole, TKey>
    : IdentityDbContext<TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>, IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
    : IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
         where TUser : IdentityUser<TKey>
         where TRole : IdentityRole<TKey>
         where TKey : IEquatable<TKey>
         where TUserClaim : IdentityUserClaim<TKey>
         where TUserRole : IdentityUserRole<TKey>
         where TUserLogin : IdentityUserLogin<TKey>
         where TRoleClaim : IdentityRoleClaim<TKey>
         where TUserToken : IdentityUserToken<TKey>

```

<span data-ttu-id="9af73-158">Je také možné použít identitu bez rolí (jenom deklarace identity), v takovém případě `IdentityUserContext` má být použita třída:</span><span class="sxs-lookup"><span data-stu-id="9af73-158">It is also possible to use Identity without roles (only claims), in which case an `IdentityUserContext` class should be used:</span></span>


```CSharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey>
    : IdentityUserContext<TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
    : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customizing-the-model"></a><span data-ttu-id="9af73-159">Přizpůsobení modelu</span><span class="sxs-lookup"><span data-stu-id="9af73-159">Customizing the model</span></span>

<span data-ttu-id="9af73-160">Výchozím bodem pro přizpůsobení modelu je odvozen od typu odpovídající kontext; najdete v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="9af73-160">The starting point for customizing the model is to derive from the appropriate context type; see the preceding section.</span></span> <span data-ttu-id="9af73-161">Tento typ kontextu se běžně označuje `ApplicationDbContext` a je vytvořen pomocí šablony ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9af73-161">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="9af73-162">Kontext slouží ke konfiguraci modelu dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="9af73-162">The context is used to configure the model in two ways:</span></span>
* <span data-ttu-id="9af73-163">Zadáním typy entit a klíče pro parametry obecného typu.</span><span class="sxs-lookup"><span data-stu-id="9af73-163">By supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="9af73-164">Přepsáním `OnModelCreating` upravit mapování těchto typů.</span><span class="sxs-lookup"><span data-stu-id="9af73-164">By overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="9af73-165">Při přepsání `OnModelCreating`, `base.OnModelCreating` by měla být volána nejprve přepsání konfigurace by měla být volána Další.</span><span class="sxs-lookup"><span data-stu-id="9af73-165">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first, the overriding configuration should be called next.</span></span> <span data-ttu-id="9af73-166">Základní EF má obvykle služby wins poslední jeden zásad pro konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="9af73-166">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="9af73-167">Například pokud `ToTable` pro entitu typu je názvem první s názvem jednu tabulku a pak znovu později s jiný název tabulky a název tabulky v druhé volání je ten, který se používá.</span><span class="sxs-lookup"><span data-stu-id="9af73-167">For example, if the `ToTable` for an entity type is called first with one table name and then again later with a different table name, then the table name in the second call is the one that is used.</span></span>

### <a name="using-a-custom-user-type"></a><span data-ttu-id="9af73-168">Pomocí vlastního typu uživatele</span><span class="sxs-lookup"><span data-stu-id="9af73-168">Using a custom User type</span></span>

<span data-ttu-id="9af73-169">Pokud chcete používat vlastní typ uživatele, vytvořit typ a mějte ho dědí `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="9af73-169">To use a custom User type, create the type and have it inherit from `IdentityUser`.</span></span> <span data-ttu-id="9af73-170">Se obvykle název tohoto typu `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="9af73-170">It is customary to name this type `ApplicationUser`.</span></span> <span data-ttu-id="9af73-171">Tento typ obvykle mají další vlastnosti není v základním typu, jinak by žádná hodnota v její vytvoření.</span><span class="sxs-lookup"><span data-stu-id="9af73-171">This type will typically have additional properties not in the base type, otherwise there would be no value in creating it.</span></span> <span data-ttu-id="9af73-172">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9af73-172">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="9af73-173">Tento typ vedle použijte jako obecné argument pro kontext:</span><span class="sxs-lookup"><span data-stu-id="9af73-173">Next use this type as a generic argument for the context:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="9af73-174">Aktualizace `ConfigureServices` použití nového `ApplicationUser` třídy:</span><span class="sxs-lookup"><span data-stu-id="9af73-174">Update `ConfigureServices` to use the new `ApplicationUser` class:</span></span>

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="9af73-175">Není nutné přepsat `OnModelCreating` sem, protože základní EF bude mapovat `CustomTag` vlastnost podle konvence.</span><span class="sxs-lookup"><span data-stu-id="9af73-175">There is no need to override `OnModelCreating` here because EF Core will map the `CustomTag` property by convention.</span></span> <span data-ttu-id="9af73-176">Však bude nutné aktualizovat k získání nového databázi `CustomTag` sloupce.</span><span class="sxs-lookup"><span data-stu-id="9af73-176">However, the database will need to be updated to get a new `CustomTag` column.</span></span> <span data-ttu-id="9af73-177">K tomu, přidejte migrace a pak aktualizujte databázi, jak je popsáno v [identitu a migrace základní EF](#identity-migrations).</span><span class="sxs-lookup"><span data-stu-id="9af73-177">To do this, add a migration, and then update the database as described in  [Identity and EF Core Migrations](#identity-migrations).</span></span>

### <a name="changing-the-key-type"></a><span data-ttu-id="9af73-178">Změna typu klíče</span><span class="sxs-lookup"><span data-stu-id="9af73-178">Changing the key type</span></span>

<span data-ttu-id="9af73-179">Změna typu sloupec primárního klíče (Primárníklíč), po vytvoření databáze je problematické na mnoha systémy databáze.</span><span class="sxs-lookup"><span data-stu-id="9af73-179">Changing the type of a primary key (PK) column after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="9af73-180">Změna primárnímu Klíči obvykle zahrnuje vyřadit a znovu vytvořit v tabulce.</span><span class="sxs-lookup"><span data-stu-id="9af73-180">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="9af73-181">Proto se doporučuje, aby typy klíčů zadat počáteční migrace tak, aby klíče typy cíle jsou vytvořeny při vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="9af73-181">Therefore, it is recommended that key types be specified in the initial migration such that the target key types are created when the database is created.</span></span>

<span data-ttu-id="9af73-182">Pokud databáze byla vytvořena, pak `Drop-Database` (pomocí PMC) nebo `dotnet ef database drop` (.NET Core CLI) je možné jej odstranit.</span><span class="sxs-lookup"><span data-stu-id="9af73-182">If the database has been created, then `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) can be used to delete it.</span></span>

<span data-ttu-id="9af73-183">Jakmile se potvrzuje, že databáze neexistuje, odeberte počáteční migrace s `Remove-Migration` (pomocí PMC) nebo `dotnet ef migrations remove` (.NET Core CLI).</span><span class="sxs-lookup"><span data-stu-id="9af73-183">Once it is confirmed that the database doesn't exist,  remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>

<span data-ttu-id="9af73-184">Aktualizace `ApplicationDbContext` použití různých základní třídy určující nový typ klíče pro `TKey`.</span><span class="sxs-lookup"><span data-stu-id="9af73-184">Update the `ApplicationDbContext` to use a different base class, specifying the new key type for `TKey`.</span></span> <span data-ttu-id="9af73-185">Chcete-li například použít `Guid` klíč:</span><span class="sxs-lookup"><span data-stu-id="9af73-185">For example, to use a `Guid` key:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="9af73-186">Všimněte si, že obecné třídy `IdentityUser<TKey>` a `IdentityRole<TKey>` musí být zadaná také používat nový typ klíče.</span><span class="sxs-lookup"><span data-stu-id="9af73-186">Notice that the generic classes `IdentityUser<TKey>` and `IdentityRole<TKey>` must also be specified to use the new key type.</span></span> <span data-ttu-id="9af73-187">`ConfigureServices` musí být aktualizovány k použití obecné uživatele:</span><span class="sxs-lookup"><span data-stu-id="9af73-187">`ConfigureServices` must be updated to use the generic user:</span></span>

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="9af73-188">Pokud vlastní `ApplicationUser` je používají, aktualizujte dědění z `IdentityUser<TKey>`.</span><span class="sxs-lookup"><span data-stu-id="9af73-188">If a custom `ApplicationUser` is being used, update it to inherit from `IdentityUser<TKey>`.</span></span> <span data-ttu-id="9af73-189">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9af73-189">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser<Guid>
{
    public string CustomTag { get; set; }
}

public class ApplicationDbContext : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="adding-navigation-properties"></a><span data-ttu-id="9af73-190">Přidání navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9af73-190">Adding navigation properties</span></span>

<span data-ttu-id="9af73-191">Změna konfigurace modelu u relací, může být obtížnější než dělat jiné změny.</span><span class="sxs-lookup"><span data-stu-id="9af73-191">Changing the model configuration for relationships can be a more difficult than making other changes.</span></span> <span data-ttu-id="9af73-192">Musí být pozor na místo vytvoření nové relace další nahradit existující relace.</span><span class="sxs-lookup"><span data-stu-id="9af73-192">Care must be taken to replace the existing relationships rather than create a new additional relationships.</span></span> <span data-ttu-id="9af73-193">Konkrétně změněné relace, musíte zadat stejnou vlastností cizího klíče jako existující relaci.</span><span class="sxs-lookup"><span data-stu-id="9af73-193">In particular, the changed relationship must specify the same foreign key property as the existing relationship.</span></span> <span data-ttu-id="9af73-194">Například vztah mezi `Users` a `UserClaims` je ve výchozím nastavení zadaný jako:</span><span class="sxs-lookup"><span data-stu-id="9af73-194">For example, the relationship between `Users` and `UserClaims` is by default specified as:</span></span>

```CSharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

<span data-ttu-id="9af73-195">Cizí klíč pro tento vztah je zadán jako `UserClaim.UserId` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="9af73-195">The foreign key for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="9af73-196">`HasMany` a `WithOne` se nazývají bez argumentů k vytvoření vztahu bez navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9af73-196">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="9af73-197">Přidat navigační vlastnost, která má `ApplicationUser` který vám umožní přidružené `UserClaims` bude odkazovat od uživatele:</span><span class="sxs-lookup"><span data-stu-id="9af73-197">Add a navigation property to `ApplicationUser` that will allow associated `UserClaims` to be referenced from the user:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="9af73-198">Všimněte si, že `TKey` pro `IdentityUserClaim<TKey>` je typ určený pro primární klíč uživatelů – v tomto případě `string` vzhledem k tomu, že používáme výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9af73-198">Note that the `TKey` for `IdentityUserClaim<TKey>` is type specified for the primary key of users--in this case `string` since we're using the defaults.</span></span> <span data-ttu-id="9af73-199">Je **není** primární typ klíče pro `UserClaim` typ entity.</span><span class="sxs-lookup"><span data-stu-id="9af73-199">It is **not** the primary key type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="9af73-200">Teď, když existuje navigační vlastnost musí být konfigurované v `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="9af73-200">Now that the navigation property exists it must be configured in `OnModelCreating`:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();
        });
    }
}
```

<span data-ttu-id="9af73-201">Všimněte si, že je nakonfigurována relace přesně tak, jak bylo dříve, pouze s navigační vlastnost zadaný ve volání `HasMany`.</span><span class="sxs-lookup"><span data-stu-id="9af73-201">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="9af73-202">Navigační vlastnosti existovat pouze ve model EF, nikoli databáze.</span><span class="sxs-lookup"><span data-stu-id="9af73-202">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="9af73-203">Protože cizí klíč pro relaci se nezměnila, tento druh změn modelu nevyžaduje databázi aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="9af73-203">Because the foreign key for the relationship has not changed, this kind of model change does not require the database to be updated.</span></span> <span data-ttu-id="9af73-204">To lze ověřit tak, že přidáte migrace po provedení změny: `Up` a `Down` metody jsou prázdné.</span><span class="sxs-lookup"><span data-stu-id="9af73-204">This can be checked by adding a migration after making the change: the `Up` and `Down` methods are empty.</span></span>

### <a name="adding-all-user-navigation-properties"></a><span data-ttu-id="9af73-205">Přidat všechny uživatele navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9af73-205">Adding all User navigation properties</span></span>

<span data-ttu-id="9af73-206">Pomocí výše uvedeného oddílu jako vodítko, tady je příklad, který konfiguruje jednosměrný navigační vlastnosti pro všechny relace na uživatele:</span><span class="sxs-lookup"><span data-stu-id="9af73-206">Using the section above as guidance, here is an example that configures unidirectional navigation properties for all relationships on User:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne()
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });
    }
}
```

### <a name="adding-user-and-role-navigation-properties"></a><span data-ttu-id="9af73-207">Přidání uživatelů a rolí navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9af73-207">Adding User and Role navigation properties</span></span>

<span data-ttu-id="9af73-208">Pomocí výše uvedeného oddílu jako vodítko, tady je příklad, který nakonfiguruje navigační vlastnosti pro všechny relace na uživatele a Role:</span><span class="sxs-lookup"><span data-stu-id="9af73-208">Using the section above as guidance, here is an example that configures navigation properties for all relationships on User and Role:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}
```

```CSharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        IdentityUserClaim<string>, ApplicationUserRole, IdentityUserLogin<string>,
        IdentityRoleClaim<string>, IdentityUserToken<string>>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();
        });

    }
}
```

<span data-ttu-id="9af73-209">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="9af73-209">Notes:</span></span>

* <span data-ttu-id="9af73-210">Tento příklad také obsahuje `UserRole` připojení entity, která je nutná pro přejděte relace m: n od uživatelů do rolí.</span><span class="sxs-lookup"><span data-stu-id="9af73-210">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="9af73-211">Mějte na paměti, chcete-li změnit typy navigační vlastnosti tak, aby odrážela který `ApplicationXxx` typy jsou nyní používány místo `IdentityXxx` typy.</span><span class="sxs-lookup"><span data-stu-id="9af73-211">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="9af73-212">Nezapomeňte použít `ApplicationXxx` v Obecné `ApplicationContext` definice.</span><span class="sxs-lookup"><span data-stu-id="9af73-212">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="adding-all-navigation-properties"></a><span data-ttu-id="9af73-213">Přidání všech navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9af73-213">Adding all navigation properties</span></span>

<span data-ttu-id="9af73-214">Pomocí výše uvedeného oddílu jako vodítko, tady je příklad, který nakonfiguruje navigační vlastnosti pro všechny relace na všech typů entit:</span><span class="sxs-lookup"><span data-stu-id="9af73-214">Using the section above as guidance, here is an example that configures navigation properties for all relationships on all entity types:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<ApplicationUserClaim> Claims { get; set; }
    public virtual ICollection<ApplicationUserLogin> Logins { get; set; }
    public virtual ICollection<ApplicationUserToken> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
    public virtual ICollection<ApplicationRoleClaim> RoleClaims { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserClaim : IdentityUserClaim<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationUserLogin : IdentityUserLogin<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationRoleClaim : IdentityRoleClaim<string>
{
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserToken : IdentityUserToken<string>
{
    public virtual ApplicationUser User { get; set; }
}
```

```CSharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        ApplicationUserClaim, ApplicationUserRole, ApplicationUserLogin,
        ApplicationRoleClaim, ApplicationUserToken>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne(e => e.User)
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne(e => e.User)
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne(e => e.User)
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();

            // Each Role can have many associated RoleClaims
            b.HasMany(e => e.RoleClaims)
                .WithOne(e => e.Role)
                .HasForeignKey(rc => rc.RoleId)
                .IsRequired();
        });

    }
}
```

### <a name="using-composite-keys"></a><span data-ttu-id="9af73-215">Pomocí složených klíčů</span><span class="sxs-lookup"><span data-stu-id="9af73-215">Using composite keys</span></span>

<span data-ttu-id="9af73-216">V předchozích částech ukázán Změna typu klíč použít v modelu Identity.</span><span class="sxs-lookup"><span data-stu-id="9af73-216">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="9af73-217">Změna klíče model Identity, který se má použít složené klíče není podporován nebo doporučené.</span><span class="sxs-lookup"><span data-stu-id="9af73-217">Changing the Identity key model to use composite keys is not supported or recommended.</span></span> <span data-ttu-id="9af73-218">Pomocí Identity složený klíč by zahrnuje změnu, jak kód Identity manager komunikuje s modelem, což je nepodporovaný přizpůsobení a nad rámec tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9af73-218">Using a composite key with Identity would involve changing how the Identity manager code interacts with the model, which is an unsupported customization and beyond the scope of this document.</span></span>

### <a name="changing-tablecolumn-names-and-facets"></a><span data-ttu-id="9af73-219">Změna tabulky nebo sloupce názvy a omezující vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9af73-219">Changing table/column names and facets</span></span>

<span data-ttu-id="9af73-220">Chcete-li změnit názvy tabulek a sloupců, volejte `base.OnModelCreating`a poté přidejte konfigurace přepsat všechny výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9af73-220">To change the names of tables and columns, call `base.OnModelCreating`, and then add configuration to override any of the defaults.</span></span> <span data-ttu-id="9af73-221">Chcete-li například změnit název všechny identity tabulky:</span><span class="sxs-lookup"><span data-stu-id="9af73-221">For example, to change the name of all the identity tables:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.ToTable("MyUsers");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.ToTable("MyUserClaims");
    });

    modelBuilder.Entity<IdentityUserLogin<string>>(b =>
    {
        b.ToTable("MyUserLogins");
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.ToTable("MyUserTokens");
    });

    modelBuilder.Entity<IdentityRole>(b =>
    {
        b.ToTable("MyRoles");
    });

    modelBuilder.Entity<IdentityRoleClaim<string>>(b =>
    {
        b.ToTable("MyRoleClaims");
    });

    modelBuilder.Entity<IdentityUserRole<string>>(b =>
    {
        b.ToTable("MyUserRoles");
    });
}
```

<span data-ttu-id="9af73-222">Všimněte si, že tyto příklady použijte výchozí typy Identity.</span><span class="sxs-lookup"><span data-stu-id="9af73-222">Note that these examples use the default Identity types.</span></span> <span data-ttu-id="9af73-223">Pokud použijete typ aplikace, jako například `ApplicationUser` nakonfigurujte typu místo výchozího typu.</span><span class="sxs-lookup"><span data-stu-id="9af73-223">If you are using an application type, such as `ApplicationUser` then configure that type instead of the default type.</span></span>

<span data-ttu-id="9af73-224">Tady je příklad, který změní některé názvy sloupců:</span><span class="sxs-lookup"><span data-stu-id="9af73-224">Here is an example that changes some column names:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(e => e.Email).HasColumnName("EMail");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.Property(e => e.ClaimType).HasColumnName("CType");
        b.Property(e => e.ClaimValue).HasColumnName("CValue");
    });
}
```

<span data-ttu-id="9af73-225">Některé typy sloupců databáze může být nakonfigurovaná s určité _omezující vlastnosti_ například maximální délku řetězce povoleny.</span><span class="sxs-lookup"><span data-stu-id="9af73-225">Some types of database columns can be configured with certain _facets_ such as the maximum string length allowed.</span></span> <span data-ttu-id="9af73-226">Tady je příklad, který nastaví sloupce maximální délky pro několik řetězec vlastnosti v modelu:</span><span class="sxs-lookup"><span data-stu-id="9af73-226">Here is an example that sets column max lengths for several string properties in the model:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(u => u.UserName).HasMaxLength(128);
        b.Property(u => u.NormalizedUserName).HasMaxLength(128);
        b.Property(u => u.Email).HasMaxLength(128);
        b.Property(u => u.NormalizedEmail).HasMaxLength(128);
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.Property(t => t.LoginProvider).HasMaxLength(128);
        b.Property(t => t.Name).HasMaxLength(128);
    });
}
```

### <a name="mapping-to-a-different-schema"></a><span data-ttu-id="9af73-227">Mapování na jiné schéma.</span><span class="sxs-lookup"><span data-stu-id="9af73-227">Mapping to a different schema</span></span>

<span data-ttu-id="9af73-228">Schémata mohou chovat jinak v jiné databázi poskytovatelů, ale pro SQL Server, ve výchozím nastavení se vytvořit všechny tabulky ve schématu "dbo".</span><span class="sxs-lookup"><span data-stu-id="9af73-228">Schemas can behave differently in different database providers, but for SQL Server, the default is to create all tables in the "dbo" schema.</span></span> <span data-ttu-id="9af73-229">Toto můžete změnit na místo vytváření tabulky v jiném schématu.</span><span class="sxs-lookup"><span data-stu-id="9af73-229">This can be changed to instead create the tables in a different schema.</span></span> <span data-ttu-id="9af73-230">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9af73-230">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a><span data-ttu-id="9af73-231">Opožděného načítání</span><span class="sxs-lookup"><span data-stu-id="9af73-231">Lazy loading</span></span>

<span data-ttu-id="9af73-232">V této části je přidána podpora pro proxy opožděného načítání modelu Identity.</span><span class="sxs-lookup"><span data-stu-id="9af73-232">In this section support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="9af73-233">Lazy načítání je užitečné, protože umožňuje navigační vlastnosti bez první zajistíte, že jsou načtená.</span><span class="sxs-lookup"><span data-stu-id="9af73-233">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they are loaded.</span></span>

<span data-ttu-id="9af73-234">Typy entit nelze realizovat vhodný pro opožděného načítání několika způsoby, jak je popsáno v [EF základní dokumentace](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="9af73-234">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="9af73-235">Pro jednoduchost použijeme opožděného načítání proxy, který vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="9af73-235">For simplicity, we will use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="9af73-236">Instalace [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="9af73-236">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="9af73-237">Volání `.UseLazyLoadingProxies()` uvnitř `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="9af73-237">A call to `.UseLazyLoadingProxies()` inside `AddDbContext`.</span></span>
* <span data-ttu-id="9af73-238">Typy entit veřejné s veřejné virtuální navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9af73-238">Public entity types with public virtual navigation properties.</span></span>

<span data-ttu-id="9af73-239">Tady je příklad volání `.UseLazyLoadingProxies()`:</span><span class="sxs-lookup"><span data-stu-id="9af73-239">Here is an example of calling `.UseLazyLoadingProxies()`:</span></span>

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="9af73-240">Odkazuje zpět na příklady výše pro přidání navigační vlastnosti do typy entit.</span><span class="sxs-lookup"><span data-stu-id="9af73-240">Refer back to the examples above for adding navigation properties to the entity types.</span></span>
