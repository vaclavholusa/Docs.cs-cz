---
title: Začínáme s ASP.NET Core
author: rick-anderson
description: Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: a6a5023594aec01370143e7d1f35fb45c109122a
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860937"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="9633e-103">Začínáme s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9633e-103">Get started with ASP.NET Core</span></span>

<span data-ttu-id="9633e-104">Tento dokument popisuje kroky pro vytvoření a spuštění aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9633e-104">This document provides steps for creating and running an ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="9633e-105">Nainstalujte [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="9633e-105">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="9633e-106">Vytvoření projektu aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9633e-106">Create an ASP.NET Core project.</span></span> <span data-ttu-id="9633e-107">Otevřete příkazové okno a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9633e-107">Open a command shell and enter the following command:</span></span>

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

3. <span data-ttu-id="9633e-108">Důvěřujete certifikátu vývoj HTTPS:</span><span class="sxs-lookup"><span data-stu-id="9633e-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="9633e-109">Windows</span><span class="sxs-lookup"><span data-stu-id="9633e-109">Windows</span></span>](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="9633e-110">Ve výstupu předchozího příkazu se zobrazí následující dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="9633e-110">The preceding command displays the following dialog:</span></span>

  ![Dialogové okno upozornění zabezpečení](_static/cert.png)

  <span data-ttu-id="9633e-112">Vyberte **Ano** Pokud vyjádříte souhlas s důvěřovat certifikátu vývoje.</span><span class="sxs-lookup"><span data-stu-id="9633e-112">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="9633e-113">macOS</span><span class="sxs-lookup"><span data-stu-id="9633e-113">macOS</span></span>](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="9633e-114">Ve výstupu předchozího příkazu se zobrazí následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="9633e-114">The preceding command displays the following message:</span></span>

  <span data-ttu-id="9633e-115">*Byla vyžádána důvěřující vývojářský certifikát HTTPS. Pokud certifikát není důvěryhodný provedeme následující příkaz:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="9633e-115">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
  <span data-ttu-id="9633e-116">\* Tento příkaz vás může vyzvat k zadání hesla k instalaci certifikátu v řetězci klíčů systému.</span><span class="sxs-lookup"><span data-stu-id="9633e-116">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>
  
  <span data-ttu-id="9633e-117">Heslo: \*</span><span class="sxs-lookup"><span data-stu-id="9633e-117">Password:\*</span></span>

  <span data-ttu-id="9633e-118">Zadejte svoje heslo, pokud vyjádříte souhlas s důvěřovat certifikátu vývoje.</span><span class="sxs-lookup"><span data-stu-id="9633e-118">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="9633e-119">Linux</span><span class="sxs-lookup"><span data-stu-id="9633e-119">Linux</span></span>](#tab/linux)

  <span data-ttu-id="9633e-120">O tom, jak důvěřovat certifikátu protokolu HTTPS vývoj naleznete v dokumentaci k vaší distribuci Linuxu.</span><span class="sxs-lookup"><span data-stu-id="9633e-120">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>
   
---

4. <span data-ttu-id="9633e-121">Spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="9633e-121">Run the app:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

5. <span data-ttu-id="9633e-122">Přejděte do [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="9633e-122">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="9633e-123">Klikněte na tlačítko **přijmout** přijměte zásady ochrany osobních údajů a soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="9633e-123">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="9633e-124">Tato aplikace nemá uchovává osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="9633e-124">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="9633e-125">Otevřít *Pages/About.cshtml* a upravovat na stránce s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="9633e-125">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="9633e-126">Přejděte do [ http://localhost:5001/About ](http://localhost:5001/About) a ověřte změny jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="9633e-126">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="9633e-127">Nainstalujte [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="9633e-127">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="9633e-128">Vytvořte nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9633e-128">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="9633e-129">Otevřete příkazové okno.</span><span class="sxs-lookup"><span data-stu-id="9633e-129">Open a command shell.</span></span> <span data-ttu-id="9633e-130">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9633e-130">Enter the following command:</span></span>

   ```console
   dotnet new razor -o aspnetcoreapp
   ```

3. <span data-ttu-id="9633e-131">Spusťte aplikaci pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="9633e-131">Run the app with the following commands:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

4. <span data-ttu-id="9633e-132">Přejděte do [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="9633e-132">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="9633e-133">Otevřít *Pages/About.cshtml* a upravovat na stránce zobrazí zprávu "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="9633e-133">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="9633e-134">Je čas na serveru @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="9633e-134">The time on the server is @DateTime.Now":</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="9633e-135">Přejděte do [ http://localhost:5000/About ](http://localhost:5000/About) a ověřit změny.</span><span class="sxs-lookup"><span data-stu-id="9633e-135">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="9633e-136">Nainstalovat sadu .NET Core **instalační program sady SDK** pro sadu SDK 1.0.4 z [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="9633e-136">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="9633e-137">Vytvořte složku pro nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9633e-137">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="9633e-138">Otevřete příkazové okno.</span><span class="sxs-lookup"><span data-stu-id="9633e-138">Open a command shell.</span></span> <span data-ttu-id="9633e-139">Zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="9633e-139">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="9633e-140">Pokud nainstalujete novější verze sady SDK na svém počítači vytvořte *global.json* vyberte 1.0.4 SDK.</span><span class="sxs-lookup"><span data-stu-id="9633e-140">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="9633e-141">Vytvořte nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9633e-141">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="9633e-142">Obnovení balíčků.</span><span class="sxs-lookup"><span data-stu-id="9633e-142">Restore the packages.</span></span>

   ```console
   dotnet restore
   ```

6. <span data-ttu-id="9633e-143">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9633e-143">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="9633e-144">[Dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz sestaví aplikaci nejprve v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="9633e-144">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="9633e-145">Přejděte do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="9633e-145">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end
