---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Soubor příkazů pro vytvoření a spuštění nasazení | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak vytvořit soubor příkazů, který vám umožní spustit nasazení, které využívá Microsoft Build Engine (MSBuild) souborů projektu jako jeden krok, re...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 6b2a75e0740f648a3a6e4f39c00bd30609325728
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830299"
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="79be5-103">Vytvoření a spuštění souboru příkazů k nasazení</span><span class="sxs-lookup"><span data-stu-id="79be5-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="79be5-104">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="79be5-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="79be5-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="79be5-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="79be5-106">Toto téma popisuje, jak vytvořit soubor příkazů, který vám umožní spustit nasazení, které využívá Microsoft Build Engine (MSBuild) souborů projektu jako krokování a opakovatelného procesu.</span><span class="sxs-lookup"><span data-stu-id="79be5-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="79be5-107">Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [Správce kontaktů](the-contact-manager-solution.md) řešení&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.</span><span class="sxs-lookup"><span data-stu-id="79be5-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="79be5-108">Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [Principy procesu sestavení](understanding-the-build-process.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení.</span><span class="sxs-lookup"><span data-stu-id="79be5-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="79be5-109">V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.</span><span class="sxs-lookup"><span data-stu-id="79be5-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="79be5-110">Přehled procesu</span><span class="sxs-lookup"><span data-stu-id="79be5-110">Process Overview</span></span>

<span data-ttu-id="79be5-111">V tomto tématu se dozvíte, jak vytvořit a spustit příkaz soubor, který používá tyto soubory projektu nasazení opakovatelným na vaše cílové prostředí.</span><span class="sxs-lookup"><span data-stu-id="79be5-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="79be5-112">V podstatě souboru příkazů jednoduše musí obsahovat příkaz MSBuild, který:</span><span class="sxs-lookup"><span data-stu-id="79be5-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="79be5-113">Dává pokyn ke spuštění prostředí bez ohledu na MSBuild *Publish.proj* souboru.</span><span class="sxs-lookup"><span data-stu-id="79be5-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="79be5-114">Říká *Publish.proj* souboru, soubor, který obsahuje nastavení projektu pro konkrétní prostředí a kde ho najít.</span><span class="sxs-lookup"><span data-stu-id="79be5-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="79be5-115">Vytvoření příkazu MSBuild</span><span class="sxs-lookup"><span data-stu-id="79be5-115">Create an MSBuild Command</span></span>

<span data-ttu-id="79be5-116">Jak je popsáno v [Principy procesu sestavení](understanding-the-build-process.md), soubor projektu specifických pro prostředí&#x2014;například *Env Dev.proj*&#x2014;slouží k importu do prostředí nezávislá *Publish.proj* soubor v okamžiku sestavení.</span><span class="sxs-lookup"><span data-stu-id="79be5-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="79be5-117">Tyto dva soubory společně poskytují kompletní sadu pokynů, které informují MSBuild, jak sestavit a nasadit řešení.</span><span class="sxs-lookup"><span data-stu-id="79be5-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="79be5-118">*Publish.proj* soubor používá **importovat** – element pro import souboru projektu pro konkrétní prostředí.</span><span class="sxs-lookup"><span data-stu-id="79be5-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="79be5-119">V důsledku toho použijete-li vytvářet a nasazovat řešení Správce kontaktů MSBuild.exe, musíte:</span><span class="sxs-lookup"><span data-stu-id="79be5-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="79be5-120">Spouštět MSBuild.exe *Publish.proj* souboru.</span><span class="sxs-lookup"><span data-stu-id="79be5-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="79be5-121">Zadejte umístění souboru projektu pro konkrétní prostředí zadáním příkazového řádku parametr s názvem **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="79be5-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="79be5-122">K tomuto účelu nástroj MSBuild příkazu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="79be5-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="79be5-123">Tady je jednoduchý kroku a přesunu do nasazení opakovatelným a krokování.</span><span class="sxs-lookup"><span data-stu-id="79be5-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="79be5-124">Všechno, co musíte udělat, je přidání příkazu MSBuild do souboru .cmd.</span><span class="sxs-lookup"><span data-stu-id="79be5-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="79be5-125">V řešení Správce kontaktů publikovat složka obsahuje soubor s názvem *publikovat Dev.cmd* , který přesně to dělá.</span><span class="sxs-lookup"><span data-stu-id="79be5-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="79be5-126">**/Fl** přepínač nastaví nástroj MSBuild vytvoří soubor protokolu s názvem *msbuild.log* v pracovním adresáři, ve kterém se vyvolala MSBuild.exe.</span><span class="sxs-lookup"><span data-stu-id="79be5-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="79be5-127">Nasazení nebo znovu nasadit řešení Správce kontaktů, stačí provést spuštění *publikovat Dev.cmd* souboru.</span><span class="sxs-lookup"><span data-stu-id="79be5-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="79be5-128">Když spustíte tento soubor, bude nástroj MSBuild:</span><span class="sxs-lookup"><span data-stu-id="79be5-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="79be5-129">Sestavte všechny projekty v řešení.</span><span class="sxs-lookup"><span data-stu-id="79be5-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="79be5-130">Generovat nasaditelný webových balíčků pro projekty webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="79be5-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="79be5-131">Generovat .dbschema a .deploymanifest souborů pro databázové projekty.</span><span class="sxs-lookup"><span data-stu-id="79be5-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="79be5-132">Nasazení webových balíčků na webový server.</span><span class="sxs-lookup"><span data-stu-id="79be5-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="79be5-133">Nasazení databáze na serveru databáze.</span><span class="sxs-lookup"><span data-stu-id="79be5-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="79be5-134">Spustit nasazení</span><span class="sxs-lookup"><span data-stu-id="79be5-134">Run the Deployment</span></span>

<span data-ttu-id="79be5-135">Když vytvoříte soubor příkazů pro cílové prostředí byste měli dokončit celé nasazení jednoduše spuštěním souboru.</span><span class="sxs-lookup"><span data-stu-id="79be5-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="79be5-136">**K nasazení řešení Správce kontaktů na vašem testovacím prostředí**</span><span class="sxs-lookup"><span data-stu-id="79be5-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="79be5-137">Na vaši vývojářskou pracovní stanici, otevřete Průzkumníka Windows a pak přejděte do umístění *publikovat Dev.cmd* souboru.</span><span class="sxs-lookup"><span data-stu-id="79be5-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="79be5-138">Poklikejte na soubor a spustíme ji.</span><span class="sxs-lookup"><span data-stu-id="79be5-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="79be5-139">Pokud **otevření souboru – upozornění zabezpečení** dialogové okno se zobrazí, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="79be5-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="79be5-140">Pokud nastavení konfigurace a testovací servery jsou nastaveny správně, okna příkazového řádku se zobrazí **úspěšnými Buildy** zprávu po dokončení zpracování souborů projektu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="79be5-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="79be5-141">Pokud to je poprvé, kdy jste nasadili řešení do tohoto prostředí, budete muset přidat účet počítače serveru webového testu k **db\_datawriter nesmí být** a **db\_datareader**role na **ContactManager** databáze.</span><span class="sxs-lookup"><span data-stu-id="79be5-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="79be5-142">Tento postup je popsaný v [konfigurace databázového serveru pro publikování nasazení na webu](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="79be5-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="79be5-143">Potřebujete pouze přiřadit tato oprávnění, když vytvoříte databázi.</span><span class="sxs-lookup"><span data-stu-id="79be5-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="79be5-144">Ve výchozím nastavení, proces sestavení nebude znovu vytvořit databázi na každé nasazení&#x2014;místo toho se porovnat existující databázi a nejnovější schéma a provádět pouze změny, které vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="79be5-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="79be5-145">V důsledku toho by měla pouze potřeba namapovat tyto databázové role při prvním nasazení řešení.</span><span class="sxs-lookup"><span data-stu-id="79be5-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="79be5-146">Spusťte aplikaci Internet Explorer a přejděte na adresu URL aplikace Správce kontaktů (například `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="79be5-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="79be5-147">Ověřte, že aplikace funguje podle očekávání a budete moct přidat kontakty.</span><span class="sxs-lookup"><span data-stu-id="79be5-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="79be5-148">Závěr</span><span class="sxs-lookup"><span data-stu-id="79be5-148">Conclusion</span></span>

<span data-ttu-id="79be5-149">Vytváření souboru příkazů obsahující pokyny k MSBuild poskytuje rychlý a snadný způsob sestavování a nasazování řešení vícenásobného projektu pro konkrétní cílové prostředí.</span><span class="sxs-lookup"><span data-stu-id="79be5-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="79be5-150">Pokud je potřeba opakované nasazení svého řešení do cílového prostředí s více, můžete vytvořit více souborů příkazů.</span><span class="sxs-lookup"><span data-stu-id="79be5-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="79be5-151">V každém souboru příkaz příkaz MSBuild se sestaví stejný soubor projektu univerzální, ale určí soubor jiného projektu pro konkrétní prostředí.</span><span class="sxs-lookup"><span data-stu-id="79be5-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="79be5-152">Soubor příkazů publikovat vývojáři nebo testovací prostředí může obsahovat například tento příkaz MSBuild:</span><span class="sxs-lookup"><span data-stu-id="79be5-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="79be5-153">Soubor příkazů k publikování do přípravného prostředí může obsahovat tento příkaz MSBuild:</span><span class="sxs-lookup"><span data-stu-id="79be5-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="79be5-154">Pokyny k přizpůsobení souborů projektu specifických pro prostředí pro prostředí serveru, naleznete v tématu [nakonfigurovat vlastnosti nasazení pro cílové prostředí](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="79be5-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="79be5-155">Můžete také přizpůsobit proces sestavení pro jednotlivá prostředí přepsání vlastností nebo nastavením různé jinými přepínači ve svých rukou nástroje MSBuild.</span><span class="sxs-lookup"><span data-stu-id="79be5-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="79be5-156">Další informace najdete v tématu [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="79be5-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="79be5-157">[Předchozí](deploying-database-projects.md)
> [další](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="79be5-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
