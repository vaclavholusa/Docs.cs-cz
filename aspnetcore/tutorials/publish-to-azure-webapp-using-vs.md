---
title: Publikování aplikace ASP.NET Core do Azure pomocí sady Visual Studio
author: rick-anderson
description: Zjistěte, jak publikovat aplikaci ASP.NET Core Azure App Service pomocí sady Visual Studio.
manager: wpickett
ms.author: riande
ms.date: 12/16/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: e5c213a682c9bf7c64c40fad630cacfff24e23bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897619"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio"></a><span data-ttu-id="73ea7-103">Publikování aplikace ASP.NET Core do Azure pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73ea7-103">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>

<span data-ttu-id="73ea7-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), a [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="73ea7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="73ea7-105">V tématu [publikovat do Azure ze sady Visual Studio pro Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) při práci v systému macOS.</span><span class="sxs-lookup"><span data-stu-id="73ea7-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on macOS.</span></span>

<span data-ttu-id="73ea7-106">Chcete-li vyřešit problém nasazení služby App Service, najdete v části [řešení potíží s ASP.NET Core v Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="73ea7-106">To troubleshoot an App Service deployment issue, see [Troubleshoot ASP.NET Core on Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

## <a name="set-up"></a><span data-ttu-id="73ea7-107">Nastavení</span><span class="sxs-lookup"><span data-stu-id="73ea7-107">Set up</span></span>

* <span data-ttu-id="73ea7-108">Otevřete [bezplatný účet Azure](https://aka.ms/K5y5yh) Pokud nemáte.</span><span class="sxs-lookup"><span data-stu-id="73ea7-108">Open a [free Azure account](https://aka.ms/K5y5yh) if you don't have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="73ea7-109">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="73ea7-109">Create a web app</span></span>

<span data-ttu-id="73ea7-110">Ve Visual Studio – úvodní stránka, vyberte **soubor > Nový > projekt...**</span><span class="sxs-lookup"><span data-stu-id="73ea7-110">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![nabídka Soubor](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="73ea7-112">Dokončení **nový projekt** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="73ea7-112">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="73ea7-113">V levém podokně vyberte **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-113">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="73ea7-114">V prostředním podokně vyberte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-114">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="73ea7-115">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-115">Select **OK**.</span></span>

![Dialogové okno Nový projekt](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="73ea7-117">V **nové webové aplikace ASP.NET Core** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="73ea7-117">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="73ea7-118">Vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-118">Select **Web Application**.</span></span>
* <span data-ttu-id="73ea7-119">Vyberte **změnit ověřování**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-119">Select **Change Authentication**.</span></span>

![Dialogové okno Nový projekt](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="73ea7-121">**Změna ověřování** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73ea7-121">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="73ea7-122">Vyberte **jednotlivých uživatelských účtů**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-122">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="73ea7-123">Vyberte **OK** se vrátíte do **nové webové aplikace ASP.NET Core**, pak vyberte **OK** znovu.</span><span class="sxs-lookup"><span data-stu-id="73ea7-123">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Dialogové okno Nový ASP.NET Web základní ověřování](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="73ea7-125">Visual Studio vytvoří řešení.</span><span class="sxs-lookup"><span data-stu-id="73ea7-125">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="73ea7-126">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="73ea7-126">Run the app</span></span>

* <span data-ttu-id="73ea7-127">Stisknutím kombinace kláves CTRL + F5 a spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="73ea7-127">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="73ea7-128">Testovací **o** a **kontaktujte** odkazy.</span><span class="sxs-lookup"><span data-stu-id="73ea7-128">Test the **About** and **Contact** links.</span></span>

![Webové aplikace otevřete v Microsoft Edge na místním hostiteli](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="73ea7-130">Registrace uživatele</span><span class="sxs-lookup"><span data-stu-id="73ea7-130">Register a user</span></span>

* <span data-ttu-id="73ea7-131">Vyberte **zaregistrovat** a registraci nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="73ea7-131">Select **Register** and register a new user.</span></span> <span data-ttu-id="73ea7-132">Můžete použít fiktivní e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="73ea7-132">You can use a fictitious email address.</span></span> <span data-ttu-id="73ea7-133">Při odesílání, na stránce se zobrazí chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="73ea7-133">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="73ea7-134">*"Vnitřní chyba serveru: Databázová operace se nezdařila při zpracování požadavku. Výjimky SQL: databázi nelze otevřít. Použití existující migrace pro kontext databáze aplikace může tento problém vyřešit."*</span><span class="sxs-lookup"><span data-stu-id="73ea7-134">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="73ea7-135">Vyberte **použít migrace** a jakmile se aktualizace stránky, aktualizujte stránku.</span><span class="sxs-lookup"><span data-stu-id="73ea7-135">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Vnitřní chyba serveru: Databázová operace se nezdařila při zpracování požadavku.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="73ea7-139">Aplikace zobrazí e-mail použitý k registraci nového uživatele a **odhlášení** odkaz.</span><span class="sxs-lookup"><span data-stu-id="73ea7-139">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Webové aplikace, otevřete v Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="73ea7-142">Nasazení aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="73ea7-142">Deploy the app to Azure</span></span>

<span data-ttu-id="73ea7-143">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **publikování...** .</span><span class="sxs-lookup"><span data-stu-id="73ea7-143">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Kontextové nabídky otevřete se zvýrazněnou odkaz publikování](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="73ea7-145">V **publikovat** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="73ea7-145">In the **Publish** dialog:</span></span>

* <span data-ttu-id="73ea7-146">Vyberte **služby Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-146">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="73ea7-147">Vyberte ikonu zařízení a pak vyberte **vytvořit profil**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-147">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="73ea7-148">Vyberte **vytvořit profil**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-148">Select **Create Profile**.</span></span>

![Dialogové okno publikování](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="73ea7-150">Vytváření prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="73ea7-150">Create Azure resources</span></span>

<span data-ttu-id="73ea7-151">**Vytvořit službu App Service** otevře se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="73ea7-151">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="73ea7-152">Zadejte vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="73ea7-152">Enter your subscription.</span></span>
* <span data-ttu-id="73ea7-153">**Název aplikace**, **skupiny prostředků**, a **plán služby App Service** vstupní pole se vyplní.</span><span class="sxs-lookup"><span data-stu-id="73ea7-153">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="73ea7-154">Můžete ponechat tyto názvy nebo změnit.</span><span class="sxs-lookup"><span data-stu-id="73ea7-154">You can keep these names or change them.</span></span>

![Dialogovém okně App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="73ea7-156">Vyberte **služby** a vytvořit novou databázi.</span><span class="sxs-lookup"><span data-stu-id="73ea7-156">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="73ea7-157">Vyberte zeleným **+** vytvořte novou databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="73ea7-157">Select the green **+** icon to create a new SQL Database</span></span>

![Novou databázi SQL.](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="73ea7-159">Vyberte **nové...**  na **nakonfigurovat databázi SQL** dialogovém okně můžete vytvořit novou databázi.</span><span class="sxs-lookup"><span data-stu-id="73ea7-159">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Nové databáze SQL a serveru](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="73ea7-161">**Nakonfigurujte systém SQL Server** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73ea7-161">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="73ea7-162">Zadejte uživatelské jméno správce a heslo a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-162">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="73ea7-163">Můžete ponechat výchozí **název serveru**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-163">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="73ea7-164">"admin" není povolen jako uživatelské jméno správce.</span><span class="sxs-lookup"><span data-stu-id="73ea7-164">"admin" isn't allowed as the administrator user name.</span></span>

![Konfigurace systému SQL Server dialogové okno](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="73ea7-166">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-166">Select **OK**.</span></span>

<span data-ttu-id="73ea7-167">Visual Studio vrátí **vytvořit službu App Service** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73ea7-167">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="73ea7-168">Vyberte **vytvořit** na **vytvořit službu App Service** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73ea7-168">Select **Create** on the **Create App Service** dialog.</span></span>

![Konfigurace dialogové okno databáze SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="73ea7-170">Visual Studio vytvoří webovou aplikaci a SQL Server na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="73ea7-170">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="73ea7-171">Tento krok může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="73ea7-171">This step can take a few minutes.</span></span> <span data-ttu-id="73ea7-172">Informace o prostředky vytvořené v tématu [dalších prostředků](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="73ea7-172">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="73ea7-173">Po dokončení nasazení, vyberte **nastavení**:</span><span class="sxs-lookup"><span data-stu-id="73ea7-173">When deployment completes, select **Settings**:</span></span>

![Konfigurace systému SQL Server dialogové okno](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="73ea7-175">Na **nastavení** stránky **publikovat** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="73ea7-175">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="73ea7-176">Rozbalte položku **databáze** a zkontrolujte **použít tento připojovací řetězec za běhu**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-176">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="73ea7-177">Rozbalte položku **Entity Framework migrace** a zkontrolujte **použít publikování této migrace na**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-177">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="73ea7-178">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-178">Select **Save**.</span></span> <span data-ttu-id="73ea7-179">Visual Studio vrátí **publikovat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73ea7-179">Visual Studio returns to the **Publish** dialog.</span></span> 

![Dialogové okno publikování: panel nastavení](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="73ea7-181">Klikněte na tlačítko **publikování**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-181">Click **Publish**.</span></span> <span data-ttu-id="73ea7-182">Visual Studio publishs vaší aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="73ea7-182">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="73ea7-183">Po dokončení nasazení aplikace se otevře v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="73ea7-183">When the deployment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="73ea7-184">Testování aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="73ea7-184">Test your app in Azure</span></span>

* <span data-ttu-id="73ea7-185">Testovací **o** a **kontaktujte** odkazy</span><span class="sxs-lookup"><span data-stu-id="73ea7-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="73ea7-186">Registrace nového uživatele</span><span class="sxs-lookup"><span data-stu-id="73ea7-186">Register a new user</span></span>

![Webová aplikace otevřít v Microsoft Edge ve službě Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="73ea7-188">Aktualizace aplikace</span><span class="sxs-lookup"><span data-stu-id="73ea7-188">Update the app</span></span>

* <span data-ttu-id="73ea7-189">Upravit *Pages/About.cshtml* Razor stránky a změňte jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="73ea7-189">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="73ea7-190">Například můžete upravit odstavce. Tím vyjádříte "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="73ea7-190">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="73ea7-191">Klikněte pravým tlačítkem na projekt a vyberte **publikování...**  znovu.</span><span class="sxs-lookup"><span data-stu-id="73ea7-191">Right-click on the project and select **Publish...** again.</span></span>

![Kontextové nabídky otevřete se zvýrazněnou odkaz publikování](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="73ea7-193">Po publikování aplikace, ověřte, zda jsou k dispozici v Azure provedené změny.</span><span class="sxs-lookup"><span data-stu-id="73ea7-193">After the app is published, verify the changes you made are available on Azure.</span></span>

![Ověřte, že úkol je dokončen](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="73ea7-195">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="73ea7-195">Clean up</span></span>

<span data-ttu-id="73ea7-196">Po dokončení testování aplikace, přejděte na [portál Azure](https://portal.azure.com/) a odstraňte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="73ea7-196">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="73ea7-197">Vyberte **skupiny prostředků**, pak vyberte skupinu prostředků, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="73ea7-197">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure Portal: Skupiny prostředků v nabídce bočním panelu](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="73ea7-199">V **skupiny prostředků** vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-199">In the **Resource groups** page, select **Delete**.</span></span>

![Portálu Azure: Skupiny prostředků stránky](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="73ea7-201">Zadejte název skupiny prostředků a vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="73ea7-201">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="73ea7-202">Vaše aplikace a všechny další prostředky, které jsou vytvořené v tomto kurzu se teď odstraní z Azure.</span><span class="sxs-lookup"><span data-stu-id="73ea7-202">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="73ea7-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="73ea7-203">Next steps</span></span>

* [<span data-ttu-id="73ea7-204">Průběžné nasazování do Azure pomocí sady Visual Studio a Git</span><span class="sxs-lookup"><span data-stu-id="73ea7-204">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="73ea7-205">Dalších prostředků</span><span class="sxs-lookup"><span data-stu-id="73ea7-205">Additonal resources</span></span>

* [<span data-ttu-id="73ea7-206">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="73ea7-206">Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="73ea7-207">Skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="73ea7-207">Azure resource groups</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="73ea7-208">Databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="73ea7-208">Azure SQL Database</span></span>](https://docs.microsoft.com/azure/sql-database/)
* [<span data-ttu-id="73ea7-209">Řešení potíží s ASP.NET Core ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="73ea7-209">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
