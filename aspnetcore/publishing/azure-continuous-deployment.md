---
title: "Průběžné nasazování do Azure pomocí sady Visual Studio a Git"
author: rick-anderson
description: "Naučte se vytvářet webové aplikace ASP.NET Core pomocí sady Visual Studio a nasadíte ho do služby Azure App Service pro průběžné nasazování pomocí Git."
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: a9efad38b1c75bd3a186b4ec85861357ecf744b9
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="04e39-104">Průběžné nasazování do Azure pro ASP.NET Core, sadou Visual Studio a Git</span><span class="sxs-lookup"><span data-stu-id="04e39-104">Continuous deployment to Azure for ASP.NET Core, with Visual Studio and Git</span></span>

<span data-ttu-id="04e39-105">Podle [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="04e39-105">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="04e39-106">Tento kurz ukazuje, jak chcete vytvořit webové aplikace ASP.NET Core pomocí sady Visual Studio a nasaďte ho ze sady Visual Studio do služby Azure App Service pomocí průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="04e39-106">This tutorial shows you how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span> 

<span data-ttu-id="04e39-107">Viz také [služby VSTS použít při vytváření a publikování do webové aplikace Azure s průběžné nasazování](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), který ukazuje, jak nakonfigurovat nastavené průběžné doručování (CD) pracovního postupu pro [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) pomocí Visual Studio Team Služby.</span><span class="sxs-lookup"><span data-stu-id="04e39-107">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) using Visual Studio Team Services.</span></span> <span data-ttu-id="04e39-108">Průběžné doručování Azure v Team Services zjednodušuje nastavení robustní nasazení kanálu publikování aktualizací pro vaši aplikaci do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="04e39-108">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for your app to Azure App Service.</span></span> <span data-ttu-id="04e39-109">Kanál lze nakonfigurovat z portálu Azure k vytvoření, spuštění testů, nasazení na přípravný slot a pak nasadit do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="04e39-109">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot,  and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="04e39-110">K dokončení tohoto kurzu potřebujete účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="04e39-110">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="04e39-111">Pokud účet nemáte, můžete [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) nebo [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="04e39-111">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="04e39-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="04e39-112">Prerequisites</span></span>

<span data-ttu-id="04e39-113">V tomto kurzu se předpokládá, že jste již nainstalovali následující:</span><span class="sxs-lookup"><span data-stu-id="04e39-113">This tutorial assumes you have already installed the following:</span></span>

* [<span data-ttu-id="04e39-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="04e39-114">Visual Studio</span></span>](https://www.visualstudio.com)

* <span data-ttu-id="04e39-115">[ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (runtime a nástrojů)</span><span class="sxs-lookup"><span data-stu-id="04e39-115">[ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (runtime and tooling)</span></span>

* <span data-ttu-id="04e39-116">[Git](https://git-scm.com/downloads) pro Windows</span><span class="sxs-lookup"><span data-stu-id="04e39-116">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="04e39-117">Vytvoření webové aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04e39-117">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="04e39-118">Spuštění sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="04e39-118">Start Visual Studio.</span></span>

2. <span data-ttu-id="04e39-119">Z **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="04e39-119">From the **File** menu, select **New** > **Project**.</span></span>

3. <span data-ttu-id="04e39-120">Vyberte **webové aplikace ASP.NET** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="04e39-120">Select the **ASP.NET Web Application** project template.</span></span> <span data-ttu-id="04e39-121">Zobrazí se pod **nainstalovaná** > **šablony** > **Visual C#** > **webové**.</span><span class="sxs-lookup"><span data-stu-id="04e39-121">It appears under **Installed** > **Templates** > **Visual C#** > **Web**.</span></span> <span data-ttu-id="04e39-122">Název projektu `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="04e39-122">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="04e39-123">Vyberte **vytvoření nového úložiště Git** možnost a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="04e39-123">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![Dialogové okno Nový projekt](azure-continuous-deployment/_static/01-new-project.png)

4. <span data-ttu-id="04e39-125">V **nový projekt ASP.NET** dialogovém okně, vyberte ASP.NET Core **prázdný** šablony, pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="04e39-125">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Dialogové okno Nový projekt ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)


### <a name="running-the-web-app-locally"></a><span data-ttu-id="04e39-127">Místní spuštění webové aplikace</span><span class="sxs-lookup"><span data-stu-id="04e39-127">Running the web app locally</span></span>

1. <span data-ttu-id="04e39-128">Po dokončení vytváření aplikace Visual Studio spusťte aplikaci tak, že vyberete **ladění** -> **spustit ladění**.</span><span class="sxs-lookup"><span data-stu-id="04e39-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** -> **Start Debugging**.</span></span> <span data-ttu-id="04e39-129">Jako alternativu, můžete stisknout **F5**.</span><span class="sxs-lookup"><span data-stu-id="04e39-129">As an alternative, you can press **F5**.</span></span>

   <span data-ttu-id="04e39-130">To může trvat času k chybě při inicializaci sady Visual Studio a nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="04e39-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="04e39-131">Po dokončení se zobrazí v prohlížeči spuštěné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="04e39-131">Once it is complete, the browser will show the running app.</span></span>

   ![Zobrazení okna prohlížeče spuštění aplikace, která zobrazuje "Hello, World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

2. <span data-ttu-id="04e39-133">Po zkontrolování funkční webovou aplikaci, zavřete prohlížeč a klikněte na ikonu "Zastavte ladění" na panelu nástrojů sady Visual Studio k zastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="04e39-133">After reviewing the running Web app, close the browser and click the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="04e39-134">Vytvořit webovou aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="04e39-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="04e39-135">Následující postup vás provede vytvořením webové aplikace na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="04e39-135">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="04e39-136">Přihlaste se k [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="04e39-136">Log in to the [Azure Portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="04e39-137">Klepněte na **nový** v horní pravé portálu</span><span class="sxs-lookup"><span data-stu-id="04e39-137">TAP **NEW** at the top left of the Portal</span></span>
3. <span data-ttu-id="04e39-138">Klepněte na **Web + mobilní** > **webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="04e39-138">TAP **Web + Mobile** > **Web App**</span></span>

    ![Portál Microsoft Azure: Tlačítko Nový: Web + mobilní v Marketplace: tlačítko webové aplikace v rámci vybrané aplikace](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  <span data-ttu-id="04e39-140">V **webové aplikace** okno, zadejte jedinečnou hodnotu **název služby App Service**.</span><span class="sxs-lookup"><span data-stu-id="04e39-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

    ![Okně webové aplikace](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    ><span data-ttu-id="04e39-142">**Název služby App Service** název musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="04e39-142">The **App Service Name** name needs to be unique.</span></span> <span data-ttu-id="04e39-143">Při pokusu o zadání názvu, vynutí portálu toto pravidlo.</span><span class="sxs-lookup"><span data-stu-id="04e39-143">The portal will enforce this rule when you attempt to enter the name.</span></span> <span data-ttu-id="04e39-144">Po zadání jinou hodnotu, budete muset nahraďte tuto hodnotu pro každý výskyt **SampleWebAppDemo** , které vidíte v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="04e39-144">After you enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span>

    &nbsp;
    
    <span data-ttu-id="04e39-145">Také v **webové aplikace** okně Vybrat existující **umístění plán služby App** nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="04e39-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="04e39-146">Pokud vytvoříte nový plán, vyberte cenovou úroveň, umístění a další možnosti.</span><span class="sxs-lookup"><span data-stu-id="04e39-146">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="04e39-147">Další informace o plánech služby App Service [podrobný přehled plánů služby Azure App Service](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span><span class="sxs-lookup"><span data-stu-id="04e39-147">For more information on App Service plans, [Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span></span>

5.  <span data-ttu-id="04e39-148">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="04e39-148">Click **Create**.</span></span> <span data-ttu-id="04e39-149">Azure bude zřídit a spuštění webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="04e39-149">Azure will provision and start your web app.</span></span>

    ![Portálu Azure: Okno Essentials ukázkové webové aplikaci ukázku 01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="04e39-151">Povolení publikování Git pro nové webové aplikace</span><span class="sxs-lookup"><span data-stu-id="04e39-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="04e39-152">Git je systém správy distribuovaných verzí, který můžete použít k nasazení webové aplikace služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="04e39-152">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="04e39-153">Budete ukládat kód napsaný pro webovou aplikaci v místní úložiště Git a kód budete nasazovat do Azure nuceným doručením do vzdáleného úložiště.</span><span class="sxs-lookup"><span data-stu-id="04e39-153">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="04e39-154">Přihlaste se [portálu Azure](https://portal.azure.com), pokud jste již přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="04e39-154">Log into the [Azure Portal](https://portal.azure.com), if you're not already logged in.</span></span>

2. <span data-ttu-id="04e39-155">Klikněte na tlačítko **Procházet**, umístěné v dolní části navigačního podokna.</span><span class="sxs-lookup"><span data-stu-id="04e39-155">Click **Browse**, located at the bottom of the navigation pane.</span></span>

3. <span data-ttu-id="04e39-156">Klikněte na tlačítko **webové aplikace** zobrazíte seznam webové aplikace přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="04e39-156">Click **Web Apps** to view a list of the web apps associated with your Azure subscription.</span></span>

4. <span data-ttu-id="04e39-157">Vyberte webovou aplikaci, kterou jste vytvořili v předchozí části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="04e39-157">Select the web app you created in the previous section of this tutorial.</span></span>

5. <span data-ttu-id="04e39-158">Pokud **nastavení** okno není zobrazený, vyberte **nastavení** v **webové aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="04e39-158">If the **Settings** blade is not shown, select **Settings** in the **Web App** blade.</span></span>

6. <span data-ttu-id="04e39-159">V **nastavení** vyberte **zdroj nasazení** > **zvolit zdroj** > **místní úložiště Git**.</span><span class="sxs-lookup"><span data-stu-id="04e39-159">In the **Settings** blade, select **Deployment source** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Okno nastavení: okno zdroj nasazení: Zvolte zdroj okno](azure-continuous-deployment/_static/08-azure-localrepository.png)

7. <span data-ttu-id="04e39-161">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="04e39-161">Click **OK**.</span></span>

8. <span data-ttu-id="04e39-162">Pokud jste dříve vytvořili přihlašovací údaje nasazení pro publikování webové aplikace nebo jiná aplikace služby App Service, nastavte je nyní:</span><span class="sxs-lookup"><span data-stu-id="04e39-162">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>

   * <span data-ttu-id="04e39-163">Klikněte na tlačítko **nastavení** > **přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="04e39-163">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="04e39-164">**Nastavit přihlašovací údaje nasazení** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="04e39-164">The **Set deployment credentials** blade will be displayed.</span></span>

   * <span data-ttu-id="04e39-165">Vytvořte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="04e39-165">Create a user name and password.</span></span>  <span data-ttu-id="04e39-166">Toto heslo budete potřebovat později při nastavování Git.</span><span class="sxs-lookup"><span data-stu-id="04e39-166">You'll need this password later when setting up Git.</span></span>

   * <span data-ttu-id="04e39-167">Klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="04e39-167">Click **Save**.</span></span>

9. <span data-ttu-id="04e39-168">V **webové aplikace** okně klikněte na tlačítko **nastavení** > **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="04e39-168">In the **Web App** blade, click **Settings** > **Properties**.</span></span> <span data-ttu-id="04e39-169">Vzdálené úložiště Git, které budete nasazovat na adresu URL se zobrazí v části **adresy URL pro GIT**.</span><span class="sxs-lookup"><span data-stu-id="04e39-169">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>

10. <span data-ttu-id="04e39-170">Kopírování **adresy URL pro GIT** hodnoty pro pozdější použití v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="04e39-170">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Portálu Azure: okno vlastností aplikace](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="04e39-172">Publikování webové aplikace do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="04e39-172">Publish your web app to Azure App Service</span></span>

<span data-ttu-id="04e39-173">V této části vytvoříte místní úložiště Git pomocí sady Visual Studio a posílejte nabízená oznámení z tohoto úložiště do Azure k nasazení vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="04e39-173">In this section, you will create a local Git repository using Visual Studio and push from that repository to Azure to deploy your web app.</span></span> <span data-ttu-id="04e39-174">Kroky zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="04e39-174">The steps involved include the following:</span></span>

   * <span data-ttu-id="04e39-175">Přidejte nastavení vzdáleného úložiště pomocí adresy URL pro GIT hodnota, abyste mohli nasadit místního úložiště do Azure.</span><span class="sxs-lookup"><span data-stu-id="04e39-175">Add the remote repository setting using your GIT URL value, so you can deploy your local repository to Azure.</span></span>

   * <span data-ttu-id="04e39-176">Potvrdíte změny projektu.</span><span class="sxs-lookup"><span data-stu-id="04e39-176">Commit your project changes.</span></span>

   * <span data-ttu-id="04e39-177">Odešlete své změny projektu z místního úložiště do vzdáleného úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="04e39-177">Push your project changes from your local repository to your remote repository on Azure.</span></span>

&nbsp;
   
1.  <span data-ttu-id="04e39-178">V **Průzkumníku řešení** klikněte pravým tlačítkem na **řešení 'SampleWebAppDemo'** a vyberte **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="04e39-178">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="04e39-179">**Team Explorer** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="04e39-179">The **Team Explorer** will be displayed.</span></span>

    ![Karta Team Explorer se připojit](azure-continuous-deployment/_static/10-team-explorer.png)

2.  <span data-ttu-id="04e39-181">V **Team Explorer**, vyberte **domácí** (Ikona Domovská stránka) > **nastavení** > **nastavení úložiště**.</span><span class="sxs-lookup"><span data-stu-id="04e39-181">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

3.  <span data-ttu-id="04e39-182">V **dálkové ovladače** části **nastavení úložiště** vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="04e39-182">In the **Remotes** section of the **Repository Settings** select **Add**.</span></span> <span data-ttu-id="04e39-183">**Přidat vzdálené** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="04e39-183">The **Add Remote** dialog box will be displayed.</span></span>

4.  <span data-ttu-id="04e39-184">Nastavte **název** vzdáleného k **Azure SampleApp**.</span><span class="sxs-lookup"><span data-stu-id="04e39-184">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

5.  <span data-ttu-id="04e39-185">Nastavit hodnotu pro **načíst** k **adresy URL pro Git** který jste zkopírovali z Azure dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="04e39-185">Set the value for **Fetch** to the **Git URL** that you copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="04e39-186">Všimněte si, že toto je adresa URL, který končí **.git**.</span><span class="sxs-lookup"><span data-stu-id="04e39-186">Note that this is the URL that ends with **.git**.</span></span>

    ![Vzdálené dialogové okno Upravit](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    ><span data-ttu-id="04e39-188">Jako alternativu, můžete zadat vzdálené úložiště, ze kterého **příkazové okno** otevřením **příkazové okno**, změna do vašeho adresáře projektu a zadáte příkaz.</span><span class="sxs-lookup"><span data-stu-id="04e39-188">As an alternative, you can specify the remote repository from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the command.</span></span> <span data-ttu-id="04e39-189">Například:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span><span class="sxs-lookup"><span data-stu-id="04e39-189">For example:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span></span>

6.  <span data-ttu-id="04e39-190">Vyberte **domácí** (Ikona Domovská stránka) > **nastavení** > **globální nastavení**.</span><span class="sxs-lookup"><span data-stu-id="04e39-190">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="04e39-191">Ujistěte se, že máte své jméno a e-mailovou adresu, nastavte.</span><span class="sxs-lookup"><span data-stu-id="04e39-191">Make sure you have your name and your email address set.</span></span> <span data-ttu-id="04e39-192">Také můžete potřebovat k výběru **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="04e39-192">You may also need to select **Update**.</span></span>

7.  <span data-ttu-id="04e39-193">Vyberte **Domů** > **změny** se vrátíte do **změny** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="04e39-193">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

8.  <span data-ttu-id="04e39-194">Zadejte zprávu o potvrzení, jako například **počáteční Push č. 1** a klikněte na tlačítko **potvrzení**.</span><span class="sxs-lookup"><span data-stu-id="04e39-194">Enter a commit message, such as **Initial Push #1** and click **Commit**.</span></span> <span data-ttu-id="04e39-195">Tato akce vytvoří *potvrzení* místně.</span><span class="sxs-lookup"><span data-stu-id="04e39-195">This action will create a *commit* locally.</span></span> <span data-ttu-id="04e39-196">Potom budete muset *synchronizace* s Azure.</span><span class="sxs-lookup"><span data-stu-id="04e39-196">Next, you need to *sync* with Azure.</span></span>

    ![Karta Team Explorer se připojit](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    ><span data-ttu-id="04e39-198">Jako alternativu můžete potvrdit změny z **příkazové okno** otevřením **příkazové okno**, změna do vašeho adresáře projektu a zadáte příkazy gitu.</span><span class="sxs-lookup"><span data-stu-id="04e39-198">As an alternative, you can commit your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the git commands.</span></span> <span data-ttu-id="04e39-199">Příklad:</span><span class="sxs-lookup"><span data-stu-id="04e39-199">For example:</span></span>
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  <span data-ttu-id="04e39-200">Vyberte **Domů** > **synchronizace** > **akce** > **otevřete příkazový řádek**.</span><span class="sxs-lookup"><span data-stu-id="04e39-200">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="04e39-201">Do příkazového řádku se otevře do vašeho adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="04e39-201">The command prompt will open to your project directory.</span></span>

10.  <span data-ttu-id="04e39-202">Do příkazového řádku zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="04e39-202">Enter the following command in the command window:</span></span>

    `git push -u Azure-SampleApp master`

11.  <span data-ttu-id="04e39-203">Zadejte vaše Azure **přihlašovací údaje nasazení** heslo, které jste vytvořili dříve v Azure.</span><span class="sxs-lookup"><span data-stu-id="04e39-203">Enter your Azure **deployment credentials** password that you created earlier in Azure.</span></span>

    >[!NOTE]
    ><span data-ttu-id="04e39-204">Heslo se nezobrazí, jak ho zadáte.</span><span class="sxs-lookup"><span data-stu-id="04e39-204">Your password will not be visible as you enter it.</span></span>

    <span data-ttu-id="04e39-205">Tento příkaz spustí proces nabízení soubory místního projektu do Azure.</span><span class="sxs-lookup"><span data-stu-id="04e39-205">This command will start the process of pushing your local project files to Azure.</span></span> <span data-ttu-id="04e39-206">Výstup z výše uvedeném příkazu končí zprávu, že nasazení bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="04e39-206">The output from the above command ends with a message that deployment was successful.</span></span>
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > <span data-ttu-id="04e39-207">Pokud budete potřebovat spolupracovat na projektu, měli byste zvážit nabízet [Githubu](https://github.com) v rozmezí vkládání do Azure.</span><span class="sxs-lookup"><span data-stu-id="04e39-207">If you need to collaborate on a project, you should consider pushing to [GitHub](https://github.com) in between pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="04e39-208">Ověření aktivní nasazení</span><span class="sxs-lookup"><span data-stu-id="04e39-208">Verify the Active Deployment</span></span>

<span data-ttu-id="04e39-209">Můžete ověřit, že jste úspěšně přenesla webové aplikace z vaší místní prostředí do Azure.</span><span class="sxs-lookup"><span data-stu-id="04e39-209">You can verify that you successfully transferred the web app from your local environment to Azure.</span></span> <span data-ttu-id="04e39-210">Uvidíte uvedené úspěšné nasazení.</span><span class="sxs-lookup"><span data-stu-id="04e39-210">You'll see the listed successful deployment.</span></span>

1. <span data-ttu-id="04e39-211">V [portálu Azure](https://portal.azure.com), vyberte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="04e39-211">In the [Azure Portal](https://portal.azure.com), select your web app.</span></span> <span data-ttu-id="04e39-212">Pak vyberte **nastavení** > **průběžné nasazování**.</span><span class="sxs-lookup"><span data-stu-id="04e39-212">Then, select **Settings** > **Continuous deployment**.</span></span>

   ![Portálu Azure: Okno nastavení: nasazení okna zobrazující úspěšné nasazení](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="04e39-214">Spusťte aplikaci v Azure</span><span class="sxs-lookup"><span data-stu-id="04e39-214">Run the app in Azure</span></span>

<span data-ttu-id="04e39-215">Teď, když jste nasadili vaší webové aplikace do Azure, můžete spustit aplikace.</span><span class="sxs-lookup"><span data-stu-id="04e39-215">Now that you have deployed your web app to Azure, you can run the app.</span></span>

<span data-ttu-id="04e39-216">Tento krok můžete provést dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="04e39-216">This can be done in two ways:</span></span>

* <span data-ttu-id="04e39-217">Na portálu Azure, vyhledejte okně webové aplikace pro vaši webovou aplikaci a klikněte na tlačítko **Procházet** Chcete-li zobrazit aplikaci ve výchozím prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="04e39-217">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app in your default browser.</span></span>

* <span data-ttu-id="04e39-218">Otevřete prohlížeč a zadejte adresu URL pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="04e39-218">Open a browser and enter the URL for your web app.</span></span> <span data-ttu-id="04e39-219">Příklad:</span><span class="sxs-lookup"><span data-stu-id="04e39-219">For example:</span></span>

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a><span data-ttu-id="04e39-220">Aktualizovat webovou aplikaci a znovu publikovat</span><span class="sxs-lookup"><span data-stu-id="04e39-220">Update your web app and republish</span></span>

<span data-ttu-id="04e39-221">Po provedení změn do místní kódu můžete znovu publikovat.</span><span class="sxs-lookup"><span data-stu-id="04e39-221">After you make changes to your local code, you can republish.</span></span>

1.  <span data-ttu-id="04e39-222">V **Průzkumníku řešení** sady Visual Studio, otevřete *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="04e39-222">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

2.  <span data-ttu-id="04e39-223">V `Configure` metoda, upravte `Response.WriteAsync` metoda tak to vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="04e39-223">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  <span data-ttu-id="04e39-224">Uložit změny do *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="04e39-224">Save changes to *Startup.cs*.</span></span>

4.  <span data-ttu-id="04e39-225">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **řešení 'SampleWebAppDemo'** a vyberte **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="04e39-225">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="04e39-226">**Team Explorer** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="04e39-226">The **Team Explorer** will be displayed.</span></span>

5.  <span data-ttu-id="04e39-227">Zadejte zprávu o potvrzení, jako například:</span><span class="sxs-lookup"><span data-stu-id="04e39-227">Enter a commit message, such as:</span></span>

    ```none
    Update #2
    ```

6.  <span data-ttu-id="04e39-228">Stiskněte **potvrzení** tlačítko potvrzení změn projektu.</span><span class="sxs-lookup"><span data-stu-id="04e39-228">Press the **Commit** button to commit the project changes.</span></span>

7.  <span data-ttu-id="04e39-229">Vyberte **Domů** > **synchronizace** > **akce** > **Push**.</span><span class="sxs-lookup"><span data-stu-id="04e39-229">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

>[!NOTE]
><span data-ttu-id="04e39-230">Jako alternativu můžete odešlete své změny z **příkazové okno** otevřením **příkazové okno**, změna do vašeho adresáře projektu a zadáním příkazu git.</span><span class="sxs-lookup"><span data-stu-id="04e39-230">As an alternative, you can push your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering a git command.</span></span> <span data-ttu-id="04e39-231">Příklad:</span><span class="sxs-lookup"><span data-stu-id="04e39-231">For example:</span></span>
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="04e39-232">Zobrazení aktualizované webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="04e39-232">View the updated web app in Azure</span></span>

<span data-ttu-id="04e39-233">Zobrazení aktualizované webové aplikace tak, že vyberete **Procházet** v okně webové aplikace na portálu Azure nebo otevřením prohlížeče a zadávat adresu URL pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="04e39-233">View your updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for your web app.</span></span> <span data-ttu-id="04e39-234">Příklad:</span><span class="sxs-lookup"><span data-stu-id="04e39-234">For example:</span></span>

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a><span data-ttu-id="04e39-235">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="04e39-235">Additional Resources</span></span>

* [<span data-ttu-id="04e39-236">Publikování a nasazení</span><span class="sxs-lookup"><span data-stu-id="04e39-236">Publishing and Deployment</span></span>](index.md)

* [<span data-ttu-id="04e39-237">Kudu projektu</span><span class="sxs-lookup"><span data-stu-id="04e39-237">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
