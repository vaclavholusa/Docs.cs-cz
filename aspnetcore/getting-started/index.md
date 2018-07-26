---
title: Začínáme s ASP.NET Core
author: rick-anderson
description: Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 7ab9f303d74786c4ac76f002d0f2c66371e78cb8
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228579"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="96b38-103">Začínáme s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96b38-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="96b38-104">Nainstalujte [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="96b38-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="96b38-105">Vytvoření projektu aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96b38-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="96b38-106">Otevřete příkazové okno a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="96b38-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="96b38-107">Důvěřujete certifikátu vývoj HTTPS:</span><span class="sxs-lookup"><span data-stu-id="96b38-107">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="96b38-108">Windows</span><span class="sxs-lookup"><span data-stu-id="96b38-108">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="96b38-109">Ve výstupu předchozího příkazu se zobrazí následující dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="96b38-109">The preceding command displays the following dialog:</span></span>

   ![Dialogové okno upozornění zabezpečení](_static/cert.png)

   <span data-ttu-id="96b38-111">Vyberte **Ano** Pokud vyjádříte souhlas s důvěřovat certifikátu vývoje.</span><span class="sxs-lookup"><span data-stu-id="96b38-111">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="96b38-112">macOS</span><span class="sxs-lookup"><span data-stu-id="96b38-112">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="96b38-113">Ve výstupu předchozího příkazu se zobrazí následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="96b38-113">The preceding command displays the following message:</span></span>

   <span data-ttu-id="96b38-114">*Byla vyžádána důvěřující vývojářský certifikát HTTPS. Pokud certifikát není důvěryhodný provedeme následující příkaz:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *tento příkaz vás může vyzvat k zadání hesla k instalaci certifikátu v řetězci klíčů systému.    Heslo:*</span><span class="sxs-lookup"><span data-stu-id="96b38-114">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *This command might prompt you for your password to install the certificate on the system keychain.    Password:*</span></span>

   <span data-ttu-id="96b38-115">Zadejte svoje heslo, pokud vyjádříte souhlas s důvěřovat certifikátu vývoje.</span><span class="sxs-lookup"><span data-stu-id="96b38-115">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="96b38-116">Linux</span><span class="sxs-lookup"><span data-stu-id="96b38-116">Linux</span></span>](#tab/linux)

   <a name="see-the-documentation-for-your-linux-distribution-on-how-to-trust-the-https-development-certificate"></a><span data-ttu-id="96b38-117">O tom, jak důvěřovat certifikátu protokolu HTTPS vývoj naleznete v dokumentaci k vaší distribuci Linuxu</span><span class="sxs-lookup"><span data-stu-id="96b38-117">See the documentation for your Linux distribution on how to trust the HTTPS development certificate</span></span>
---

4. <span data-ttu-id="96b38-118">Spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="96b38-118">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="96b38-119">Přejděte do [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="96b38-119">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="96b38-120">Klikněte na tlačítko **přijmout** přijměte zásady ochrany osobních údajů a soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="96b38-120">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="96b38-121">Tato aplikace nemá uchovává osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="96b38-121">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="96b38-122">Otevřít *Pages/About.cshtml* a upravovat na stránce s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="96b38-122">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="96b38-123">Přejděte do [ http://localhost:5001/About ](http://localhost:5001/About) a ověřte změny jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="96b38-123">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="96b38-124">Nainstalujte [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="96b38-124">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="96b38-125">Vytvořte nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96b38-125">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="96b38-126">Otevřete příkazové okno.</span><span class="sxs-lookup"><span data-stu-id="96b38-126">Open a command shell.</span></span> <span data-ttu-id="96b38-127">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="96b38-127">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="96b38-128">Spusťte aplikaci pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="96b38-128">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="96b38-129">Přejděte do [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="96b38-129">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="96b38-130">Otevřít *Pages/About.cshtml* a upravovat na stránce zobrazí zprávu "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="96b38-130">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="96b38-131">Je čas na serveru @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="96b38-131">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="96b38-132">Přejděte do [ http://localhost:5000/About ](http://localhost:5000/About) a ověřit změny.</span><span class="sxs-lookup"><span data-stu-id="96b38-132">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="96b38-133">Nainstalovat sadu .NET Core **instalační program sady SDK** pro sadu SDK 1.0.4 z [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="96b38-133">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="96b38-134">Vytvořte složku pro nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96b38-134">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="96b38-135">Otevřete příkazové okno.</span><span class="sxs-lookup"><span data-stu-id="96b38-135">Open a command shell.</span></span> <span data-ttu-id="96b38-136">Zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="96b38-136">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="96b38-137">Pokud nainstalujete novější verze sady SDK na svém počítači vytvořte *global.json* vyberte 1.0.4 SDK.</span><span class="sxs-lookup"><span data-stu-id="96b38-137">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="96b38-138">Vytvořte nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96b38-138">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="96b38-139">Obnovení balíčků.</span><span class="sxs-lookup"><span data-stu-id="96b38-139">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="96b38-140">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="96b38-140">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="96b38-141">[Dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz sestaví aplikaci nejprve v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="96b38-141">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="96b38-142">Přejděte do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="96b38-142">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
