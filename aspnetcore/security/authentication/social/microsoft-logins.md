---
title: Nastavení Microsoft Account externí přihlášení pomocí ASP.NET Core
author: rick-anderson
description: Tento kurz představuje integraci ověřování uživatele účtu Microsoft do existující aplikace ASP.NET Core.
ms.author: riande
ms.date: 08/24/2017
uid: security/authentication/microsoft-logins
ms.openlocfilehash: cc4fe8c71b97d29cc6697e2aebf04694afb753ec
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272773"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="4ea0d-103">Nastavení Microsoft Account externí přihlášení pomocí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4ea0d-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="4ea0d-104">Podle [Valeriy Novytskyy](https://github.com/01binary) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4ea0d-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4ea0d-105">V tomto kurzu se dozvíte, jak povolit uživatelům přihlásit se pomocí svého účtu Microsoft pomocí projektu ASP.NET 2.0 základní ukázka na vytvořit [předchozí stránce](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="4ea0d-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="4ea0d-106">Vytvoření aplikace v portálu pro vývojáře společnosti Microsoft</span><span class="sxs-lookup"><span data-stu-id="4ea0d-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="4ea0d-107">Přejděte na [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) a vytvořte nebo se přihlaste účtem Microsoft:</span><span class="sxs-lookup"><span data-stu-id="4ea0d-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![Přihlaste se dialogové okno](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="4ea0d-109">Pokud nemáte účet Microsoft, klepněte na  **[vytvořit!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="4ea0d-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="4ea0d-110">Po přihlášení budete přesměrováni na **Moje aplikace** stránky:</span><span class="sxs-lookup"><span data-stu-id="4ea0d-110">After signing in you are redirected to **My applications** page:</span></span>

![Otevřete v Microsoft Edge portál pro vývojáře společnosti Microsoft](index/_static/MicrosoftDev.png)

* <span data-ttu-id="4ea0d-112">Klepněte na **přidat aplikaci** v pravém horním rohu a zadejte vaše **název aplikace** a **e-mailu kontaktujte**:</span><span class="sxs-lookup"><span data-stu-id="4ea0d-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Dialogové okno Nový registraci aplikace](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="4ea0d-114">Pro účely tohoto kurzu, zrušte **instalace na základě** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="4ea0d-115">Klepněte na **vytvořit** nadále **registrace** stránky.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="4ea0d-116">Zadejte **název** a poznamenejte si hodnotu **Id aplikace**, který použijete jako `ClientId` dál v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="4ea0d-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Registrační stránce](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="4ea0d-118">Klepněte na **přidejte platformu** v **platformy** a vyberte **webové** platformy:</span><span class="sxs-lookup"><span data-stu-id="4ea0d-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Přidejte platformu dialogové okno](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="4ea0d-120">V novém **webové** platformy zadejte URL vývoj s `/signin-microsoft` připojí do **adres URL pro přesměrování** pole (například: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="4ea0d-120">In the new **Web** platform section, enter your development URL with `/signin-microsoft` appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="4ea0d-121">Schéma ověřování Microsoft nakonfigurované později v tomto kurzu bude automaticky zpracovávat požadavky na `/signin-microsoft` trasy k implementaci toku OAuth:</span><span class="sxs-lookup"><span data-stu-id="4ea0d-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow:</span></span>

![Webové části platforma](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> <span data-ttu-id="4ea0d-123">Segment identifikátoru URI `/signin-microsoft` je nastaven jako výchozí zpětného volání zprostředkovatele ověřování společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="4ea0d-124">Můžete změnit výchozí identifikátor URI zpětného volání při konfiguraci middlewaru ověřování Microsoft prostřednictvím zděděnou [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) vlastnost [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) třídy.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

* <span data-ttu-id="4ea0d-125">Klepněte na **přidat adresu URL** aby adresa URL byla přidána.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-125">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="4ea0d-126">V případě potřeby zadejte další nastavení aplikace a klepněte na **Uložit** v dolní části stránky se uložit změny do konfigurace aplikací.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-126">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="4ea0d-127">Při nasazování webu budete potřebovat k pokroku **registrace** stránky a nastavte novou veřejnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-127">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="4ea0d-128">Uložit aplikaci Microsoft Id a heslo</span><span class="sxs-lookup"><span data-stu-id="4ea0d-128">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="4ea0d-129">Poznámka: `Application Id` zobrazí na **registrace** stránky.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-129">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="4ea0d-130">Klepněte na **generovat nové heslo** v **tajné klíče aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-130">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="4ea0d-131">Zobrazí se v poli, kde můžete kopírovat heslo aplikace:</span><span class="sxs-lookup"><span data-stu-id="4ea0d-131">This displays a box where you can copy the application password:</span></span>

![Dialogové okno Nový hesla generovaného](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="4ea0d-133">Odkaz citlivá nastavení, jako je Microsoft `Application ID` a `Password` do konfigurace vaší aplikace pomocí [tajný klíč správce](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="4ea0d-133">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="4ea0d-134">Pro účely tohoto kurzu, název tokeny `Authentication:Microsoft:ApplicationId` a `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-134">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="4ea0d-135">Konfigurovat ověřování účet Microsoft</span><span class="sxs-lookup"><span data-stu-id="4ea0d-135">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="4ea0d-136">Šablona projektu použili v tomto kurzu zajistí, že [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) balíček je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-136">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="4ea0d-137">Chcete-li nainstalovat tento balíček s Visual Studio 2017, klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="4ea0d-138">Chcete-li nainstalovat s .NET Core rozhraní příkazového řádku, spusťte následující v adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="4ea0d-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ea0d-139">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="4ea0d-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="4ea0d-140">Přidání služby Account Microsoft `ConfigureServices` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="4ea0d-140">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ea0d-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ea0d-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="4ea0d-142">Přidat middlewaru Microsoft Account v `Configure` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="4ea0d-142">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

<span data-ttu-id="4ea0d-143">I když tyto tokeny názvy technologiím použitým na portál pro vývojáře společnosti Microsoft `ApplicationId` a `Password`, že zveřejněné jako `ClientId` a `ClientSecret` v konfiguraci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-143">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="4ea0d-144">Najdete v článku [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) referenční dokumentace rozhraní API pro další informace o možností konfigurace podporovanou Account Microsoft ověřování.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-144">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="4ea0d-145">To slouží k požadavku na jiné informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-145">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="4ea0d-146">Přihlaste se pomocí účtu Microsoft</span><span class="sxs-lookup"><span data-stu-id="4ea0d-146">Sign in with Microsoft Account</span></span>

<span data-ttu-id="4ea0d-147">Spusťte aplikaci a klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-147">Run your application and click **Log in**.</span></span> <span data-ttu-id="4ea0d-148">Zobrazí se možnost přihlásit se pomocí Microsoft:</span><span class="sxs-lookup"><span data-stu-id="4ea0d-148">An option to sign in with Microsoft appears:</span></span>

![Webovou aplikaci protokolu na stránce: uživatel není ověřen.](index/_static/DoneMicrosoft.png)

<span data-ttu-id="4ea0d-150">Když kliknete na Microsoft, budete přesměrováni do společnosti Microsoft pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-150">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="4ea0d-151">Po přihlášení pomocí Account Microsoft (pokud ještě není přihlášení) se zobrazí výzva k umožňuje aplikaci přístup k informacím:</span><span class="sxs-lookup"><span data-stu-id="4ea0d-151">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Dialogové okno Microsoft ověřování](index/_static/MicrosoftLogin.png)

<span data-ttu-id="4ea0d-153">Klepněte na **Ano** a budete přesměrováni zpět na webovou stránku, kde můžete nastavit e-mailu.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-153">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="4ea0d-154">Nyní jste se přihlásili pomocí přihlašovacích údajů společnosti Microsoft:</span><span class="sxs-lookup"><span data-stu-id="4ea0d-154">You are now logged in using your Microsoft credentials:</span></span>

![Webové aplikace: uživatel ověřený](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="4ea0d-156">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="4ea0d-156">Troubleshooting</span></span>

* <span data-ttu-id="4ea0d-157">Pokud zprostředkovatel Account Microsoft vás přesměruje na přihlašovací stránce chyba, vezměte na vědomí chyba název a popis parametrů řetězce dotazu přímo následující `#` (hashtag) v identifikátoru Uri.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-157">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="4ea0d-158">I když se chybová zpráva se zdá, že znamenat problém související s ověřováním Microsoft, nejčastější příčinou je identifikátor Uri neodpovídá žádné z vaší aplikace **identifikátory URI přesměrování** zadaná pro **webové** platformy .</span><span class="sxs-lookup"><span data-stu-id="4ea0d-158">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="4ea0d-159">**ASP.NET Core 2.x pouze:** pokud identita není nastavena voláním `services.AddIdentity` v `ConfigureServices`, pokusu o ověření bude mít za následek *ArgumentException –: musí být použita volba 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-159">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="4ea0d-160">Šablona projektu použili v tomto kurzu zajistí, že to probíhá.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-160">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="4ea0d-161">Pokud databázi lokality s použitím počáteční migrace vytvořena nebyla, zobrazí se *databázová operace se nezdařila při zpracování požadavku* chyby.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-161">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="4ea0d-162">Klepněte na **použít migrace** vytvořit databázi a aktualizujte pokračujte dále chyba.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-162">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4ea0d-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4ea0d-163">Next steps</span></span>

* <span data-ttu-id="4ea0d-164">Tento článek vám ukázal, jak můžete ověřovat se společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-164">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="4ea0d-165">Můžete postupovat podle podobný postup k ověření pomocí jiných poskytovatelů uvedené na [předchozí stránce](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="4ea0d-165">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="4ea0d-166">Jakmile budete publikovat web vaší webové aplikace Azure, měli byste vytvořit nový `Password` v portálu pro vývojáře společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-166">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="4ea0d-167">Nastavte `Authentication:Microsoft:ApplicationId` a `Authentication:Microsoft:Password` jako nastavení aplikace v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-167">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="4ea0d-168">Konfigurace systému je nastavený čtení klíčů z proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="4ea0d-168">The configuration system is set up to read keys from environment variables.</span></span>
