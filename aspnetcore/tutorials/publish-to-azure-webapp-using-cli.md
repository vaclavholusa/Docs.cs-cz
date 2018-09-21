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
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a>Publikování aplikace ASP.NET Core do Azure pomocí nástrojů příkazového řádku

Podle [kamera Soper](https://twitter.com/camsoper)

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

V tomto kurzu se seznámíte se způsobem sestavení a nasazení aplikace ASP.NET Core do služby Microsoft Azure App Service pomocí nástrojů příkazového řádku. Až budete hotovi, budete mít Razor Pages hostovaná webová aplikace vytvořená v ASP.NET Core jako webová aplikace služby Azure App Service. V tomto kurzu jsou zapsána pomocí nástroje příkazového řádku Windows, ale můžete použít pro macOS a Linux prostředí, stejně.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření webu s Azure App Service pomocí Azure CLI
> * Nasazení aplikace ASP.NET Core do Azure App Service pomocí nástroje příkazového řádku Git

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu, budete potřebovat:

* A [předplatné Microsoft Azure](https://azure.microsoft.com/free/)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Git](https://www.git-scm.com/) klienta příkazového řádku

## <a name="create-a-web-app"></a>Vytvoření webové aplikace

Vytvořte nový adresář pro webové aplikace, vytvořte novou aplikaci ASP.NET Core Razor Pages a poté spuštěn místně na webu.

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

Otestujte aplikaci tak, že přejdete do `http://localhost:5000`.

![Na webu spuštěná místně](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a>Vytvoření instance služby Azure App Service

Použití [Azure Cloud Shell](/azure/cloud-shell/quickstart), vytvořte skupinu prostředků, plán služby App Service a webovou aplikaci služby App Service.

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

Před nasazením nastavte přihlašovací údaje nasazení na úrovni účtu, pomocí následujícího příkazu:

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

Adresa URL nasazení je potřeba k nasazení aplikace pomocí Gitu. Načtěte adresu URL takto.

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

Všimněte si zobrazenou adresu URL s koncovkou `.git`. Používá se v dalším kroku.

## <a name="deploy-the-app-using-git"></a>Nasazení aplikace pomocí Gitu

Jste připravení nasadit z místního počítače pomocí Gitu.

> [!NOTE]
> Je bezpečné ignorovat upozornění z Gitu o koncích řádků klepněte.

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

Git zobrazí výzvu pro přihlašovací údaje nasazení, které byly dříve nastavené. Po ověření, aplikace bude vložena do vzdáleného umístění, sestaven a nasadit.

![Výstup nasazení Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a>Testování aplikace

Otestujte aplikaci tak, že přejdete do `https://<web app name>.azurewebsites.net`. Pro zobrazení adresy ve službě Cloud Shell (nebo Azure CLI), použijte následující:

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Aplikace běžící v Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a>Vyčistit

Po dokončení testování aplikace a zkontrolujete kód a prostředky, odstraňte webovou aplikaci a plán tak, že odstraníte skupinu prostředků.

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste zjistili, jak:

> [!div class="checklist"]
> * Vytvoření webu s Azure App Service pomocí Azure CLI
> * Nasazení aplikace ASP.NET Core do Azure App Service pomocí nástroje příkazového řádku Git

V dalším kroku zjistěte, jak pomocí příkazového řádku k nasazení webové aplikace, které používá služby cosmos DB.

> [!div class="nextstepaction"]
> [Nasazení do Azure z příkazového řádku s .NET Core](/dotnet/azure/dotnet-quickstart-xplat)
