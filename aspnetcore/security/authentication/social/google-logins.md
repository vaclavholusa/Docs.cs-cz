---
title: "Instalační program externí přihlášení Google v ASP.NET Core"
author: rick-anderson
description: "Tento kurz představuje integraci ověřování uživatele účet Google do existující aplikace ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/google-logins
ms.openlocfilehash: 30d224061bce3a727fc31d19c194e96559e28310
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="configuring-google-authentication-in-aspnet-core"></a><span data-ttu-id="160a4-103">Konfigurace ověřování Google v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="160a4-103">Configuring Google authentication in ASP.NET Core</span></span>

<span data-ttu-id="160a4-104">Podle [Valeriy Novytskyy](https://github.com/01binary) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="160a4-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="160a4-105">V tomto kurzu se dozvíte, jak povolit uživatelům přihlásit se pomocí účtu své Google + pomocí projektu ASP.NET 2.0 základní ukázka na vytvořit [předchozí stránce](index.md).</span><span class="sxs-lookup"><span data-stu-id="160a4-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="160a4-106">Začneme podle [oficiální kroky](https://developers.google.com/identity/sign-in/web/devconsole-project) k vytvoření nové aplikace v konzole rozhraní API Google.</span><span class="sxs-lookup"><span data-stu-id="160a4-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="160a4-107">Vytvořit aplikaci v konzole rozhraní API Google</span><span class="sxs-lookup"><span data-stu-id="160a4-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="160a4-108">Přejděte na [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="160a4-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="160a4-109">Pokud nemáte účet Google, použijte **další možnosti** > **[vytvořit účet](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  odkaz k jeho vytvoření:</span><span class="sxs-lookup"><span data-stu-id="160a4-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Konzoly rozhraní Google API](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="160a4-111">Budete přesměrováni na **knihovna rozhraní API Správce** stránky:</span><span class="sxs-lookup"><span data-stu-id="160a4-111">You are redirected to **API Manager Library** page:</span></span>

![Stránka rozhraní API Správce knihovny](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="160a4-113">Klepněte na **vytvořit** a zadejte vaše **název projektu**:</span><span class="sxs-lookup"><span data-stu-id="160a4-113">Tap **Create** and enter your **Project name**:</span></span>

![Dialogové okno Nový projekt](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="160a4-115">Po přijetí dialogové okno, budete přesměrováni zpět na stránku knihovny si vybrat funkce pro novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="160a4-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="160a4-116">Najít **rozhraní API Google +** v seznamu a klikněte na jeho odkazu přidat funkci rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="160a4-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Stránka rozhraní API Správce knihovny](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="160a4-118">Zobrazí se stránka pro nově přidaného rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="160a4-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="160a4-119">Klepněte na **povolit** pro přidání Google + se funkce do vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="160a4-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![Správce rozhraní API Google + API stránky](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="160a4-121">Po povolení rozhraní API, klepněte na **vytvořit přihlašovací údaje** ke konfiguraci těchto tajných klíčů:</span><span class="sxs-lookup"><span data-stu-id="160a4-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![Správce rozhraní API Google + API stránky](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="160a4-123">Zvolte:</span><span class="sxs-lookup"><span data-stu-id="160a4-123">Choose:</span></span>
   * <span data-ttu-id="160a4-124">**Google + rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="160a4-124">**Google+ API**</span></span>
   * <span data-ttu-id="160a4-125">**Webový server (například node.js, Tomcat)**, a</span><span class="sxs-lookup"><span data-stu-id="160a4-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
   * <span data-ttu-id="160a4-126">**Uživatelská data**:</span><span class="sxs-lookup"><span data-stu-id="160a4-126">**User data**:</span></span>

![Stránka přihlašovací údaje rozhraní API Správce: Podívejte se, co typ pověření budete potřebovat panely](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="160a4-128">Klepněte na **jaké přihlašovací údaje potřebuji?** kterého přejdete na druhý krok konfigurace aplikací **vytvoření ID klienta OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="160a4-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![Stránka přihlašovací údaje rozhraní API Správce: vytvoření ID klienta OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="160a4-130">Protože vytváříme projektu Google + s jedním funkcí (přihlášení), můžeme zadat stejný **název** pro ID klienta OAuth 2.0, jako jsme použili pro projekt.</span><span class="sxs-lookup"><span data-stu-id="160a4-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="160a4-131">Zadejte váš vývojový identifikátor URI s */signin-google* připojí do **autorizováno přesměrování identifikátory URI** pole (například: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="160a4-131">Enter your development URI with */signin-google* appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="160a4-132">Ověřování Google nakonfigurované později v tomto kurzu bude automaticky zpracovávat požadavky na */signin-google* trasy k implementaci toku OAuth.</span><span class="sxs-lookup"><span data-stu-id="160a4-132">The Google authentication configured later in this tutorial will automatically handle requests at */signin-google* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="160a4-133">Stisknutím klávesy TAB přidat **autorizováno přesměrování identifikátory URI** položku.</span><span class="sxs-lookup"><span data-stu-id="160a4-133">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="160a4-134">Klepněte na **vytvořit ID klienta**, která přejde třetí krok **nastavení na obrazovce souhlas OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="160a4-134">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![Stránka přihlašovací údaje rozhraní API Správce: nastavení na obrazovce souhlas OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="160a4-136">Zadejte vaše veřejné přístupem **e-mailová adresa** a **název produktu** zobrazené pro vaši aplikaci při Google + vyzývá uživatele k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="160a4-136">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="160a4-137">Další možnosti jsou k dispozici v části **další možnosti přizpůsobení**.</span><span class="sxs-lookup"><span data-stu-id="160a4-137">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="160a4-138">Klepněte na **pokračovat** k přejděte k poslednímu kroku **stáhnout přihlašovací údaje**:</span><span class="sxs-lookup"><span data-stu-id="160a4-138">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![Stránka přihlašovací údaje rozhraní API Správce: Stáhněte si přihlašovací údaje](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="160a4-140">Klepněte na **Stáhnout** uložit soubor JSON s tajné klíče aplikace, a **provádí** k dokončení vytvoření nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="160a4-140">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="160a4-141">Při nasazování webu budete potřebovat k pokroku **konzoly Google** a zaregistrujte novou veřejnou adresu url.</span><span class="sxs-lookup"><span data-stu-id="160a4-141">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="160a4-142">Store Google ClientID a ClientSecret</span><span class="sxs-lookup"><span data-stu-id="160a4-142">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="160a4-143">Odkaz citlivá nastavení, jako je Google `Client ID` a `Client Secret` do konfigurace vaší aplikace pomocí [tajný klíč správce](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="160a4-143">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="160a4-144">Pro účely tohoto kurzu, název tokeny `Authentication:Google:ClientId` a `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="160a4-144">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="160a4-145">Hodnoty pro tyto tokeny naleznete v souboru JSON stáhli v předchozím kroku v části `web.client_id` a `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="160a4-145">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="160a4-146">Konfigurace ověřování Google</span><span class="sxs-lookup"><span data-stu-id="160a4-146">Configure Google Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="160a4-147">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="160a4-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="160a4-148">Přidání služby Google `ConfigureServices` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="160a4-148">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="160a4-149">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="160a4-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="160a4-150">Šablona projektu použili v tomto kurzu zajistí, že [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) balíček nainstalován.</span><span class="sxs-lookup"><span data-stu-id="160a4-150">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

 * <span data-ttu-id="160a4-151">Chcete-li nainstalovat tento balíček s Visual Studio 2017, klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="160a4-151">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
 * <span data-ttu-id="160a4-152">Chcete-li nainstalovat s .NET Core rozhraní příkazového řádku, spusťte následující v adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="160a4-152">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="160a4-153">Přidat v middlewaru Google `Configure` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="160a4-153">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

<span data-ttu-id="160a4-154">Najdete v článku [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) referenční dokumentace rozhraní API pro další informace o možnostech konfigurace nepodporuje ověřování Google.</span><span class="sxs-lookup"><span data-stu-id="160a4-154">See the [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="160a4-155">To slouží k požadavku na jiné informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="160a4-155">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="160a4-156">Přihlaste se pomocí služby Google</span><span class="sxs-lookup"><span data-stu-id="160a4-156">Sign in with Google</span></span>

<span data-ttu-id="160a4-157">Spusťte aplikaci a klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="160a4-157">Run your application and click **Log in**.</span></span> <span data-ttu-id="160a4-158">Zobrazí se možnost přihlásit se přes Google:</span><span class="sxs-lookup"><span data-stu-id="160a4-158">An option to sign in with Google appears:</span></span>

![Webová aplikace spuštěna v Microsoft Edge: uživatel není ověřen.](index/_static/DoneGoogle.png)

<span data-ttu-id="160a4-160">Když kliknete na Google, budete přesměrováni na Google pro ověřování:</span><span class="sxs-lookup"><span data-stu-id="160a4-160">When you click on Google, you are redirected to Google for authentication:</span></span>

![Dialogové okno ověřování Google](index/_static/GoogleLogin.png)

<span data-ttu-id="160a4-162">Po zadání přihlašovacích údajů Google, pak budete přesměrováni zpět na webovou stránku, kde můžete nastavit e-mailu.</span><span class="sxs-lookup"><span data-stu-id="160a4-162">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="160a4-163">Nyní jste se přihlásili pomocí svých přihlašovacích údajů Google:</span><span class="sxs-lookup"><span data-stu-id="160a4-163">You are now logged in using your Google credentials:</span></span>

![Webová aplikace spuštěna v Microsoft Edge: uživatel ověřený](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="160a4-165">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="160a4-165">Troubleshooting</span></span>

* <span data-ttu-id="160a4-166">Pokud se zobrazí `403 (Forbidden)` chybová stránka ze své vlastní aplikace při spuštění v režimu pro vývoj (nebo přerušení ladicího s ke stejné chybě), ujistěte se, že **rozhraní API Google +** povolen v **knihovna rozhraní API Správce** podle následujících kroků uvedených v tomto [starší na této stránce](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="160a4-166">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="160a4-167">Pokud nefunguje přihlášení a se nezobrazují žádné chyby, přepněte do režimu vývoj usnadnění ladění problém.</span><span class="sxs-lookup"><span data-stu-id="160a4-167">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="160a4-168">**ASP.NET Core 2.x pouze:** pokud identita není nastavena voláním `services.AddIdentity` v `ConfigureServices`, pokusu o ověření bude mít za následek *ArgumentException –: musí být použita volba 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="160a4-168">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="160a4-169">Šablona projektu použili v tomto kurzu zajistí, že to probíhá.</span><span class="sxs-lookup"><span data-stu-id="160a4-169">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="160a4-170">Pokud databázi lokality s použitím počáteční migrace vytvořena nebyla, zobrazí se *databázová operace se nezdařila při zpracování požadavku* chyby.</span><span class="sxs-lookup"><span data-stu-id="160a4-170">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="160a4-171">Klepněte na **použít migrace** vytvořit databázi a aktualizujte pokračujte dále chyba.</span><span class="sxs-lookup"><span data-stu-id="160a4-171">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="160a4-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="160a4-172">Next steps</span></span>

* <span data-ttu-id="160a4-173">Tento článek vám ukázal, jak můžete ověřit pomocí služby Google.</span><span class="sxs-lookup"><span data-stu-id="160a4-173">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="160a4-174">Můžete postupovat podle podobný postup k ověření pomocí jiných poskytovatelů uvedené na [předchozí stránce](index.md).</span><span class="sxs-lookup"><span data-stu-id="160a4-174">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="160a4-175">Jakmile budete publikovat web vaší webové aplikace Azure, byste měli obnovit `ClientSecret` v konzole rozhraní API Google.</span><span class="sxs-lookup"><span data-stu-id="160a4-175">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="160a4-176">Nastavte `Authentication:Google:ClientId` a `Authentication:Google:ClientSecret` jako nastavení aplikace v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="160a4-176">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="160a4-177">Konfigurace systému je nastavený čtení klíčů z proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="160a4-177">The configuration system is set up to read keys from environment variables.</span></span>
