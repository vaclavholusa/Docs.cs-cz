---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Vytvoření týmového projektu v sadě TFS | Microsoft Docs
author: jrjlee
description: Toto téma popisuje postup vytvoření nového týmového projektu v Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 79c069a601c0eafd84ae142241895428052acd29
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880424"
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="8bff7-103">Vytvoření týmového projektu v sadě TFS</span><span class="sxs-lookup"><span data-stu-id="8bff7-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="8bff7-104">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="8bff7-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="8bff7-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="8bff7-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="8bff7-106">Toto téma popisuje postup vytvoření nového týmového projektu v Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="8bff7-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="8bff7-107">Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz řady používá ukázkové řešení&#x2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3, komunikaci Windows Služba Foundation (WCF) a projekt databáze.</span><span class="sxs-lookup"><span data-stu-id="8bff7-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="8bff7-108">Přehled úloh</span><span class="sxs-lookup"><span data-stu-id="8bff7-108">Task Overview</span></span>

<span data-ttu-id="8bff7-109">Zřídit a použití nového týmového projektu v sadě TFS, budete potřebovat k dokončení těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="8bff7-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="8bff7-110">Udělení oprávnění pro uživatele, který se vytvoří nový týmový projekt.</span><span class="sxs-lookup"><span data-stu-id="8bff7-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="8bff7-111">Vytvoření týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="8bff7-111">Create the team project.</span></span>
- <span data-ttu-id="8bff7-112">Udělení oprávnění pro členy týmu, kteří budou pracovat na projekt.</span><span class="sxs-lookup"><span data-stu-id="8bff7-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="8bff7-113">Zkontrolujte, že v některých obsahu.</span><span class="sxs-lookup"><span data-stu-id="8bff7-113">Check in some content.</span></span>

<span data-ttu-id="8bff7-114">Toto téma vám ukáže, jak k provedení těchto postupů, a určí uživatele a role úlohy, které by mohly být zodpovědná za každý postup.</span><span class="sxs-lookup"><span data-stu-id="8bff7-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="8bff7-115">Mějte na paměti, že v závislosti na struktuře vaší organizace, každý z těchto úloh může být jiné osoba.</span><span class="sxs-lookup"><span data-stu-id="8bff7-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="8bff7-116">Úlohy a postupy v tomto tématu předpokládá, že jste nainstalován a nakonfigurován sady TFS a že jste vytvořili kolekci týmových projektů jako součást procesu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8bff7-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="8bff7-117">Další informace o těchto domněnek a pro obecné informace na scénáři, najdete v části [konfigurace serveru TFS sestavení pro nasazení webu](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="8bff7-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="8bff7-118">Udělení oprávnění pro tvůrce týmového projektu</span><span class="sxs-lookup"><span data-stu-id="8bff7-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="8bff7-119">Chcete-li vytvořit nového týmového projektu, potřebujete tato oprávnění:</span><span class="sxs-lookup"><span data-stu-id="8bff7-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="8bff7-120">Musíte mít **vytvořit nové projekty** oprávnění v aplikační vrstvě TFS.</span><span class="sxs-lookup"><span data-stu-id="8bff7-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="8bff7-121">Toto oprávnění přidáním uživatelům udělíte obvykle **správci kolekcí projektů** skupiny sady TFS.</span><span class="sxs-lookup"><span data-stu-id="8bff7-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="8bff7-122">**Team Foundation správci** toto oprávnění zahrnuje globální skupinu.</span><span class="sxs-lookup"><span data-stu-id="8bff7-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="8bff7-123">Musíte mít oprávnění k vytvoření nové týmové weby v rámci kolekce webů služby SharePoint, která odpovídá kolekci týmového projektu sady TFS.</span><span class="sxs-lookup"><span data-stu-id="8bff7-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="8bff7-124">Toto oprávnění přidáním uživatele do skupiny služby SharePoint s udělíte obvykle **úplné řízení** kolekce webů oprávnění na webu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="8bff7-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="8bff7-125">Pokud používáte funkce SQL Server Reporting Services, musíte být členem skupiny **Team Foundation Content Manager** role ve službě Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="8bff7-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="8bff7-126">Kdo provádí tyto postupy?</span><span class="sxs-lookup"><span data-stu-id="8bff7-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="8bff7-127">Obvykle osoba nebo skupina, který spravuje nasazení TFS také provede tyto postupy.</span><span class="sxs-lookup"><span data-stu-id="8bff7-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="8bff7-128">Protože to je vysoce privilegované sadu oprávnění, nové týmové projekty obvykle vytvářejí malou podmnožinu uživatelů s zodpovědnost za správu nasazení sady TFS.</span><span class="sxs-lookup"><span data-stu-id="8bff7-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="8bff7-129">Vývojáři obvykle neposkytne oprávnění potřebná k vytvoření nového týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="8bff7-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="8bff7-130">Udělení oprávnění v sadě TFS</span><span class="sxs-lookup"><span data-stu-id="8bff7-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="8bff7-131">Pokud chcete povolit uživateli vytvoření nového týmového projektu, první úloh vysoké úrovně je přidání uživateli **správci kolekcí projektů** pro kolekci týmových projektů.</span><span class="sxs-lookup"><span data-stu-id="8bff7-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="8bff7-132">**Přidání uživatele do skupiny Administrators kolekci projektu**</span><span class="sxs-lookup"><span data-stu-id="8bff7-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="8bff7-133">Na serveru TFS na **spustit** nabídky, přejděte na příkaz **všechny programy**, klikněte na tlačítko **Microsoft Team Foundation Server 2010**a potom klikněte na **Team Foundation Konzole pro správu**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="8bff7-134">Ve stromovém zobrazení navigace, rozbalte položku **aplikační vrstvě**a potom klikněte na **kolekcemi týmových projektů**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="8bff7-135">V **kolekcemi týmových projektů** podokně, vyberte tým projektu kolekce, kterou chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="8bff7-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="8bff7-136">Na **Obecné** , klikněte na **členství ve skupině**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="8bff7-137">V **globální skupiny** dialogové okno, vyberte **správci kolekcí projektů** skupiny a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="8bff7-138">V **vlastnosti Team Foundation Server skupiny** dialogové okno, vyberte **Windows uživatele nebo skupinu**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="8bff7-139">V **vybrat uživatele, počítače nebo skupiny** dialogové okno, zadejte uživatelské jméno uživatele, kterou chcete k vytvoření nového týmového projektu, klikněte na **Kontrola názvů**a potom klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="8bff7-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="8bff7-140">V **vlastnosti Team Foundation Server skupiny** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="8bff7-141">V **globální skupiny** dialogové okno, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="8bff7-142">Udělení oprávnění ve službě SharePoint Services</span><span class="sxs-lookup"><span data-stu-id="8bff7-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="8bff7-143">Dále musíte poskytnout oprávnění uživatele k vytvoření nových lokalit týmu v kolekce webů služby SharePoint, která odpovídá vaší kolekce týmového projektu sady TFS.</span><span class="sxs-lookup"><span data-stu-id="8bff7-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="8bff7-144">**Udělit oprávnění k úplnému řízení na kolekce webů služby SharePoint**</span><span class="sxs-lookup"><span data-stu-id="8bff7-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="8bff7-145">V konzole správy pro Team Foundation Server na **kolekcemi týmových projektů** vyberte kolekce týmových projektů, které chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="8bff7-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="8bff7-146">Na **web služby SharePoint** kartě, poznamenejte si hodnotu **aktuální výchozí umístění lokality** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="8bff7-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="8bff7-147">Otevřete Internet Explorer a přejděte na adresu URL, které jste si poznamenali v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="8bff7-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8bff7-148">Pokud nejste přihlášení k systému Windows jako uživatel, který vytvořil kolekce týmového projektu, budete muset přihlásit do služby SharePoint jako tento uživatel aby bylo možné pokračovat.</span><span class="sxs-lookup"><span data-stu-id="8bff7-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="8bff7-149">Na **Akce webu** nabídky, klikněte na tlačítko **nastavení lokality**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="8bff7-150">Na **nastavení lokality** v části **uživatele a oprávnění**, klikněte na tlačítko **lidé a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="8bff7-151">V levém navigačním panelu klikněte na tlačítko **skupiny**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="8bff7-152">Na **lidé a skupiny: všechny skupiny** klikněte na tlačítko **nastavit skupiny pro tento web**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="8bff7-153">Může se zobrazit <strong>HTTP 404 nebyl nalezen</strong> chyba z důvodu dvojité kódování chyb HTTP.</span><span class="sxs-lookup"><span data-stu-id="8bff7-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="8bff7-154">Pokud k tomu dojde, nahraďte adresu URL s tímto:</span><span class="sxs-lookup"><span data-stu-id="8bff7-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="8bff7-155">[<em>URL kolekce webů</em>] /\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="8bff7-155">[<em>site collection URL</em>]/\_layouts/permsetup.aspx</span></span>  
   > <span data-ttu-id="8bff7-156">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8bff7-156">For example:</span></span>  
   > <span data-ttu-id="8bff7-157">http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="8bff7-157">http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span></span>
8. <span data-ttu-id="8bff7-158">Na **nastavit skupiny pro tento web** přidejte uživatele, který vytvoří týmové projekty k **vlastníky** skupiny a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-158">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="8bff7-159">Další informace o povolení uživatelům vytvářet nové týmové projekty v rámci kolekce týmového projektu, najdete v části [nastavit oprávnění správce pro kolekce týmových projektů](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="8bff7-159">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="8bff7-160">Vytvoření nového týmového projektu a přidání uživatelů</span><span class="sxs-lookup"><span data-stu-id="8bff7-160">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="8bff7-161">Jakmile máte potřebná oprávnění, můžete použít **Team Explorer** oken v sadě Visual Studio 2010 pro vytvoření nového týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="8bff7-161">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="8bff7-162">Tento přístup poskytuje průvodce, který shromažďuje všechny požadované informace a provede nezbytných úloh v sadě TFS, SharePoint a SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="8bff7-162">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="8bff7-163">Musíte také udělit oprávnění pro nový týmový projekt na členy týmu pro vývojáře, aby se mohly přidávat a upravovat obsah.</span><span class="sxs-lookup"><span data-stu-id="8bff7-163">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="8bff7-164">Kdo provádí tyto postupy?</span><span class="sxs-lookup"><span data-stu-id="8bff7-164">Who Performs These Procedures?</span></span>

<span data-ttu-id="8bff7-165">Obvykle správce sady TFS nebo vedoucí týmu vývojáře provede tyto postupy.</span><span class="sxs-lookup"><span data-stu-id="8bff7-165">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="8bff7-166">Vytvoření nového týmového projektu</span><span class="sxs-lookup"><span data-stu-id="8bff7-166">Create a New Team Project</span></span>

<span data-ttu-id="8bff7-167">Následující postup popisuje vytvoření nového týmového projektu v sadě TFS 2010.</span><span class="sxs-lookup"><span data-stu-id="8bff7-167">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="8bff7-168">**K vytvoření nového týmového projektu**</span><span class="sxs-lookup"><span data-stu-id="8bff7-168">**To create a new team project**</span></span>

1. <span data-ttu-id="8bff7-169">Na **spustit** nabídky, přejděte na příkaz **všechny programy**, klikněte na tlačítko **Microsoft Visual Studio 2010**, klikněte pravým tlačítkem na **Microsoft Visual Studio 2010**, a pak klikněte na **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-169">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8bff7-170">Pokud sadu Visual Studio 2010 není spustit jako správce, Průvodce novým týmovým projektem na poslední krok selžou.</span><span class="sxs-lookup"><span data-stu-id="8bff7-170">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="8bff7-171">Pokud **řízení uživatelských účtů** se zobrazí dialogové okno, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-171">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="8bff7-172">V sadě Visual Studio na **Team** nabídky, klikněte na tlačítko **připojit k serveru Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-172">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8bff7-173">Pokud jste již nakonfigurovali připojení k serveru TFS, můžete vynechat kroky 4 až 7.</span><span class="sxs-lookup"><span data-stu-id="8bff7-173">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="8bff7-174">V **připojení k týmovému projektu** dialogové okno, klikněte na tlačítko **servery**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-174">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="8bff7-175">V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-175">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="8bff7-176">V **přidat Team Foundation Server** dialogové okno, zadejte podrobnosti o instanci sady TFS a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-176">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="8bff7-177">V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-177">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="8bff7-178">V **připojení k týmovému projektu** dialogové okno, vyberte kolekce, které chcete přidat a pak klikněte na projekt sady TFS instanci chcete připojit, vyberte tým **Connect**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-178">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="8bff7-179">V **Team Explorer** okna, klikněte pravým tlačítkem do týmového projektu kolekce a potom klikněte na **nového týmového projektu**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-179">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="8bff7-180">V **nového týmového projektu** dialogové okno, zadejte název a popis pro týmový projekt a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-180">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8bff7-181">Pokud váš týmový projekt obsahuje mezery, může mít některé potíže, když dojdete ke použít nástroj nasazení webu Internetové informační služby (IIS) (Web Deploy) nasazení balíčků z výstupní cesta.</span><span class="sxs-lookup"><span data-stu-id="8bff7-181">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="8bff7-182">Mezery v cestě může být mnohem obtížnější ke spuštění příkazů nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="8bff7-182">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="8bff7-183">Na **vyberte šablonu procesu** vyberte šablonu procesu, který chcete použít ke správě procesu vývoje a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-183">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8bff7-184">Další informace o šablony procesů pro TFS najdete v tématu [nástroje a šablony procesů](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="8bff7-184">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="8bff7-185">Na **nastavení lokality Team** stránky, ponechejte výchozí nastavení beze změny a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-185">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="8bff7-186">Toto nastavení se vytvoří nebo identifikuje, team Web služby SharePoint, který je přidružen týmového projektu sady TFS.</span><span class="sxs-lookup"><span data-stu-id="8bff7-186">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="8bff7-187">Váš vývojový tým mohou používat tuto lokalitu pro správu dokumentaci, účastnit diskuse, vytvářet wiki stránky a provádět různé úlohy, které nejsou v relaci ke kódu.</span><span class="sxs-lookup"><span data-stu-id="8bff7-187">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="8bff7-188">Další informace najdete v tématu [interakce mezi produkty SharePoint a serveru Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="8bff7-188">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="8bff7-189">Na **zadejte nastavení řízení zdroje** stránky, ponechejte výchozí nastavení beze změny a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-189">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="8bff7-190">Toto nastavení určuje, nebo vytvoří umístění v hierarchii složek sady TFS, který bude fungovat jako kořenová složka pro obsah.</span><span class="sxs-lookup"><span data-stu-id="8bff7-190">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="8bff7-191">Na **potvrďte nastavení týmového projektu** klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-191">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="8bff7-192">Pokud nový týmový projekt je úspěšně vytvořen, na **vytvoření týmového projektu** klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-192">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="8bff7-193">Přidání uživatelů do týmového projektu</span><span class="sxs-lookup"><span data-stu-id="8bff7-193">Add Users to a Team Project</span></span>

<span data-ttu-id="8bff7-194">Teď, když jste vytvořili nový týmový projekt, můžete udělit oprávnění uživatelům, aby se mohly spustit přidávání a spolupráce na obsah.</span><span class="sxs-lookup"><span data-stu-id="8bff7-194">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="8bff7-195">**K přidání uživatele do týmového projektu**</span><span class="sxs-lookup"><span data-stu-id="8bff7-195">**To add users to a team project**</span></span>

1. <span data-ttu-id="8bff7-196">V sadě Visual Studio 2010 v **Team Explorer** okna, klikněte pravým tlačítkem na týmový projekt, přejděte na **nastavení projektu Team**a potom klikněte na **členství ve skupině**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-196">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="8bff7-197">Pokud chcete povolit uživatelům přidávat, upravovat a odebírat ve správě zdrojového kódu, je nutné přidat ji do **přispěvatelé** skupiny.</span><span class="sxs-lookup"><span data-stu-id="8bff7-197">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="8bff7-198">V **projektu skupiny** dialogové okno, vyberte **přispěvatelé** skupiny a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-198">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="8bff7-199">V **vlastnosti Team Foundation Server skupiny** dialogové okno, vyberte **Windows uživatele nebo skupinu**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-199">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="8bff7-200">V **vybrat uživatele, počítače nebo skupiny** dialogové okno, zadejte uživatelské jméno uživatele, které chcete přidat do týmového projektu, klikněte na **Kontrola názvů**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-200">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="8bff7-201">V **vlastnosti Team Foundation Server skupiny** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-201">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="8bff7-202">V **projektu skupiny** dialogové okno, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="8bff7-202">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="8bff7-203">Závěr</span><span class="sxs-lookup"><span data-stu-id="8bff7-203">Conclusion</span></span>

<span data-ttu-id="8bff7-204">V tomto okamžiku je připravený k použití nového týmového projektu a váš tým vývojáře můžete spustit přidávání obsahu a spolupráce v procesu vývoje.</span><span class="sxs-lookup"><span data-stu-id="8bff7-204">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="8bff7-205">Dalším tématu [přidávání obsahu do správy zdrojového kódu](adding-content-to-source-control.md), popisuje postup přidání obsahu do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="8bff7-205">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="8bff7-206">Další čtení</span><span class="sxs-lookup"><span data-stu-id="8bff7-206">Further Reading</span></span>

<span data-ttu-id="8bff7-207">Širší pokyny týkající se vytváření týmových projektů sady TFS najdete v tématu [vytvoření týmového projektu](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="8bff7-207">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="8bff7-208">Další informace o povolení uživatelům vytvářet nové týmové projekty v rámci kolekce týmového projektu, najdete v části [nastavit oprávnění správce pro kolekce týmových projektů](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="8bff7-208">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="8bff7-209">Další informace o přidávání uživatelů do týmových projektů najdete v tématu [přidání uživatelů do týmových projektů](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="8bff7-209">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8bff7-210">[Předchozí](configuring-team-foundation-server-for-web-deployment.md)
> [další](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="8bff7-210">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
