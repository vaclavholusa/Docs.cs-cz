---
title: Publikování aplikace ASP.NET Core do Azure pomocí nástroje příkazového řádku
author: camsoper
description: Zjistěte, jak publikovat aplikaci ASP.NET Core Azure App Service pomocí příkazového řádku klienta Git.
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 526ceef469d473706f39cdc3ee645280e99315b1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279243"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a>Publikování aplikace ASP.NET Core do Azure pomocí nástroje příkazového řádku

Podle [Soper kamera](https://twitter.com/camsoper)

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

Tento kurz vám ukáže, jak vytvářet a nasazovat aplikaci ASP.NET Core do Microsoft Azure App Service pomocí nástrojů příkazového řádku. Po dokončení, budete mít stránky Razor, webové aplikace, které jsou součástí ASP.NET Core hostované jako webová aplikace Azure App Service. V tomto kurzu je zapsána pomocí nástroje příkazového řádku Windows, ale můžete použít a Linux prostředí také systému macOS.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření webu k službě Azure App Service pomocí rozhraní příkazového řádku Azure
> * Nasazení aplikace ASP.NET Core do Azure App Service pomocí nástroje příkazového řádku Git

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu, budete potřebovat:

* A [předplatné Microsoft Azure](https://azure.microsoft.com/free/)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Git](https://www.git-scm.com/) klienta příkazového řádku

## <a name="create-a-web-app"></a>Vytvoření webové aplikace

Vytvořte nový adresář pro webovou aplikaci, vytvořte novou aplikaci ASP.NET Core Razor stránky a poté spuštěn místně na webu.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

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

# <a name="othertabother"></a>[Jiné](#tab/other)

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

Testování aplikací procházením `http://localhost:5000`.

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

Adresa URL nasazení je potřeba k nasazení aplikace pomocí Git. Získat adresu URL takto.

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

Poznámka: na uvedené adrese URL končící na `.git`. Používá se v dalším kroku.

## <a name="deploy-the-app-using-git"></a>Nasazení aplikace pomocí Git

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

Git vyzve k zadání přihlašovacích údajů k nasazení, které byly dříve nastavené. Po ověření, aplikace bude se instaluje do vzdáleného umístění, vytvořené a nasazené.

![Výstup nasazení Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a>Testování aplikace

Testování aplikací procházením `https://<web app name>.azurewebsites.net`. Chcete-li zobrazit adresu v prostředí cloudu (nebo Azure CLI), použijte následující:

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
> * Nasazení aplikace ASP.NET Core do Azure App Service pomocí nástroje příkazového řádku Git

Dále se naučíte, jak nasadit webovou aplikaci, která používá CosmosDB pomocí příkazového řádku.

> [!div class="nextstepaction"]
> [Nasazení do Azure z příkazového řádku s .NET Core](/dotnet/azure/dotnet-quickstart-xplat)
