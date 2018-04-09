---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Nastavení řešení obraťte se na správce | Microsoft Docs
author: jrjlee
description: Toto téma popisuje, jak stáhnout a nakonfigurovat spustit místně na pracovní stanici vývojáře řešení obraťte se na správce.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: e8fb24f5b2d96d864d1aa6bc0f78644773de00ab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="c29c6-103">Nastavení řešení obraťte se na správce</span><span class="sxs-lookup"><span data-stu-id="c29c6-103">Setting Up the Contact Manager Solution</span></span>
====================
<span data-ttu-id="c29c6-104">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="c29c6-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="c29c6-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="c29c6-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="c29c6-106">Toto téma popisuje, jak stáhnout a nakonfigurovat spustit místně na pracovní stanici vývojáře řešení obraťte se na správce.</span><span class="sxs-lookup"><span data-stu-id="c29c6-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>


## <a name="system-requirements"></a><span data-ttu-id="c29c6-107">Požadavky na systém</span><span class="sxs-lookup"><span data-stu-id="c29c6-107">System Requirements</span></span>

<span data-ttu-id="c29c6-108">Chcete-li spustit místně řešení obraťte se na správce a provádět další úlohy popsané v tomto kurzu, budete potřebovat k instalaci tohoto softwaru na pracovní stanici developer:</span><span class="sxs-lookup"><span data-stu-id="c29c6-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="c29c6-109">Visual Studio 2010 Service Pack 1, Premium nebo Ultimate Edition</span><span class="sxs-lookup"><span data-stu-id="c29c6-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="c29c6-110">Internetová informační služba (IIS) 7.5 Express</span><span class="sxs-lookup"><span data-stu-id="c29c6-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="c29c6-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="c29c6-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="c29c6-112">Služba IIS nástroj pro nasazení webu (Web Deploy) 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="c29c6-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="c29c6-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="c29c6-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="c29c6-114">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="c29c6-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="c29c6-115">.NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="c29c6-115">.NET Framework 4</span></span>
- <span data-ttu-id="c29c6-116">.NET Framework 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="c29c6-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="c29c6-117">S výjimkou Visual Studio 2010 můžete stáhnout a nainstalovat nejnovější verze všech těchto produktů a součástí prostřednictvím [instalačního programu webové platformy](https://go.microsoft.com/?linkid=9805118).</span><span class="sxs-lookup"><span data-stu-id="c29c6-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="c29c6-118">Stažení a extrakci řešení</span><span class="sxs-lookup"><span data-stu-id="c29c6-118">Download and Extract the Solution</span></span>

<span data-ttu-id="c29c6-119">Obraťte se na správce ukázkovou aplikaci si můžete stáhnout z galerie kódů MSDN [zde](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span><span class="sxs-lookup"><span data-stu-id="c29c6-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="c29c6-120">Konfigurace a spuštění řešení</span><span class="sxs-lookup"><span data-stu-id="c29c6-120">Configure and Run the Solution</span></span>

<span data-ttu-id="c29c6-121">Pro konfiguraci a spuštění řešení obraťte se na správce na místním počítači, budete potřebovat k provedení těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="c29c6-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="c29c6-122">Pokud nemáte již, vytvořte místní databáze služby aplikace ASP.NET pomocí funkce správy členství a rolí povolené.</span><span class="sxs-lookup"><span data-stu-id="c29c6-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="c29c6-123">Upravování připojovacích řetězců v *web.config* soubory tak, aby odkazoval na místní instanci systému SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="c29c6-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="c29c6-124">Spuštění řešení ze sady Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="c29c6-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="c29c6-125">Zbývající část Tato část obsahuje další pokyny k dokončení těchto postupů.</span><span class="sxs-lookup"><span data-stu-id="c29c6-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="c29c6-126">**K vytvoření databáze aplikace služby**</span><span class="sxs-lookup"><span data-stu-id="c29c6-126">**To create the application services database**</span></span>

1. <span data-ttu-id="c29c6-127">Otevřete příkazový řádek sady Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="c29c6-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="c29c6-128">To uděláte, na **spustit** nabídky, přejděte na příkaz **všechny programy**, klikněte na tlačítko **Microsoft Visual Studio 2010**, klikněte na tlačítko **nástroje sady Visual Studio**a potom Klikněte na tlačítko **Visual Studio – příkazový řádek (2010)**.</span><span class="sxs-lookup"><span data-stu-id="c29c6-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="c29c6-129">Na příkazovém řádku zadejte tento příkaz a stiskněte klávesu Enter:</span><span class="sxs-lookup"><span data-stu-id="c29c6-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="c29c6-130">Použití **– C** přepínač tak, aby zadejte připojovací řetězec pro databázový server.</span><span class="sxs-lookup"><span data-stu-id="c29c6-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="c29c6-131">Použití **– A** přepínač tak, aby zadejte aplikaci služby funkce, které chcete přidat do databáze.</span><span class="sxs-lookup"><span data-stu-id="c29c6-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="c29c6-132">V takovém případě **m** označuje, že chcete přidat podporu pro zprostředkovatele členství a **r** označuje, že chcete přidat podporu pro roli správce.</span><span class="sxs-lookup"><span data-stu-id="c29c6-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="c29c6-133">Použití **– d** přepínač tak, aby zadejte název pro vaši databázi aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="c29c6-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="c29c6-134">Pokud tento přepínač vynecháte, nástroj vytvoří databázi s názvem výchozí **aspnetdb**.</span><span class="sxs-lookup"><span data-stu-id="c29c6-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="c29c6-135">Když databáze byla úspěšně vytvořena, příkazovém řádku se zobrazí potvrzení.</span><span class="sxs-lookup"><span data-stu-id="c29c6-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="c29c6-136">Další informace o aspnet\_regsql nástroj, najdete v části [ASP.NET nástroj pro registraci serveru SQL Server (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="c29c6-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>


<span data-ttu-id="c29c6-137">Dalším krokem je zajistit, že připojovací řetězce v řešení obraťte se na správce, přejděte na vaše místní instance systému SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="c29c6-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="c29c6-138">**Chcete-li aktualizovat připojovací řetězce**</span><span class="sxs-lookup"><span data-stu-id="c29c6-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="c29c6-139">Obraťte se na správce řešení otevřete v sadě Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="c29c6-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="c29c6-140">V **Průzkumníku řešení** okno, rozbalte **ContactManager.Mvc** projektu a potom dvakrát klikněte na **Web.config** uzlu.</span><span class="sxs-lookup"><span data-stu-id="c29c6-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c29c6-141">Tento projekt ContactManager.Mvc zahrnuje dvě *web.config* soubory.</span><span class="sxs-lookup"><span data-stu-id="c29c6-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="c29c6-142">Budete muset upravit soubor projektu.</span><span class="sxs-lookup"><span data-stu-id="c29c6-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="c29c6-143">V **connectionStrings** elementu, ověřte, že připojovací řetězec s názvem **ApplicationServices** odkazuje na vaše místní databáze aplikace služby ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c29c6-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="c29c6-144">V **Průzkumníku řešení** okno, rozbalte **ContactManager.Service** projektu a potom dvakrát klikněte na **Web.config** uzlu.</span><span class="sxs-lookup"><span data-stu-id="c29c6-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="c29c6-145">V **connectionStrings** element v připojovací řetězec s názvem **ContactManagerContext**, ověřte, zda **zdroj dat** je nastavena na vaše místní instanci systému SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="c29c6-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="c29c6-146">Nemusíte nic jiného v připojovacím řetězci změnit.</span><span class="sxs-lookup"><span data-stu-id="c29c6-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="c29c6-147">Všechny otevřené soubory uložte.</span><span class="sxs-lookup"><span data-stu-id="c29c6-147">Save all open files.</span></span>

<span data-ttu-id="c29c6-148">Teď byste měli být připravení spustit řešení obraťte se na správce na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="c29c6-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="c29c6-149">Pokud budete postupovat podle těchto kroků bez první vytvoření databázi aplikačních služeb ASP.NET databáze se vytvoří při prvním pokusu vytvořit uživatele.</span><span class="sxs-lookup"><span data-stu-id="c29c6-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="c29c6-150">Ale ruční vytvoření databáze vám dává mnohem větší kontrolu nad sadu funkce služby aplikace, do které chcete podporovat.</span><span class="sxs-lookup"><span data-stu-id="c29c6-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>


<span data-ttu-id="c29c6-151">**Ke spuštění řešení obraťte se na správce**</span><span class="sxs-lookup"><span data-stu-id="c29c6-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="c29c6-152">V sadě Visual Studio 2010 stisknutím klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="c29c6-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="c29c6-153">Internet Explorer spuštění a požadavky adresu URL aplikace, obraťte se na správce ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="c29c6-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="c29c6-154">Ve výchozím nastavení, zobrazí aplikace **všechny kontakty** stránky.</span><span class="sxs-lookup"><span data-stu-id="c29c6-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="c29c6-155">Přidejte několik kontakty a potom ověřte, že aplikace funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="c29c6-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="c29c6-156">Přejděte do `http://localhost:50114/Account/Register` (Upravit adresu URL, pokud máte aplikace na jiný port).</span><span class="sxs-lookup"><span data-stu-id="c29c6-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="c29c6-157">Přidejte uživatelské jméno, e-mailovou adresu a heslo a ověřte, že budete moct zaregistrovat účet úspěšně.</span><span class="sxs-lookup"><span data-stu-id="c29c6-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="c29c6-158">Přejděte do `http://localhost:50114/Account/LogOn` (Upravit adresu URL, pokud máte aplikace na jiný port).</span><span class="sxs-lookup"><span data-stu-id="c29c6-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="c29c6-159">Ověřte, že budete moci přihlásit pomocí účtu, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="c29c6-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="c29c6-160">Zavřete Internet Explorer, aby se ukončilo ladění.</span><span class="sxs-lookup"><span data-stu-id="c29c6-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="c29c6-161">Závěr</span><span class="sxs-lookup"><span data-stu-id="c29c6-161">Conclusion</span></span>

<span data-ttu-id="c29c6-162">Obraťte se na správce řešení v tomto okamžiku by měl být plně nakonfigurovaný, ke spuštění na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="c29c6-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="c29c6-163">Řešení můžete použít jako referenci, při práci prostřednictvím na další témata v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c29c6-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="c29c6-164">Dalším tématu [vysvětlení souboru projektu](understanding-the-project-file.md), vysvětluje, jak můžete použít vlastní soubory projektu Microsoft Build Engine (MSBuild) v rámci řešení obraťte se na správce pro řízení procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="c29c6-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c29c6-165">[Předchozí](the-contact-manager-solution.md)
> [další](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="c29c6-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
