---
title: Úvod do Identity v ASP.NET Core
author: rick-anderson
description: Pomocí Identity aplikace v ASP.NET Core. Obsahuje požadavky na heslo nastavení (RequireDigit, RequiredLength, RequiredUniqueChars a další).
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: c231a7619a4433ce004342ce68564e4c3892e702
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829299"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="af344-104">Úvod do Identity v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af344-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="af344-105">Podle [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Petr Dykstra](https://github.com/tdykstra), Jon Galloway [Erik Reitan](https://github.com/Erikre), a [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="af344-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="af344-106">ASP.NET Core Identity je systém členství, který slouží k přidání funkcí přihlášení do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="af344-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="af344-107">Uživatelé můžou vytvářet účet a přihlášení s uživatelským jménem a heslem nebo že můžete použít externího zprostředkovatele přihlášení jako je Facebook, Google, Microsoft Account, Twitteru nebo jiné.</span><span class="sxs-lookup"><span data-stu-id="af344-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="af344-108">ASP.NET Core Identity pro použití databáze SQL serveru k ukládání uživatelská jména, hesla a data profilu můžete nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="af344-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="af344-109">Alternativně můžete použít vlastní trvalého úložiště, například Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="af344-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="af344-110">Tento dokument obsahuje pokyny pro Visual Studio a pomocí rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="af344-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="af344-111">Zobrazení nebo stažení ukázkového kódu.</span><span class="sxs-lookup"><span data-stu-id="af344-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="af344-112">(Jak stáhnout)</span><span class="sxs-lookup"><span data-stu-id="af344-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="af344-113">Přehled Identity</span><span class="sxs-lookup"><span data-stu-id="af344-113">Overview of Identity</span></span>

<span data-ttu-id="af344-114">V tomto tématu budete zjistěte, jak přidat funkci do zaregistrovat, přihlaste se pomocí ASP.NET Core Identity a odhlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="af344-114">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="af344-115">Podrobnější pokyny týkající se vytváření aplikací pomocí ASP.NET Core Identity najdete v části Další kroky na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="af344-115">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1. <span data-ttu-id="af344-116">Vytvoření projektu webové aplikace ASP.NET Core s jednotlivými uživatelskými účty.</span><span class="sxs-lookup"><span data-stu-id="af344-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="af344-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af344-117">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="af344-118">V sadě Visual Studio, vyberte **souboru** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="af344-118">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="af344-119">Vyberte **webové aplikace ASP.NET Core** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="af344-119">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

   ![Dialogové okno nového projektu](identity/_static/01-new-project.png)

   <span data-ttu-id="af344-121">Vyberte v ASP.NET Core **webové aplikace (Model-View-Controller)** pro ASP.NET Core 2.x, pak vyberte **změna ověřování**.</span><span class="sxs-lookup"><span data-stu-id="af344-121">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

   ![Dialogové okno nového projektu](identity/_static/02-new-project.png)

   <span data-ttu-id="af344-123">Dialogové okno se zobrazí nabídka možnosti ověřování.</span><span class="sxs-lookup"><span data-stu-id="af344-123">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="af344-124">Vyberte **jednotlivé uživatelské účty** a klikněte na tlačítko **OK** vrátit do předchozího dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="af344-124">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

   ![Dialogové okno nového projektu](identity/_static/03-new-project-auth.png)

   <span data-ttu-id="af344-126">Výběr **jednotlivé uživatelské účty** instruuje Visual Studio a vytvářet modely, modely ViewModels, zobrazení, Kontrolerů a další prostředky požadované pro ověřování jako součást šablony projektu.</span><span class="sxs-lookup"><span data-stu-id="af344-126">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="af344-127">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="af344-127">.NET Core CLI</span></span>](#tab/netcore-cli)

   <span data-ttu-id="af344-128">Pokud používáte rozhraní příkazového řádku .NET Core, vytvořte nový projekt pomocí `dotnet new mvc --auth Individual`.</span><span class="sxs-lookup"><span data-stu-id="af344-128">If using the .NET Core CLI, create the new project using `dotnet new mvc --auth Individual`.</span></span> <span data-ttu-id="af344-129">Tento příkaz vytvoří nový projekt se stejným kódem šablony Identity, které sada Visual Studio vytvoří.</span><span class="sxs-lookup"><span data-stu-id="af344-129">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

   <span data-ttu-id="af344-130">Vytvořený projekt obsahuje `Microsoft.AspNetCore.Identity.EntityFrameworkCore` balíček, který ukládá data identit a schémat na SQL Server pomocí [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="af344-130">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

   ---

2. <span data-ttu-id="af344-131">Nakonfigurujte identitu služby a přidejte middlewaru v `Startup`.</span><span class="sxs-lookup"><span data-stu-id="af344-131">Configure Identity services and add middleware in `Startup`.</span></span>

   <span data-ttu-id="af344-132">Služby identit jsou přidané do aplikace v `ConfigureServices` metodu `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="af344-132">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="af344-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="af344-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="af344-134">Tyto služby jsou k dispozici pro aplikace prostřednictvím [injektáž závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="af344-134">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="af344-135">Povolení identity pro aplikace po zavolání `UseAuthentication` v `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="af344-135">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="af344-136">`UseAuthentication` Přidá ověřování [middleware](xref:fundamentals/middleware/index) požadavku kanálu.</span><span class="sxs-lookup"><span data-stu-id="af344-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="af344-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="af344-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="af344-138">Tyto služby jsou k dispozici pro aplikace prostřednictvím [injektáž závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="af344-138">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="af344-139">Povolení identity pro aplikace po zavolání `UseIdentity` v `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="af344-139">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="af344-140">`UseIdentity` Přidá na základě souboru cookie ověřování [middleware](xref:fundamentals/middleware/index) požadavku kanálu.</span><span class="sxs-lookup"><span data-stu-id="af344-140">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   <span data-ttu-id="af344-141">Další informace o spuštění aplikace procesu naleznete v tématu [spuštění aplikace](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="af344-141">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3. <span data-ttu-id="af344-142">Vytvoření uživatele.</span><span class="sxs-lookup"><span data-stu-id="af344-142">Create a user.</span></span>

   <span data-ttu-id="af344-143">Spusťte aplikaci a potom klikněte na **zaregistrovat** odkaz.</span><span class="sxs-lookup"><span data-stu-id="af344-143">Launch the application and then click on the **Register** link.</span></span>

   <span data-ttu-id="af344-144">Pokud je to poprvé při provádění této akce, může být potřeba ke spouštění migrace.</span><span class="sxs-lookup"><span data-stu-id="af344-144">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="af344-145">Aplikace vás vyzve, abyste **migrace použít**.</span><span class="sxs-lookup"><span data-stu-id="af344-145">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="af344-146">V případě potřeby aktualizujte stránku.</span><span class="sxs-lookup"><span data-stu-id="af344-146">Refresh the page if needed.</span></span>

   ![Použití migrace webové stránky](identity/_static/apply-migrations.png)

   <span data-ttu-id="af344-148">Alternativně můžete otestovat pomocí ASP.NET Core Identity s vaší aplikací bez trvalé databáze pomocí databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="af344-148">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="af344-149">Chcete-li používat databázi v paměti, přidejte `Microsoft.EntityFrameworkCore.InMemory` balíček do vaší aplikace a upravte volání vaší aplikace `AddDbContext` v `ConfigureServices` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="af344-149">To use an in-memory database, add the `Microsoft.EntityFrameworkCore.InMemory` package to your app and modify your app's call to `AddDbContext` in `ConfigureServices` as follows:</span></span>

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   <span data-ttu-id="af344-150">Pokud uživatel klikne **zaregistrovat** odkaz, `Register` akce je volána na `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="af344-150">When the user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="af344-151">`Register` Akce vytvoří uživatele voláním `CreateAsync` na `_userManager` objektu (poskytnuté `AccountController` pomocí vkládání závislostí):</span><span class="sxs-lookup"><span data-stu-id="af344-151">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   <span data-ttu-id="af344-152">Pokud byl uživatel vytvořen úspěšně, uživatel je přihlášen voláním `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="af344-152">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="af344-153">**Poznámka:** naleznete v tématu [účtu potvrzení](xref:security/authentication/accconfirm#prevent-login-at-registration) kroky, jak zabránit okamžité přihlášení při registraci.</span><span class="sxs-lookup"><span data-stu-id="af344-153">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

4. <span data-ttu-id="af344-154">Přihlásit se.</span><span class="sxs-lookup"><span data-stu-id="af344-154">Log in.</span></span>

   <span data-ttu-id="af344-155">Uživatelé můžou přihlásit po klepnutí na **přihlášení** odkazu v horní části webu, nebo na přihlašovací stránku, může přejde v případě podobného pokusu získat přístup k součásti lokality, která vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="af344-155">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="af344-156">Když uživatel odešle formulář na přihlašovací stránku, `AccountController` `Login` akce je volána.</span><span class="sxs-lookup"><span data-stu-id="af344-156">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

   <span data-ttu-id="af344-157">`Login` Volání akce `PasswordSignInAsync` na `_signInManager` objektu (poskytnuté `AccountController` pomocí vkládání závislostí).</span><span class="sxs-lookup"><span data-stu-id="af344-157">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   <span data-ttu-id="af344-158">Základní `Controller` třídy zpřístupňuje `User` vlastnost, která se dá dostat z metody kontroleru.</span><span class="sxs-lookup"><span data-stu-id="af344-158">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="af344-159">Například můžete zobrazit výčet `User.Claims` a rozhodnutí o autorizaci.</span><span class="sxs-lookup"><span data-stu-id="af344-159">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="af344-160">Další informace najdete v tématu [autorizace](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="af344-160">For more information, see [Authorization](xref:security/authorization/index).</span></span>

5. <span data-ttu-id="af344-161">Odhlaste.</span><span class="sxs-lookup"><span data-stu-id="af344-161">Log out.</span></span>

   <span data-ttu-id="af344-162">Kliknutím **Odhlásit** propojit volání `LogOut` akce.</span><span class="sxs-lookup"><span data-stu-id="af344-162">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="af344-163">Předchozí kód výše volání `_signInManager.SignOutAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="af344-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="af344-164">`SignOutAsync` Metoda vymaže deklarací identity uživatele uložené v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="af344-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

<a name="pw"></a>
6. <span data-ttu-id="af344-165">Konfigurace.</span><span class="sxs-lookup"><span data-stu-id="af344-165">Configuration.</span></span>

   <span data-ttu-id="af344-166">Identita má některé výchozí chování, které mohou být přepsána nastaveními v třída při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="af344-166">Identity has some default behaviors that can be overridden in the app's startup class.</span></span> <span data-ttu-id="af344-167">`IdentityOptions` není potřeba konfigurovat při použití výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="af344-167">`IdentityOptions` don't need to be configured when using the default behaviors.</span></span> <span data-ttu-id="af344-168">Následující kód nastaví několik možností, jak síly hesla:</span><span class="sxs-lookup"><span data-stu-id="af344-168">The following code sets several password strength options:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="af344-169">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="af344-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="af344-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="af344-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   <span data-ttu-id="af344-171">Další informace o tom, jak nakonfigurovat Identity, najdete v části [konfigurace Identity](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="af344-171">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>

   <span data-ttu-id="af344-172">Můžete také konfigurovat datový typ primárního klíče, naleznete v tématu [konfigurace Identity primárních klíčů datový typ](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="af344-172">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>

7. <span data-ttu-id="af344-173">Zobrazí se databáze.</span><span class="sxs-lookup"><span data-stu-id="af344-173">View the database.</span></span>

   <span data-ttu-id="af344-174">Pokud vaše aplikace používá databázi serveru SQL Server (výchozí nastavení na Windows a pro uživatele sady Visual Studio), můžete zobrazit aplikace vytvořená databáze.</span><span class="sxs-lookup"><span data-stu-id="af344-174">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="af344-175">Můžete použít **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="af344-175">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="af344-176">Můžete také z aplikace Visual Studio vyberte **zobrazení** > **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="af344-176">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="af344-177">Připojte se k **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="af344-177">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="af344-178">Databáze s názvem odpovídajícím `aspnet-<name of your project>-<guid>` se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="af344-178">The database with a name matching `aspnet-<name of your project>-<guid>` is displayed.</span></span>

   ![Kontextové nabídky v tabulce databáze AspNetUsers](identity/_static/04-db.png)

   <span data-ttu-id="af344-180">Rozbalte databázi a její **tabulky**, klepněte pravým tlačítkem myši **dbo. AspNetUsers** tabulce a vybrat **Data zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="af344-180">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="af344-181">Zkontrolujte, jestli funguje Identity</span><span class="sxs-lookup"><span data-stu-id="af344-181">Verify Identity works</span></span>

    <span data-ttu-id="af344-182">Výchozí hodnota *webové aplikace ASP.NET Core* šablona projektu umožňuje uživatelům přístup k žádné akci v aplikaci bez nutnosti přihlášení.</span><span class="sxs-lookup"><span data-stu-id="af344-182">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="af344-183">Chcete-li ověřit, jestli ASP.NET Identity funguje, přidejte`[Authorize]` atribut `About` akce `Home` Kontroleru.</span><span class="sxs-lookup"><span data-stu-id="af344-183">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="af344-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af344-184">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="af344-185">Spustit projekt pomocí **Ctrl** + **F5** a přejděte **o** stránky.</span><span class="sxs-lookup"><span data-stu-id="af344-185">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="af344-186">Může používat jenom ověření uživatelé **o** stránky teď ASP.NET vás přesměruje na přihlašovací stránku a přihlásit nebo zaregistrovat.</span><span class="sxs-lookup"><span data-stu-id="af344-186">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="af344-187">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="af344-187">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="af344-188">Otevřete okno příkazového řádku a přejděte do kořenového adresáře projektu adresáře, který obsahuje `.csproj` souboru.</span><span class="sxs-lookup"><span data-stu-id="af344-188">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="af344-189">Spustit [dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz ke spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="af344-189">Run the [dotnet run](/dotnet/core/tools/dotnet-run) command to run the app:</span></span>

    ```csharp
    dotnet run 
    ```

    <span data-ttu-id="af344-190">Adresa URL zadaná ve výstupu vyhledejte [dotnet spustit](/dotnet/core/tools/dotnet-run) příkazu.</span><span class="sxs-lookup"><span data-stu-id="af344-190">Browse the URL specified in the output from the [dotnet run](/dotnet/core/tools/dotnet-run) command.</span></span> <span data-ttu-id="af344-191">Adresa URL by měly odkazovat na `localhost` s vygenerované číslo portu.</span><span class="sxs-lookup"><span data-stu-id="af344-191">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="af344-192">Přejděte **o** stránky.</span><span class="sxs-lookup"><span data-stu-id="af344-192">Navigate to the **About** page.</span></span> <span data-ttu-id="af344-193">Může používat jenom ověření uživatelé **o** stránky teď ASP.NET vás přesměruje na přihlašovací stránku a přihlásit nebo zaregistrovat.</span><span class="sxs-lookup"><span data-stu-id="af344-193">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="af344-194">Komponenty identit</span><span class="sxs-lookup"><span data-stu-id="af344-194">Identity Components</span></span>

<span data-ttu-id="af344-195">Primární referenční sestavení pro systém identit je `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="af344-195">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="af344-196">Tento balíček obsahuje základní sadu rozhraní pro ASP.NET Core Identity a je součástí `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="af344-196">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="af344-197">Tyto závislosti jsou potřeba pro použití systému identit v aplikacích ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="af344-197">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="af344-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Obsahuje požadované typy pro použití s Entity Framework Core Identity.</span><span class="sxs-lookup"><span data-stu-id="af344-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="af344-199">`Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core je technologie přístup doporučené dat společnosti Microsoft pro relační databáze jako SQL Server.</span><span class="sxs-lookup"><span data-stu-id="af344-199">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="af344-200">Pro účely testování můžete použít `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="af344-200">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="af344-201">`Microsoft.AspNetCore.Authentication.Cookies` -Middlewaru, který umožňuje aplikaci používat na základě souboru cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="af344-201">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="af344-202">Migrace do ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="af344-202">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="af344-203">Pro další informace a pokyny k migraci svou existující identitu úložiště najdete v tématu [migrovat ověřování a identita](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="af344-203">For additional information and guidance on migrating your existing Identity store see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="af344-204">Nastavení síly hesla</span><span class="sxs-lookup"><span data-stu-id="af344-204">Setting password strength</span></span>

<span data-ttu-id="af344-205">Zobrazit [konfigurace](#pw) ukázku, která nastavuje minimální požadavky.</span><span class="sxs-lookup"><span data-stu-id="af344-205">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="af344-206">Další kroky</span><span class="sxs-lookup"><span data-stu-id="af344-206">Next Steps</span></span>

* [<span data-ttu-id="af344-207">Migrace ověřování a identita</span><span class="sxs-lookup"><span data-stu-id="af344-207">Migrate Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="af344-208">Potvrzení účtu a obnovení hesla</span><span class="sxs-lookup"><span data-stu-id="af344-208">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="af344-209">Dvoufaktorové ověřování přes SMS</span><span class="sxs-lookup"><span data-stu-id="af344-209">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="af344-210">Facebook, Google a externí zprostředkovatel ověřování</span><span class="sxs-lookup"><span data-stu-id="af344-210">Facebook, Google, and external provider authentication</span></span>](xref:security/authentication/social/index)
