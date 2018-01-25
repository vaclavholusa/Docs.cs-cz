---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: "Spuštěné skripty prostředí PowerShell systému Windows ze souborů projektu nástroje MSBuild | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje, jak spustit skript prostředí Windows PowerShell jako součást procesu sestavení a nasazení. Můžete spustit skript místně (jinými slovy, na b..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: afee7b0621df42a8bc70fc6f7c4a8fd0383fa83a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="75dcc-104">Spuštěné skripty prostředí PowerShell systému Windows ze souborů projektu nástroje MSBuild</span><span class="sxs-lookup"><span data-stu-id="75dcc-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="75dcc-105">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="75dcc-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="75dcc-106">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="75dcc-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="75dcc-107">Toto téma popisuje, jak spustit skript prostředí Windows PowerShell jako součást procesu sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="75dcc-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="75dcc-108">Můžete spustit skript místně (jinými slovy, na serveru sestavení) nebo vzdáleně, jako na cílový webový server nebo serveru databáze.</span><span class="sxs-lookup"><span data-stu-id="75dcc-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="75dcc-109">Existuje mnoho důvodů, proč můžete chtít spustit skript prostředí Windows PowerShell po nasazení.</span><span class="sxs-lookup"><span data-stu-id="75dcc-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="75dcc-110">Například můžete chtít:</span><span class="sxs-lookup"><span data-stu-id="75dcc-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="75dcc-111">Přidání vlastních událostí zdroje do registru.</span><span class="sxs-lookup"><span data-stu-id="75dcc-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="75dcc-112">Generovat adresář systému souborů pro nahrávání.</span><span class="sxs-lookup"><span data-stu-id="75dcc-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="75dcc-113">Vyčištění sestavení adresáře.</span><span class="sxs-lookup"><span data-stu-id="75dcc-113">Clean up build directories.</span></span>
> - <span data-ttu-id="75dcc-114">Položky k zápisu do vlastního souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="75dcc-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="75dcc-115">Odesílání e-mailů pozvat uživatele na nově zřízeného webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="75dcc-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="75dcc-116">Vytvořte uživatelské účty s příslušnými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="75dcc-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="75dcc-117">Konfiguraci replikace mezi instancí systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="75dcc-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="75dcc-118">Toto téma vám ukáže, jak ke spouštění skriptů prostředí Windows PowerShell místně i vzdáleně z vlastní cíl v souboru projektu Microsoft Build Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="75dcc-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="75dcc-119">Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz series používá ukázkové řešení & #x 2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3 systému Windows Komunikační služby Foundation (WCF) a projekt databáze.</span><span class="sxs-lookup"><span data-stu-id="75dcc-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="75dcc-120">Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízené procesu sestavení dva projektu soubory & #x 2014; jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="75dcc-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="75dcc-121">V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.</span><span class="sxs-lookup"><span data-stu-id="75dcc-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="75dcc-122">Přehled úloh</span><span class="sxs-lookup"><span data-stu-id="75dcc-122">Task Overview</span></span>

<span data-ttu-id="75dcc-123">Pokud chcete spustit skript prostředí Windows PowerShell jako součást procesu nasazení automatizované nebo jeden krok, budete potřebovat k dokončení těchto úloh vysoké úrovně:</span><span class="sxs-lookup"><span data-stu-id="75dcc-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="75dcc-124">Přidejte skript prostředí Windows PowerShell pro vaše řešení a k řízení zdrojů.</span><span class="sxs-lookup"><span data-stu-id="75dcc-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="75dcc-125">Vytvořte příkaz, který vyvolává skript prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75dcc-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="75dcc-126">Escape všechny vyhrazené znaky jazyka XML v příkazu.</span><span class="sxs-lookup"><span data-stu-id="75dcc-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="75dcc-127">Vytvořit cíl ve vaší vlastní soubor projektu nástroje MSBuild a použít **Exec** úloha pro spuštění příkazu.</span><span class="sxs-lookup"><span data-stu-id="75dcc-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="75dcc-128">Toto téma vám ukáže, jak k provedení těchto postupů.</span><span class="sxs-lookup"><span data-stu-id="75dcc-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="75dcc-129">Postupy v tomto tématu a úlohy předpokládají, že jste již obeznámeni s cíle MSBuild a vlastnosti a víte, jak používat vlastní soubor projektu nástroje MSBuild k řízení procesem sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="75dcc-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="75dcc-130">Další informace najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="75dcc-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="75dcc-131">Vytvoření a přidání skriptů prostředí PowerShell systému Windows</span><span class="sxs-lookup"><span data-stu-id="75dcc-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="75dcc-132">Úkoly v tomto tématu použijte ukázkový skript prostředí Windows PowerShell s názvem **LogDeploy.ps1** k ukazují, jak spouštět skripty z nástroje MSBuild.</span><span class="sxs-lookup"><span data-stu-id="75dcc-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="75dcc-133">**LogDeploy.ps1** skript obsahuje jednoduché funkce, který zapíše položku jeden řádek do souboru protokolu:</span><span class="sxs-lookup"><span data-stu-id="75dcc-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


<span data-ttu-id="75dcc-134">**LogDeploy.ps1** skriptu přijímá dva parametry.</span><span class="sxs-lookup"><span data-stu-id="75dcc-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="75dcc-135">První parametr představuje úplnou cestu k souboru protokolu, do které chcete přidat položku a druhý parametr představuje cíl nasazení, který chcete záznam v souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="75dcc-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="75dcc-136">Když spustíte skript, přidá řádek do souboru protokolu v tomto formátu:</span><span class="sxs-lookup"><span data-stu-id="75dcc-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="75dcc-137">Chcete-li **LogDeploy.ps1** skript k dispozici pro MSBuild, budete muset:</span><span class="sxs-lookup"><span data-stu-id="75dcc-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="75dcc-138">Přidejte skript do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="75dcc-138">Add the script to source control.</span></span>
- <span data-ttu-id="75dcc-139">Přidejte skript do řešení v sadě Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="75dcc-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="75dcc-140">Nemusíte nasazovat skriptu pomocí řešení obsah, bez ohledu na to, jestli chcete spusťte tento skript na serveru sestavení nebo na vzdáleném počítači.</span><span class="sxs-lookup"><span data-stu-id="75dcc-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="75dcc-141">Jednou z možností je přidat skript do složky řešení.</span><span class="sxs-lookup"><span data-stu-id="75dcc-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="75dcc-142">V příkladu obraťte se na správce vzhledem k tomu, že chcete použít skript prostředí Windows PowerShell jako součást procesu nasazení, má smysl přidat skript do složky publikovat řešení.</span><span class="sxs-lookup"><span data-stu-id="75dcc-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="75dcc-143">Obsah složky řešení se zkopírují do sestavení servery jako zdroj materiálu.</span><span class="sxs-lookup"><span data-stu-id="75dcc-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="75dcc-144">Tvoří ale žádná z částí žádný výstup projektu.</span><span class="sxs-lookup"><span data-stu-id="75dcc-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="75dcc-145">Provádění skriptu prostředí Windows PowerShell na serveru sestavení</span><span class="sxs-lookup"><span data-stu-id="75dcc-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="75dcc-146">V některých případech můžete spouštět skripty Windows PowerShell v počítači, který sestaví vašich projektů.</span><span class="sxs-lookup"><span data-stu-id="75dcc-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="75dcc-147">Můžete například použít skript prostředí Windows PowerShell k vyčištění sestavení složek nebo o zapsání položky do vlastního souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="75dcc-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="75dcc-148">Z hlediska syntaxe z soubor projektu nástroje MSBuild spustíte skript prostředí Windows PowerShell je stejný jako regulární příkazovém řádku spustíte skript prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75dcc-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="75dcc-149">Je nutné vyvolat spustitelný soubor powershell.exe a použít **– příkaz** přepínač tak, aby poskytují příkazy, které chcete spustit prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75dcc-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="75dcc-150">(V prostředí Windows PowerShell v2, můžete také použít **– soubor** přepínače).</span><span class="sxs-lookup"><span data-stu-id="75dcc-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="75dcc-151">Příkaz měla mít tento formát:</span><span class="sxs-lookup"><span data-stu-id="75dcc-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="75dcc-152">Příklad:</span><span class="sxs-lookup"><span data-stu-id="75dcc-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="75dcc-153">Pokud cesta k váš skript obsahuje mezery, budete muset uzavřete cestu k souboru v jednoduchých uvozovkách ampersand před sebou.</span><span class="sxs-lookup"><span data-stu-id="75dcc-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="75dcc-154">Dvojité uvozovky, nemůžete použít, protože jste je již použit pro uzavřete příkaz:</span><span class="sxs-lookup"><span data-stu-id="75dcc-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="75dcc-155">Existuje několik další aspekty při vyvolání tento příkaz z nástroje MSBuild.</span><span class="sxs-lookup"><span data-stu-id="75dcc-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="75dcc-156">Nejdřív by měla obsahovat **– NonInteractive** příznak, ujistěte se, zda se skript spustí tiše.</span><span class="sxs-lookup"><span data-stu-id="75dcc-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="75dcc-157">V dalším kroku by měla obsahovat **– ExecutionPolicy** příznak s hodnotou odpovídající argument.</span><span class="sxs-lookup"><span data-stu-id="75dcc-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="75dcc-158">Určuje zásady spouštění, použije se pro váš skript prostředí Windows PowerShell a umožňuje potlačit výchozí zásady spouštění, která může zabránit spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="75dcc-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="75dcc-159">Můžete vybrat z těchto hodnot argument:</span><span class="sxs-lookup"><span data-stu-id="75dcc-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="75dcc-160">Hodnota **bez omezení** vám umožní spuštění skriptu, bez ohledu na to, jestli je podepsaný skript prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75dcc-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="75dcc-161">Hodnota **RemoteSigned** bude možné spustit nepodepsané skripty, které byly vytvořeny v místním počítači prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75dcc-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="75dcc-162">Ale musí být podepsané skripty, které byly vytvořeny jinde.</span><span class="sxs-lookup"><span data-stu-id="75dcc-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="75dcc-163">(V praxi, jste velmi pravděpodobně jste vytvořili skript prostředí Windows PowerShell místně na serveru sestavení).</span><span class="sxs-lookup"><span data-stu-id="75dcc-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="75dcc-164">Hodnota **AllSigned** vám umožní provést pouze podepsaných skriptů prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75dcc-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="75dcc-165">Výchozí zásada spouštění je **s omezeným přístupem**, který zabrání spuštění žádné soubory skriptu prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75dcc-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="75dcc-166">Nakonec je třeba abyste se vyhnuli vyhrazené znaky jazyka XML, které se vyskytují v příkazu prostředí Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="75dcc-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="75dcc-167">Nahrazení jednoduchých uvozovek a být s  **&amp;apos;**</span><span class="sxs-lookup"><span data-stu-id="75dcc-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="75dcc-168">Nahraďte dvojité uvozovky se  **&amp;potřebná;**</span><span class="sxs-lookup"><span data-stu-id="75dcc-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="75dcc-169">Nahraďte ampersandy s  **&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="75dcc-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="75dcc-170">Pokud provedete tyto změny, bude váš příkaz vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="75dcc-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="75dcc-171">V rámci vaší vlastní soubor projektu nástroje MSBuild, můžete vytvořit nový cíl a použít **Exec** úlohy ke spuštění tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="75dcc-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="75dcc-172">V tomto příkladu Všimněte si, že:</span><span class="sxs-lookup"><span data-stu-id="75dcc-172">In this example, note that:</span></span>

- <span data-ttu-id="75dcc-173">Všechny proměnné, jako jsou hodnoty parametrů a umístění spustitelného souboru prostředí Windows PowerShell, jsou deklarovány jako vlastnosti nástroje MSBuild.</span><span class="sxs-lookup"><span data-stu-id="75dcc-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="75dcc-174">Podmínky jsou zahrnuty umožníte uživatelům přepsat tyto hodnoty z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="75dcc-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="75dcc-175">**MSDeployComputerName** vlastnost je deklarovaná jinde v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="75dcc-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="75dcc-176">Pokud tento cíl je spuštěn jako součást procesu sestavení, spustí příkazu prostředí Windows PowerShell a zapisovat položky protokolu do souboru, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="75dcc-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="75dcc-177">Provádění skriptu prostředí Windows PowerShell ve vzdáleném počítači</span><span class="sxs-lookup"><span data-stu-id="75dcc-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="75dcc-178">Prostředí Windows PowerShell je schopen spuštění skriptů na vzdálených počítačích prostřednictvím [Vzdálená správa systému Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="75dcc-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="75dcc-179">K tomuto účelu, budete muset použít [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="75dcc-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="75dcc-180">Díky tomu můžete spustit skript na jeden nebo více vzdálených počítačů bez kopírování skript do vzdálených počítačů.</span><span class="sxs-lookup"><span data-stu-id="75dcc-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="75dcc-181">Výsledky jsou vráceny do místního počítače, z níž jste spustili skript.</span><span class="sxs-lookup"><span data-stu-id="75dcc-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="75dcc-182">Před použitím **Invoke-Command** rutiny prostředí Windows PowerShell spuštění skriptů ve vzdáleném počítači, je nutné nakonfigurovat naslouchací proces služby WinRM tak, aby přijímal vzdálená zprávy.</span><span class="sxs-lookup"><span data-stu-id="75dcc-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="75dcc-183">To provedete spuštěním příkazu **rychlou konfiguraci winrm** na vzdáleném počítači.</span><span class="sxs-lookup"><span data-stu-id="75dcc-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="75dcc-184">Další informace najdete v tématu [instalace a konfigurace pro vzdálenou správu systému Windows](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="75dcc-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="75dcc-185">V okně prostředí Windows PowerShell by tuto syntaxi používají ke spuštění **LogDeploy.ps1** skriptu ve vzdáleném počítači:</span><span class="sxs-lookup"><span data-stu-id="75dcc-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="75dcc-186">Existují různé způsoby použití **Invoke-Command** spuštění skriptu souboru, ale tento přístup je nejvíc přímočarý když potřebujete zadat hodnoty parametrů a spravovat cesty s mezerami.</span><span class="sxs-lookup"><span data-stu-id="75dcc-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="75dcc-187">Když to spustíte z příkazového řádku, budete muset vyvolat spustitelný soubor prostředí Windows PowerShell a použít **– příkaz** parametru vytvořit pokyny:</span><span class="sxs-lookup"><span data-stu-id="75dcc-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="75dcc-188">Jako před, je potřeba zadat některé další přepínače a při spuštění příkazu z nástroje MSBuild vyhnuli všechny vyhrazené znaky jazyka XML:</span><span class="sxs-lookup"><span data-stu-id="75dcc-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="75dcc-189">Nakonec jako dříve, můžete použít **Exec** úloh v rámci vlastní cíle MSBuild k provedení příkazu:</span><span class="sxs-lookup"><span data-stu-id="75dcc-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="75dcc-190">Pokud tento cíl je spuštěn jako součást procesu sestavení, prostředí Windows PowerShell se spustí skript na počítači zadaném v **– computername** argument.</span><span class="sxs-lookup"><span data-stu-id="75dcc-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="75dcc-191">Závěr</span><span class="sxs-lookup"><span data-stu-id="75dcc-191">Conclusion</span></span>

<span data-ttu-id="75dcc-192">Toto téma popisuje, jak spustit skript prostředí Windows PowerShell ze souboru projektu nástroje MSBuild.</span><span class="sxs-lookup"><span data-stu-id="75dcc-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="75dcc-193">Tento postup můžete použít ke spuštění skriptu prostředí Windows PowerShell buď místně nebo ve vzdáleném počítači, v rámci procesu automatické nebo krokování sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="75dcc-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="75dcc-194">Další čtení</span><span class="sxs-lookup"><span data-stu-id="75dcc-194">Further Reading</span></span>

<span data-ttu-id="75dcc-195">Informace o podepisování skriptů prostředí Windows PowerShell a správě zásad spouštění, najdete v části [spuštění skriptů prostředí PowerShell systému Windows](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="75dcc-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="75dcc-196">Informace o spouštění příkazů prostředí Windows PowerShell ze vzdáleného počítače, najdete v části [spuštění vzdálené příkazy](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="75dcc-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="75dcc-197">Další informace o používání vlastních souborů projektu nástroje MSBuild k řízení procesu nasazení najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="75dcc-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="75dcc-198">[Předchozí](taking-web-applications-offline-with-web-deploy.md)
[další](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="75dcc-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
