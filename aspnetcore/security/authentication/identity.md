---
title: "Úvod do Identity na jádro ASP.NET"
author: rick-anderson
description: "Pomocí Identity aplikace ASP.NET Core. Obsahuje požadavky na heslo nastavení (RequireDigit, RequiredLength, RequiredUniqueChars a další)."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity
ms.openlocfilehash: a84c5f1d4cf802ee0c4116d2a02bdbfbab9aa72b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="1abed-104">Úvod do Identity na jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1abed-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="1abed-105">Podle [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [tní Dykstra](https://github.com/tdykstra), Jon Galloway [Erik Reitan](https://github.com/Erikre), a [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="1abed-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1abed-106">Identita ASP.NET Core je systém členství, který umožňuje přidat funkce přihlášení do aplikace.</span><span class="sxs-lookup"><span data-stu-id="1abed-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="1abed-107">Uživatelé mohou vytvářet účet a přihlášení s uživatelským jménem a heslem nebo se můžete použít externího zprostředkovatele přihlášení jako je Facebook, Google, Microsoft Account, služby Twitter nebo jiné.</span><span class="sxs-lookup"><span data-stu-id="1abed-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="1abed-108">Můžete nakonfigurovat ASP.NET Identity Core ukládat uživatelská jména, hesla a data profilu do databáze serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1abed-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="1abed-109">Alternativně můžete použít vlastní trvalého úložiště, například Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="1abed-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="1abed-110">Tento dokument obsahuje pokyny pro sadu Visual Studio a pro používání rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="1abed-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="1abed-111">Zobrazení nebo stažení ukázkového kódu.</span><span class="sxs-lookup"><span data-stu-id="1abed-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="1abed-112">(Postup stažení)</span><span class="sxs-lookup"><span data-stu-id="1abed-112">(How to download)</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="1abed-113">Přehled identity</span><span class="sxs-lookup"><span data-stu-id="1abed-113">Overview of Identity</span></span>

<span data-ttu-id="1abed-114">V tomto tématu budete Naučte se používat ASP.NET Core Identity k přidání funkcí registrace, přihlášení a odhlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="1abed-114">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="1abed-115">Podrobnější pokyny k vytváření aplikací pomocí ASP.NET Core Identity najdete v části Další kroky na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="1abed-115">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="1abed-116">Vytvoření projektu webové aplikace ASP.NET Core s jednotlivé uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="1abed-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1abed-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1abed-117">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="1abed-118">V sadě Visual Studio, vyberte **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="1abed-118">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="1abed-119">Vyberte **webové aplikace ASP.NET Core** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1abed-119">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

    ![Dialogové okno Nový projekt](identity/_static/01-new-project.png)

    <span data-ttu-id="1abed-121">Vyberte ASP.NET Core **webové aplikace (Model-View-Controller)** pro technologii ASP.NET základní 2.x a pak vyberte **změna ověřování**.</span><span class="sxs-lookup"><span data-stu-id="1abed-121">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

    ![Dialogové okno Nový projekt](identity/_static/02-new-project.png)

    <span data-ttu-id="1abed-123">Zobrazí se dialogové okno se zobrazí nabízení možnosti ověřování.</span><span class="sxs-lookup"><span data-stu-id="1abed-123">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="1abed-124">Vyberte **jednotlivé uživatelské účty** a klikněte na tlačítko **OK** se vrátíte do předchozího dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="1abed-124">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

    ![Dialogové okno Nový projekt](identity/_static/03-new-project-auth.png)

    <span data-ttu-id="1abed-126">Výběr **jednotlivé uživatelské účty** přesměruje Chcete-li vytvořit modely, ViewModels, zobrazení, řadiče a další prostředky požadované pro ověřování v rámci šablony projektů Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1abed-126">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1abed-127">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="1abed-127">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="1abed-128">Pokud používáte rozhraní příkazového řádku .NET Core, vytvořte nový projekt pomocí ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="1abed-128">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="1abed-129">Tento příkaz vytvoří nový projekt se stejným kódem šablony Identity, které sada Visual Studio vytvoří.</span><span class="sxs-lookup"><span data-stu-id="1abed-129">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

    <span data-ttu-id="1abed-130">Vytvořený projekt obsahuje `Microsoft.AspNetCore.Identity.EntityFrameworkCore` balíček, která ukládá data identit a schématu na SQL Server pomocí [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="1abed-130">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

    ---

2.  <span data-ttu-id="1abed-131">Nakonfigurujte identitu služby a přidejte middleware v `Startup`.</span><span class="sxs-lookup"><span data-stu-id="1abed-131">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="1abed-132">Identita služby jsou přidány do aplikace `ConfigureServices` metoda v `Startup` – třída:</span><span class="sxs-lookup"><span data-stu-id="1abed-132">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1abed-133">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1abed-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]
    
    <span data-ttu-id="1abed-134">Tyto služby jsou k dispozici pro aplikace prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1abed-134">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="1abed-135">Identity je pro tuto aplikaci povolena voláním `UseAuthentication` v `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="1abed-135">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="1abed-136">`UseAuthentication` Přidá ověřování [middleware](xref:fundamentals/middleware/index) požadavku kanálu.</span><span class="sxs-lookup"><span data-stu-id="1abed-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1abed-137">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="1abed-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]
    
    <span data-ttu-id="1abed-138">Tyto služby jsou k dispozici pro aplikace prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1abed-138">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="1abed-139">Identity je pro tuto aplikaci povolena voláním `UseIdentity` v `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="1abed-139">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="1abed-140">`UseIdentity` Přidá ověřování na základě souborů cookie [middleware](xref:fundamentals/middleware/index) požadavku kanálu.</span><span class="sxs-lookup"><span data-stu-id="1abed-140">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>
        
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="1abed-141">Další informace o spuštění aplikace se proces najdete v tématu [spuštění aplikace](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="1abed-141">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="1abed-142">Vytvořte uživatele.</span><span class="sxs-lookup"><span data-stu-id="1abed-142">Create a user.</span></span>

    <span data-ttu-id="1abed-143">Spuštění aplikace a potom klikněte na **zaregistrovat** odkaz.</span><span class="sxs-lookup"><span data-stu-id="1abed-143">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="1abed-144">Pokud je při prvním provedení této akce, může být potřeba spustit migrace.</span><span class="sxs-lookup"><span data-stu-id="1abed-144">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="1abed-145">Aplikace vás vyzve, abyste **použít migrace**.</span><span class="sxs-lookup"><span data-stu-id="1abed-145">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="1abed-146">V případě potřeby aktualizujte stránku.</span><span class="sxs-lookup"><span data-stu-id="1abed-146">Refresh the page if needed.</span></span>
    
    ![Použití migrace webové stránky](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="1abed-148">Alternativně můžete otestovat pomocí ASP.NET Core Identity s vaší aplikací bez trvalé databáze pomocí databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="1abed-148">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="1abed-149">Chcete-li používat databázi v paměti, přidejte ``Microsoft.EntityFrameworkCore.InMemory`` balíčku do vaší aplikace a upravit volání vaší aplikace ``AddDbContext`` v ``ConfigureServices`` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1abed-149">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="1abed-150">Když uživatel klikne **zaregistrovat** odkaz, ``Register`` akce je volána v ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="1abed-150">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="1abed-151">``Register`` Akce vytvoří uživatele voláním `CreateAsync` na `_userManager` objektu (poskytované ``AccountController`` pomocí vkládání závislostí):</span><span class="sxs-lookup"><span data-stu-id="1abed-151">The ``Register`` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="1abed-152">Pokud byl uživatel vytvořen úspěšně, uživatel je přihlášen pomocí volání ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="1abed-152">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="1abed-153">**Poznámka:** najdete v části [účet potvrzení](xref:security/authentication/accconfirm#prevent-login-at-registration) kroky, aby se zabránilo okamžitou přihlášení při registraci.</span><span class="sxs-lookup"><span data-stu-id="1abed-153">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="1abed-154">Přihlásit se.</span><span class="sxs-lookup"><span data-stu-id="1abed-154">Log in.</span></span>
 
    <span data-ttu-id="1abed-155">Uživatelé se mohou přihlásit kliknutím **přihlásit** odkaz v horní části webu, nebo se může přesměrováni na stránku přihlášení Pokud pokus o přístup k součástí lokality, která vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="1abed-155">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="1abed-156">Když uživatel odešle formulář na přihlašovací stránku, ``AccountController`` ``Login`` akce je volána.</span><span class="sxs-lookup"><span data-stu-id="1abed-156">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    <span data-ttu-id="1abed-157">``Login`` Volání akce ``PasswordSignInAsync`` na ``_signInManager`` objektu (poskytované ``AccountController`` pomocí vkládání závislostí).</span><span class="sxs-lookup"><span data-stu-id="1abed-157">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="1abed-158">Základní ``Controller`` třídy zpřístupňuje ``User`` vlastnost, která je přístupné z metody kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1abed-158">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="1abed-159">Například můžete vytvořit výčet `User.Claims` a autorizační rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="1abed-159">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="1abed-160">Další informace najdete v tématu [autorizace](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="1abed-160">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="1abed-161">Odhlaste.</span><span class="sxs-lookup"><span data-stu-id="1abed-161">Log out.</span></span>
 
    <span data-ttu-id="1abed-162">Kliknutím **odhlášení** odkaz volání `LogOut` akce.</span><span class="sxs-lookup"><span data-stu-id="1abed-162">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="1abed-163">Předchozí kód výše volání `_signInManager.SignOutAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="1abed-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="1abed-164">`SignOutAsync` Metoda vymaže deklaracích identity uživatele v souborech cookie.</span><span class="sxs-lookup"><span data-stu-id="1abed-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
<a name="pw"></a>
6.  <span data-ttu-id="1abed-165">Konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1abed-165">Configuration.</span></span>

    <span data-ttu-id="1abed-166">Identita má některé výchozí chování, které mohou být přepsána nastaveními v třída při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="1abed-166">Identity has some default behaviors that can be overridden in the app's startup class.</span></span> <span data-ttu-id="1abed-167">`IdentityOptions` není nutné nakonfigurovat při použití výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="1abed-167">`IdentityOptions` don't need to be configured when using the default behaviors.</span></span> <span data-ttu-id="1abed-168">Následující kód nastaví několik možností síly hesla:</span><span class="sxs-lookup"><span data-stu-id="1abed-168">The following code sets several password strength options:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1abed-169">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1abed-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1abed-170">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="1abed-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

    ---
    
    <span data-ttu-id="1abed-171">Další informace o tom, jak nakonfigurovat Identity najdete v tématu [konfigurace Identity](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="1abed-171">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="1abed-172">Můžete také konfigurovat datový typ primární klíč, najdete v části [konfigurace Identity primárních klíčů datový typ](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="1abed-172">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="1abed-173">Zobrazte databázi.</span><span class="sxs-lookup"><span data-stu-id="1abed-173">View the database.</span></span>

    <span data-ttu-id="1abed-174">Pokud vaše aplikace používá databázi systému SQL Server (výchozí možnost v systému Windows a pro uživatele v sadě Visual Studio), můžete zobrazit databázi aplikace vytvořená.</span><span class="sxs-lookup"><span data-stu-id="1abed-174">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="1abed-175">Můžete použít **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="1abed-175">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="1abed-176">Případně ze sady Visual Studio, vyberte možnost **zobrazení** > **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="1abed-176">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="1abed-177">Připojení k **\MSSQLLocalDB (localdb)**.</span><span class="sxs-lookup"><span data-stu-id="1abed-177">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="1abed-178">Databáze s odpovídajícím názvem **aspnet - <*název projektu*>-<*datum řetězec* >**  se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="1abed-178">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Kontextové nabídky na AspNetUsers databázové tabulky](identity/_static/04-db.png)
    
    <span data-ttu-id="1abed-180">Rozbalte databázi a její **tabulky**, klikněte pravým tlačítkem **dbo. AspNetUsers** tabulky a vyberte **Data zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="1abed-180">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="1abed-181">Ověření Identity funguje</span><span class="sxs-lookup"><span data-stu-id="1abed-181">Verify Identity works</span></span>

    <span data-ttu-id="1abed-182">Výchozí hodnota *webové aplikace ASP.NET Core* šablona projektu umožňuje uživatelům přístup k veškeré akce v aplikaci bez nutnosti k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1abed-182">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="1abed-183">Chcete-li ověřit, že ASP.NET Identity funguje, přidejte`[Authorize]` atribut `About` akce `Home` řadiče.</span><span class="sxs-lookup"><span data-stu-id="1abed-183">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1abed-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1abed-184">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="1abed-185">Spusťte projekt pomocí **Ctrl** + **F5** a přejděte do **o** stránky.</span><span class="sxs-lookup"><span data-stu-id="1abed-185">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="1abed-186">Může používat pouze ověření uživatelé **o** stránka nyní, takže ASP.NET vás přesměruje na přihlašovací stránku pro přihlášení nebo registraci.</span><span class="sxs-lookup"><span data-stu-id="1abed-186">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1abed-187">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="1abed-187">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="1abed-188">Otevřete okno příkazového řádku a přejděte do projektu kořenový adresář obsahující `.csproj` souboru.</span><span class="sxs-lookup"><span data-stu-id="1abed-188">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="1abed-189">Spustit [dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz ke spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="1abed-189">Run the [dotnet run](/dotnet/core/tools/dotnet-run) command to run the app:</span></span>

    ```cs
    dotnet run 
    ```

    <span data-ttu-id="1abed-190">Vyhledejte ve výstupu zadanou adresu URL [dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1abed-190">Browse the URL specified in the output from the [dotnet run](/dotnet/core/tools/dotnet-run) command.</span></span> <span data-ttu-id="1abed-191">Adresa URL by měla odkazovat na `localhost` s číslem generovaného portu.</span><span class="sxs-lookup"><span data-stu-id="1abed-191">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="1abed-192">Přejděte na **o** stránky.</span><span class="sxs-lookup"><span data-stu-id="1abed-192">Navigate to the **About** page.</span></span> <span data-ttu-id="1abed-193">Může používat pouze ověření uživatelé **o** stránka nyní, takže ASP.NET vás přesměruje na přihlašovací stránku pro přihlášení nebo registraci.</span><span class="sxs-lookup"><span data-stu-id="1abed-193">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="1abed-194">Komponent identity</span><span class="sxs-lookup"><span data-stu-id="1abed-194">Identity Components</span></span>

<span data-ttu-id="1abed-195">Je primární referenční sestavení pro systém identit `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="1abed-195">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="1abed-196">Tento balíček obsahuje sadu základní rozhraní pro ASP.NET Core Identity a je začleněn sám v `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="1abed-196">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="1abed-197">Tyto závislosti jsou potřeba k použití v aplikacích ASP.NET Core systém identit:</span><span class="sxs-lookup"><span data-stu-id="1abed-197">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="1abed-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Obsahuje požadované typy, které mají pomocí Identity Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1abed-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="1abed-199">`Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core je technologie přístup doporučené dat společnosti Microsoft pro relační databáze jako SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1abed-199">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="1abed-200">Pro testování, můžete použít `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="1abed-200">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="1abed-201">`Microsoft.AspNetCore.Authentication.Cookies` -Middleware, který umožňuje aplikaci používat ověřování na základě souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="1abed-201">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="1abed-202">Migrace na jádro ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="1abed-202">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="1abed-203">Pro další informace a pokyny k migraci stávající identitu úložiště najdete v tématu [migrace ověřování a identita](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="1abed-203">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="1abed-204">Nastavení síly hesla</span><span class="sxs-lookup"><span data-stu-id="1abed-204">Setting password strength</span></span>

<span data-ttu-id="1abed-205">V tématu [konfigurace](#pw) pro ukázku, která nastaví požadavky na minimální hesla.</span><span class="sxs-lookup"><span data-stu-id="1abed-205">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1abed-206">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1abed-206">Next Steps</span></span>

* [<span data-ttu-id="1abed-207">Migrace ověřování a identita</span><span class="sxs-lookup"><span data-stu-id="1abed-207">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="1abed-208">Potvrzení účtu a obnovení hesla</span><span class="sxs-lookup"><span data-stu-id="1abed-208">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="1abed-209">Dvoufaktorové ověřování přes SMS</span><span class="sxs-lookup"><span data-stu-id="1abed-209">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="1abed-210">Facebook, Google a externí zprostředkovatel ověřování</span><span class="sxs-lookup"><span data-stu-id="1abed-210">Facebook, Google, and external provider authentication</span></span>](xref:security/authentication/social/index)
