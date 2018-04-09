---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Nasazení konkrétní sestavení | Microsoft Docs
author: jrjlee
description: Toto téma popisuje postup nasazení webových balíčků a databázové skripty z konkrétní předchozího sestavení do nového cíle, jako je pracovním nebo produkčním enviro...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 271d084b3c69016df5be28ada032973bf7fd5a49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="deploying-a-specific-build"></a><span data-ttu-id="33d12-103">Nasazení konkrétní sestavení</span><span class="sxs-lookup"><span data-stu-id="33d12-103">Deploying a Specific Build</span></span>
====================
<span data-ttu-id="33d12-104">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="33d12-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="33d12-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="33d12-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="33d12-106">Toto téma popisuje postup nasazení webových balíčků a databázové skripty z konkrétní předchozího sestavení do nového cíle, jako je pracovním nebo produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="33d12-106">This topic describes how to deploy web packages and database scripts from a specific previous build to a new destination, like a staging or production environment.</span></span>


<span data-ttu-id="33d12-107">Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz řady používá ukázkové řešení&#x2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3, komunikaci Windows Služba Foundation (WCF) a projekt databáze.</span><span class="sxs-lookup"><span data-stu-id="33d12-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="33d12-108">Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve kterém je řízena procesem sestavení a nasazení dva soubory projektu&#x2014;jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="33d12-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="33d12-109">V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.</span><span class="sxs-lookup"><span data-stu-id="33d12-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="33d12-110">Přehled úloh</span><span class="sxs-lookup"><span data-stu-id="33d12-110">Task Overview</span></span>

<span data-ttu-id="33d12-111">Dosud témata v této sadě kurz zaměřuje na postup sestavení, balíčků a nasazování webových aplikací a databází v rámci jednoho kroku nebo automatizované procesu.</span><span class="sxs-lookup"><span data-stu-id="33d12-111">Until now, the topics in this tutorial set have focused on how to build, package, and deploy web applications and databases as part of a single-step or automated process.</span></span> <span data-ttu-id="33d12-112">Ale v některé běžné scénáře, budete chtít ze sestavení do složky rozevíracího seznamu vyberte prostředky, které nasadíte.</span><span class="sxs-lookup"><span data-stu-id="33d12-112">However, in some common scenarios, you'll want to select the resources that you deploy from a list of builds in a drop folder.</span></span> <span data-ttu-id="33d12-113">Jinými slovy nemusí být nejnovější sestavení sestavení, který chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="33d12-113">In other words, the latest build may not be the build you want to deploy.</span></span>

<span data-ttu-id="33d12-114">Vezměte v úvahu scénář průběžnou integraci (CI) popsané v předchozí tématu [vytváření sestavení definice, podporuje nasazení](creating-a-build-definition-that-supports-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="33d12-114">Consider the continuous integration (CI) scenario described in the previous topic, [Creating a Build Definition That Supports Deployment](creating-a-build-definition-that-supports-deployment.md).</span></span> <span data-ttu-id="33d12-115">Jste vytvořili definice sestavení v Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="33d12-115">You've created a build definition in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="33d12-116">Pokaždé, když vývojář kontroluje kód do sady TFS, Team Build bude sestavení kódu, vytváření balíčků webové a databázové skripty jako součást procesu sestavení, spustit všechny testy jednotek a nasazení vašich prostředků v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="33d12-116">Every time a developer checks code into TFS, Team Build will build your code, create web packages and database scripts as part of the build process, run any unit tests, and deploy your resources to a test environment.</span></span> <span data-ttu-id="33d12-117">V závislosti na zásady uchovávání informací, které jste nakonfigurovali, když jste vytvořili definice sestavení zachovají TFS určitého počtu předchozích sestavení.</span><span class="sxs-lookup"><span data-stu-id="33d12-117">Depending on the retention policy you configured when you created the build definition, TFS will retain a certain number of previous builds.</span></span>

![](deploying-a-specific-build/_static/image1.png)

<span data-ttu-id="33d12-118">Nyní předpokládejme, že jste provedli ověření a ověření testování proti jednu z těchto sestavení v testovacím prostředí a jste připraveni k nasazení aplikace do pracovního prostředí.</span><span class="sxs-lookup"><span data-stu-id="33d12-118">Now, suppose you've performed verification and validation testing against one of these builds in your test environment, and you're ready to deploy your application to a staging environment.</span></span> <span data-ttu-id="33d12-119">Do té doby vývojáři mohou mít změnami nový kód.</span><span class="sxs-lookup"><span data-stu-id="33d12-119">In the meantime, developers may have checked in new code.</span></span> <span data-ttu-id="33d12-120">Nechcete, aby znovu sestavte řešení a nasazení do fázovacího prostředí a nechcete, aby k nasazení nejnovější sestavení pro testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="33d12-120">You don't want to rebuild the solution and deploy to the staging environment, and you don't want to deploy the latest build to the staging environment.</span></span> <span data-ttu-id="33d12-121">Místo toho kterou chcete nasadit konkrétní build, který jste se ověřit a ověřit na serveru test.</span><span class="sxs-lookup"><span data-stu-id="33d12-121">Instead, you want to deploy the specific build that you've verified and validated on the test servers.</span></span>

<span data-ttu-id="33d12-122">K tomu, budete muset v Microsoft Build Engine (MSBuild) umožňují vyhledávání webových balíčků a databázové skripty, které vygenerovaly konkrétní sestavení.</span><span class="sxs-lookup"><span data-stu-id="33d12-122">To accomplish this, you need to tell the Microsoft Build Engine (MSBuild) where to find the web packages and database scripts that a specific build generated.</span></span>

## <a name="overriding-the-outputroot-property"></a><span data-ttu-id="33d12-123">Přepisování vlastnosti OutputRoot</span><span class="sxs-lookup"><span data-stu-id="33d12-123">Overriding the OutputRoot Property</span></span>

<span data-ttu-id="33d12-124">V [ukázkové řešení](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* souboru deklaruje vlastnost s názvem **OutputRoot**.</span><span class="sxs-lookup"><span data-stu-id="33d12-124">In the [sample solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), the *Publish.proj* file declares a property named **OutputRoot**.</span></span> <span data-ttu-id="33d12-125">Jak již název naznačuje, jde kořenové složky, která obsahuje všechno, co se generuje procesu sestavení.</span><span class="sxs-lookup"><span data-stu-id="33d12-125">As the name suggests, this is the root folder that contains everything that the build process generates.</span></span> <span data-ttu-id="33d12-126">V *Publish.proj* souboru, můžete zobrazit, **OutputRoot** vlastnost odkazuje na kořenový adresář pro všechny prostředky pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="33d12-126">In the *Publish.proj* file, you can see that the **OutputRoot** property refers to the root location for all deployment resources.</span></span>

> [!NOTE]
> <span data-ttu-id="33d12-127">**OutputRoot** je název běžně používané vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="33d12-127">**OutputRoot** is a commonly used property name.</span></span> <span data-ttu-id="33d12-128">Visual C# a Visual Basic soubory projektu také deklarovat tuto vlastnost k uložení kořenový adresář pro všechny výstupy sestavení.</span><span class="sxs-lookup"><span data-stu-id="33d12-128">Visual C# and Visual Basic project files also declare this property to store the root location for all build outputs.</span></span>


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


<span data-ttu-id="33d12-129">Pokud chcete soubor projektu do nasazení webových balíčků a databáze skripty z jiného umístění&#x2014;jako výstupy předchozí sestavení TFS&#x2014;jednoduše je potřeba přepsat **OutputRoot** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="33d12-129">If you want your project file to deploy web packages and database scripts from a different location&#x2014;like the outputs of a previous TFS build&#x2014;you simply need to override the **OutputRoot** property.</span></span> <span data-ttu-id="33d12-130">Na serveru Team Build byste měli nastavit hodnotu vlastnosti ke složce relevantní sestavení.</span><span class="sxs-lookup"><span data-stu-id="33d12-130">You should set the property value to the relevant build folder on the Team Build server.</span></span> <span data-ttu-id="33d12-131">MSBuild spustili z příkazového řádku, můžete zadat hodnotu pro **OutputRoot** jako argument příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="33d12-131">If you were running MSBuild from the command line, you could specify a value for **OutputRoot** as a command-line argument:</span></span>


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


<span data-ttu-id="33d12-132">V praxi však by také chcete přeskočit **sestavení** cíl&#x2014;neexistuje bod při vytváření řešení, pokud nemáte v úmyslu použít výstupy sestavení.</span><span class="sxs-lookup"><span data-stu-id="33d12-132">In practice, however, you'd also want to skip the **Build** target&#x2014;there's no point in building your solution if you don't plan to use the build outputs.</span></span> <span data-ttu-id="33d12-133">Může to uděláte tak, že zadáte cíle, které chcete spustit z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="33d12-133">You could do this by specifying the targets you want to execute from the command line:</span></span>


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


<span data-ttu-id="33d12-134">Ale ve většině případů budete chtít vytvořit logika nasazení do definice sestavení sady TFS.</span><span class="sxs-lookup"><span data-stu-id="33d12-134">However, in most cases, you'll want to build your deployment logic into a TFS build definition.</span></span> <span data-ttu-id="33d12-135">Díky tomu mohou uživatelé s **fronty sestavení** oprávnění k aktivaci nasazení z jakékoli instalaci sady Visual Studio s připojením k serveru TFS.</span><span class="sxs-lookup"><span data-stu-id="33d12-135">This enables users with the **Queue builds** permission to trigger the deployment from any Visual Studio installation with a connection to the TFS server.</span></span>

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a><span data-ttu-id="33d12-136">Vytvoření definice sestavení pro nasazení konkrétní sestavení</span><span class="sxs-lookup"><span data-stu-id="33d12-136">Creating a Build Definition to Deploy Specific Builds</span></span>

<span data-ttu-id="33d12-137">Následující postup popisuje, jak k vytvoření definice sestavení, která umožní uživatelům aktivační událost nasazení pro pracovní prostředí s jedním příkazem.</span><span class="sxs-lookup"><span data-stu-id="33d12-137">The next procedure describes how to create a build definition that enables users to trigger deployments to a staging environment with a single command.</span></span>

<span data-ttu-id="33d12-138">V takovém případě nechcete definici sestavení ve skutečnosti sestavit nic&#x2014;chcete být spuštěna v souboru projektu vlastní logiky nasazení.</span><span class="sxs-lookup"><span data-stu-id="33d12-138">In this case, you don't want the build definition to actually build anything&#x2014;you just want it to execute the deployment logic in your custom project file.</span></span> <span data-ttu-id="33d12-139">*Publish.proj* soubor obsahuje podmíněnou logiku, který přeskočí **sestavení** cíle, pokud soubor běží v Team Build.</span><span class="sxs-lookup"><span data-stu-id="33d12-139">The *Publish.proj* file includes conditional logic that skips the **Build** target if the file is running in Team Build.</span></span> <span data-ttu-id="33d12-140">Dělá to pomocí vyhodnocení integrované **BuildingInTeamBuild** vlastnosti, která se automaticky nastaví na **true** při spuštění souboru projektu v Team Build.</span><span class="sxs-lookup"><span data-stu-id="33d12-140">It does this by evaluating the built-in **BuildingInTeamBuild** property, which is automatically set to **true** if you run your project file in Team Build.</span></span> <span data-ttu-id="33d12-141">V důsledku toho můžete přeskočit procesu sestavení a jednoduše spusťte soubor projektu nasazení existující sestavení.</span><span class="sxs-lookup"><span data-stu-id="33d12-141">As a result, you can skip the build process and simply run the project file to deploy an existing build.</span></span>

<span data-ttu-id="33d12-142">**K vytvoření definice sestavení pro ruční aktivaci nasazení**</span><span class="sxs-lookup"><span data-stu-id="33d12-142">**To create a build definition to trigger deployment manually**</span></span>

1. <span data-ttu-id="33d12-143">V sadě Visual Studio 2010 v **Team Explorer** okno, rozbalte uzel vaší týmového projektu, klikněte pravým tlačítkem na **sestavení**a potom klikněte na **novou definici sestavení**.</span><span class="sxs-lookup"><span data-stu-id="33d12-143">In Visual Studio 2010, in the **Team Explorer** window, expand your team project node, right-click **Builds**, and then click **New Build Definition**.</span></span>

    ![](deploying-a-specific-build/_static/image2.png)
2. <span data-ttu-id="33d12-144">Na **Obecné** kartě, pojmenujte definici sestavení (například **DeployToStaging**) a volitelný popis.</span><span class="sxs-lookup"><span data-stu-id="33d12-144">On the **General** tab, give the build definition a name (for example, **DeployToStaging**) and an optional description.</span></span>
3. <span data-ttu-id="33d12-145">Na **aktivační událost** vyberte **Ruční – vrácení se změnami nespouštějí nového sestavení**.</span><span class="sxs-lookup"><span data-stu-id="33d12-145">On the **Trigger** tab, select **Manual – Check-ins do not trigger a new build**.</span></span>
4. <span data-ttu-id="33d12-146">Na **sestavení výchozí** ve **kopie sestavení výstup do následující složky, vyřaďte** pole, zadejte cestu Universal Naming Convention (UNC) vaší složky (například  **\\TFSBUILD\Drops**).</span><span class="sxs-lookup"><span data-stu-id="33d12-146">On the **Build Defaults** tab, in the **Copy build output to the following drop folder** box, type the Universal Naming Convention (UNC) path of your drop folder (for example, **\\TFSBUILD\Drops**).</span></span>

    ![](deploying-a-specific-build/_static/image3.png)
5. <span data-ttu-id="33d12-147">Na **proces** ve **souboru procesu sestavení** rozevíracího seznamu, ponechejte **DefaultTemplate.xaml** vybrané.</span><span class="sxs-lookup"><span data-stu-id="33d12-147">On the **Process** tab, in the **Build process file** dropdown list, leave **DefaultTemplate.xaml** selected.</span></span> <span data-ttu-id="33d12-148">Toto je jedna z výchozích šablon procesu sestavení, které se přidají do všechny nové týmových projektů.</span><span class="sxs-lookup"><span data-stu-id="33d12-148">This is one of the default build process templates that get added to all new team projects.</span></span>
6. <span data-ttu-id="33d12-149">V **parametry procesu sestavení** tabulky, klikněte na tlačítko v **položky k sestavení** řádek a klikněte **třemi tečkami** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="33d12-149">In the **Build process parameters** table, click in the **Items to Build** row, and then click the **ellipsis** button.</span></span>

    ![](deploying-a-specific-build/_static/image4.png)
7. <span data-ttu-id="33d12-150">V **položky k sestavení** dialogové okno, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="33d12-150">In the **Items to Build** dialog box, click **Add**.</span></span>
8. <span data-ttu-id="33d12-151">V **položky typu** rozevíracího seznamu vyberte **soubory projektu nástroje MSBuild**.</span><span class="sxs-lookup"><span data-stu-id="33d12-151">In the **Items of type** dropdown list, select **MSBuild Project files**.</span></span>
9. <span data-ttu-id="33d12-152">Přejděte do umístění souboru vlastní projektu, se kterým řízení procesu nasazení, vyberte soubor a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="33d12-152">Browse to the location of the custom project file with which you control the deployment process, select the file, and then click **OK**.</span></span>

    ![](deploying-a-specific-build/_static/image5.png)
10. <span data-ttu-id="33d12-153">V **položky k sestavení** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="33d12-153">In the **Items to Build** dialog box, click **OK**.</span></span>
11. <span data-ttu-id="33d12-154">V **parametry procesu sestavení** tabulky, rozbalte **Upřesnit** části.</span><span class="sxs-lookup"><span data-stu-id="33d12-154">In the **Build process parameters** table, expand the **Advanced** section.</span></span>
12. <span data-ttu-id="33d12-155">V **argumenty MSBuild** řádek, zadejte umístění souboru projektu konkrétní prostředí a přidejte zástupný symbol pro umístění složky sestavení:</span><span class="sxs-lookup"><span data-stu-id="33d12-155">In the **MSBuild Arguments** row, specify the location of your environment-specific project file and add a placeholder for the location of your build folder:</span></span>

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > <span data-ttu-id="33d12-156">Budete potřebovat k přepsání **OutputRoot** hodnotu pokaždé, když fronty sestavení.</span><span class="sxs-lookup"><span data-stu-id="33d12-156">You'll need to override the **OutputRoot** value every time you queue a build.</span></span> <span data-ttu-id="33d12-157">Je to popsané v dalším postupu.</span><span class="sxs-lookup"><span data-stu-id="33d12-157">This is covered in the next procedure.</span></span>
13. <span data-ttu-id="33d12-158">Klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="33d12-158">Click **Save**.</span></span>

<span data-ttu-id="33d12-159">Když spustíte sestavení, je potřeba aktualizovat **OutputRoot** vlastnost tak, aby odkazoval na sestavení, které chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="33d12-159">When you trigger a build, you need to update the **OutputRoot** property to point to the build you want to deploy.</span></span>

<span data-ttu-id="33d12-160">**K nasazení konkrétní sestavení z definice sestavení**</span><span class="sxs-lookup"><span data-stu-id="33d12-160">**To deploy a specific build from a build definition**</span></span>

1. <span data-ttu-id="33d12-161">V **Team Explorer** okno, klikněte pravým tlačítkem na definici sestavení a pak klikněte na tlačítko **fronty nové sestavení**.</span><span class="sxs-lookup"><span data-stu-id="33d12-161">In the **Team Explorer** window, right-click the build definition, and then click **Queue New Build**.</span></span>

    ![](deploying-a-specific-build/_static/image7.png)
2. <span data-ttu-id="33d12-162">V **fronty sestavení** v dialogovém **parametry** rozbalte **Upřesnit** části.</span><span class="sxs-lookup"><span data-stu-id="33d12-162">In the **Queue Build** dialog box, on the **Parameters** tab, expand the **Advanced** section.</span></span>
3. <span data-ttu-id="33d12-163">V **argumenty MSBuild** řádek, nahraďte hodnotu **OutputRoot** vlastnost s umístění složky sestavení.</span><span class="sxs-lookup"><span data-stu-id="33d12-163">In the **MSBuild Arguments** row, replace the value of the **OutputRoot** property with the location of your build folder.</span></span> <span data-ttu-id="33d12-164">Příklad:</span><span class="sxs-lookup"><span data-stu-id="33d12-164">For example:</span></span>

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="33d12-165">Ujistěte se, že zahrnovat koncové lomítko na konci cestu do složky sestavení.</span><span class="sxs-lookup"><span data-stu-id="33d12-165">Be sure to include a trailing slash at the end of the path to your build folder.</span></span>
4. <span data-ttu-id="33d12-166">Klikněte na tlačítko **fronty**.</span><span class="sxs-lookup"><span data-stu-id="33d12-166">Click **Queue**.</span></span>

<span data-ttu-id="33d12-167">Když fronty sestavení, bude soubor projektu nasazení databázové skripty a webové balíčky ze složky rozevírací sestavení, kterou jste zadali v **OutputRoot** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="33d12-167">When you queue the build, the project file will deploy the database scripts and web packages from the build drop folder you specified in the **OutputRoot** property.</span></span>

## <a name="conclusion"></a><span data-ttu-id="33d12-168">Závěr</span><span class="sxs-lookup"><span data-stu-id="33d12-168">Conclusion</span></span>

<span data-ttu-id="33d12-169">Toto téma popisuje postup publikování materiály pro nasazení, jako je webových balíčků a databázové skripty, z předchozí konkrétní sestavení pomocí modelu nasazení souboru projektu rozdělení.</span><span class="sxs-lookup"><span data-stu-id="33d12-169">This topic described how to publish deployment resources, like web packages and database scripts, from a specific previous build using the split project file deployment model.</span></span> <span data-ttu-id="33d12-170">Ho vysvětlení, jak lze přepsat **OutputRoot** vlastnost a jak začlenit logiku nasazení do sady TFS sestavení definice.</span><span class="sxs-lookup"><span data-stu-id="33d12-170">It explained how to override the **OutputRoot** property and how to incorporate the deployment logic into a TFS build definition.</span></span>

## <a name="further-reading"></a><span data-ttu-id="33d12-171">Další čtení</span><span class="sxs-lookup"><span data-stu-id="33d12-171">Further Reading</span></span>

<span data-ttu-id="33d12-172">Další informace o vytváření definic sestavení, najdete v části [vytvoření základní definice sestavení](https://msdn.microsoft.com/library/ms181716.aspx) a [definovat proces vaše sestavení](https://msdn.microsoft.com/library/ms181715.aspx).</span><span class="sxs-lookup"><span data-stu-id="33d12-172">For more information on creating build definitions, see [Create a Basic Build Definition](https://msdn.microsoft.com/library/ms181716.aspx) and [Define Your Build Process](https://msdn.microsoft.com/library/ms181715.aspx).</span></span> <span data-ttu-id="33d12-173">Další pokyny k sestavení služby Řízení front, najdete v části [fronty sestavení](https://msdn.microsoft.com/library/ms181722.aspx).</span><span class="sxs-lookup"><span data-stu-id="33d12-173">For more guidance on queuing builds, see [Queue a Build](https://msdn.microsoft.com/library/ms181722.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="33d12-174">[Předchozí](creating-a-build-definition-that-supports-deployment.md)
> [další](configuring-permissions-for-team-build-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="33d12-174">[Previous](creating-a-build-definition-that-supports-deployment.md)
[Next](configuring-permissions-for-team-build-deployment.md)</span></span>
