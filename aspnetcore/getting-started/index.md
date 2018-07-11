---
title: Začínáme s ASP.NET Core
author: rick-anderson
description: Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216210"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="9533d-103">Začínáme s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9533d-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="9533d-104">Nainstalujte [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="9533d-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="9533d-105">Vytvoření projektu aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9533d-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="9533d-106">Otevřete příkazové okno a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9533d-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    <span data-ttu-id="9533d-107">[! Zahrnout [] (~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span><span class="sxs-lookup"><span data-stu-id="9533d-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span></span>

3. <span data-ttu-id="9533d-108">Důvěřujete certifikátu vývoj HTTPS:</span><span class="sxs-lookup"><span data-stu-id="9533d-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="9533d-109">Windows</span><span class="sxs-lookup"><span data-stu-id="9533d-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="9533d-110">macOS</span><span class="sxs-lookup"><span data-stu-id="9533d-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="9533d-111">Linux</span><span class="sxs-lookup"><span data-stu-id="9533d-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="9533d-112">Spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="9533d-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="9533d-113">Přejděte do [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="9533d-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="9533d-114">Klikněte na tlačítko **přijmout** přijměte zásady ochrany osobních údajů a soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="9533d-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="9533d-115">Tato aplikace nemá uchovává osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="9533d-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="9533d-116">Otevřít *Pages/About.cshtml* a upravovat na stránce s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="9533d-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="9533d-117">Přejděte do [ http://localhost:5001/About ](http://localhost:5001/About) a ověřte změny jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="9533d-117">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="9533d-118">Nainstalujte [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="9533d-118">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="9533d-119">Vytvořte nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9533d-119">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="9533d-120">Otevřete příkazové okno.</span><span class="sxs-lookup"><span data-stu-id="9533d-120">Open a command shell.</span></span> <span data-ttu-id="9533d-121">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9533d-121">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="9533d-122">Spusťte aplikaci pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="9533d-122">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="9533d-123">Přejděte do [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="9533d-123">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="9533d-124">Otevřít *Pages/About.cshtml* a upravovat na stránce zobrazí zprávu "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="9533d-124">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="9533d-125">Je čas na serveru @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="9533d-125">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="9533d-126">Přejděte do [ http://localhost:5000/About ](http://localhost:5000/About) a ověřit změny.</span><span class="sxs-lookup"><span data-stu-id="9533d-126">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="9533d-127">Nainstalovat sadu .NET Core **instalační program sady SDK** pro sadu SDK 1.0.4 z [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="9533d-127">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="9533d-128">Vytvořte složku pro nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9533d-128">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="9533d-129">Otevřete příkazové okno.</span><span class="sxs-lookup"><span data-stu-id="9533d-129">Open a command shell.</span></span> <span data-ttu-id="9533d-130">Zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="9533d-130">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="9533d-131">Pokud nainstalujete novější verze sady SDK na svém počítači vytvořte *global.json* vyberte 1.0.4 SDK.</span><span class="sxs-lookup"><span data-stu-id="9533d-131">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="9533d-132">Vytvořte nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9533d-132">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="9533d-133">Obnovení balíčků.</span><span class="sxs-lookup"><span data-stu-id="9533d-133">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="9533d-134">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9533d-134">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="9533d-135">[Dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz sestaví aplikaci nejprve v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="9533d-135">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="9533d-136">Přejděte do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="9533d-136">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
