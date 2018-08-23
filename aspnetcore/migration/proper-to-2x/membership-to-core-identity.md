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
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="99e38-103">Migrace z ověřování členství technologie ASP.NET do ASP.NET Core 2.0 Identity</span><span class="sxs-lookup"><span data-stu-id="99e38-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="99e38-104">Podle [Petr Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="99e38-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="99e38-105">Tento článek popisuje migraci schématu databáze pro aplikace v ASP.NET pomocí členství ověřování ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="99e38-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="99e38-106">Tento dokument obsahuje kroky potřebné k migraci schématu databáze pro aplikace založené na členství technologie ASP.NET na schéma databáze používané pro ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="99e38-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="99e38-107">Další informace o migraci z ověřování na základě členství technologie ASP.NET na ASP.NET Identity najdete v tématu [migrovat existující aplikace z členství SQL na ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="99e38-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="99e38-108">Další informace o ASP.NET Core Identity najdete v tématu [Úvod do Identity v ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="99e38-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="99e38-109">Přehled schématu členství</span><span class="sxs-lookup"><span data-stu-id="99e38-109">Review of Membership schema</span></span>

<span data-ttu-id="99e38-110">Před ASP.NET 2.0 byly vývojáři úkol s vytvářením celý proces ověřování a autorizace pro své aplikace.</span><span class="sxs-lookup"><span data-stu-id="99e38-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="99e38-111">S prostředím ASP.NET 2.0 byla zavedena členství, poskytuje řešení často používaný k řešení zabezpečení v rámci aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="99e38-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="99e38-112">Vývojáři byli schopni bootstrap schéma do databáze SQL serveru se teď [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) příkazu.</span><span class="sxs-lookup"><span data-stu-id="99e38-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="99e38-113">Po spuštění tohoto příkazu, byly vytvořeny následující tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="99e38-113">After running this command, the following tables were created in the database.</span></span>

  ![Tabulky členství](identity/_static/membership-tables.png)

<span data-ttu-id="99e38-115">K migraci stávajících aplikací pro ASP.NET Core 2.0 Identity, je potřeba migrovat do tabulky používají nové schéma Identity data v těchto tabulkách.</span><span class="sxs-lookup"><span data-stu-id="99e38-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="99e38-116">ASP.NET Core Identity 2.0 schématu</span><span class="sxs-lookup"><span data-stu-id="99e38-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="99e38-117">Následuje ASP.NET Core 2.0 [Identity](/aspnet/identity/index) Princip zavedené v ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="99e38-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="99e38-118">Když se sdílí se zásadou, implementace mezi rozhraní se liší, dokonce i mezi verzemi ASP.NET Core (viz [migrace ověřování a identita na ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="99e38-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="99e38-119">Nejrychlejší způsob, jak zobrazit schéma pro ASP.NET Core 2.0 Identity je vytvoření nové aplikace ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="99e38-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="99e38-120">Postupujte podle těchto kroků v sadě Visual Studio 2017:</span><span class="sxs-lookup"><span data-stu-id="99e38-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="99e38-121">Vyberte **souboru** > **nové** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="99e38-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="99e38-122">Vytvořte nový **webové aplikace ASP.NET Core** a projekt pojmenujte *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="99e38-122">Create a new **ASP.NET Core Web Application** and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="99e38-123">Vyberte **ASP.NET Core 2.0** v rozevíracím seznamu a pak vyberte **webovou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="99e38-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="99e38-124">Tato šablona vytvoří [Razor Pages](xref:razor-pages/index) aplikace.</span><span class="sxs-lookup"><span data-stu-id="99e38-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="99e38-125">Před kliknutím na tlačítko **OK**, klikněte na tlačítko **změna ověřování**.</span><span class="sxs-lookup"><span data-stu-id="99e38-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="99e38-126">Zvolte **jednotlivé uživatelské účty** pro šablony Identity.</span><span class="sxs-lookup"><span data-stu-id="99e38-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="99e38-127">Nakonec klikněte na tlačítko **OK**, pak **OK**.</span><span class="sxs-lookup"><span data-stu-id="99e38-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="99e38-128">Visual Studio vytvoří projekt pomocí šablony ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="99e38-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="99e38-129">ASP.NET Core 2.0 Identity používá [Entity Framework Core](/ef/core) interakci s databází ukládání ověřovací data.</span><span class="sxs-lookup"><span data-stu-id="99e38-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="99e38-130">Aby nově vytvořené aplikace pro práci existuje musí být databázi pro ukládání těchto dat.</span><span class="sxs-lookup"><span data-stu-id="99e38-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="99e38-131">Po vytvoření nové aplikace, je vytvoření databáze pomocí migrace Entity Framework nejrychlejší způsob, jak zkontrolovat schéma v prostředí s databází.</span><span class="sxs-lookup"><span data-stu-id="99e38-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="99e38-132">Tento proces vytvoří databázi, buď místně nebo někde jinde, která napodobuje tohoto schématu.</span><span class="sxs-lookup"><span data-stu-id="99e38-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="99e38-133">Předchozí dokumentaci pro další informace.</span><span class="sxs-lookup"><span data-stu-id="99e38-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="99e38-134">Chcete-li vytvořit databázi s ASP.NET Core Identity schématu, spusťte `Update-Database` příkaz v sadě Visual Studio **Konzola správce balíčků** okno (PMC)&mdash;se nachází ve **nástroje**  >  **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="99e38-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="99e38-135">PMC umožňuje spouštění příkazů rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="99e38-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="99e38-136">Entity Framework příkazy používají připojovací řetězec k databázi zadané v *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="99e38-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="99e38-137">Následující připojovací řetězec je zaměřen na databázi na *localhost* s názvem *asp net core identity*.</span><span class="sxs-lookup"><span data-stu-id="99e38-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="99e38-138">V tomto nastavení se Entity Framework je nakonfigurován pro použití `DefaultConnection` připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="99e38-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="99e38-139">Tento příkaz vytvoří databázi se schématem a jakékoli data potřebná pro inicializaci aplikace.</span><span class="sxs-lookup"><span data-stu-id="99e38-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="99e38-140">Následující obrázek znázorňuje strukturu tabulky, který je vytvořen v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="99e38-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![Identity tabulky](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="99e38-142">Migrace schématu</span><span class="sxs-lookup"><span data-stu-id="99e38-142">Migrate the schema</span></span>

<span data-ttu-id="99e38-143">Jsou drobné rozdíly ve strukturách tabulky a pole pro členství a ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="99e38-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="99e38-144">Vzor se změnilo podstatně ověřování/autorizace pomocí aplikací pro ASP.NET a ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="99e38-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="99e38-145">Jsou klíčové objekty, které se stále používají s identitou *uživatelé* a *role*.</span><span class="sxs-lookup"><span data-stu-id="99e38-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="99e38-146">Tady jsou mapování tabulky pro *uživatelé*, *role*, a *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="99e38-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="99e38-147">Uživatelé</span><span class="sxs-lookup"><span data-stu-id="99e38-147">Users</span></span>

| <span data-ttu-id="99e38-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="99e38-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="99e38-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="99e38-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="99e38-150">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="99e38-150">**Field Name**</span></span> | <span data-ttu-id="99e38-151">**Typ**</span><span class="sxs-lookup"><span data-stu-id="99e38-151">**Type**</span></span>  |   <span data-ttu-id="99e38-152">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="99e38-152">**Field Name**</span></span> | <span data-ttu-id="99e38-153">**Typ**</span><span class="sxs-lookup"><span data-stu-id="99e38-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="99e38-154">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="99e38-155">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-155">string</span></span>
|`UserName` | <span data-ttu-id="99e38-156">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="99e38-157">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-157">string</span></span>
|`Email` | <span data-ttu-id="99e38-158">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="99e38-159">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="99e38-160">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="99e38-161">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="99e38-162">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="99e38-163">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="99e38-164">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="99e38-165">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="99e38-166">bitové</span><span class="sxs-lookup"><span data-stu-id="99e38-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="99e38-167">bitové</span><span class="sxs-lookup"><span data-stu-id="99e38-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="99e38-168">Ne všechny mapování polí vypadat podobně jako relace 1: 1 z členství pro ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="99e38-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="99e38-169">V předchozí tabulce přebírá výchozí schéma uživatele se členstvím a mapuje na schéma ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="99e38-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="99e38-170">Další vlastní pole, které byly použity pro členství je potřeba namapovat ručně.</span><span class="sxs-lookup"><span data-stu-id="99e38-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="99e38-171">V toto mapování není žádné mapování pro hesla, jako kritéria hesla a heslo soli není migrace mezi dvěma.</span><span class="sxs-lookup"><span data-stu-id="99e38-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="99e38-172">**Doporučujeme ponechat heslo jako hodnota null a pokládat uživatelům resetovat svá hesla.**</span><span class="sxs-lookup"><span data-stu-id="99e38-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="99e38-173">V ASP.NET Core Identity `LockoutEnd` musí být nastavená na datum v budoucnosti, pokud je uživatel uzamčen. To je ukázáno v skript migrace.</span><span class="sxs-lookup"><span data-stu-id="99e38-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="99e38-174">Role</span><span class="sxs-lookup"><span data-stu-id="99e38-174">Roles</span></span>

| <span data-ttu-id="99e38-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="99e38-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="99e38-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="99e38-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="99e38-177">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="99e38-177">**Field Name**</span></span> | <span data-ttu-id="99e38-178">**Typ**</span><span class="sxs-lookup"><span data-stu-id="99e38-178">**Type**</span></span>  |   <span data-ttu-id="99e38-179">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="99e38-179">**Field Name**</span></span> | <span data-ttu-id="99e38-180">**Typ**</span><span class="sxs-lookup"><span data-stu-id="99e38-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="99e38-181">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-181">string</span></span> | `RoleId` | <span data-ttu-id="99e38-182">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-182">string</span></span>
|`Name` | <span data-ttu-id="99e38-183">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-183">string</span></span> | `RoleName` | <span data-ttu-id="99e38-184">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="99e38-185">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="99e38-186">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="99e38-187">Role uživatele</span><span class="sxs-lookup"><span data-stu-id="99e38-187">User Roles</span></span>

| <span data-ttu-id="99e38-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="99e38-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="99e38-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="99e38-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="99e38-190">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="99e38-190">**Field Name**</span></span> | <span data-ttu-id="99e38-191">**Typ**</span><span class="sxs-lookup"><span data-stu-id="99e38-191">**Type**</span></span>  |   <span data-ttu-id="99e38-192">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="99e38-192">**Field Name**</span></span> | <span data-ttu-id="99e38-193">**Typ**</span><span class="sxs-lookup"><span data-stu-id="99e38-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="99e38-194">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-194">string</span></span> | `RoleId` | <span data-ttu-id="99e38-195">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-195">string</span></span>
|`UserId` | <span data-ttu-id="99e38-196">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-196">string</span></span> | `UserId` | <span data-ttu-id="99e38-197">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="99e38-197">string</span></span>

<span data-ttu-id="99e38-198">Při vytváření skript migrace pro odkazují předchozí tabulky mapování *uživatelé* a *role*.</span><span class="sxs-lookup"><span data-stu-id="99e38-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="99e38-199">V následujícím příkladu se předpokládá, že máte dvě databáze na serveru databáze.</span><span class="sxs-lookup"><span data-stu-id="99e38-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="99e38-200">Jedna databáze obsahuje existující členství technologie ASP.NET schéma a data.</span><span class="sxs-lookup"><span data-stu-id="99e38-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="99e38-201">Ostatní databáze byla vytvořena pomocí kroků popsaných dříve.</span><span class="sxs-lookup"><span data-stu-id="99e38-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="99e38-202">Komentáře jsou zahrnuty vložené další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="99e38-202">Comments are included inline for more details.</span></span>

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

<span data-ttu-id="99e38-203">Po dokončení tohoto skriptu aplikace ASP.NET Core Identity dříve vytvořenou se vyplní uživatelů se členstvím.</span><span class="sxs-lookup"><span data-stu-id="99e38-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="99e38-204">Uživatelé potřebují ke změně hesla před přihlášením.</span><span class="sxs-lookup"><span data-stu-id="99e38-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="99e38-205">Pokud systém členství uživatele s uživatelská jména, které se neshoduje jejich e-mailovou adresu, se musí aplikaci vytvořili dříve, aby tuto skutečnost zohlednit změny.</span><span class="sxs-lookup"><span data-stu-id="99e38-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="99e38-206">Výchozí šablony očekává, že `UserName` a `Email` být stejné.</span><span class="sxs-lookup"><span data-stu-id="99e38-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="99e38-207">V situacích, ve kterých charakteristik, je potřeba proces přihlášení by vedla k použití `UserName` místo `Email`.</span><span class="sxs-lookup"><span data-stu-id="99e38-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="99e38-208">V `PageModel` přihlašovací stránky v *Pages\Account\Login.cshtml.cs*, odeberte `[EmailAddress]` atribut z *e-mailu* vlastnost.</span><span class="sxs-lookup"><span data-stu-id="99e38-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="99e38-209">Přejmenujte ho na *uživatelské jméno*.</span><span class="sxs-lookup"><span data-stu-id="99e38-209">Rename it to *UserName*.</span></span> <span data-ttu-id="99e38-210">To vyžaduje, aby změnu bez ohledu na to `EmailAddress` je uvedený v *zobrazení* a *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="99e38-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="99e38-211">Výsledek bude vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="99e38-211">The result looks like the following:</span></span>

 ![Oprava přihlášení](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="99e38-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="99e38-213">Next steps</span></span>

<span data-ttu-id="99e38-214">V tomto kurzu jste zjistili, jak přenést uživatele z členství SQL na ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="99e38-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="99e38-215">Další informace o ASP.NET Core Identity najdete v tématu [Úvod do Identity](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="99e38-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
