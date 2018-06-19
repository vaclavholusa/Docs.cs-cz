---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Vytváření a spouštění nasazení příkaz Soubor | Microsoft Docs
author: jrjlee
description: Toto téma popisuje, jak vytvořit soubor příkazů, které vám umožní spustit nasazení, které využívá soubory projektu Microsoft Build Engine (MSBuild) jako jeden krok, znovu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: e5fb034a67bc9f2ea549af269eae51a49acc4d98
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891175"
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="583f9-103">Vytváření a spouštění soubor příkazů nasazení</span><span class="sxs-lookup"><span data-stu-id="583f9-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="583f9-104">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="583f9-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="583f9-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="583f9-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="583f9-106">Toto téma popisuje, jak vytvořit soubor příkazů, které vám umožní spuštění pomocí souborů projektu Microsoft Build Engine (MSBuild) jako jeden krok, opakovatelných procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="583f9-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="583f9-107">Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz řady používá ukázkové řešení&#x2014; [obraťte se na správce](the-contact-manager-solution.md) řešení&#x2014;představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3, komunikaci Windows Služba Foundation (WCF) a projekt databáze.</span><span class="sxs-lookup"><span data-stu-id="583f9-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="583f9-108">Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [Principy procesu sestavení](understanding-the-build-process.md), ve které je řízené procesu sestavení dva soubory projektu&#x2014;jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="583f9-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="583f9-109">V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.</span><span class="sxs-lookup"><span data-stu-id="583f9-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="583f9-110">Přehled procesu</span><span class="sxs-lookup"><span data-stu-id="583f9-110">Process Overview</span></span>

<span data-ttu-id="583f9-111">V tomto tématu se dozvíte, jak vytvořit a spustit soubor příkazů, který používá tyto soubory projektu pro provedení opakované nasazení do cílového prostředí.</span><span class="sxs-lookup"><span data-stu-id="583f9-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="583f9-112">V podstatě příkazového řádku jednoduše tak, aby obsahovala příkaz MSBuild který:</span><span class="sxs-lookup"><span data-stu-id="583f9-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="583f9-113">Informuje MSBuild provést v prostředí bez ohledu na *Publish.proj* souboru.</span><span class="sxs-lookup"><span data-stu-id="583f9-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="583f9-114">Informuje *Publish.proj* souboru, soubor, který obsahuje nastavení projektu konkrétní prostředí a kde najít ho.</span><span class="sxs-lookup"><span data-stu-id="583f9-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="583f9-115">Vytvořit příkaz nástroje MSBuild</span><span class="sxs-lookup"><span data-stu-id="583f9-115">Create an MSBuild Command</span></span>

<span data-ttu-id="583f9-116">Jak je popsáno v [Principy procesu sestavení](understanding-the-build-process.md), soubor projektu konkrétní prostředí&#x2014;například *Env Dev.proj*&#x2014;slouží k importu do prostředí na úlohách *Publish.proj* souboru v čase vytvoření buildu.</span><span class="sxs-lookup"><span data-stu-id="583f9-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="583f9-117">Společně poskytují tyto dva soubory kompletní sada pokynů, které informují MSBuild, jak vytvořit a nasadit řešení.</span><span class="sxs-lookup"><span data-stu-id="583f9-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="583f9-118">*Publish.proj* soubor používá **importovat** elementu, který chcete importovat soubor projektu konkrétní prostředí.</span><span class="sxs-lookup"><span data-stu-id="583f9-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="583f9-119">Jako takový když použijete MSBuild.exe sestavení a nasazení řešení. Obraťte se na správce, budete muset:</span><span class="sxs-lookup"><span data-stu-id="583f9-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="583f9-120">Spustit MSBuild.exe na *Publish.proj* souboru.</span><span class="sxs-lookup"><span data-stu-id="583f9-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="583f9-121">Zadejte umístění souboru projektu konkrétní prostředí zadáním příkazového řádku parametr s názvem **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="583f9-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="583f9-122">K tomuto účelu MSBuild příkazu by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="583f9-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="583f9-123">Tady je jednoduchý kroku a přesunu do opakovatelné a krokování nasazení.</span><span class="sxs-lookup"><span data-stu-id="583f9-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="583f9-124">Všechny, které musíte udělat je přidání příkazu MSBuild do souboru CMD.</span><span class="sxs-lookup"><span data-stu-id="583f9-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="583f9-125">V řešení obraťte se na správce, publikovat složky obsahuje soubor s názvem *publikovat Dev.cmd* který přesně to provádí.</span><span class="sxs-lookup"><span data-stu-id="583f9-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="583f9-126">**/Fl** přepínač dá pokyn MSBuild vytvořit soubor protokolu s názvem *msbuild.log* v pracovním adresáři, ve kterém byl vyvolán MSBuild.exe.</span><span class="sxs-lookup"><span data-stu-id="583f9-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="583f9-127">Nasazení nebo znovu nasadit řešení obraťte se na správce, stačí provést spuštění *publikovat Dev.cmd* souboru.</span><span class="sxs-lookup"><span data-stu-id="583f9-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="583f9-128">Při spuštění souboru, bude MSBuild:</span><span class="sxs-lookup"><span data-stu-id="583f9-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="583f9-129">Vytvořte všechny projekty v řešení.</span><span class="sxs-lookup"><span data-stu-id="583f9-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="583f9-130">Generovat nasadit webových balíčků pro projekty webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="583f9-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="583f9-131">Generovat .dbschema a .deploymanifest souborů pro databázové projekty.</span><span class="sxs-lookup"><span data-stu-id="583f9-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="583f9-132">Balíčky webového nasazení na webový server.</span><span class="sxs-lookup"><span data-stu-id="583f9-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="583f9-133">K databázovému serveru nasazení databáze.</span><span class="sxs-lookup"><span data-stu-id="583f9-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="583f9-134">Spustit nasazení</span><span class="sxs-lookup"><span data-stu-id="583f9-134">Run the Deployment</span></span>

<span data-ttu-id="583f9-135">Když vytvoříte soubor příkazů pro cílové prostředí byste měli mít k dokončení celého nasazení jednoduše spuštěním souboru.</span><span class="sxs-lookup"><span data-stu-id="583f9-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="583f9-136">**K nasazení řešení. Obraťte se na správce vašeho testovacího prostředí**</span><span class="sxs-lookup"><span data-stu-id="583f9-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="583f9-137">Na pracovní stanici developer, otevřete Průzkumníka Windows a pak přejděte do umístění *publikovat Dev.cmd* souboru.</span><span class="sxs-lookup"><span data-stu-id="583f9-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="583f9-138">Poklikejte na soubor ji spustit.</span><span class="sxs-lookup"><span data-stu-id="583f9-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="583f9-139">Pokud **otevření souboru – upozornění zabezpečení** se zobrazí dialogové okno, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="583f9-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="583f9-140">Pokud je nastaven nastavení konfigurace a testovací servery správně, okna příkazového řádku se zobrazí **sestavení bylo úspěšné** zprávu po dokončení zpracování souborů projektu nástroje MSBuild.</span><span class="sxs-lookup"><span data-stu-id="583f9-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="583f9-141">Pokud je při prvním nasazení řešení do prostředí tohoto nástroje, budete muset přidat účet počítače serveru webového testu k **db\_datawriter** a **db\_DataReader –** rolí na **ContactManager** databáze.</span><span class="sxs-lookup"><span data-stu-id="583f9-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="583f9-142">Tento postup je popsaný v [konfigurace databáze serveru pro webové nasazení publikování](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="583f9-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="583f9-143">Potřebujete tato oprávnění přiřadit, když vytvoříte databázi.</span><span class="sxs-lookup"><span data-stu-id="583f9-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="583f9-144">Ve výchozím nastavení, procesu sestavení nebude znovu vytvořit databázi na každé nasazení&#x2014;místo toho porovná existující databázi a schéma nejnovější a proveďte pouze požadované změny.</span><span class="sxs-lookup"><span data-stu-id="583f9-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="583f9-145">V důsledku toho by měla pouze potřebujete namapovat tyto databázové role při prvním nasazení řešení.</span><span class="sxs-lookup"><span data-stu-id="583f9-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="583f9-146">Otevřete Internet Explorer a přejděte na adresu URL aplikace, obraťte se na správce (například `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="583f9-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="583f9-147">Ověřte, že aplikace funguje podle očekávání a vy budete moci přidat kontakty.</span><span class="sxs-lookup"><span data-stu-id="583f9-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="583f9-148">Závěr</span><span class="sxs-lookup"><span data-stu-id="583f9-148">Conclusion</span></span>

<span data-ttu-id="583f9-149">Vytvoření souboru příkaz, který obsahuje vaše pokyny MSBuild nabízí rychlý a snadný způsob vytváření a nasazování řešení vícenásobného projektu v určitém cílovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="583f9-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="583f9-150">Pokud potřebujete opakovaně nasadit řešení v prostředí s více cílové, můžete vytvořit více příkazové soubory.</span><span class="sxs-lookup"><span data-stu-id="583f9-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="583f9-151">Do každého souboru příkaz příkaz MSBuild budete stavět stejný soubor universal projektu, ale specifikujete soubor jiný projekt konkrétní prostředí.</span><span class="sxs-lookup"><span data-stu-id="583f9-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="583f9-152">Soubor příkazů publikování do vývojář nebo testovacím prostředí, může například obsahovat tento příkaz nástroje MSBuild:</span><span class="sxs-lookup"><span data-stu-id="583f9-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="583f9-153">Tento příkaz nástroje MSBuild může obsahovat soubor příkazů pro publikování do pracovní prostředí:</span><span class="sxs-lookup"><span data-stu-id="583f9-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="583f9-154">Pokyny k přizpůsobení souborů projektu specifický pro vaše vlastní prostředí serveru najdete v tématu [nakonfigurovat vlastnosti nasazení pro cílové prostředí](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="583f9-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="583f9-155">Přepsání vlastností nebo nastavení různé další přepínače v příkazu MSBuild můžete také upravit procesu sestavení pro každé prostředí.</span><span class="sxs-lookup"><span data-stu-id="583f9-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="583f9-156">Další informace najdete v tématu [příkazového řádku MSBuild – Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="583f9-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="583f9-157">[Předchozí](deploying-database-projects.md)
> [další](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="583f9-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
