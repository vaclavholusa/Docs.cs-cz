---
title: Migrace z ověřování členství technologie ASP.NET na ASP.NET Core 2.0 Identity
author: isaac2004
description: Zjistěte, jak migrovat existující aplikace ASP.NET pomocí ověřování členství ASP.NET Core 2.0 identitě.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 3ec22713997a74b587ef5d18e71a28668a5481e2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274102"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>Migrace z ověřování členství technologie ASP.NET na ASP.NET Core 2.0 Identity

Podle [Isaac Levin](https://isaaclevin.com)

Tento článek ukazuje migraci schématu databáze pro aplikace ASP.NET pomocí ověřování členství ASP.NET Core 2.0 identitě.

> [!NOTE]
> Tento dokument obsahuje kroky potřebné k migraci schématu databáze pro aplikace založené na členství technologie ASP.NET do schématu databáze pro ASP.NET Core Identity. Další informace o migraci z ověřování na základě členství technologie ASP.NET na ASP.NET Identity najdete v tématu [migrovat existující aplikaci ze SQL členství ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). Další informace o ASP.NET Core Identity najdete v tématu [Úvod do identita na ASP.NET Core](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Zkontrolujte schématu členství

Vývojáři byly před aplikaci ASP.NET 2.0, musí s vytvářením celý proces ověřování a autorizace pro svoje aplikace. S prostředím ASP.NET 2.0 byla zavedena členství, že poskytuje řešení, často používaný pro zpracování zabezpečení v rámci aplikace ASP.NET. Vývojáři byli schopni bootstrap schématu do databáze systému SQL Server se teď [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) příkaz. Po spuštění tohoto příkazu, byly vytvořeny následující tabulky v databázi.

  ![Tabulky členství](identity/_static/membership-tables.png)

Pokud chcete migrovat existující aplikace ASP.NET Core 2.0 identitu, je potřeba migrovat do tabulky používané nové schéma Identity data v těchto tabulkách.

## <a name="aspnet-core-identity-20-schema"></a>ASP.NET Core Identity 2.0 schématu

Následuje jádro ASP.NET 2.0 [Identity](/aspnet/identity/index) Princip zavedená v technologii ASP.NET 4.5. Když je sdílená zásada, implementace mezi rozhraní se liší, i mezi verzemi ASP.NET Core (najdete v části [migrovat ověřování a identita na technologii ASP.NET 2.0 základní](xref:migration/1x-to-2x/index)).

Nejrychlejší způsob, jak zobrazit schématu pro ASP.NET Core 2.0 Identity je vytvořit novou aplikaci ASP.NET 2.0 jádra. Postupujte podle těchto kroků v Visual Studio 2017:

* Select **File** > **New** > **Project**.
* Vytvořte novou **webové aplikace ASP.NET Core**a název projektu *CoreIdentitySample*.
* Vyberte **technologii ASP.NET 2.0 základní** v rozevírací nabídce a potom vyberte **webové aplikace**. Tato šablona vytvoří [stránky Razor](xref:razor-pages/index) aplikace. Před kliknutím na tlačítko **OK**, klikněte na tlačítko **změna ověřování**.
* Zvolte **jednotlivé uživatelské účty** pro šablony Identity. Nakonec klikněte na **OK**, pak **OK**. Visual Studio vytvoří projekt pomocí ASP.NET Core Identity šablony.

Základní technologii ASP.NET 2.0 Identity používá [Entity Framework Core](/ef/core) pro interakci s databází ukládání dat ověřování. Aby nově vytvořené aplikace pro práci je potřeba pro uložení dat této databáze. Po vytvoření nové aplikace, je vytvořit databázi pomocí Entity Framework migrace nejrychlejší způsob, jak zkontrolovat na schéma v prostředí s databáze. Tento proces vytvoří databázi, buď místně nebo jinde, která napodobuje schéma této. Předchozí dokumentaci pro další informace.

K vytvoření databáze se schématem ASP.NET Core Identity, spusťte `Update-Database` příkazu v sadě Visual Studio **Konzola správce balíčků** okno (pomocí PMC)&mdash;je umístěn v **nástroje**  >  **Správce balíčků NuGet** > **Konzola správce balíčků**. Pomocí PMC podporuje spuštěné příkazy rozhraní Entity Framework.

Entity Framework příkazy používají připojovací řetězec pro databáze zadaná v *appSettings.JSON určený*. Následující připojovací řetězec databáze cílí na *localhost* s názvem *asp-net-core-identity*. Nastavením této Entity Framework konfigurován pro použití `DefaultConnection` připojovací řetězec.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

Tento příkaz vytvoří databázi zadaným schéma a všechna data potřebná pro inicializaci aplikace. Následující obrázek znázorňuje struktura tabulky, který je vytvořen v předchozích krocích.

   ![Identity tabulky](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>Migraci schématu

Existují jemně lišit v struktury tabulku a pole pro členství a ASP.NET Core Identity. Vzor změnil podstatně pro ověřování/autorizace aplikace ASP.NET a ASP.NET Core. Klíče objekty, které se stále používají s identitou, jsou *uživatelé* a *role*. Tady jsou mapování tabulek pro *uživatelé*, *role*, a *UserRoles*.

### <a name="users"></a>Uživatelé

| *Identity(AspNetUsers)* |   | *Membership(aspnet_Users/aspnet_Membership)* ||
| --- | --- | --- | --- | --- | --- |
| **Název pole** | **Typ**  |   **Název pole** | **Typ**  |
|`Id` | odkazy řetězců | `aspnet_Users.UserId` | odkazy řetězců
|`UserName` | odkazy řetězců | `aspnet_Users.UserName` | odkazy řetězců
|`Email` | odkazy řetězců | `aspnet_Membership.Email` | odkazy řetězců
|`NormalizedUserName` | odkazy řetězců | `aspnet_Users.LoweredUserName` | odkazy řetězců
|`NormalizedEmail` | odkazy řetězců | `aspnet_Membership.LoweredEmail` | odkazy řetězců
|`PhoneNumber` | odkazy řetězců | `aspnet_Users.MobileAlias` | odkazy řetězců
|`LockoutEnabled` | bitové | `aspnet_Membership.IsLockedOut` | bitové

> [!NOTE]
> Ne všechny mapování polí vypadat relace 1: 1 z členství pro ASP.NET Core Identity. V předchozí tabulce trvá výchozí schéma uživatele se členstvím a mapuje na schéma ASP.NET Core Identity. Další vlastní pole, které byly používány pro členství je nutné namapovat ručně. Na toto mapování není žádné mapování pro hesla, jako kritéria heslo a heslo soli není migrace mezi nimi. **Se doporučuje ponechat heslo jako hodnotu null a požádejte uživatelům resetovat jejich hesla.** V identitě ASP.NET Identity Core `LockoutEnd` musí být nastavena na některé datum v budoucnosti, pokud uživatel je uzamčen. To je ukázáno v skript pro migraci.

### <a name="roles"></a>Role

| *Identity(AspNetRoles)* |   | *Membership(aspnet_Roles)* ||
| --- | --- | --- | --- | --- | --- |
| **Název pole** | **Typ**  |   **Název pole** | **Typ**  |
|`Id` | odkazy řetězců | `RoleId` | odkazy řetězců
|`Name` | odkazy řetězců | `RoleName` | odkazy řetězců
|`NormalizedName` | odkazy řetězců | `LoweredRoleName` | odkazy řetězců

### <a name="user-roles"></a>Role uživatele

| *Identity(AspNetUserRoles)* |   | *Membership(aspnet_UsersInRoles)* ||
| --- | --- | --- | --- | --- | --- |
| **Název pole** | **Typ**  |   **Název pole** | **Typ**  |
|`RoleId` | odkazy řetězců | `RoleId` | odkazy řetězců
|`UserId` | odkazy řetězců | `UserId` | odkazy řetězců

Odkazy na předchozí tabulky mapování při vytváření migrace skript pro *uživatelé* a *role*. Následující příklad předpokládá, že máte dvě databáze na serveru databáze. Jedna databáze obsahuje existující schéma členství technologie ASP.NET a data. Ostatní databáze byla vytvořena pomocí kroků popsaných výše. Komentáře jsou zahrnuty vložené další podrobnosti.

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
use aspnetdb

-- INSERT USERS
INSERT INTO coreidentity.dbo.aspnetusers
            (id,
             username,
             normalizedusername,
             passwordhash,
             securitystamp,
             emailconfirmed,
             phonenumber,
             phonenumberconfirmed,
             twofactorenabled,
             lockoutend,
             lockoutenabled,
             accessfailedcount,
             email,
             normalizedemail)
SELECT aspnet_users.userid,
       aspnet_users.username,
       aspnet_users.loweredusername,
       --Creates an empty password since passwords don't map between the two schemas
       '',
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be intialized as a new ID.
       NewID(),
       --EmailConfirmed is set when a new user is created and confirmed via email. Users must have this set during migration to ensure they're able to reset passwords.
       1,
       aspnet_users.mobilealias,
       CASE
         WHEN aspnet_Users.MobileAlias is null THEN 0
         ELSE 1
       END,
       --2-factor Auth likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         --Setting lockout date to time in the future (1000 years)
         WHEN aspnet_membership.islockedout = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_membership.islockedout,
       --AccessFailedAccount is used to track failed logins. This is stored in membership in multiple columns. Setting to 0 arbitrarily.
       0,
       aspnet_membership.email,
       aspnet_membership.loweredemail
FROM   aspnet_users
       LEFT OUTER JOIN aspnet_membership
                    ON aspnet_membership.applicationid =
                       aspnet_users.applicationid
                       AND aspnet_users.userid = aspnet_membership.userid
       LEFT OUTER JOIN coreidentity.dbo.aspnetusers
                    ON aspnet_membership.userid = aspnetusers.id
WHERE  aspnetusers.id IS NULL

-- INSERT ROLES
INSERT INTO coreIdentity.dbo.aspnetroles(id,name)
SELECT roleId,rolename
FROM aspnet_roles;

-- INSERT USER ROLES
INSERT INTO coreidentity.dbo.aspnetuserroles(userid,roleid)
SELECT userid,roleid
FROM aspnet_usersinroles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

Po dokončení skriptu aplikace ASP.NET Core Identity vytvořený naplněný uživatelů se členstvím. Uživatelé musí před přihlášení změnit své heslo.

> [!NOTE]
> Jestliže systém členství, že uživatelé s uživatelská jména, která se neshodovaly e-mailové adresy, je potřeba aplikaci vytvořili dříve, aby tomuto požadavku vyhovělo změny. Výchozí šablony očekává `UserName` a `Email` být stejné. V situacích, ve kterých jsou různé, potřeba upravit tak, aby pomocí procesu přihlášení `UserName` místo `Email`.

V `PageModel` přihlašovací stránky, nacházející se v *Pages\Account\Login.cshtml.cs*, odeberte `[EmailAddress]` atribut z *e-mailu* vlastnost. Přejmenujte jej na *uživatelské jméno*. To vyžaduje změnu bez ohledu na `EmailAddress` je uveden v *zobrazení* a *PageModel*. Výsledek vypadá takto:

 ![Opravené přihlášení](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste zjistili, jak k portu uživatelům z SQL členství ASP.NET Core 2.0 Identity. Další informace o ASP.NET Core Identity najdete v tématu [Úvod k identitě](xref:security/authentication/identity).
