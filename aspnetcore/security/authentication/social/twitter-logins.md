---
title: Instalační program služby Twitter externí přihlášení pomocí ASP.NET Core
author: rick-anderson
description: Tento kurz představuje integrační služby Twitter účet uživatele ověřování do existující aplikace ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/twitter-logins
ms.openlocfilehash: 3f59f7d1bf0280cef8f7757e8cd57d4872769b3d
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34688993"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="57665-103">Instalační program služby Twitter externí přihlášení pomocí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="57665-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="57665-104">Podle [Valeriy Novytskyy](https://github.com/01binary) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="57665-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="57665-105">V tomto kurzu se dozvíte, jak povolit uživatelům [přihlásit pomocí svého účtu služby Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) pomocí projektu ASP.NET 2.0 základní ukázka na vytvořit [předchozí stránce](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="57665-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="57665-106">Vytvoření aplikace v Twitter</span><span class="sxs-lookup"><span data-stu-id="57665-106">Create the app in Twitter</span></span>

* <span data-ttu-id="57665-107">Přejděte na [ https://apps.twitter.com/ ](https://apps.twitter.com/) a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="57665-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="57665-108">Pokud nemáte účet služby Twitter, pomocí **[nyní](https://twitter.com/signup)** odkaz k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="57665-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="57665-109">Po přihlášení, **Správa aplikací** stránky se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="57665-109">After signing in, the **Application Management** page is shown:</span></span>

![Otevřete správu aplikací v Microsoft Edge služby Twitter](index/_static/TwitterAppManage.png)

* <span data-ttu-id="57665-111">Klepněte na **vytvořit novou aplikaci** a vyplňte aplikace **název**, **popis** a veřejné **webu** identifikátor URI (může to být dočasný dokud Zaregistrujte název domény):</span><span class="sxs-lookup"><span data-stu-id="57665-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![Vytvoření stránky aplikace](index/_static/TwitterCreate.png)

* <span data-ttu-id="57665-113">Zadejte váš vývojový identifikátor URI s */signin-twitter* připojí do **identifikátory URI pro přesměrování platný OAuth** pole (například: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="57665-113">Enter your development URI with */signin-twitter* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="57665-114">Schéma ověřování služby Twitter nakonfigurované později v tomto kurzu bude automaticky zpracovávat požadavky na */signin-twitter* trasy k implementaci toku OAuth.</span><span class="sxs-lookup"><span data-stu-id="57665-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at */signin-twitter* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="57665-115">Vyplňte zbytek formuláře a klepněte na **vytvořit aplikaci služby Twitter**.</span><span class="sxs-lookup"><span data-stu-id="57665-115">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="57665-116">Podrobnosti o nové aplikace se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="57665-116">New application details are displayed:</span></span>

![Podrobnosti karty na stránce aplikace](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="57665-118">Při nasazování webu budete potřebovat k pokroku **Správa aplikací** stránky a zaregistrujte nový veřejný identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="57665-118">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="57665-119">Ukládání ConsumerKey služby Twitter a ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="57665-119">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="57665-120">Odkaz citlivá nastavení, jako je Twitter `Consumer Key` a `Consumer Secret` do konfigurace vaší aplikace pomocí [tajný klíč správce](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="57665-120">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="57665-121">Pro účely tohoto kurzu, název tokeny `Authentication:Twitter:ConsumerKey` a `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="57665-121">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="57665-122">Tyto tokeny můžete najít na **klíče a přístupové tokeny** karta po vytvoření nové aplikace služby Twitter:</span><span class="sxs-lookup"><span data-stu-id="57665-122">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Karta klíče a přístupové tokeny](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="57665-124">Nakonfigurujte ověřování služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="57665-124">Configure Twitter Authentication</span></span>

<span data-ttu-id="57665-125">Šablona projektu použili v tomto kurzu zajistí, že [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) balíček je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="57665-125">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="57665-126">Chcete-li nainstalovat tento balíček s Visual Studio 2017, klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="57665-126">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="57665-127">Chcete-li nainstalovat s .NET Core rozhraní příkazového řádku, spusťte následující v adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="57665-127">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="57665-128">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="57665-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="57665-129">Přidání služby Twitter `ConfigureServices` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="57665-129">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="57665-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="57665-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="57665-131">Přidat v middlewaru Twitter `Configure` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="57665-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

<span data-ttu-id="57665-132">Najdete v článku [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) referenční dokumentace rozhraní API pro další informace o možností konfigurace podporovanou ověřováním služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="57665-132">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="57665-133">To slouží k požadavku na jiné informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="57665-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="57665-134">Přihlaste se pomocí služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="57665-134">Sign in with Twitter</span></span>

<span data-ttu-id="57665-135">Spusťte aplikaci a klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="57665-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="57665-136">Zobrazí se možnost přihlásit se přes Twitter:</span><span class="sxs-lookup"><span data-stu-id="57665-136">An option to sign in with Twitter appears:</span></span>

![Webové aplikace: uživatel není ověřen.](index/_static/DoneTwitter.png)

<span data-ttu-id="57665-138">Kliknutím na **Twitter** přesměruje na Twitteru pro ověřování:</span><span class="sxs-lookup"><span data-stu-id="57665-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Stránka ověřování služby Twitter.](index/_static/TwitterLogin.png)

<span data-ttu-id="57665-140">Po zadání přihlašovacích údajů služby Twitter, budete přesměrováni zpět na webovou stránku, kde můžete nastavit e-mailu.</span><span class="sxs-lookup"><span data-stu-id="57665-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="57665-141">Nyní jste se přihlásili pomocí přihlašovacích údajů služby Twitter:</span><span class="sxs-lookup"><span data-stu-id="57665-141">You are now logged in using your Twitter credentials:</span></span>

![Webové aplikace: uživatel ověřený](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="57665-143">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="57665-143">Troubleshooting</span></span>

* <span data-ttu-id="57665-144">**ASP.NET Core 2.x pouze:** pokud identita není nastavena voláním `services.AddIdentity` v `ConfigureServices`, pokusu o ověření bude mít za následek *ArgumentException –: musí být použita volba 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="57665-144">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="57665-145">Šablona projektu použili v tomto kurzu zajistí, že to probíhá.</span><span class="sxs-lookup"><span data-stu-id="57665-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="57665-146">Pokud databázi lokality s použitím počáteční migrace vytvořena nebyla, zobrazí se *databázová operace se nezdařila při zpracování požadavku* chyby.</span><span class="sxs-lookup"><span data-stu-id="57665-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="57665-147">Klepněte na **použít migrace** vytvořit databázi a aktualizujte pokračujte dále chyba.</span><span class="sxs-lookup"><span data-stu-id="57665-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57665-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="57665-148">Next steps</span></span>

* <span data-ttu-id="57665-149">Tento článek vám ukázal, jak můžete ověřit pomocí služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="57665-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="57665-150">Můžete postupovat podle podobný postup k ověření pomocí jiných poskytovatelů uvedené na [předchozí stránce](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="57665-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="57665-151">Jakmile budete publikovat web vaší webové aplikace Azure, byste měli obnovit `ConsumerSecret` v portálu pro vývojáře služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="57665-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="57665-152">Nastavte `Authentication:Twitter:ConsumerKey` a `Authentication:Twitter:ConsumerSecret` jako nastavení aplikace v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="57665-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="57665-153">Konfigurace systému je nastavený čtení klíčů z proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="57665-153">The configuration system is set up to read keys from environment variables.</span></span>
