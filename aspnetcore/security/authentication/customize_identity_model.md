---
title: Přizpůsobení modelu identity
author: ajcvickers
description: Tento článek popisuje, jak přizpůsobit základní datový model Entity Framework Core pro ASP.NET Core Identity.
ms.author: avickers
ms.date: 04/12/2018
ms.prod: asp.net-core
uid: security/authentication/customize_identity_model
ms.openlocfilehash: b44b4fd0f24d245b969588a7226ea6aacbe2a722
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036908"
---
# <a name="identity-model-customization"></a>Přizpůsobení modelu identity

Podle [podle Arthur Vickerse](https://github.com/ajcvickers)

Jádro ASP.NET Identity poskytuje rozhraní pro správu a ukládání uživatelských účtů v aplikacích ASP.NET Core. Identity se přidá do projektu při "Jednotlivé uživatelské účty" je vybrán jako ověřovací mechanismus. Ve výchozím nastavení, Identity využívá z Entity Framework (EF) základní datový model. Tento článek popisuje postup přizpůsobení modelu Identity.

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a>Migrace základní EF a identity

Před zkoumání modelu, je vhodné pochopit, jak Identity funguje s [EF základní migrace](/ef/core/managing-schemas/migrations/) k vytváření a aktualizaci databáze. Na nejvyšší úrovni je proces:

1. Definování nebo aktualizovat [datového modelu v kódu](/ef/core/modeling/).
1. Přidejte migrace převede tento model na změny, které lze použít k databázi.
1. Zkontrolujte, že migrace správně představuje vaše záměry.
1. Použijte migraci tak, aby byly synchronizované s modelem má databáze aktualizovat.
1. Opakováním kroků 1 – 4 dál Upřesnit modelu a synchronizujte databázi.

Migrace jsou přidány a použije pomocí:

* [Konzola správce balíčků](/ef/core/miscellaneous/cli/powershell) (PMC), pokud používáte Visual Studio.
* [Dotnet příkazy](/ef/core/miscellaneous/cli/dotnet) na příkazovém řádku.
* Při spuštění aplikace, vyberte odkaz migrace na chybové stránce.

ASP.NET Core má stránky vývoj chyba obslužné rutiny, které je možné použít migrace při spuštění aplikace. Aplikacích v produkčním prostředí, je často vhodné pro generování skriptů SQL z migrace a nasadit je do databáze v rámci řízené nasazení aplikace a databáze.

Při vytváření nové aplikace pomocí Identity kroky 1 a 2 výše již byly dokončeny. To znamená počáteční datového modelu již existuje, a počáteční migrace byla přidána do projektu. Počáteční migraci je ještě potřeba použijí k databázi. Počáteční migrace můžete provést spuštěním Update-Database (pomocí PMC), příkaz dotnet ef databáze aktualizace (rozhraní .NET Core CLI) nebo pomocí chybovou stránku, pokud je aplikace spuštěna.

Předchozí kroky musí zopakovat, jakmile jsou změny modelu.

<a name="identity-model"></a>

## <a name="the-identity-model"></a>Modelem Identity

### <a name="entity-types"></a>Typy entit

Modelem Identity se skládá ze sedmi typy entit:

* `User` -představuje uživatele
* `Role` -představuje roli
* `UserClaim` -představuje požadavek, aby měl uživatel
* `UserToken` -představuje ověřovací token pro uživatele
* `UserLogin` -propojuje uživatele s přihlašovacími údaji
* `RoleClaim` -představuje deklaraci, která jsou udělena pro všechny uživatele v rámci role
* `UserRole` -připojení entita, která přidruží uživatelů a rolí

### <a name="entity-type-relationships"></a>Vztahy mezi typy entit

Tyto typy entit se vztahují k sobě navzájem následujícími způsoby:

* Každý `User` může mít mnoho `UserClaims`
* Každý `User` může mít mnoho `UserLogins`
* Každý `User` může mít mnoho `UserTokens`
* Každý `Role` může mít mnoho přidružené `RoleClaims`
* Každý `User` může mít mnoho přidružené `Roles`a každou `Role` může být spojen s mnoha uživateli
  * Toto je vztah m: n, která vyžaduje připojení k tabulku v databázi. V tabulce spojení je reprezentována `UserRole` entity.

### <a name="default-model-configuration"></a>Výchozí konfigurace modelu

Identity definuje celou řadu "třídy kontextu", které dědí [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) konfigurace a použití modelu. Tato konfigurace se provádí pomocí [EF základní kódu první rozhraní Fluent API](/ef/core/modeling/) v [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) metoda třídy kontextu. Výchozí konfigurace je:

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

### <a name="model-generic-types"></a>Obecné typy v modelu

Identity definuje výchozí typy CLR pro každou z výše uvedené typy entit. Tyto typy jsou všechny s předponou "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, a `IdentityUserRole`.

Místo přímo pomocí tyto typy, typy slouží jako základní třídy pro aplikace pro vlastní typy. `DbContext` Tříd definovaných výčtem Identity jsou obecné tak, že různé typy CLR lze použít pro jeden nebo více typů entit v modelu. Tyto obecné typy také umožňují typ uživatele primární klíč se musí změnit.

Při použití Identity s podporou pro role, [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) má být použita třída:

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

Je také možné použít identitu bez rolí (jenom deklarace identity), v takovém případě `IdentityUserContext` má být použita třída:


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

## <a name="customizing-the-model"></a>Přizpůsobení modelu

Výchozím bodem pro přizpůsobení modelu je odvozen od typu odpovídající kontext; najdete v předchozí části. Tento typ kontextu se běžně označuje `ApplicationDbContext` a je vytvořen pomocí šablony ASP.NET Core.

Kontext slouží ke konfiguraci modelu dvěma způsoby:
* Zadáním typy entit a klíče pro parametry obecného typu.
* Přepsáním `OnModelCreating` upravit mapování těchto typů.

Při přepsání `OnModelCreating`, `base.OnModelCreating` by měla být volána nejprve přepsání konfigurace by měla být volána Další. Základní EF má obvykle služby wins poslední jeden zásad pro konfiguraci. Například pokud `ToTable` pro entitu typu je názvem první s názvem jednu tabulku a pak znovu později s jiný název tabulky a název tabulky v druhé volání je ten, který se používá.

### <a name="using-a-custom-user-type"></a>Pomocí vlastního typu uživatele

Pokud chcete používat vlastní typ uživatele, vytvořit typ a mějte ho dědí `IdentityUser`. Se obvykle název tohoto typu `ApplicationUser`. Tento typ obvykle mají další vlastnosti není v základním typu, jinak by žádná hodnota v její vytvoření. Příklad:

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Tento typ vedle použijte jako obecné argument pro kontext:

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Aktualizace `ConfigureServices` použití nového `ApplicationUser` třídy:

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Není nutné přepsat `OnModelCreating` sem, protože základní EF bude mapovat `CustomTag` vlastnost podle konvence. Však bude nutné aktualizovat k získání nového databázi `CustomTag` sloupce. K tomu, přidejte migrace a pak aktualizujte databázi, jak je popsáno v [identitu a migrace základní EF](#identity-migrations).

### <a name="changing-the-key-type"></a>Změna typu klíče

Změna typu sloupec primárního klíče (Primárníklíč), po vytvoření databáze je problematické na mnoha systémy databáze. Změna primárnímu Klíči obvykle zahrnuje vyřadit a znovu vytvořit v tabulce. Proto se doporučuje, aby typy klíčů zadat počáteční migrace tak, aby klíče typy cíle jsou vytvořeny při vytvoření databáze.

Pokud databáze byla vytvořena, pak `Drop-Database` (pomocí PMC) nebo `dotnet ef database drop` (.NET Core CLI) je možné jej odstranit.

Jakmile se potvrzuje, že databáze neexistuje, odeberte počáteční migrace s `Remove-Migration` (pomocí PMC) nebo `dotnet ef migrations remove` (.NET Core CLI).

Aktualizace `ApplicationDbContext` použití různých základní třídy určující nový typ klíče pro `TKey`. Chcete-li například použít `Guid` klíč:

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Všimněte si, že obecné třídy `IdentityUser<TKey>` a `IdentityRole<TKey>` musí být zadaná také používat nový typ klíče. `ConfigureServices` musí být aktualizovány k použití obecné uživatele:

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Pokud vlastní `ApplicationUser` je používají, aktualizujte dědění z `IdentityUser<TKey>`. Příklad:

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

### <a name="adding-navigation-properties"></a>Přidání navigační vlastnosti

Změna konfigurace modelu u relací, může být obtížnější než dělat jiné změny. Musí být pozor na místo vytvoření nové relace další nahradit existující relace. Konkrétně změněné relace, musíte zadat stejnou vlastností cizího klíče jako existující relaci. Například vztah mezi `Users` a `UserClaims` je ve výchozím nastavení zadaný jako:

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

Cizí klíč pro tento vztah je zadán jako `UserClaim.UserId` vlastnost. `HasMany` a `WithOne` se nazývají bez argumentů k vytvoření vztahu bez navigační vlastnosti.

Přidat navigační vlastnost, která má `ApplicationUser` který vám umožní přidružené `UserClaims` bude odkazovat od uživatele:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

Všimněte si, že `TKey` pro `IdentityUserClaim<TKey>` je typ určený pro primární klíč uživatelů – v tomto případě `string` vzhledem k tomu, že používáme výchozí hodnoty. Je **není** primární typ klíče pro `UserClaim` typ entity.

Teď, když existuje navigační vlastnost musí být konfigurované v `OnModelCreating`:

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

Všimněte si, že je nakonfigurována relace přesně tak, jak bylo dříve, pouze s navigační vlastnost zadaný ve volání `HasMany`.

Navigační vlastnosti existovat pouze ve model EF, nikoli databáze. Protože cizí klíč pro relaci se nezměnila, tento druh změn modelu nevyžaduje databázi aktualizovat. To lze ověřit tak, že přidáte migrace po provedení změny: `Up` a `Down` metody jsou prázdné.

### <a name="adding-all-user-navigation-properties"></a>Přidat všechny uživatele navigační vlastnosti

Pomocí výše uvedeného oddílu jako vodítko, tady je příklad, který konfiguruje jednosměrný navigační vlastnosti pro všechny relace na uživatele:

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

### <a name="adding-user-and-role-navigation-properties"></a>Přidání uživatelů a rolí navigační vlastnosti

Pomocí výše uvedeného oddílu jako vodítko, tady je příklad, který nakonfiguruje navigační vlastnosti pro všechny relace na uživatele a Role:

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

Poznámky:

* Tento příklad také obsahuje `UserRole` připojení entity, která je nutná pro přejděte relace m: n od uživatelů do rolí.
* Mějte na paměti, chcete-li změnit typy navigační vlastnosti tak, aby odrážela který `ApplicationXxx` typy jsou nyní používány místo `IdentityXxx` typy.
* Nezapomeňte použít `ApplicationXxx` v Obecné `ApplicationContext` definice.

### <a name="adding-all-navigation-properties"></a>Přidání všech navigační vlastnosti

Pomocí výše uvedeného oddílu jako vodítko, tady je příklad, který nakonfiguruje navigační vlastnosti pro všechny relace na všech typů entit:

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

### <a name="using-composite-keys"></a>Pomocí složených klíčů

V předchozích částech ukázán Změna typu klíč použít v modelu Identity. Změna klíče model Identity, který se má použít složené klíče není podporován nebo doporučené. Pomocí Identity složený klíč by zahrnuje změnu, jak kód Identity manager komunikuje s modelem, což je nepodporovaný přizpůsobení a nad rámec tohoto dokumentu.

### <a name="changing-tablecolumn-names-and-facets"></a>Změna tabulky nebo sloupce názvy a omezující vlastnosti

Chcete-li změnit názvy tabulek a sloupců, volejte `base.OnModelCreating`a poté přidejte konfigurace přepsat všechny výchozí hodnoty. Chcete-li například změnit název všechny identity tabulky:

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

Všimněte si, že tyto příklady použijte výchozí typy Identity. Pokud použijete typ aplikace, jako například `ApplicationUser` nakonfigurujte typu místo výchozího typu.

Tady je příklad, který změní některé názvy sloupců:

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

Některé typy sloupců databáze může být nakonfigurovaná s určité _omezující vlastnosti_ například maximální délku řetězce povoleny. Tady je příklad, který nastaví sloupce maximální délky pro několik řetězec vlastnosti v modelu:

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

### <a name="mapping-to-a-different-schema"></a>Mapování na jiné schéma.

Schémata mohou chovat jinak v jiné databázi poskytovatelů, ale pro SQL Server, ve výchozím nastavení se vytvořit všechny tabulky ve schématu "dbo". Toto můžete změnit na místo vytváření tabulky v jiném schématu. Příklad:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a>Opožděného načítání

V této části je přidána podpora pro proxy opožděného načítání modelu Identity. Lazy načítání je užitečné, protože umožňuje navigační vlastnosti bez první zajistíte, že jsou načtená.

Typy entit nelze realizovat vhodný pro opožděného načítání několika způsoby, jak je popsáno v [EF základní dokumentace](/ef/core/querying/related-data#lazy-loading). Pro jednoduchost použijeme opožděného načítání proxy, který vyžaduje:

* Instalace [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) balíčku.
* Volání `.UseLazyLoadingProxies()` uvnitř `AddDbContext`.
* Typy entit veřejné s veřejné virtuální navigační vlastnosti.

Tady je příklad volání `.UseLazyLoadingProxies()`:

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Odkazuje zpět na příklady výše pro přidání navigační vlastnosti do typy entit.
