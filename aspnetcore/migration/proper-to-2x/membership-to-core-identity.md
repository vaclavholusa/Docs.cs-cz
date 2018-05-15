---
title: Migrace z ověřování členství technologie ASP.NET na ASP.NET Core 2.0 Identity
author: isaac2004
description: Zjistěte, jak migrovat existující aplikace ASP.NET pomocí ověřování členství ASP.NET Core 2.0 identitě.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: f0d1099bfda01d036831350e0888ae3830ad3d58
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="51dd7-103">Migrace z ověřování členství technologie ASP.NET na ASP.NET Core 2.0 Identity</span><span class="sxs-lookup"><span data-stu-id="51dd7-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="51dd7-104">Podle [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="51dd7-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="51dd7-105">Tento článek ukazuje migraci schématu databáze pro aplikace ASP.NET pomocí ověřování členství ASP.NET Core 2.0 identitě.</span><span class="sxs-lookup"><span data-stu-id="51dd7-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="51dd7-106">Tento dokument obsahuje kroky potřebné k migraci schématu databáze pro aplikace založené na členství technologie ASP.NET do schématu databáze pro ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="51dd7-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="51dd7-107">Další informace o migraci z ověřování na základě členství technologie ASP.NET na ASP.NET Identity najdete v tématu [migrovat existující aplikaci ze SQL členství ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="51dd7-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="51dd7-108">Další informace o ASP.NET Core Identity najdete v tématu [Úvod do identita na ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="51dd7-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="51dd7-109">Zkontrolujte schématu členství</span><span class="sxs-lookup"><span data-stu-id="51dd7-109">Review of Membership schema</span></span>

<span data-ttu-id="51dd7-110">Vývojáři byly před aplikaci ASP.NET 2.0, musí s vytvářením celý proces ověřování a autorizace pro svoje aplikace.</span><span class="sxs-lookup"><span data-stu-id="51dd7-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="51dd7-111">S prostředím ASP.NET 2.0 byla zavedena členství, že poskytuje řešení, často používaný pro zpracování zabezpečení v rámci aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="51dd7-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="51dd7-112">Vývojáři byli schopni bootstrap schématu do databáze systému SQL Server se teď [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) příkaz.</span><span class="sxs-lookup"><span data-stu-id="51dd7-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="51dd7-113">Po spuštění tohoto příkazu, byly vytvořeny následující tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="51dd7-113">After running this command, the following tables were created in the database.</span></span>

  ![Tabulky členství](identity/_static/membership-tables.png)

<span data-ttu-id="51dd7-115">Pokud chcete migrovat existující aplikace ASP.NET Core 2.0 identitu, je potřeba migrovat do tabulky používané nové schéma Identity data v těchto tabulkách.</span><span class="sxs-lookup"><span data-stu-id="51dd7-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="51dd7-116">ASP.NET Core Identity 2.0 schématu</span><span class="sxs-lookup"><span data-stu-id="51dd7-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="51dd7-117">Následuje jádro ASP.NET 2.0 [Identity](/aspnet/identity/index) Princip zavedená v technologii ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="51dd7-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="51dd7-118">Když je sdílená zásada, implementace mezi rozhraní se liší, i mezi verzemi ASP.NET Core (najdete v části [migrovat ověřování a identita na technologii ASP.NET 2.0 základní](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="51dd7-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="51dd7-119">Nejrychlejší způsob, jak zobrazit schématu pro ASP.NET Core 2.0 Identity je vytvořit novou aplikaci ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="51dd7-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="51dd7-120">Postupujte podle těchto kroků v Visual Studio 2017:</span><span class="sxs-lookup"><span data-stu-id="51dd7-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="51dd7-121">Select **File** > **New** > **Project**.</span><span class="sxs-lookup"><span data-stu-id="51dd7-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="51dd7-122">Vytvořte novou **webové aplikace ASP.NET Core**a název projektu *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="51dd7-122">Create a new **ASP.NET Core Web Application**, and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="51dd7-123">Vyberte **technologii ASP.NET 2.0 základní** v rozevírací nabídce a potom vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="51dd7-123">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span> <span data-ttu-id="51dd7-124">Tato šablona vytvoří [stránky Razor](xref:mvc/razor-pages/index) aplikace.</span><span class="sxs-lookup"><span data-stu-id="51dd7-124">This template produces a [Razor Pages](xref:mvc/razor-pages/index) app.</span></span> <span data-ttu-id="51dd7-125">Před kliknutím na tlačítko **OK**, klikněte na tlačítko **změna ověřování**.</span><span class="sxs-lookup"><span data-stu-id="51dd7-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="51dd7-126">Zvolte **jednotlivé uživatelské účty** pro šablony Identity.</span><span class="sxs-lookup"><span data-stu-id="51dd7-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="51dd7-127">Nakonec klikněte na **OK**, pak **OK**.</span><span class="sxs-lookup"><span data-stu-id="51dd7-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="51dd7-128">Visual Studio vytvoří projekt pomocí ASP.NET Core Identity šablony.</span><span class="sxs-lookup"><span data-stu-id="51dd7-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="51dd7-129">Základní technologii ASP.NET 2.0 Identity používá [Entity Framework Core](/ef/core) pro interakci s databází ukládání dat ověřování.</span><span class="sxs-lookup"><span data-stu-id="51dd7-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="51dd7-130">Aby nově vytvořené aplikace pro práci je potřeba pro uložení dat této databáze.</span><span class="sxs-lookup"><span data-stu-id="51dd7-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="51dd7-131">Po vytvoření nové aplikace, je vytvořit databázi pomocí Entity Framework migrace nejrychlejší způsob, jak zkontrolovat na schéma v prostředí s databáze.</span><span class="sxs-lookup"><span data-stu-id="51dd7-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="51dd7-132">Tento proces vytvoří databázi, buď místně nebo jinde, která napodobuje schéma této.</span><span class="sxs-lookup"><span data-stu-id="51dd7-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="51dd7-133">Předchozí dokumentaci pro další informace.</span><span class="sxs-lookup"><span data-stu-id="51dd7-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="51dd7-134">K vytvoření databáze se schématem ASP.NET Core Identity, spusťte `Update-Database` příkazu v sadě Visual Studio **Konzola správce balíčků** okno (pomocí PMC)&mdash;je umístěn v **nástroje**  >  **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="51dd7-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="51dd7-135">Pomocí PMC podporuje spuštěné příkazy rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="51dd7-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="51dd7-136">Entity Framework příkazy používají připojovací řetězec pro databáze zadaná v *appSettings.JSON určený*.</span><span class="sxs-lookup"><span data-stu-id="51dd7-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="51dd7-137">Následující připojovací řetězec databáze cílí na *localhost* s názvem *asp-net-core-identity*.</span><span class="sxs-lookup"><span data-stu-id="51dd7-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="51dd7-138">Nastavením této Entity Framework konfigurován pro použití `DefaultConnection` připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="51dd7-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="51dd7-139">Tento příkaz vytvoří databázi zadaným schéma a všechna data potřebná pro inicializaci aplikace.</span><span class="sxs-lookup"><span data-stu-id="51dd7-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="51dd7-140">Následující obrázek znázorňuje struktura tabulky, který je vytvořen v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="51dd7-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![Identity tabulky](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="51dd7-142">Migraci schématu</span><span class="sxs-lookup"><span data-stu-id="51dd7-142">Migrate the schema</span></span>

<span data-ttu-id="51dd7-143">Existují jemně lišit v struktury tabulku a pole pro členství a ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="51dd7-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="51dd7-144">Vzor změnil podstatně pro ověřování/autorizace aplikace ASP.NET a ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="51dd7-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="51dd7-145">Klíče objekty, které se stále používají s identitou, jsou *uživatelé* a *role*.</span><span class="sxs-lookup"><span data-stu-id="51dd7-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="51dd7-146">Tady jsou mapování tabulek pro *uživatelé*, *role*, a *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="51dd7-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="51dd7-147">Uživatelé</span><span class="sxs-lookup"><span data-stu-id="51dd7-147">Users</span></span>

| <span data-ttu-id="51dd7-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="51dd7-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="51dd7-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="51dd7-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="51dd7-150">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="51dd7-150">**Field Name**</span></span> | <span data-ttu-id="51dd7-151">**Typ**</span><span class="sxs-lookup"><span data-stu-id="51dd7-151">**Type**</span></span>  |   <span data-ttu-id="51dd7-152">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="51dd7-152">**Field Name**</span></span> | <span data-ttu-id="51dd7-153">**Typ**</span><span class="sxs-lookup"><span data-stu-id="51dd7-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="51dd7-154">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="51dd7-155">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-155">string</span></span>
|`UserName` | <span data-ttu-id="51dd7-156">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="51dd7-157">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-157">string</span></span>
|`Email` | <span data-ttu-id="51dd7-158">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="51dd7-159">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="51dd7-160">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="51dd7-161">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="51dd7-162">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="51dd7-163">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="51dd7-164">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="51dd7-165">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="51dd7-166">bitové</span><span class="sxs-lookup"><span data-stu-id="51dd7-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="51dd7-167">bitové</span><span class="sxs-lookup"><span data-stu-id="51dd7-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="51dd7-168">Ne všechny mapování polí vypadat relace 1: 1 z členství pro ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="51dd7-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="51dd7-169">V předchozí tabulce trvá výchozí schéma uživatele se členstvím a mapuje na schéma ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="51dd7-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="51dd7-170">Další vlastní pole, které byly používány pro členství je nutné namapovat ručně.</span><span class="sxs-lookup"><span data-stu-id="51dd7-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="51dd7-171">Na toto mapování není žádné mapování pro hesla, jako kritéria heslo a heslo soli není migrace mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="51dd7-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="51dd7-172">**Se doporučuje ponechat heslo jako hodnotu null a požádejte uživatelům resetovat jejich hesla.**</span><span class="sxs-lookup"><span data-stu-id="51dd7-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="51dd7-173">V identitě ASP.NET Identity Core `LockoutEnd` musí být nastavena na některé datum v budoucnosti, pokud uživatel je uzamčen. To je ukázáno v skript pro migraci.</span><span class="sxs-lookup"><span data-stu-id="51dd7-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="51dd7-174">Role</span><span class="sxs-lookup"><span data-stu-id="51dd7-174">Roles</span></span>

| <span data-ttu-id="51dd7-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="51dd7-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="51dd7-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="51dd7-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="51dd7-177">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="51dd7-177">**Field Name**</span></span> | <span data-ttu-id="51dd7-178">**Typ**</span><span class="sxs-lookup"><span data-stu-id="51dd7-178">**Type**</span></span>  |   <span data-ttu-id="51dd7-179">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="51dd7-179">**Field Name**</span></span> | <span data-ttu-id="51dd7-180">**Typ**</span><span class="sxs-lookup"><span data-stu-id="51dd7-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="51dd7-181">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-181">string</span></span> | `RoleId` | <span data-ttu-id="51dd7-182">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-182">string</span></span>
|`Name` | <span data-ttu-id="51dd7-183">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-183">string</span></span> | `RoleName` | <span data-ttu-id="51dd7-184">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="51dd7-185">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="51dd7-186">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="51dd7-187">Role uživatele</span><span class="sxs-lookup"><span data-stu-id="51dd7-187">User Roles</span></span>

| <span data-ttu-id="51dd7-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="51dd7-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="51dd7-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="51dd7-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="51dd7-190">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="51dd7-190">**Field Name**</span></span> | <span data-ttu-id="51dd7-191">**Typ**</span><span class="sxs-lookup"><span data-stu-id="51dd7-191">**Type**</span></span>  |   <span data-ttu-id="51dd7-192">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="51dd7-192">**Field Name**</span></span> | <span data-ttu-id="51dd7-193">**Typ**</span><span class="sxs-lookup"><span data-stu-id="51dd7-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="51dd7-194">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-194">string</span></span> | `RoleId` | <span data-ttu-id="51dd7-195">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-195">string</span></span>
|`UserId` | <span data-ttu-id="51dd7-196">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-196">string</span></span> | `UserId` | <span data-ttu-id="51dd7-197">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="51dd7-197">string</span></span>

<span data-ttu-id="51dd7-198">Odkazy na předchozí tabulky mapování při vytváření migrace skript pro *uživatelé* a *role*.</span><span class="sxs-lookup"><span data-stu-id="51dd7-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="51dd7-199">Následující příklad předpokládá, že máte dvě databáze na serveru databáze.</span><span class="sxs-lookup"><span data-stu-id="51dd7-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="51dd7-200">Jedna databáze obsahuje existující schéma členství technologie ASP.NET a data.</span><span class="sxs-lookup"><span data-stu-id="51dd7-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="51dd7-201">Ostatní databáze byla vytvořena pomocí kroků popsaných výše.</span><span class="sxs-lookup"><span data-stu-id="51dd7-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="51dd7-202">Komentáře jsou zahrnuty vložené další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="51dd7-202">Comments are included inline for more details.</span></span>

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

<span data-ttu-id="51dd7-203">Po dokončení skriptu aplikace ASP.NET Core Identity vytvořený naplněný uživatelů se členstvím.</span><span class="sxs-lookup"><span data-stu-id="51dd7-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="51dd7-204">Uživatelé musí před přihlášení změnit své heslo.</span><span class="sxs-lookup"><span data-stu-id="51dd7-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="51dd7-205">Jestliže systém členství, že uživatelé s uživatelská jména, která se neshodovaly e-mailové adresy, je potřeba aplikaci vytvořili dříve, aby tomuto požadavku vyhovělo změny.</span><span class="sxs-lookup"><span data-stu-id="51dd7-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="51dd7-206">Výchozí šablony očekává `UserName` a `Email` být stejné.</span><span class="sxs-lookup"><span data-stu-id="51dd7-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="51dd7-207">V situacích, ve kterých jsou různé, potřeba upravit tak, aby pomocí procesu přihlášení `UserName` místo `Email`.</span><span class="sxs-lookup"><span data-stu-id="51dd7-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="51dd7-208">V `PageModel` přihlašovací stránky, nacházející se v *Pages\Account\Login.cshtml.cs*, odeberte `[EmailAddress]` atribut z *e-mailu* vlastnost.</span><span class="sxs-lookup"><span data-stu-id="51dd7-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="51dd7-209">Přejmenujte jej na *uživatelské jméno*.</span><span class="sxs-lookup"><span data-stu-id="51dd7-209">Rename it to *UserName*.</span></span> <span data-ttu-id="51dd7-210">To vyžaduje změnu bez ohledu na `EmailAddress` je uveden v *zobrazení* a *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="51dd7-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="51dd7-211">Výsledek vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="51dd7-211">The result looks like the following:</span></span>

 ![Opravené přihlášení](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="51dd7-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51dd7-213">Next steps</span></span>

<span data-ttu-id="51dd7-214">V tomto kurzu jste zjistili, jak k portu uživatelům z SQL členství ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="51dd7-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="51dd7-215">Další informace o ASP.NET Core Identity najdete v tématu [Úvod k identitě](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="51dd7-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
