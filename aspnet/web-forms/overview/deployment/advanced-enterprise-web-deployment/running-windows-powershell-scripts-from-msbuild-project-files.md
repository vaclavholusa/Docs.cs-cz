---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Spouštění skriptů Windows Powershellu ze souborů projektu MSBuild | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak spustit skript prostředí Windows PowerShell jako součást procesu sestavení a nasazení. Skript můžete spustit místně (jinými slovy na b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: ddb658d8a8f224a7c417321df3e17ce0610d2473
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362892"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="12f12-104">Spouštění skriptů Windows Powershellu ze souborů projektu MSBuild</span><span class="sxs-lookup"><span data-stu-id="12f12-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="12f12-105">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="12f12-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="12f12-106">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="12f12-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="12f12-107">Toto téma popisuje, jak spustit skript prostředí Windows PowerShell jako součást procesu sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="12f12-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="12f12-108">Můžete spustit skript místně (jinými slovy, na serveru sestavení) nebo vzdáleně, třeba na cílový webový server nebo databázový server.</span><span class="sxs-lookup"><span data-stu-id="12f12-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="12f12-109">Existuje mnoho důvodů, proč můžete chtít spustit skript prostředí Windows PowerShell po nasazení.</span><span class="sxs-lookup"><span data-stu-id="12f12-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="12f12-110">Například můžete chtít:</span><span class="sxs-lookup"><span data-stu-id="12f12-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="12f12-111">Přidání zdroje vlastní událost do registru.</span><span class="sxs-lookup"><span data-stu-id="12f12-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="12f12-112">Generovat adresář systému souborů pro nahrávání.</span><span class="sxs-lookup"><span data-stu-id="12f12-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="12f12-113">Čištění adresáře sestavení.</span><span class="sxs-lookup"><span data-stu-id="12f12-113">Clean up build directories.</span></span>
> - <span data-ttu-id="12f12-114">Zápis položky do vlastního souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="12f12-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="12f12-115">Odesílání e-mailů pozvání uživatelů do nově zřízeného webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="12f12-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="12f12-116">Vytvořte uživatelské účty s příslušnými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="12f12-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="12f12-117">Konfigurace replikace mezi instancí systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="12f12-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="12f12-118">Toto téma se zobrazí spuštění skriptů Windows Powershellu z vlastního cíle v souboru projektu Microsoft Build Engine (MSBuild) místně i vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="12f12-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="12f12-119">Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.</span><span class="sxs-lookup"><span data-stu-id="12f12-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="12f12-120">Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení.</span><span class="sxs-lookup"><span data-stu-id="12f12-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="12f12-121">V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.</span><span class="sxs-lookup"><span data-stu-id="12f12-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="12f12-122">Přehled úloh</span><span class="sxs-lookup"><span data-stu-id="12f12-122">Task Overview</span></span>

<span data-ttu-id="12f12-123">Chcete-li spustit skript prostředí Windows PowerShell jako součást procesu krokování nebo automatizované nasazení, budete potřebovat k dokončení těchto úloh:</span><span class="sxs-lookup"><span data-stu-id="12f12-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="12f12-124">Přidáte skript prostředí Windows PowerShell do svého řešení a do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="12f12-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="12f12-125">Vytvořte příkaz, který vyvolává skript prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="12f12-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="12f12-126">Řídicí vyhrazené znaky XML ve svých rukou.</span><span class="sxs-lookup"><span data-stu-id="12f12-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="12f12-127">Vytvořit cíl v souboru projektu MSBuild vlastní a použít **Exec** úkolů ke spuštění příkazu.</span><span class="sxs-lookup"><span data-stu-id="12f12-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="12f12-128">Toto téma se ukazují, jak provést tyto postupy.</span><span class="sxs-lookup"><span data-stu-id="12f12-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="12f12-129">Úlohy a názorné postupy v tomto tématu se předpokládá, že jste již obeznámeni s MSBuild cíle a vlastnosti, a že vám pochopit, jak můžete použít vlastní soubor projektu MSBuild k řízení procesu sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="12f12-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="12f12-130">Další informace najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="12f12-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="12f12-131">Vytváření a přidávání skriptů Windows Powershellu</span><span class="sxs-lookup"><span data-stu-id="12f12-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="12f12-132">Úlohy v tomto tématu použijte ukázkový skript prostředí Windows PowerShell s názvem **LogDeploy.ps1** si ukážeme, jak spouštět skripty MSBuild.</span><span class="sxs-lookup"><span data-stu-id="12f12-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="12f12-133">**LogDeploy.ps1** skript obsahuje jednoduchou funkci, která zapíše položku jedním řádkem do souboru protokolu:</span><span class="sxs-lookup"><span data-stu-id="12f12-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


<span data-ttu-id="12f12-134">**LogDeploy.ps1** skript přijímá dva parametry.</span><span class="sxs-lookup"><span data-stu-id="12f12-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="12f12-135">První parametr představuje úplnou cestu k souboru protokolu, do které chcete přidat položku, a druhý parametr představuje cíl nasazení, který chcete nahrát do souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="12f12-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="12f12-136">Když spustíte skript, přidá nový řádek do souboru protokolu v tomto formátu:</span><span class="sxs-lookup"><span data-stu-id="12f12-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="12f12-137">Chcete-li **LogDeploy.ps1** skript k dispozici pro MSBuild, budete muset:</span><span class="sxs-lookup"><span data-stu-id="12f12-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="12f12-138">Přidáte skript do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="12f12-138">Add the script to source control.</span></span>
- <span data-ttu-id="12f12-139">Přidáte skript do svého řešení v sadě Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="12f12-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="12f12-140">Není nutné k nasazení skriptu s obsahem vašeho řešení, bez ohledu na to, zda máte v plánu spuštění skriptu na serveru sestavení nebo ve vzdáleném počítači.</span><span class="sxs-lookup"><span data-stu-id="12f12-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="12f12-141">Jednou z možností je přidat skript do složky řešení.</span><span class="sxs-lookup"><span data-stu-id="12f12-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="12f12-142">V příkladu správce kontaktů vzhledem k tomu, že chcete použít skript prostředí Windows PowerShell jako součást procesu nasazení je vhodné přidat skript do složky řešení publikovat.</span><span class="sxs-lookup"><span data-stu-id="12f12-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="12f12-143">Obsah složky řešení se zkopírují na servery sestavení jako zdrojový materiál.</span><span class="sxs-lookup"><span data-stu-id="12f12-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="12f12-144">Nicméně jsou součástí žádné žádný výstup projektu.</span><span class="sxs-lookup"><span data-stu-id="12f12-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="12f12-145">Provádění skriptu prostředí Windows PowerShell na serveru sestavení</span><span class="sxs-lookup"><span data-stu-id="12f12-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="12f12-146">V některých případech můžete chtít spustit skripty Windows Powershellu na počítači, který sestaví vaše projekty.</span><span class="sxs-lookup"><span data-stu-id="12f12-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="12f12-147">Například můžete použít skript prostředí Windows PowerShell pro vyčištění sestavení složky nebo zápis položky do vlastního souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="12f12-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="12f12-148">Z hlediska syntaxi skriptu Windows Powershellu ze souboru projektu MSBuild je stejný jako regulární příkazovém řádku spustíte skript prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="12f12-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="12f12-149">Je nutné vyvolat powershell.exe spustitelný soubor a použít **– příkaz** přepínač k poskytování příkazů Windows Powershellu, aby fungoval.</span><span class="sxs-lookup"><span data-stu-id="12f12-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="12f12-150">(V prostředí Windows PowerShell verze 2, můžete také použít **– soubor** přepínače).</span><span class="sxs-lookup"><span data-stu-id="12f12-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="12f12-151">Příkaz by měl provést tento formát:</span><span class="sxs-lookup"><span data-stu-id="12f12-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="12f12-152">Příklad:</span><span class="sxs-lookup"><span data-stu-id="12f12-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="12f12-153">Pokud cesta ke skriptu obsahuje mezery, budete muset uzavřít cesta k souboru v jednoduchých uvozovkách předchází znak ampersand.</span><span class="sxs-lookup"><span data-stu-id="12f12-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="12f12-154">Dvojité uvozovky, nemůžete použít, protože jste už použili k uzavření příkazu:</span><span class="sxs-lookup"><span data-stu-id="12f12-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="12f12-155">Existuje několik dalších důležitých informací při vyvolání tento příkaz MSBuild.</span><span class="sxs-lookup"><span data-stu-id="12f12-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="12f12-156">Nejprve, měli byste zahrnout **– NonInteractive** příznak Ujistěte se, že se skript spustí tiše.</span><span class="sxs-lookup"><span data-stu-id="12f12-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="12f12-157">V dalším kroku, měli byste zahrnout **– ExecutionPolicy** příznak s hodnotou odpovídající argument.</span><span class="sxs-lookup"><span data-stu-id="12f12-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="12f12-158">Toto nastavení určuje zásady spouštění, že bude vztahovat na váš skript prostředí Windows PowerShell a umožňuje potlačit výchozí zásadu spouštění, což může zabránit spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="12f12-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="12f12-159">Můžete vybrat z těchto argumentů:</span><span class="sxs-lookup"><span data-stu-id="12f12-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="12f12-160">Hodnota **Unrestricted** vám umožní prostředí Windows PowerShell pro spuštění vašeho skriptu, bez ohledu na to, jestli je skript podepsán.</span><span class="sxs-lookup"><span data-stu-id="12f12-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="12f12-161">Hodnota **RemoteSigned** vám umožní prostředí Windows PowerShell ke spouštění nepodepsaných skriptů, které byly vytvořeny na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="12f12-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="12f12-162">Však musí být podepsané skripty, které byly vytvořeny jinde.</span><span class="sxs-lookup"><span data-stu-id="12f12-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="12f12-163">(V praxi, jste velmi nepravděpodobné, že jste vytvořili skript prostředí Windows PowerShell místně na serveru sestavení).</span><span class="sxs-lookup"><span data-stu-id="12f12-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="12f12-164">Hodnota **AllSigned** vám umožní spustit jenom podepsané skripty Windows Powershellu.</span><span class="sxs-lookup"><span data-stu-id="12f12-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="12f12-165">Je výchozí zásadu spouštění **s omezeným přístupem**, která zabrání spuštění jakékoli soubory skriptů prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="12f12-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="12f12-166">Nakonec musíte řídicí vyhrazené znaky XML, ke kterým dochází v příkazu Windows Powershellu:</span><span class="sxs-lookup"><span data-stu-id="12f12-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="12f12-167">Nahraďte jednoduchých uvozovek a být s  **&amp;apos;**</span><span class="sxs-lookup"><span data-stu-id="12f12-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="12f12-168">Nahraďte dvojité uvozovky se  **&amp;quot;**</span><span class="sxs-lookup"><span data-stu-id="12f12-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="12f12-169">Nahraďte tyto znaky s  **&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="12f12-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="12f12-170">Pokud provedete tyto změny, bude příkaz vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="12f12-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="12f12-171">V rámci vlastního souboru projektu MSBuild, můžete vytvořit nový cíl a použít **Exec** úkolů ke spuštění tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="12f12-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="12f12-172">V tomto příkladu Všimněte si, že:</span><span class="sxs-lookup"><span data-stu-id="12f12-172">In this example, note that:</span></span>

- <span data-ttu-id="12f12-173">Všechny proměnné, jako jsou hodnoty parametrů a umístění ke spustitelnému souboru prostředí Windows PowerShell jsou deklarovány jako vlastnosti nástroje MSBuild.</span><span class="sxs-lookup"><span data-stu-id="12f12-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="12f12-174">Podmínky jsou zahrnuty umožňující uživatelům přepsat tyto hodnoty z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="12f12-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="12f12-175">**MSDeployComputerName** vlastnost je deklarován jinde v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="12f12-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="12f12-176">Při spouštění tohoto cíle jako součást procesu sestavení spustí příkazu prostředí Windows PowerShell a zapisovat položky protokolu do souboru, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="12f12-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="12f12-177">Provádění skriptu prostředí Windows PowerShell ve vzdáleném počítači</span><span class="sxs-lookup"><span data-stu-id="12f12-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="12f12-178">Prostředí Windows PowerShell je schopen spuštění skriptů ve vzdálených počítačích prostřednictvím [Vzdálená správa Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="12f12-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="12f12-179">K tomuto účelu, budete muset použít [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="12f12-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="12f12-180">To umožňuje spuštění skriptu na jeden nebo více vzdálených počítačů bez kopírování skript na vzdálených počítačích.</span><span class="sxs-lookup"><span data-stu-id="12f12-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="12f12-181">Všechny výsledky jsou vráceny do místního počítače, ze kterého jste spustili skript.</span><span class="sxs-lookup"><span data-stu-id="12f12-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="12f12-182">Než použijete **Invoke-Command** rutiny prostředí Windows PowerShell spouštět skripty ve vzdáleném počítači, je nutné nakonfigurovat naslouchací proces služby WinRM tak, aby přijímal vzdálená zprávy.</span><span class="sxs-lookup"><span data-stu-id="12f12-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="12f12-183">Můžete to provést spuštěním příkazu **winrm quickconfig** na vzdáleném počítači.</span><span class="sxs-lookup"><span data-stu-id="12f12-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="12f12-184">Další informace najdete v tématu [instalace a konfigurace pro Windows Vzdálená správa](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="12f12-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="12f12-185">V okně Windows Powershellu, můžete využít tuto syntaxi pro spuštění **LogDeploy.ps1** skriptů ve vzdáleném počítači:</span><span class="sxs-lookup"><span data-stu-id="12f12-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="12f12-186">Existují různé způsoby použití **Invoke-Command** ke spuštění skriptu je nejvíc přímočarý soubor, ale tento přístup při je potřeba zadat hodnoty parametrů a spravovat cesty obsahující mezery.</span><span class="sxs-lookup"><span data-stu-id="12f12-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="12f12-187">Pokud při spuštění z příkazového řádku, je potřeba vyvolat spustitelný soubor Windows Powershellu a použít **– příkaz** parametr vaše pokyny:</span><span class="sxs-lookup"><span data-stu-id="12f12-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="12f12-188">Jako dříve, budete muset zadat nějaké další přepínače a řídicí vyhrazené znaky XML, když spustíte příkaz MSBUILD:</span><span class="sxs-lookup"><span data-stu-id="12f12-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="12f12-189">A konečně, stejně jako dříve, můžete použít **Exec** úkol v rámci vlastního cíle nástroje MSBuild pro provedení příkazu:</span><span class="sxs-lookup"><span data-stu-id="12f12-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="12f12-190">Při spouštění tohoto cíle jako součást procesu sestavení prostředí Windows PowerShell se spustí skript na počítači zadaném v **– computername** argument.</span><span class="sxs-lookup"><span data-stu-id="12f12-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="12f12-191">Závěr</span><span class="sxs-lookup"><span data-stu-id="12f12-191">Conclusion</span></span>

<span data-ttu-id="12f12-192">Toto téma popisuje, jak spustit skript prostředí Windows PowerShell ze souboru projektu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="12f12-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="12f12-193">Tento přístup můžete použít ke spuštění skriptu prostředí Windows PowerShell buď místně nebo ve vzdáleném počítači, jako součást krokování nebo automatizovaného sestavení a nasazení procesu.</span><span class="sxs-lookup"><span data-stu-id="12f12-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="12f12-194">Další čtení</span><span class="sxs-lookup"><span data-stu-id="12f12-194">Further Reading</span></span>

<span data-ttu-id="12f12-195">Pokyny k podepisování skriptů prostředí Windows PowerShell a správou zásad v provádění, naleznete v tématu [spouštění skriptů Windows Powershellu](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="12f12-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="12f12-196">Informace o spouštění příkazů prostředí Windows PowerShell ze vzdáleného počítače najdete v tématu [spouštění vzdálených příkazů](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="12f12-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="12f12-197">Další informace o používání vlastní soubory projektu MSBuild k řízení procesu nasazení najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="12f12-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="12f12-198">[Předchozí](taking-web-applications-offline-with-web-deploy.md)
> [další](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="12f12-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
