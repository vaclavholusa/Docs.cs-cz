---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatizovat vše (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs
author: MikeWasson
description: Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: 2e30ab7831a10f215a08f74e61adf2d147e76543
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="fc626-104">Automatizovat vše (vytváření reálných cloudových aplikací s Azure)</span><span class="sxs-lookup"><span data-stu-id="fc626-104">Automate Everything (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="fc626-105">podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="fc626-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="fc626-106">[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="fc626-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="fc626-107">**Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie.</span><span class="sxs-lookup"><span data-stu-id="fc626-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="fc626-108">Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud.</span><span class="sxs-lookup"><span data-stu-id="fc626-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="fc626-109">Úvod do e adresáře, najdete v části [první kapitoly](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fc626-109">For an introduction to the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="fc626-110">Prvních tří vzorů, které podíváme ve skutečnosti použít k žádným projektem vývoj softwaru, ale hlavně pro projekty cloudových.</span><span class="sxs-lookup"><span data-stu-id="fc626-110">The first three patterns we'll look at actually apply to any software development project, but especially to cloud projects.</span></span> <span data-ttu-id="fc626-111">Tento vzor je o automatizaci úloh vývoj.</span><span class="sxs-lookup"><span data-stu-id="fc626-111">This pattern is about automating development tasks.</span></span> <span data-ttu-id="fc626-112">Je důležité tématu, protože manuální procesy pomalé a náchylný; automatizace, kolik z nich možné pomáhá nastavit rychlé, spolehlivé a agilní pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="fc626-112">It's an important topic because manual processes are slow and error-prone; automating as many of them as possible helps set up a fast, reliable, and agile workflow.</span></span> <span data-ttu-id="fc626-113">Je jednoznačně důležité pro vývoj cloudové protože snadno můžete automatizovat řadu úkolů, které je obtížné nebo dokonce znemožňují k automatizaci v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="fc626-113">It's uniquely important for cloud development because you can easily automate many tasks that are difficult or impossible to automate in an on-premises environment.</span></span> <span data-ttu-id="fc626-114">Například můžete nastavit celou testovací prostředí, včetně nového webového serveru a back-end virtuálních počítačů, databází, blob storage (úložiště souborů), fronty, atd.</span><span class="sxs-lookup"><span data-stu-id="fc626-114">For example, you can set up whole test environments including new web server and back-end VMs, databases, blob storage (file storage), queues, etc.</span></span>

## <a name="devops-workflow"></a><span data-ttu-id="fc626-115">Pracovní postup DevOps</span><span class="sxs-lookup"><span data-stu-id="fc626-115">DevOps Workflow</span></span>

<span data-ttu-id="fc626-116">Stále uslyšíte termín "DevOps."</span><span class="sxs-lookup"><span data-stu-id="fc626-116">Increasingly you hear the term "DevOps."</span></span> <span data-ttu-id="fc626-117">Termín vyvinuté mimo rozpoznávání, budete muset integrovat vývoj a provoz úloh a vývoji softwaru efektivně.</span><span class="sxs-lookup"><span data-stu-id="fc626-117">The term developed out of a recognition that you have to integrate development and operations tasks in order to develop software efficiently.</span></span> <span data-ttu-id="fc626-118">Druh pracovního postupu, který chcete povolit je taková, ve kterém můžete vyvíjet aplikace, nasazení, dozvědět se od produkční použití ho, změnit v reakci na co když jste se naučili a opakujte cyklus rychle a spolehlivě.</span><span class="sxs-lookup"><span data-stu-id="fc626-118">The kind of workflow you want to enable is one in which you can develop an app, deploy it, learn from production usage of it, change it in response to what you've learned, and repeat the cycle quickly and reliably.</span></span>

<span data-ttu-id="fc626-119">Některé vývojové týmy úspěšné cloudu nasadit několikrát za den za provozu prostředí.</span><span class="sxs-lookup"><span data-stu-id="fc626-119">Some successful cloud development teams deploy multiple times a day to a live environment.</span></span> <span data-ttu-id="fc626-120">Tým Azure použít k nasazení hlavní každé 2 – 3 měsíce, ale nyní jej aktualizovat verze dílčími aktualizacemi každé 2 až 3 dny a hlavní verze každé 2 až 3 týdny.</span><span class="sxs-lookup"><span data-stu-id="fc626-120">The Azure team used to deploy a major update every 2-3 months, but now it releases minor updates every 2-3 days and major releases every 2-3 weeks.</span></span> <span data-ttu-id="fc626-121">Získávání do této cadence skutečně vám pomůže se reaguje na názory zákazníků.</span><span class="sxs-lookup"><span data-stu-id="fc626-121">Getting into that cadence really helps you be responsive to customer feedback.</span></span>

<span data-ttu-id="fc626-122">Aby bylo možné provést, je třeba povolit cyklus vývoj a nasazení, který je opakovatelných, spolehlivé, předvídatelný a nízkou cyklus doby.</span><span class="sxs-lookup"><span data-stu-id="fc626-122">In order to do that, you have to enable a development and deployment cycle that is repeatable, reliable, predictable, and has low cycle time.</span></span>

![Pracovní postup DevOps](automate-everything/_static/image1.png)

<span data-ttu-id="fc626-124">Doba mezi Pokud máte představu pro funkci a když jsou zákazníci použití a poskytování zpětné vazby jinými slovy, musí být co nejkratší.</span><span class="sxs-lookup"><span data-stu-id="fc626-124">In other words, the period of time between when you have an idea for a feature and when the customers are using it and providing feedback must be as short as possible.</span></span> <span data-ttu-id="fc626-125">První tři vzory – automatizovat vše, Správa zdrojového kódu a průběžnou integraci a doručení – jsou všechny informace o doporučených postupech, které doporučujeme, aby bylo možné povolit tento druh procesu.</span><span class="sxs-lookup"><span data-stu-id="fc626-125">The first three patterns – automate everything, source control, and continuous integration and delivery -- are all about best practices that we recommend in order to enable that kind of process.</span></span>

## <a name="azure-management-scripts"></a><span data-ttu-id="fc626-126">Skripty pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="fc626-126">Azure management scripts</span></span>

<span data-ttu-id="fc626-127">V [Úvod do této e knihy](introduction.md), jste viděli webové konzoly, portálu pro správu Azure.</span><span class="sxs-lookup"><span data-stu-id="fc626-127">In the [introduction to this e-book](introduction.md), you saw the web-based console, the Azure Management Portal.</span></span> <span data-ttu-id="fc626-128">Portálu pro správu umožňuje sledovat a spravovat všechny prostředky, které jste nasadili v Azure.</span><span class="sxs-lookup"><span data-stu-id="fc626-128">The management portal enables you to monitor and manage all of the resources that you have deployed on Azure.</span></span> <span data-ttu-id="fc626-129">Je snadný způsob, jak vytvořit a odstranění služeb jako třeba webové aplikace a virtuálních počítačů, tyto služby konfigurovat, sledovat operace služby a tak dále.</span><span class="sxs-lookup"><span data-stu-id="fc626-129">It's an easy way to create and delete services such as web apps and VMs, configure those services, monitor service operation, and so forth.</span></span> <span data-ttu-id="fc626-130">Je skvělý nástroj, ale jeho použití je ruční proces.</span><span class="sxs-lookup"><span data-stu-id="fc626-130">It's a great tool, but using it is a manual process.</span></span> <span data-ttu-id="fc626-131">Pokud budete vyvíjet produkční aplikace libovolnou velikost, a hlavně v prostředí team, doporučujeme, aby přejděte prostřednictvím portálu uživatelského rozhraní, chcete-li další informace a seznamte se s Azure a poté automatizaci procesů, které jste budete by opakovaně.</span><span class="sxs-lookup"><span data-stu-id="fc626-131">If you're going to develop a production application of any size, and especially in a team environment, we recommend that you go through the portal UI in order to learn and explore Azure, and then automate the processes that you'll be doing repetitively.</span></span>

<span data-ttu-id="fc626-132">Téměř vše, co můžete udělat ručně v portálu pro správu nebo ze sady Visual Studio můžete také provést volání rozhraní API REST správy.</span><span class="sxs-lookup"><span data-stu-id="fc626-132">Nearly everything that you can do manually in the management portal or from Visual Studio can also be done by calling the REST management API.</span></span> <span data-ttu-id="fc626-133">Můžete napsat skripty s použitím [prostředí Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), nebo můžete použít otevřeným zdrojem framework [Chef](http://www.opscode.com/chef/) nebo [Puppet](http://puppetlabs.com/puppet/what-is-puppet).</span><span class="sxs-lookup"><span data-stu-id="fc626-133">You can write scripts using [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), or you can use an open source framework such as [Chef](http://www.opscode.com/chef/) or [Puppet](http://puppetlabs.com/puppet/what-is-puppet).</span></span> <span data-ttu-id="fc626-134">Můžete také použít nástroj příkazového řádku Bash v prostředí Mac nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="fc626-134">You can also use the Bash command-line tool in a Mac or Linux environment.</span></span> <span data-ttu-id="fc626-135">Azure má skriptování rozhraní API pro tyto různých prostředích, která je [rozhraní API pro správu .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) v případě, že chcete napsat kód místo skriptu.</span><span class="sxs-lookup"><span data-stu-id="fc626-135">Azure has scripting APIs for all those different environments, and it has a [.NET management API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) in case you want to write code instead of script.</span></span>

<span data-ttu-id="fc626-136">Aplikaci, opravte ji vytvořili jsme některé skripty prostředí Windows PowerShell, které automatizují procesy vytváření testovacího prostředí a nasazení projektu do prostředí a přečtěte část obsah těchto skriptů.</span><span class="sxs-lookup"><span data-stu-id="fc626-136">For the Fix It app we've created some Windows PowerShell scripts that automate the processes of creating a test environment and deploying the project to that environment, and we'll review some of the contents of those scripts.</span></span>

## <a name="environment-creation-script"></a><span data-ttu-id="fc626-137">Vytvoření skriptu prostředí</span><span class="sxs-lookup"><span data-stu-id="fc626-137">Environment creation script</span></span>

<span data-ttu-id="fc626-138">Je název první skriptu podíváme *New-AzureWebsiteEnv.ps1*.</span><span class="sxs-lookup"><span data-stu-id="fc626-138">The first script we'll look at is named *New-AzureWebsiteEnv.ps1*.</span></span> <span data-ttu-id="fc626-139">Vytvoří prostředí Azure, můžete nasadit opravte ji aplikaci pro testování.</span><span class="sxs-lookup"><span data-stu-id="fc626-139">It creates an Azure environment that you can deploy the Fix It app to for testing.</span></span> <span data-ttu-id="fc626-140">Hlavní úlohy, které tento skript provede jsou následující:</span><span class="sxs-lookup"><span data-stu-id="fc626-140">The main tasks that this script performs are the following:</span></span>

- <span data-ttu-id="fc626-141">Vytvoření webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fc626-141">Create a web app.</span></span>
- <span data-ttu-id="fc626-142">Vytvoření účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="fc626-142">Create a storage account.</span></span> <span data-ttu-id="fc626-143">(Požadováno pro objekty BLOB a fronty, jak uvidíte v dalších kapitolách.)</span><span class="sxs-lookup"><span data-stu-id="fc626-143">(Required for blobs and queues, as you'll see in later chapters.)</span></span>
- <span data-ttu-id="fc626-144">Vytvoření databáze SQL serveru a dvě databáze: aplikační databáze a databáze členství.</span><span class="sxs-lookup"><span data-stu-id="fc626-144">Create a SQL Database server and two databases: an application database, and a membership database.</span></span>
- <span data-ttu-id="fc626-145">Ukládání nastavení v Azure, která aplikace bude používat pro přístup k účtu úložiště a databáze.</span><span class="sxs-lookup"><span data-stu-id="fc626-145">Store settings in Azure that the app will use to access the storage account and databases.</span></span>
- <span data-ttu-id="fc626-146">Vytvořte soubory nastavení, které se použijí k automatizaci nasazení.</span><span class="sxs-lookup"><span data-stu-id="fc626-146">Create settings files that will be used to automate deployment.</span></span>

### <a name="run-the-script"></a><span data-ttu-id="fc626-147">Spusťte skript</span><span class="sxs-lookup"><span data-stu-id="fc626-147">Run the script</span></span>


> [!NOTE]
> <span data-ttu-id="fc626-148">Tato část kapitoly jsou uvedeny příklady skripty a příkazy, které zadáte, aby bylo možné spustit.</span><span class="sxs-lookup"><span data-stu-id="fc626-148">This part of the chapter shows examples of scripts and the commands that you enter in order to run them.</span></span> <span data-ttu-id="fc626-149">Tuto ukázku a neposkytuje všechno, co potřebujete vědět, aby bylo možné spustit skripty.</span><span class="sxs-lookup"><span data-stu-id="fc626-149">This a demo and doesn't provide everything you need to know in order to run the scripts.</span></span> <span data-ttu-id="fc626-150">Podrobné pokyny how-k-proveďte it, najdete v [příloha: Opravte ji ukázka Application](the-fix-it-sample-application.md#deploybase).</span><span class="sxs-lookup"><span data-stu-id="fc626-150">For step-by-step how-to-do-it instructions, see [Appendix: The Fix It Sample Application](the-fix-it-sample-application.md#deploybase).</span></span>


<span data-ttu-id="fc626-151">Chcete-li spustit skript prostředí PowerShell, který spravuje služby Azure, budete muset nainstalovat konzoly Azure PowerShell a nakonfigurovat ho na spolupráci s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="fc626-151">To run a PowerShell script that manages Azure services you have to install the Azure PowerShell console and configure it to work with your Azure subscription.</span></span> <span data-ttu-id="fc626-152">Po nastavení, můžete spustit opravte ji prostředí vytvoření skriptu pomocí příkazu podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="fc626-152">Once you're set up, you can run the Fix It environment creation script with a command like this one:</span></span>

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

<span data-ttu-id="fc626-153">`Name` Parametr určuje název, který se má použít při vytváření databáze a úložiště účtů a `SqlDatabasePassword` parametr určuje heslo pro účet správce, který se vytvoří pro databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="fc626-153">The `Name` parameter specifies the name to be used when creating the database and storage accounts, and the `SqlDatabasePassword` parameter specifies the password for the admin account that will be created for SQL Database.</span></span> <span data-ttu-id="fc626-154">Existují další parametry, které můžete použít, podíváme později.</span><span class="sxs-lookup"><span data-stu-id="fc626-154">There are other parameters you can use that we'll look at later.</span></span>

![Okno prostředí PowerShell](automate-everything/_static/image2.png)

<span data-ttu-id="fc626-156">Po dokončení skriptu uvidíte na portálu management portal, co byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="fc626-156">After the script finishes you can see in the management portal what was created.</span></span> <span data-ttu-id="fc626-157">Zjistíte dvě databáze:</span><span class="sxs-lookup"><span data-stu-id="fc626-157">You'll find two databases:</span></span>

![Databáze](automate-everything/_static/image3.png)

<span data-ttu-id="fc626-159">Účet úložiště:</span><span class="sxs-lookup"><span data-stu-id="fc626-159">A storage account:</span></span>

![Účet úložiště](automate-everything/_static/image4.png)

<span data-ttu-id="fc626-161">A webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="fc626-161">And a web app:</span></span>

![Webový server](automate-everything/_static/image5.png)

<span data-ttu-id="fc626-163">Na **konfigurace** kartě pro webovou aplikaci, uvidíte, že má nastavení účtu úložiště a připojovací řetězce SQL databáze nastavení pro opravu jeho aplikace.</span><span class="sxs-lookup"><span data-stu-id="fc626-163">On the **Configure** tab for the web app, you can see that it has the storage account settings and SQL database connection strings set up for the Fix It app.</span></span>

![appSettings a connectionStrings](automate-everything/_static/image6.png)

<span data-ttu-id="fc626-165">*Automatizace* složka teď také obsahuje  *&lt;zadaným hodnotám websitename&gt;.pubxml* souboru.</span><span class="sxs-lookup"><span data-stu-id="fc626-165">The *Automation* folder now also contains a *&lt;websitename&gt;.pubxml* file.</span></span> <span data-ttu-id="fc626-166">Tento soubor uchovává nastavení, která MSBuild použijete k nasazení aplikace do prostředí Azure, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="fc626-166">This file stores settings that MSBuild will use to deploy the application to the Azure environment that was just created.</span></span> <span data-ttu-id="fc626-167">Příklad:</span><span class="sxs-lookup"><span data-stu-id="fc626-167">For example:</span></span>

[!code-xml[Main](automate-everything/samples/sample1.xml)]

<span data-ttu-id="fc626-168">Jak vidíte, skript vytvořil dokončení testovacího prostředí a celý proces se provádí v přibližně 90 sekund.</span><span class="sxs-lookup"><span data-stu-id="fc626-168">As you can see, the script has created a complete test environment, and the whole process is done in about 90 seconds.</span></span>

<span data-ttu-id="fc626-169">Pokud někdo na váš tým chce vytvořit testovací prostředí, mohou pouze spouštět skript.</span><span class="sxs-lookup"><span data-stu-id="fc626-169">If someone else on your team wants to create a test environment, they can just run the script.</span></span> <span data-ttu-id="fc626-170">Jenom je rychlý, ale také může být jistý, že používají stejný jako ten, který používáte prostředí.</span><span class="sxs-lookup"><span data-stu-id="fc626-170">Not only is it fast, but also they can be confident that they are using an environment identical to the one you're using.</span></span> <span data-ttu-id="fc626-171">Nepodařilo se poměrně jako jisti této, pokud všichni byla nastavení věcí ručně pomocí portálu pro správu uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fc626-171">You couldn't be quite as confident of that if everyone was setting things up manually by using the management portal UI.</span></span>

### <a name="a-look-at-the-scripts"></a><span data-ttu-id="fc626-172">Podívejte se na tyto skripty</span><span class="sxs-lookup"><span data-stu-id="fc626-172">A look at the scripts</span></span>

<span data-ttu-id="fc626-173">Existují ve skutečnosti tři skripty, které tuto práci.</span><span class="sxs-lookup"><span data-stu-id="fc626-173">There are actually three scripts that do this work.</span></span> <span data-ttu-id="fc626-174">Můžete volat z příkazového řádku a automaticky použije další dvě udělat některé úlohy:</span><span class="sxs-lookup"><span data-stu-id="fc626-174">You call one from the command line and it automatically uses the other two to do some of the tasks:</span></span>

- <span data-ttu-id="fc626-175">*Nové AzureWebSiteEnv.ps1* je hlavního skriptu.</span><span class="sxs-lookup"><span data-stu-id="fc626-175">*New-AzureWebSiteEnv.ps1* is the main script.</span></span>

    - <span data-ttu-id="fc626-176">*Nové AzureStorage.ps1* vytvoří účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="fc626-176">*New-AzureStorage.ps1* creates the storage account.</span></span>
    - <span data-ttu-id="fc626-177">*Nové AzureSql.ps1* vytvoří databáze.</span><span class="sxs-lookup"><span data-stu-id="fc626-177">*New-AzureSql.ps1* creates the databases.</span></span>

### <a name="parameters-in-the-main-script"></a><span data-ttu-id="fc626-178">Parametry v hlavního skriptu</span><span class="sxs-lookup"><span data-stu-id="fc626-178">Parameters in the main script</span></span>

<span data-ttu-id="fc626-179">Hlavní skriptu *New-AzureWebSiteEnv.ps1*, definuje několik parametrů:</span><span class="sxs-lookup"><span data-stu-id="fc626-179">The main script, *New-AzureWebSiteEnv.ps1*, defines several parameters:</span></span>

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

<span data-ttu-id="fc626-180">Jsou vyžadovány dva parametry:</span><span class="sxs-lookup"><span data-stu-id="fc626-180">Two parameters are required:</span></span>

- <span data-ttu-id="fc626-181">Název webové aplikace, kterou vytvoří skript.</span><span class="sxs-lookup"><span data-stu-id="fc626-181">The name of the web app that the script creates.</span></span> <span data-ttu-id="fc626-182">(Používá se také pro adresu URL: `<name>.azurewebsites.net`.)</span><span class="sxs-lookup"><span data-stu-id="fc626-182">(This is also used for the URL: `<name>.azurewebsites.net`.)</span></span>
- <span data-ttu-id="fc626-183">Heslo pro nového správce databáze serveru, který vytvoří skript.</span><span class="sxs-lookup"><span data-stu-id="fc626-183">The password for the new administrative user of the database server that the script creates.</span></span>

<span data-ttu-id="fc626-184">Volitelné parametry umožňují určit umístění center dat (výchozí hodnota je "Západní USA"), správce název databázového serveru (výchozí hodnota je "dbuser") a pravidla brány firewall pro server databáze.</span><span class="sxs-lookup"><span data-stu-id="fc626-184">Optional parameters enable you to specify the data center location (defaults to "West US"), database server administrator name (defaults to "dbuser"), and a firewall rule for the database server.</span></span>

### <a name="create-the-web-app"></a><span data-ttu-id="fc626-185">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="fc626-185">Create the web app</span></span>

<span data-ttu-id="fc626-186">První věc skript nemá, je vytvoření webové aplikace pomocí volání `New-AzureWebsite` rutiny předávání v k němu webovou aplikaci název a umístění hodnoty parametrů:</span><span class="sxs-lookup"><span data-stu-id="fc626-186">The first thing the script does is create the web app by calling the `New-AzureWebsite` cmdlet, passing in to it the web app name and location parameter values:</span></span>

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a><span data-ttu-id="fc626-187">Vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="fc626-187">Create the storage account</span></span>

<span data-ttu-id="fc626-188">Potom hlavní skript se spustí <em>New-AzureStorage.ps1</em> skriptu, zadání "<em>&lt;zadaným hodnotám websitename&gt;</em>úložiště" pro název účtu úložiště a stejná data center umístění jako webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fc626-188">Then the main script runs the <em>New-AzureStorage.ps1</em> script, specifying "<em>&lt;websitename&gt;</em>storage" for the storage account name, and the same data center location as the web app.</span></span>

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

<span data-ttu-id="fc626-189">*Nové AzureStorage.ps1* volání `New-AzureStorageAccount` vytvořte účet úložiště a vrátí účet názvem a přístupovým klíčové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="fc626-189">*New-AzureStorage.ps1* calls the `New-AzureStorageAccount` cmdlet to create the storage account, and it returns the account name and access key values.</span></span> <span data-ttu-id="fc626-190">Aplikace bude nutné tyto hodnoty pro přístup k objektům BLOB a fronty v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="fc626-190">The application will need these values in order to access the blobs and queues in the storage account.</span></span>

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

<span data-ttu-id="fc626-191">Nemusí se vždy chcete vytvořit nový účet úložiště; skript může zvýšit tak, že přidáte parametr, který volitelně přesměruje ho na použití existující účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="fc626-191">You might not always want to create a new storage account; you could enhance the script by adding a parameter that optionally directs it to use an existing storage account.</span></span>

### <a name="create-the-databases"></a><span data-ttu-id="fc626-192">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="fc626-192">Create the databases</span></span>

<span data-ttu-id="fc626-193">Hlavního skriptu pak spustí skript vytvoření databáze, *New-AzureSql.ps1*, po nastavení výchozí databáze a názvy pravidel brány firewall:</span><span class="sxs-lookup"><span data-stu-id="fc626-193">The main script then runs the database creation script, *New-AzureSql.ps1*, after setting up default database and firewall rule names:</span></span>

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

<span data-ttu-id="fc626-194">Vytvoření databázového skriptu načte vývojářském počítači IP adresu a nastaví pravidlo brány firewall tak, aby vývojářském počítači můžete připojit k a správě serveru.</span><span class="sxs-lookup"><span data-stu-id="fc626-194">The database creation script retrieves the dev machine's IP address and sets a firewall rule so the dev machine can connect to and manage the server.</span></span> <span data-ttu-id="fc626-195">Vytvoření databázového skriptu pak prochází nastavte databáze několik kroků:</span><span class="sxs-lookup"><span data-stu-id="fc626-195">The database creation script then goes through several steps to set up the databases:</span></span>

- <span data-ttu-id="fc626-196">Vytvoří server pomocí `New-AzureSqlDatabaseServer` rutiny.</span><span class="sxs-lookup"><span data-stu-id="fc626-196">Creates the server by using the `New-AzureSqlDatabaseServer` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- <span data-ttu-id="fc626-197">Vytvoří pravidla brány firewall pro povolení vývojářském počítači ke správě serveru a umožňuje připojení k tomuto webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fc626-197">Creates firewall rules to enable the dev machine to manage the server and to enable the web app to connect to it.</span></span> 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- <span data-ttu-id="fc626-198">Vytvoří kontext databáze, která obsahuje název serveru a přihlašovací údaje, pomocí `New-AzureSqlDatabaseServerContext` rutiny.</span><span class="sxs-lookup"><span data-stu-id="fc626-198">Creates a database context that includes the server name and credentials, by using the `New-AzureSqlDatabaseServerContext` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    <span data-ttu-id="fc626-199">`New-PSCredentialFromPlainText` je funkce ve skriptu, který volá `ConvertTo-SecureString` rutiny k šifrování hesla a vrátí `PSCredential` objektu, stejný typ, který `Get-Credential` rutina vrátí.</span><span class="sxs-lookup"><span data-stu-id="fc626-199">`New-PSCredentialFromPlainText` is a function in the script that calls the `ConvertTo-SecureString` cmdlet to encrypt the password and returns a `PSCredential` object, the same type that the `Get-Credential` cmdlet returns.</span></span>
- <span data-ttu-id="fc626-200">Vytvoří aplikační databázi a databázi členství pomocí `New-AzureSqlDatabase` rutiny.</span><span class="sxs-lookup"><span data-stu-id="fc626-200">Creates the application database and the membership database by using the `New-AzureSqlDatabase` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- <span data-ttu-id="fc626-201">Volá tocreates místně definované funkce připojovací řetězec pro každou databázi.</span><span class="sxs-lookup"><span data-stu-id="fc626-201">Calls a locally defined function tocreates a connection string for each database.</span></span> <span data-ttu-id="fc626-202">Aplikace bude používat tyto připojovací řetězce pro přístup k databázím.</span><span class="sxs-lookup"><span data-stu-id="fc626-202">The application will use these connection strings to access the databases.</span></span> 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    <span data-ttu-id="fc626-203">Get-SQLAzureDatabaseConnectionString je funkci definovanou v skript, který vytvoří připojovací řetězec z hodnoty parametrů použité k němu.</span><span class="sxs-lookup"><span data-stu-id="fc626-203">Get-SQLAzureDatabaseConnectionString is a function defined in the script that creates the connection string from the parameter values supplied to it.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- <span data-ttu-id="fc626-204">Vrátí tabulku hash se název databázového serveru a připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="fc626-204">Returns a hash table with the database server name and the connection strings.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

<span data-ttu-id="fc626-205">Aplikace opravte ji používá samostatné členství a databáze aplikace.</span><span class="sxs-lookup"><span data-stu-id="fc626-205">The Fix It app uses separate membership and application databases.</span></span> <span data-ttu-id="fc626-206">Je také možné uvést dat členství a aplikací do jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="fc626-206">It's also possible to put both membership and application data in a single database.</span></span>

### <a name="store-app-settings-and-connection-strings"></a><span data-ttu-id="fc626-207">Ukládání nastavení aplikace a připojovacích řetězců</span><span class="sxs-lookup"><span data-stu-id="fc626-207">Store app settings and connection strings</span></span>

<span data-ttu-id="fc626-208">Azure nabízí funkce, která vám umožní uložit nastavení a připojovací řetězce, které automaticky přepsání, které jsou vraceny aplikace při pokusu o čtení `appSettings` nebo `connectionStrings` kolekcí v souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="fc626-208">Azure has a feature that enables you to store settings and connection strings that automatically override what is returned to the application when it tries to read the `appSettings` or `connectionStrings` collections in the Web.config file.</span></span> <span data-ttu-id="fc626-209">Jde o alternativu k použití [transformace Web.config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) při nasazení.</span><span class="sxs-lookup"><span data-stu-id="fc626-209">This is an alternative to applying [Web.config transformations](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) when you deploy.</span></span> <span data-ttu-id="fc626-210">Další informace najdete v tématu [ukládat citlivá data v Azure](source-control.md#appsettings) dál v této příručce e.</span><span class="sxs-lookup"><span data-stu-id="fc626-210">For more information, see [Store sensitive data in Azure](source-control.md#appsettings) later in this e-book.</span></span>

<span data-ttu-id="fc626-211">Vytvoření skriptu prostředí ukládá v Azure všechny `appSettings` a `connectionStrings` hodnoty, které aplikace potřebuje přístup k účtu úložiště a databáze při spuštění v Azure.</span><span class="sxs-lookup"><span data-stu-id="fc626-211">The environment creation script stores in Azure all of the `appSettings` and `connectionStrings` values that the application needs to access the storage account and databases when it runs in Azure.</span></span>

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

<span data-ttu-id="fc626-212">[New Relic](http://newrelic.com/) telemetrie rozhraní, které jsme předvedení v [monitorování a Telemetrie](monitoring-and-telemetry.md) kapitoly.</span><span class="sxs-lookup"><span data-stu-id="fc626-212">[New Relic](http://newrelic.com/) is a telemetry framework that we demonstrate in the [Monitoring and Telemetry](monitoring-and-telemetry.md) chapter.</span></span> <span data-ttu-id="fc626-213">Skript pro vytvoření prostředí se také restartuje, webové aplikace, abyste měli jistotu, že vybere nastavení New Relic.</span><span class="sxs-lookup"><span data-stu-id="fc626-213">The environment creation script also restarts the web app to make sure that it picks up the New Relic settings.</span></span>

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a><span data-ttu-id="fc626-214">Příprava pro nasazení</span><span class="sxs-lookup"><span data-stu-id="fc626-214">Preparing for deployment</span></span>

<span data-ttu-id="fc626-215">Na konci procesu vytvoření skriptu prostředí volá dvě funkce postup vytvoření souborů, které se použijí skript nasazení.</span><span class="sxs-lookup"><span data-stu-id="fc626-215">At the end of the process, the environment creation script calls two functions to create files that will be used by the deployment script.</span></span>

<span data-ttu-id="fc626-216">Jednu z těchto funkcí vytvoří profil publikování *(&lt;zadaným hodnotám websitename&gt;.pubxml* souboru).</span><span class="sxs-lookup"><span data-stu-id="fc626-216">One of these functions creates a publish profile *(&lt;websitename&gt;.pubxml* file).</span></span> <span data-ttu-id="fc626-217">Kód zavolá rozhraní API REST Azure pro získání nastavení publikování a ukládá informace v *.publishsettings* souboru.</span><span class="sxs-lookup"><span data-stu-id="fc626-217">The code calls the Azure REST API to get the publish settings, and it saves the information in a *.publishsettings* file.</span></span> <span data-ttu-id="fc626-218">Pak používá informace z tohoto souboru spolu s souboru šablony (*pubxml.template*) k vytvoření *.pubxml* soubor, který obsahuje profil publikování.</span><span class="sxs-lookup"><span data-stu-id="fc626-218">Then it uses the information from that file along with a template file (*pubxml.template*) to create the *.pubxml* file that contains the publish profile.</span></span> <span data-ttu-id="fc626-219">Tento dvoukrokový proces simuluje, co se děje v sadě Visual Studio: stažení *.publishsettings* souboru a import, který chcete vytvořit profil publikování.</span><span class="sxs-lookup"><span data-stu-id="fc626-219">This two-step process simulates what you do in Visual Studio: download a *.publishsettings* file and import that to create a publish profile.</span></span>

<span data-ttu-id="fc626-220">Další funkce používá jiný soubor šablony (Web environment.template) k vytvoření *webu environment.xml* soubor, který obsahuje nastavení skript nasazení použije spolu s *.pubxml*souboru.</span><span class="sxs-lookup"><span data-stu-id="fc626-220">The other function uses another template file (website-environment.template) to create a *website-environment.xml* file that contains settings the deployment script will use along with the *.pubxml* file.</span></span>

### <a name="troubleshooting-and-error-handling"></a><span data-ttu-id="fc626-221">Řešení potíží a zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="fc626-221">Troubleshooting and error handling</span></span>

<span data-ttu-id="fc626-222">Skripty se podobně jako programy: můžou selhat a pokud se tak chcete vědět, co nejvíce můžete o selhání, a zda ji způsobil.</span><span class="sxs-lookup"><span data-stu-id="fc626-222">Scripts are like programs: they can fail, and when they do you want to know as much as you can about the failure and what caused it.</span></span> <span data-ttu-id="fc626-223">Z toho důvodu vytvoření skriptu prostředí změní hodnotu `VerbosePreference` proměnnou z `SilentlyContinue` k `Continue` tak, aby všechny podrobné zprávy se zobrazují.</span><span class="sxs-lookup"><span data-stu-id="fc626-223">For this reason, the environment creation script changes the value of the `VerbosePreference` variable from `SilentlyContinue` to `Continue` so that all verbose messages are displayed.</span></span> <span data-ttu-id="fc626-224">Také změní hodnotu `ErrorActionPreference` proměnnou z `Continue` k `Stop`tak, aby skript zastaví, i když zjistí neukončující chyby:</span><span class="sxs-lookup"><span data-stu-id="fc626-224">It also changes the value of the `ErrorActionPreference` variable from `Continue` to `Stop`, so that the script stops even when it encounters non-terminating errors:</span></span>

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

<span data-ttu-id="fc626-225">Před jeho veškerou práci, ukládá skript tak, aby ho můžete vypočítat uplynulý čas, pokud se provádí čas spuštění:</span><span class="sxs-lookup"><span data-stu-id="fc626-225">Before it does any work, the script stores the start time so that it can calculate the elapsed time when it's done:</span></span>

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

<span data-ttu-id="fc626-226">Až se dokončí svojí práci, skript zobrazí uplynulý čas:</span><span class="sxs-lookup"><span data-stu-id="fc626-226">After it completes its work, the script displays the elapsed time:</span></span>

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

<span data-ttu-id="fc626-227">A pro každé klíčové operace skript zapisuje podrobné zprávy, například:</span><span class="sxs-lookup"><span data-stu-id="fc626-227">And for every key operation the script writes verbose messages, for example:</span></span>

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a><span data-ttu-id="fc626-228">Skript nasazení</span><span class="sxs-lookup"><span data-stu-id="fc626-228">Deployment script</span></span>

<span data-ttu-id="fc626-229">Co *New-AzureWebsiteEnv.ps1* nemá skript pro vytvoření prostředí, *publikovat AzureWebsite.ps1* nemá skriptu pro nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="fc626-229">What the *New-AzureWebsiteEnv.ps1* script does for environment creation, the *Publish-AzureWebsite.ps1* script does for application deployment.</span></span>

<span data-ttu-id="fc626-230">Skript nasazení získá název webové aplikace z *webu environment.xml* soubor vytvořený pomocí skriptu pro vytváření prostředí.</span><span class="sxs-lookup"><span data-stu-id="fc626-230">The deployment script gets the name of the web app from the *website-environment.xml* file created by the environment creation script.</span></span>

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

<span data-ttu-id="fc626-231">Získá heslo uživatele nasazení z *.publishsettings* souboru:</span><span class="sxs-lookup"><span data-stu-id="fc626-231">It gets the deployment user password from the *.publishsettings* file:</span></span>

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

<span data-ttu-id="fc626-232">Se provede [MSBuild](http://msbuildbook.com/) příkaz, který vytvoří a nasadí projekt:</span><span class="sxs-lookup"><span data-stu-id="fc626-232">It executes the [MSBuild](http://msbuildbook.com/) command that builds and deploys the project:</span></span>

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

<span data-ttu-id="fc626-233">A pokud jste určili `Launch` parametr na příkazovém řádku, zavolá `Show-AzureWebsite` rutiny otevře výchozí prohlížeč na adresu URL webu.</span><span class="sxs-lookup"><span data-stu-id="fc626-233">And if you've specified the `Launch` parameter on the command line, it calls the `Show-AzureWebsite` cmdlet to open your default browser to the website URL.</span></span>

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

<span data-ttu-id="fc626-234">Skript nasazení můžete spustit pomocí příkazu podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="fc626-234">You can run the deployment script with a command like this one:</span></span>

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

<span data-ttu-id="fc626-235">A až skončíte, prohlížeči se otevře s web spuštěný v cloudu v `<websitename>.azurewebsites.net` adresy URL.</span><span class="sxs-lookup"><span data-stu-id="fc626-235">And when it's done, the browser opens with the site running in the cloud at the `<websitename>.azurewebsites.net` URL.</span></span>

![Automatická oprava aplikace nasazené do systému Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a><span data-ttu-id="fc626-237">Souhrn</span><span class="sxs-lookup"><span data-stu-id="fc626-237">Summary</span></span>

<span data-ttu-id="fc626-238">Pomocí těchto skriptů máte jistotu, že se shoduje s kroky bude vždy provést ve stejném pořadí pomocí stejných možností.</span><span class="sxs-lookup"><span data-stu-id="fc626-238">With these scripts you can be confident that the same steps will always be executed in the same order using the same options.</span></span> <span data-ttu-id="fc626-239">To pomáhá zajistit, že každý vývojář v týmu není neproběhly něco nebo zřeteli něco a nasazení něco vlastní na vlastním počítači, který nebude fungovat ve skutečnosti stejným způsobem jako v prostředí druhý člen seskupení, nebo v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="fc626-239">This helps ensure that each developer on the team doesn't miss something or mess something up or deploy something custom on his own machine that won't actually work the same way in another team member's environment or in production.</span></span>

<span data-ttu-id="fc626-240">Podobným způsobem můžete automatizovat nejvíce Azure funkce správy, které můžete provést v portálu pro správu pomocí rozhraní REST API, skriptů prostředí Windows PowerShell, jazyk rozhraní .NET API nebo Bash nástroj, který můžete spustit na platformě Linux nebo Mac.</span><span class="sxs-lookup"><span data-stu-id="fc626-240">In a similar way, you can automate most Azure management functions that you can do in the management portal, by using the REST API, Windows PowerShell scripts, a .NET language API, or a Bash utility that you can run on Linux or Mac.</span></span>

<span data-ttu-id="fc626-241">V [další kapitoly](source-control.md) jsme budete podívejte se na zdrojový kód a vysvětlit, proč je důležité zahrnout skripty do vašeho úložiště zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="fc626-241">In the [next chapter](source-control.md) we'll look at source code and explain why it's important to include your scripts in your source code repository.</span></span>

## <a name="resources"></a><span data-ttu-id="fc626-242">Prostředky</span><span class="sxs-lookup"><span data-stu-id="fc626-242">Resources</span></span>

- <span data-ttu-id="fc626-243">[Instalace a konfigurace prostředí Windows PowerShell pro Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).</span><span class="sxs-lookup"><span data-stu-id="fc626-243">[Install and Configure Windows PowerShell for Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).</span></span> <span data-ttu-id="fc626-244">Popisuje postup instalace rutin prostředí Azure PowerShell a jak nainstalovat certifikát, zda je nutné v počítači, abyste mohli spravovat vaše Azure účtu.</span><span class="sxs-lookup"><span data-stu-id="fc626-244">Explains how to install the Azure PowerShell cmdlets and how to install the certificate that you need on your computer in order to manage your Azure account.</span></span> <span data-ttu-id="fc626-245">Toto je skvělým místem začít pracovat, protože má také odkazy na zdroje informací prostředí PowerShell, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="fc626-245">This is a great place to get started because it also has links to resources for learning PowerShell itself.</span></span>
- <span data-ttu-id="fc626-246">[Centra skriptů Azure](https://docs.microsoft.com/azure/automation/automation-runbook-gallery).</span><span class="sxs-lookup"><span data-stu-id="fc626-246">[Azure Script Center](https://docs.microsoft.com/azure/automation/automation-runbook-gallery).</span></span> <span data-ttu-id="fc626-247">Portál WindowsAzure.com prostředky pro vývoj skriptů, které spravují služby Azure, s odkazy na získávání kurzy Začínáme, rutina referenční dokumentaci a zdrojový kód a ukázkové skripty</span><span class="sxs-lookup"><span data-stu-id="fc626-247">WindowsAzure.com portal to resources for developing scripts that manage Azure services, with links to getting started tutorials, cmdlet reference documentation and source code, and sample scripts</span></span>
- <span data-ttu-id="fc626-248">[Tvůrce skriptů víkendu: Začínáme s Azure a prostředí PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx).</span><span class="sxs-lookup"><span data-stu-id="fc626-248">[Weekend Scripter: Getting Started with Azure and PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx).</span></span> <span data-ttu-id="fc626-249">V blogu vyhrazený pro prostředí Windows PowerShell poskytuje tento příspěvek Skvělý úvod pomocí prostředí PowerShell pro Azure správy funkcí.</span><span class="sxs-lookup"><span data-stu-id="fc626-249">In a blog dedicated to Windows PowerShell, this post provides a great introduction to using PowerShell for Azure management functions.</span></span>
- <span data-ttu-id="fc626-250">[Instalace a konfigurace rozhraní příkazového řádku Azure napříč platformami](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="fc626-250">[Install and Configure the Azure Cross-Platform Command-Line Interface](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).</span></span> <span data-ttu-id="fc626-251">Úvodní kurz pro Azure skriptování framework, který funguje na Mac a Linux, jakož i Windows systémy.</span><span class="sxs-lookup"><span data-stu-id="fc626-251">Getting-started tutorial for an Azure scripting framework that works on Mac and Linux as well as Windows systems.</span></span>
- <span data-ttu-id="fc626-252">[Nástroje příkazového řádku části tématu stáhnout sady Azure SDK a nástroje](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="fc626-252">[Command-line tools section of the Download Azure SDKs and Tools topic](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="fc626-253">Stránka portálu dokumentace a produkty ke stažení související s nástroje příkazového řádku pro Azure.</span><span class="sxs-lookup"><span data-stu-id="fc626-253">Portal page for documentation and downloads related to command-line tools for Azure.</span></span>
- <span data-ttu-id="fc626-254">[Automatizace všechno, co se knihovny správy Azure a rozhraní .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx).</span><span class="sxs-lookup"><span data-stu-id="fc626-254">[Automating everything with the Azure Management Libraries and .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx).</span></span> <span data-ttu-id="fc626-255">Scott Hanselman zavádí .NET správy rozhraní API pro Azure.</span><span class="sxs-lookup"><span data-stu-id="fc626-255">Scott Hanselman introduces the .NET management API for Azure.</span></span>
- <span data-ttu-id="fc626-256">[Pomocí skriptů prostředí PowerShell systému Windows k publikování pro vývojáře a testovací prostředí](https://msdn.microsoft.com/library/azure/dn642480.aspx).</span><span class="sxs-lookup"><span data-stu-id="fc626-256">[Using Windows PowerShell Scripts to Publish to Dev and Test Environments](https://msdn.microsoft.com/library/azure/dn642480.aspx).</span></span> <span data-ttu-id="fc626-257">Dokumentace MSDN, která vysvětluje, jak používat publikovat skripty, které automaticky generuje sada Visual Studio pro webové projekty.</span><span class="sxs-lookup"><span data-stu-id="fc626-257">MSDN documentation that explains how to use publish scripts that Visual Studio automatically generates for web projects.</span></span>
- <span data-ttu-id="fc626-258">[Prostředí PowerShell nástroje pro sadu Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597).</span><span class="sxs-lookup"><span data-stu-id="fc626-258">[PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597).</span></span> <span data-ttu-id="fc626-259">Visual Studio rozšíření, která přidá podporu pro prostředí Windows PowerShell v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc626-259">Visual Studio extension that adds language support for Windows PowerShell in Visual Studio.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fc626-260">[Předchozí](introduction.md)
> [další](source-control.md)</span><span class="sxs-lookup"><span data-stu-id="fc626-260">[Previous](introduction.md)
[Next](source-control.md)</span></span>
