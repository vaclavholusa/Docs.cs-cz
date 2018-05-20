---
title: Bezpečné úložiště tajné klíče aplikace v vývoj v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak k ukládání a načítání citlivé informace jako tajné klíče aplikace během vývoje aplikace ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 88b4ee9a963543f8cc97cb66271628a14fe657de
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/18/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="f4736-103">Bezpečné úložiště tajné klíče aplikace v vývoj v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f4736-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="f4736-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [ADAM Roth](https://github.com/danroth27), a [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="f4736-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f4736-105">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f4736-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="f4736-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f4736-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="f4736-107">Tento dokument popisuje techniky pro ukládání a načítání citlivá data během vývoje aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4736-107">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="f4736-108">Ve zdrojovém kódu by nikdy neukládají hesla nebo dalších citlivých dat, a nesmí použít produkční tajných klíčů v vývoj nebo testování režimu.</span><span class="sxs-lookup"><span data-stu-id="f4736-108">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="f4736-109">Můžete ukládat a chránit Azure testovací a produkční tajných klíčů s [poskytovatele konfigurace Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="f4736-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="f4736-110">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="f4736-110">Environment variables</span></span>

<span data-ttu-id="f4736-111">Proměnné prostředí se používají k ukládání tajné klíče aplikace v kódu nebo v místní konfigurační soubory vyhnete.</span><span class="sxs-lookup"><span data-stu-id="f4736-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="f4736-112">Proměnné prostředí přepsat hodnoty konfigurace pro všechny zdroje dříve zadané konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f4736-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f4736-113">Čtení hodnot proměnných prostředí nakonfigurovat voláním [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) v `Startup` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="f4736-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="f4736-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="f4736-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="f4736-115">Vezměte v úvahu webové aplikace ASP.NET Core, ve kterém **jednotlivé uživatelské účty** je zabezpečená.</span><span class="sxs-lookup"><span data-stu-id="f4736-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="f4736-116">Připojovací řetězec databáze výchozí je zahrnut do projektu *appSettings.JSON určený* soubor s klíčem `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="f4736-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="f4736-117">Výchozí připojovací řetězec je pro LocalDB, který běží v uživatelském režimu a nevyžaduje heslo.</span><span class="sxs-lookup"><span data-stu-id="f4736-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="f4736-118">Během nasazení aplikace `DefaultConnection` klíče hodnotu je možné přepsat hodnotou proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="f4736-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="f4736-119">Proměnné prostředí může ukládat úplný připojovací řetězec s citlivé údaje.</span><span class="sxs-lookup"><span data-stu-id="f4736-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="f4736-120">Proměnné prostředí jsou obecně uložené v nešifrované prostého textu.</span><span class="sxs-lookup"><span data-stu-id="f4736-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="f4736-121">Když počítač nebo proces ohrožení, proměnné prostředí jsou přístupné nedůvěryhodné strany.</span><span class="sxs-lookup"><span data-stu-id="f4736-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="f4736-122">Další opatření, aby se zabránilo úniku tajné klíče uživatele může být požadováno.</span><span class="sxs-lookup"><span data-stu-id="f4736-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="f4736-123">Tajný klíč správce</span><span class="sxs-lookup"><span data-stu-id="f4736-123">Secret Manager</span></span>

<span data-ttu-id="f4736-124">Nástroj tajný klíč správce ukládá citlivá data během vývoje projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4736-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="f4736-125">V tomto kontextu je úsek citlivých dat tajný klíč aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4736-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="f4736-126">Tajné klíče aplikace jsou uložené v samostatné umístění ze stromu projektu.</span><span class="sxs-lookup"><span data-stu-id="f4736-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="f4736-127">Tajné klíče aplikace jsou spojené s konkrétní projekt nebo sdílená mezi několika projekty.</span><span class="sxs-lookup"><span data-stu-id="f4736-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="f4736-128">Tajné klíče aplikace nejsou změnami do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="f4736-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="f4736-129">Nástroj tajný klíč správce nemá zašifrování těchto tajných klíčů uložených a nesmí být považované za důvěryhodné úložiště.</span><span class="sxs-lookup"><span data-stu-id="f4736-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="f4736-130">Je jenom pro účely vývoje.</span><span class="sxs-lookup"><span data-stu-id="f4736-130">It's for development purposes only.</span></span> <span data-ttu-id="f4736-131">Klíče a hodnoty jsou uložené v konfiguračním souboru JSON v adresáři profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="f4736-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="f4736-132">Jak funguje nástroj Správce tajný klíč</span><span class="sxs-lookup"><span data-stu-id="f4736-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="f4736-133">Nástroj tajný klíč správce abstrahuje rychle podrobnosti implementace, jako je například kde a jak jsou uložené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f4736-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="f4736-134">Nástroj můžete použít bez znalosti tyto podrobnosti implementace.</span><span class="sxs-lookup"><span data-stu-id="f4736-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="f4736-135">Hodnoty jsou uloženy v [JSON](https://json.org/) konfigurační soubor ve složce profil systému chráněný uživatel v místním počítači:</span><span class="sxs-lookup"><span data-stu-id="f4736-135">The values are stored in a [JSON](https://json.org/) configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f4736-136">Windows</span><span class="sxs-lookup"><span data-stu-id="f4736-136">Windows</span></span>](#tab/windows)

<span data-ttu-id="f4736-137">Cesta v souborovém systému:</span><span class="sxs-lookup"><span data-stu-id="f4736-137">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="f4736-138">macOS</span><span class="sxs-lookup"><span data-stu-id="f4736-138">macOS</span></span>](#tab/macos)

<span data-ttu-id="f4736-139">Cesta v souborovém systému:</span><span class="sxs-lookup"><span data-stu-id="f4736-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="f4736-140">Linux</span><span class="sxs-lookup"><span data-stu-id="f4736-140">Linux</span></span>](#tab/linux)

<span data-ttu-id="f4736-141">Cesta v souborovém systému:</span><span class="sxs-lookup"><span data-stu-id="f4736-141">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="f4736-142">V předchozím cesty k souboru, nahraďte `<user_secrets_id>` s `UserSecretsId` hodnoty zadané ve *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="f4736-142">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="f4736-143">Nemusíte psát kód, který závisí na umístění nebo formát data uložená pomocí nástroje Správce tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="f4736-143">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="f4736-144">Tyto podrobnosti implementace může změnit.</span><span class="sxs-lookup"><span data-stu-id="f4736-144">These implementation details may change.</span></span> <span data-ttu-id="f4736-145">Například tajný hodnoty nejsou zašifrované, ale může být v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="f4736-145">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="f4736-146">Nainstalujte nástroj Správce tajný klíč</span><span class="sxs-lookup"><span data-stu-id="f4736-146">Install the Secret Manager tool</span></span>

<span data-ttu-id="f4736-147">Nástroj Správce tajný klíč je instalován s rozhraní příkazového řádku .NET Core v rozhraní .NET Core SDK 2.1.</span><span class="sxs-lookup"><span data-stu-id="f4736-147">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.</span></span> <span data-ttu-id="f4736-148">Pro rozhraní .NET Core SDK 2.0 a starší je nutné instalaci nástroje.</span><span class="sxs-lookup"><span data-stu-id="f4736-148">For .NET Core SDK 2.0 and earlier, tool installation is necessary.</span></span>

<span data-ttu-id="f4736-149">Nainstalujte [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) balíček NuGet do projektu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f4736-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project:</span></span>

<span data-ttu-id="f4736-150">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="f4736-150">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="f4736-151">Spusťte následující příkaz v příkazovém řádku ověření instalace nástroje:</span><span class="sxs-lookup"><span data-stu-id="f4736-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="f4736-152">Nástroj tajný klíč správce zobrazí využití vzorků, možnosti a nápovědu k příkazu:</span><span class="sxs-lookup"><span data-stu-id="f4736-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> <span data-ttu-id="f4736-153">Musí být ve stejném adresáři jako *.csproj* spuštění nástroje definované v souboru *.csproj* souboru `DotNetCliToolReference` elementy.</span><span class="sxs-lookup"><span data-stu-id="f4736-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="f4736-154">Nastavit tajný klíč</span><span class="sxs-lookup"><span data-stu-id="f4736-154">Set a secret</span></span>

<span data-ttu-id="f4736-155">Nástroj tajný klíč Manager funguje v nastavení projektu specifické konfigurace uložené v profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="f4736-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="f4736-156">Pokud chcete použít tajné klíče uživatele, zadejte `UserSecretsId` v rámci `PropertyGroup` z *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="f4736-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="f4736-157">Hodnota `UserSecretsId` je volitelný, ale je jedinečné pro projekt.</span><span class="sxs-lookup"><span data-stu-id="f4736-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="f4736-158">Vývojáři obvykle generovat identifikátor GUID pro `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="f4736-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f4736-159">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="f4736-159">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="f4736-160">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="f4736-160">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="f4736-161">V sadě Visual Studio, klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **spravovat tajné klíče uživatele** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="f4736-161">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="f4736-162">Přidá tento gesto `UserSecretsId` elementu, naplní na identifikátor GUID *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="f4736-162">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="f4736-163">Otevře se Visual Studio *secrets.json* soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="f4736-163">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="f4736-164">Nahraďte obsah *secrets.json* s páry klíč hodnota k uložení.</span><span class="sxs-lookup"><span data-stu-id="f4736-164">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="f4736-165">Například: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="f4736-165">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="f4736-166">Definujte tajný klíč aplikace, který se skládá z klíče a jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f4736-166">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="f4736-167">Tajný klíč je přidružen k projektu `UserSecretsId` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f4736-167">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="f4736-168">Například spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:</span><span class="sxs-lookup"><span data-stu-id="f4736-168">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="f4736-169">V předchozím příkladu, který označuje dvojtečkou `Movies` je literál s objektem `ServiceApiKey` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f4736-169">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="f4736-170">Nástroj Správce tajný klíč lze z dalších adresářů příliš.</span><span class="sxs-lookup"><span data-stu-id="f4736-170">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="f4736-171">Použití `--project` možnost zadat cesta v souborovém systému, kdy *.csproj* soubor existuje.</span><span class="sxs-lookup"><span data-stu-id="f4736-171">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="f4736-172">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f4736-172">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="f4736-173">Nastavit více tajné klíče</span><span class="sxs-lookup"><span data-stu-id="f4736-173">Set multiple secrets</span></span>

<span data-ttu-id="f4736-174">Tím JSON pro lze nastavit dávky tajné klíče `set` příkaz.</span><span class="sxs-lookup"><span data-stu-id="f4736-174">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="f4736-175">V následujícím příkladu *input.json* jeho obsah se přesměruje do `set` příkaz.</span><span class="sxs-lookup"><span data-stu-id="f4736-175">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f4736-176">Windows</span><span class="sxs-lookup"><span data-stu-id="f4736-176">Windows</span></span>](#tab/windows)

<span data-ttu-id="f4736-177">Otevřete příkazové okno a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f4736-177">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="f4736-178">macOS</span><span class="sxs-lookup"><span data-stu-id="f4736-178">macOS</span></span>](#tab/macos)

<span data-ttu-id="f4736-179">Otevřete příkazové okno a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f4736-179">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="f4736-180">Linux</span><span class="sxs-lookup"><span data-stu-id="f4736-180">Linux</span></span>](#tab/linux)

<span data-ttu-id="f4736-181">Otevřete příkazové okno a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f4736-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="f4736-182">Přístup a tajný klíč</span><span class="sxs-lookup"><span data-stu-id="f4736-182">Access a secret</span></span>

<span data-ttu-id="f4736-183">[Rozhraní API ASP.NET základní konfigurace](xref:fundamentals/configuration/index) poskytuje přístup k tajné klíče tajný klíč správce.</span><span class="sxs-lookup"><span data-stu-id="f4736-183">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="f4736-184">Pokud cílení na .NET Core 1.x nebo rozhraní .NET Framework, nainstalujte [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="f4736-184">If targeting .NET Core 1.x or .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f4736-185">Přidat uživatelský zdroj konfigurace tajné klíče na `Startup` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="f4736-185">Add the user secrets configuration source to the `Startup` constructor:</span></span>

<span data-ttu-id="f4736-186">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="f4736-186">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="f4736-187">Tajné klíče uživatele lze načíst prostřednictvím `Configuration` rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="f4736-187">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f4736-188">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="f4736-188">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="f4736-189">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="f4736-189">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="f4736-190">Náhradní řetězec obsahující tajné údaje</span><span class="sxs-lookup"><span data-stu-id="f4736-190">String replacement with secrets</span></span>

<span data-ttu-id="f4736-191">Ukládání hesel ve formátu prostého textu je rizikové.</span><span class="sxs-lookup"><span data-stu-id="f4736-191">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="f4736-192">Například připojovací řetězec databáze uložené v *appSettings.JSON určený* může zahrnovat heslo pro zadaného uživatele:</span><span class="sxs-lookup"><span data-stu-id="f4736-192">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="f4736-193">Bezpečnější přístup je uložit heslo jako tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="f4736-193">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="f4736-194">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f4736-194">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="f4736-195">Nahraďte heslo v *appSettings.JSON určený* s zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="f4736-195">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="f4736-196">V následujícím příkladu `{0}` slouží jako zástupný symbol pro formulář [složené řetězec formátu](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span><span class="sxs-lookup"><span data-stu-id="f4736-196">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="f4736-197">Hodnota tajného klíče může vložit do zástupný symbol pro dokončení připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="f4736-197">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f4736-198">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="f4736-198">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="f4736-199">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="f4736-199">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="f4736-200">Seznam těchto tajných klíčů</span><span class="sxs-lookup"><span data-stu-id="f4736-200">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f4736-201">Spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:</span><span class="sxs-lookup"><span data-stu-id="f4736-201">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="f4736-202">Zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="f4736-202">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="f4736-203">V předchozím příkladu, dvojtečka v názvy klíčů označuje hierarchie objektů v rámci *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="f4736-203">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="f4736-204">Odeberte jeden tajný klíč</span><span class="sxs-lookup"><span data-stu-id="f4736-204">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f4736-205">Spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:</span><span class="sxs-lookup"><span data-stu-id="f4736-205">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="f4736-206">Aplikace *secrets.json* úpravy souboru odebrat přidružené dvojice klíč hodnota `MoviesConnectionString` klíč:</span><span class="sxs-lookup"><span data-stu-id="f4736-206">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="f4736-207">Spuštění `dotnet user-secrets list` zobrazí následující zprávu:</span><span class="sxs-lookup"><span data-stu-id="f4736-207">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="f4736-208">Odebrání všech tajných klíčů</span><span class="sxs-lookup"><span data-stu-id="f4736-208">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f4736-209">Spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:</span><span class="sxs-lookup"><span data-stu-id="f4736-209">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="f4736-210">Všechny tajné klíče uživatele pro aplikaci byl odstraněn z *secrets.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="f4736-210">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="f4736-211">Spuštění `dotnet user-secrets list` zobrazí následující zprávu:</span><span class="sxs-lookup"><span data-stu-id="f4736-211">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="f4736-212">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f4736-212">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>