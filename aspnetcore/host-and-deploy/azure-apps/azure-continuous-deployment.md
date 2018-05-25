---
title: Průběžné nasazování do Azure pomocí sady Visual Studio a Git s ASP.NET Core
author: rick-anderson
description: Naučte se vytvářet webové aplikace ASP.NET Core pomocí sady Visual Studio a nasadíte ho do Azure App Service pro průběžné nasazování pomocí Git.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 4de1893e8c1f7f2f4d9af7278a110067ea777c61
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="78d2a-103">Průběžné nasazování do Azure pomocí sady Visual Studio a Git s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78d2a-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="78d2a-104">Podle [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="78d2a-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="78d2a-105">Tento kurz ukazuje, jak webová aplikace ASP.NET Core pomocí sady Visual Studio vytvořte a nasaďte ho ze sady Visual Studio do služby Azure App Service pomocí průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="78d2a-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="78d2a-106">Viz také [služby VSTS použít při vytváření a publikování do webové aplikace Azure s průběžné nasazování](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), který ukazuje, jak nakonfigurovat nastavené průběžné doručování (CD) pracovního postupu pro [Azure App Service](/azure/app-service/app-service-web-overview) pomocí Visual Studio Team Služby.</span><span class="sxs-lookup"><span data-stu-id="78d2a-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="78d2a-107">Průběžné doručování Azure v Team Services zjednodušuje nastavení robustní nasazení kanálu publikování aktualizací pro aplikace, které jsou hostované v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="78d2a-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="78d2a-108">Kanál lze nakonfigurovat z portálu Azure k vytvoření, spuštění testů, nasazení na přípravný slot a pak nasadit do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="78d2a-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="78d2a-109">K dokončení tohoto kurzu, je nutné účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="78d2a-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="78d2a-110">Získat účet, [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) nebo [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="78d2a-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="78d2a-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="78d2a-111">Prerequisites</span></span>

<span data-ttu-id="78d2a-112">V tomto kurzu se předpokládá, že je nainstalován následující software:</span><span class="sxs-lookup"><span data-stu-id="78d2a-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="78d2a-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="78d2a-113">Visual Studio</span></span>](https://www.visualstudio.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="78d2a-114">[Git](https://git-scm.com/downloads) pro Windows</span><span class="sxs-lookup"><span data-stu-id="78d2a-114">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="78d2a-115">Vytvoření webové aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78d2a-115">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="78d2a-116">Spuštění sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78d2a-116">Start Visual Studio.</span></span>

1. <span data-ttu-id="78d2a-117">Z **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-117">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="78d2a-118">Vyberte **webové aplikace ASP.NET Core** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="78d2a-118">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="78d2a-119">Zobrazí se pod **nainstalovaná** > **šablony** > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-119">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="78d2a-120">Název projektu `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="78d2a-120">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="78d2a-121">Vyberte **vytvoření nového úložiště Git** možnost a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-121">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![Dialogové okno Nový projekt](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="78d2a-123">V **nový projekt ASP.NET Core** dialogovém okně, vyberte ASP.NET Core **prázdný** šablony, pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-123">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Dialogové okno Nový projekt ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="78d2a-125">Nejnovější verzi .NET Core je 2.0.</span><span class="sxs-lookup"><span data-stu-id="78d2a-125">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="78d2a-126">Místní spuštění webové aplikace</span><span class="sxs-lookup"><span data-stu-id="78d2a-126">Running the web app locally</span></span>

1. <span data-ttu-id="78d2a-127">Po dokončení vytváření aplikace Visual Studio spusťte aplikaci tak, že vyberete **ladění** > **spustit ladění**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-127">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="78d2a-128">Jako alternativu, stiskněte klávesu **F5**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-128">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="78d2a-129">To může trvat času k chybě při inicializaci sady Visual Studio a nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="78d2a-129">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="78d2a-130">Po jeho dokončení se zobrazí v prohlížeči spuštěné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="78d2a-130">Once it's complete, the browser shows the running app.</span></span>

   ![Zobrazení okna prohlížeče spuštění aplikace, která zobrazuje "Hello, World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="78d2a-132">Po zkontrolování funkční webovou aplikaci, zavřete prohlížeč a vyberte ikonu "Zastavte ladění" na panelu nástrojů sady Visual Studio k zastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="78d2a-132">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="78d2a-133">Vytvořit webovou aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="78d2a-133">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="78d2a-134">Následující postup vytvoření webové aplikace na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="78d2a-134">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="78d2a-135">Přihlaste se k [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="78d2a-135">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="78d2a-136">Vyberte **nový** v levém horním rohu rozhraní portálu.</span><span class="sxs-lookup"><span data-stu-id="78d2a-136">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="78d2a-137">Vyberte **Web + mobilní** > **webová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-137">Select **Web + Mobile** > **Web App**.</span></span>

   ![Portál Microsoft Azure: Tlačítko Nový: Web + mobilní v Marketplace: tlačítko webové aplikace v rámci vybrané aplikace](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="78d2a-139">V **webové aplikace** okno, zadejte jedinečnou hodnotu **název služby App Service**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-139">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Okně webové aplikace](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="78d2a-141">**Název služby App Service** název musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="78d2a-141">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="78d2a-142">Na portálu vynucuje toto pravidlo, pokud je zadaný název.</span><span class="sxs-lookup"><span data-stu-id="78d2a-142">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="78d2a-143">Pokud poskytnete jinou hodnotu, nahraďte tuto hodnotu pro každý výskyt **SampleWebAppDemo** v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="78d2a-143">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="78d2a-144">Také v **webové aplikace** okně Vybrat existující **umístění plán služby App** nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="78d2a-144">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="78d2a-145">Pokud vytváříte nový plán, vyberte cenovou úroveň, umístění a další možnosti.</span><span class="sxs-lookup"><span data-stu-id="78d2a-145">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="78d2a-146">Další informace o plánech služby App Service naleznete v tématu [podrobný přehled plánů služby Azure App Service](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="78d2a-146">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="78d2a-147">Vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-147">Select **Create**.</span></span> <span data-ttu-id="78d2a-148">Azure bude zřídit a spustí webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="78d2a-148">Azure will provision and start the web app.</span></span>

   ![Portálu Azure: Okno Essentials ukázkové webové aplikaci ukázku 01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="78d2a-150">Povolení publikování Git pro nové webové aplikace</span><span class="sxs-lookup"><span data-stu-id="78d2a-150">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="78d2a-151">Git je systém správy distribuovaných verzí, který slouží k nasazení webové aplikace služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="78d2a-151">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="78d2a-152">Kódu webové aplikace je uložen v místní úložiště Git a kód je nasadit do Azure nuceným doručením do vzdáleného úložiště.</span><span class="sxs-lookup"><span data-stu-id="78d2a-152">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="78d2a-153">Přihlaste se [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="78d2a-153">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="78d2a-154">Vyberte **App Services** zobrazení seznamu služeb aplikace přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="78d2a-154">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="78d2a-155">Vyberte webovou aplikaci vytvořili v předchozí části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="78d2a-155">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="78d2a-156">V **nasazení** vyberte **možnosti nasazení** > **zvolit zdroj** > **místní úložiště Git**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-156">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Okno nastavení: okno zdroj nasazení: Zvolte zdroj okno](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="78d2a-158">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-158">Select **OK**.</span></span>

1. <span data-ttu-id="78d2a-159">Pokud přihlašovací údaje nasazení pro publikování webové aplikace nebo jiná aplikace služby App Service nebyly dříve nastaveny, nastavte je nyní:</span><span class="sxs-lookup"><span data-stu-id="78d2a-159">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="78d2a-160">Vyberte **nastavení** > **přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-160">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="78d2a-161">**Nastavit přihlašovací údaje nasazení** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="78d2a-161">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="78d2a-162">Vytvořte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="78d2a-162">Create a user name and password.</span></span> <span data-ttu-id="78d2a-163">Uložte heslo pro pozdější použití při nastavování Git.</span><span class="sxs-lookup"><span data-stu-id="78d2a-163">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="78d2a-164">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-164">Select **Save**.</span></span>

1. <span data-ttu-id="78d2a-165">V **webové aplikace** vyberte **nastavení** > **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-165">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="78d2a-166">Adresu URL vzdáleného úložiště Git k nasazení se zobrazí v části **adresy URL pro GIT**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-166">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="78d2a-167">Kopírování **adresy URL pro GIT** hodnoty pro pozdější použití v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="78d2a-167">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Portálu Azure: okno vlastností aplikace](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="78d2a-169">Publikování webové aplikace do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="78d2a-169">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="78d2a-170">V této části vytvořte místní úložiště Git pomocí sady Visual Studio a posílejte nabízená oznámení z tohoto úložiště do Azure k nasazení webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="78d2a-170">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="78d2a-171">Kroky zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="78d2a-171">The steps involved include the following:</span></span>

* <span data-ttu-id="78d2a-172">Přidejte nastavení vzdáleného úložiště pomocí adresy URL pro GIT hodnoty, aby místní úložiště můžete nasadit do Azure.</span><span class="sxs-lookup"><span data-stu-id="78d2a-172">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="78d2a-173">Potvrďte změny projektu.</span><span class="sxs-lookup"><span data-stu-id="78d2a-173">Commit project changes.</span></span>
* <span data-ttu-id="78d2a-174">Doručte změny projektu z místního úložiště do vzdáleného úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="78d2a-174">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="78d2a-175">V **Průzkumníku řešení** klikněte pravým tlačítkem na **řešení 'SampleWebAppDemo'** a vyberte **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-175">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="78d2a-176">**Team Explorer** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="78d2a-176">The **Team Explorer** is displayed.</span></span>

   ![Karta Team Explorer se připojit](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="78d2a-178">V **Team Explorer**, vyberte **domácí** (Ikona Domovská stránka) > **nastavení** > **nastavení úložiště**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-178">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="78d2a-179">V **dálkové ovladače** části **nastavení úložiště**, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-179">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="78d2a-180">**Přidat vzdálené** se zobrazí dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="78d2a-180">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="78d2a-181">Nastavte **název** vzdáleného k **Azure SampleApp**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-181">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="78d2a-182">Nastavit hodnotu pro **načíst** k **adresy URL pro Git** , zkopírovat z Azure dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="78d2a-182">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="78d2a-183">Všimněte si, že toto je adresa URL, který končí **.git**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-183">Note that this is the URL that ends with **.git**.</span></span>

   ![Vzdálené dialogové okno Upravit](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="78d2a-185">Jako alternativu, zadejte vzdálené úložiště, ze kterého **příkazové okno** otevřením **příkazové okno**, změna k adresáři projektu a zadáte příkaz.</span><span class="sxs-lookup"><span data-stu-id="78d2a-185">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="78d2a-186">Příklad:</span><span class="sxs-lookup"><span data-stu-id="78d2a-186">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="78d2a-187">Vyberte **domácí** (Ikona Domovská stránka) > **nastavení** > **globální nastavení**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-187">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="78d2a-188">Zkontrolujte, jestli jsou nastavené jméno a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="78d2a-188">Confirm that the name and email address are set.</span></span> <span data-ttu-id="78d2a-189">Vyberte **aktualizace** v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="78d2a-189">Select **Update** if required.</span></span>

1. <span data-ttu-id="78d2a-190">Vyberte **Domů** > **změny** se vrátíte do **změny** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="78d2a-190">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="78d2a-191">Zadejte zprávu o potvrzení, jako například **počáteční Push č. 1** a vyberte **potvrzení**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-191">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="78d2a-192">Tato akce vytvoří *potvrzení* místně.</span><span class="sxs-lookup"><span data-stu-id="78d2a-192">This action creates a *commit* locally.</span></span>

   ![Karta Team Explorer se připojit](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="78d2a-194">Jako alternativu, potvrzení změny z **příkazové okno** otevřením **příkazové okno**, změna k adresáři projektu a zadáte příkazy gitu.</span><span class="sxs-lookup"><span data-stu-id="78d2a-194">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="78d2a-195">Příklad:</span><span class="sxs-lookup"><span data-stu-id="78d2a-195">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="78d2a-196">Vyberte **Domů** > **synchronizace** > **akce** > **otevřete příkazový řádek**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-196">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="78d2a-197">Do příkazového řádku se otevře adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="78d2a-197">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="78d2a-198">Do příkazového řádku zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="78d2a-198">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="78d2a-199">Zadejte Azure **přihlašovací údaje nasazení** heslo v Azure vytvořili.</span><span class="sxs-lookup"><span data-stu-id="78d2a-199">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="78d2a-200">Tento příkaz spustí proces vkládání souborů místní projektu do Azure.</span><span class="sxs-lookup"><span data-stu-id="78d2a-200">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="78d2a-201">Výstup z výše uvedeném příkazu končí zprávu, že nasazení bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="78d2a-201">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="78d2a-202">Pokud spolupráce na projekt se požaduje, zvažte, když zavedete [Githubu](https://github.com) před odesláním do Azure.</span><span class="sxs-lookup"><span data-stu-id="78d2a-202">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="78d2a-203">Ověření aktivní nasazení</span><span class="sxs-lookup"><span data-stu-id="78d2a-203">Verify the Active Deployment</span></span>

<span data-ttu-id="78d2a-204">Ověřte, jestli přenos webové aplikace z místního prostředí do Azure se úspěšně.</span><span class="sxs-lookup"><span data-stu-id="78d2a-204">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="78d2a-205">V [portálu Azure](https://portal.azure.com), vyberte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="78d2a-205">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="78d2a-206">Vyberte **nasazení** > **možnosti nasazení**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-206">Select **Deployment** > **Deployment options**.</span></span>

![Portálu Azure: Okno nastavení: nasazení okna zobrazující úspěšné nasazení](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="78d2a-208">Spusťte aplikaci v Azure</span><span class="sxs-lookup"><span data-stu-id="78d2a-208">Run the app in Azure</span></span>

<span data-ttu-id="78d2a-209">Teď, když webové aplikace je nasazená do Azure, spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="78d2a-209">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="78d2a-210">Můžete to provést dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="78d2a-210">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="78d2a-211">Na portálu Azure vyhledejte okně webové aplikace pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="78d2a-211">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="78d2a-212">Vyberte **Procházet** Chcete-li zobrazit aplikaci ve výchozím prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="78d2a-212">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="78d2a-213">Otevřete prohlížeč a zadejte adresu URL pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="78d2a-213">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="78d2a-214">Příklad: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="78d2a-214">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="78d2a-215">Aktualizovat webovou aplikaci a znovu publikovat</span><span class="sxs-lookup"><span data-stu-id="78d2a-215">Update the web app and republish</span></span>

<span data-ttu-id="78d2a-216">Po provedení změn pomocí místního kódu, znovu publikujte:</span><span class="sxs-lookup"><span data-stu-id="78d2a-216">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="78d2a-217">V **Průzkumníku řešení** sady Visual Studio, otevřete *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="78d2a-217">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="78d2a-218">V `Configure` metoda, upravte `Response.WriteAsync` metoda tak to vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="78d2a-218">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="78d2a-219">Uložit změny do *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="78d2a-219">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="78d2a-220">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **řešení 'SampleWebAppDemo'** a vyberte **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-220">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="78d2a-221">**Team Explorer** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="78d2a-221">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="78d2a-222">Zadejte zprávu o potvrzení, jako například `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="78d2a-222">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="78d2a-223">Stiskněte **potvrzení** tlačítko potvrzení změn projektu.</span><span class="sxs-lookup"><span data-stu-id="78d2a-223">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="78d2a-224">Vyberte **Domů** > **synchronizace** > **akce** > **Push**.</span><span class="sxs-lookup"><span data-stu-id="78d2a-224">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="78d2a-225">Jako alternativu, odešlete změny z **příkazové okno** otevřením **příkazové okno**, změna k adresáři projektu a zadáním příkazu git.</span><span class="sxs-lookup"><span data-stu-id="78d2a-225">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="78d2a-226">Příklad:</span><span class="sxs-lookup"><span data-stu-id="78d2a-226">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="78d2a-227">Zobrazení aktualizované webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="78d2a-227">View the updated web app in Azure</span></span>

<span data-ttu-id="78d2a-228">Zobrazení aktualizované webové aplikace tak, že vyberete **Procházet** v okně webové aplikace na portálu Azure nebo otevřením prohlížeče a zadávat adresu URL pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="78d2a-228">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="78d2a-229">Příklad: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="78d2a-229">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="78d2a-230">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="78d2a-230">Additional resources</span></span>

* [<span data-ttu-id="78d2a-231">Použití služby VSTS vytvářet a publikovat do webové aplikace Azure s průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="78d2a-231">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="78d2a-232">Kudu projektu</span><span class="sxs-lookup"><span data-stu-id="78d2a-232">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
