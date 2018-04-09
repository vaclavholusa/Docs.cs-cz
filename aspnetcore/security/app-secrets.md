---
title: Bezpečné úložiště tajné klíče aplikace v vývoj v ASP.NET Core
author: rick-anderson
description: Ukazuje, jak bezpečně uložit tajné klíče během vývoje
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 166111696a9c4244ede44fca8878dd3725bb3099
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Bezpečné úložiště tajné klíče aplikace v vývoj v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [ADAM Roth](https://github.com/danroth27), a [Scott Addie](https://scottaddie.com) 

Tento dokument ukazuje, jak pomocí nástroje Správce tajný klíč v vývoj zachovat tajné klíče z vašeho kódu. Nejdůležitější bod je ve zdrojovém kódu by nikdy neukládají hesla nebo dalších citlivých dat, a tajné klíče produkční byste neměli používat v režimu pro vývoj a testování. Místo toho můžete [konfigurace](xref:fundamentals/configuration/index) systému a přečtěte si tyto hodnoty z proměnné prostředí nebo z hodnot uložených pomocí tajný klíč správce nástroje. Nástroj tajný klíč správce pomáhá zabránit citlivá data z kontroly do správy zdrojového kódu. [Konfigurace](xref:fundamentals/configuration/index) systému může číst tajné klíče uložené pomocí nástroje Správce tajný klíč popsané v tomto článku.

Nástroj Správce tajný klíč se používá pouze v vývoj. Můžete zabezpečit Azure testovací a produkční tajných klíčů s [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) poskytovatele konfigurace. V tématu [poskytovatele konfigurace Azure Key Vault](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) Další informace.

## <a name="environment-variables"></a>Proměnné prostředí

Abyste se vyhnuli ukládání tajné klíče aplikace v kódu nebo v místní konfigurační soubory, ukládat tajné klíče v seznamu proměnných prostředí. Můžete nastavit [konfigurace](xref:fundamentals/configuration/index) framework čtení hodnoty z proměnných prostředí voláním `AddEnvironmentVariables`. Proměnné prostředí poté můžete přepsat hodnoty konfigurace pro všechny zdroje dříve zadané konfigurace.

Například pokud vytvoříte novou webovou aplikaci ASP.NET Core s jednotlivých uživatelských účtů, přidá výchozí připojovacího řetězce, který *appSettings.JSON určený* v projektu s klíčem `DefaultConnection`. Výchozí připojovací řetězec je LocalDB, který běží v uživatelském režimu a nevyžaduje heslo pomocí instalačního programu. Když nasadíte aplikaci k testu nebo produkčním serveru, můžete přepsat `DefaultConnection` hodnotu klíče s nastavení proměnné prostředí, který obsahuje připojovací řetězec (s citlivé přihlašovací údaje) pro testovací nebo provozní databázi Server.

>[!WARNING]
> Proměnné prostředí jsou obecně uložené ve formátu prostého textu a nejsou šifrována. Když počítač nebo proces ohrožení, pak proměnné prostředí jsou přístupné nedůvěryhodné strany. Další opatření, aby se zabránilo úniku tajné klíče uživatele může být stále nutný.

## <a name="secret-manager"></a>Tajný klíč správce

Nástroj tajný klíč správce ukládá citlivá data pro vývojové práci mimo váš projekt stromu. Nástroj Správce tajný klíč je nástroj projektu, který slouží k uložení tajné klíče pro projekt .NET Core během vývoje. Pomocí nástroje Správce tajný klíč můžete přidružit konkrétní projekt tajné klíče aplikace a sdílet je ve více projektech.

>[!WARNING]
> Nástroj tajný klíč správce nemá zašifrování těchto tajných klíčů uložených a nesmí být považované za důvěryhodné úložiště. Je jenom pro účely vývoje. Klíče a hodnoty jsou uložené v konfiguračním souboru JSON v adresáři profilu uživatele.

## <a name="installing-the-secret-manager-tool"></a>Instalace nástroje Správce tajný klíč

#### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)
Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **upravit \<název_projektu\>.csproj** v místní nabídce. Přidejte zvýrazněný řádek na *.csproj* souboru a uložte obnovit přidruženého balíčku NuGet:

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

Znovu klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **spravovat tajné klíče uživatele** v místní nabídce. Přidá nový tento gesto `UserSecretsId` uzel v rámci `PropertyGroup` z *.csproj* souboru, jak je znázorněno v následující ukázce:

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

Ukládání upravenou *.csproj* také soubor otevře `secrets.json` soubor v textovém editoru. Nahraďte obsah `secrets.json` soubor s následujícím kódem:

```json
{
    "MySecret": "ValueOfMySecret"
}
```

#### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)
Přidat `Microsoft.Extensions.SecretManager.Tools` k *.csproj* souboru a spusťte [dotnet obnovení](/dotnet/core/tools/dotnet-restore). Stejný postup můžete použít k instalaci nástroje Správce tajný klíč pomocí příkazového řádku.

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

Testovací nástroje tajný klíč Manager tak, že spustíte následující příkaz:

```console
dotnet user-secrets -h
```

Nástroj tajný klíč správce se zobrazí, využití, možnosti a nápovědu k příkazu.

> [!NOTE]
> Musí být ve stejném adresáři jako *.csproj* spuštění nástroje definované v souboru *.csproj* souboru `DotNetCliToolReference` uzlů.

Nástroj tajný klíč Manager funguje v nastavení konfigurace specifických projektu, které jsou uložené v profilu uživatele. Chcete-li použít tajné klíče uživatele, musíte zadat projektu `UserSecretsId` hodnotu v jeho *.csproj* souboru. Hodnota `UserSecretsId` je volitelný, ale je obecně jedinečné do projektu. Vývojáři obvykle generovat identifikátor GUID pro `UserSecretsId`.

Přidat `UserSecretsId` pro svůj projekt v *.csproj* souboru:

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

Chcete-li nastavit tajného klíče pomocí nástroje Správce tajný klíč. Například v příkazovém okně z adresáře projektu, zadejte následující příkaz:

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

Můžete spustit nástroj Správce tajný klíč z dalších adresářů, ale je nutné použít `--project` možnost předat v cestě k *.csproj* souboru:

```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

Můžete také nástroj Správce tajný klíč do seznamu, odebrat a vymazat tajné klíče aplikace.

* * *
## <a name="accessing-user-secrets-via-configuration"></a>Přístup k tajné klíče uživatele prostřednictvím konfigurace

Tajné klíče tajný klíč správce přistupujete prostřednictvím systému konfigurace. Přidat `Microsoft.Extensions.Configuration.UserSecrets` balíček a spusťte [dotnet obnovení](/dotnet/core/tools/dotnet-restore).

Přidat zdroj konfigurace tajné klíče uživatele na `Startup` metoda:

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

Tajné klíče uživatele prostřednictvím rozhraní API konfigurace se můžete dostat:

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a>Jak funguje nástroj Správce tajný klíč

Nástroj tajný klíč správce abstrahuje rychle podrobnosti implementace, jako je například kde a jak jsou uložené hodnoty. Nástroj můžete použít bez znalosti tyto podrobnosti implementace. V aktuální verzi, hodnoty jsou uloženy v [JSON](http://json.org/) konfigurační soubor v adresáři profilu uživatele:

* Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`

* Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

* macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

Hodnota `userSecretsId` pochází z hodnoty zadané ve *.csproj* souboru.

Neměli psát kód, který závisí na umístění nebo formát dat pomocí nástroje Správce tajný klíč uložit jako, může se měnit tyto podrobnosti implementace. Například tajný hodnoty jsou aktuálně *není* šifruje dnes, ale může být jednou budete.

## <a name="additional-resources"></a>Další zdroje

* [Konfigurace](xref:fundamentals/configuration/index)
