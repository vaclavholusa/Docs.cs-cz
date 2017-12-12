---
title: "Úvod do Identity na jádro ASP.NET"
author: rick-anderson
description: "Pomocí Identity aplikace ASP.NET Core"
keywords: "ASP.NET Core, Identity, autorizaci zabezpečení"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 0679663b3b3b66f9935d0fb24360be2954fcdee1
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="39ced-104">Úvod do Identity na jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="39ced-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="39ced-105">Podle [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [tní Dykstra](https://github.com/tdykstra), Jon Galloway [Erik Reitan](https://github.com/Erikre), a [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="39ced-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="39ced-106">Identita ASP.NET Core je systém členství, který umožňuje přidat funkce přihlášení do aplikace.</span><span class="sxs-lookup"><span data-stu-id="39ced-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="39ced-107">Uživatelé mohou vytvářet účet a přihlášení s uživatelským jménem a heslem nebo se můžete použít externího zprostředkovatele přihlášení jako je Facebook, Google, Microsoft Account, služby Twitter nebo jiné.</span><span class="sxs-lookup"><span data-stu-id="39ced-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="39ced-108">Můžete nakonfigurovat ASP.NET Identity Core ukládat uživatelská jména, hesla a data profilu do databáze serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="39ced-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="39ced-109">Alternativně můžete použít vlastní trvalého úložiště, například Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="39ced-109">Alternatively, you can use your own persistent store, for example Azure Table Storage.</span></span> <span data-ttu-id="39ced-110">Tento dokument obsahuje pokyny pro sadu Visual Studio a pro používání rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="39ced-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

## <a name="overview-of-identity"></a><span data-ttu-id="39ced-111">Přehled identity</span><span class="sxs-lookup"><span data-stu-id="39ced-111">Overview of Identity</span></span>

<span data-ttu-id="39ced-112">V tomto tématu budete Naučte se používat ASP.NET Core Identity k přidání funkcí registrace, přihlášení a odhlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="39ced-112">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="39ced-113">Podrobnější pokyny k vytváření aplikací pomocí ASP.NET Core Identity najdete v části Další kroky na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="39ced-113">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="39ced-114">Vytvoření projektu webové aplikace ASP.NET Core s jednotlivé uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="39ced-114">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="39ced-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39ced-115">Visual Studio</span></span>](#tab/visual-studio)
    <span data-ttu-id="39ced-116">V sadě Visual Studio, vyberte **soubor** -> **nový** -> **projektu**.</span><span class="sxs-lookup"><span data-stu-id="39ced-116">In Visual Studio, select **File** -> **New** -> **Project**.</span></span> <span data-ttu-id="39ced-117">Vyberte **webové aplikace ASP.NET** z **nový projekt** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="39ced-117">Select the **ASP.NET Web Application** from the **New Project** dialog box.</span></span> <span data-ttu-id="39ced-118">Výběr ASP.NET Core **webové Application(Model-View-Controller)** pro ASP.NET Core 2.x s **jednotlivé uživatelské účty** jako metodu ověřování.</span><span class="sxs-lookup"><span data-stu-id="39ced-118">Selecting an ASP.NET Core **Web Application(Model-View-Controller)** for ASP.NET Core 2.x with **Individual User Accounts** as the authentication method.</span></span>

    <span data-ttu-id="39ced-119">Poznámka: Je nutné vybrat **jednotlivé uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="39ced-119">Note: You must select **Individual User Accounts**.</span></span>
 
    ![Dialogové okno Nový projekt](identity/_static/01-mvc_2.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="39ced-121">.NET core rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="39ced-121">.NET Core CLI</span></span>](#tab/netcore-cli)
    <span data-ttu-id="39ced-122">Pokud používáte rozhraní příkazového řádku .NET Core, vytvořte nový projekt pomocí ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="39ced-122">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="39ced-123">Tím se vytvoří nový projekt se stejným kódem šablony Identity, které sada Visual Studio vytvoří.</span><span class="sxs-lookup"><span data-stu-id="39ced-123">This will create a new project with the same Identity template code Visual Studio creates.</span></span>
 
    <span data-ttu-id="39ced-124">Vytvořený projekt obsahuje `Microsoft.AspNetCore.Identity.EntityFrameworkCore` balíček, který se uchová Identity dat a schématu na SQL Server pomocí [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="39ced-124">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which will persist the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>
    
    ---
 
2.  <span data-ttu-id="39ced-125">Nakonfigurujte identitu služby a přidejte middleware v `Startup`.</span><span class="sxs-lookup"><span data-stu-id="39ced-125">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="39ced-126">Identita služby jsou přidány do aplikace `ConfigureServices` metoda v `Startup` – třída:</span><span class="sxs-lookup"><span data-stu-id="39ced-126">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="39ced-127">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="39ced-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="39ced-128">Tyto služby jsou k dispozici pro aplikace prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="39ced-128">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="39ced-129">Identity je pro tuto aplikaci povolena voláním `UseAuthentication` v `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="39ced-129">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="39ced-130">`UseAuthentication`Přidá ověřování [middleware](xref:fundamentals/middleware) požadavku kanálu.</span><span class="sxs-lookup"><span data-stu-id="39ced-130">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="39ced-131">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="39ced-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="39ced-132">Tyto služby jsou k dispozici pro aplikace prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="39ced-132">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="39ced-133">Identity je pro tuto aplikaci povolena voláním `UseIdentity` v `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="39ced-133">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="39ced-134">`UseIdentity`Přidá ověřování na základě souborů cookie [middleware](xref:fundamentals/middleware) požadavku kanálu.</span><span class="sxs-lookup"><span data-stu-id="39ced-134">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="39ced-135">Další informace o spuštění aplikace se proces najdete v tématu [spuštění aplikace](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="39ced-135">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="39ced-136">Vytvořte uživatele.</span><span class="sxs-lookup"><span data-stu-id="39ced-136">Create a user.</span></span>

    <span data-ttu-id="39ced-137">Spuštění aplikace a potom klikněte na **zaregistrovat** odkaz.</span><span class="sxs-lookup"><span data-stu-id="39ced-137">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="39ced-138">Pokud je při prvním provedení této akce, může být potřeba spustit migrace.</span><span class="sxs-lookup"><span data-stu-id="39ced-138">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="39ced-139">Aplikace vás vyzve, abyste **použít migrace**:</span><span class="sxs-lookup"><span data-stu-id="39ced-139">The application prompts you to **Apply Migrations**:</span></span>
    
    ![Použití migrace webové stránky](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="39ced-141">Alternativně můžete otestovat pomocí ASP.NET Core Identity s vaší aplikací bez trvalé databáze pomocí databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="39ced-141">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="39ced-142">Chcete-li používat databázi v paměti, přidejte ``Microsoft.EntityFrameworkCore.InMemory`` balíčku do vaší aplikace a upravit volání vaší aplikace ``AddDbContext`` v ``ConfigureServices`` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="39ced-142">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="39ced-143">Když uživatel klikne **zaregistrovat** odkaz, ``Register`` akce je volána v ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="39ced-143">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="39ced-144">``Register`` Akce vytvoří uživatele voláním `CreateAsync` na `_userManager` objektu (poskytované ``AccountController`` pomocí vkládání závislostí):</span><span class="sxs-lookup"><span data-stu-id="39ced-144">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="39ced-145">Pokud byl uživatel vytvořen úspěšně, uživatel je přihlášen pomocí volání ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="39ced-145">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="39ced-146">**Poznámka:** najdete v části [účet potvrzení](xref:security/authentication/accconfirm#prevent-login-at-registration) kroky, aby se zabránilo okamžitou přihlášení při registraci.</span><span class="sxs-lookup"><span data-stu-id="39ced-146">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="39ced-147">Přihlásit se.</span><span class="sxs-lookup"><span data-stu-id="39ced-147">Log in.</span></span>
 
    <span data-ttu-id="39ced-148">Uživatelé se mohou přihlásit kliknutím **přihlásit** odkaz v horní části webu, nebo se může přesměrováni na stránku přihlášení Pokud pokus o přístup k součástí lokality, která vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="39ced-148">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="39ced-149">Když uživatel odešle formulář na přihlašovací stránku, ``AccountController`` ``Login`` akce je volána.</span><span class="sxs-lookup"><span data-stu-id="39ced-149">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="39ced-150">``Login`` Volání akce ``PasswordSignInAsync`` na ``_signInManager`` objektu (poskytované ``AccountController`` pomocí vkládání závislostí).</span><span class="sxs-lookup"><span data-stu-id="39ced-150">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>
 
    <span data-ttu-id="39ced-151">Základní ``Controller`` třídy zpřístupňuje ``User`` vlastnost, která je přístupné z metody kontroleru.</span><span class="sxs-lookup"><span data-stu-id="39ced-151">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="39ced-152">Například můžete vytvořit výčet `User.Claims` a autorizační rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="39ced-152">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="39ced-153">Další informace najdete v tématu [autorizace](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="39ced-153">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="39ced-154">Odhlaste.</span><span class="sxs-lookup"><span data-stu-id="39ced-154">Log out.</span></span>
 
    <span data-ttu-id="39ced-155">Kliknutím **odhlášení** odkaz volání `LogOut` akce.</span><span class="sxs-lookup"><span data-stu-id="39ced-155">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="39ced-156">Předchozí kód výše volání `_signInManager.SignOutAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="39ced-156">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="39ced-157">`SignOutAsync` Metoda vymaže deklaracích identity uživatele v souborech cookie.</span><span class="sxs-lookup"><span data-stu-id="39ced-157">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="39ced-158">Konfigurace.</span><span class="sxs-lookup"><span data-stu-id="39ced-158">Configuration.</span></span>

    <span data-ttu-id="39ced-159">Identita má některé výchozí chování lze přepsat v třídě spuštění vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="39ced-159">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="39ced-160">Není potřeba konfigurovat ``IdentityOptions`` Pokud používáte výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="39ced-160">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="39ced-161">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="39ced-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="39ced-162">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="39ced-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="39ced-163">Další informace o tom, jak nakonfigurovat Identity najdete v tématu [konfigurace Identity](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="39ced-163">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="39ced-164">Můžete také konfigurovat datový typ primární klíč, najdete v části [konfigurace Identity primárních klíčů datový typ](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="39ced-164">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="39ced-165">Zobrazte databázi.</span><span class="sxs-lookup"><span data-stu-id="39ced-165">View the database.</span></span>

    <span data-ttu-id="39ced-166">Pokud vaše aplikace používá databázi systému SQL Server (výchozí možnost v systému Windows a pro uživatele v sadě Visual Studio), můžete zobrazit databázi aplikace vytvořená.</span><span class="sxs-lookup"><span data-stu-id="39ced-166">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="39ced-167">Můžete použít **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="39ced-167">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="39ced-168">Případně ze sady Visual Studio, vyberte možnost **zobrazení** -> **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="39ced-168">Alternatively, from Visual Studio, select **View** -> **SQL Server Object Explorer**.</span></span> <span data-ttu-id="39ced-169">Připojení k **\MSSQLLocalDB (localdb)**.</span><span class="sxs-lookup"><span data-stu-id="39ced-169">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="39ced-170">Databáze s odpovídajícím názvem  **aspnet - <*název projektu*>-<*datum řetězec*> ** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="39ced-170">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Kontextové nabídky na AspNetUsers databázové tabulky](identity/_static/04-db.png)
    
    <span data-ttu-id="39ced-172">Rozbalte databázi a její **tabulky**, klikněte pravým tlačítkem **dbo. AspNetUsers** tabulky a vyberte **Data zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="39ced-172">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

## <a name="identity-components"></a><span data-ttu-id="39ced-173">Komponent identity</span><span class="sxs-lookup"><span data-stu-id="39ced-173">Identity Components</span></span>

<span data-ttu-id="39ced-174">Je primární referenční sestavení pro systém identit `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="39ced-174">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="39ced-175">Tento balíček obsahuje sadu základní rozhraní pro ASP.NET Core Identity a je začleněn sám v `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="39ced-175">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="39ced-176">Tyto závislosti jsou potřeba k použití v aplikacích ASP.NET Core systém identit:</span><span class="sxs-lookup"><span data-stu-id="39ced-176">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="39ced-177">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Obsahuje požadované typy, které mají pomocí Identity Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="39ced-177">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="39ced-178">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core je technologie přístup doporučené dat společnosti Microsoft pro relační databáze jako SQL Server.</span><span class="sxs-lookup"><span data-stu-id="39ced-178">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="39ced-179">Pro testování, můžete použít `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="39ced-179">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="39ced-180">`Microsoft.AspNetCore.Authentication.Cookies`-Middleware, který umožňuje aplikaci používat ověřování na základě souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="39ced-180">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="39ced-181">Migrace na jádro ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="39ced-181">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="39ced-182">Pro další informace a pokyny k migraci stávající identitu úložiště najdete v tématu [migrace ověřování a identita](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="39ced-182">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="39ced-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="39ced-183">Next Steps</span></span>

* [<span data-ttu-id="39ced-184">Migrace ověřování a identita</span><span class="sxs-lookup"><span data-stu-id="39ced-184">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="39ced-185">Potvrzení účtu a heslo pro obnovení</span><span class="sxs-lookup"><span data-stu-id="39ced-185">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="39ced-186">Dvoufaktorové ověřování pomocí serveru SMS</span><span class="sxs-lookup"><span data-stu-id="39ced-186">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="39ced-187">Povolení ověřování pomocí služby Facebook, Google a dalších externích zprostředkovatelů</span><span class="sxs-lookup"><span data-stu-id="39ced-187">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
