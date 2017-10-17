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
ms.openlocfilehash: df22852d2daddb2a3faef8404d0d250a6a1697a5
ms.sourcegitcommit: e987c950caae7af9c4ece8a82228caa364e0a5df
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/05/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="b3c1a-103">Publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b3c1a-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="b3c1a-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), a [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="b3c1a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="set-up"></a><span data-ttu-id="b3c1a-105">Nastavení</span><span class="sxs-lookup"><span data-stu-id="b3c1a-105">Set up</span></span>

* <span data-ttu-id="b3c1a-106">Otevřete [bezplatný účet Azure](https://aka.ms/K5y5yh) Pokud nemáte jeden.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-106">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="b3c1a-107">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="b3c1a-107">Create a web app</span></span>

<span data-ttu-id="b3c1a-108">Ve Visual Studio – úvodní stránka, vyberte **soubor > Nový > projekt...**</span><span class="sxs-lookup"><span data-stu-id="b3c1a-108">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![nabídka Soubor](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="b3c1a-110">Dokončení **nový projekt** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="b3c1a-110">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="b3c1a-111">V levém podokně vyberte **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-111">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="b3c1a-112">V prostředním podokně vyberte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-112">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="b3c1a-113">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-113">Select **OK**.</span></span>

![Dialogové okno Nový projekt](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="b3c1a-115">V **nové webové aplikace ASP.NET Core** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="b3c1a-115">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="b3c1a-116">Vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-116">Select **Web Application**.</span></span>

* <span data-ttu-id="b3c1a-117">Vyberte **změnit ověřování**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-117">Select **Change Authentication**.</span></span>

![Dialogové okno Nový projekt](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="b3c1a-119">**Změna ověřování** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-119">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="b3c1a-120">Vyberte **jednotlivých uživatelských účtů**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-120">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="b3c1a-121">Vyberte **OK** se vrátíte do **nové webové aplikace ASP.NET Core**, pak vyberte **OK** znovu.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-121">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Dialogové okno Nový ASP.NET Web základní ověřování](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="b3c1a-123">Visual Studio vytvoří řešení.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-123">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="b3c1a-124">Místní spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b3c1a-124">Run the app locally</span></span>

* <span data-ttu-id="b3c1a-125">Zvolte **ladění** pak **spustit bez ladění** a spusťte aplikaci místně.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-125">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="b3c1a-126">Klikněte na tlačítko **o** a **kontaktujte** odkazy na ověření webové aplikace funguje.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-126">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Webové aplikace otevřete v Microsoft Edge na místním hostiteli](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="b3c1a-128">Vyberte **zaregistrovat** a registraci nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-128">Select **Register** and register a new user.</span></span> <span data-ttu-id="b3c1a-129">Můžete použít fiktivní e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-129">You can use a fictitious email address.</span></span> <span data-ttu-id="b3c1a-130">Při odesílání, na stránce se zobrazí chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="b3c1a-130">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="b3c1a-131">*"Vnitřní chyba serveru: Databázová operace se nezdařila při zpracování požadavku. Výjimky SQL: databázi nelze otevřít. Použití existující migrace pro kontext databáze aplikace může tento problém vyřešit."*</span><span class="sxs-lookup"><span data-stu-id="b3c1a-131">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="b3c1a-132">Vyberte **použít migrace** a jakmile se aktualizace stránky, aktualizujte stránku.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-132">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Vnitřní chyba serveru: Databázová operace se nezdařila při zpracování požadavku.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="b3c1a-136">Aplikace zobrazí e-mail použitý k registraci nového uživatele a **odhlášení** odkaz.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-136">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Webové aplikace, otevřete v Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="b3c1a-139">Nasazení aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="b3c1a-139">Deploy the app to Azure</span></span>

<span data-ttu-id="b3c1a-140">Zavřete webové stránky, vraťte se na Visual Studio a vyberte **Zastavte ladění** z **ladění** nabídky.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-140">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="b3c1a-141">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **publikování...** .</span><span class="sxs-lookup"><span data-stu-id="b3c1a-141">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Kontextové nabídky otevřete se zvýrazněnou odkaz publikování](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="b3c1a-143">V **publikovat** dialogovém okně, vyberte **Microsoft Azure App Service** a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-143">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Dialogové okno publikování](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="b3c1a-145">Název aplikace jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-145">Name the app a unique name.</span></span> 

* <span data-ttu-id="b3c1a-146">Vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-146">Select a subscription.</span></span>

* <span data-ttu-id="b3c1a-147">Vyberte **nové...**  pro prostředek skupiny a zadejte název pro novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-147">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="b3c1a-148">Vyberte **nové...**  pro plán služby app service a vyberte umístění okolo vás.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-148">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="b3c1a-149">Můžete ponechat název, který je generován ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-149">You can keep the name that is generated by default.</span></span>

![Dialogovém okně App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="b3c1a-151">Vyberte **služby** a vytvořit novou databázi.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-151">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="b3c1a-152">Vyberte zeleným  **+**  vytvořte novou databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-152">Select the green **+** icon to create a new SQL Database</span></span>

![Novou databázi SQL.](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="b3c1a-154">Vyberte **nové...**  na **nakonfigurovat databázi SQL** dialogovém okně můžete vytvořit novou databázi.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-154">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Nové databáze SQL a serveru](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="b3c1a-156">**Nakonfigurujte systém SQL Server** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-156">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="b3c1a-157">Zadejte uživatelské jméno správce a heslo a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-157">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="b3c1a-158">Nezapomeňte, uživatelské jméno a heslo, které vytvoříte v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-158">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="b3c1a-159">Můžete ponechat výchozí **název serveru**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-159">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="b3c1a-160">Zadejte názvy databáze a připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-160">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="b3c1a-161">"admin" není povolen jako uživatelské jméno správce.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-161">"admin" is not allowed as the administrator user name.</span></span>

![Konfigurace systému SQL Server dialogové okno](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="b3c1a-163">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-163">Select **OK**.</span></span>

<span data-ttu-id="b3c1a-164">Visual Studio vrátí **vytvořit službu App Service** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-164">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="b3c1a-165">Vyberte **vytvořit** na **vytvořit službu App Service** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-165">Select **Create** on the **Create App Service** dialog.</span></span>

![Konfigurace dialogové okno databáze SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="b3c1a-167">Klikněte **nastavení** na odkaz v **publikovat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-167">Click the **Settings** link in the **Publish** dialog.</span></span>

![Dialogové okno publikování: připojení panely](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="b3c1a-169">Na **nastavení** stránky **publikovat** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="b3c1a-169">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="b3c1a-170">Rozbalte položku **databáze** a zkontrolujte **použít tento připojovací řetězec za běhu**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-170">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="b3c1a-171">Rozbalte položku **Entity Framework migrace** a zkontrolujte **použít publikování této migrace na**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-171">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="b3c1a-172">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-172">Select **Save**.</span></span> <span data-ttu-id="b3c1a-173">Visual Studio vrátí **publikovat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-173">Visual Studio returns to the **Publish** dialog.</span></span> 

![Dialogové okno publikování: panel nastavení](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="b3c1a-175">Klikněte na tlačítko **publikování**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-175">Click **Publish**.</span></span> <span data-ttu-id="b3c1a-176">Visual Studio bude publikování aplikace do Azure a spouštět cloudové aplikace v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-176">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="b3c1a-177">Testování aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="b3c1a-177">Test your app in Azure</span></span>

* <span data-ttu-id="b3c1a-178">Testovací **o** a **kontaktujte** odkazy</span><span class="sxs-lookup"><span data-stu-id="b3c1a-178">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="b3c1a-179">Registrace nového uživatele</span><span class="sxs-lookup"><span data-stu-id="b3c1a-179">Register a new user</span></span>

![Webová aplikace otevřít v Microsoft Edge ve službě Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="b3c1a-181">Aktualizace aplikace</span><span class="sxs-lookup"><span data-stu-id="b3c1a-181">Update the app</span></span>

* <span data-ttu-id="b3c1a-182">Upravit *Pages/About.cshtml* Razor stránky a změňte jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-182">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="b3c1a-183">Například můžete upravit odstavce. Tím vyjádříte "Hello ASP.NET Core!":</span><span class="sxs-lookup"><span data-stu-id="b3c1a-183">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="b3c1a-184">Klikněte pravým tlačítkem na projekt a vyberte **publikování...**  znovu.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-184">Right-click on the project and select **Publish...** again.</span></span>

![Kontextové nabídky otevřete se zvýrazněnou odkaz publikování](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="b3c1a-186">Po publikování aplikace, ověřte, zda jsou k dispozici v Azure provedené změny.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-186">After the app is published, verify the changes you made are available on Azure.</span></span>

![Ověřte, že úkol je dokončen](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="b3c1a-188">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="b3c1a-188">Clean up</span></span>

<span data-ttu-id="b3c1a-189">Po dokončení testování aplikace, přejděte na [portál Azure](https://portal.azure.com/) a odstraňte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-189">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="b3c1a-190">Vyberte **skupiny prostředků**, pak vyberte skupinu prostředků, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-190">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure Portal: Skupiny prostředků v nabídce bočním panelu](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="b3c1a-192">V **skupiny prostředků** vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-192">In the **Resource groups** page, select **Delete**.</span></span>

![Portálu Azure: Skupiny prostředků stránky](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="b3c1a-194">Zadejte název skupiny prostředků a vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-194">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="b3c1a-195">Vaše aplikace a všechny další prostředky, které jsou vytvořené v tomto kurzu se teď odstraní z Azure.</span><span class="sxs-lookup"><span data-stu-id="b3c1a-195">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="b3c1a-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b3c1a-196">Next steps</span></span>

* [<span data-ttu-id="b3c1a-197">Průběžné nasazování do Azure pomocí sady Visual Studio a Git</span><span class="sxs-lookup"><span data-stu-id="b3c1a-197">Continuous Deployment to Azure with Visual Studio and Git</span></span>](../publishing/azure-continuous-deployment.md)