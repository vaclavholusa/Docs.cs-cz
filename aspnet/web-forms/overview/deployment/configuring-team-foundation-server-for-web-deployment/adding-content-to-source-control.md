---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Přidávání obsahu do správy zdrojového kódu | Dokumentace Microsoftu
author: jrjlee
description: Toto téma vysvětluje, jak přidat obsah do správy zdrojového kódu v Team Foundation Server (TFS) 2010. Popisuje postup přidání řešení a projektů do týmu proje...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: b4cbe16915919bcdbabcc3f9769beb15720af5eb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362992"
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="86b83-104">Přidávání obsahu do správy zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="86b83-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="86b83-105">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="86b83-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="86b83-106">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="86b83-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="86b83-107">Toto téma vysvětluje, jak přidat obsah do správy zdrojového kódu v Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="86b83-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="86b83-108">Popisuje postup přidání řešení a projektů do týmového projektu v TFS, a vysvětluje, jak přidat externí závislosti, jako jsou rozhraní nebo sestavení do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="86b83-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="86b83-109">Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.</span><span class="sxs-lookup"><span data-stu-id="86b83-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="86b83-110">Přehled úloh</span><span class="sxs-lookup"><span data-stu-id="86b83-110">Task Overview</span></span>

<span data-ttu-id="86b83-111">Ve většině případů by měl být každý člen týmu vývojářů možnost přidávat obsah do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="86b83-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="86b83-112">Chcete-li přidat řešení do správy zdrojového kódu v sadě TFS, budete potřebovat k dokončení těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="86b83-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="86b83-113">Připojení k týmovému projektu.</span><span class="sxs-lookup"><span data-stu-id="86b83-113">Connect to a team project.</span></span>
- <span data-ttu-id="86b83-114">Struktura složek týmového projektu na serveru namapujte na strukturu složek v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="86b83-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="86b83-115">Přidáte do správy zdrojového kódu řešení a jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="86b83-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="86b83-116">Přidání externích závislostí do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="86b83-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="86b83-117">Toto téma se ukazují, jak provést tyto postupy.</span><span class="sxs-lookup"><span data-stu-id="86b83-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="86b83-118">Úlohy a názorné postupy v tomto tématu se předpokládá, že jste již vytvořili nového týmového projektu sady TFS spravovat obsah.</span><span class="sxs-lookup"><span data-stu-id="86b83-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="86b83-119">Další informace o vytvoření nového týmového projektu naleznete v tématu [vytvořením týmového projektu v TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="86b83-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="86b83-120">Kdo provádí tyto postupy?</span><span class="sxs-lookup"><span data-stu-id="86b83-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="86b83-121">Ve většině případů by měl být každý člen týmu vývojářů moct přidávat a upravovat obsah v rámci konkrétní týmové projekty.</span><span class="sxs-lookup"><span data-stu-id="86b83-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="86b83-122">Připojte k týmovému projektu a vytvořte mapování složky</span><span class="sxs-lookup"><span data-stu-id="86b83-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="86b83-123">Předtím, než přidáte veškerý obsah do správy zdrojových kódů, musíte připojit k týmovému projektu a vytvořte mapování mezi strukturu složek na serveru a systému souborů na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="86b83-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="86b83-124">**Připojení k týmovému projektu a přiřadit místní cesta**</span><span class="sxs-lookup"><span data-stu-id="86b83-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="86b83-125">Na vaši vývojářskou pracovní stanici otevřete Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="86b83-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="86b83-126">V sadě Visual Studio na **týmu** nabídky, klikněte na tlačítko **připojit k Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="86b83-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="86b83-127">Pokud jste už nakonfigurovali připojení k serveru TFS, můžete vynechat kroky 3 až 6.</span><span class="sxs-lookup"><span data-stu-id="86b83-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="86b83-128">V **připojení k týmovému projektu** dialogové okno, klikněte na tlačítko **servery**.</span><span class="sxs-lookup"><span data-stu-id="86b83-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="86b83-129">V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="86b83-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="86b83-130">V **přidat Team Foundation Server** dialogové okno, zadejte podrobnosti o instanci TFS a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="86b83-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="86b83-131">V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="86b83-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="86b83-132">V **připojit k týmovému projektu** dialogové okno, vyberte instanci TFS, kterou chcete připojit, vyberte tým projektu kolekce, vyberte týmový projekt, který chcete přidat do a pak klikněte na tlačítko **připojit**.</span><span class="sxs-lookup"><span data-stu-id="86b83-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="86b83-133">V **Team Exploreru** okna, rozbalte týmový projekt a potom dvakrát klikněte na **správy zdrojových kódů**.</span><span class="sxs-lookup"><span data-stu-id="86b83-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="86b83-134">Na **Průzkumníka správy zdrojového kódu** klikněte na tlačítko **nenamapované**.</span><span class="sxs-lookup"><span data-stu-id="86b83-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="86b83-135">V **mapy** v dialogu **místní složky** pole, přejděte do (nebo vytvoření) do místní složky vystupovat jako kořenová složka pro týmový projekt a potom klikněte na tlačítko **mapy**.</span><span class="sxs-lookup"><span data-stu-id="86b83-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="86b83-136">Po zobrazení výzvy ke stažení zdrojových souborů, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="86b83-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="86b83-137">V tomto okamžiku jste namapovali serverové složky pro týmový projekt do místní složky na vaši vývojářskou pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="86b83-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="86b83-138">Jakýkoli existující obsah jsme také stáhnout z týmového projektu do struktury vaší místní složky.</span><span class="sxs-lookup"><span data-stu-id="86b83-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="86b83-139">Teď můžete začít přidávat svůj obsah do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="86b83-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="86b83-140">Přidat projekty a řešení do správy zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="86b83-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="86b83-141">Chcete-li přidat projekty a řešení do správy zdrojových kódů, musíte nejprve přesuňte je pro mapovanou složku pro týmový projekt na svém místním počítači.</span><span class="sxs-lookup"><span data-stu-id="86b83-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="86b83-142">Potom můžete zkontrolovat v obsahu k synchronizaci dodatky k serveru.</span><span class="sxs-lookup"><span data-stu-id="86b83-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="86b83-143">**K přidání projektů do správy zdrojového kódu**</span><span class="sxs-lookup"><span data-stu-id="86b83-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="86b83-144">Na vaši vývojářskou pracovní stanici přesuňte své projekty a řešení na příslušné místo v rámci struktury mapovaná složka pro týmový projekt.</span><span class="sxs-lookup"><span data-stu-id="86b83-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="86b83-145">Mnoho organizací bude mít upřednostňovaný způsob jak projekty a řešení by měly být uspořádány ve správě zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="86b83-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="86b83-146">Informace o tom, jak struktura složek najdete v tématu [How To: struktura Your složky správy zdrojového kódu v sadě Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="86b83-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="86b83-147">Otevřete řešení v sadě Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="86b83-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="86b83-148">V **Průzkumníka řešení** okna, klikněte pravým tlačítkem na řešení a potom klikněte na tlačítko **přidat řešení do správy zdrojových kódů**.</span><span class="sxs-lookup"><span data-stu-id="86b83-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="86b83-149">V některých případech může v závislosti na tom, jak vaše organizace ráda struktura obsahu na serveru TFS budete muset přidat projekty do správy zdrojových kódů jednotlivě a poskytuje jemněji odstupňovanou kontrolu nad uspořádání zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="86b83-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="86b83-150">Ověřte, že **Průzkumníka správy zdrojového kódu** karta zobrazuje obsah, který jste přidali v rámci struktury složky serveru pro týmový projekt.</span><span class="sxs-lookup"><span data-stu-id="86b83-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="86b83-151">**Průzkumníka správy zdrojového kódu** karta zobrazuje váš obsah s zobrazovat žádné další výzvy vzhledem k tomu, že jste přidali řešení pro mapovanou složku na místním systému souborů.</span><span class="sxs-lookup"><span data-stu-id="86b83-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="86b83-152">Pokud vaše řešení v nenamapovaného umístění, by vyzváni k zadání umístění složek na serveru TFS i vašeho místního systému souborů.</span><span class="sxs-lookup"><span data-stu-id="86b83-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="86b83-153">Na **Průzkumníka správy zdrojového kódu** kartě **složky** podokně klikněte pravým tlačítkem na týmový projekt (například **ContactManager**) a potom klikněte na tlačítko **vrátit se změnami Čekající změny**.</span><span class="sxs-lookup"><span data-stu-id="86b83-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="86b83-154">V **vrátit se změnami – zdrojové soubory** dialogové okno, zadejte komentář a potom klikněte na tlačítko **vrátit se změnami**.</span><span class="sxs-lookup"><span data-stu-id="86b83-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="86b83-155">V tomto okamžiku jste přidali řešení do správy zdrojového kódu v sadě TFS.</span><span class="sxs-lookup"><span data-stu-id="86b83-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="86b83-156">Přidat externí závislosti do správy zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="86b83-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="86b83-157">Když přidáte projekt nebo řešení do správy zdrojových kódů, všechny soubory a složky v projektu nebo řešení se také přidají.</span><span class="sxs-lookup"><span data-stu-id="86b83-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="86b83-158">Ale v mnoha případech, projekty a řešení využívají také externí závislosti, jako je místní sestavení, aby správně fungoval.</span><span class="sxs-lookup"><span data-stu-id="86b83-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="86b83-159">Budete muset přidat tyto prostředky do správy zdrojového kódu Team Build a ostatní členové týmu pro vývojáře, aby váš kód sestavení proběhne úspěšně.</span><span class="sxs-lookup"><span data-stu-id="86b83-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="86b83-160">Například strukturu složek pro ukázkové řešení Správce kontaktů obsahuje složku s názvem balíčky.</span><span class="sxs-lookup"><span data-stu-id="86b83-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="86b83-161">Tato položka obsahuje sestavení a různé Podpůrné prostředky pro ADO.NET Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="86b83-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="86b83-162">Složku packages není součástí řešení Správce kontaktů, ale řešení nebude úspěšně sestavit bez něj.</span><span class="sxs-lookup"><span data-stu-id="86b83-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="86b83-163">Povolit týmového sestavení k sestavení řešení, budete muset přidat složku balíčků do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="86b83-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="86b83-164">Zahrnutí složky packages je typický pro co se stane, když přidáte Entity Framework nebo podobné prostředky do řešení pomocí rozšíření NuGet pro Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="86b83-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="86b83-165">**K přidání obsahu mimo projekt do správy zdrojového kódu**</span><span class="sxs-lookup"><span data-stu-id="86b83-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="86b83-166">Zajistěte, aby byly položky, které chcete přidat (například složce balíčků) v příslušné umístění v rámci mapované složky na vašem místním systému souborů.</span><span class="sxs-lookup"><span data-stu-id="86b83-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="86b83-167">V sadě Visual Studio 2010 v **Team Exploreru** okna, rozbalte týmový projekt a potom dvakrát klikněte na **správy zdrojových kódů**.</span><span class="sxs-lookup"><span data-stu-id="86b83-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="86b83-168">Na **Průzkumníka správy zdrojového kódu** kartě **složky** podokně, vyberte složku, která obsahuje položky nebo položky, které chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="86b83-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="86b83-169">Klikněte na tlačítko **přidat položky do složky** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="86b83-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="86b83-170">V **přidat do správy zdrojových kódů** dialogového okna, vyberte složku nebo položky, které chcete přidat, a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="86b83-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="86b83-171">Na **vyloučené položky** kartu, vyberte všechny požadované položky, které jsou automaticky vyloučené (například sestavení) a potom klikněte na tlačítko **zahrnout položky**.</span><span class="sxs-lookup"><span data-stu-id="86b83-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="86b83-172">Na **položky, které chcete přidat** kartu, ověřte, zda jsou uvedeny všechny soubory, které chcete zahrnout a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="86b83-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="86b83-173">V **Průzkumníka správy zdrojového kódu** okna, klikněte na tlačítko **vrátit se změnami** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="86b83-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="86b83-174">V **vrátit se změnami – zdrojové soubory** dialogové okno, zadejte komentář a potom klikněte na tlačítko **vrátit se změnami**.</span><span class="sxs-lookup"><span data-stu-id="86b83-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="86b83-175">V tomto okamžiku jste přidali vnější závislosti pro vaše řešení do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="86b83-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="86b83-176">Závěr</span><span class="sxs-lookup"><span data-stu-id="86b83-176">Conclusion</span></span>

<span data-ttu-id="86b83-177">Toto téma popisuje, jak se připojit k týmovému projektu, zmapování struktury složky a přidat obsah do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="86b83-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="86b83-178">Další informace o tom, jak pracovat s položkami pod správou zdrojových kódů, najdete v části [pomocí správy verzí](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="86b83-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="86b83-179">Dalším tématu s názvem [konfigurace TFS Build Server pro nasazení webu](configuring-a-tfs-build-server-for-web-deployment.md), popisuje postup přípravy serveru TFS Team Build pro sestavení a nasazení vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="86b83-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="86b83-180">Další čtení</span><span class="sxs-lookup"><span data-stu-id="86b83-180">Further Reading</span></span>

<span data-ttu-id="86b83-181">Další informace o práci se správou zdrojového kódu v sadě TFS, naleznete v tématu [pomocí správy verzí](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="86b83-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="86b83-182">[Předchozí](creating-a-team-project-in-tfs.md)
> [další](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="86b83-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
