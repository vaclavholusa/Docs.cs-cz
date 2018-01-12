---
title: "Publikování aplikace ASP.NET Core do Azure pomocí sady Visual Studio"
author: rick-anderson
description: "Zjistěte, jak publikovat aplikaci ASP.NET Core Azure App Service pomocí sady Visual Studio."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/16/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: e8de630c4fa8cff5395f7246b91384d4725e4ca6
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
<span data-ttu-id="6f0c8-104">/en-nám</span><span class="sxs-lookup"><span data-stu-id="6f0c8-104">/en-us</span></span>

# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="6f0c8-105">Publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f0c8-105">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="6f0c8-106">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), a [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="6f0c8-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="6f0c8-107">V tématu [publikovat do Azure ze sady Visual Studio pro Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) při práci na macu.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-107">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="6f0c8-108">Nastavení</span><span class="sxs-lookup"><span data-stu-id="6f0c8-108">Set up</span></span>

* <span data-ttu-id="6f0c8-109">Otevřete [bezplatný účet Azure](https://aka.ms/K5y5yh) Pokud nemáte jeden.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-109">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="6f0c8-110">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="6f0c8-110">Create a web app</span></span>

<span data-ttu-id="6f0c8-111">Ve Visual Studio – úvodní stránka, vyberte **soubor > Nový > projekt...**</span><span class="sxs-lookup"><span data-stu-id="6f0c8-111">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![nabídka Soubor](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="6f0c8-113">Dokončení **nový projekt** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="6f0c8-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="6f0c8-114">V levém podokně vyberte **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-114">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="6f0c8-115">V prostředním podokně vyberte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-115">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="6f0c8-116">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-116">Select **OK**.</span></span>

![Dialogové okno Nový projekt](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="6f0c8-118">V **nové webové aplikace ASP.NET Core** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="6f0c8-118">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="6f0c8-119">Vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-119">Select **Web Application**.</span></span>
* <span data-ttu-id="6f0c8-120">Vyberte **změnit ověřování**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-120">Select **Change Authentication**.</span></span>

![Dialogové okno Nový projekt](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="6f0c8-122">**Změna ověřování** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-122">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="6f0c8-123">Vyberte **jednotlivých uživatelských účtů**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-123">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="6f0c8-124">Vyberte **OK** se vrátíte do **nové webové aplikace ASP.NET Core**, pak vyberte **OK** znovu.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-124">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Dialogové okno Nový ASP.NET Web základní ověřování](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="6f0c8-126">Visual Studio vytvoří řešení.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-126">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="6f0c8-127">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="6f0c8-127">Run the app</span></span>

* <span data-ttu-id="6f0c8-128">Stisknutím kombinace kláves CTRL + F5 a spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-128">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="6f0c8-129">Testovací **o** a **kontaktujte** odkazy.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-129">Test the **About** and **Contact** links.</span></span>

![Webové aplikace otevřete v Microsoft Edge na místním hostiteli](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="6f0c8-131">Registrace uživatele</span><span class="sxs-lookup"><span data-stu-id="6f0c8-131">Register a user</span></span>

* <span data-ttu-id="6f0c8-132">Vyberte **zaregistrovat** a registraci nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-132">Select **Register** and register a new user.</span></span> <span data-ttu-id="6f0c8-133">Můžete použít fiktivní e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-133">You can use a fictitious email address.</span></span> <span data-ttu-id="6f0c8-134">Při odesílání, na stránce se zobrazí chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="6f0c8-134">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="6f0c8-135">*"Vnitřní chyba serveru: Databázová operace se nezdařila při zpracování požadavku. Výjimky SQL: databázi nelze otevřít. Použití existující migrace pro kontext databáze aplikace může tento problém vyřešit."*</span><span class="sxs-lookup"><span data-stu-id="6f0c8-135">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="6f0c8-136">Vyberte **použít migrace** a jakmile se aktualizace stránky, aktualizujte stránku.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-136">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Vnitřní chyba serveru: Databázová operace se nezdařila při zpracování požadavku.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="6f0c8-140">Aplikace zobrazí e-mail použitý k registraci nového uživatele a **odhlášení** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-140">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Webové aplikace, otevřete v Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="6f0c8-143">Nasazení aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="6f0c8-143">Deploy the app to Azure</span></span>

<span data-ttu-id="6f0c8-144">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **publikování...** .</span><span class="sxs-lookup"><span data-stu-id="6f0c8-144">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Kontextové nabídky otevřete se zvýrazněnou odkaz publikování](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="6f0c8-146">V **publikovat** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="6f0c8-146">In the **Publish** dialog:</span></span>

* <span data-ttu-id="6f0c8-147">Vyberte **služby Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-147">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="6f0c8-148">Vyberte ikonu zařízení a pak vyberte **vytvořit profil**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-148">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="6f0c8-149">Vyberte **vytvořit profil**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-149">Select **Create Profile**.</span></span>

![Dialogové okno publikování](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="6f0c8-151">Vytváření prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="6f0c8-151">Create Azure resources</span></span>

<span data-ttu-id="6f0c8-152">**Vytvořit službu App Service** otevře se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="6f0c8-152">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="6f0c8-153">Zadejte vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-153">Enter your subscription.</span></span>
* <span data-ttu-id="6f0c8-154">**Název aplikace**, **skupiny prostředků**, a **plán služby App Service** vstupní pole se vyplní.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-154">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="6f0c8-155">Můžete ponechat tyto názvy nebo změnit.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-155">You can keep these names or change them.</span></span>

![Dialogovém okně App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="6f0c8-157">Vyberte **služby** a vytvořit novou databázi.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-157">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="6f0c8-158">Vyberte zeleným  **+**  vytvořte novou databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-158">Select the green **+** icon to create a new SQL Database</span></span>

![Novou databázi SQL.](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="6f0c8-160">Vyberte **nové...**  na **nakonfigurovat databázi SQL** dialogovém okně můžete vytvořit novou databázi.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-160">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Nové databáze SQL a serveru](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="6f0c8-162">**Nakonfigurujte systém SQL Server** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-162">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="6f0c8-163">Zadejte uživatelské jméno správce a heslo a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-163">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="6f0c8-164">Můžete ponechat výchozí **název serveru**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-164">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="6f0c8-165">"admin" není povolen jako uživatelské jméno správce.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-165">"admin" is not allowed as the administrator user name.</span></span>

![Konfigurace systému SQL Server dialogové okno](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="6f0c8-167">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-167">Select **OK**.</span></span>

<span data-ttu-id="6f0c8-168">Visual Studio vrátí **vytvořit službu App Service** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-168">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="6f0c8-169">Vyberte **vytvořit** na **vytvořit službu App Service** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-169">Select **Create** on the **Create App Service** dialog.</span></span>

![Konfigurace dialogové okno databáze SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="6f0c8-171">Visual Studio vytvoří webovou aplikaci a SQL Server na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-171">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="6f0c8-172">Tento krok může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-172">This step can take a few minutes.</span></span> <span data-ttu-id="6f0c8-173">Informace o prostředky vytvořené v tématu [dalších prostředků](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="6f0c8-173">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="6f0c8-174">Po dokončení nasazení, vyberte **nastavení**:</span><span class="sxs-lookup"><span data-stu-id="6f0c8-174">When deployment completes, select **Settings**:</span></span>

![Konfigurace systému SQL Server dialogové okno](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="6f0c8-176">Na **nastavení** stránky **publikovat** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="6f0c8-176">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="6f0c8-177">Rozbalte položku **databáze** a zkontrolujte **použít tento připojovací řetězec za běhu**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-177">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="6f0c8-178">Rozbalte položku **Entity Framework migrace** a zkontrolujte **použít publikování této migrace na**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-178">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="6f0c8-179">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-179">Select **Save**.</span></span> <span data-ttu-id="6f0c8-180">Visual Studio vrátí **publikovat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-180">Visual Studio returns to the **Publish** dialog.</span></span> 

![Dialogové okno publikování: panel nastavení](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="6f0c8-182">Klikněte na tlačítko **publikování**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-182">Click **Publish**.</span></span> <span data-ttu-id="6f0c8-183">Visual Studio publishs vaší aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-183">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="6f0c8-184">Po dokončení depoyment aplikace se otevře v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-184">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="6f0c8-185">Testování aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="6f0c8-185">Test your app in Azure</span></span>

* <span data-ttu-id="6f0c8-186">Testovací **o** a **kontaktujte** odkazy</span><span class="sxs-lookup"><span data-stu-id="6f0c8-186">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="6f0c8-187">Registrace nového uživatele</span><span class="sxs-lookup"><span data-stu-id="6f0c8-187">Register a new user</span></span>

![Webová aplikace otevřít v Microsoft Edge ve službě Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="6f0c8-189">Aktualizace aplikace</span><span class="sxs-lookup"><span data-stu-id="6f0c8-189">Update the app</span></span>

* <span data-ttu-id="6f0c8-190">Upravit *Pages/About.cshtml* Razor stránky a změňte jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-190">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="6f0c8-191">Například můžete upravit odstavce. Tím vyjádříte "Hello ASP.NET Core!":[!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="6f0c8-191">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="6f0c8-192">Klikněte pravým tlačítkem na projekt a vyberte **publikování...**  znovu.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-192">Right-click on the project and select **Publish...** again.</span></span>

![Kontextové nabídky otevřete se zvýrazněnou odkaz publikování](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="6f0c8-194">Po publikování aplikace, ověřte, zda jsou k dispozici v Azure provedené změny.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-194">After the app is published, verify the changes you made are available on Azure.</span></span>

![Ověřte, že úkol je dokončen](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="6f0c8-196">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="6f0c8-196">Clean up</span></span>

<span data-ttu-id="6f0c8-197">Po dokončení testování aplikace, přejděte na [portál Azure](https://portal.azure.com/) a odstraňte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-197">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="6f0c8-198">Vyberte **skupiny prostředků**, pak vyberte skupinu prostředků, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-198">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure Portal: Skupiny prostředků v nabídce bočním panelu](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="6f0c8-200">V **skupiny prostředků** vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-200">In the **Resource groups** page, select **Delete**.</span></span>

![Portálu Azure: Skupiny prostředků stránky](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="6f0c8-202">Zadejte název skupiny prostředků a vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-202">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="6f0c8-203">Vaše aplikace a všechny další prostředky, které jsou vytvořené v tomto kurzu se teď odstraní z Azure.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-203">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="6f0c8-204">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f0c8-204">Next steps</span></span>

* [<span data-ttu-id="6f0c8-205">Průběžné nasazování do Azure pomocí sady Visual Studio a Git</span><span class="sxs-lookup"><span data-stu-id="6f0c8-205">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="6f0c8-206">Dalších prostředků</span><span class="sxs-lookup"><span data-stu-id="6f0c8-206">Additonal resources</span></span>

* [<span data-ttu-id="6f0c8-207">Aplikační služba Azure</span><span class="sxs-lookup"><span data-stu-id="6f0c8-207">Azure App Service</span></span>](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="6f0c8-208">Skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="6f0c8-208">Azure resource groups</span></span>](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="6f0c8-209">Databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="6f0c8-209">Azure SQL Database</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/)