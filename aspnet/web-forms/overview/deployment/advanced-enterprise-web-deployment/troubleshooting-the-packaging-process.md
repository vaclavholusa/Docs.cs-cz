---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Řešení potíží s procesem vytváření balíčku | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak můžete pomocí vlastnosti EnablePackageProcessLoggingAndAssert v M shromáždit podrobné informace o proces vytváření balíčku...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 22086d3c154214457fe35794998accdf6109471c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384141"
---
<a name="troubleshooting-the-packaging-process"></a><span data-ttu-id="f242e-103">Řešení potíží s procesem vytváření balíčku</span><span class="sxs-lookup"><span data-stu-id="f242e-103">Troubleshooting the Packaging Process</span></span>
====================
<span data-ttu-id="f242e-104">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="f242e-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="f242e-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="f242e-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="f242e-106">Toto téma popisuje, jak můžete shromažďovat podrobné informace o procesu vytváření balíčků pomocí **EnablePackageProcessLoggingAndAssert** vlastnost v Microsoft Build Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="f242e-106">This topic describes how you can collect detailed information about the packaging process by using the **EnablePackageProcessLoggingAndAssert** property in the Microsoft Build Engine (MSBuild).</span></span>
> 
> <span data-ttu-id="f242e-107">Při nastavení **EnablePackageProcessLoggingAndAssert** vlastnost **true**, bude nástroj MSBuild:</span><span class="sxs-lookup"><span data-stu-id="f242e-107">When you set the **EnablePackageProcessLoggingAndAssert** property to **true**, MSBuild will:</span></span>
> 
> - <span data-ttu-id="f242e-108">Přidejte další informace o procesu balení do protokolů o sestavení.</span><span class="sxs-lookup"><span data-stu-id="f242e-108">Add additional information about the packaging process to the build logs.</span></span>
> - <span data-ttu-id="f242e-109">Protokolování chyb za určitých podmínek, například pokud duplicitních souborů se nenachází v seznamu balení.</span><span class="sxs-lookup"><span data-stu-id="f242e-109">Log errors under certain conditions, for example, if duplicate files are found in the packaging list.</span></span>
> - <span data-ttu-id="f242e-110">Vytvořte adresář protokolů v *ProjectName*\_balíček složky a použít ho k zaznamenání informací o souborech, které jste balení.</span><span class="sxs-lookup"><span data-stu-id="f242e-110">Create a Log directory in the *ProjectName*\_Package folder and use it to record information about the files you're packaging.</span></span>
> 
> <span data-ttu-id="f242e-111">Pokud proces vytváření balíčku se nedaří nebo balíčky pro nasazení vaší webové neobsahují soubory, které jste očekávali, můžete použít tyto informace k řešení potíží s procesem a pinpoint kde neuvádíme nesprávné.</span><span class="sxs-lookup"><span data-stu-id="f242e-111">If the packaging process is failing, or your web deployment packages don't contain the files that you expect, you can use this information to troubleshoot the process and pinpoint where things are going wrong.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="f242e-112">**EnablePackageProcessLoggingAndAssert** vlastnost funguje jenom v případě sestavení projektu pomocí **ladění** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f242e-112">The **EnablePackageProcessLoggingAndAssert** property only works if you build your project using the **Debug** configuration.</span></span> <span data-ttu-id="f242e-113">Vlastnost je ignorována v jiné konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f242e-113">The property is ignored in other configurations.</span></span>


<span data-ttu-id="f242e-114">Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.</span><span class="sxs-lookup"><span data-stu-id="f242e-114">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="f242e-115">Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení.</span><span class="sxs-lookup"><span data-stu-id="f242e-115">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="f242e-116">V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.</span><span class="sxs-lookup"><span data-stu-id="f242e-116">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a><span data-ttu-id="f242e-117">Principy vlastnost EnablePackageProcessLoggingAndAssert</span><span class="sxs-lookup"><span data-stu-id="f242e-117">Understanding the EnablePackageProcessLoggingAndAssert Property</span></span>

<span data-ttu-id="f242e-118">[Sestavení a balení projektů webových aplikací](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) popsané, jak webové publikování kanálu (WPP) poskytuje sadu cílů MSBuild, které rozšiřují funkce nástroje MSBuild a umožňuje integraci s webovým rozhraním Internetové informační služby (IIS) Nástroj pro nasazení (Web Deploy).</span><span class="sxs-lookup"><span data-stu-id="f242e-118">[Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) described how the Web Publishing Pipeline (WPP) provides a set of MSBuild targets that extend the functionality of MSBuild and enable it to integrate with the Internet Information Services (IIS) Web Deployment Tool (Web Deploy).</span></span> <span data-ttu-id="f242e-119">Když vytvoříte balíček projektu webové aplikace, volání WPP cíle.</span><span class="sxs-lookup"><span data-stu-id="f242e-119">When you package a web application project, you're invoking WPP targets.</span></span>

<span data-ttu-id="f242e-120">Spoustu těchto cílů WPP zahrnují podmíněnou logiku, která ukládá do protokolu Další informace při **EnablePackageProcessLoggingAndAssert** je nastavena na **true**.</span><span class="sxs-lookup"><span data-stu-id="f242e-120">Lots of these WPP targets include conditional logic that logs additional information when the **EnablePackageProcessLoggingAndAssert** property is set to **true**.</span></span> <span data-ttu-id="f242e-121">Například, když se podíváte **balíčku** cíl, můžete vidět, že vytvoří adresář služby dalších protokolů a zapíše seznam souborů do textového souboru, pokud **EnablePackageProcessLoggingAndAssert** rovná **true**.</span><span class="sxs-lookup"><span data-stu-id="f242e-121">For example, if you review the **Package** target, you can see that it creates an additional log directory and writes a list of files to a text file if **EnablePackageProcessLoggingAndAssert** is equal to **true**.</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> <span data-ttu-id="f242e-122">WPP cíle, které jsou definovány v *Microsoft.Web.Publishing.targets* souboru ve složce % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.</span><span class="sxs-lookup"><span data-stu-id="f242e-122">The WPP targets are defined in the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span> <span data-ttu-id="f242e-123">Můžete otevřít tento soubor a zkontrolujte cílů v sadě Visual Studio 2010 nebo editoru XML.</span><span class="sxs-lookup"><span data-stu-id="f242e-123">You can open this file and review the targets in Visual Studio 2010 or any XML editor.</span></span> <span data-ttu-id="f242e-124">Zajistíme, abyste neupravovali obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="f242e-124">Take care not to modify the contents of the file.</span></span>


## <a name="enabling-the-additional-logging"></a><span data-ttu-id="f242e-125">Povolení dodatečné protokolování</span><span class="sxs-lookup"><span data-stu-id="f242e-125">Enabling the Additional Logging</span></span>

<span data-ttu-id="f242e-126">Můžete zadat hodnotu **EnablePackageProcessLoggingAndAssert** vlastnost různými způsoby v závislosti na tom, jak svůj projekt sestavit.</span><span class="sxs-lookup"><span data-stu-id="f242e-126">You can supply a value for the **EnablePackageProcessLoggingAndAssert** property in various ways, depending on how you build your project.</span></span>

<span data-ttu-id="f242e-127">Pokud se sestavení projektu z příkazového řádku můžete zadat hodnotu **EnablePackageProcessLoggingAndAssert** vlastnost jako argument příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="f242e-127">If you build your project from the command line, you can supply a value for the **EnablePackageProcessLoggingAndAssert** property as a command-line argument:</span></span>


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


<span data-ttu-id="f242e-128">Pokud používáte soubor vlastních projektů k sestavení projektů, můžete zahrnout **EnablePackageProcessLoggingAndAssert** hodnotu **vlastnosti** atribut **MSBuild**úloh:</span><span class="sxs-lookup"><span data-stu-id="f242e-128">If you're using a custom project file to build your projects, you can include the **EnablePackageProcessLoggingAndAssert** value in the **Properties** attribute of the **MSBuild** task:</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


<span data-ttu-id="f242e-129">Pokud používáte definici sestavení Team Foundation Server (TFS) k sestavení projektů, můžete zadat hodnotu **EnablePackageProcessLoggingAndAssert** vlastnost **argumenty nástroje MSBuild** řádek:![](troubleshooting-the-packaging-process/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f242e-129">If you're using a Team Foundation Server (TFS) build definition to build your projects, you can supply a value for the **EnablePackageProcessLoggingAndAssert** property in the **MSBuild Arguments** row:![](troubleshooting-the-packaging-process/_static/image1.png)</span></span>

> [!NOTE]
> <span data-ttu-id="f242e-130">Další informace o vytváření a konfiguraci definic sestavení, naleznete v tématu [vytvoření sestavení definice, že podporuje nasazení](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="f242e-130">For more information on creating and configuring build definitions, see [Creating a Build Definition That Supports Deployment](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).</span></span>


<span data-ttu-id="f242e-131">Případně, pokud chcete, aby zahrnovaly balíček v každém sestavení, můžete upravit soubor projektu pro webový projekt aplikace nastavit **EnablePackageProcessLoggingAndAssert** vlastnost **true**.</span><span class="sxs-lookup"><span data-stu-id="f242e-131">Alternatively, if you want to include the package in every build, you can modify the project file for your web application project to set the **EnablePackageProcessLoggingAndAssert** property to **true**.</span></span> <span data-ttu-id="f242e-132">Měli byste přidat vlastnost na první **PropertyGroup** element v souboru .csproj nebo .vbproj.</span><span class="sxs-lookup"><span data-stu-id="f242e-132">You should add the property to the first **PropertyGroup** element within your .csproj or .vbproj file.</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a><span data-ttu-id="f242e-133">Kontrola souborů protokolu</span><span class="sxs-lookup"><span data-stu-id="f242e-133">Reviewing the Log Files</span></span>

<span data-ttu-id="f242e-134">Při sestavení a zabalení webové aplikace s **EnablePackageProcessLoggingAndAssert** nastavena na **true**, MSBuild vytvoří další složku s názvem protokolu v *ProjectName* \_Složky balíčku.</span><span class="sxs-lookup"><span data-stu-id="f242e-134">When you build and package a web application project with **EnablePackageProcessLoggingAndAssert** set to **true**, MSBuild creates an additional folder named Log in the *ProjectName*\_Package folder.</span></span> <span data-ttu-id="f242e-135">Složka protokolu obsahuje různé soubory:</span><span class="sxs-lookup"><span data-stu-id="f242e-135">The Log folder contains various files:</span></span>

![](troubleshooting-the-packaging-process/_static/image2.png)

<span data-ttu-id="f242e-136">Seznam souborů, které jste se bude lišit podle věci v projektu a procesu sestavení.</span><span class="sxs-lookup"><span data-stu-id="f242e-136">The list of files that you see will vary according to the things in your project and your build process.</span></span> <span data-ttu-id="f242e-137">Tyto soubory jsou obvykle používá k zaznamenání seznamu souborů, které shromažďuje WPP balení v různých fázích procesu:</span><span class="sxs-lookup"><span data-stu-id="f242e-137">However, these files are typically used to record the list of files that the WPP is collecting for packaging, at various stages of the process:</span></span>

- <span data-ttu-id="f242e-138">*PreExcludePipelineCollectFilesPhaseFileList.txt* soubor obsahuje seznam souborů, které shromažďuje MSBuild pro vytváření balíčků před odstraněním všechny soubory, které jsou určené pro vyloučení.</span><span class="sxs-lookup"><span data-stu-id="f242e-138">The *PreExcludePipelineCollectFilesPhaseFileList.txt* file lists the files that MSBuild collects for packaging before any files that are specified for exclusion are removed.</span></span>
- <span data-ttu-id="f242e-139">*AfterExcludeFilesFilesList.txt* soubor obsahuje seznam upravený soubor po odebrání všechny soubory, které jsou určené pro vyloučení.</span><span class="sxs-lookup"><span data-stu-id="f242e-139">The *AfterExcludeFilesFilesList.txt* file contains the modified file list after any files that are specified for exclusion are removed.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f242e-140">Další informace o vyloučení souborů a složek z procesem vytváření balíčku najdete v tématu [vyloučení souborů a složek z nasazení](excluding-files-and-folders-from-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="f242e-140">For more information on excluding files and folders from the packaging process, see [Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span>
- <span data-ttu-id="f242e-141">*AfterTransformWebConfig.txt* soubor seznamy pro balení po všech shromážděných souborů *Web.config* transformace byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="f242e-141">The *AfterTransformWebConfig.txt* file lists the files collected for packaging after any *Web.config* transforms have been performed.</span></span> <span data-ttu-id="f242e-142">V tomto seznamu všech určených pro konfigurace *Web.config* transformační soubory, jako je třeba *Web.Debug.config* a *Web.Release.config*, nezahrnou se seznam souborů pro balení.</span><span class="sxs-lookup"><span data-stu-id="f242e-142">In this list, any configuration-specific *Web.config* transform files, like *Web.Debug.config* and *Web.Release.config*, are excluded from the list of files for packaging.</span></span> <span data-ttu-id="f242e-143">Jediný transformuje *Web.config* je součástí jejich místo.</span><span class="sxs-lookup"><span data-stu-id="f242e-143">A single transformed *Web.config* is included in their place.</span></span>
- <span data-ttu-id="f242e-144">*PostAutoParameterizationWebConfigConnectionStrings.txt* soubor obsahuje seznam souborů, které po připojovací řetězce v *Web.config* soubor mít parametrizované.</span><span class="sxs-lookup"><span data-stu-id="f242e-144">The *PostAutoParameterizationWebConfigConnectionStrings.txt* file contains the list of files after the connection strings in the *Web.config* file have been parameterized.</span></span> <span data-ttu-id="f242e-145">To je proces, který umožňuje při nasazení balíčku nahradit připojovací řetězce správné nastavení pro cílové prostředí.</span><span class="sxs-lookup"><span data-stu-id="f242e-145">This is the process that lets you replace your connection strings with the right settings for your target environment when you deploy the package.</span></span>
- <span data-ttu-id="f242e-146">*Prepackage.txt* soubor obsahuje dokončené před sestavením seznam souborů, které mají být součástí balíčku.</span><span class="sxs-lookup"><span data-stu-id="f242e-146">The *Prepackage.txt* file contains the finalized pre-build list of files to be included in the package.</span></span>

> [!NOTE]
> <span data-ttu-id="f242e-147">Názvy souborů dalších protokolů obvykle odpovídají WPP cíle.</span><span class="sxs-lookup"><span data-stu-id="f242e-147">The names of the additional log files typically correspond to WPP targets.</span></span> <span data-ttu-id="f242e-148">Můžete zkontrolovat tyto cíle prozkoumáním *Microsoft.Web.Publishing.targets* souboru ve složce % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.</span><span class="sxs-lookup"><span data-stu-id="f242e-148">You can review these targets by examining the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span>


<span data-ttu-id="f242e-149">Pokud obsah webového balíčku se, co jste očekávali, kontrola těchto souborů může být užitečný způsob, jak identifikovat na jaké bodu ve věci procesu došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="f242e-149">If the contents of your web package aren't what you expected, reviewing these files can be a useful way to identify at what point in the process things went wrong.</span></span>

## <a name="conclusion"></a><span data-ttu-id="f242e-150">Závěr</span><span class="sxs-lookup"><span data-stu-id="f242e-150">Conclusion</span></span>

<span data-ttu-id="f242e-151">Toto téma popisuje, jak můžete použít **EnablePackageProcessLoggingAndAssert** vlastnost MSBuild k řešení potíží s procesem vytváření balíčku.</span><span class="sxs-lookup"><span data-stu-id="f242e-151">This topic described how you can use the **EnablePackageProcessLoggingAndAssert** property in MSBuild to troubleshoot the packaging process.</span></span> <span data-ttu-id="f242e-152">Vysvětlení různých způsobů, ve kterém můžete zadat hodnotu vlastnosti, aby proces sestavení, a popisuje další informace, které zaznamenává, když nastavíte vlastnost na **true**.</span><span class="sxs-lookup"><span data-stu-id="f242e-152">It explained the different ways in which you can supply the property value to the build process, and it described the additional information that is recorded when you set the property to **true**.</span></span>

## <a name="further-reading"></a><span data-ttu-id="f242e-153">Další čtení</span><span class="sxs-lookup"><span data-stu-id="f242e-153">Further Reading</span></span>

<span data-ttu-id="f242e-154">Další informace o používání vlastní soubory projektu MSBuild k řízení procesu nasazení najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="f242e-154">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="f242e-155">Další informace o WPP a jak spravuje proces vytváření balíčku naleznete v tématu [sestavení a balení projektů webových aplikací](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span><span class="sxs-lookup"><span data-stu-id="f242e-155">For more information on the WPP and how it manages the packaging process, see [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span></span> <span data-ttu-id="f242e-156">Informace o tom, jak vyloučit určité soubory a složky z balíčky nasazení webu najdete v tématu [vyloučení souborů a složek z nasazení](excluding-files-and-folders-from-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="f242e-156">For guidance on how to exclude specific files and folders from web deployment packages, see [Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f242e-157">Předchozí</span><span class="sxs-lookup"><span data-stu-id="f242e-157">Previous</span></span>](running-windows-powershell-scripts-from-msbuild-project-files.md)
