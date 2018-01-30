---
title: "Publikování aplikace ASP.NET Core do Azure pomocí pomocí nástroje příkazového řádku"
author: camsoper
description: "Zjistěte, jak publikovat aplikaci ASP.NET Core Azure App Service pomocí příkazového řádku klienta Git."
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
ms.openlocfilehash: 0418a2695d3afb6dc2c55b8f694a97d62239835f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a><span data-ttu-id="9ba52-103">Nasazení aplikace ASP.NET Core do služby Azure App Service z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="9ba52-103">Deploy an ASP.NET Core application to Azure App Service from the command line</span></span>

<span data-ttu-id="9ba52-104">Podle [Soper kamera](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="9ba52-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="9ba52-105">Tento kurz vám ukáže, jak vytvářet a nasazovat aplikaci ASP.NET Core ke službě Microsoft Azure App Service pomocí nástrojů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9ba52-105">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="9ba52-106">Po dokončení, budete mít webovou aplikaci v ASP.NET MVC Core hostované jako webová aplikace Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="9ba52-106">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="9ba52-107">V tomto kurzu je zapsána pomocí nástroje příkazového řádku Windows, ale můžete použít a Linux prostředí také systému macOS.</span><span class="sxs-lookup"><span data-stu-id="9ba52-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="9ba52-108">V tomto kurzu zjistíte, jak:</span><span class="sxs-lookup"><span data-stu-id="9ba52-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9ba52-109">Vytvoření webu k službě Azure App Service pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="9ba52-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="9ba52-110">Nasazení aplikace ASP.NET Core Azure App Service pomocí nástroje příkazového řádku Git</span><span class="sxs-lookup"><span data-stu-id="9ba52-110">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ba52-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9ba52-111">Prerequisites</span></span>

<span data-ttu-id="9ba52-112">K dokončení tohoto kurzu, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="9ba52-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="9ba52-113">A [předplatné Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="9ba52-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [<span data-ttu-id="9ba52-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="9ba52-114">.NET Core</span></span>](https://www.microsoft.com/net/download/core)
* <span data-ttu-id="9ba52-115">[Git](https://www.git-scm.com/) klienta příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="9ba52-115">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="9ba52-116">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="9ba52-116">Create a web application</span></span>

<span data-ttu-id="9ba52-117">Vytvořte nový adresář pro webovou aplikaci, vytvořte novou aplikaci ASP.NET MVC jádra a poté spuštěn místně na webu.</span><span class="sxs-lookup"><span data-stu-id="9ba52-117">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="9ba52-118">Windows</span><span class="sxs-lookup"><span data-stu-id="9ba52-118">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="9ba52-119">Jiné</span><span class="sxs-lookup"><span data-stu-id="9ba52-119">Other</span></span>](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![Výstup příkazového řádku](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="9ba52-121">Testování aplikace procházením http://localhost: 5000.</span><span class="sxs-lookup"><span data-stu-id="9ba52-121">Test the application by browsing to http://localhost:5000.</span></span>

![Web spuštěn místně](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="9ba52-123">Vytvoření instance služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9ba52-123">Create the Azure App Service instance</span></span>

<span data-ttu-id="9ba52-124">Pomocí [prostředí cloudu Azure](/azure/cloud-shell/quickstart), vytvořte skupinu prostředků, plán služby App Service a webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="9ba52-124">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="9ba52-125">Před nasazením nastavte přihlašovací údaje nasazení úrovni účtu, pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="9ba52-125">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="9ba52-126">Adresa URL nasazení je potřeba k nasazení aplikace pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="9ba52-126">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="9ba52-127">Získat adresu URL takto.</span><span class="sxs-lookup"><span data-stu-id="9ba52-127">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="9ba52-128">Poznámka: na uvedené adrese URL končící na `.git`.</span><span class="sxs-lookup"><span data-stu-id="9ba52-128">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="9ba52-129">Používá se v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="9ba52-129">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="9ba52-130">Nasazení aplikace pomocí Git</span><span class="sxs-lookup"><span data-stu-id="9ba52-130">Deploy the application using Git</span></span>

<span data-ttu-id="9ba52-131">Jste připraveni k nasazení z místního počítače pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="9ba52-131">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="9ba52-132">Je bezpečné ignorovat všechna upozornění z Git o konce řádků.</span><span class="sxs-lookup"><span data-stu-id="9ba52-132">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="9ba52-133">Windows</span><span class="sxs-lookup"><span data-stu-id="9ba52-133">Windows</span></span>](#tab/windows)
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

# <a name="othertabother"></a>[<span data-ttu-id="9ba52-134">Jiné</span><span class="sxs-lookup"><span data-stu-id="9ba52-134">Other</span></span>](#tab/other)
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

<span data-ttu-id="9ba52-135">Git vyzve k zadání přihlašovacích údajů k nasazení, které byly dříve nastavené.</span><span class="sxs-lookup"><span data-stu-id="9ba52-135">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="9ba52-136">Po ověření, aplikace bude se instaluje do vzdáleného umístění, vytvořené a nasazené.</span><span class="sxs-lookup"><span data-stu-id="9ba52-136">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Výstup nasazení Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="9ba52-138">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="9ba52-138">Test the application</span></span>

<span data-ttu-id="9ba52-139">Testování aplikace procházením `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="9ba52-139">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="9ba52-140">Chcete-li zobrazit adresu v prostředí cloudu (nebo Azure CLI), použijte následující:</span><span class="sxs-lookup"><span data-stu-id="9ba52-140">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Aplikace běžící v Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="9ba52-142">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="9ba52-142">Clean up</span></span>

<span data-ttu-id="9ba52-143">Po dokončení testování aplikace a zkontrolujete kód a prostředky, odstraňte webovou aplikaci a plán odstraněním skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="9ba52-143">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="9ba52-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ba52-144">Next steps</span></span>

<span data-ttu-id="9ba52-145">V tomto kurzu jste se dozvěděli, jak:</span><span class="sxs-lookup"><span data-stu-id="9ba52-145">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9ba52-146">Vytvoření webu k službě Azure App Service pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="9ba52-146">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="9ba52-147">Nasazení aplikace ASP.NET Core Azure App Service pomocí nástroje příkazového řádku Git</span><span class="sxs-lookup"><span data-stu-id="9ba52-147">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="9ba52-148">Dále se naučíte, jak nasadit webovou aplikaci, která používá CosmosDB pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9ba52-148">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9ba52-149">Nasazení do Azure z příkazového řádku s .NET Core</span><span class="sxs-lookup"><span data-stu-id="9ba52-149">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
