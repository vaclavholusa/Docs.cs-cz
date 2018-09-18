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
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="29f4f-103">Začínáme s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="29f4f-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="29f4f-104">Nainstalujte [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="29f4f-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="29f4f-105">Vytvoření projektu aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="29f4f-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="29f4f-106">Otevřete příkazové okno a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="29f4f-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="29f4f-107">Důvěřujete certifikátu vývoj HTTPS:</span><span class="sxs-lookup"><span data-stu-id="29f4f-107">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="29f4f-108">Windows</span><span class="sxs-lookup"><span data-stu-id="29f4f-108">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="29f4f-109">Ve výstupu předchozího příkazu se zobrazí následující dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="29f4f-109">The preceding command displays the following dialog:</span></span>

   ![Dialogové okno upozornění zabezpečení](_static/cert.png)

   <span data-ttu-id="29f4f-111">Vyberte **Ano** Pokud vyjádříte souhlas s důvěřovat certifikátu vývoje.</span><span class="sxs-lookup"><span data-stu-id="29f4f-111">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="29f4f-112">macOS</span><span class="sxs-lookup"><span data-stu-id="29f4f-112">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="29f4f-113">Ve výstupu předchozího příkazu se zobrazí následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="29f4f-113">The preceding command displays the following message:</span></span>

   <span data-ttu-id="29f4f-114">*Byla vyžádána důvěřující vývojářský certifikát HTTPS. Pokud certifikát není důvěryhodný jsme se spusťte následující příkaz:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="29f4f-114">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>
   <span data-ttu-id="29f4f-115">*Tento příkaz vás může vyzvat k zadání hesla k instalaci certifikátu v řetězci klíčů systému. Heslo:*</span><span class="sxs-lookup"><span data-stu-id="29f4f-115">*This command might prompt you for your password to install the certificate on the system keychain. Password:*</span></span>

   <span data-ttu-id="29f4f-116">Zadejte svoje heslo, pokud vyjádříte souhlas s důvěřovat certifikátu vývoje.</span><span class="sxs-lookup"><span data-stu-id="29f4f-116">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="29f4f-117">Linux</span><span class="sxs-lookup"><span data-stu-id="29f4f-117">Linux</span></span>](#tab/linux)

   <span data-ttu-id="29f4f-118">O tom, jak důvěřovat certifikátu protokolu HTTPS vývoj naleznete v dokumentaci k vaší distribuci Linuxu</span><span class="sxs-lookup"><span data-stu-id="29f4f-118">See the documentation for your Linux distribution on how to trust the HTTPS development certificate</span></span>
   
---

4. <span data-ttu-id="29f4f-119">Spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="29f4f-119">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="29f4f-120">Přejděte do [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="29f4f-120">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="29f4f-121">Klikněte na tlačítko **přijmout** přijměte zásady ochrany osobních údajů a soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="29f4f-121">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="29f4f-122">Tato aplikace nemá uchovává osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="29f4f-122">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="29f4f-123">Otevřít *Pages/About.cshtml* a upravovat na stránce s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="29f4f-123">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="29f4f-124">Přejděte do [ http://localhost:5001/About ](http://localhost:5001/About) a ověřte změny jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="29f4f-124">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="29f4f-125">Nainstalujte [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="29f4f-125">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="29f4f-126">Vytvořte nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="29f4f-126">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="29f4f-127">Otevřete příkazové okno.</span><span class="sxs-lookup"><span data-stu-id="29f4f-127">Open a command shell.</span></span> <span data-ttu-id="29f4f-128">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="29f4f-128">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="29f4f-129">Spusťte aplikaci pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="29f4f-129">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="29f4f-130">Přejděte do [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="29f4f-130">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="29f4f-131">Otevřít *Pages/About.cshtml* a upravovat na stránce zobrazí zprávu "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="29f4f-131">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="29f4f-132">Je čas na serveru @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="29f4f-132">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="29f4f-133">Přejděte do [ http://localhost:5000/About ](http://localhost:5000/About) a ověřit změny.</span><span class="sxs-lookup"><span data-stu-id="29f4f-133">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="29f4f-134">Nainstalovat sadu .NET Core **instalační program sady SDK** pro sadu SDK 1.0.4 z [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="29f4f-134">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="29f4f-135">Vytvořte složku pro nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="29f4f-135">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="29f4f-136">Otevřete příkazové okno.</span><span class="sxs-lookup"><span data-stu-id="29f4f-136">Open a command shell.</span></span> <span data-ttu-id="29f4f-137">Zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="29f4f-137">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="29f4f-138">Pokud nainstalujete novější verze sady SDK na svém počítači vytvořte *global.json* vyberte 1.0.4 SDK.</span><span class="sxs-lookup"><span data-stu-id="29f4f-138">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="29f4f-139">Vytvořte nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="29f4f-139">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="29f4f-140">Obnovení balíčků.</span><span class="sxs-lookup"><span data-stu-id="29f4f-140">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="29f4f-141">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="29f4f-141">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="29f4f-142">[Dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz sestaví aplikaci nejprve v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="29f4f-142">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="29f4f-143">Přejděte do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="29f4f-143">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end
