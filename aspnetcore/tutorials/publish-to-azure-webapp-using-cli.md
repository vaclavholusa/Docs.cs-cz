---
title: "Publikování aplikace ASP.NET Core do Azure pomocí pomocí nástroje příkazového řádku | Microsoft Docs"
description: "Zjistěte, jak publikovat aplikaci ASP.NET Core Azure App Service pomocí příkazového řádku klienta Git."
services: multiple
keywords: "ASP.NET Core, Azure, služby App Service, Git, příkazového řádku"
author: camsoper
ms.author: casoper
manager: wpickett
ms.date: 11/03/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
ms.custom: mvc
ms.devlang: dotnet
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 6af5de584cbf8cd59d86a965592b958061014c95
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a>Nasazení aplikace ASP.NET Core do služby Azure App Service z příkazového řádku

Podle [Soper kamera](https://twitter.com/camsoper)

Tento kurz vám ukáže, jak vytvářet a nasazovat aplikaci ASP.NET Core ke službě Microsoft Azure App Service pomocí nástrojů příkazového řádku.  Po dokončení, budete mít webovou aplikaci v ASP.NET MVC Core hostované jako webová aplikace Azure App Service.  V tomto kurzu je zapsána pomocí nástroje příkazového řádku Windows, ale můžete použít a Linux prostředí také systému macOS.  

V tomto kurzu zjistíte, jak:

> [!div class="checklist"]
> * Vytvoření webu k službě Azure App Service pomocí rozhraní příkazového řádku Azure
> * Nasazení aplikace ASP.NET Core Azure App Service pomocí nástroje příkazového řádku Git

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu, budete potřebovat:

* A [předplatné Microsoft Azure](https://azure.microsoft.com/free/)
* [.NET Core](https://www.microsoft.com/net/download/core)
* [Git](https://www.git-scm.com/) klienta příkazového řádku

## <a name="create-a-web-application"></a>Vytvoření webové aplikace

Vytvořte nový adresář pro webovou aplikaci, vytvořte novou aplikaci ASP.NET MVC jádra a poté spuštěn místně na webu.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[Jiné](#tab/other)
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

Testování aplikace procházením http://localhost: 5000.

![Web spuštěn místně](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a>Vytvoření instance služby Azure App Service

Pomocí [prostředí cloudu Azure](/azure/cloud-shell/quickstart), vytvořte skupinu prostředků, plán služby App Service a webové aplikace služby App Service.

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

Před nasazením nastavte přihlašovací údaje nasazení úrovni účtu, pomocí následujícího příkazu:

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

Adresa URL nasazení je potřeba k nasazení aplikace pomocí Git.  Získat adresu URL takto.

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
Poznámka: na uvedené adrese URL končící na `.git`. Používá se v dalším kroku.

## <a name="deploy-the-application-using-git"></a>Nasazení aplikace pomocí Git

Jste připraveni k nasazení z místního počítače pomocí Git.

> [!NOTE]
> Je bezpečné ignorovat všechna upozornění z Git o konce řádků.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
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

# <a name="othertabother"></a>[Jiné](#tab/other)
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

Git zobrazí výzvu pro přihlašovací údaje nasazení, které byly dříve nastavené.  Po ověření, aplikace bude se instaluje do vzdáleného umístění, vytvořené a nasazené.

![Výstup nasazení Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a>Testování aplikace

Testování aplikace procházením `https://<web app name>.azurewebsites.net`.  Chcete-li zobrazit adresu v prostředí cloudu (nebo Azure CLI), použijte následující:

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Aplikace běžící v Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a>Vyčištění

Po dokončení testování aplikace a zkontrolujete kód a prostředky, odstraňte webovou aplikaci a plán odstraněním skupiny prostředků.

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli, jak:

> [!div class="checklist"]
> * Vytvoření webu k službě Azure App Service pomocí rozhraní příkazového řádku Azure
> * Nasazení aplikace ASP.NET Core Azure App Service pomocí nástroje příkazového řádku Git

Dále se naučíte, jak nasadit webovou aplikaci, která používá CosmosDB pomocí příkazového řádku.

> [!div class="nextstepaction"]
> [Nasazení do Azure z příkazového řádku s .NET Core](/dotnet/azure/dotnet-quickstart-xplat)
