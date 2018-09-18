---
title: Začínáme s ASP.NET Core
author: rick-anderson
description: Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 06129200834607188052f44a888749c51662f638
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011592"
---
# <a name="get-started-with-aspnet-core"></a>Začínáme s ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

1. Nainstalujte [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Vytvoření projektu aplikace ASP.NET Core. Otevřete příkazové okno a zadejte následující příkaz:

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. Důvěřujete certifikátu vývoj HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   Ve výstupu předchozího příkazu se zobrazí následující dialogové okno:

   ![Dialogové okno upozornění zabezpečení](_static/cert.png)

   Vyberte **Ano** Pokud vyjádříte souhlas s důvěřovat certifikátu vývoje.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   Ve výstupu předchozího příkazu se zobrazí následující zpráva:

   *Byla vyžádána důvěřující vývojářský certifikát HTTPS. Pokud certifikát není důvěryhodný jsme se spusťte následující příkaz:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
   *Tento příkaz vás může vyzvat k zadání hesla k instalaci certifikátu v řetězci klíčů systému. Heslo:*

   Zadejte svoje heslo, pokud vyjádříte souhlas s důvěřovat certifikátu vývoje.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

   O tom, jak důvěřovat certifikátu protokolu HTTPS vývoj naleznete v dokumentaci k vaší distribuci Linuxu
   
---

4. Spuštění aplikace:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. Přejděte do [ http://localhost:5001 ](http://localhost:5001).  Klikněte na tlačítko **přijmout** přijměte zásady ochrany osobních údajů a soubory cookie. Tato aplikace nemá uchovává osobní údaje.

6. Otevřít *Pages/About.cshtml* a upravovat na stránce s následující zvýrazněný kód:

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. Přejděte do [ http://localhost:5001/About ](http://localhost:5001/About) a ověřte změny jsou zobrazeny.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. Nainstalujte [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Vytvořte nový projekt ASP.NET Core.

   Otevřete příkazové okno. Zadejte následující příkaz:

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. Spusťte aplikaci pomocí následujících příkazů:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. Přejděte do [ http://localhost:5000 ](http://localhost:5000).

5. Otevřít *Pages/About.cshtml* a upravovat na stránce zobrazí zprávu "Hello, world! Je čas na serveru @DateTime.Now":

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Přejděte do [ http://localhost:5000/About ](http://localhost:5000/About) a ověřit změny.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Nainstalovat sadu .NET Core **instalační program sady SDK** pro sadu SDK 1.0.4 z [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).

2. Vytvořte složku pro nový projekt ASP.NET Core.

   Otevřete příkazové okno. Zadejte následující příkazy:

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Pokud nainstalujete novější verze sady SDK na svém počítači vytvořte *global.json* vyberte 1.0.4 SDK.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Vytvořte nový projekt ASP.NET Core.

   ```console
   dotnet new web
   ```

5. Obnovení balíčků.

    ```console
    dotnet restore
    ```

6. Spusťte aplikaci.

   ```console
   dotnet run
   ```

   [Dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz sestaví aplikaci nejprve v případě potřeby.

7. Přejděte do `http://localhost:5000`.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end
