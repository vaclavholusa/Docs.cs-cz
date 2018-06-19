---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Přidávání obsahu do správy zdrojového kódu | Microsoft Docs
author: jrjlee
description: Toto téma vysvětluje, jak přidávat obsah do správy zdrojového kódu v Team Foundation Server (TFS) 2010. Popisuje postup přidání řešení a projekty do týmu proje...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: c9c3a506d2745a6793661453a293732429d3e46e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890431"
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="01fd1-104">Přidávání obsahu do správy zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="01fd1-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="01fd1-105">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="01fd1-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="01fd1-106">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="01fd1-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="01fd1-107">Toto téma vysvětluje, jak přidávat obsah do správy zdrojového kódu v Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="01fd1-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="01fd1-108">Popisuje postup přidání řešení a projekty do týmového projektu v sadě TFS, a vysvětluje postup přidání externí závislosti, jako je rozhraní nebo sestavení do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="01fd1-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="01fd1-109">Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz řady používá ukázkové řešení&#x2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3, komunikaci Windows Služba Foundation (WCF) a projekt databáze.</span><span class="sxs-lookup"><span data-stu-id="01fd1-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="01fd1-110">Přehled úloh</span><span class="sxs-lookup"><span data-stu-id="01fd1-110">Task Overview</span></span>

<span data-ttu-id="01fd1-111">Ve většině případů by měl být každý člen týmu pro vývojáře přidávat obsah do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="01fd1-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="01fd1-112">Pokud chcete přidat řešení ke správě zdrojového kódu v sadě TFS, budete potřebovat k dokončení těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="01fd1-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="01fd1-113">Připojení k týmovému projektu.</span><span class="sxs-lookup"><span data-stu-id="01fd1-113">Connect to a team project.</span></span>
- <span data-ttu-id="01fd1-114">Struktura složek týmového projektu na serveru mapují strukturu složek v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="01fd1-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="01fd1-115">Přidáte řešení a její obsah do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="01fd1-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="01fd1-116">Přidáte žádné externí závislosti do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="01fd1-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="01fd1-117">Toto téma vám ukáže, jak k provedení těchto postupů.</span><span class="sxs-lookup"><span data-stu-id="01fd1-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="01fd1-118">Postupy v tomto tématu a úlohy Předpokládejme, že jste již vytvořili nového týmového projektu sady TFS ke správě vašeho obsahu.</span><span class="sxs-lookup"><span data-stu-id="01fd1-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="01fd1-119">Další informace o vytvoření nového týmového projektu, najdete v části [vytvoření týmového projektu v sadě TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="01fd1-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="01fd1-120">Kdo provádí tyto postupy?</span><span class="sxs-lookup"><span data-stu-id="01fd1-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="01fd1-121">Ve většině případů by měl být každý člen týmu pro vývojáře možnost přidávat a upravovat obsah v rámci konkrétní týmové projekty.</span><span class="sxs-lookup"><span data-stu-id="01fd1-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="01fd1-122">Připojení k týmovému projektu a vytvořte složku mapování</span><span class="sxs-lookup"><span data-stu-id="01fd1-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="01fd1-123">Než přidáte žádný obsah k řízení zdrojů, potřebujete připojení k týmovému projektu a vytvořte mapování mezi struktura složek na serveru a systému souborů na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="01fd1-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="01fd1-124">**K připojení k týmovému projektu a mapování místní cestu**</span><span class="sxs-lookup"><span data-stu-id="01fd1-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="01fd1-125">Na pracovní stanici developer otevřete Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="01fd1-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="01fd1-126">V sadě Visual Studio na **Team** nabídky, klikněte na tlačítko **připojit k serveru Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="01fd1-127">Pokud jste již nakonfigurovali připojení k serveru TFS, můžete vynechat kroky 3 až 6.</span><span class="sxs-lookup"><span data-stu-id="01fd1-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="01fd1-128">V **připojení k týmovému projektu** dialogové okno, klikněte na tlačítko **servery**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="01fd1-129">V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="01fd1-130">V **přidat Team Foundation Server** dialogové okno, zadejte podrobnosti o instanci sady TFS a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="01fd1-131">V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="01fd1-132">V **připojení k týmovému projektu** dialogové okno, vyberte instanci sady TFS, chcete připojit, vyberte tým projektu vyberte týmový projekt, který chcete přidat do, kolekce a pak klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="01fd1-133">V **Team Explorer** okně rozbalte vašemu týmovému projektu a potom dvakrát klikněte na **správy zdrojového kódu**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="01fd1-134">Na **Průzkumník správy zdrojového kódu** , klikněte na **není mapováno**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="01fd1-135">V **mapy** dialogu **místní složky** pole, přejděte do (nebo ji vytvořte) do místní složky fungovat jako kořenová složka pro týmový projekt, a pak klikněte na **mapy**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="01fd1-136">Když se zobrazí výzva ke stažení zdrojových souborů, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="01fd1-137">V tomto okamžiku jste namapovali serverové složky pro týmový projekt do místní složky na pracovní stanici developer.</span><span class="sxs-lookup"><span data-stu-id="01fd1-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="01fd1-138">Existující obsah si také stáhnout z týmový projekt na strukturu místní složky.</span><span class="sxs-lookup"><span data-stu-id="01fd1-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="01fd1-139">Nyní můžete spustit přidat vlastní obsah do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="01fd1-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="01fd1-140">Přidat projekty a řešení do správy zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="01fd1-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="01fd1-141">Pokud chcete přidat do správy zdrojového kódu projekty a řešení, musíte nejprve přesunout do mapované složky pro týmový projekt na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="01fd1-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="01fd1-142">Potom můžete zkontrolovat obsah dodatky synchronizovat se serverem.</span><span class="sxs-lookup"><span data-stu-id="01fd1-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="01fd1-143">**K přidání projektů do správy zdrojového kódu**</span><span class="sxs-lookup"><span data-stu-id="01fd1-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="01fd1-144">Na pracovní stanici developer přesuňte do příslušného umístění v rámci strukturu mapované složky pro týmový projekt projekty a řešení.</span><span class="sxs-lookup"><span data-stu-id="01fd1-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="01fd1-145">Mnoho organizací bude mít žádoucí pro projekty a řešení uspořádání ve správě zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="01fd1-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="01fd1-146">Informace o tom, jak struktura složek, najdete v části [postupy: struktura si zdroj ovládacího prvku složek na serveru Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="01fd1-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="01fd1-147">Otevřete řešení v sadě Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="01fd1-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="01fd1-148">V **Průzkumníku řešení** oken, klikněte pravým tlačítkem na řešení a pak klikněte na tlačítko **přidat řešení správy zdrojového kódu**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="01fd1-149">V některých případech, v závislosti na tom, jak vaše organizace Neradi struktura obsahu v sadě TFS musíte k přidání projektů do správy zdrojového jednotlivě a poskytují jemně odstupňovanou kontrolu nad uspořádání vašeho zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="01fd1-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="01fd1-150">Ověřte, zda **Průzkumník správy zdrojového kódu** karta zobrazuje obsah, který jste přidali v rámci struktury složek serveru pro týmový projekt.</span><span class="sxs-lookup"><span data-stu-id="01fd1-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="01fd1-151">**Průzkumník správy zdrojového kódu** karta zobrazuje svůj obsah pomocí žádné další výzvy, protože jste přidali řešení do mapované složky v místním systému souborů.</span><span class="sxs-lookup"><span data-stu-id="01fd1-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="01fd1-152">Pokud vaše řešení v nenamapovaný umístění, by vyzváni k zadání umístění složek v sadě TFS a do místního systému souborů.</span><span class="sxs-lookup"><span data-stu-id="01fd1-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="01fd1-153">Na **Průzkumník správy zdrojového kódu** ve **složky** podokně klikněte pravým tlačítkem na týmový projekt (například **ContactManager**) a pak klikněte na tlačítko **vrátit se změnami Změny čekající na zpracování**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="01fd1-154">V **změnami – zdrojové soubory** dialogové okno, zadejte komentář a potom klikněte na **změnami**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="01fd1-155">V tomto okamžiku jste přidali řešení ke správě zdrojového kódu v sadě TFS.</span><span class="sxs-lookup"><span data-stu-id="01fd1-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="01fd1-156">Přidat externí závislosti do správy zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="01fd1-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="01fd1-157">Když přidáte do správy zdrojového kódu projekt nebo řešení, všechny soubory a složky v projekt nebo řešení bude také přidán.</span><span class="sxs-lookup"><span data-stu-id="01fd1-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="01fd1-158">Ale v mnoha případech, projekty a řešení také spoléhají na externí závislosti, stejně jako místní sestavení, aby správně fungoval.</span><span class="sxs-lookup"><span data-stu-id="01fd1-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="01fd1-159">Je nutné přidat, že tyto prostředky k řízení zdrojů umožníte Team Build i ostatní členové týmu pro vývojáře sestavení kódu úspěšně.</span><span class="sxs-lookup"><span data-stu-id="01fd1-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="01fd1-160">Struktura složek pro ukázkové řešení obraťte se na správce například obsahuje složku s názvem balíčky.</span><span class="sxs-lookup"><span data-stu-id="01fd1-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="01fd1-161">Tato položka obsahuje sestavení a různé doprovodné materiály pro ADO.NET Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="01fd1-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="01fd1-162">Balíčky složka není součástí řešení obraťte se na správce, ale nebude řešení úspěšně sestavení bez.</span><span class="sxs-lookup"><span data-stu-id="01fd1-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="01fd1-163">Povolit Team Build sestavte řešení, musíte přidat složku balíčky do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="01fd1-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="01fd1-164">Zahrnutí do složky balíčků je typický pro co se stane, když přidáte Entity Framework nebo podobné prostředky, vaše řešení pomocí rozšíření NuGet pro Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="01fd1-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="01fd1-165">**Přidání obsahu jiný projekt do správy zdrojového kódu**</span><span class="sxs-lookup"><span data-stu-id="01fd1-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="01fd1-166">Zajistěte, aby položky, které chcete přidat (například složce balíčků) do příslušného umístění v rámci mapované složky v místním systému souborů.</span><span class="sxs-lookup"><span data-stu-id="01fd1-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="01fd1-167">V sadě Visual Studio 2010 v **Team Explorer** okně rozbalte vašemu týmovému projektu a potom dvakrát klikněte na **správy zdrojového kódu**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="01fd1-168">Na **Průzkumník správy zdrojového kódu** ve **složky** podokně, vyberte složku, která obsahuje položky nebo položky, které chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="01fd1-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="01fd1-169">Klikněte **přidat položky do složky** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="01fd1-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="01fd1-170">V **přidat do správy zdrojového kódu** dialogové okno, vyberte složku nebo položky, které chcete přidat, a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="01fd1-171">Na **vyloučit položky** , vyberte všechny požadované položky, které byly automaticky vyloučeny (například sestavení) a potom klikněte na **zahrnout položky**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="01fd1-172">Na **položky k přidání** kartě, ověřte, zda jsou uvedeny všechny soubory, které chcete zahrnout a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="01fd1-173">V **Průzkumník správy zdrojového kódu** okně klikněte na tlačítko **změnami** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="01fd1-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="01fd1-174">V **změnami – zdrojové soubory** dialogové okno, zadejte komentář a potom klikněte na **změnami**.</span><span class="sxs-lookup"><span data-stu-id="01fd1-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="01fd1-175">V tomto okamžiku jste přidali vnější závislosti pro vaše řešení do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="01fd1-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="01fd1-176">Závěr</span><span class="sxs-lookup"><span data-stu-id="01fd1-176">Conclusion</span></span>

<span data-ttu-id="01fd1-177">Toto téma popisuje postup připojení k týmovému projektu, mapy strukturu složek a přidání obsahu do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="01fd1-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="01fd1-178">Další informace o tom, jak pracovat s položkami ve správě zdrojového kódu najdete v tématu [pomocí verzí](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="01fd1-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="01fd1-179">Dalším tématu [konfigurace serveru TFS sestavení pro nasazení webu](configuring-a-tfs-build-server-for-web-deployment.md), popisuje postup přípravy serveru TFS Team Build k vytváření a nasazování řešení.</span><span class="sxs-lookup"><span data-stu-id="01fd1-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="01fd1-180">Další čtení</span><span class="sxs-lookup"><span data-stu-id="01fd1-180">Further Reading</span></span>

<span data-ttu-id="01fd1-181">Další informace o práci se správa zdrojového kódu v sadě TFS naleznete v tématu [pomocí verzí](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="01fd1-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="01fd1-182">[Předchozí](creating-a-team-project-in-tfs.md)
> [další](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="01fd1-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
