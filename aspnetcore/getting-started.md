---
title: Začínáme s ASP.NET Core
author: rick-anderson
description: Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 5/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: a7233082e9262e6976cec3b086900a5dda069213
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34730435"
---
# <a name="get-started-with-aspnet-core"></a>Začínáme s ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

1. Nainstalujte [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Vytvoření projektu ASP.NET Core. Otevřete příkazové okno a zadejte následující příkaz:

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

3. Důvěřujete certifikátu vývoj HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](getting-started/_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. Spuštění aplikace:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. Přejděte do [ http://localhost:5001 ](http://localhost:5001).  Klikněte na tlačítko **přijmout** přijměte zásady ochrany osobních údajů a souborů cookie. Tato aplikace nepodporuje uchovává osobní údaje.

6. Otevřete *Pages/About.cshtml* a upravovat stránky s následující zvýrazněný kód:

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9)]

7. Přejděte do [ http://localhost:5001/About ](http://localhost:5001/About) a ověřte, změny se projeví.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

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

5. Otevřete *Pages/About.cshtml* a upravte stránku a zobrazí se zpráva "Hello, world! Je čas na serveru @DateTime.Now":

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Přejděte do [ http://localhost:5000/About ](http://localhost:5000/About) a ověřit změny.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Instalace .NET Core **instalační program sady SDK** pro sadu SDK 1.0.4 z [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).

2. Vytvořte složku pro nový projekt ASP.NET Core.

   Otevřete příkazové okno. Zadejte následující příkazy:

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Pokud jste nainstalovali novější verze sady SDK na váš počítač, vytvořte *global.json* soubor a vyberte 1.0.4 SDK.

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

   [Dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz sestavení aplikace nejprve v případě potřeby.

7. Přejděte do `http://localhost:5000`.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
