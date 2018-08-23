---
title: Migrace z ověřování členství technologie ASP.NET do ASP.NET Core 2.0 Identity
author: isaac2004
description: Zjistěte, jak migrovat existující aplikace v ASP.NET pomocí členství ověřování ASP.NET Core 2.0 Identity.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 82158ec500151a0bb61fb1da55a53684367d9a4e
ms.sourcegitcommit: 2e054638b69f2b14f6d67d9fa3664999172ee1b2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/14/2018
ms.locfileid: "41753417"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>Migrace z ověřování členství technologie ASP.NET do ASP.NET Core 2.0 Identity

Podle [Petr Levin](https://isaaclevin.com)

Tento článek popisuje migraci schématu databáze pro aplikace v ASP.NET pomocí členství ověřování ASP.NET Core 2.0 Identity.

> [!NOTE]
> Tento dokument obsahuje kroky potřebné k migraci schématu databáze pro aplikace založené na členství technologie ASP.NET na schéma databáze používané pro ASP.NET Core Identity. Další informace o migraci z ověřování na základě členství technologie ASP.NET na ASP.NET Identity najdete v tématu [migrovat existující aplikace z členství SQL na ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). Další informace o ASP.NET Core Identity najdete v tématu [Úvod do Identity v ASP.NET Core](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Přehled schématu členství

Před ASP.NET 2.0 byly vývojáři úkol s vytvářením celý proces ověřování a autorizace pro své aplikace. S prostředím ASP.NET 2.0 byla zavedena členství, poskytuje řešení často používaný k řešení zabezpečení v rámci aplikace ASP.NET. Vývojáři byli schopni bootstrap schéma do databáze SQL serveru se teď [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) příkazu. Po spuštění tohoto příkazu, byly vytvořeny následující tabulky v databázi.

  ![Tabulky členství](identity/_static/membership-tables.png)

K migraci stávajících aplikací pro ASP.NET Core 2.0 Identity, je potřeba migrovat do tabulky používají nové schéma Identity data v těchto tabulkách.

## <a name="aspnet-core-identity-20-schema"></a>ASP.NET Core Identity 2.0 schématu

Následuje ASP.NET Core 2.0 [Identity](/aspnet/identity/index) Princip zavedené v ASP.NET 4.5. Když se sdílí se zásadou, implementace mezi rozhraní se liší, dokonce i mezi verzemi ASP.NET Core (viz [migrace ověřování a identita na ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).

Nejrychlejší způsob, jak zobrazit schéma pro ASP.NET Core 2.0 Identity je vytvoření nové aplikace ASP.NET Core 2.0. Postupujte podle těchto kroků v sadě Visual Studio 2017:

* Vyberte **souboru** > **nové** > **projektu**.
* Vytvořte nový **webové aplikace ASP.NET Core** a projekt pojmenujte *CoreIdentitySample*.
* Vyberte **ASP.NET Core 2.0** v rozevíracím seznamu a pak vyberte **webovou aplikaci**. Tato šablona vytvoří [Razor Pages](xref:razor-pages/index) aplikace. Před kliknutím na tlačítko **OK**, klikněte na tlačítko **změna ověřování**.
* Zvolte **jednotlivé uživatelské účty** pro šablony Identity. Nakonec klikněte na tlačítko **OK**, pak **OK**. Visual Studio vytvoří projekt pomocí šablony ASP.NET Core Identity.

ASP.NET Core 2.0 Identity používá [Entity Framework Core](/ef/core) interakci s databází ukládání ověřovací data. Aby nově vytvořené aplikace pro práci existuje musí být databázi pro ukládání těchto dat. Po vytvoření nové aplikace, je vytvoření databáze pomocí migrace Entity Framework nejrychlejší způsob, jak zkontrolovat schéma v prostředí s databází. Tento proces vytvoří databázi, buď místně nebo někde jinde, která napodobuje tohoto schématu. Předchozí dokumentaci pro další informace.

Chcete-li vytvořit databázi s ASP.NET Core Identity schématu, spusťte `Update-Database` příkaz v sadě Visual Studio **Konzola správce balíčků** okno (PMC)&mdash;se nachází ve **nástroje**  >  **Správce balíčků NuGet** > **Konzola správce balíčků**. PMC umožňuje spouštění příkazů rozhraní Entity Framework.

Entity Framework příkazy používají připojovací řetězec k databázi zadané v *appsettings.json*. Následující připojovací řetězec je zaměřen na databázi na *localhost* s názvem *asp net core identity*. V tomto nastavení se Entity Framework je nakonfigurován pro použití `DefaultConnection` připojovací řetězec.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

Tento příkaz vytvoří databázi se schématem a jakékoli data potřebná pro inicializaci aplikace. Následující obrázek znázorňuje strukturu tabulky, který je vytvořen v předchozích krocích.

   ![Identity tabulky](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>Migrace schématu

Jsou drobné rozdíly ve strukturách tabulky a pole pro členství a ASP.NET Core Identity. Vzor se změnilo podstatně ověřování/autorizace pomocí aplikací pro ASP.NET a ASP.NET Core. Jsou klíčové objekty, které se stále používají s identitou *uživatelé* a *role*. Tady jsou mapování tabulky pro *uživatelé*, *role*, a *UserRoles*.

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
> Ne všechny mapování polí vypadat podobně jako relace 1: 1 z členství pro ASP.NET Core Identity. V předchozí tabulce přebírá výchozí schéma uživatele se členstvím a mapuje na schéma ASP.NET Core Identity. Další vlastní pole, které byly použity pro členství je potřeba namapovat ručně. V toto mapování není žádné mapování pro hesla, jako kritéria hesla a heslo soli není migrace mezi dvěma. **Doporučujeme ponechat heslo jako hodnota null a pokládat uživatelům resetovat svá hesla.** V ASP.NET Core Identity `LockoutEnd` musí být nastavená na datum v budoucnosti, pokud je uživatel uzamčen. To je ukázáno v skript migrace.

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

Při vytváření skript migrace pro odkazují předchozí tabulky mapování *uživatelé* a *role*. V následujícím příkladu se předpokládá, že máte dvě databáze na serveru databáze. Jedna databáze obsahuje existující členství technologie ASP.NET schéma a data. Ostatní databáze byla vytvořena pomocí kroků popsaných dříve. Komentáře jsou zahrnuty vložené další podrobnosti.

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
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be initialized as a new ID.
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

Po dokončení tohoto skriptu aplikace ASP.NET Core Identity dříve vytvořenou se vyplní uživatelů se členstvím. Uživatelé potřebují ke změně hesla před přihlášením.

> [!NOTE]
> Pokud systém členství uživatele s uživatelská jména, které se neshoduje jejich e-mailovou adresu, se musí aplikaci vytvořili dříve, aby tuto skutečnost zohlednit změny. Výchozí šablony očekává, že `UserName` a `Email` být stejné. V situacích, ve kterých charakteristik, je potřeba proces přihlášení by vedla k použití `UserName` místo `Email`.

V `PageModel` přihlašovací stránky v *Pages\Account\Login.cshtml.cs*, odeberte `[EmailAddress]` atribut z *e-mailu* vlastnost. Přejmenujte ho na *uživatelské jméno*. To vyžaduje, aby změnu bez ohledu na to `EmailAddress` je uvedený v *zobrazení* a *PageModel*. Výsledek bude vypadat nějak takto:

 ![Oprava přihlášení](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste zjistili, jak přenést uživatele z členství SQL na ASP.NET Core 2.0 Identity. Další informace o ASP.NET Core Identity najdete v tématu [Úvod do Identity](xref:security/authentication/identity).
