---
title: Publikování aplikace ASP.NET Core do Azure pomocí nástrojů příkazového řádku
author: camsoper
description: Zjistěte, jak publikovat aplikace ASP.NET Core do Azure App Service pomocí klienta příkazového řádku Git.
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 63a313de786b1f89e84c594cbd665d1b230e4ba3
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523243"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="c2984-103">Publikování aplikace ASP.NET Core do Azure pomocí nástrojů příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="c2984-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="c2984-104">Podle [kamera Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="c2984-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="c2984-105">V tomto kurzu se seznámíte se způsobem sestavení a nasazení aplikace ASP.NET Core do služby Microsoft Azure App Service pomocí nástrojů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="c2984-105">This tutorial will show you how to build and deploy an ASP.NET Core app to Microsoft Azure App Service using command line tools.</span></span> <span data-ttu-id="c2984-106">Až budete hotovi, budete mít Razor Pages hostovaná webová aplikace vytvořená v ASP.NET Core jako webová aplikace služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c2984-106">When finished, you'll have a Razor Pages web app built in ASP.NET Core hosted as an Azure App Service Web App.</span></span> <span data-ttu-id="c2984-107">V tomto kurzu jsou zapsána pomocí nástroje příkazového řádku Windows, ale můžete použít pro macOS a Linux prostředí, stejně.</span><span class="sxs-lookup"><span data-stu-id="c2984-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>

<span data-ttu-id="c2984-108">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="c2984-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c2984-109">Vytvoření webu s Azure App Service pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c2984-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="c2984-110">Nasazení aplikace ASP.NET Core do Azure App Service pomocí nástroje příkazového řádku Git</span><span class="sxs-lookup"><span data-stu-id="c2984-110">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2984-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c2984-111">Prerequisites</span></span>

<span data-ttu-id="c2984-112">K dokončení tohoto kurzu, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="c2984-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="c2984-113">A [předplatné Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="c2984-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="c2984-114">[Git](https://www.git-scm.com/) klienta příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="c2984-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="c2984-115">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c2984-115">Create a web app</span></span>

<span data-ttu-id="c2984-116">Vytvořte nový adresář pro webové aplikace, vytvořte novou aplikaci ASP.NET Core Razor Pages a poté spuštěn místně na webu.</span><span class="sxs-lookup"><span data-stu-id="c2984-116">Create a new directory for the web app, create a new ASP.NET Core Razor Pages app, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="c2984-117">Windows</span><span class="sxs-lookup"><span data-stu-id="c2984-117">Windows</span></span>](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

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

# <a name="othertabother"></a>[<span data-ttu-id="c2984-118">Jiné</span><span class="sxs-lookup"><span data-stu-id="c2984-118">Other</span></span>](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

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

<span data-ttu-id="c2984-120">Otestujte aplikaci tak, že přejdete do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c2984-120">Test the app by browsing to `http://localhost:5000`.</span></span>

![Na webu spuštěná místně](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="c2984-122">Vytvoření instance služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c2984-122">Create the Azure App Service instance</span></span>

<span data-ttu-id="c2984-123">Použití [Azure Cloud Shell](/azure/cloud-shell/quickstart), vytvořte skupinu prostředků, plán služby App Service a webovou aplikaci služby App Service.</span><span class="sxs-lookup"><span data-stu-id="c2984-123">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="c2984-124">Před nasazením nastavte přihlašovací údaje nasazení na úrovni účtu, pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="c2984-124">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="c2984-125">Adresa URL nasazení je potřeba k nasazení aplikace pomocí Gitu.</span><span class="sxs-lookup"><span data-stu-id="c2984-125">A deployment URL is needed to deploy the app using Git.</span></span> <span data-ttu-id="c2984-126">Načtěte adresu URL takto.</span><span class="sxs-lookup"><span data-stu-id="c2984-126">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

<span data-ttu-id="c2984-127">Všimněte si zobrazenou adresu URL s koncovkou `.git`.</span><span class="sxs-lookup"><span data-stu-id="c2984-127">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="c2984-128">Používá se v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="c2984-128">It's used in the next step.</span></span>

## <a name="deploy-the-app-using-git"></a><span data-ttu-id="c2984-129">Nasazení aplikace pomocí Gitu</span><span class="sxs-lookup"><span data-stu-id="c2984-129">Deploy the app using Git</span></span>

<span data-ttu-id="c2984-130">Jste připravení nasadit z místního počítače pomocí Gitu.</span><span class="sxs-lookup"><span data-stu-id="c2984-130">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="c2984-131">Je bezpečné ignorovat upozornění z Gitu o koncích řádků klepněte.</span><span class="sxs-lookup"><span data-stu-id="c2984-131">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="c2984-132">Windows</span><span class="sxs-lookup"><span data-stu-id="c2984-132">Windows</span></span>](#tab/windows)

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

# <a name="othertabother"></a>[<span data-ttu-id="c2984-133">Jiné</span><span class="sxs-lookup"><span data-stu-id="c2984-133">Other</span></span>](#tab/other)

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

<span data-ttu-id="c2984-134">Git zobrazí výzvu pro přihlašovací údaje nasazení, které byly dříve nastavené.</span><span class="sxs-lookup"><span data-stu-id="c2984-134">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="c2984-135">Po ověření, aplikace bude vložena do vzdáleného umístění, sestaven a nasadit.</span><span class="sxs-lookup"><span data-stu-id="c2984-135">After authenticating, the app will be pushed to the remote location, built, and deployed.</span></span>

![Výstup nasazení Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a><span data-ttu-id="c2984-137">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="c2984-137">Test the app</span></span>

<span data-ttu-id="c2984-138">Otestujte aplikaci tak, že přejdete do `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="c2984-138">Test the app by browsing to `https://<web app name>.azurewebsites.net`.</span></span> <span data-ttu-id="c2984-139">Pro zobrazení adresy ve službě Cloud Shell (nebo Azure CLI), použijte následující:</span><span class="sxs-lookup"><span data-stu-id="c2984-139">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Aplikace běžící v Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="c2984-141">Vyčistit</span><span class="sxs-lookup"><span data-stu-id="c2984-141">Clean up</span></span>

<span data-ttu-id="c2984-142">Po dokončení testování aplikace a zkontrolujete kód a prostředky, odstraňte webovou aplikaci a plán tak, že odstraníte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c2984-142">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="c2984-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c2984-143">Next steps</span></span>

<span data-ttu-id="c2984-144">V tomto kurzu jste zjistili, jak:</span><span class="sxs-lookup"><span data-stu-id="c2984-144">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c2984-145">Vytvoření webu s Azure App Service pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c2984-145">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="c2984-146">Nasazení aplikace ASP.NET Core do Azure App Service pomocí nástroje příkazového řádku Git</span><span class="sxs-lookup"><span data-stu-id="c2984-146">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="c2984-147">V dalším kroku zjistěte, jak pomocí příkazového řádku k nasazení webové aplikace, které používá služby cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c2984-147">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c2984-148">Nasazení do Azure z příkazového řádku s .NET Core</span><span class="sxs-lookup"><span data-stu-id="c2984-148">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
