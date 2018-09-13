---
title: Úvod do Identity v ASP.NET Core
author: rick-anderson
description: Pomocí Identity aplikace v ASP.NET Core. Zjistěte, jak nastavit požadavky na heslo (RequireDigit RequiredLength, RequiredUniqueChars a další).
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: af07adcc7f9513845bb91eb233f0a9840e1bd6f4
ms.sourcegitcommit: 4db337bd47d70c06fff91000c58bc048a491ccec
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2018
ms.locfileid: "44749305"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="61a34-104">Úvod do Identity v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61a34-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="61a34-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="61a34-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="61a34-106">ASP.NET Core Identity je systém členství, který přidá funkce přihlášení do aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61a34-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="61a34-107">Uživatelé můžou vytvářet účet s přihlašovací údaje uložené v Identity nebo může použít externího zprostředkovatele přihlášení.</span><span class="sxs-lookup"><span data-stu-id="61a34-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="61a34-108">Podporované externí přihlášení zprostředkovatele patří [Facebook, Google, Account Microsoft a Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="61a34-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="61a34-109">Identity lze konfigurovat pomocí databáze SQL serveru ukládat uživatelská jména, hesla a data profilu.</span><span class="sxs-lookup"><span data-stu-id="61a34-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="61a34-110">Můžete také jiné trvalého úložiště je možné, například Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="61a34-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

[<span data-ttu-id="61a34-111">Zobrazení nebo stažení ukázkového kódu.</span><span class="sxs-lookup"><span data-stu-id="61a34-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="61a34-112">(Jak stáhnout)</span><span class="sxs-lookup"><span data-stu-id="61a34-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

<span data-ttu-id="61a34-113">V tomto tématu se naučíte, jak zaregistrovat, přihlaste se pomocí Identity a odhlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="61a34-113">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="61a34-114">Podrobnější pokyny k vytváření aplikací, které používají Identity najdete v části Další kroky na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="61a34-114">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>
## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="61a34-115">AddDefaultIdentity a AddIdentity</span><span class="sxs-lookup"><span data-stu-id="61a34-115">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="61a34-116">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) byla zavedena v ASP. Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="61a34-116">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.Core 2.1.</span></span> <span data-ttu-id="61a34-117">Volání `AddDefaultIdentity` je podobná následující volání:</span><span class="sxs-lookup"><span data-stu-id="61a34-117">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="61a34-118">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="61a34-118">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="61a34-119">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="61a34-119">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="61a34-120">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="61a34-120">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="61a34-121">Zobrazit [AddDefaultIdentity zdroj](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64) Další informace.</span><span class="sxs-lookup"><span data-stu-id="61a34-121">See [AddDefaultIdentity source](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="61a34-122">Vytvoření webové aplikace s ověřováním</span><span class="sxs-lookup"><span data-stu-id="61a34-122">Create a Web app with authentication</span></span>

<span data-ttu-id="61a34-123">Vytvoření projektu webové aplikace ASP.NET Core s jednotlivými uživatelskými účty.</span><span class="sxs-lookup"><span data-stu-id="61a34-123">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61a34-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61a34-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="61a34-125">Vyberte **souboru** > **nové** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="61a34-125">Select **File** > **New** > **Project**.</span></span> 
* <span data-ttu-id="61a34-126">Vyberte **webová aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="61a34-126">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="61a34-127">Pojmenujte projekt **WebApp1** stejný obor názvů jako projekt soubor ke stažení.</span><span class="sxs-lookup"><span data-stu-id="61a34-127">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="61a34-128">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="61a34-128">Click **OK**.</span></span>
* <span data-ttu-id="61a34-129">Vyberte v ASP.NET Core **webovou aplikaci** ASP.NET Core 2.1 vyberte **změna ověřování**.</span><span class="sxs-lookup"><span data-stu-id="61a34-129">Select an ASP.NET Core **Web Application** for ASP.NET Core 2.1, then select **Change Authentication**.</span></span>
* <span data-ttu-id="61a34-130">Vyberte **jednotlivé uživatelské účty** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="61a34-130">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="61a34-131">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="61a34-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="61a34-132">Generovaný projekt poskytuje [ASP.NET Core Identity](xref:security/authentication/identity) jako [knihovny tříd Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="61a34-132">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="61a34-133">Test registrace a přihlášení</span><span class="sxs-lookup"><span data-stu-id="61a34-133">Test Register and Login</span></span>

<span data-ttu-id="61a34-134">Spusťte aplikaci a zaregistrovat uživatele.</span><span class="sxs-lookup"><span data-stu-id="61a34-134">Run the app and register a user.</span></span> <span data-ttu-id="61a34-135">V závislosti na velikost obrazovky může být nutné vybrat navigace přepínacího tlačítka zobrazíte **zaregistrovat** a **přihlášení** odkazy.</span><span class="sxs-lookup"><span data-stu-id="61a34-135">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

![přepínací tlačítko navigační panel](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="61a34-137">Konfigurace Identity služby</span><span class="sxs-lookup"><span data-stu-id="61a34-137">Configure Identity services</span></span>

<span data-ttu-id="61a34-138">Služby jsou přidány v `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="61a34-138">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="61a34-139">Typický vzor je volat všechny `Add{Service}` metody a poté zavolejte všechny `services.Configure{Service}` metody.</span><span class="sxs-lookup"><span data-stu-id="61a34-139">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span> <span data-ttu-id="61a34-140">Následující kód neobsahuje vygenerovanou šablonu `CookiePolicyOptions`:</span><span class="sxs-lookup"><span data-stu-id="61a34-140">The following code doesn't include the template generated `CookiePolicyOptions`:</span></span>

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="61a34-141">Předchozí kód konfiguruje Identity s výchozími hodnotami. možnost.</span><span class="sxs-lookup"><span data-stu-id="61a34-141">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="61a34-142">Služby jsou k dispozici pro aplikaci přes [injektáž závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="61a34-142">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="61a34-143">Povolení identity voláním [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="61a34-143">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="61a34-144">`UseAuthentication` Přidá ověřování [middleware](xref:fundamentals/middleware/index) požadavku kanálu.</span><span class="sxs-lookup"><span data-stu-id="61a34-144">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="61a34-145">Služby jsou k dispozici pro aplikace prostřednictvím [injektáž závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="61a34-145">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="61a34-146">Povolení identity pro aplikace po zavolání `UseAuthentication` v `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="61a34-146">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="61a34-147">`UseAuthentication` Přidá ověřování [middleware](xref:fundamentals/middleware/index) požadavku kanálu.</span><span class="sxs-lookup"><span data-stu-id="61a34-147">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="61a34-148">Tyto služby jsou k dispozici pro aplikace prostřednictvím [injektáž závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="61a34-148">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="61a34-149">Povolení identity pro aplikace po zavolání `UseIdentity` v `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="61a34-149">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="61a34-150">`UseIdentity` Přidá na základě souboru cookie ověřování [middleware](xref:fundamentals/middleware/index) požadavku kanálu.</span><span class="sxs-lookup"><span data-stu-id="61a34-150">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="61a34-151">Další informace najdete v tématu [IdentityOptions třídy](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) a [spuštění aplikace](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="61a34-151">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="61a34-152">Vygenerované uživatelské rozhraní registrace, přihlášení a odhlášení</span><span class="sxs-lookup"><span data-stu-id="61a34-152">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="61a34-153">Postupujte podle [generování uživatelského rozhraní identity do projektu Razor s autorizací](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) pokyny.</span><span class="sxs-lookup"><span data-stu-id="61a34-153">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61a34-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61a34-154">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="61a34-155">Přidáte soubory registrace, přihlášení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="61a34-155">Add the Register, Login, and LogOut files.</span></span>


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="61a34-156">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="61a34-156">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="61a34-157">Pokud jste vytvořili projekt s názvem **WebApp1**, spusťte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="61a34-157">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="61a34-158">Jinak použijte správný obor názvů pro `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="61a34-158">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>


```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"

```

<span data-ttu-id="61a34-159">PowerShell používá jako oddělovač příkazu středník.</span><span class="sxs-lookup"><span data-stu-id="61a34-159">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="61a34-160">Při použití prostředí PowerShell, řídicí středníky v seznamu souborů nebo vložit seznam souborů v dvojitých uvozovkách, jak ukazuje předchozí příklad.</span><span class="sxs-lookup"><span data-stu-id="61a34-160">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="61a34-161">Prozkoumejte registru</span><span class="sxs-lookup"><span data-stu-id="61a34-161">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="61a34-162">Když uživatel klikne **zaregistrovat** odkaz, `RegisterModel.OnPostAsync` vyvolání akce.</span><span class="sxs-lookup"><span data-stu-id="61a34-162">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="61a34-163">Uživatel vytvoří [asynchronně](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) na `_userManager` objektu.</span><span class="sxs-lookup"><span data-stu-id="61a34-163">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="61a34-164">`_userManager` poskytuje injektáž závislostí):</span><span class="sxs-lookup"><span data-stu-id="61a34-164">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="61a34-165">Když uživatel klikne **zaregistrovat** odkaz, `Register` akce je volána na `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="61a34-165">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="61a34-166">`Register` Akce vytvoří uživatele voláním `CreateAsync` na `_userManager` objektu (poskytnuté `AccountController` pomocí vkládání závislostí):</span><span class="sxs-lookup"><span data-stu-id="61a34-166">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="61a34-167">Pokud byl uživatel vytvořen úspěšně, uživatel je přihlášen voláním `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="61a34-167">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="61a34-168">**Poznámka:** naleznete v tématu [účtu potvrzení](xref:security/authentication/accconfirm#prevent-login-at-registration) kroky, jak zabránit okamžité přihlášení při registraci.</span><span class="sxs-lookup"><span data-stu-id="61a34-168">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="61a34-169">Přihlásit se</span><span class="sxs-lookup"><span data-stu-id="61a34-169">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="61a34-170">Přihlašovací formulář se zobrazí při:</span><span class="sxs-lookup"><span data-stu-id="61a34-170">The Login form is displayed when:</span></span>

* <span data-ttu-id="61a34-171">**Přihlášení** vybraný odkaz.</span><span class="sxs-lookup"><span data-stu-id="61a34-171">The **Log in** link  is selected.</span></span>
* <span data-ttu-id="61a34-172">Když uživatel přistupuje k stránku, kde nejsou ověřené **nebo** ověřen, bude přesměrován na přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="61a34-172">When a user accesses a page where they are not authenticated **or** authorized, they are redirected to the Login page.</span></span> 

<span data-ttu-id="61a34-173">Když se odešle formulář na přihlašovací stránku, `OnPostAsync` akce je volána.</span><span class="sxs-lookup"><span data-stu-id="61a34-173">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="61a34-174">`PasswordSignInAsync` je volán na `_signInManager` objektu (postkytovatel: injektáž závislostí).</span><span class="sxs-lookup"><span data-stu-id="61a34-174">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="61a34-175">Základní `Controller` třídy zpřístupňuje `User` vlastnost, která se dá dostat z metody kontroleru.</span><span class="sxs-lookup"><span data-stu-id="61a34-175">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="61a34-176">Například můžete zobrazit výčet `User.Claims` a rozhodnutí o autorizaci.</span><span class="sxs-lookup"><span data-stu-id="61a34-176">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="61a34-177">Další informace najdete v tématu [autorizace](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="61a34-177">For more information, see [Authorization](xref:security/authorization/index).</span></span>

::: moniker-end
::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="61a34-178">Přihlašovací formulář se zobrazí, když uživatelé vyberou **přihlášení** propojení nebo přesměrováni při přístupu ke stránce, která vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="61a34-178">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="61a34-179">Když uživatel odešle formulář na přihlašovací stránku, `AccountController` `Login` akce je volána.</span><span class="sxs-lookup"><span data-stu-id="61a34-179">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="61a34-180">`Login` Volání akce `PasswordSignInAsync` na `_signInManager` objektu (poskytnuté `AccountController` pomocí vkládání závislostí).</span><span class="sxs-lookup"><span data-stu-id="61a34-180">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="61a34-181">Základní (`Controller` nebo `PageModel`) třídy zpřístupňuje `User` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="61a34-181">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="61a34-182">Například `User.Claims` může být přezkoumána za účelem rozhodnutí o autorizaci.</span><span class="sxs-lookup"><span data-stu-id="61a34-182">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="61a34-183">Odhlásit se</span><span class="sxs-lookup"><span data-stu-id="61a34-183">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="61a34-184">**Odhlásit** vyvolá tento odkaz `LogoutModel.OnPost` akce.</span><span class="sxs-lookup"><span data-stu-id="61a34-184">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

<span data-ttu-id="61a34-185">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) vymaže deklarací identity uživatele uložené v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="61a34-185">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="61a34-186">Není přesměrování po volání `SignOutAsync` nebo uživatel uvidí **není** odhlásit.</span><span class="sxs-lookup"><span data-stu-id="61a34-186">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="61a34-187">Příspěvek je zadán v *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="61a34-187">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"
   <span data-ttu-id="61a34-188">Kliknutím **Odhlásit** propojit volání `LogOut` akce.</span><span class="sxs-lookup"><span data-stu-id="61a34-188">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="61a34-189">Předchozí kód volá `_signInManager.SignOutAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="61a34-189">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="61a34-190">`SignOutAsync` Metoda vymaže deklarací identity uživatele uložené v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="61a34-190">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="61a34-191">Otestování Identity</span><span class="sxs-lookup"><span data-stu-id="61a34-191">Test Identity</span></span>

<span data-ttu-id="61a34-192">Šablony webových projektů výchozí povolit anonymní přístup k domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="61a34-192">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="61a34-193">K otestování Identity, přidejte [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) na stránku o.</span><span class="sxs-lookup"><span data-stu-id="61a34-193">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the About page.</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

<span data-ttu-id="61a34-194">Pokud jste přihlášeni, odhlaste se. Spusťte aplikaci a vyberte **o** odkaz.</span><span class="sxs-lookup"><span data-stu-id="61a34-194">If you are signed in, sign out. Run the app and select the **About** link.</span></span> <span data-ttu-id="61a34-195">Budete přesměrováni na přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="61a34-195">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="61a34-196">Prozkoumejte službu Identity</span><span class="sxs-lookup"><span data-stu-id="61a34-196">Explore Identity</span></span>

<span data-ttu-id="61a34-197">Identita prozkoumat podrobněji:</span><span class="sxs-lookup"><span data-stu-id="61a34-197">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="61a34-198">Vytvoření zdroje plnou identitou uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="61a34-198">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="61a34-199">Zkontrolujte příčiny jednotlivé stránky a projít ladicí program.</span><span class="sxs-lookup"><span data-stu-id="61a34-199">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="61a34-200">Komponenty identit</span><span class="sxs-lookup"><span data-stu-id="61a34-200">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="61a34-201">Všechny Identity závislé balíčky NuGet jsou součástí [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="61a34-201">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
::: moniker-end

<span data-ttu-id="61a34-202">Primární balíček pro identitu [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="61a34-202">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="61a34-203">Tento balíček obsahuje základní sadu rozhraní pro ASP.NET Core Identity a je součástí `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="61a34-203">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="61a34-204">Migrace do ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="61a34-204">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="61a34-205">Další informace a pokyny k migraci vaší existující úložiště identit najdete v tématu [migrovat ověřování a identita](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="61a34-205">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="61a34-206">Nastavení síly hesla</span><span class="sxs-lookup"><span data-stu-id="61a34-206">Setting password strength</span></span>

<span data-ttu-id="61a34-207">Zobrazit [konfigurace](#pw) ukázku, která nastavuje minimální požadavky.</span><span class="sxs-lookup"><span data-stu-id="61a34-207">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="61a34-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61a34-208">Next Steps</span></span>

* [<span data-ttu-id="61a34-209">Konfigurace systému Identity</span><span class="sxs-lookup"><span data-stu-id="61a34-209">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <span data-ttu-id="61a34-210">[Konfigurace Identity primárních klíčů datový typ](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="61a34-210">[Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
