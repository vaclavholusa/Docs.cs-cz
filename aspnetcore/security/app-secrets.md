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
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Bezpečné úložiště tajné klíče aplikace v vývoj v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [ADAM Roth](https://github.com/danroth27), a [Scott Addie](https://github.com/scottaddie)

::: moniker range="<= aspnetcore-1.1"
[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([stažení](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([stažení](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end

Tento dokument popisuje techniky pro ukládání a načítání citlivá data během vývoje aplikace ASP.NET Core. Ve zdrojovém kódu by nikdy neukládají hesla nebo dalších citlivých dat, a nesmí použít produkční tajných klíčů v vývoj nebo testování režimu. Můžete ukládat a chránit Azure testovací a produkční tajných klíčů s [poskytovatele konfigurace Azure Key Vault](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Proměnné prostředí

Proměnné prostředí se používají k ukládání tajné klíče aplikace v kódu nebo v místní konfigurační soubory vyhnete. Proměnné prostředí přepsat hodnoty konfigurace pro všechny zdroje dříve zadané konfigurace.

::: moniker range="<= aspnetcore-1.1"
Čtení hodnot proměnných prostředí nakonfigurovat voláním [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) v `Startup` konstruktor:

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

Vezměte v úvahu webové aplikace ASP.NET Core, ve kterém **jednotlivé uživatelské účty** je zabezpečená. Připojovací řetězec databáze výchozí je zahrnut do projektu *appSettings.JSON určený* soubor s klíčem `DefaultConnection`. Výchozí připojovací řetězec je pro LocalDB, který běží v uživatelském režimu a nevyžaduje heslo. Během nasazení aplikace `DefaultConnection` klíče hodnotu je možné přepsat hodnotou proměnné prostředí. Proměnné prostředí může ukládat úplný připojovací řetězec s citlivé údaje.

> [!WARNING]
> Proměnné prostředí jsou obecně uložené v nešifrované prostého textu. Když počítač nebo proces ohrožení, proměnné prostředí jsou přístupné nedůvěryhodné strany. Další opatření, aby se zabránilo úniku tajné klíče uživatele může být požadováno.

## <a name="secret-manager"></a>Tajný klíč správce

Nástroj tajný klíč správce ukládá citlivá data během vývoje projektu ASP.NET Core. V tomto kontextu je úsek citlivých dat tajný klíč aplikace. Tajné klíče aplikace jsou uložené v samostatné umístění ze stromu projektu. Tajné klíče aplikace jsou spojené s konkrétní projekt nebo sdílená mezi několika projekty. Tajné klíče aplikace nejsou změnami do správy zdrojového kódu.

> [!WARNING]
> Nástroj tajný klíč správce nemá zašifrování těchto tajných klíčů uložených a nesmí být považované za důvěryhodné úložiště. Je jenom pro účely vývoje. Klíče a hodnoty jsou uložené v konfiguračním souboru JSON v adresáři profilu uživatele.

## <a name="how-the-secret-manager-tool-works"></a>Jak funguje nástroj Správce tajný klíč

Nástroj tajný klíč správce abstrahuje rychle podrobnosti implementace, jako je například kde a jak jsou uložené hodnoty. Nástroj můžete použít bez znalosti tyto podrobnosti implementace. Hodnoty jsou uloženy v [JSON](https://json.org/) konfigurační soubor ve složce profil systému chráněný uživatel v místním počítači:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Cesta v souborovém systému:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Cesta v souborovém systému:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Cesta v souborovém systému:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

V předchozím cesty k souboru, nahraďte `<user_secrets_id>` s `UserSecretsId` hodnoty zadané ve *.csproj* souboru.

Nemusíte psát kód, který závisí na umístění nebo formát data uložená pomocí nástroje Správce tajný klíč. Tyto podrobnosti implementace může změnit. Například tajný hodnoty nejsou zašifrované, ale může být v budoucnu.

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a>Nainstalujte nástroj Správce tajný klíč

Nástroj Správce tajný klíč je instalován s rozhraní příkazového řádku .NET Core v rozhraní .NET Core SDK 2.1. Pro rozhraní .NET Core SDK 2.0 a starší je nutné instalaci nástroje.

Nainstalujte [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) balíček NuGet do projektu ASP.NET Core:

[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

Spusťte následující příkaz v příkazovém řádku ověření instalace nástroje:

```console
dotnet user-secrets -h
```

Nástroj tajný klíč správce zobrazí využití vzorků, možnosti a nápovědu k příkazu:

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
> Musí být ve stejném adresáři jako *.csproj* spuštění nástroje definované v souboru *.csproj* souboru `DotNetCliToolReference` elementy.
::: moniker-end

## <a name="set-a-secret"></a>Nastavit tajný klíč

Nástroj tajný klíč Manager funguje v nastavení projektu specifické konfigurace uložené v profilu uživatele. Pokud chcete použít tajné klíče uživatele, zadejte `UserSecretsId` v rámci `PropertyGroup` z *.csproj* souboru. Hodnota `UserSecretsId` je volitelný, ale je jedinečné pro projekt. Vývojáři obvykle generovat identifikátor GUID pro `UserSecretsId`.

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> V sadě Visual Studio, klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **spravovat tajné klíče uživatele** v místní nabídce. Přidá tento gesto `UserSecretsId` elementu, naplní na identifikátor GUID *.csproj* souboru. Otevře se Visual Studio *secrets.json* soubor v textovém editoru. Nahraďte obsah *secrets.json* s páry klíč hodnota k uložení. Například: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]

Definujte tajný klíč aplikace, který se skládá z klíče a jeho hodnotu. Tajný klíč je přidružen k projektu `UserSecretsId` hodnotu. Například spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

V předchozím příkladu, který označuje dvojtečkou `Movies` je literál s objektem `ServiceApiKey` vlastnost.

Nástroj Správce tajný klíč lze z dalších adresářů příliš. Použití `--project` možnost zadat cesta v souborovém systému, kdy *.csproj* soubor existuje. Příklad:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>Nastavit více tajné klíče

Tím JSON pro lze nastavit dávky tajné klíče `set` příkaz. V následujícím příkladu *input.json* jeho obsah se přesměruje do `set` příkaz.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Otevřete příkazové okno a spusťte následující příkaz:

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Otevřete příkazové okno a spusťte následující příkaz:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Otevřete příkazové okno a spusťte následující příkaz:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Přístup a tajný klíč

[Rozhraní API ASP.NET základní konfigurace](xref:fundamentals/configuration/index) poskytuje přístup k tajné klíče tajný klíč správce. Pokud cílení na .NET Core 1.x nebo rozhraní .NET Framework, nainstalujte [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) balíček NuGet.

::: moniker range="<= aspnetcore-1.1"
Přidat uživatelský zdroj konfigurace tajné klíče na `Startup` konstruktor:

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

Tajné klíče uživatele lze načíst prostřednictvím `Configuration` rozhraní API:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a>Náhradní řetězec obsahující tajné údaje

Ukládání hesel ve formátu prostého textu je rizikové. Například připojovací řetězec databáze uložené v *appSettings.JSON určený* může zahrnovat heslo pro zadaného uživatele:

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

Bezpečnější přístup je uložit heslo jako tajný klíč. Příklad:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Nahraďte heslo v *appSettings.JSON určený* s zástupný symbol. V následujícím příkladu `{0}` slouží jako zástupný symbol pro formulář [složené řetězec formátu](/dotnet/standard/base-types/composite-formatting#composite-format-string).

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

Hodnota tajného klíče může vložit do zástupný symbol pro dokončení připojovací řetězec:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]
::: moniker-end

## <a name="list-the-secrets"></a>Seznam těchto tajných klíčů

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:

```console
dotnet user-secrets list
```

Zobrazí se následující výstup:

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

V předchozím příkladu, dvojtečka v názvy klíčů označuje hierarchie objektů v rámci *secrets.json*.

## <a name="remove-a-single-secret"></a>Odeberte jeden tajný klíč

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

Aplikace *secrets.json* úpravy souboru odebrat přidružené dvojice klíč hodnota `MoviesConnectionString` klíč:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

Spuštění `dotnet user-secrets list` zobrazí následující zprávu:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Odebrání všech tajných klíčů

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Spusťte následující příkaz z adresáře, ve kterém *.csproj* soubor existuje:

```console
dotnet user-secrets clear
```

Všechny tajné klíče uživatele pro aplikaci byl odstraněn z *secrets.json* souboru:

```json
{}
```

Spuštění `dotnet user-secrets list` zobrazí následující zprávu:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Další zdroje

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>