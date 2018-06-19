---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Řešení potíží s proces balení | Microsoft Docs
author: jrjlee
description: Toto téma popisuje, jak můžete shromáždit podrobné informace o vytváření balíčku pomocí vlastnosti EnablePackageProcessLoggingAndAssert v M...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 816ab77c44b52c6449a139475f2ef8546bd38071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892683"
---
<a name="troubleshooting-the-packaging-process"></a><span data-ttu-id="604f8-103">Řešení potíží s proces balení</span><span class="sxs-lookup"><span data-stu-id="604f8-103">Troubleshooting the Packaging Process</span></span>
====================
<span data-ttu-id="604f8-104">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="604f8-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="604f8-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="604f8-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="604f8-106">Toto téma popisuje, jak můžete shromáždit podrobné informace o vytváření balíčku pomocí **EnablePackageProcessLoggingAndAssert** vlastnost v Microsoft Build Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="604f8-106">This topic describes how you can collect detailed information about the packaging process by using the **EnablePackageProcessLoggingAndAssert** property in the Microsoft Build Engine (MSBuild).</span></span>
> 
> <span data-ttu-id="604f8-107">Když nastavíte **EnablePackageProcessLoggingAndAssert** vlastnost **true**, bude MSBuild:</span><span class="sxs-lookup"><span data-stu-id="604f8-107">When you set the **EnablePackageProcessLoggingAndAssert** property to **true**, MSBuild will:</span></span>
> 
> - <span data-ttu-id="604f8-108">Přidejte další informace o vytváření balíčku do protokolů o sestavení.</span><span class="sxs-lookup"><span data-stu-id="604f8-108">Add additional information about the packaging process to the build logs.</span></span>
> - <span data-ttu-id="604f8-109">Protokolování chyb za určitých podmínek, například pokud se v seznamu balení nacházejí duplicitní soubory.</span><span class="sxs-lookup"><span data-stu-id="604f8-109">Log errors under certain conditions, for example, if duplicate files are found in the packaging list.</span></span>
> - <span data-ttu-id="604f8-110">Vytvořte adresář protokolu v *ProjectName*\_balíček složky a použít ho k zaznamenání informací o souborech jste balení.</span><span class="sxs-lookup"><span data-stu-id="604f8-110">Create a Log directory in the *ProjectName*\_Package folder and use it to record information about the files you're packaging.</span></span>
> 
> <span data-ttu-id="604f8-111">Pokud se nedaří proces balení nebo balíčky pro nasazení vaší webové neobsahují soubory, které očekáváte, můžete použít tyto informace k řešení potíží s proces a pinpoint kam věcí nesprávný.</span><span class="sxs-lookup"><span data-stu-id="604f8-111">If the packaging process is failing, or your web deployment packages don't contain the files that you expect, you can use this information to troubleshoot the process and pinpoint where things are going wrong.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="604f8-112">**EnablePackageProcessLoggingAndAssert** vlastnost funguje jenom v případě, že vytvoříte projekt pomocí **ladění** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="604f8-112">The **EnablePackageProcessLoggingAndAssert** property only works if you build your project using the **Debug** configuration.</span></span> <span data-ttu-id="604f8-113">Vlastnost je ignorován v ostatní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="604f8-113">The property is ignored in other configurations.</span></span>


<span data-ttu-id="604f8-114">Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz řady používá ukázkové řešení&#x2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3, komunikaci Windows Služba Foundation (WCF) a projekt databáze.</span><span class="sxs-lookup"><span data-stu-id="604f8-114">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="604f8-115">Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízené procesu sestavení dva soubory projektu&#x2014;jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="604f8-115">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="604f8-116">V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.</span><span class="sxs-lookup"><span data-stu-id="604f8-116">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a><span data-ttu-id="604f8-117">Principy vlastnost EnablePackageProcessLoggingAndAssert</span><span class="sxs-lookup"><span data-stu-id="604f8-117">Understanding the EnablePackageProcessLoggingAndAssert Property</span></span>

<span data-ttu-id="604f8-118">[Vytváření a projekty webových aplikací balení](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) popisuje, jak webové publikování kanálu (WPP) poskytuje sadu cíle MSBuild, které rozšiřují funkce nástroje MSBuild jej a povolte integraci s Internetové informační služby (IIS) webu Nástroj pro nasazení (Web Deploy).</span><span class="sxs-lookup"><span data-stu-id="604f8-118">[Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) described how the Web Publishing Pipeline (WPP) provides a set of MSBuild targets that extend the functionality of MSBuild and enable it to integrate with the Internet Information Services (IIS) Web Deployment Tool (Web Deploy).</span></span> <span data-ttu-id="604f8-119">Když vytvoříte balíček projekt webové aplikace, se volání WPP cíle.</span><span class="sxs-lookup"><span data-stu-id="604f8-119">When you package a web application project, you're invoking WPP targets.</span></span>

<span data-ttu-id="604f8-120">Velké množství těchto cílů WPP zahrnují podmíněnou logiku, která ukládá do protokolu Další informace při **EnablePackageProcessLoggingAndAssert** je nastavena na **true**.</span><span class="sxs-lookup"><span data-stu-id="604f8-120">Lots of these WPP targets include conditional logic that logs additional information when the **EnablePackageProcessLoggingAndAssert** property is set to **true**.</span></span> <span data-ttu-id="604f8-121">Například, pokud byste si projít **balíček** cíl, můžete zjistit, že vytvoří adresář dodatečných protokolů a zapisuje do textového souboru seznam souborů, pokud **EnablePackageProcessLoggingAndAssert** rovná **true**.</span><span class="sxs-lookup"><span data-stu-id="604f8-121">For example, if you review the **Package** target, you can see that it creates an additional log directory and writes a list of files to a text file if **EnablePackageProcessLoggingAndAssert** is equal to **true**.</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> <span data-ttu-id="604f8-122">Cíle WPP jsou definovány v *Microsoft.Web.Publishing.targets* soubor ve složce % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.</span><span class="sxs-lookup"><span data-stu-id="604f8-122">The WPP targets are defined in the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span> <span data-ttu-id="604f8-123">Můžete otevřít tento soubor a zkontrolujte cíle v sadě Visual Studio 2010 nebo editoru XML.</span><span class="sxs-lookup"><span data-stu-id="604f8-123">You can open this file and review the targets in Visual Studio 2010 or any XML editor.</span></span> <span data-ttu-id="604f8-124">Je třeba dbát na změnit obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="604f8-124">Take care not to modify the contents of the file.</span></span>


## <a name="enabling-the-additional-logging"></a><span data-ttu-id="604f8-125">Povolení další protokolování</span><span class="sxs-lookup"><span data-stu-id="604f8-125">Enabling the Additional Logging</span></span>

<span data-ttu-id="604f8-126">Můžete zadat hodnotu **EnablePackageProcessLoggingAndAssert** vlastnost různými způsoby v závislosti na tom, jak sestavit projekt.</span><span class="sxs-lookup"><span data-stu-id="604f8-126">You can supply a value for the **EnablePackageProcessLoggingAndAssert** property in various ways, depending on how you build your project.</span></span>

<span data-ttu-id="604f8-127">Pokud vytvoříte projekt z příkazového řádku, můžete zadat hodnotu **EnablePackageProcessLoggingAndAssert** vlastnost jako argument příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="604f8-127">If you build your project from the command line, you can supply a value for the **EnablePackageProcessLoggingAndAssert** property as a command-line argument:</span></span>


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


<span data-ttu-id="604f8-128">Pokud používáte vlastní projektu soubor vytvářet projekty, můžete zahrnout **EnablePackageProcessLoggingAndAssert** hodnotu **vlastnosti** atribut **MSBuild**úloh:</span><span class="sxs-lookup"><span data-stu-id="604f8-128">If you're using a custom project file to build your projects, you can include the **EnablePackageProcessLoggingAndAssert** value in the **Properties** attribute of the **MSBuild** task:</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


<span data-ttu-id="604f8-129">Používáte-li vytvářet projekty definice sestavení Team Foundation Server (TFS), můžete zadat hodnotu **EnablePackageProcessLoggingAndAssert** vlastnost **argumenty MSBuild** řádek:![](troubleshooting-the-packaging-process/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="604f8-129">If you're using a Team Foundation Server (TFS) build definition to build your projects, you can supply a value for the **EnablePackageProcessLoggingAndAssert** property in the **MSBuild Arguments** row:![](troubleshooting-the-packaging-process/_static/image1.png)</span></span>

> [!NOTE]
> <span data-ttu-id="604f8-130">Další informace o vytváření a konfiguraci definice sestavení, najdete v části [vytváření sestavení definice, podporuje nasazení](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="604f8-130">For more information on creating and configuring build definitions, see [Creating a Build Definition That Supports Deployment](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).</span></span>


<span data-ttu-id="604f8-131">Případně, pokud chcete zahrnout do každé sestavení balíčku, můžete upravit soubor projektu pro webový projekt aplikace nastavit **EnablePackageProcessLoggingAndAssert** vlastnost **true**.</span><span class="sxs-lookup"><span data-stu-id="604f8-131">Alternatively, if you want to include the package in every build, you can modify the project file for your web application project to set the **EnablePackageProcessLoggingAndAssert** property to **true**.</span></span> <span data-ttu-id="604f8-132">Měli byste přidat vlastnost prvního **PropertyGroup** element v souboru .csproj nebo .vbproj.</span><span class="sxs-lookup"><span data-stu-id="604f8-132">You should add the property to the first **PropertyGroup** element within your .csproj or .vbproj file.</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a><span data-ttu-id="604f8-133">Kontrola soubory protokolu</span><span class="sxs-lookup"><span data-stu-id="604f8-133">Reviewing the Log Files</span></span>

<span data-ttu-id="604f8-134">Při sestavení a balíček projekt webové aplikace s **EnablePackageProcessLoggingAndAssert** nastavena na **true**, MSBuild vytvoří další složku s názvem přihlášení *ProjectName* \_Složky balíčku.</span><span class="sxs-lookup"><span data-stu-id="604f8-134">When you build and package a web application project with **EnablePackageProcessLoggingAndAssert** set to **true**, MSBuild creates an additional folder named Log in the *ProjectName*\_Package folder.</span></span> <span data-ttu-id="604f8-135">Složka protokolu obsahuje různé soubory:</span><span class="sxs-lookup"><span data-stu-id="604f8-135">The Log folder contains various files:</span></span>

![](troubleshooting-the-packaging-process/_static/image2.png)

<span data-ttu-id="604f8-136">Seznam souborů, které jste se bude lišit podle věcí v projektu a vaše procesu sestavení.</span><span class="sxs-lookup"><span data-stu-id="604f8-136">The list of files that you see will vary according to the things in your project and your build process.</span></span> <span data-ttu-id="604f8-137">Tyto soubory jsou obvykle používány k zaznamenání seznam souborů, jako je shromažďování pro balení v různých fázích procesu:</span><span class="sxs-lookup"><span data-stu-id="604f8-137">However, these files are typically used to record the list of files that the WPP is collecting for packaging, at various stages of the process:</span></span>

- <span data-ttu-id="604f8-138">*PreExcludePipelineCollectFilesPhaseFileList.txt* jsou uvedené soubory, které shromažďuje MSBuild pro balení předtím, než se odeberou všechny soubory, které jsou určené pro vyloučení souborů.</span><span class="sxs-lookup"><span data-stu-id="604f8-138">The *PreExcludePipelineCollectFilesPhaseFileList.txt* file lists the files that MSBuild collects for packaging before any files that are specified for exclusion are removed.</span></span>
- <span data-ttu-id="604f8-139">*AfterExcludeFilesFilesList.txt* soubor obsahuje seznam upravené soubor po odebrání všechny soubory, které jsou určené k vyloučení.</span><span class="sxs-lookup"><span data-stu-id="604f8-139">The *AfterExcludeFilesFilesList.txt* file contains the modified file list after any files that are specified for exclusion are removed.</span></span>

    > [!NOTE]
    > <span data-ttu-id="604f8-140">Další informace o vyloučení souborů a složek z procesu balení, najdete v části [vyloučení souborů a složek z nasazení](excluding-files-and-folders-from-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="604f8-140">For more information on excluding files and folders from the packaging process, see [Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span>
- <span data-ttu-id="604f8-141">*AfterTransformWebConfig.txt* souborů obsahuje soubory, které shromažďují pro balení po všech *Web.config* prováděly transformací.</span><span class="sxs-lookup"><span data-stu-id="604f8-141">The *AfterTransformWebConfig.txt* file lists the files collected for packaging after any *Web.config* transforms have been performed.</span></span> <span data-ttu-id="604f8-142">V tomto seznamu žádné specifické konfigurace *Web.config* transformace souborů, jako je třeba *Web.Debug.config* a *Web.Release.config*, jsou vyloučeny ze seznamu souborů pro balení.</span><span class="sxs-lookup"><span data-stu-id="604f8-142">In this list, any configuration-specific *Web.config* transform files, like *Web.Debug.config* and *Web.Release.config*, are excluded from the list of files for packaging.</span></span> <span data-ttu-id="604f8-143">Jediný transformovat *Web.config* je součástí místo nich.</span><span class="sxs-lookup"><span data-stu-id="604f8-143">A single transformed *Web.config* is included in their place.</span></span>
- <span data-ttu-id="604f8-144">*PostAutoParameterizationWebConfigConnectionStrings.txt* soubor obsahuje seznam souborů, které po připojovací řetězce v *Web.config* souboru mají byla parametry.</span><span class="sxs-lookup"><span data-stu-id="604f8-144">The *PostAutoParameterizationWebConfigConnectionStrings.txt* file contains the list of files after the connection strings in the *Web.config* file have been parameterized.</span></span> <span data-ttu-id="604f8-145">Toto je proces, který umožňuje připojovací řetězce nahraďte správné nastavení pro cílové prostředí při nasazení balíčku.</span><span class="sxs-lookup"><span data-stu-id="604f8-145">This is the process that lets you replace your connection strings with the right settings for your target environment when you deploy the package.</span></span>
- <span data-ttu-id="604f8-146">*Prepackage.txt* soubor obsahuje dokončené před sestavením seznam souborů, které mají být zahrnuty v balíčku.</span><span class="sxs-lookup"><span data-stu-id="604f8-146">The *Prepackage.txt* file contains the finalized pre-build list of files to be included in the package.</span></span>

> [!NOTE]
> <span data-ttu-id="604f8-147">Názvy souborů protokolu další obvykle odpovídají WPP cíle.</span><span class="sxs-lookup"><span data-stu-id="604f8-147">The names of the additional log files typically correspond to WPP targets.</span></span> <span data-ttu-id="604f8-148">Tyto cíle můžete zkontrolovat tak, že prověří *Microsoft.Web.Publishing.targets* soubor ve složce % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.</span><span class="sxs-lookup"><span data-stu-id="604f8-148">You can review these targets by examining the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span>


<span data-ttu-id="604f8-149">Pokud obsah webového balíčku nejsou, co jste očekávali, kontrola těchto souborů může být užitečný způsob, jak identifikovat v jaké bodu v procesu věcí došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="604f8-149">If the contents of your web package aren't what you expected, reviewing these files can be a useful way to identify at what point in the process things went wrong.</span></span>

## <a name="conclusion"></a><span data-ttu-id="604f8-150">Závěr</span><span class="sxs-lookup"><span data-stu-id="604f8-150">Conclusion</span></span>

<span data-ttu-id="604f8-151">Toto téma popisuje, jak můžete použít **EnablePackageProcessLoggingAndAssert** vlastnost v nástroji MSBuild k řešení potíží během balení.</span><span class="sxs-lookup"><span data-stu-id="604f8-151">This topic described how you can use the **EnablePackageProcessLoggingAndAssert** property in MSBuild to troubleshoot the packaging process.</span></span> <span data-ttu-id="604f8-152">Je popsané různé způsoby, ve kterém můžete zadat hodnotu vlastnosti do procesu sestavení a jeho popsané Další informace, které se zaznamená při nastavte vlastnost na **true**.</span><span class="sxs-lookup"><span data-stu-id="604f8-152">It explained the different ways in which you can supply the property value to the build process, and it described the additional information that is recorded when you set the property to **true**.</span></span>

## <a name="further-reading"></a><span data-ttu-id="604f8-153">Další čtení</span><span class="sxs-lookup"><span data-stu-id="604f8-153">Further Reading</span></span>

<span data-ttu-id="604f8-154">Další informace o používání vlastních souborů projektu nástroje MSBuild k řízení procesu nasazení najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="604f8-154">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="604f8-155">Další informace o jako a jak se spravuje proces balení, najdete v části [budova a projekty webových aplikací balení](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span><span class="sxs-lookup"><span data-stu-id="604f8-155">For more information on the WPP and how it manages the packaging process, see [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span></span> <span data-ttu-id="604f8-156">Pokyny o tom, jak vyloučit konkrétní soubory a složky balíčky webového nasazení najdete v tématu [vyloučení souborů a složek z nasazení](excluding-files-and-folders-from-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="604f8-156">For guidance on how to exclude specific files and folders from web deployment packages, see [Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="604f8-157">Předchozí</span><span class="sxs-lookup"><span data-stu-id="604f8-157">Previous</span></span>](running-windows-powershell-scripts-from-msbuild-project-files.md)
