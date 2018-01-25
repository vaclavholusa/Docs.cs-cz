---
title: "Poskytovatelé vlastní úložiště pro ASP.NET Core Identity | Microsoft Docs"
author: ardalis
description: "Postup konfigurace zprostředkovatele vlastního úložiště pro ASP.NET Core Identity."
ms.author: riande
manager: wpickett
ms.date: 05/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: f0953ad5d9f1bfa92ecc5169d9a211ce6b8cda8f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>Poskytovatelé vlastní úložiště pro ASP.NET Core Identity

Podle [Steve Smith](https://ardalis.com/)

Identita ASP.NET Core je rozšiřitelný systému, který umožňuje vytvořit vlastního poskytovatele úložiště a připojte ho k vaší aplikace. Toto téma popisuje postup vytvoření poskytovatele vlastní úložiště pro ASP.NET Core Identity. Popisuje důležité koncepty pro vytvoření vlastního zprostředkovatele úložiště, ale není podrobný návod.

[Zobrazení nebo stažení ukázky z webu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).

## <a name="introduction"></a>Úvod

Ve výchozím nastavení systém ASP.NET Core Identity ukládá informace o uživateli v databázi systému SQL Server pomocí Entity Framework Core. Pro velký počet aplikací tento postup funguje dobře. Použití různých trvalost mechanismus nebo schéma dat ale můžou upřednostňovat. Příklad:

* Používáte [Azure Table Storage](https://docs.microsoft.com/azure/storage/) nebo jiného úložiště dat.
* Databázové tabulky mít jiné struktury. 
* Můžete použít různé datové přístup přístup, jako například [Dapper](https://github.com/StackExchange/Dapper). 

V každé z těchto případech můžete napsat vlastní zprostředkovatele pro mechanizmus pro ukládání a zařadit tohoto zprostředkovatele do vaší aplikace.

Jádro ASP.NET Identity je součástí šablony projektů v sadě Visual Studio s parametrem "Jednotlivé uživatelské účty".

Pokud používáte rozhraní příkazového řádku .NET Core, přidejte `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>Architektura ASP.NET Core Identity

ASP.NET Core Identity se skládá z třídy jako správce a úložiště. *Správci* jsou základní třídy, které vývojář aplikace používá k provádění operací, jako je například vytváření Identity user. *Úložiště* jsou třídy nižší úrovně, které určují, jak jsou entity, jako jsou uživatelé a role, nastavené jako trvalé. Postupujte podle úložiště [použitému vzoru](http://deviq.com/repository-pattern/) a jsou úzce kombinaci s mechanismus trvalosti. Správci jsou odpojené od úložiště, což znamená, že nahradíte mechanismu trvalosti beze změny kódu aplikace (s výjimkou konfigurace).

Následující diagram znázorňuje, jak webová aplikace komunikuje s správce, zatímco úložiště interakci s vrstva přístupu k datům.

![Aplikace ASP.NET Core fungovat s manažery (například 'Nastavení UserManager', 'RoleManager'). Správci pracují s úložiště (například "úložiště UserStore'), které komunikují s zdroji dat pomocí knihovny jako Entity Framework Core.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

Pokud chcete vytvořit vlastního poskytovatele úložiště, vytvořte zdroj dat, vrstva přístupu k datům a třídy úložiště, které pracují s Tato vrstva přístupu k datům (zelený šedé polí a v diagramu výše). Není nutné přizpůsobit vybraných manažerů nebo váš kód aplikace, která komunikuje s nimi (políčka výše blue).

Při vytváření novou instanci třídy `UserManager` nebo `RoleManager` zadejte typ třídy uživatelů a předat jako argument instance třídy úložiště. Tento přístup umožňuje připojte vaše vlastní třídy ASP.NET Core. 

[Znovu nakonfigurujte aplikaci pro použití nového poskytovatele úložiště](#reconfigure-app-to-use-new-storage-provider) ukazuje, jak vytvořit instanci `UserManager` a `RoleManager` s vlastní úložiště.

## <a name="aspnet-core-identity-stores-data-types"></a>Jádro ASP.NET Identity ukládá datové typy

[ASP.NET Core Identity](https://github.com/aspnet/identity) datové typy jsou podrobně popsané v následujících částech:

### <a name="users"></a>Uživatelé

Registrovaní uživatelé vašeho webu. [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser) typ může rozšířit nebo používat jako příklad pro vlastní vlastního typu. Nemusíte dědit z konkrétního typu implementovat vlastní řešení úložiště vlastní identitu.

### <a name="user-claims"></a>Deklarace identity uživatelů

Sada příkazů (nebo [deklarace identity](https://docs.microsoft.com//dotnet/api/system.security.claims.claim)) o uživatele, který reprezentuje identitu uživatele. Můžete povolit větší výraz identitu uživatele, než lze dosáhnout pomocí rolí.

### <a name="user-logins"></a>Přihlášení uživatele

Informace o zprostředkovateli externího ověřování (jako je Facebook nebo účtu Microsoft) pro použití při přihlášení uživatele. [Příklad](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>Role

Skupiny autorizaci pro svůj web. Obsahuje název role a Id role (například "Admin" nebo "Zaměstnanec"). [Příklad](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>Vrstva přístupu k datům

Toto téma předpokládá, že jste obeznámeni s trvalost mechanismus, který chcete použít a jak vytvořit entit pro tento mechanismus. Toto téma neposkytuje informace o tom, jak vytvořit úložiště nebo data přístupové třídy; poskytuje několik návrhů o rozhodnutí o návrhu při práci s ASP.NET Core Identity.

Máte spoustu volnost při navrhování vrstva přístupu k datům pro zprostředkovatele vlastní úložiště. Potřebujete vytvořit trvalost mechanismy pro funkce, které chcete používat ve vaší aplikaci. Například pokud nepoužíváte role ve vaší aplikaci, nepotřebujete vytvořit úložiště pro role nebo přiřazení role uživatelů. Technologie a existující infrastruktura může vyžadovat strukturu, která je příliš neliší od výchozí implementaci ASP.NET Core Identity. Přístup k vaší Datová vrstva poskytnete logiku pro práci s strukturu vaší implementace úložiště.

Vrstva přístupu k datům poskytuje logiku pro uložení dat z ASP.NET Core Identity ke zdroji dat. Vrstva přístupu k datům pro poskytovatele vlastní úložiště může obsahovat následující třídy k ukládání informací o uživateli a role.

### <a name="context-class"></a>context – třída

Zapouzdří informace pro připojení k vaší mechanismu trvalosti a spouštět dotazy. Několik datových tříd vyžadují instance této třídy, většinou poskytuje prostřednictvím vkládání závislostí. [Příklad](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).

### <a name="user-storage"></a>Úložiště uživatele

Ukládá a načte informace o uživateli (například uživatelské jméno a heslo hash). [Příklad](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>Role úložiště

Ukládá a načte informace o rolích (například název role). [Příklad](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>Úložiště objektu UserClaims.

Ukládá a načte informace o deklaraci identity uživatele (například typ deklarace identity a hodnota). [Příklad](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>Sada úložiště

Ukládá a načte přihlašovací informace uživatele (například externí zprostředkovatel ověřování). [Příklad](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>UserRole Storage

Ukládá a načte role, které jsou přiřazeny k uživatelů, kteří. [Příklad](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

**TIP:** pouze implementace třídy, který chcete používat ve vaší aplikaci.

V datových tříd přístup zadejte kód pro provádění operací s daty pro váš mechanismus trvalosti. Například v rámci vlastní zprostředkovatele, může mít následující kód k vytvoření nového uživatele v *ukládání* třídy:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

Implementační logika pro vytvoření uživatele se v ``_usersTable.CreateAsync`` metoda, viz následující obrázek.

## <a name="customize-the-user-class"></a>Přizpůsobit třídu uživatelů

Při implementaci zprostředkovatele úložiště, vytvořte uživatele třídu, která je ekvivalentní [ `IdentityUser` třída](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser).

Minimálně musí zahrnovat třídě user `Id` a `UserName` vlastnost.

`IdentityUser` Třída definuje vlastnosti, ``UserManager`` volání při provádění požadované operace. Výchozí typ `Id` vlastnost je řetězec, ale může dědit vlastnosti z `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` a zadejte jiný typ. Rozhraní framework očekává implementace úložiště pro zpracování konverze datových typů.

## <a name="customize-the-user-store"></a>Přizpůsobení úložiště uživatele

Vytvoření `UserStore` třídu, která poskytuje metody pro všechny operace data na uživatele. Tato třída je ekvivalentní [objektu UserStore<TUser> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) třídy. Ve vaší `UserStore` třídy, implementovat `IUserStore<TUser>` a volitelné rozhraní vyžaduje. Vyberete které volitelné rozhraní k implementaci na základě u funkce poskytované ve vaší aplikaci.

### <a name="optional-interfaces"></a>Volitelné rozhraní

- IUserRoleStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1
- IUserClaimStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1
- IUserPasswordStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1
- IUserSecurityStampStore<!-- make these all links and remove / -->
- IUserEmailStore
- IPhoneNumberStore
- IQueryableUserStore
- IUserLoginStore
- IUserTwoFactorStore
- IUserLockoutStore

Volitelné rozhraní dědí `IUserStore`. Můžete zobrazit částečně implementováno ukázkového uživatele uložit [zde](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).

V rámci `UserStore` třídy, použijte přístupové třídy dat, které jste vytvořili k provádění operací. Tyto jsou předána pomocí vkládání závislostí. Například v systému SQL Server s Dapper implementací `UserStore` třída má `CreateAsync` metoda, která používá instanci `DapperUsersTable` vložit nový záznam:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Rozhraní k implementaci při přizpůsobení úložiště uživatele

- **IUserStore**  
 [IUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserstore-1) rozhraní je pouze rozhraní musí implementovat v úložišti uživatele. Definuje metody pro vytváření, aktualizaci, odstranění a načítání uživatelů.
- **IUserClaimStore**  
 [IUserClaimStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1) rozhraní definuje metody, implementovat povolující deklarace identity uživatele. Obsahuje metody pro přidání, odebrání a načítání deklarace identity uživatelů.
- **IUserLoginStore**  
 [IUserLoginStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserloginstore-1) definuje metody, implementovat povolit externí zprostředkovatele ověřování. Obsahuje metody pro přidání, odebrání a načítání přihlášení uživatele a metodu pro načtení uživatele podle přihlašovacích údajů.
- **IUserRoleStore**  
 [IUserRoleStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1) rozhraní definuje metody, implementaci pro mapování uživatele k roli. Obsahuje metody pro přidání, odebrání a načíst role uživatele a umožňuje kontrolu, pokud uživatel je přiřazena k roli.
- **IUserPasswordStore**  
 [IUserPasswordStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) rozhraní definuje metody, implementovat udržení hash hesla. Obsahuje metody pro získání a nastavení hodnoty hash hesla a metodu, která označuje, zda má uživatel nastavit heslo.
- **IUserSecurityStampStore**  
 [IUserSecurityStampStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) rozhraní definuje metody, implementovat používat razítko zabezpečení pro označující, zda došlo ke změně informací o účtu uživatele. Razítko se aktualizuje, když uživatel změní heslo, nebo přidá nebo odebere přihlášení. Obsahuje metody pro získání a nastavení razítko zabezpečení.
- **IUserTwoFactorStore**  
 [IUserTwoFactorStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) rozhraní definuje metody, implementovat pro podporu dvoufaktorové ověřování. Obsahuje metody pro získání a nastavení, zda je pro uživatele povoleno dvoufaktorové ověřování.
- **IUserPhoneNumberStore**  
 [IUserPhoneNumberStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) rozhraní definuje metody, implementaci pro uložení telefonních čísel uživatele. Obsahuje metody pro získání a nastavení telefonní číslo a zda telefonní číslo je potvrzeno.
- **IUserEmailStore**  
 [IUserEmailStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuseremailstore-1) rozhraní definuje metody, implementaci pro uložení e-mailové adresy uživatele. Obsahuje metody pro získání a nastavení e-mailovou adresu a zda e-mail je potvrzen.
- **IUserLockoutStore**  
 [IUserLockoutStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) rozhraní definuje metody, implementaci pro uložení informací o uzamčení účtu. Obsahuje metody pro sledování neúspěšných pokusů o přístup a uzamčení.
- **IQueryableUserStore**  
 [IQueryableUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) rozhraní definuje členy implementace zajistit dotazovatelné úložiště uživatelů.

Můžete implementovat jenom na rozhraní, které jsou potřeba v aplikaci. Příklad:

```csharp
public class UserStore : IUserStore<IdentityUser>,
                         IUserClaimStore<IdentityUser>,
                         IUserLoginStore<IdentityUser>,
                         IUserRoleStore<IdentityUser>,
                         IUserPasswordStore<IdentityUser>,
                         IUserSecurityStampStore<IdentityUser>
{
    // interface implementations not shown
}
```

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin a IdentityUserRole

``Microsoft.AspNet.Identity.EntityFramework`` Obor názvů obsahuje implementace [IdentityUserClaim](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin), a [IdentityUserRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) třídy. Pokud používáte tyto funkce, můžete vytvořit vlastní verzích tyto třídy a definujte vlastnosti pro vaši aplikaci. V některých případech je však efektivnější zatížení tyto entity do paměti při provádění základních operací (například přidáním nebo odebráním deklarace identity uživatele). Třídy úložiště back-end místo toho můžete tyto operace přímo na zdroj dat provést. Například ``UserStore.GetClaimsAsync`` můžete volat metodu ``userClaimTable.FindByUserId(user.Id)`` metodu při spuštění dotazu na, která přímo tabulky a vrátí seznam deklarací identity.

## <a name="customize-the-role-class"></a>Přizpůsobení role – třída

Při implementaci zprostředkovatele úložiště role, můžete vytvořit vlastní role typu. Jej není nutné implementovat určité rozhraní, ale musí mít `Id` a obvykle bude mít `Name` vlastnost.

Toto je ukázková třída role:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>Přizpůsobení úložiště rolí

Můžete vytvořit ``RoleStore`` třídu, která poskytuje metody pro všechny operace data na rolích. Tato třída je ekvivalentní [úložiště RoleStore<TRole> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) třídy. V `RoleStore` implementovat třídu, ``IRoleStore<TRole>`` a volitelně ``IQueryableRoleStore<TRole>`` rozhraní.

- **IRoleStore&lt;TRole&gt;**  
 [Úložiště IRoleStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.irolestore-1) rozhraní definuje metody k implementaci ve třídě roli úložiště. Obsahuje metody pro vytváření, aktualizaci, odstranění a načítání rolí.
- **RoleStore&lt;TRole&gt;**  
 Chcete-li přizpůsobit `RoleStore`, vytvořte třídu, která implementuje `IRoleStore` rozhraní. 

## <a name="reconfigure-app-to-use-new-storage-provider"></a>Znovu nakonfigurujte aplikaci pro použití nového poskytovatele úložiště

Když naimplementujete zprostředkovatele úložiště, nakonfigurujete aplikaci používat. Pokud vaše aplikace používá výchozího zprostředkovatele, nahraďte ji metodou vlastního poskytovatele.

1. Odeberte `Microsoft.AspNetCore.EntityFramework.Identity` balíček NuGet.
1. Pokud zprostředkovatel úložiště se nachází v samostatné projektu nebo balíček, přidejte odkaz na ni.
1. Nahraďte všechny odkazy na `Microsoft.AspNetCore.EntityFramework.Identity` s pomocí příkazu pro obor názvů poskytovatele úložiště.
1. V ``ConfigureServices`` metoda, změny `AddIdentity` metodu použít vlastní typy. Můžete vytvořit vlastní metody rozšíření pro tento účel. V tématu [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) příklad.
1. Pokud používáte role, aktualizovat `RoleManager` používat vaše `RoleStore` třídy.
1. Aktualizujte připojovací řetězec a přihlašovací údaje ke konfiguraci vaší aplikace.

Příklad:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add identity types
    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddDefaultTokenProviders();

    // Identity Services
    services.AddTransient<IUserStore<ApplicationUser>, CustomUserStore>();
    services.AddTransient<IRoleStore<ApplicationRole>, CustomRoleStore>();
    string connectionString = Configuration.GetConnectionString("DefaultConnection");
    services.AddTransient<SqlConnection>(e => new SqlConnection(connectionString));
    services.AddTransient<DapperUsersTable>();

    // additional configuration
}
```

## <a name="references"></a>Odkazy

- [Poskytovatelé vlastní úložiště pro identitu ASP.NET](https://docs.microsoft.com/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- [ASP.NET Core Identity](https://github.com/aspnet/identity) -toto úložiště obsahuje odkazy na komunity udržovat poskytovatelé úložiště.
