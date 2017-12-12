---
title: "Publikování aplikace ASP.NET Core do Azure pomocí sady Visual Studio"
author: rick-anderson
description: 
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 6f697ed4d8876a19cd058533e4f6a5d4f7cdc2fb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="9dd88-103">Publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9dd88-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="9dd88-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), a [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="9dd88-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="9dd88-105">V tématu [publikovat do Azure ze sady Visual Studio pro Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) při práci na macu.</span><span class="sxs-lookup"><span data-stu-id="9dd88-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="9dd88-106">Nastavení</span><span class="sxs-lookup"><span data-stu-id="9dd88-106">Set up</span></span>

* <span data-ttu-id="9dd88-107">Otevřete [bezplatný účet Azure](https://aka.ms/K5y5yh) Pokud nemáte jeden.</span><span class="sxs-lookup"><span data-stu-id="9dd88-107">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="9dd88-108">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="9dd88-108">Create a web app</span></span>

<span data-ttu-id="9dd88-109">Ve Visual Studio – úvodní stránka, vyberte **soubor > Nový > projekt...**</span><span class="sxs-lookup"><span data-stu-id="9dd88-109">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![nabídka Soubor](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="9dd88-111">Dokončení **nový projekt** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="9dd88-111">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="9dd88-112">V levém podokně vyberte **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-112">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="9dd88-113">V prostředním podokně vyberte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-113">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="9dd88-114">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-114">Select **OK**.</span></span>

![Dialogové okno Nový projekt](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="9dd88-116">V **nové webové aplikace ASP.NET Core** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="9dd88-116">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="9dd88-117">Vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-117">Select **Web Application**.</span></span>

* <span data-ttu-id="9dd88-118">Vyberte **změnit ověřování**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-118">Select **Change Authentication**.</span></span>

![Dialogové okno Nový projekt](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="9dd88-120">**Změna ověřování** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9dd88-120">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="9dd88-121">Vyberte **jednotlivých uživatelských účtů**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-121">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="9dd88-122">Vyberte **OK** se vrátíte do **nové webové aplikace ASP.NET Core**, pak vyberte **OK** znovu.</span><span class="sxs-lookup"><span data-stu-id="9dd88-122">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Dialogové okno Nový ASP.NET Web základní ověřování](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="9dd88-124">Visual Studio vytvoří řešení.</span><span class="sxs-lookup"><span data-stu-id="9dd88-124">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="9dd88-125">Místní spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="9dd88-125">Run the app locally</span></span>

* <span data-ttu-id="9dd88-126">Zvolte **ladění** pak **spustit bez ladění** a spusťte aplikaci místně.</span><span class="sxs-lookup"><span data-stu-id="9dd88-126">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="9dd88-127">Klikněte na tlačítko **o** a **kontaktujte** odkazy na ověření webové aplikace funguje.</span><span class="sxs-lookup"><span data-stu-id="9dd88-127">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Webové aplikace otevřete v Microsoft Edge na místním hostiteli](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="9dd88-129">Vyberte **zaregistrovat** a registraci nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="9dd88-129">Select **Register** and register a new user.</span></span> <span data-ttu-id="9dd88-130">Můžete použít fiktivní e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="9dd88-130">You can use a fictitious email address.</span></span> <span data-ttu-id="9dd88-131">Při odesílání, na stránce se zobrazí chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="9dd88-131">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="9dd88-132">*"Vnitřní chyba serveru: Databázová operace se nezdařila při zpracování požadavku. Výjimky SQL: databázi nelze otevřít. Použití existující migrace pro kontext databáze aplikace může tento problém vyřešit."*</span><span class="sxs-lookup"><span data-stu-id="9dd88-132">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="9dd88-133">Vyberte **použít migrace** a jakmile se aktualizace stránky, aktualizujte stránku.</span><span class="sxs-lookup"><span data-stu-id="9dd88-133">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Vnitřní chyba serveru: Databázová operace se nezdařila při zpracování požadavku.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="9dd88-137">Aplikace zobrazí e-mail použitý k registraci nového uživatele a **odhlášení** odkaz.</span><span class="sxs-lookup"><span data-stu-id="9dd88-137">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Webové aplikace, otevřete v Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="9dd88-140">Nasazení aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="9dd88-140">Deploy the app to Azure</span></span>

<span data-ttu-id="9dd88-141">Zavřete webové stránky, vraťte se na Visual Studio a vyberte **Zastavte ladění** z **ladění** nabídky.</span><span class="sxs-lookup"><span data-stu-id="9dd88-141">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="9dd88-142">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **publikování...** .</span><span class="sxs-lookup"><span data-stu-id="9dd88-142">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Kontextové nabídky otevřete se zvýrazněnou odkaz publikování](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="9dd88-144">V **publikovat** dialogovém okně, vyberte **Microsoft Azure App Service** a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-144">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Dialogové okno publikování](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="9dd88-146">Název aplikace jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="9dd88-146">Name the app a unique name.</span></span> 

* <span data-ttu-id="9dd88-147">Vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="9dd88-147">Select a subscription.</span></span>

* <span data-ttu-id="9dd88-148">Vyberte **nové...**  pro prostředek skupiny a zadejte název pro novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="9dd88-148">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="9dd88-149">Vyberte **nové...**  pro plán služby app service a vyberte umístění okolo vás.</span><span class="sxs-lookup"><span data-stu-id="9dd88-149">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="9dd88-150">Můžete ponechat název, který je generován ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="9dd88-150">You can keep the name that is generated by default.</span></span>

![Dialogovém okně App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="9dd88-152">Vyberte **služby** a vytvořit novou databázi.</span><span class="sxs-lookup"><span data-stu-id="9dd88-152">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="9dd88-153">Vyberte zeleným  **+**  vytvořte novou databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="9dd88-153">Select the green **+** icon to create a new SQL Database</span></span>

![Novou databázi SQL.](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="9dd88-155">Vyberte **nové...**  na **nakonfigurovat databázi SQL** dialogovém okně můžete vytvořit novou databázi.</span><span class="sxs-lookup"><span data-stu-id="9dd88-155">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Nové databáze SQL a serveru](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="9dd88-157">**Nakonfigurujte systém SQL Server** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9dd88-157">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="9dd88-158">Zadejte uživatelské jméno správce a heslo a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-158">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="9dd88-159">Nezapomeňte, uživatelské jméno a heslo, které vytvoříte v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="9dd88-159">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="9dd88-160">Můžete ponechat výchozí **název serveru**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-160">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="9dd88-161">Zadejte názvy databáze a připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="9dd88-161">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="9dd88-162">"admin" není povolen jako uživatelské jméno správce.</span><span class="sxs-lookup"><span data-stu-id="9dd88-162">"admin" is not allowed as the administrator user name.</span></span>

![Konfigurace systému SQL Server dialogové okno](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="9dd88-164">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-164">Select **OK**.</span></span>

<span data-ttu-id="9dd88-165">Visual Studio vrátí **vytvořit službu App Service** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9dd88-165">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="9dd88-166">Vyberte **vytvořit** na **vytvořit službu App Service** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9dd88-166">Select **Create** on the **Create App Service** dialog.</span></span>

![Konfigurace dialogové okno databáze SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="9dd88-168">Klikněte **nastavení** na odkaz v **publikovat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9dd88-168">Click the **Settings** link in the **Publish** dialog.</span></span>

![Dialogové okno publikování: připojení panely](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="9dd88-170">Na **nastavení** stránky **publikovat** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="9dd88-170">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="9dd88-171">Rozbalte položku **databáze** a zkontrolujte **použít tento připojovací řetězec za běhu**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-171">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="9dd88-172">Rozbalte položku **Entity Framework migrace** a zkontrolujte **použít publikování této migrace na**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-172">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="9dd88-173">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-173">Select **Save**.</span></span> <span data-ttu-id="9dd88-174">Visual Studio vrátí **publikovat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9dd88-174">Visual Studio returns to the **Publish** dialog.</span></span> 

![Dialogové okno publikování: panel nastavení](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="9dd88-176">Klikněte na tlačítko **publikování**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-176">Click **Publish**.</span></span> <span data-ttu-id="9dd88-177">Visual Studio bude publikování aplikace do Azure a spouštět cloudové aplikace v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="9dd88-177">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="9dd88-178">Testování aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="9dd88-178">Test your app in Azure</span></span>

* <span data-ttu-id="9dd88-179">Testovací **o** a **kontaktujte** odkazy</span><span class="sxs-lookup"><span data-stu-id="9dd88-179">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="9dd88-180">Registrace nového uživatele</span><span class="sxs-lookup"><span data-stu-id="9dd88-180">Register a new user</span></span>

![Webová aplikace otevřít v Microsoft Edge ve službě Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="9dd88-182">Aktualizace aplikace</span><span class="sxs-lookup"><span data-stu-id="9dd88-182">Update the app</span></span>

* <span data-ttu-id="9dd88-183">Upravit *Pages/About.cshtml* Razor stránky a změňte jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="9dd88-183">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="9dd88-184">Například můžete upravit odstavce. Tím vyjádříte "Hello ASP.NET Core!":</span><span class="sxs-lookup"><span data-stu-id="9dd88-184">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="9dd88-185">Klikněte pravým tlačítkem na projekt a vyberte **publikování...**  znovu.</span><span class="sxs-lookup"><span data-stu-id="9dd88-185">Right-click on the project and select **Publish...** again.</span></span>

![Kontextové nabídky otevřete se zvýrazněnou odkaz publikování](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="9dd88-187">Po publikování aplikace, ověřte, zda jsou k dispozici v Azure provedené změny.</span><span class="sxs-lookup"><span data-stu-id="9dd88-187">After the app is published, verify the changes you made are available on Azure.</span></span>

![Ověřte, že úkol je dokončen](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="9dd88-189">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="9dd88-189">Clean up</span></span>

<span data-ttu-id="9dd88-190">Po dokončení testování aplikace, přejděte na [portál Azure](https://portal.azure.com/) a odstraňte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9dd88-190">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="9dd88-191">Vyberte **skupiny prostředků**, pak vyberte skupinu prostředků, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="9dd88-191">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure Portal: Skupiny prostředků v nabídce bočním panelu](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="9dd88-193">V **skupiny prostředků** vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-193">In the **Resource groups** page, select **Delete**.</span></span>

![Portálu Azure: Skupiny prostředků stránky](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="9dd88-195">Zadejte název skupiny prostředků a vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="9dd88-195">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="9dd88-196">Vaše aplikace a všechny další prostředky, které jsou vytvořené v tomto kurzu se teď odstraní z Azure.</span><span class="sxs-lookup"><span data-stu-id="9dd88-196">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="9dd88-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9dd88-197">Next steps</span></span>

* [<span data-ttu-id="9dd88-198">Průběžné nasazování do Azure pomocí sady Visual Studio a Git</span><span class="sxs-lookup"><span data-stu-id="9dd88-198">Continuous Deployment to Azure with Visual Studio and Git</span></span>](../publishing/azure-continuous-deployment.md)
