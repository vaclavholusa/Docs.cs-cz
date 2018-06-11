---
title: Publikování aplikace ASP.NET Core do Azure pomocí nástroje příkazového řádku
author: camsoper
description: Zjistěte, jak publikovat aplikaci ASP.NET Core Azure App Service pomocí příkazového řádku klienta Git.
manager: wpickett
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
ms.devlang: dotnet
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 3fc068096a4b8696340787aa15120a2f97d10164
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252435"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="21e12-103">Publikování aplikace ASP.NET Core do Azure pomocí nástroje příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="21e12-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="21e12-104">Podle [Soper kamera](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="21e12-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="21e12-105">Tento kurz vám ukáže, jak vytvářet a nasazovat aplikaci ASP.NET Core do Microsoft Azure App Service pomocí nástrojů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="21e12-105">This tutorial will show you how to build and deploy an ASP.NET Core app to Microsoft Azure App Service using command line tools.</span></span> <span data-ttu-id="21e12-106">Po dokončení, budete mít stránky Razor, webové aplikace, které jsou součástí ASP.NET Core hostované jako webová aplikace Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="21e12-106">When finished, you'll have a Razor Pages web app built in ASP.NET Core hosted as an Azure App Service Web App.</span></span> <span data-ttu-id="21e12-107">V tomto kurzu je zapsána pomocí nástroje příkazového řádku Windows, ale můžete použít a Linux prostředí také systému macOS.</span><span class="sxs-lookup"><span data-stu-id="21e12-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>

<span data-ttu-id="21e12-108">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="21e12-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="21e12-109">Vytvoření webu k službě Azure App Service pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="21e12-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="21e12-110">Nasazení aplikace ASP.NET Core do Azure App Service pomocí nástroje příkazového řádku Git</span><span class="sxs-lookup"><span data-stu-id="21e12-110">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21e12-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="21e12-111">Prerequisites</span></span>

<span data-ttu-id="21e12-112">K dokončení tohoto kurzu, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="21e12-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="21e12-113">A [předplatné Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="21e12-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="21e12-114">[Git](https://www.git-scm.com/) klienta příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="21e12-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="21e12-115">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="21e12-115">Create a web app</span></span>

<span data-ttu-id="21e12-116">Vytvořte nový adresář pro webovou aplikaci, vytvořte novou aplikaci ASP.NET Core Razor stránky a poté spuštěn místně na webu.</span><span class="sxs-lookup"><span data-stu-id="21e12-116">Create a new directory for the web app, create a new ASP.NET Core Razor Pages app, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="21e12-117">Windows</span><span class="sxs-lookup"><span data-stu-id="21e12-117">Windows</span></span>](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

::: moniker-end

# <a name="othertabother"></a>[<span data-ttu-id="21e12-119">Jiné</span><span class="sxs-lookup"><span data-stu-id="21e12-119">Other</span></span>](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

::: moniker-end

---

![Výstup příkazového řádku](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="21e12-122">Testování aplikací procházením `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="21e12-122">Test the app by browsing to `http://localhost:5000`.</span></span>

![Web spuštěn místně](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="21e12-124">Vytvoření instance služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="21e12-124">Create the Azure App Service instance</span></span>

<span data-ttu-id="21e12-125">Pomocí [prostředí cloudu Azure](/azure/cloud-shell/quickstart), vytvořte skupinu prostředků, plán služby App Service a webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="21e12-125">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

<span data-ttu-id="21e12-126">Před nasazením nastavte přihlašovací údaje nasazení úrovni účtu, pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="21e12-126">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="21e12-127">Adresa URL nasazení je potřeba k nasazení aplikace pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="21e12-127">A deployment URL is needed to deploy the app using Git.</span></span> <span data-ttu-id="21e12-128">Získat adresu URL takto.</span><span class="sxs-lookup"><span data-stu-id="21e12-128">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

<span data-ttu-id="21e12-129">Poznámka: na uvedené adrese URL končící na `.git`.</span><span class="sxs-lookup"><span data-stu-id="21e12-129">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="21e12-130">Používá se v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="21e12-130">It's used in the next step.</span></span>

## <a name="deploy-the-app-using-git"></a><span data-ttu-id="21e12-131">Nasazení aplikace pomocí Git</span><span class="sxs-lookup"><span data-stu-id="21e12-131">Deploy the app using Git</span></span>

<span data-ttu-id="21e12-132">Jste připraveni k nasazení z místního počítače pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="21e12-132">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="21e12-133">Je bezpečné ignorovat všechna upozornění z Git o konce řádků.</span><span class="sxs-lookup"><span data-stu-id="21e12-133">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="21e12-134">Windows</span><span class="sxs-lookup"><span data-stu-id="21e12-134">Windows</span></span>](#tab/windows)

```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[<span data-ttu-id="21e12-135">Jiné</span><span class="sxs-lookup"><span data-stu-id="21e12-135">Other</span></span>](#tab/other)

```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```

---

<span data-ttu-id="21e12-136">Git vyzve k zadání přihlašovacích údajů k nasazení, které byly dříve nastavené.</span><span class="sxs-lookup"><span data-stu-id="21e12-136">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="21e12-137">Po ověření, aplikace bude se instaluje do vzdáleného umístění, vytvořené a nasazené.</span><span class="sxs-lookup"><span data-stu-id="21e12-137">After authenticating, the app will be pushed to the remote location, built, and deployed.</span></span>

![Výstup nasazení Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a><span data-ttu-id="21e12-139">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="21e12-139">Test the app</span></span>

<span data-ttu-id="21e12-140">Testování aplikací procházením `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="21e12-140">Test the app by browsing to `https://<web app name>.azurewebsites.net`.</span></span> <span data-ttu-id="21e12-141">Chcete-li zobrazit adresu v prostředí cloudu (nebo Azure CLI), použijte následující:</span><span class="sxs-lookup"><span data-stu-id="21e12-141">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Aplikace běžící v Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="21e12-143">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="21e12-143">Clean up</span></span>

<span data-ttu-id="21e12-144">Po dokončení testování aplikace a zkontrolujete kód a prostředky, odstraňte webovou aplikaci a plán odstraněním skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="21e12-144">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="21e12-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="21e12-145">Next steps</span></span>

<span data-ttu-id="21e12-146">V tomto kurzu jste se dozvěděli, jak:</span><span class="sxs-lookup"><span data-stu-id="21e12-146">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="21e12-147">Vytvoření webu k službě Azure App Service pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="21e12-147">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="21e12-148">Nasazení aplikace ASP.NET Core do Azure App Service pomocí nástroje příkazového řádku Git</span><span class="sxs-lookup"><span data-stu-id="21e12-148">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="21e12-149">Dále se naučíte, jak nasadit webovou aplikaci, která používá CosmosDB pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="21e12-149">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="21e12-150">Nasazení do Azure z příkazového řádku s .NET Core</span><span class="sxs-lookup"><span data-stu-id="21e12-150">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
