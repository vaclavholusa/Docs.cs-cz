---
title: "Bezpečné úložiště tajné klíče aplikace během vývoje v ASP.NET Core"
author: rick-anderson
description: "Ukazuje, jak bezpečně uložit tajné klíče během vývoje"
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 09/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/app-secrets
ms.openlocfilehash: 897d9b360ceeb5fbb0863ff1c1fcec039e1a8b8f
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a><span data-ttu-id="80527-104">Bezpečné úložiště tajné klíče aplikace během vývoje v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="80527-104">Safe storage of app secrets during development in ASP.NET Core</span></span>

<span data-ttu-id="80527-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [ADAM Roth](https://github.com/danroth27), a [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="80527-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="80527-106">Tento dokument ukazuje, jak pomocí nástroje Správce tajný klíč v vývoj zachovat tajné klíče z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="80527-106">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="80527-107">Nejdůležitější bod je ve zdrojovém kódu by nikdy neukládají hesla nebo dalších citlivých dat, a tajné klíče produkční byste neměli používat v režimu pro vývoj a testování.</span><span class="sxs-lookup"><span data-stu-id="80527-107">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="80527-108">Místo toho můžete [konfigurace](xref:fundamentals/configuration/index) systému a přečtěte si tyto hodnoty z proměnné prostředí nebo z hodnot uložených pomocí tajný klíč správce nástroje.</span><span class="sxs-lookup"><span data-stu-id="80527-108">You can instead use the [configuration](xref:fundamentals/configuration/index) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="80527-109">Nástroj tajný klíč správce pomáhá zabránit citlivá data z kontroly do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="80527-109">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="80527-110">[Konfigurace](xref:fundamentals/configuration/index) systému může číst tajné klíče uložené pomocí nástroje Správce tajný klíč popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="80527-110">The [configuration](xref:fundamentals/configuration/index) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="80527-111">Nástroj Správce tajný klíč se používá pouze v vývoj.</span><span class="sxs-lookup"><span data-stu-id="80527-111">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="80527-112">Můžete zabezpečit Azure testovací a produkční tajných klíčů s [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) poskytovatele konfigurace.</span><span class="sxs-lookup"><span data-stu-id="80527-112">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="80527-113">V tématu [poskytovatele konfigurace Azure Key Vault](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) Další informace.</span><span class="sxs-lookup"><span data-stu-id="80527-113">See [Azure Key Vault configuration provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="80527-114">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="80527-114">Environment variables</span></span>

<span data-ttu-id="80527-115">Abyste se vyhnuli ukládání tajné klíče aplikace v kódu nebo v místní konfigurační soubory, ukládat tajné klíče v seznamu proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="80527-115">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="80527-116">Můžete nastavit [konfigurace](xref:fundamentals/configuration/index) framework čtení hodnoty z proměnných prostředí voláním `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="80527-116">You can setup the [configuration](xref:fundamentals/configuration/index) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="80527-117">Proměnné prostředí poté můžete přepsat hodnoty konfigurace pro všechny zdroje dříve zadané konfigurace.</span><span class="sxs-lookup"><span data-stu-id="80527-117">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="80527-118">Například pokud vytvoříte novou webovou aplikaci ASP.NET Core s jednotlivých uživatelských účtů, přidá výchozí připojovacího řetězce, který *appSettings.JSON určený* v projektu s klíčem `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="80527-118">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="80527-119">Výchozí připojovací řetězec je LocalDB, který běží v uživatelském režimu a nevyžaduje heslo pomocí instalačního programu.</span><span class="sxs-lookup"><span data-stu-id="80527-119">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="80527-120">Když nasadíte aplikaci k testu nebo produkčním serveru, můžete přepsat `DefaultConnection` hodnotu klíče s nastavení proměnné prostředí, který obsahuje připojovací řetězec (s citlivé přihlašovací údaje) pro testovací nebo provozní databázi Server.</span><span class="sxs-lookup"><span data-stu-id="80527-120">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="80527-121">Proměnné prostředí jsou obecně uložené ve formátu prostého textu a nejsou šifrována.</span><span class="sxs-lookup"><span data-stu-id="80527-121">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="80527-122">Když počítač nebo proces ohrožení, pak proměnné prostředí jsou přístupné nedůvěryhodné strany.</span><span class="sxs-lookup"><span data-stu-id="80527-122">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="80527-123">Další opatření, aby se zabránilo úniku tajné klíče uživatele může být stále nutný.</span><span class="sxs-lookup"><span data-stu-id="80527-123">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="80527-124">Tajný klíč správce</span><span class="sxs-lookup"><span data-stu-id="80527-124">Secret Manager</span></span>

<span data-ttu-id="80527-125">Nástroj tajný klíč správce ukládá citlivá data pro vývojové práci mimo váš projekt stromu.</span><span class="sxs-lookup"><span data-stu-id="80527-125">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="80527-126">Nástroj Správce tajný klíč je nástroj projektu, který slouží k uložení tajné klíče pro [.NET Core](https://www.microsoft.com/net/core) projektu během vývoje.</span><span class="sxs-lookup"><span data-stu-id="80527-126">The Secret Manager tool is a project tool that can be used to store secrets for a [.NET Core](https://www.microsoft.com/net/core) project during development.</span></span> <span data-ttu-id="80527-127">Pomocí nástroje Správce tajný klíč můžete přidružit konkrétní projekt tajné klíče aplikace a sdílet je ve více projektech.</span><span class="sxs-lookup"><span data-stu-id="80527-127">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="80527-128">Nástroj tajný klíč správce nešifruje uložené tajných klíčů a nesmí být považované za důvěryhodné úložiště.</span><span class="sxs-lookup"><span data-stu-id="80527-128">The Secret Manager tool does not encrypt the stored secrets and should not be treated as a trusted store.</span></span> <span data-ttu-id="80527-129">Je jenom pro účely vývoje.</span><span class="sxs-lookup"><span data-stu-id="80527-129">It is for development purposes only.</span></span> <span data-ttu-id="80527-130">Klíče a hodnoty jsou uložené v konfiguračním souboru JSON v adresáři profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="80527-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="installing-the-secret-manager-tool"></a><span data-ttu-id="80527-131">Instalace nástroje Správce tajný klíč</span><span class="sxs-lookup"><span data-stu-id="80527-131">Installing the Secret Manager tool</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="80527-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80527-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="80527-133">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **upravit \<název_projektu\>.csproj** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="80527-133">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="80527-134">Přidejte zvýrazněný řádek na *.csproj* souboru a uložte obnovit přidruženého balíčku NuGet:</span><span class="sxs-lookup"><span data-stu-id="80527-134">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="80527-135">Znovu klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **spravovat tajné klíče uživatele** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="80527-135">Right-click the project in Solution Explorer again, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="80527-136">Přidá nový tento gesto `UserSecretsId` uzel v rámci `PropertyGroup` z *.csproj* souboru, jak je znázorněno v následující ukázce:</span><span class="sxs-lookup"><span data-stu-id="80527-136">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file, as highlighted in the following sample:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="80527-137">Ukládání upravenou *.csproj* také soubor otevře `secrets.json` soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="80527-137">Saving the modified *.csproj* file also opens a `secrets.json` file in the text editor.</span></span> <span data-ttu-id="80527-138">Nahraďte obsah `secrets.json` soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="80527-138">Replace the contents of the `secrets.json` file with the following code:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="80527-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="80527-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="80527-140">Přidat `Microsoft.Extensions.SecretManager.Tools` k *.csproj* souboru a spusťte `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="80527-140">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run `dotnet restore`.</span></span> <span data-ttu-id="80527-141">Stejný postup můžete použít k instalaci nástroje Správce tajný klíč pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="80527-141">You can use the same steps to install the Secret Manager Tool using for the command line.</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="80527-142">Testovací nástroje tajný klíč Manager tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="80527-142">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="80527-143">Nástroj tajný klíč správce se zobrazí, využití, možnosti a nápovědu k příkazu.</span><span class="sxs-lookup"><span data-stu-id="80527-143">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="80527-144">Musí být ve stejném adresáři jako *.csproj* spuštění nástroje definované v souboru *.csproj* souboru `DotNetCliToolReference` uzlů.</span><span class="sxs-lookup"><span data-stu-id="80527-144">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="80527-145">Nástroj tajný klíč Manager funguje v nastavení konfigurace specifických projektu, které jsou uložené v profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="80527-145">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="80527-146">Chcete-li použít tajné klíče uživatele, musíte zadat projektu `UserSecretsId` hodnotu v jeho *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="80527-146">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="80527-147">Hodnota `UserSecretsId` je volitelný, ale je obecně jedinečné do projektu.</span><span class="sxs-lookup"><span data-stu-id="80527-147">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="80527-148">Vývojáři obvykle generovat identifikátor GUID pro `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="80527-148">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="80527-149">Přidat `UserSecretsId` pro svůj projekt v *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="80527-149">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="80527-150">Chcete-li nastavit tajného klíče pomocí nástroje Správce tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="80527-150">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="80527-151">Například v příkazovém okně z adresáře projektu, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="80527-151">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="80527-152">Můžete spustit nástroj Správce tajný klíč z dalších adresářů, ale je nutné použít `--project` možnost předat v cestě k *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="80527-152">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="80527-153">Můžete také nástroj Správce tajný klíč do seznamu, odebrat a vymazat tajné klíče aplikace.</span><span class="sxs-lookup"><span data-stu-id="80527-153">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

-----

## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="80527-154">Přístup k tajné klíče uživatele prostřednictvím konfigurace</span><span class="sxs-lookup"><span data-stu-id="80527-154">Accessing user secrets via configuration</span></span>

<span data-ttu-id="80527-155">Tajné klíče tajný klíč správce přistupujete prostřednictvím systému konfigurace.</span><span class="sxs-lookup"><span data-stu-id="80527-155">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="80527-156">Přidat `Microsoft.Extensions.Configuration.UserSecrets` balíček a spusťte `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="80527-156">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run `dotnet restore`.</span></span>

<span data-ttu-id="80527-157">Přidat zdroj konfigurace tajné klíče uživatele na `Startup` metoda:</span><span class="sxs-lookup"><span data-stu-id="80527-157">Add the user secrets configuration source to the `Startup` method:</span></span>

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

<span data-ttu-id="80527-158">Tajné klíče uživatele prostřednictvím rozhraní API konfigurace se můžete dostat:</span><span class="sxs-lookup"><span data-stu-id="80527-158">You can access user secrets via the configuration API:</span></span>

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="80527-159">Jak funguje nástroj Správce tajný klíč</span><span class="sxs-lookup"><span data-stu-id="80527-159">How the Secret Manager tool works</span></span>

<span data-ttu-id="80527-160">Nástroj tajný klíč správce abstrahuje rychle podrobnosti implementace, jako je například kde a jak jsou uložené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="80527-160">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="80527-161">Nástroj můžete použít bez znalosti tyto podrobnosti implementace.</span><span class="sxs-lookup"><span data-stu-id="80527-161">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="80527-162">V aktuální verzi, hodnoty jsou uloženy v [JSON](http://json.org/) konfigurační soubor v adresáři profilu uživatele:</span><span class="sxs-lookup"><span data-stu-id="80527-162">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="80527-163">Windows:`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="80527-163">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="80527-164">Linux:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="80527-164">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="80527-165">Mac:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="80527-165">Mac: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="80527-166">Hodnota `userSecretsId` pochází z hodnoty zadané ve *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="80527-166">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="80527-167">Neměli psát kód, který závisí na umístění nebo formát dat pomocí nástroje Správce tajný klíč uložit jako, může se měnit tyto podrobnosti implementace.</span><span class="sxs-lookup"><span data-stu-id="80527-167">You should not write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="80527-168">Například tajný hodnoty jsou aktuálně *není* šifruje dnes, ale může být jednou budete.</span><span class="sxs-lookup"><span data-stu-id="80527-168">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="80527-169">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="80527-169">Additional Resources</span></span>

* [<span data-ttu-id="80527-170">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="80527-170">Configuration</span></span>](xref:fundamentals/configuration/index)
