---
title: "Bezpečné úložiště tajné klíče aplikace během vývoje v ASP.NET Core"
author: rick-anderson
description: "Ukazuje, jak bezpečně uložit tajné klíče během vývoje"
ms.author: riande
manager: wpickett
ms.date: 09/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/app-secrets
ms.openlocfilehash: 94356cef7a0333f0faac6420b1b5425920b99deb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a><span data-ttu-id="9bc76-103">Bezpečné úložiště tajné klíče aplikace během vývoje v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9bc76-103">Safe storage of app secrets during development in ASP.NET Core</span></span>

<span data-ttu-id="9bc76-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [ADAM Roth](https://github.com/danroth27), a [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="9bc76-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="9bc76-105">Tento dokument ukazuje, jak pomocí nástroje Správce tajný klíč v vývoj zachovat tajné klíče z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="9bc76-105">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="9bc76-106">Nejdůležitější bod je ve zdrojovém kódu by nikdy neukládají hesla nebo dalších citlivých dat, a tajné klíče produkční byste neměli používat v režimu pro vývoj a testování.</span><span class="sxs-lookup"><span data-stu-id="9bc76-106">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="9bc76-107">Místo toho můžete [konfigurace](xref:fundamentals/configuration/index) systému a přečtěte si tyto hodnoty z proměnné prostředí nebo z hodnot uložených pomocí tajný klíč správce nástroje.</span><span class="sxs-lookup"><span data-stu-id="9bc76-107">You can instead use the [configuration](xref:fundamentals/configuration/index) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="9bc76-108">Nástroj tajný klíč správce pomáhá zabránit citlivá data z kontroly do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="9bc76-108">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="9bc76-109">[Konfigurace](xref:fundamentals/configuration/index) systému může číst tajné klíče uložené pomocí nástroje Správce tajný klíč popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="9bc76-109">The [configuration](xref:fundamentals/configuration/index) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="9bc76-110">Nástroj Správce tajný klíč se používá pouze v vývoj.</span><span class="sxs-lookup"><span data-stu-id="9bc76-110">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="9bc76-111">Můžete zabezpečit Azure testovací a produkční tajných klíčů s [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) poskytovatele konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9bc76-111">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="9bc76-112">V tématu [poskytovatele konfigurace Azure Key Vault](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) Další informace.</span><span class="sxs-lookup"><span data-stu-id="9bc76-112">See [Azure Key Vault configuration provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="9bc76-113">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="9bc76-113">Environment variables</span></span>

<span data-ttu-id="9bc76-114">Abyste se vyhnuli ukládání tajné klíče aplikace v kódu nebo v místní konfigurační soubory, ukládat tajné klíče v seznamu proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="9bc76-114">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="9bc76-115">Můžete nastavit [konfigurace](xref:fundamentals/configuration/index) framework čtení hodnoty z proměnných prostředí voláním `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="9bc76-115">You can setup the [configuration](xref:fundamentals/configuration/index) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="9bc76-116">Proměnné prostředí poté můžete přepsat hodnoty konfigurace pro všechny zdroje dříve zadané konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9bc76-116">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="9bc76-117">Například pokud vytvoříte novou webovou aplikaci ASP.NET Core s jednotlivých uživatelských účtů, přidá výchozí připojovacího řetězce, který *appSettings.JSON určený* v projektu s klíčem `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="9bc76-117">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="9bc76-118">Výchozí připojovací řetězec je LocalDB, který běží v uživatelském režimu a nevyžaduje heslo pomocí instalačního programu.</span><span class="sxs-lookup"><span data-stu-id="9bc76-118">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="9bc76-119">Když nasadíte aplikaci k testu nebo produkčním serveru, můžete přepsat `DefaultConnection` hodnotu klíče s nastavení proměnné prostředí, který obsahuje připojovací řetězec (s citlivé přihlašovací údaje) pro testovací nebo provozní databázi Server.</span><span class="sxs-lookup"><span data-stu-id="9bc76-119">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="9bc76-120">Proměnné prostředí jsou obecně uložené ve formátu prostého textu a nejsou šifrována.</span><span class="sxs-lookup"><span data-stu-id="9bc76-120">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="9bc76-121">Když počítač nebo proces ohrožení, pak proměnné prostředí jsou přístupné nedůvěryhodné strany.</span><span class="sxs-lookup"><span data-stu-id="9bc76-121">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="9bc76-122">Další opatření, aby se zabránilo úniku tajné klíče uživatele může být stále nutný.</span><span class="sxs-lookup"><span data-stu-id="9bc76-122">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="9bc76-123">Tajný klíč správce</span><span class="sxs-lookup"><span data-stu-id="9bc76-123">Secret Manager</span></span>

<span data-ttu-id="9bc76-124">Nástroj tajný klíč správce ukládá citlivá data pro vývojové práci mimo váš projekt stromu.</span><span class="sxs-lookup"><span data-stu-id="9bc76-124">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="9bc76-125">Nástroj Správce tajný klíč je nástroj projektu, který slouží k uložení tajné klíče pro [.NET Core](https://www.microsoft.com/net/core) projektu během vývoje.</span><span class="sxs-lookup"><span data-stu-id="9bc76-125">The Secret Manager tool is a project tool that can be used to store secrets for a [.NET Core](https://www.microsoft.com/net/core) project during development.</span></span> <span data-ttu-id="9bc76-126">Pomocí nástroje Správce tajný klíč můžete přidružit konkrétní projekt tajné klíče aplikace a sdílet je ve více projektech.</span><span class="sxs-lookup"><span data-stu-id="9bc76-126">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="9bc76-127">Nástroj tajný klíč správce nemá zašifrování těchto tajných klíčů uložených a nesmí být považované za důvěryhodné úložiště.</span><span class="sxs-lookup"><span data-stu-id="9bc76-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="9bc76-128">Je jenom pro účely vývoje.</span><span class="sxs-lookup"><span data-stu-id="9bc76-128">It's for development purposes only.</span></span> <span data-ttu-id="9bc76-129">Klíče a hodnoty jsou uložené v konfiguračním souboru JSON v adresáři profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="9bc76-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="installing-the-secret-manager-tool"></a><span data-ttu-id="9bc76-130">Instalace nástroje Správce tajný klíč</span><span class="sxs-lookup"><span data-stu-id="9bc76-130">Installing the Secret Manager tool</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9bc76-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9bc76-131">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9bc76-132">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **upravit \<název_projektu\>.csproj** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="9bc76-132">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="9bc76-133">Přidejte zvýrazněný řádek na *.csproj* souboru a uložte obnovit přidruženého balíčku NuGet:</span><span class="sxs-lookup"><span data-stu-id="9bc76-133">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="9bc76-134">Znovu klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **spravovat tajné klíče uživatele** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="9bc76-134">Right-click the project in Solution Explorer again, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="9bc76-135">Přidá nový tento gesto `UserSecretsId` uzel v rámci `PropertyGroup` z *.csproj* souboru, jak je znázorněno v následující ukázce:</span><span class="sxs-lookup"><span data-stu-id="9bc76-135">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file, as highlighted in the following sample:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="9bc76-136">Ukládání upravenou *.csproj* také soubor otevře `secrets.json` soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="9bc76-136">Saving the modified *.csproj* file also opens a `secrets.json` file in the text editor.</span></span> <span data-ttu-id="9bc76-137">Nahraďte obsah `secrets.json` soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="9bc76-137">Replace the contents of the `secrets.json` file with the following code:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9bc76-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9bc76-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9bc76-139">Přidat `Microsoft.Extensions.SecretManager.Tools` k *.csproj* souboru a spusťte `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="9bc76-139">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run `dotnet restore`.</span></span> <span data-ttu-id="9bc76-140">Stejný postup můžete použít k instalaci nástroje Správce tajný klíč pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9bc76-140">You can use the same steps to install the Secret Manager Tool using for the command line.</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="9bc76-141">Testovací nástroje tajný klíč Manager tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9bc76-141">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="9bc76-142">Nástroj tajný klíč správce se zobrazí, využití, možnosti a nápovědu k příkazu.</span><span class="sxs-lookup"><span data-stu-id="9bc76-142">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="9bc76-143">Musí být ve stejném adresáři jako *.csproj* spuštění nástroje definované v souboru *.csproj* souboru `DotNetCliToolReference` uzlů.</span><span class="sxs-lookup"><span data-stu-id="9bc76-143">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="9bc76-144">Nástroj tajný klíč Manager funguje v nastavení konfigurace specifických projektu, které jsou uložené v profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="9bc76-144">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="9bc76-145">Chcete-li použít tajné klíče uživatele, musíte zadat projektu `UserSecretsId` hodnotu v jeho *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="9bc76-145">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="9bc76-146">Hodnota `UserSecretsId` je volitelný, ale je obecně jedinečné do projektu.</span><span class="sxs-lookup"><span data-stu-id="9bc76-146">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="9bc76-147">Vývojáři obvykle generovat identifikátor GUID pro `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="9bc76-147">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="9bc76-148">Přidat `UserSecretsId` pro svůj projekt v *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="9bc76-148">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="9bc76-149">Chcete-li nastavit tajného klíče pomocí nástroje Správce tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="9bc76-149">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="9bc76-150">Například v příkazovém okně z adresáře projektu, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9bc76-150">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="9bc76-151">Můžete spustit nástroj Správce tajný klíč z dalších adresářů, ale je nutné použít `--project` možnost předat v cestě k *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="9bc76-151">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="9bc76-152">Můžete také nástroj Správce tajný klíč do seznamu, odebrat a vymazat tajné klíče aplikace.</span><span class="sxs-lookup"><span data-stu-id="9bc76-152">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

-----

## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="9bc76-153">Přístup k tajné klíče uživatele prostřednictvím konfigurace</span><span class="sxs-lookup"><span data-stu-id="9bc76-153">Accessing user secrets via configuration</span></span>

<span data-ttu-id="9bc76-154">Tajné klíče tajný klíč správce přistupujete prostřednictvím systému konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9bc76-154">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="9bc76-155">Přidat `Microsoft.Extensions.Configuration.UserSecrets` balíček a spusťte `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="9bc76-155">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run `dotnet restore`.</span></span>

<span data-ttu-id="9bc76-156">Přidat zdroj konfigurace tajné klíče uživatele na `Startup` metoda:</span><span class="sxs-lookup"><span data-stu-id="9bc76-156">Add the user secrets configuration source to the `Startup` method:</span></span>

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

<span data-ttu-id="9bc76-157">Tajné klíče uživatele prostřednictvím rozhraní API konfigurace se můžete dostat:</span><span class="sxs-lookup"><span data-stu-id="9bc76-157">You can access user secrets via the configuration API:</span></span>

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="9bc76-158">Jak funguje nástroj Správce tajný klíč</span><span class="sxs-lookup"><span data-stu-id="9bc76-158">How the Secret Manager tool works</span></span>

<span data-ttu-id="9bc76-159">Nástroj tajný klíč správce abstrahuje rychle podrobnosti implementace, jako je například kde a jak jsou uložené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9bc76-159">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="9bc76-160">Nástroj můžete použít bez znalosti tyto podrobnosti implementace.</span><span class="sxs-lookup"><span data-stu-id="9bc76-160">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="9bc76-161">V aktuální verzi, hodnoty jsou uloženy v [JSON](http://json.org/) konfigurační soubor v adresáři profilu uživatele:</span><span class="sxs-lookup"><span data-stu-id="9bc76-161">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="9bc76-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="9bc76-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="9bc76-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="9bc76-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="9bc76-164">Mac: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="9bc76-164">Mac: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="9bc76-165">Hodnota `userSecretsId` pochází z hodnoty zadané ve *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="9bc76-165">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="9bc76-166">Neměli psát kód, který závisí na umístění nebo formát dat pomocí nástroje Správce tajný klíč uložit jako, může se měnit tyto podrobnosti implementace.</span><span class="sxs-lookup"><span data-stu-id="9bc76-166">You shouldn't write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="9bc76-167">Například tajný hodnoty jsou aktuálně *není* šifruje dnes, ale může být jednou budete.</span><span class="sxs-lookup"><span data-stu-id="9bc76-167">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9bc76-168">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="9bc76-168">Additional Resources</span></span>

* [<span data-ttu-id="9bc76-169">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="9bc76-169">Configuration</span></span>](xref:fundamentals/configuration/index)
