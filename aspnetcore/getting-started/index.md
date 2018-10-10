---
title: Začínáme s ASP.NET Core
author: rick-anderson
description: Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 4a5a0cc5a5dab2171ab8ef43818185a4ee91af0e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912563"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="d5827-103">Kurz: Začínáme s ASP.NET jádro</span><span class="sxs-lookup"><span data-stu-id="d5827-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="d5827-104">Tento návod ukazuje, jak vytvořit webovou aplikaci ASP.NET jádro pomocí rozhraní příkazového řádku .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d5827-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span> <span data-ttu-id="d5827-105">Se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="d5827-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d5827-106">Vytvoření projektu webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d5827-106">Create a web app project.</span></span>
> * <span data-ttu-id="d5827-107">Povolte místní HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5827-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="d5827-108">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d5827-108">Run the app.</span></span>
> * <span data-ttu-id="d5827-109">Úprava stránky syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="d5827-109">Edit a Razor page.</span></span>

<span data-ttu-id="d5827-110">Na konci máte spuštěn v místním počítači pomocí aplikace pracovní web app.</span><span class="sxs-lookup"><span data-stu-id="d5827-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Domovská stránka aplikace](_static/home-page.png)


## <a name="prerequisites"></a><span data-ttu-id="d5827-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d5827-112">Prerequisites</span></span>

* <span data-ttu-id="d5827-113">Nainstalujte [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="d5827-113">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

## <a name="create-a-web-app-project"></a><span data-ttu-id="d5827-114">Vytvoření projektu webové aplikace</span><span class="sxs-lookup"><span data-stu-id="d5827-114">Create a web app project</span></span>

* <span data-ttu-id="d5827-115">Otevřete příkazové prostředí a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d5827-115">Open a command shell, and enter the following command:</span></span>

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

## <a name="enable-local-https"></a><span data-ttu-id="d5827-116">Povolit místní HTTPS</span><span class="sxs-lookup"><span data-stu-id="d5827-116">Enable local HTTPS</span></span>

* <span data-ttu-id="d5827-117">Důvěřujete certifikátu vývoj HTTPS:</span><span class="sxs-lookup"><span data-stu-id="d5827-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="d5827-118">Windows</span><span class="sxs-lookup"><span data-stu-id="d5827-118">Windows</span></span>](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="d5827-119">Ve výstupu předchozího příkazu se zobrazí následující dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="d5827-119">The preceding command displays the following dialog:</span></span>

  ![Dialogové okno upozornění zabezpečení](_static/cert.png)

  <span data-ttu-id="d5827-121">Vyberte **Ano** Pokud vyjádříte souhlas s důvěřovat certifikátu vývoje.</span><span class="sxs-lookup"><span data-stu-id="d5827-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="d5827-122">macOS</span><span class="sxs-lookup"><span data-stu-id="d5827-122">macOS</span></span>](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="d5827-123">Ve výstupu předchozího příkazu se zobrazí následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="d5827-123">The preceding command displays the following message:</span></span>

  <span data-ttu-id="d5827-124">*Byla vyžádána důvěřující vývojářský certifikát HTTPS. Pokud certifikát není důvěryhodný provedeme následující příkaz:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="d5827-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
  <span data-ttu-id="d5827-125">\* Tento příkaz vás může vyzvat k zadání hesla k instalaci certifikátu v řetězci klíčů systému.</span><span class="sxs-lookup"><span data-stu-id="d5827-125">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>
  
  <span data-ttu-id="d5827-126">Heslo: \*</span><span class="sxs-lookup"><span data-stu-id="d5827-126">Password:\*</span></span>

  <span data-ttu-id="d5827-127">Zadejte svoje heslo, pokud vyjádříte souhlas s důvěřovat certifikátu vývoje.</span><span class="sxs-lookup"><span data-stu-id="d5827-127">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="d5827-128">Linux</span><span class="sxs-lookup"><span data-stu-id="d5827-128">Linux</span></span>](#tab/linux)

  <span data-ttu-id="d5827-129">O tom, jak důvěřovat certifikátu protokolu HTTPS vývoj naleznete v dokumentaci k vaší distribuci Linuxu.</span><span class="sxs-lookup"><span data-stu-id="d5827-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>
   
---

## <a name="run-the-app"></a><span data-ttu-id="d5827-130">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="d5827-130">Run the app</span></span>

* <span data-ttu-id="d5827-131">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d5827-131">Run the following commands:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

* <span data-ttu-id="d5827-132">Přejděte do [ https://localhost:5001 ](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="d5827-132">Browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="d5827-133">Klikněte na tlačítko **přijmout** přijměte zásady ochrany osobních údajů a soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="d5827-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="d5827-134">Tato aplikace nemá uchovává osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="d5827-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="d5827-135">Úprava stránky syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="d5827-135">Edit a Razor page</span></span>

* <span data-ttu-id="d5827-136">Otevřít *Pages/About.cshtml* a upravovat na stránce s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="d5827-136">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

* <span data-ttu-id="d5827-137">Přejděte do [ https://localhost:5001/About ](https://localhost:5001/About) a ověřte změny jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="d5827-137">Browse to [https://localhost:5001/About](https://localhost:5001/About) and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5827-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5827-138">Next steps</span></span>

<span data-ttu-id="d5827-139">V tomto kurzu jste zjistili, jak:</span><span class="sxs-lookup"><span data-stu-id="d5827-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d5827-140">Vytvoření projektu webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d5827-140">Create a web app project.</span></span>
> * <span data-ttu-id="d5827-141">Povolte místní HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5827-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="d5827-142">Spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="d5827-142">Run the project.</span></span>
> * <span data-ttu-id="d5827-143">Proveďte změnu.</span><span class="sxs-lookup"><span data-stu-id="d5827-143">Make a change.</span></span>

<span data-ttu-id="d5827-144">Další informace o základní technologie ASP.NET naleznete v tématu zavedení:</span><span class="sxs-lookup"><span data-stu-id="d5827-144">To learn more about ASP.NET Core, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index>
