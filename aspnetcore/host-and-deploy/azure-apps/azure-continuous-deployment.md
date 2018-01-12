---
title: "Průběžné nasazování do Azure pomocí sady Visual Studio a Git"
author: rick-anderson
description: "Naučte se vytvářet webové aplikace ASP.NET Core pomocí sady Visual Studio a nasadíte ho do Azure App Service pro průběžné nasazování pomocí Git."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: c1d25b109bbf211eb476860ff77b649565960b62
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="01129-103">Průběžné nasazování do Azure pro ASP.NET Core sadou Visual Studio a Git</span><span class="sxs-lookup"><span data-stu-id="01129-103">Continuous deployment to Azure for ASP.NET Core with Visual Studio and Git</span></span>

<span data-ttu-id="01129-104">Podle [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="01129-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="01129-105">Tento kurz ukazuje, jak webová aplikace ASP.NET Core pomocí sady Visual Studio vytvořte a nasaďte ho ze sady Visual Studio do služby Azure App Service pomocí průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="01129-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="01129-106">Viz také [služby VSTS použít při vytváření a publikování do webové aplikace Azure s průběžné nasazování](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), který ukazuje, jak nakonfigurovat nastavené průběžné doručování (CD) pracovního postupu pro [Azure App Service](/azure/app-service/app-service-web-overview) pomocí Visual Studio Team Služby.</span><span class="sxs-lookup"><span data-stu-id="01129-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="01129-107">Průběžné doručování Azure v Team Services zjednodušuje nastavení robustní nasazení kanálu publikování aktualizací pro aplikace, které jsou hostované v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="01129-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="01129-108">Kanál lze nakonfigurovat z portálu Azure k vytvoření, spuštění testů, nasazení na přípravný slot a pak nasadit do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="01129-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="01129-109">K dokončení tohoto kurzu, je nutné účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="01129-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="01129-110">Získat účet, [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) nebo [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="01129-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01129-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01129-111">Prerequisites</span></span>

<span data-ttu-id="01129-112">V tomto kurzu se předpokládá, že je nainstalován následující software:</span><span class="sxs-lookup"><span data-stu-id="01129-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="01129-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01129-113">Visual Studio</span></span>](https://www.visualstudio.com)
* <span data-ttu-id="01129-114">[.NET core SDK](https://www.microsoft.com/net/download/core) (runtime a nástrojů)</span><span class="sxs-lookup"><span data-stu-id="01129-114">[.NET Core SDK](https://www.microsoft.com/net/download/core) (runtime and tooling)</span></span>
* <span data-ttu-id="01129-115">[Git](https://git-scm.com/downloads) pro Windows</span><span class="sxs-lookup"><span data-stu-id="01129-115">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="01129-116">Vytvoření webové aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01129-116">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="01129-117">Spuštění sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01129-117">Start Visual Studio.</span></span>

1. <span data-ttu-id="01129-118">Z **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="01129-118">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="01129-119">Vyberte **webové aplikace ASP.NET Core** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="01129-119">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="01129-120">Zobrazí se pod **nainstalovaná** > **šablony** > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="01129-120">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="01129-121">Název projektu `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="01129-121">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="01129-122">Vyberte **vytvoření nového úložiště Git** možnost a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="01129-122">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![Dialogové okno Nový projekt](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="01129-124">V **nový projekt ASP.NET Core** dialogovém okně, vyberte ASP.NET Core **prázdný** šablony, pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="01129-124">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Dialogové okno Nový projekt ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="01129-126">Nejnovější verzi .NET Core je 2.0.</span><span class="sxs-lookup"><span data-stu-id="01129-126">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="01129-127">Místní spuštění webové aplikace</span><span class="sxs-lookup"><span data-stu-id="01129-127">Running the web app locally</span></span>

1. <span data-ttu-id="01129-128">Po dokončení vytváření aplikace Visual Studio spusťte aplikaci tak, že vyberete **ladění** > **spustit ladění**.</span><span class="sxs-lookup"><span data-stu-id="01129-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="01129-129">Jako alternativu, stiskněte klávesu **F5**.</span><span class="sxs-lookup"><span data-stu-id="01129-129">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="01129-130">To může trvat času k chybě při inicializaci sady Visual Studio a nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="01129-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="01129-131">Po jeho dokončení se zobrazí v prohlížeči spuštěné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01129-131">Once it's complete, the browser shows the running app.</span></span>

   ![Zobrazení okna prohlížeče spuštění aplikace, která zobrazuje "Hello, World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="01129-133">Po zkontrolování funkční webovou aplikaci, zavřete prohlížeč a vyberte ikonu "Zastavte ladění" na panelu nástrojů sady Visual Studio k zastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="01129-133">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="01129-134">Vytvořit webovou aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="01129-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="01129-135">Následující postup vytvoření webové aplikace na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="01129-135">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="01129-136">Přihlaste se k [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01129-136">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="01129-137">Vyberte **nový** v levém horním rohu rozhraní portálu.</span><span class="sxs-lookup"><span data-stu-id="01129-137">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="01129-138">Vyberte **Web + mobilní** > **webová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="01129-138">Select **Web + Mobile** > **Web App**.</span></span>

   ![Portál Microsoft Azure: Tlačítko Nový: Web + mobilní v Marketplace: tlačítko webové aplikace v rámci vybrané aplikace](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="01129-140">V **webové aplikace** okno, zadejte jedinečnou hodnotu **název služby App Service**.</span><span class="sxs-lookup"><span data-stu-id="01129-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Okně webové aplikace](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="01129-142">**Název služby App Service** název musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="01129-142">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="01129-143">Na portálu vynucuje toto pravidlo, pokud je zadaný název.</span><span class="sxs-lookup"><span data-stu-id="01129-143">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="01129-144">Pokud poskytnete jinou hodnotu, nahraďte tuto hodnotu pro každý výskyt **SampleWebAppDemo** v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="01129-144">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="01129-145">Také v **webové aplikace** okně Vybrat existující **umístění plán služby App** nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="01129-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="01129-146">Pokud vytváříte nový plán, vyberte cenovou úroveň, umístění a další možnosti.</span><span class="sxs-lookup"><span data-stu-id="01129-146">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="01129-147">Další informace o plánech služby App Service naleznete v tématu [podrobný přehled plánů služby Azure App Service](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="01129-147">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="01129-148">Vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01129-148">Select **Create**.</span></span> <span data-ttu-id="01129-149">Azure bude zřídit a spustí webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01129-149">Azure will provision and start the web app.</span></span>

   ![Portálu Azure: Okno Essentials ukázkové webové aplikaci ukázku 01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="01129-151">Povolení publikování Git pro nové webové aplikace</span><span class="sxs-lookup"><span data-stu-id="01129-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="01129-152">Git je systém správy distribuovaných verzí, který slouží k nasazení webové aplikace služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="01129-152">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="01129-153">Kódu webové aplikace je uložen v místní úložiště Git a kód je nasadit do Azure nuceným doručením do vzdáleného úložiště.</span><span class="sxs-lookup"><span data-stu-id="01129-153">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="01129-154">Přihlaste se [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01129-154">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="01129-155">Vyberte **App Services** zobrazení seznamu služeb aplikace přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="01129-155">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="01129-156">Vyberte webovou aplikaci vytvořili v předchozí části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="01129-156">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="01129-157">V **nasazení** vyberte **možnosti nasazení** > **zvolit zdroj** > **místní úložiště Git**.</span><span class="sxs-lookup"><span data-stu-id="01129-157">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Okno nastavení: okno zdroj nasazení: Zvolte zdroj okno](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="01129-159">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="01129-159">Select **OK**.</span></span>

1. <span data-ttu-id="01129-160">Pokud přihlašovací údaje nasazení pro publikování webové aplikace nebo jiná aplikace služby App Service nebyly dříve nastaveny, nastavte je nyní:</span><span class="sxs-lookup"><span data-stu-id="01129-160">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="01129-161">Vyberte **nastavení** > **přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="01129-161">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="01129-162">**Nastavit přihlašovací údaje nasazení** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="01129-162">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="01129-163">Vytvořte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="01129-163">Create a user name and password.</span></span> <span data-ttu-id="01129-164">Uložte heslo pro pozdější použití při nastavování Git.</span><span class="sxs-lookup"><span data-stu-id="01129-164">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="01129-165">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="01129-165">Select **Save**.</span></span>

1. <span data-ttu-id="01129-166">V **webové aplikace** vyberte **nastavení** > **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="01129-166">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="01129-167">Adresu URL vzdáleného úložiště Git k nasazení se zobrazí v části **adresy URL pro GIT**.</span><span class="sxs-lookup"><span data-stu-id="01129-167">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="01129-168">Kopírování **adresy URL pro GIT** hodnoty pro pozdější použití v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="01129-168">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Portálu Azure: okno vlastností aplikace](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="01129-170">Publikování webové aplikace do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="01129-170">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="01129-171">V této části vytvořte místní úložiště Git pomocí sady Visual Studio a posílejte nabízená oznámení z tohoto úložiště do Azure k nasazení webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="01129-171">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="01129-172">Kroky zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="01129-172">The steps involved include the following:</span></span>

* <span data-ttu-id="01129-173">Přidejte nastavení vzdáleného úložiště pomocí adresy URL pro GIT hodnoty, aby místní úložiště můžete nasadit do Azure.</span><span class="sxs-lookup"><span data-stu-id="01129-173">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="01129-174">Potvrďte změny projektu.</span><span class="sxs-lookup"><span data-stu-id="01129-174">Commit project changes.</span></span>
* <span data-ttu-id="01129-175">Doručte změny projektu z místního úložiště do vzdáleného úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="01129-175">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="01129-176">V **Průzkumníku řešení** klikněte pravým tlačítkem na **řešení 'SampleWebAppDemo'** a vyberte **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="01129-176">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="01129-177">**Team Explorer** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="01129-177">The **Team Explorer** is displayed.</span></span>

   ![Karta Team Explorer se připojit](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="01129-179">V **Team Explorer**, vyberte **domácí** (Ikona Domovská stránka) > **nastavení** > **nastavení úložiště**.</span><span class="sxs-lookup"><span data-stu-id="01129-179">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="01129-180">V **dálkové ovladače** části **nastavení úložiště**, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="01129-180">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="01129-181">**Přidat vzdálené** se zobrazí dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01129-181">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="01129-182">Nastavte **název** vzdáleného k **Azure SampleApp**.</span><span class="sxs-lookup"><span data-stu-id="01129-182">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="01129-183">Nastavit hodnotu pro **načíst** k **adresy URL pro Git** , zkopírovat z Azure dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="01129-183">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="01129-184">Všimněte si, že toto je adresa URL, který končí **.git**.</span><span class="sxs-lookup"><span data-stu-id="01129-184">Note that this is the URL that ends with **.git**.</span></span>

   ![Vzdálené dialogové okno Upravit](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="01129-186">Jako alternativu, zadejte vzdálené úložiště, ze kterého **příkazové okno** otevřením **příkazové okno**, změna k adresáři projektu a zadáte příkaz.</span><span class="sxs-lookup"><span data-stu-id="01129-186">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="01129-187">Příklad:</span><span class="sxs-lookup"><span data-stu-id="01129-187">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="01129-188">Vyberte **domácí** (Ikona Domovská stránka) > **nastavení** > **globální nastavení**.</span><span class="sxs-lookup"><span data-stu-id="01129-188">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="01129-189">Zkontrolujte, jestli jsou nastavené jméno a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="01129-189">Confirm that the name and email address are set.</span></span> <span data-ttu-id="01129-190">Vyberte **aktualizace** v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="01129-190">Select **Update** if required.</span></span>

1. <span data-ttu-id="01129-191">Vyberte **Domů** > **změny** se vrátíte do **změny** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="01129-191">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="01129-192">Zadejte zprávu o potvrzení, jako například **počáteční Push č. 1** a vyberte **potvrzení**.</span><span class="sxs-lookup"><span data-stu-id="01129-192">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="01129-193">Tato akce vytvoří *potvrzení* místně.</span><span class="sxs-lookup"><span data-stu-id="01129-193">This action creates a *commit* locally.</span></span>

   ![Karta Team Explorer se připojit](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="01129-195">Jako alternativu, potvrzení změny z **příkazové okno** otevřením **příkazové okno**, změna k adresáři projektu a zadáte příkazy gitu.</span><span class="sxs-lookup"><span data-stu-id="01129-195">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="01129-196">Příklad:</span><span class="sxs-lookup"><span data-stu-id="01129-196">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="01129-197">Vyberte **Domů** > **synchronizace** > **akce** > **otevřete příkazový řádek**.</span><span class="sxs-lookup"><span data-stu-id="01129-197">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="01129-198">Do příkazového řádku se otevře adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="01129-198">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="01129-199">Do příkazového řádku zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="01129-199">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="01129-200">Zadejte Azure **přihlašovací údaje nasazení** heslo v Azure vytvořili.</span><span class="sxs-lookup"><span data-stu-id="01129-200">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="01129-201">Tento příkaz spustí proces vkládání souborů místní projektu do Azure.</span><span class="sxs-lookup"><span data-stu-id="01129-201">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="01129-202">Výstup z výše uvedeném příkazu končí zprávu, že nasazení bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="01129-202">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="01129-203">Pokud spolupráce na projekt se požaduje, zvažte, když zavedete [Githubu](https://github.com) před odesláním do Azure.</span><span class="sxs-lookup"><span data-stu-id="01129-203">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="01129-204">Ověření aktivní nasazení</span><span class="sxs-lookup"><span data-stu-id="01129-204">Verify the Active Deployment</span></span>

<span data-ttu-id="01129-205">Ověřte, jestli přenos webové aplikace z místního prostředí do Azure se úspěšně.</span><span class="sxs-lookup"><span data-stu-id="01129-205">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="01129-206">V [portálu Azure](https://portal.azure.com), vyberte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01129-206">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="01129-207">Vyberte **nasazení** > **možnosti nasazení**.</span><span class="sxs-lookup"><span data-stu-id="01129-207">Select **Deployment** > **Deployment options**.</span></span>

![Portálu Azure: Okno nastavení: nasazení okna zobrazující úspěšné nasazení](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="01129-209">Spusťte aplikaci v Azure</span><span class="sxs-lookup"><span data-stu-id="01129-209">Run the app in Azure</span></span>

<span data-ttu-id="01129-210">Teď, když webové aplikace je nasazená do Azure, spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01129-210">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="01129-211">Můžete to provést dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="01129-211">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="01129-212">Na portálu Azure vyhledejte okně webové aplikace pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01129-212">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="01129-213">Vyberte **Procházet** Chcete-li zobrazit aplikaci ve výchozím prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="01129-213">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="01129-214">Otevřete prohlížeč a zadejte adresu URL pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01129-214">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="01129-215">Příklad:`http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="01129-215">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="01129-216">Aktualizovat webovou aplikaci a znovu publikovat</span><span class="sxs-lookup"><span data-stu-id="01129-216">Update the web app and republish</span></span>

<span data-ttu-id="01129-217">Po provedení změn pomocí místního kódu, znovu publikujte:</span><span class="sxs-lookup"><span data-stu-id="01129-217">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="01129-218">V **Průzkumníku řešení** sady Visual Studio, otevřete *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="01129-218">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="01129-219">V `Configure` metoda, upravte `Response.WriteAsync` metoda tak to vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="01129-219">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="01129-220">Uložit změny do *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="01129-220">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="01129-221">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **řešení 'SampleWebAppDemo'** a vyberte **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="01129-221">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="01129-222">**Team Explorer** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="01129-222">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="01129-223">Zadejte zprávu o potvrzení, jako například `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="01129-223">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="01129-224">Stiskněte **potvrzení** tlačítko potvrzení změn projektu.</span><span class="sxs-lookup"><span data-stu-id="01129-224">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="01129-225">Vyberte **Domů** > **synchronizace** > **akce** > **Push**.</span><span class="sxs-lookup"><span data-stu-id="01129-225">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="01129-226">Jako alternativu, odešlete změny z **příkazové okno** otevřením **příkazové okno**, změna k adresáři projektu a zadáním příkazu git.</span><span class="sxs-lookup"><span data-stu-id="01129-226">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="01129-227">Příklad:</span><span class="sxs-lookup"><span data-stu-id="01129-227">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="01129-228">Zobrazení aktualizované webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="01129-228">View the updated web app in Azure</span></span>

<span data-ttu-id="01129-229">Zobrazení aktualizované webové aplikace tak, že vyberete **Procházet** v okně webové aplikace na portálu Azure nebo otevřením prohlížeče a zadávat adresu URL pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01129-229">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="01129-230">Příklad:`http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="01129-230">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01129-231">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="01129-231">Additional resources</span></span>

* [<span data-ttu-id="01129-232">Použití služby VSTS vytvářet a publikovat do webové aplikace Azure s průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="01129-232">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="01129-233">Kudu projektu</span><span class="sxs-lookup"><span data-stu-id="01129-233">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
