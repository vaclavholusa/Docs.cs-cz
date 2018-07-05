---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Vytvoření týmového projektu v TFS | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje postup vytvoření nového týmového projektu v Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 2c3b30cac408f47d7d15ae7456f0744219506c85
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397558"
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="df74b-103">Vytvoření týmového projektu v sadě TFS</span><span class="sxs-lookup"><span data-stu-id="df74b-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="df74b-104">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="df74b-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="df74b-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="df74b-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="df74b-106">Toto téma popisuje postup vytvoření nového týmového projektu v Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="df74b-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="df74b-107">Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.</span><span class="sxs-lookup"><span data-stu-id="df74b-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="df74b-108">Přehled úloh</span><span class="sxs-lookup"><span data-stu-id="df74b-108">Task Overview</span></span>

<span data-ttu-id="df74b-109">Zřízení a použití nového týmového projektu v TFS, budete potřebovat k dokončení těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="df74b-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="df74b-110">Udělení oprávnění pro uživatele, který se vytvoří nový týmový projekt.</span><span class="sxs-lookup"><span data-stu-id="df74b-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="df74b-111">Vytvořte týmový projekt.</span><span class="sxs-lookup"><span data-stu-id="df74b-111">Create the team project.</span></span>
- <span data-ttu-id="df74b-112">Udělení oprávnění členům týmu, kteří budou pracovat na projektu.</span><span class="sxs-lookup"><span data-stu-id="df74b-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="df74b-113">Vrátit se změnami nějaký obsah.</span><span class="sxs-lookup"><span data-stu-id="df74b-113">Check in some content.</span></span>

<span data-ttu-id="df74b-114">V tomto tématu ukazují, jak provést tyto postupy, a určí uživatele a úlohy, které by mohly být zodpovědný za každý postup.</span><span class="sxs-lookup"><span data-stu-id="df74b-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="df74b-115">Mějte na paměti, že v závislosti na struktuře vaší organizace, každá z těchto úloh může být odpovědnost na jinou osobu.</span><span class="sxs-lookup"><span data-stu-id="df74b-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="df74b-116">Úlohy a názorné postupy v tomto tématu se předpokládá, že je nainstalujete a nakonfigurujete TFS, a že jste vytvořili kolekci týmového projektu jako součást procesu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="df74b-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="df74b-117">Další informace o těchto domněnek a obecné informace na scénáři, najdete v části [nakonfigurovat Server sestavení TFS pro nasazení webu](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="df74b-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="df74b-118">Udělení oprávnění pro autora týmového projektu</span><span class="sxs-lookup"><span data-stu-id="df74b-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="df74b-119">Chcete-li vytvořit nový týmový projekt, je třeba tato oprávnění:</span><span class="sxs-lookup"><span data-stu-id="df74b-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="df74b-120">Musíte mít **vytvořit nové projekty** oprávnění na aplikační vrstvu TFS.</span><span class="sxs-lookup"><span data-stu-id="df74b-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="df74b-121">Obvykle tak, že přidáte uživatelům udělit toto oprávnění **Project Collection Administrators** skupinu TFS.</span><span class="sxs-lookup"><span data-stu-id="df74b-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="df74b-122">**Správci serveru Team Foundation** toto oprávnění zahrnuje globální skupina.</span><span class="sxs-lookup"><span data-stu-id="df74b-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="df74b-123">Musíte mít oprávnění k vytvoření nových lokalit týmu v rámci kolekce webů služby SharePoint, která odpovídá kolekci týmových projektů TFS.</span><span class="sxs-lookup"><span data-stu-id="df74b-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="df74b-124">Obvykle udělit tato oprávnění tak, že přidáte uživatele do skupiny SharePoint s **úplné řízení** kolekce webů oprávnění služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="df74b-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="df74b-125">Pokud používáte funkce SQL Server Reporting Services, musíte být členem skupiny **správce obsahu Team Foundation** role ve službě Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="df74b-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="df74b-126">Kdo provádí tyto postupy?</span><span class="sxs-lookup"><span data-stu-id="df74b-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="df74b-127">Obvykle osoba nebo skupina, která spravuje nasazení TFS také provádí tyto postupy.</span><span class="sxs-lookup"><span data-stu-id="df74b-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="df74b-128">Protože se jedná s vysokou úrovní oprávnění sadu oprávnění, nové týmové projekty jsou obvykle vytvořené malou podmnožinu uživatelů s zodpovědnost za správu nasazení TFS.</span><span class="sxs-lookup"><span data-stu-id="df74b-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="df74b-129">Vývojáři obvykle neposkytne oprávnění požadovaná k vytvoření nových týmových projektů.</span><span class="sxs-lookup"><span data-stu-id="df74b-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="df74b-130">Udělení oprávnění v TFS</span><span class="sxs-lookup"><span data-stu-id="df74b-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="df74b-131">Pokud chcete povolit tak uživateli vytvářet nové týmové projekty, prvního úkolu vysoké úrovně je přidání uživatele do **Project Collection Administrators** pro kolekci týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="df74b-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="df74b-132">**K přidání uživatele do skupiny Správci kolekcí projektů**</span><span class="sxs-lookup"><span data-stu-id="df74b-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="df74b-133">Na serveru TFS na **Start** nabídky, přejděte k **všechny programy**, klikněte na tlačítko **Microsoft Team Foundation Server 2010**a potom klikněte na tlačítko **Team Foundation Konzola pro správu**.</span><span class="sxs-lookup"><span data-stu-id="df74b-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="df74b-134">Ve stromovém zobrazení navigace, rozbalte **aplikační vrstva**a potom klikněte na tlačítko **kolekce týmových projektů**.</span><span class="sxs-lookup"><span data-stu-id="df74b-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="df74b-135">V **kolekce týmových projektů** podokně, vyberte je vhodné spravovat kolekce týmových projektů.</span><span class="sxs-lookup"><span data-stu-id="df74b-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="df74b-136">Na **Obecné** klikněte na tlačítko **členství ve skupině**.</span><span class="sxs-lookup"><span data-stu-id="df74b-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="df74b-137">V **globální skupiny** dialogové okno, vyberte **Project Collection Administrators** skupiny a potom klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="df74b-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="df74b-138">V **vlastnosti skupiny serveru Team Foundation** dialogu **Windows uživatele nebo skupinu**a potom klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="df74b-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="df74b-139">V **vybrat uživatele, počítače nebo skupiny** dialogové okno, zadejte uživatelské jméno uživatele, který chcete vytvořit nové týmové projekty, klikněte na **Kontrola názvů**a potom klikněte na tlačítko **OK** .</span><span class="sxs-lookup"><span data-stu-id="df74b-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="df74b-140">V **vlastnosti skupiny serveru Team Foundation** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="df74b-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="df74b-141">V **globální skupiny** dialogové okno, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="df74b-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="df74b-142">Udělení oprávnění ve službě SharePoint Services</span><span class="sxs-lookup"><span data-stu-id="df74b-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="df74b-143">Dále je třeba udělit oprávnění uživatele k vytvoření nové týmové weby v kolekci webů služby SharePoint, která odpovídá vaší kolekci týmových projektů TFS.</span><span class="sxs-lookup"><span data-stu-id="df74b-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="df74b-144">**Udělení oprávnění k úplnému řízení na kolekce webů služby SharePoint**</span><span class="sxs-lookup"><span data-stu-id="df74b-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="df74b-145">V Team Foundation Server Administration Console na **kolekce týmových projektů** vyberte kolekci týmových projektů, kterou chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="df74b-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="df74b-146">Na **webu služby SharePoint** kartu, poznamenejte si hodnotu **aktuální výchozí umístění webu** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="df74b-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="df74b-147">Spusťte aplikaci Internet Explorer a přejděte na adresu URL, kterou jste si poznamenali v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="df74b-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="df74b-148">Pokud nejste přihlášení k Windows jako uživatel, který vytvořil kolekci týmových projektů, budete potřebovat k přihlášení do služby SharePoint jako tento uživatel aby bylo možné pokračovat.</span><span class="sxs-lookup"><span data-stu-id="df74b-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="df74b-149">Na **Akce webu** nabídky, klikněte na tlačítko **nastavení webu**.</span><span class="sxs-lookup"><span data-stu-id="df74b-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="df74b-150">Na **nastavení webu** stránce v části **uživatelů a oprávnění**, klikněte na tlačítko **osoby a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="df74b-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="df74b-151">V levém navigačním panelu klikněte na tlačítko **skupiny**.</span><span class="sxs-lookup"><span data-stu-id="df74b-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="df74b-152">Na **osoby a skupiny: všechny skupiny** klikněte na **nastavení skupin pro tuto lokalitu**.</span><span class="sxs-lookup"><span data-stu-id="df74b-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="df74b-153">Může se zobrazit <strong>HTTP 404 Nenalezeno</strong> chyba z důvodu double kódování chyby protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="df74b-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="df74b-154">Pokud k tomu dojde, nahraďte adresu URL s tímto:</span><span class="sxs-lookup"><span data-stu-id="df74b-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="df74b-155">`[site_collection_URL]/_layouts/permsetup.aspx` Příklad:</span><span class="sxs-lookup"><span data-stu-id="df74b-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="df74b-156">Na **nastavení skupin pro tuto lokalitu** stránce, přidejte uživatele, který bude vytvářet týmové projekty do **vlastníky** skupiny a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="df74b-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="df74b-157">Další informace o povolení uživatelům vytvářet nové týmové projekty v kolekci týmových projektů, naleznete v tématu [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="df74b-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="df74b-158">Vytvoření nového týmového projektu a přidání uživatelů</span><span class="sxs-lookup"><span data-stu-id="df74b-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="df74b-159">Jakmile máte potřebná oprávnění, můžete použít **Team Exploreru** okna v sadě Visual Studio 2010 k vytvoření nového týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="df74b-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="df74b-160">Tento přístup poskytuje průvodce, který shromažďuje všechny požadované informace a provádí potřebné úkoly v TFS, SharePoint a SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="df74b-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="df74b-161">Také budete muset udělit oprávnění pro nový týmový projekt na členy týmu pro vývojáře, aby mohli přidávat a upravovat obsah.</span><span class="sxs-lookup"><span data-stu-id="df74b-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="df74b-162">Kdo provádí tyto postupy?</span><span class="sxs-lookup"><span data-stu-id="df74b-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="df74b-163">Tyto postupy provádí obvykle správcem serveru TFS nebo vedoucí týmu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="df74b-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="df74b-164">Vytvořte nový týmový projekt</span><span class="sxs-lookup"><span data-stu-id="df74b-164">Create a New Team Project</span></span>

<span data-ttu-id="df74b-165">Následující postup popisuje, jak vytvořit nový týmový projekt v TFS 2010.</span><span class="sxs-lookup"><span data-stu-id="df74b-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="df74b-166">**Chcete-li vytvořit nový týmový projekt**</span><span class="sxs-lookup"><span data-stu-id="df74b-166">**To create a new team project**</span></span>

1. <span data-ttu-id="df74b-167">Na **Start** nabídky, přejděte k **všechny programy**, klikněte na tlačítko **sadu Microsoft Visual Studio 2010**, klikněte pravým tlačítkem na **sadu Microsoft Visual Studio 2010**, a pak klikněte na tlačítko **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="df74b-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="df74b-168">Při spuštění sady Visual Studio 2010 jako správce, Průvodce novým týmovým projektem se nezdaří v posledním kroku.</span><span class="sxs-lookup"><span data-stu-id="df74b-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="df74b-169">Pokud **řízení uživatelských účtů** dialogové okno se zobrazí, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="df74b-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="df74b-170">V sadě Visual Studio na **týmu** nabídky, klikněte na tlačítko **připojit k Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="df74b-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="df74b-171">Pokud jste už nakonfigurovali připojení k serveru TFS, můžete vynechat kroky 4 až 7.</span><span class="sxs-lookup"><span data-stu-id="df74b-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="df74b-172">V **připojení k týmovému projektu** dialogové okno, klikněte na tlačítko **servery**.</span><span class="sxs-lookup"><span data-stu-id="df74b-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="df74b-173">V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="df74b-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="df74b-174">V **přidat Team Foundation Server** dialogové okno, zadejte podrobnosti o instanci TFS a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="df74b-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="df74b-175">V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="df74b-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="df74b-176">V **připojit k týmovému projektu** dialogové okno, vyberte instanci TFS, kterou chcete připojit, vyberte tým projektu kolekce, kterou chcete přidat a potom klikněte na **připojit**.</span><span class="sxs-lookup"><span data-stu-id="df74b-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="df74b-177">V **Team Exploreru** okna, klikněte pravým tlačítkem na kolekci projektu týmu a potom klikněte na **novým týmovým projektem**.</span><span class="sxs-lookup"><span data-stu-id="df74b-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="df74b-178">V **novým týmovým projektem** dialogové okno, zadejte název a popis pro týmový projekt a potom klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="df74b-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="df74b-179">Pokud váš týmový projekt obsahuje mezery, které mohou nastat některé problémy, když přejdete na použijte nástroj pro nasazení Internetové informační služby (IIS) webu (nasazení webu) nasadit balíčky z výstupní cesta.</span><span class="sxs-lookup"><span data-stu-id="df74b-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="df74b-180">Mezery v cestě může ztížit mnohem více ke spuštění příkazů pro nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="df74b-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="df74b-181">Na **vyberte šablonu procesu** vyberte šablonu procesu, kterou chcete použít ke správě procesu vývoje a potom klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="df74b-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="df74b-182">Další informace o šablonách procesu TFS naleznete v tématu [nástroje a šablony procesů](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="df74b-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="df74b-183">Na **nastavení týmového webu** stránce, ponechejte výchozí nastavení pro beze změny a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="df74b-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="df74b-184">Toto nastavení se vytvoří nebo identifikuje týmový web Sharepointu, který je spojen s týmovým projektem TFS.</span><span class="sxs-lookup"><span data-stu-id="df74b-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="df74b-185">Váš vývojový tým mohou používat tuto lokalitu ke správě dokumentaci, účastnit diskusní vlákna, vytvořit wiki stránek a provádění různých úloh, které nesouvisejí se kód.</span><span class="sxs-lookup"><span data-stu-id="df74b-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="df74b-186">Další informace najdete v tématu [interakce mezi produkty SharePoint a serveru Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="df74b-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="df74b-187">Na **zadejte nastavení správy zdrojových kódů** stránce, ponechejte výchozí nastavení pro beze změny a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="df74b-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="df74b-188">Toto nastavení určuje, nebo vytvoří umístění v hierarchii složek serveru TFS, který bude sloužit jako kořenová složka pro obsah.</span><span class="sxs-lookup"><span data-stu-id="df74b-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="df74b-189">Na **potvrzení nastavení týmového projektu** klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="df74b-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="df74b-190">Pokud nový týmový projekt je úspěšně vytvořen, na **týmový projekt vytvořen** klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="df74b-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="df74b-191">Přidání uživatelů do týmového projektu</span><span class="sxs-lookup"><span data-stu-id="df74b-191">Add Users to a Team Project</span></span>

<span data-ttu-id="df74b-192">Teď, když jste vytvořili nový týmový projekt, můžete udělit oprávnění uživatelům, aby se mohly spustit přidávání a spolupráci nad obsahem.</span><span class="sxs-lookup"><span data-stu-id="df74b-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="df74b-193">**Přidání uživatelů do týmového projektu**</span><span class="sxs-lookup"><span data-stu-id="df74b-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="df74b-194">V sadě Visual Studio 2010 v **Team Exploreru** okna, klikněte pravým tlačítkem na týmový projekt, přejděte na **nastavení týmového projektu**a potom klikněte na tlačítko **členství ve skupině**.</span><span class="sxs-lookup"><span data-stu-id="df74b-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="df74b-195">A povolit tak uživateli přidávat, upravovat a odebírat kód pod správou zdrojových kódů, je třeba přidat ji do **přispěvatelé** skupiny.</span><span class="sxs-lookup"><span data-stu-id="df74b-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="df74b-196">V **skupiny projektu** dialogové okno, vyberte **přispěvatelé** skupiny a potom klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="df74b-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="df74b-197">V **vlastnosti skupiny serveru Team Foundation** dialogu **Windows uživatele nebo skupinu**a potom klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="df74b-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="df74b-198">V **vybrat uživatele, počítače nebo skupiny** dialogové okno, zadejte uživatelské jméno uživatele, které chcete přidat do týmového projektu, klikněte na **Kontrola názvů**a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="df74b-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="df74b-199">V **vlastnosti skupiny serveru Team Foundation** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="df74b-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="df74b-200">V **skupiny projektu** dialogové okno, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="df74b-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="df74b-201">Závěr</span><span class="sxs-lookup"><span data-stu-id="df74b-201">Conclusion</span></span>

<span data-ttu-id="df74b-202">V tomto okamžiku je připravený k použití nového týmového projektu, a tým vývojářů může začít přidávání obsahu a spolupráci na proces vývoje.</span><span class="sxs-lookup"><span data-stu-id="df74b-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="df74b-203">Dalším tématu s názvem [přidávání obsahu do správy zdrojových kódů](adding-content-to-source-control.md), popisuje, jak přidat obsah do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="df74b-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="df74b-204">Další čtení</span><span class="sxs-lookup"><span data-stu-id="df74b-204">Further Reading</span></span>

<span data-ttu-id="df74b-205">Širší pokyny k vytvoření týmových projektů v TFS najdete v tématu [vytvořit týmový projekt](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="df74b-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="df74b-206">Další informace o povolení uživatelům vytvářet nové týmové projekty v kolekci týmových projektů, naleznete v tématu [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="df74b-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="df74b-207">Další informace o přidávání uživatelů do týmových projektů, naleznete v tématu [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="df74b-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="df74b-208">[Předchozí](configuring-team-foundation-server-for-web-deployment.md)
> [další](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="df74b-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
