---
title: Bezpečné ukládání tajných kódů aplikace při vývoji v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak ukládat a načítat citlivých informací jako tajných kódů aplikace během vývoje aplikace ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/16/2018
uid: security/app-secrets
ms.openlocfilehash: 35c316230c19aa69a0dac26ec25a6e017f102237
ms.sourcegitcommit: 1cf65c25ed16495e27f35ded98b3952a30c68f36
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/17/2018
ms.locfileid: "41753542"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="2758b-103">Bezpečné ukládání tajných kódů aplikace při vývoji v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2758b-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="2758b-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), a [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="2758b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="2758b-105">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2758b-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2758b-106">Tento dokument popisuje postupy pro ukládání a načítání citlivých dat během vývoje aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2758b-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="2758b-107">Ve zdrojovém kódu by nikdy ukládání hesel nebo jiných citlivých dat. a nesmí použít produkční tajných kódů při vývoji nebo otestování režimu.</span><span class="sxs-lookup"><span data-stu-id="2758b-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="2758b-108">Můžete ukládat a chránit Azure testovací a produkční tajných kódů pomocí [poskytovatele konfigurace služby Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="2758b-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="2758b-109">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="2758b-109">Environment variables</span></span>

<span data-ttu-id="2758b-110">Proměnné prostředí je použit k ukládání tajných kódů aplikace v kódu nebo v místních konfiguračních souborech.</span><span class="sxs-lookup"><span data-stu-id="2758b-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="2758b-111">Proměnné prostředí přepsat hodnoty konfigurace pro všechny zdroje dříve zadanou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2758b-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="2758b-112">Konfigurace čtení hodnot proměnných prostředí pomocí volání [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) v `Startup` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="2758b-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="2758b-113">Vezměte v úvahu webovou aplikaci ASP.NET Core, ve kterém **jednotlivé uživatelské účty** je zabezpečená.</span><span class="sxs-lookup"><span data-stu-id="2758b-113">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="2758b-114">Výchozí připojovací řetězec databáze je součástí projektu *appsettings.json* soubor s klíčem `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="2758b-114">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="2758b-115">Výchozí připojovací řetězec je pro LocalDB, který běží v uživatelském režimu a nevyžaduje, aby heslo.</span><span class="sxs-lookup"><span data-stu-id="2758b-115">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="2758b-116">Během nasazování aplikací `DefaultConnection` klíče hodnota se dá přepsat hodnotou proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="2758b-116">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="2758b-117">Proměnná prostředí může ukládat úplný připojovací řetězec s citlivé přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="2758b-117">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="2758b-118">Proměnné prostředí jsou obecně uložené v nezašifrované prostého textu.</span><span class="sxs-lookup"><span data-stu-id="2758b-118">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="2758b-119">Pokud počítači nebo procesu dojde k ohrožení, proměnné prostředí je přístupný nedůvěryhodní.</span><span class="sxs-lookup"><span data-stu-id="2758b-119">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="2758b-120">Může vyžadovat další opatření k zamezení zveřejňování těchto tajných kódů uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2758b-120">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="2758b-121">Tajný klíč správce</span><span class="sxs-lookup"><span data-stu-id="2758b-121">Secret Manager</span></span>

<span data-ttu-id="2758b-122">Tajný klíč správce nástroj ukládá citlivých dat během vývoje projektu aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2758b-122">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="2758b-123">V tomto kontextu je část citlivá data tajný kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="2758b-123">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="2758b-124">Tajné kódy aplikace jsou uložené v samostatném umístění ve stromu projektu.</span><span class="sxs-lookup"><span data-stu-id="2758b-124">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="2758b-125">Tajných kódů aplikace jsou přidružené k určitému projektu nebo sdílet mezi více projekty.</span><span class="sxs-lookup"><span data-stu-id="2758b-125">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="2758b-126">Tajných kódů aplikace se změnami do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="2758b-126">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="2758b-127">Nástroj manažera tajných nešifruje uložené tajných kódů a by neměla být zpracována jako důvěryhodné úložiště.</span><span class="sxs-lookup"><span data-stu-id="2758b-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="2758b-128">Je jenom pro účely vývoje.</span><span class="sxs-lookup"><span data-stu-id="2758b-128">It's for development purposes only.</span></span> <span data-ttu-id="2758b-129">Klíče a hodnoty jsou uložené v konfiguračním souboru JSON v adresáři profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="2758b-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="2758b-130">Jak funguje nástroj tajný klíč správce</span><span class="sxs-lookup"><span data-stu-id="2758b-130">How the Secret Manager tool works</span></span>

<span data-ttu-id="2758b-131">Nástroj manažera tajných vyčleňuje podrobnosti implementace, například jak a kde jsou uložené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2758b-131">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="2758b-132">Nástroj můžete použít bez znalosti tyto podrobnosti implementace.</span><span class="sxs-lookup"><span data-stu-id="2758b-132">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="2758b-133">Hodnoty jsou uložené v konfiguračním souboru JSON ve složce profilu systému protected Users na místním počítači:</span><span class="sxs-lookup"><span data-stu-id="2758b-133">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2758b-134">Windows</span><span class="sxs-lookup"><span data-stu-id="2758b-134">Windows</span></span>](#tab/windows)

<span data-ttu-id="2758b-135">Cesta systému souborů:</span><span class="sxs-lookup"><span data-stu-id="2758b-135">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="2758b-136">macOS</span><span class="sxs-lookup"><span data-stu-id="2758b-136">macOS</span></span>](#tab/macos)

<span data-ttu-id="2758b-137">Cesta systému souborů:</span><span class="sxs-lookup"><span data-stu-id="2758b-137">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="2758b-138">Linux</span><span class="sxs-lookup"><span data-stu-id="2758b-138">Linux</span></span>](#tab/linux)

<span data-ttu-id="2758b-139">Cesta systému souborů:</span><span class="sxs-lookup"><span data-stu-id="2758b-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="2758b-140">V předchozím cesty k souborům, nahraďte `<user_secrets_id>` s `UserSecretsId` hodnotu zadanou v *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="2758b-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="2758b-141">Nemusíte psát kód, který závisí na umístění nebo formátu data uložená pomocí nástroje Správce tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="2758b-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="2758b-142">Tyto podrobnosti implementace se může změnit.</span><span class="sxs-lookup"><span data-stu-id="2758b-142">These implementation details may change.</span></span> <span data-ttu-id="2758b-143">Například hodnoty tajných kódů nejsou zašifrované, ale může být v budoucnosti.</span><span class="sxs-lookup"><span data-stu-id="2758b-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="2758b-144">Nainstalujte nástroj tajný klíč správce</span><span class="sxs-lookup"><span data-stu-id="2758b-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="2758b-145">Tajný klíč správce je jako součást balíčku s rozhraní příkazového řádku .NET Core v rozhraní .NET Core SDK 2.1.300 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2758b-145">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="2758b-146">Pro .NET Core SDK verze před 2.1.300 je nutné instalaci nástroje.</span><span class="sxs-lookup"><span data-stu-id="2758b-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="2758b-147">Spustit `dotnet --version` z příkazové okno, chcete-li zobrazit číslo nainstalované verze .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="2758b-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="2758b-148">Pokud se používají sadu .NET Core SDK obsahuje nástroje, zobrazí se upozornění:</span><span class="sxs-lookup"><span data-stu-id="2758b-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="2758b-149">Nainstalujte [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) balíčku NuGet ve vašem projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2758b-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="2758b-150">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2758b-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="2758b-151">Spuštěním následujícího příkazu v příkazovém prostředí se ověřit instalaci nástroje:</span><span class="sxs-lookup"><span data-stu-id="2758b-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="2758b-152">Tajný klíč správce nástroj zobrazí ukázkové využití, možnosti a nápovědy k příkazům:</span><span class="sxs-lookup"><span data-stu-id="2758b-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="2758b-153">Musí být ve stejném adresáři jako *.csproj* spuštění nástroje, které jsou definovány v souboru *.csproj* souboru `DotNetCliToolReference` elementy.</span><span class="sxs-lookup"><span data-stu-id="2758b-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="2758b-154">Nastavte tajného kódu</span><span class="sxs-lookup"><span data-stu-id="2758b-154">Set a secret</span></span>

<span data-ttu-id="2758b-155">Tajný klíč správce nástroj funguje v nastavení konfigurace specifické pro projekt uložené v profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="2758b-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="2758b-156">Pro použití tajných kódů uživatelů, definovat `UserSecretsId` v elementu `PropertyGroup` z *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="2758b-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="2758b-157">Hodnota `UserSecretsId` je volitelný, ale jsou jedinečná pro projekt.</span><span class="sxs-lookup"><span data-stu-id="2758b-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="2758b-158">Vývojáři obvykle generování identifikátoru GUID pro `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="2758b-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="2758b-159">V sadě Visual Studio, klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **spravovat tajné klíče uživatelů** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="2758b-159">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="2758b-160">Přidá tento gesta `UserSecretsId` prvek vyplní identifikátor GUID položky *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="2758b-160">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="2758b-161">Visual Studio otevře *secrets.json* souboru v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="2758b-161">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="2758b-162">Nahraďte obsah *secrets.json* s páry klíč hodnota, které mají být uloženy.</span><span class="sxs-lookup"><span data-stu-id="2758b-162">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="2758b-163">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2758b-163">For example:</span></span>
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> <span data-ttu-id="2758b-164">Struktura JSON se sloučí po změny prostřednictvím `dotnet user-secrets remove` nebo `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="2758b-164">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="2758b-165">Například systém `dotnet user-secrets remove "Movies:ConnectionString"` sbalí `Movies` literálu objektu.</span><span class="sxs-lookup"><span data-stu-id="2758b-165">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="2758b-166">Upravený soubor vypadá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="2758b-166">The modified file resembles the following:</span></span>
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

<span data-ttu-id="2758b-167">Definujte tajný kód aplikace, který se skládá z klíče a jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2758b-167">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="2758b-168">Tajný kód je přiřazena k projektu `UserSecretsId` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2758b-168">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="2758b-169">Například spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:</span><span class="sxs-lookup"><span data-stu-id="2758b-169">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="2758b-170">V předchozím příkladu, který označuje dvojtečka `Movies` je literál s objektu `ServiceApiKey` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2758b-170">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="2758b-171">Nástroj tajný klíč správce je možné z jiných adresářů příliš.</span><span class="sxs-lookup"><span data-stu-id="2758b-171">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="2758b-172">Použití `--project` možnost zadat cestu systému souborů, ve kterém *.csproj* soubor existuje.</span><span class="sxs-lookup"><span data-stu-id="2758b-172">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="2758b-173">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2758b-173">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="2758b-174">Nastavit víc tajných kódů</span><span class="sxs-lookup"><span data-stu-id="2758b-174">Set multiple secrets</span></span>

<span data-ttu-id="2758b-175">Dávku tajné kódy je možné nastavit přesměrujete JSON na `set` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2758b-175">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="2758b-176">V následujícím příkladu *Input.JSON vypadá* obsah souboru jsou směrované do `set` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2758b-176">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2758b-177">Windows</span><span class="sxs-lookup"><span data-stu-id="2758b-177">Windows</span></span>](#tab/windows)

<span data-ttu-id="2758b-178">Otevřete příkazové okno a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2758b-178">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="2758b-179">macOS</span><span class="sxs-lookup"><span data-stu-id="2758b-179">macOS</span></span>](#tab/macos)

<span data-ttu-id="2758b-180">Otevřete příkazové okno a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2758b-180">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2758b-181">Linux</span><span class="sxs-lookup"><span data-stu-id="2758b-181">Linux</span></span>](#tab/linux)

<span data-ttu-id="2758b-182">Otevřete příkazové okno a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2758b-182">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="2758b-183">Přístup k tajným kódem</span><span class="sxs-lookup"><span data-stu-id="2758b-183">Access a secret</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2758b-184">[Rozhraní API pro ASP.NET Core konfigurace](xref:fundamentals/configuration/index) poskytuje přístup k tajným kódům tajný klíč správce.</span><span class="sxs-lookup"><span data-stu-id="2758b-184">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="2758b-185">Pokud váš projekt cílí na .NET Framework, nainstalujte [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="2758b-185">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="2758b-186">V technologii ASP.NET Core 2.0 nebo novější, uživatelský zdroj konfigurace tajných kódů se automaticky přidá ve vývojovém režimu při volání projektu [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) k inicializaci nové instance hostitele s předem nakonfigurované výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2758b-186">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="2758b-187">`CreateDefaultBuilder` volání [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) při [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) je [vývoj](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="2758b-187">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="2758b-188">Když `CreateDefaultBuilder` není volána během vytváření hostitele, přidejte zdroj konfigurace tajných kódů uživatelů pomocí volání [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) v `Startup` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="2758b-188">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="2758b-189">[Rozhraní API pro ASP.NET Core konfigurace](xref:fundamentals/configuration/index) poskytuje přístup k tajným kódům tajný klíč správce.</span><span class="sxs-lookup"><span data-stu-id="2758b-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="2758b-190">Nainstalujte [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="2758b-190">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="2758b-191">Přidat zdroj konfigurace tajných kódů uživatelů pomocí volání [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) v `Startup` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="2758b-191">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="2758b-192">Tajné klíče uživatelů se dá načíst pomocí `Configuration` rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="2758b-192">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="2758b-193">Mapování tajných kódů POCO</span><span class="sxs-lookup"><span data-stu-id="2758b-193">Map secrets to a POCO</span></span>

<span data-ttu-id="2758b-194">Mapování literál celého objektu POCO (jednoduchá třída .NET s vlastnostmi) je užitečné pro agregaci související vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2758b-194">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="2758b-195">Chcete-li namapovat předchozí tajných kódů POCO, použijte `Configuration` rozhraní API [objektu vazby grafu](xref:fundamentals/configuration/index#bind-to-an-object-graph) funkce.</span><span class="sxs-lookup"><span data-stu-id="2758b-195">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="2758b-196">Následující kód vytvoří vazbu k vlastní `MovieSettings` POCO a přístupy `ServiceApiKey` hodnota vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="2758b-196">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="2758b-197">`Movies:ConnectionString` a `Movies:ServiceApiKey` tajných kódů se mapují na odpovídající vlastnosti v `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="2758b-197">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="2758b-198">Náhradní řetězec s tajnými kódy</span><span class="sxs-lookup"><span data-stu-id="2758b-198">String replacement with secrets</span></span>

<span data-ttu-id="2758b-199">Není bezpečné ukládání hesel ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="2758b-199">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="2758b-200">Příklad připojovacího řetězce databáze uloženého v *appsettings.json* může zahrnovat heslo pro zadaného uživatele:</span><span class="sxs-lookup"><span data-stu-id="2758b-200">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="2758b-201">Je lépe zabezpečit přístup k uložení hesla jako tajný kód.</span><span class="sxs-lookup"><span data-stu-id="2758b-201">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="2758b-202">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2758b-202">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="2758b-203">Odeberte `Password` páru klíč hodnota z připojovacího řetězce v *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2758b-203">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="2758b-204">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2758b-204">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="2758b-205">Hodnota tajného klíče se dá nastavit na [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) objektu [heslo](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) vlastnost dokončete připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="2758b-205">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="2758b-206">Uvádí tajné klíče.</span><span class="sxs-lookup"><span data-stu-id="2758b-206">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="2758b-207">Spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:</span><span class="sxs-lookup"><span data-stu-id="2758b-207">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="2758b-208">Zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="2758b-208">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="2758b-209">V předchozím příkladu, dvojtečka v názvu klíče označuje hierarchie objektů v rámci *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="2758b-209">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="2758b-210">Odebrat tajný kód jednotného</span><span class="sxs-lookup"><span data-stu-id="2758b-210">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="2758b-211">Spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:</span><span class="sxs-lookup"><span data-stu-id="2758b-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="2758b-212">Aplikace *secrets.json* změny odebrat páru klíč hodnota, které jsou přidružené k souboru `MoviesConnectionString` klíč:</span><span class="sxs-lookup"><span data-stu-id="2758b-212">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="2758b-213">Spuštění `dotnet user-secrets list` zobrazí následující zprávu:</span><span class="sxs-lookup"><span data-stu-id="2758b-213">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="2758b-214">Odebrání všech tajných kódů</span><span class="sxs-lookup"><span data-stu-id="2758b-214">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="2758b-215">Spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:</span><span class="sxs-lookup"><span data-stu-id="2758b-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="2758b-216">Všechny tajné klíče uživatelů pro aplikaci se odstranily z *secrets.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="2758b-216">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="2758b-217">Spuštění `dotnet user-secrets list` zobrazí následující zprávu:</span><span class="sxs-lookup"><span data-stu-id="2758b-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="2758b-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2758b-218">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
