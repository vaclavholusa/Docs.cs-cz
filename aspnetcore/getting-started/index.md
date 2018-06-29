---
title: Začínáme s ASP.NET Core
author: rick-anderson
description: Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077657"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="a2c69-103">Začínáme s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2c69-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="a2c69-104">Nainstalujte [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="a2c69-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="a2c69-105">Vytvoření projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a2c69-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="a2c69-106">Otevřete příkazové okno a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a2c69-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    <span data-ttu-id="a2c69-107">[! Zahrnout [] (~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span><span class="sxs-lookup"><span data-stu-id="a2c69-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span></span>

3. <span data-ttu-id="a2c69-108">Důvěřujete certifikátu vývoj HTTPS:</span><span class="sxs-lookup"><span data-stu-id="a2c69-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="a2c69-109">Windows</span><span class="sxs-lookup"><span data-stu-id="a2c69-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="a2c69-110">macOS</span><span class="sxs-lookup"><span data-stu-id="a2c69-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="a2c69-111">Linux</span><span class="sxs-lookup"><span data-stu-id="a2c69-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="a2c69-112">Spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="a2c69-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="a2c69-113">Přejděte do [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="a2c69-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="a2c69-114">Klikněte na tlačítko **přijmout** přijměte zásady ochrany osobních údajů a souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="a2c69-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="a2c69-115">Tato aplikace nepodporuje uchovává osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="a2c69-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="a2c69-116">Otevřete *Pages/About.cshtml* a upravovat stránky s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="a2c69-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="a2c69-117">Přejděte do [ http://localhost:5001/About ](http://localhost:5001/About) a ověřte, změny se projeví.</span><span class="sxs-lookup"><span data-stu-id="a2c69-117">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="a2c69-118">Nainstalujte [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="a2c69-118">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="a2c69-119">Vytvořte nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a2c69-119">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="a2c69-120">Otevřete příkazové okno.</span><span class="sxs-lookup"><span data-stu-id="a2c69-120">Open a command shell.</span></span> <span data-ttu-id="a2c69-121">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a2c69-121">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="a2c69-122">Spusťte aplikaci pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="a2c69-122">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="a2c69-123">Přejděte do [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="a2c69-123">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="a2c69-124">Otevřete *Pages/About.cshtml* a upravte stránku a zobrazí se zpráva "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="a2c69-124">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="a2c69-125">Je čas na serveru @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="a2c69-125">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="a2c69-126">Přejděte do [ http://localhost:5000/About ](http://localhost:5000/About) a ověřit změny.</span><span class="sxs-lookup"><span data-stu-id="a2c69-126">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="a2c69-127">Instalace .NET Core **instalační program sady SDK** pro sadu SDK 1.0.4 z [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="a2c69-127">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="a2c69-128">Vytvořte složku pro nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a2c69-128">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="a2c69-129">Otevřete příkazové okno.</span><span class="sxs-lookup"><span data-stu-id="a2c69-129">Open a command shell.</span></span> <span data-ttu-id="a2c69-130">Zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="a2c69-130">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="a2c69-131">Pokud jste nainstalovali novější verze sady SDK na váš počítač, vytvořte *global.json* soubor a vyberte 1.0.4 SDK.</span><span class="sxs-lookup"><span data-stu-id="a2c69-131">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="a2c69-132">Vytvořte nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a2c69-132">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="a2c69-133">Obnovení balíčků.</span><span class="sxs-lookup"><span data-stu-id="a2c69-133">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="a2c69-134">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a2c69-134">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="a2c69-135">[Dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz sestavení aplikace nejprve v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="a2c69-135">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="a2c69-136">Přejděte do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a2c69-136">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
