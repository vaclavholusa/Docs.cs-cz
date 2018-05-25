---
title: Bezpečné úložiště tajné klíče aplikace v vývoj v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak k ukládání a načítání citlivé informace jako tajné klíče aplikace během vývoje aplikace ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: ece2bf541df2b4acac60a88767cc57ede473bd49
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/24/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="f9869-103">Bezpečné úložiště tajné klíče aplikace v vývoj v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f9869-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="f9869-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [ADAM Roth](https://github.com/danroth27), a [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="f9869-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="f9869-105">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f9869-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f9869-106">Tento dokument popisuje techniky pro ukládání a načítání citlivá data během vývoje aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f9869-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="f9869-107">Ve zdrojovém kódu by nikdy neukládají hesla nebo dalších citlivých dat, a nesmí použít produkční tajných klíčů v vývoj nebo testování režimu.</span><span class="sxs-lookup"><span data-stu-id="f9869-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="f9869-108">Můžete ukládat a chránit Azure testovací a produkční tajných klíčů s [poskytovatele konfigurace Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="f9869-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="f9869-109">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="f9869-109">Environment variables</span></span>

<span data-ttu-id="f9869-110">Proměnné prostředí se používají k ukládání tajné klíče aplikace v kódu nebo v místní konfigurační soubory vyhnete.</span><span class="sxs-lookup"><span data-stu-id="f9869-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="f9869-111">Proměnné prostředí přepsat hodnoty konfigurace pro všechny zdroje dříve zadané konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f9869-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f9869-112">Čtení hodnot proměnných prostředí nakonfigurovat voláním [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) v `Startup` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="f9869-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="f9869-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="f9869-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="f9869-114">Vezměte v úvahu webové aplikace ASP.NET Core, ve kterém **jednotlivé uživatelské účty** je zabezpečená.</span><span class="sxs-lookup"><span data-stu-id="f9869-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="f9869-115">Připojovací řetězec databáze výchozí je zahrnut do projektu *appSettings.JSON určený* soubor s klíčem `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="f9869-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="f9869-116">Výchozí připojovací řetězec je pro LocalDB, který běží v uživatelském režimu a nevyžaduje heslo.</span><span class="sxs-lookup"><span data-stu-id="f9869-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="f9869-117">Během nasazení aplikace `DefaultConnection` klíče hodnotu je možné přepsat hodnotou proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="f9869-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="f9869-118">Proměnné prostředí může ukládat úplný připojovací řetězec s citlivé údaje.</span><span class="sxs-lookup"><span data-stu-id="f9869-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="f9869-119">Proměnné prostředí jsou obecně uložené v nešifrované prostého textu.</span><span class="sxs-lookup"><span data-stu-id="f9869-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="f9869-120">Když počítač nebo proces ohrožení, proměnné prostředí jsou přístupné nedůvěryhodné strany.</span><span class="sxs-lookup"><span data-stu-id="f9869-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="f9869-121">Další opatření, aby se zabránilo úniku tajné klíče uživatele může být požadováno.</span><span class="sxs-lookup"><span data-stu-id="f9869-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="f9869-122">Tajný klíč správce</span><span class="sxs-lookup"><span data-stu-id="f9869-122">Secret Manager</span></span>

<span data-ttu-id="f9869-123">Nástroj tajný klíč správce ukládá citlivá data během vývoje projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f9869-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="f9869-124">V tomto kontextu je úsek citlivých dat tajný klíč aplikace.</span><span class="sxs-lookup"><span data-stu-id="f9869-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="f9869-125">Tajné klíče aplikace jsou uložené v samostatné umístění ze stromu projektu.</span><span class="sxs-lookup"><span data-stu-id="f9869-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="f9869-126">Tajné klíče aplikace jsou spojené s konkrétní projekt nebo sdílená mezi několika projekty.</span><span class="sxs-lookup"><span data-stu-id="f9869-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="f9869-127">Tajné klíče aplikace nejsou změnami do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="f9869-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="f9869-128">Nástroj tajný klíč správce nemá zašifrování těchto tajných klíčů uložených a nesmí být považované za důvěryhodné úložiště.</span><span class="sxs-lookup"><span data-stu-id="f9869-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="f9869-129">Je jenom pro účely vývoje.</span><span class="sxs-lookup"><span data-stu-id="f9869-129">It's for development purposes only.</span></span> <span data-ttu-id="f9869-130">Klíče a hodnoty jsou uložené v konfiguračním souboru JSON v adresáři profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="f9869-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="f9869-131">Jak funguje nástroj Správce tajný klíč</span><span class="sxs-lookup"><span data-stu-id="f9869-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="f9869-132">Nástroj tajný klíč správce abstrahuje rychle podrobnosti implementace, jako je například kde a jak jsou uložené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f9869-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="f9869-133">Nástroj můžete použít bez znalosti tyto podrobnosti implementace.</span><span class="sxs-lookup"><span data-stu-id="f9869-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="f9869-134">Hodnoty jsou uložené v konfiguračním souboru JSON ve složce profil systému chráněný uživatel v místním počítači:</span><span class="sxs-lookup"><span data-stu-id="f9869-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f9869-135">Windows</span><span class="sxs-lookup"><span data-stu-id="f9869-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="f9869-136">Cesta v souborovém systému:</span><span class="sxs-lookup"><span data-stu-id="f9869-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="f9869-137">macOS</span><span class="sxs-lookup"><span data-stu-id="f9869-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="f9869-138">Cesta v souborovém systému:</span><span class="sxs-lookup"><span data-stu-id="f9869-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="f9869-139">Linux</span><span class="sxs-lookup"><span data-stu-id="f9869-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="f9869-140">Cesta v souborovém systému:</span><span class="sxs-lookup"><span data-stu-id="f9869-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="f9869-141">V předchozím cesty k souboru, nahraďte `<user_secrets_id>` s `UserSecretsId` hodnoty zadané ve *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="f9869-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="f9869-142">Nemusíte psát kód, který závisí na umístění nebo formát data uložená pomocí nástroje Správce tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="f9869-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="f9869-143">Tyto podrobnosti implementace může změnit.</span><span class="sxs-lookup"><span data-stu-id="f9869-143">These implementation details may change.</span></span> <span data-ttu-id="f9869-144">Například tajný hodnoty nejsou zašifrované, ale může být v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="f9869-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="f9869-145">Nainstalujte nástroj Správce tajný klíč</span><span class="sxs-lookup"><span data-stu-id="f9869-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="f9869-146">Nástroj Správce tajný klíč je instalován s rozhraní příkazového řádku .NET Core od verze rozhraní .NET Core SDK 2.1.300.</span><span class="sxs-lookup"><span data-stu-id="f9869-146">The Secret Manager tool is bundled with the .NET Core CLI as of .NET Core SDK 2.1.300.</span></span> <span data-ttu-id="f9869-147">Verze rozhraní .NET Core SDK před 2.1.300 je nutné nástroj instalace.</span><span class="sxs-lookup"><span data-stu-id="f9869-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="f9869-148">Spustit `dotnet --version` z příkazového prostředí zobrazíte číslo verze nainstalované rozhraní .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="f9869-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="f9869-149">Pokud se používá .NET Core SDK obsahuje nástroje, zobrazí se upozornění:</span><span class="sxs-lookup"><span data-stu-id="f9869-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="f9869-150">Nainstalujte [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) balíček NuGet do projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f9869-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="f9869-151">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f9869-151">For example:</span></span>

<span data-ttu-id="f9869-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="f9869-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="f9869-153">Spusťte následující příkaz v příkazovém řádku ověření instalace nástroje:</span><span class="sxs-lookup"><span data-stu-id="f9869-153">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="f9869-154">Nástroj tajný klíč správce zobrazí využití vzorků, možnosti a nápovědu k příkazu:</span><span class="sxs-lookup"><span data-stu-id="f9869-154">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="f9869-155">Musí být ve stejném adresáři jako *.csproj* spuštění nástroje definované v souboru *.csproj* souboru `DotNetCliToolReference` elementy.</span><span class="sxs-lookup"><span data-stu-id="f9869-155">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="f9869-156">Nastavit tajný klíč</span><span class="sxs-lookup"><span data-stu-id="f9869-156">Set a secret</span></span>

<span data-ttu-id="f9869-157">Nástroj tajný klíč Manager funguje v nastavení projektu specifické konfigurace uložené v profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="f9869-157">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="f9869-158">Pokud chcete použít tajné klíče uživatele, zadejte `UserSecretsId` v rámci `PropertyGroup` z *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="f9869-158">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="f9869-159">Hodnota `UserSecretsId` je volitelný, ale je jedinečné pro projekt.</span><span class="sxs-lookup"><span data-stu-id="f9869-159">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="f9869-160">Vývojáři obvykle generovat identifikátor GUID pro `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="f9869-160">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f9869-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="f9869-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="f9869-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="f9869-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="f9869-163">V sadě Visual Studio, klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **spravovat tajné klíče uživatele** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="f9869-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="f9869-164">Přidá tento gesto `UserSecretsId` elementu, naplní na identifikátor GUID *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="f9869-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="f9869-165">Otevře se Visual Studio *secrets.json* soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="f9869-165">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="f9869-166">Nahraďte obsah *secrets.json* s páry klíč hodnota k uložení.</span><span class="sxs-lookup"><span data-stu-id="f9869-166">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="f9869-167">Například: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="f9869-167">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="f9869-168">Definujte tajný klíč aplikace, který se skládá z klíče a jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f9869-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="f9869-169">Tajný klíč je přidružen k projektu `UserSecretsId` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f9869-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="f9869-170">Například spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:</span><span class="sxs-lookup"><span data-stu-id="f9869-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="f9869-171">V předchozím příkladu, který označuje dvojtečkou `Movies` je literál s objektem `ServiceApiKey` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f9869-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="f9869-172">Nástroj Správce tajný klíč lze z dalších adresářů příliš.</span><span class="sxs-lookup"><span data-stu-id="f9869-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="f9869-173">Použití `--project` možnost zadat cesta v souborovém systému, kdy *.csproj* soubor existuje.</span><span class="sxs-lookup"><span data-stu-id="f9869-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="f9869-174">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f9869-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="f9869-175">Nastavit více tajné klíče</span><span class="sxs-lookup"><span data-stu-id="f9869-175">Set multiple secrets</span></span>

<span data-ttu-id="f9869-176">Tím JSON pro lze nastavit dávky tajné klíče `set` příkaz.</span><span class="sxs-lookup"><span data-stu-id="f9869-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="f9869-177">V následujícím příkladu *input.json* jeho obsah se přesměruje do `set` příkaz.</span><span class="sxs-lookup"><span data-stu-id="f9869-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f9869-178">Windows</span><span class="sxs-lookup"><span data-stu-id="f9869-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="f9869-179">Otevřete příkazové okno a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f9869-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="f9869-180">macOS</span><span class="sxs-lookup"><span data-stu-id="f9869-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="f9869-181">Otevřete příkazové okno a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f9869-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="f9869-182">Linux</span><span class="sxs-lookup"><span data-stu-id="f9869-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="f9869-183">Otevřete příkazové okno a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f9869-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="f9869-184">Přístup a tajný klíč</span><span class="sxs-lookup"><span data-stu-id="f9869-184">Access a secret</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f9869-185">[Rozhraní API ASP.NET základní konfigurace](xref:fundamentals/configuration/index) poskytuje přístup k tajné klíče tajný klíč správce.</span><span class="sxs-lookup"><span data-stu-id="f9869-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="f9869-186">Nainstalujte [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="f9869-186">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="f9869-187">Přidat uživatelský zdroj konfigurace tajných klíčů s volání [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) v `Startup` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="f9869-187">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="f9869-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="f9869-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="f9869-189">[Rozhraní API ASP.NET základní konfigurace](xref:fundamentals/configuration/index) poskytuje přístup k tajné klíče tajný klíč správce.</span><span class="sxs-lookup"><span data-stu-id="f9869-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="f9869-190">Pokud je cílem vašeho projektu je .NET Framework, nainstalujte [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="f9869-190">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="f9869-191">V technologii ASP.NET Core 2.0 nebo novější, zdroj konfigurace tajné klíče uživatele se automaticky přidá v režimu pro vývoj při volání projektu [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) k inicializaci nové instance hostitele s předem nakonfigurované výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f9869-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="f9869-192">`CreateDefaultBuilder` volání [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) při [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) je [vývoj](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="f9869-192">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

<span data-ttu-id="f9869-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="f9869-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span></span>

<span data-ttu-id="f9869-194">Když `CreateDefaultBuilder` není volá se během vytváření hostitele, přidejte zdroj konfigurace tajné klíče uživatele pomocí volání [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) v `Startup` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="f9869-194">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="f9869-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="f9869-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="f9869-196">Tajné klíče uživatele lze načíst prostřednictvím `Configuration` rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="f9869-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f9869-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="f9869-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="f9869-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="f9869-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="f9869-199">Náhradní řetězec obsahující tajné údaje</span><span class="sxs-lookup"><span data-stu-id="f9869-199">String replacement with secrets</span></span>

<span data-ttu-id="f9869-200">Ukládání hesel ve formátu prostého textu je rizikové.</span><span class="sxs-lookup"><span data-stu-id="f9869-200">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="f9869-201">Například připojovací řetězec databáze uložené v *appSettings.JSON určený* může zahrnovat heslo pro zadaného uživatele:</span><span class="sxs-lookup"><span data-stu-id="f9869-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="f9869-202">Bezpečnější přístup je uložit heslo jako tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="f9869-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="f9869-203">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f9869-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="f9869-204">Nahraďte heslo v *appSettings.JSON určený* s zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="f9869-204">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="f9869-205">V následujícím příkladu `{0}` slouží jako zástupný symbol pro formulář [složené řetězec formátu](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span><span class="sxs-lookup"><span data-stu-id="f9869-205">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="f9869-206">Hodnota tajného klíče může vložit do zástupný symbol pro dokončení připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="f9869-206">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f9869-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="f9869-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="f9869-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="f9869-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="f9869-209">Seznam těchto tajných klíčů</span><span class="sxs-lookup"><span data-stu-id="f9869-209">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f9869-210">Spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:</span><span class="sxs-lookup"><span data-stu-id="f9869-210">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="f9869-211">Zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="f9869-211">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="f9869-212">V předchozím příkladu, dvojtečka v názvy klíčů označuje hierarchie objektů v rámci *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="f9869-212">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="f9869-213">Odeberte jeden tajný klíč</span><span class="sxs-lookup"><span data-stu-id="f9869-213">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f9869-214">Spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:</span><span class="sxs-lookup"><span data-stu-id="f9869-214">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="f9869-215">Aplikace *secrets.json* úpravy souboru odebrat přidružené dvojice klíč hodnota `MoviesConnectionString` klíč:</span><span class="sxs-lookup"><span data-stu-id="f9869-215">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="f9869-216">Spuštění `dotnet user-secrets list` zobrazí následující zprávu:</span><span class="sxs-lookup"><span data-stu-id="f9869-216">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="f9869-217">Odebrání všech tajných klíčů</span><span class="sxs-lookup"><span data-stu-id="f9869-217">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f9869-218">Spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:</span><span class="sxs-lookup"><span data-stu-id="f9869-218">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="f9869-219">Všechny tajné klíče uživatele pro aplikaci byl odstraněn z *secrets.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="f9869-219">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="f9869-220">Spuštění `dotnet user-secrets list` zobrazí následující zprávu:</span><span class="sxs-lookup"><span data-stu-id="f9869-220">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="f9869-221">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f9869-221">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
