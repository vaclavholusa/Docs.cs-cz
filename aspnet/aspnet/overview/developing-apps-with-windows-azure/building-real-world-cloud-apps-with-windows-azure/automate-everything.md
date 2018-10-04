---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatizace všechno, co (sestavování skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: d0ce344bcb036819feba6218edc8dd90af501f50
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577574"
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="7b107-104">Automatizace všechno, co (sestavování skutečných cloudových aplikací s Azure)</span><span class="sxs-lookup"><span data-stu-id="7b107-104">Automate Everything (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="7b107-105">podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7b107-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="7b107-106">[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="7b107-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="7b107-107">**Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie.</span><span class="sxs-lookup"><span data-stu-id="7b107-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="7b107-108">Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu.</span><span class="sxs-lookup"><span data-stu-id="7b107-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="7b107-109">Úvod do e kniha najdete v tématu [první kapitoly](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7b107-109">For an introduction to the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="7b107-110">První tři vzory, které se podíváme na ve skutečnosti platí do jakéhokoli projektu vývoje softwaru, ale především pro projekty v cloudu.</span><span class="sxs-lookup"><span data-stu-id="7b107-110">The first three patterns we'll look at actually apply to any software development project, but especially to cloud projects.</span></span> <span data-ttu-id="7b107-111">Tento model je o automatizaci úkolů vývoje.</span><span class="sxs-lookup"><span data-stu-id="7b107-111">This pattern is about automating development tasks.</span></span> <span data-ttu-id="7b107-112">Je důležité tématu, protože ruční procesy jsou pomalé a náchylné; automatizace, kolik z nich, je to možné pomáhá nastavit rychlé, spolehlivé a flexibilní pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="7b107-112">It's an important topic because manual processes are slow and error-prone; automating as many of them as possible helps set up a fast, reliable, and agile workflow.</span></span> <span data-ttu-id="7b107-113">Je jednoznačně důležité pro vývoj pro cloud, protože můžete snadno automatizovat mnoho úloh, které je obtížné či nemožné automatizace v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7b107-113">It's uniquely important for cloud development because you can easily automate many tasks that are difficult or impossible to automate in an on-premises environment.</span></span> <span data-ttu-id="7b107-114">Například můžete nastavit celou testovací prostředí, včetně nového webovém serveru a back endové virtuální počítače, databáze, blob storage (úložiště souborů), fronty, atd.</span><span class="sxs-lookup"><span data-stu-id="7b107-114">For example, you can set up whole test environments including new web server and back-end VMs, databases, blob storage (file storage), queues, etc.</span></span>

## <a name="devops-workflow"></a><span data-ttu-id="7b107-115">Pracovní postup DevOps</span><span class="sxs-lookup"><span data-stu-id="7b107-115">DevOps Workflow</span></span>

<span data-ttu-id="7b107-116">Stále uslyšíte termín "DevOps."</span><span class="sxs-lookup"><span data-stu-id="7b107-116">Increasingly you hear the term "DevOps."</span></span> <span data-ttu-id="7b107-117">Výraz vyvinuté mimo rozpoznávání nutné integrovat úlohy vývoje a provozu, abyste mohli efektivně vyvíjet software.</span><span class="sxs-lookup"><span data-stu-id="7b107-117">The term developed out of a recognition that you have to integrate development and operations tasks in order to develop software efficiently.</span></span> <span data-ttu-id="7b107-118">Typ pracovního postupu, který chcete povolit je jedna, ve kterém můžete vyvíjet aplikace, nasaďte ji, Učte se od produkční účely v jeho, změňte ji v reakci na co jste se naučili a opakovat cyklus rychle a spolehlivě.</span><span class="sxs-lookup"><span data-stu-id="7b107-118">The kind of workflow you want to enable is one in which you can develop an app, deploy it, learn from production usage of it, change it in response to what you've learned, and repeat the cycle quickly and reliably.</span></span>

<span data-ttu-id="7b107-119">Některé vývojové týmy úspěšní cloudoví nasadit více než jednou za den na živém prostředí.</span><span class="sxs-lookup"><span data-stu-id="7b107-119">Some successful cloud development teams deploy multiple times a day to a live environment.</span></span> <span data-ttu-id="7b107-120">Tým Azure použít k nasazení hlavní aktualizace každé 2 až 3 měsíce, ale teď ji verze menší každé 2 – 3 dny a hlavní verzí každé 2 až 3 týdny.</span><span class="sxs-lookup"><span data-stu-id="7b107-120">The Azure team used to deploy a major update every 2-3 months, but now it releases minor updates every 2-3 days and major releases every 2-3 weeks.</span></span> <span data-ttu-id="7b107-121">Certifikace do tohoto tempo ve skutečnosti vám pomůže se reagovat na zpětnou vazbu od zákazníků.</span><span class="sxs-lookup"><span data-stu-id="7b107-121">Getting into that cadence really helps you be responsive to customer feedback.</span></span>

<span data-ttu-id="7b107-122">Aby bylo možné provést, budete muset povolit cyklu vývoje a nasazení opakovatelným, spolehlivý a předvídatelný a má nízkou cyklu.</span><span class="sxs-lookup"><span data-stu-id="7b107-122">In order to do that, you have to enable a development and deployment cycle that is repeatable, reliable, predictable, and has low cycle time.</span></span>

![Pracovní postup DevOps](automate-everything/_static/image1.png)

<span data-ttu-id="7b107-124">Jinými slovy musí být časový úsek mezi Pokud máte nápad na funkci a když se zákazníci jeho použití a poskytnutí zpětné vazby co nejkratší.</span><span class="sxs-lookup"><span data-stu-id="7b107-124">In other words, the period of time between when you have an idea for a feature and when the customers are using it and providing feedback must be as short as possible.</span></span> <span data-ttu-id="7b107-125">První tři vzory – plná, správy zdrojového kódu, automatizace a průběžnou integraci a doručování – se používají osvědčené postupy, které doporučujeme, aby bylo možné povolit tento druh procesu.</span><span class="sxs-lookup"><span data-stu-id="7b107-125">The first three patterns – automate everything, source control, and continuous integration and delivery -- are all about best practices that we recommend in order to enable that kind of process.</span></span>

## <a name="azure-management-scripts"></a><span data-ttu-id="7b107-126">Skripty pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="7b107-126">Azure management scripts</span></span>

<span data-ttu-id="7b107-127">V [Úvod do e kniha](introduction.md), jste viděli webové konzoly na portálu pro správu Azure.</span><span class="sxs-lookup"><span data-stu-id="7b107-127">In the [introduction to this e-book](introduction.md), you saw the web-based console, the Azure Management Portal.</span></span> <span data-ttu-id="7b107-128">Na portálu pro správu umožňuje monitorovat a spravovat všechny prostředky, které jste nasadili v Azure.</span><span class="sxs-lookup"><span data-stu-id="7b107-128">The management portal enables you to monitor and manage all of the resources that you have deployed on Azure.</span></span> <span data-ttu-id="7b107-129">Je snadný způsob, jak vytvářet a odstraňovat služeb, jako je webové aplikace a virtuální počítače, tyto služby konfigurovat, monitorovat operace služby a tak dále.</span><span class="sxs-lookup"><span data-stu-id="7b107-129">It's an easy way to create and delete services such as web apps and VMs, configure those services, monitor service operation, and so forth.</span></span> <span data-ttu-id="7b107-130">Je to skvělý nástroj, ale jeho pomocí provádí ručně.</span><span class="sxs-lookup"><span data-stu-id="7b107-130">It's a great tool, but using it is a manual process.</span></span> <span data-ttu-id="7b107-131">Pokud se chystáte vyvíjet produkční aplikace všech velikostí a obzvláště v prostředí team, doporučujeme projít uživatelského rozhraní, pokud chcete další informace a prozkoumat Azure portal a potom automatizovat procesy, které budete opakovaně dělat.</span><span class="sxs-lookup"><span data-stu-id="7b107-131">If you're going to develop a production application of any size, and especially in a team environment, we recommend that you go through the portal UI in order to learn and explore Azure, and then automate the processes that you'll be doing repetitively.</span></span>

<span data-ttu-id="7b107-132">Téměř vše, co můžete udělat ručně v portálu pro správu nebo ze sady Visual Studio je možné provést pomocí volání rozhraní API pro správu REST.</span><span class="sxs-lookup"><span data-stu-id="7b107-132">Nearly everything that you can do manually in the management portal or from Visual Studio can also be done by calling the REST management API.</span></span> <span data-ttu-id="7b107-133">Můžete psát skripty pomocí [prostředí Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), nebo můžete použít open source architektura [Chef](http://www.opscode.com/chef/) nebo [Puppet](http://puppetlabs.com/puppet/what-is-puppet).</span><span class="sxs-lookup"><span data-stu-id="7b107-133">You can write scripts using [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), or you can use an open source framework such as [Chef](http://www.opscode.com/chef/) or [Puppet](http://puppetlabs.com/puppet/what-is-puppet).</span></span> <span data-ttu-id="7b107-134">Můžete také použít nástroj příkazového řádku prostředí Bash v prostředí Mac nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="7b107-134">You can also use the Bash command-line tool in a Mac or Linux environment.</span></span> <span data-ttu-id="7b107-135">Azure nemá skriptovací rozhraní API pro těchto různých prostředích a má [.NET API pro správu](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) v případě, že chcete napsat kód namísto skriptu.</span><span class="sxs-lookup"><span data-stu-id="7b107-135">Azure has scripting APIs for all those different environments, and it has a [.NET management API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) in case you want to write code instead of script.</span></span>

<span data-ttu-id="7b107-136">Aplikace Fix It jsme vytvořili několik prostředí Windows PowerShell skripty, které automatizují procesy vytváření testovacího prostředí a nasazení projektu do daného prostředí. proto si probereme některé obsah těchto skriptů.</span><span class="sxs-lookup"><span data-stu-id="7b107-136">For the Fix It app we've created some Windows PowerShell scripts that automate the processes of creating a test environment and deploying the project to that environment, and we'll review some of the contents of those scripts.</span></span>

## <a name="environment-creation-script"></a><span data-ttu-id="7b107-137">Vytvoření skriptu prostředí</span><span class="sxs-lookup"><span data-stu-id="7b107-137">Environment creation script</span></span>

<span data-ttu-id="7b107-138">První skript podíváme na jmenuje *New-AzureWebsiteEnv.ps1*.</span><span class="sxs-lookup"><span data-stu-id="7b107-138">The first script we'll look at is named *New-AzureWebsiteEnv.ps1*.</span></span> <span data-ttu-id="7b107-139">Vytvoří prostředí Azure, můžete nasadit Fix It aplikaci pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="7b107-139">It creates an Azure environment that you can deploy the Fix It app to for testing.</span></span> <span data-ttu-id="7b107-140">Hlavní úkoly, které tento skript provádí jsou následující:</span><span class="sxs-lookup"><span data-stu-id="7b107-140">The main tasks that this script performs are the following:</span></span>

- <span data-ttu-id="7b107-141">Vytvoření webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7b107-141">Create a web app.</span></span>
- <span data-ttu-id="7b107-142">Vytvoření účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="7b107-142">Create a storage account.</span></span> <span data-ttu-id="7b107-143">(Vyžadováno pro objekty BLOB a fronty, jak uvidíte v dalších kapitolách.)</span><span class="sxs-lookup"><span data-stu-id="7b107-143">(Required for blobs and queues, as you'll see in later chapters.)</span></span>
- <span data-ttu-id="7b107-144">Vytvoření databáze SQL serveru a dvě databáze: databáze aplikace a databáze členství.</span><span class="sxs-lookup"><span data-stu-id="7b107-144">Create a SQL Database server and two databases: an application database, and a membership database.</span></span>
- <span data-ttu-id="7b107-145">Store nastavení v Azure, která aplikace bude používat pro přístup k účtu úložiště a databáze.</span><span class="sxs-lookup"><span data-stu-id="7b107-145">Store settings in Azure that the app will use to access the storage account and databases.</span></span>
- <span data-ttu-id="7b107-146">Vytvoření souborů s nastavením, které se použijí k automatizaci nasazení.</span><span class="sxs-lookup"><span data-stu-id="7b107-146">Create settings files that will be used to automate deployment.</span></span>

### <a name="run-the-script"></a><span data-ttu-id="7b107-147">Spusťte skript</span><span class="sxs-lookup"><span data-stu-id="7b107-147">Run the script</span></span>


> [!NOTE]
> <span data-ttu-id="7b107-148">Tato část kapitoly zobrazuje příklady, které zadáte, aby bylo možné je spouštět příkazy a skripty.</span><span class="sxs-lookup"><span data-stu-id="7b107-148">This part of the chapter shows examples of scripts and the commands that you enter in order to run them.</span></span> <span data-ttu-id="7b107-149">Tato ukázka a neposkytuje všechno, co potřebujete vědět, chcete-li spustit skripty.</span><span class="sxs-lookup"><span data-stu-id="7b107-149">This a demo and doesn't provide everything you need to know in order to run the scripts.</span></span> <span data-ttu-id="7b107-150">Podrobné postupy-k-it pokyny najdete v tématu [příloha: Opravte ji ukázkové aplikace](the-fix-it-sample-application.md#deploybase).</span><span class="sxs-lookup"><span data-stu-id="7b107-150">For step-by-step how-to-do-it instructions, see [Appendix: The Fix It Sample Application](the-fix-it-sample-application.md#deploybase).</span></span>


<span data-ttu-id="7b107-151">Chcete-li spustit skript prostředí PowerShell, který spravuje služby Azure, které je nutné nainstalovat konzolu prostředí Azure PowerShell a nakonfigurujte ho na spolupráci s vaším předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="7b107-151">To run a PowerShell script that manages Azure services you have to install the Azure PowerShell console and configure it to work with your Azure subscription.</span></span> <span data-ttu-id="7b107-152">Po nastavení, můžete spustit Fix It prostředí vytváření skriptu s příkaz podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="7b107-152">Once you're set up, you can run the Fix It environment creation script with a command like this one:</span></span>

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

<span data-ttu-id="7b107-153">`Name` Parametr určuje název, který má použít při vytváření účtů databáze a úložišť a `SqlDatabasePassword` parametr určuje heslo pro účet správce, která bude vytvořena pro službu SQL Database.</span><span class="sxs-lookup"><span data-stu-id="7b107-153">The `Name` parameter specifies the name to be used when creating the database and storage accounts, and the `SqlDatabasePassword` parameter specifies the password for the admin account that will be created for SQL Database.</span></span> <span data-ttu-id="7b107-154">Existují další parametry, které můžete použít, že se podíváme na později.</span><span class="sxs-lookup"><span data-stu-id="7b107-154">There are other parameters you can use that we'll look at later.</span></span>

![Okno prostředí PowerShell](automate-everything/_static/image2.png)

<span data-ttu-id="7b107-156">Po dokončení skriptu uvidíte na portálu management portal, co byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="7b107-156">After the script finishes you can see in the management portal what was created.</span></span> <span data-ttu-id="7b107-157">Zjistíte dvě databáze:</span><span class="sxs-lookup"><span data-stu-id="7b107-157">You'll find two databases:</span></span>

![Databáze](automate-everything/_static/image3.png)

<span data-ttu-id="7b107-159">Účet úložiště:</span><span class="sxs-lookup"><span data-stu-id="7b107-159">A storage account:</span></span>

![Účet úložiště](automate-everything/_static/image4.png)

<span data-ttu-id="7b107-161">A webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="7b107-161">And a web app:</span></span>

![Webové stránky](automate-everything/_static/image5.png)

<span data-ttu-id="7b107-163">Na **konfigurovat** karta pro webovou aplikaci, uvidíte, že má nastavení účtu úložiště a připojovací řetězce databáze SQL nastavené pro opravu jeho aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7b107-163">On the **Configure** tab for the web app, you can see that it has the storage account settings and SQL database connection strings set up for the Fix It app.</span></span>

![appSettings a connectionStrings](automate-everything/_static/image6.png)

<span data-ttu-id="7b107-165">*Automatizace* složka teď obsahuje také  *&lt;zadaným hodnotám websitename&gt;.pubxml* souboru.</span><span class="sxs-lookup"><span data-stu-id="7b107-165">The *Automation* folder now also contains a *&lt;websitename&gt;.pubxml* file.</span></span> <span data-ttu-id="7b107-166">Tento soubor uchovává nastavení, které MSBuild použijete k nasazení aplikace do prostředí Azure, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="7b107-166">This file stores settings that MSBuild will use to deploy the application to the Azure environment that was just created.</span></span> <span data-ttu-id="7b107-167">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7b107-167">For example:</span></span>

[!code-xml[Main](automate-everything/samples/sample1.xml)]

<span data-ttu-id="7b107-168">Jak je vidět, skript vytvořil dokončení testovacího prostředí a celý proces se provádí v přibližně 90 sekund.</span><span class="sxs-lookup"><span data-stu-id="7b107-168">As you can see, the script has created a complete test environment, and the whole process is done in about 90 seconds.</span></span>

<span data-ttu-id="7b107-169">Pokud někdo ve vašem týmu chce vytvořit testovací prostředí, lze pouze spustit skript.</span><span class="sxs-lookup"><span data-stu-id="7b107-169">If someone else on your team wants to create a test environment, they can just run the script.</span></span> <span data-ttu-id="7b107-170">Nejenže je rychlý, ale také mohli být jistí, že používáte stejný jako ten, který používáte prostředí.</span><span class="sxs-lookup"><span data-stu-id="7b107-170">Not only is it fast, but also they can be confident that they are using an environment identical to the one you're using.</span></span> <span data-ttu-id="7b107-171">Nebylo možné úplně jistí, který pokud všem uživatelům se nastavit věci ručně pomocí portálu pro správu uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="7b107-171">You couldn't be quite as confident of that if everyone was setting things up manually by using the management portal UI.</span></span>

### <a name="a-look-at-the-scripts"></a><span data-ttu-id="7b107-172">Podívejte se na skripty</span><span class="sxs-lookup"><span data-stu-id="7b107-172">A look at the scripts</span></span>

<span data-ttu-id="7b107-173">Nejsou ve skutečnosti tři skripty, které tuto práci.</span><span class="sxs-lookup"><span data-stu-id="7b107-173">There are actually three scripts that do this work.</span></span> <span data-ttu-id="7b107-174">Můžete volat z příkazového řádku a automaticky použije další dvě provést některé úlohy:</span><span class="sxs-lookup"><span data-stu-id="7b107-174">You call one from the command line and it automatically uses the other two to do some of the tasks:</span></span>

- <span data-ttu-id="7b107-175">*Nové AzureWebSiteEnv.ps1* je hlavního skriptu.</span><span class="sxs-lookup"><span data-stu-id="7b107-175">*New-AzureWebSiteEnv.ps1* is the main script.</span></span>

    - <span data-ttu-id="7b107-176">*Nové AzureStorage.ps1* vytvoří účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="7b107-176">*New-AzureStorage.ps1* creates the storage account.</span></span>
    - <span data-ttu-id="7b107-177">*Nové AzureSql.ps1* vytvoří databáze.</span><span class="sxs-lookup"><span data-stu-id="7b107-177">*New-AzureSql.ps1* creates the databases.</span></span>

### <a name="parameters-in-the-main-script"></a><span data-ttu-id="7b107-178">Parametry v hlavním skriptu</span><span class="sxs-lookup"><span data-stu-id="7b107-178">Parameters in the main script</span></span>

<span data-ttu-id="7b107-179">Hlavní skript *New-AzureWebSiteEnv.ps1*, definuje několik parametrů:</span><span class="sxs-lookup"><span data-stu-id="7b107-179">The main script, *New-AzureWebSiteEnv.ps1*, defines several parameters:</span></span>

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

<span data-ttu-id="7b107-180">Jsou vyžadovány dva parametry:</span><span class="sxs-lookup"><span data-stu-id="7b107-180">Two parameters are required:</span></span>

- <span data-ttu-id="7b107-181">Název webové aplikace, které skript vytvoří.</span><span class="sxs-lookup"><span data-stu-id="7b107-181">The name of the web app that the script creates.</span></span> <span data-ttu-id="7b107-182">(To se také používá pro adresu URL: `<name>.azurewebsites.net`.)</span><span class="sxs-lookup"><span data-stu-id="7b107-182">(This is also used for the URL: `<name>.azurewebsites.net`.)</span></span>
- <span data-ttu-id="7b107-183">Heslo pro nového správce databázového serveru, který vytvoří skript.</span><span class="sxs-lookup"><span data-stu-id="7b107-183">The password for the new administrative user of the database server that the script creates.</span></span>

<span data-ttu-id="7b107-184">Volitelné parametry umožňují určit umístěním datového centra (výchozí nastavení "Západní USA"), správce název databázového serveru (výchozí nastavení "dbuser") a pravidla brány firewall pro databázový server.</span><span class="sxs-lookup"><span data-stu-id="7b107-184">Optional parameters enable you to specify the data center location (defaults to "West US"), database server administrator name (defaults to "dbuser"), and a firewall rule for the database server.</span></span>

### <a name="create-the-web-app"></a><span data-ttu-id="7b107-185">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="7b107-185">Create the web app</span></span>

<span data-ttu-id="7b107-186">První věc, kterou skriptu je vytvoření webové aplikace pomocí volání `New-AzureWebsite` rutiny předáním ve webové aplikaci hodnoty název a umístění parametru:</span><span class="sxs-lookup"><span data-stu-id="7b107-186">The first thing the script does is create the web app by calling the `New-AzureWebsite` cmdlet, passing in to it the web app name and location parameter values:</span></span>

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a><span data-ttu-id="7b107-187">Vytvoření účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="7b107-187">Create the storage account</span></span>

<span data-ttu-id="7b107-188">Pak hlavní skript se spustí <em>New-AzureStorage.ps1</em> skriptu, určení "<em>&lt;zadaným hodnotám websitename&gt;</em>úložiště" pro název účtu úložiště a stejné datové centrum jako umístění webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7b107-188">Then the main script runs the <em>New-AzureStorage.ps1</em> script, specifying "<em>&lt;websitename&gt;</em>storage" for the storage account name, and the same data center location as the web app.</span></span>

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

<span data-ttu-id="7b107-189">*Nové AzureStorage.ps1* volání `New-AzureStorageAccount` rutina pro vytvoření účtu úložiště a vrátí účet klíčové hodnoty názvu a přístup.</span><span class="sxs-lookup"><span data-stu-id="7b107-189">*New-AzureStorage.ps1* calls the `New-AzureStorageAccount` cmdlet to create the storage account, and it returns the account name and access key values.</span></span> <span data-ttu-id="7b107-190">Aplikace tyto hodnoty budete potřebovat pro přístup k objektům BLOB a fronty v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="7b107-190">The application will need these values in order to access the blobs and queues in the storage account.</span></span>

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

<span data-ttu-id="7b107-191">Vždy nebudete chtít vytvořit nový účet úložiště; skript může vylepšit tak, že přidáte parametr, který volitelně přesměruje ho na použití existující účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="7b107-191">You might not always want to create a new storage account; you could enhance the script by adding a parameter that optionally directs it to use an existing storage account.</span></span>

### <a name="create-the-databases"></a><span data-ttu-id="7b107-192">Vytvoření databází</span><span class="sxs-lookup"><span data-stu-id="7b107-192">Create the databases</span></span>

<span data-ttu-id="7b107-193">Pak spustí skript vytvoření databáze, hlavního skriptu *New-AzureSql.ps1*, po nastavení výchozí databáze a názvy pravidel brány firewall:</span><span class="sxs-lookup"><span data-stu-id="7b107-193">The main script then runs the database creation script, *New-AzureSql.ps1*, after setting up default database and firewall rule names:</span></span>

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

<span data-ttu-id="7b107-194">Skript vytvoření databáze načte vývojového počítače IP adresu a nastaví pravidlo brány firewall, tak vývojovém počítači můžete připojit k a správě serveru.</span><span class="sxs-lookup"><span data-stu-id="7b107-194">The database creation script retrieves the dev machine's IP address and sets a firewall rule so the dev machine can connect to and manage the server.</span></span> <span data-ttu-id="7b107-195">Vytvoření skriptu databáze dále prochází kroky k několika kroky k nastavení databáze:</span><span class="sxs-lookup"><span data-stu-id="7b107-195">The database creation script then goes through several steps to set up the databases:</span></span>

- <span data-ttu-id="7b107-196">Vytvoří na serveru s použitím `New-AzureSqlDatabaseServer` rutiny.</span><span class="sxs-lookup"><span data-stu-id="7b107-196">Creates the server by using the `New-AzureSqlDatabaseServer` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- <span data-ttu-id="7b107-197">Vytvoří pravidla brány firewall pro povolení vývojovém počítači ke správě serveru a k tomu, aby webové aplikace k němu připojit.</span><span class="sxs-lookup"><span data-stu-id="7b107-197">Creates firewall rules to enable the dev machine to manage the server and to enable the web app to connect to it.</span></span> 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- <span data-ttu-id="7b107-198">Vytvoří kontext databáze, která obsahuje název serveru a přihlašovací údaje, pomocí `New-AzureSqlDatabaseServerContext` rutiny.</span><span class="sxs-lookup"><span data-stu-id="7b107-198">Creates a database context that includes the server name and credentials, by using the `New-AzureSqlDatabaseServerContext` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    <span data-ttu-id="7b107-199">`New-PSCredentialFromPlainText` je funkce ve skriptu, který volá `ConvertTo-SecureString` rutiny můžete šifrovat hesla a vrátí `PSCredential` objektu stejného typu, který `Get-Credential` rutina vrátí.</span><span class="sxs-lookup"><span data-stu-id="7b107-199">`New-PSCredentialFromPlainText` is a function in the script that calls the `ConvertTo-SecureString` cmdlet to encrypt the password and returns a `PSCredential` object, the same type that the `Get-Credential` cmdlet returns.</span></span>
- <span data-ttu-id="7b107-200">Vytvoří aplikační databázi a databázi členství pomocí `New-AzureSqlDatabase` rutiny.</span><span class="sxs-lookup"><span data-stu-id="7b107-200">Creates the application database and the membership database by using the `New-AzureSqlDatabase` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- <span data-ttu-id="7b107-201">Volá místně definované funkce tocreates připojovací řetězec pro každou databázi.</span><span class="sxs-lookup"><span data-stu-id="7b107-201">Calls a locally defined function tocreates a connection string for each database.</span></span> <span data-ttu-id="7b107-202">Aplikace bude používat tyto připojovací řetězce pro přístup k databázím.</span><span class="sxs-lookup"><span data-stu-id="7b107-202">The application will use these connection strings to access the databases.</span></span> 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    <span data-ttu-id="7b107-203">Get-SQLAzureDatabaseConnectionString je funkci definovanou ve skriptu, který vytváří připojovací řetězec z hodnoty parametrů zadat do něj.</span><span class="sxs-lookup"><span data-stu-id="7b107-203">Get-SQLAzureDatabaseConnectionString is a function defined in the script that creates the connection string from the parameter values supplied to it.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- <span data-ttu-id="7b107-204">Vrátí zatřiďovací tabulku s název databázového serveru a připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="7b107-204">Returns a hash table with the database server name and the connection strings.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

<span data-ttu-id="7b107-205">Aplikace Fix It používá samostatné členství a aplikačních databází.</span><span class="sxs-lookup"><span data-stu-id="7b107-205">The Fix It app uses separate membership and application databases.</span></span> <span data-ttu-id="7b107-206">Je také možné umístit data členství a aplikace v jedné databázi.</span><span class="sxs-lookup"><span data-stu-id="7b107-206">It's also possible to put both membership and application data in a single database.</span></span>

### <a name="store-app-settings-and-connection-strings"></a><span data-ttu-id="7b107-207">Nastavení aplikací pro Store a připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="7b107-207">Store app settings and connection strings</span></span>

<span data-ttu-id="7b107-208">Azure nabízí funkce, která umožňuje ukládání nastavení a připojovací řetězce, které automaticky přepsat, co se vrátí do aplikace při pokusu o čtení `appSettings` nebo `connectionStrings` kolekcí v souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="7b107-208">Azure has a feature that enables you to store settings and connection strings that automatically override what is returned to the application when it tries to read the `appSettings` or `connectionStrings` collections in the Web.config file.</span></span> <span data-ttu-id="7b107-209">Jedná se o alternativu k použití [transformace Web.config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) při nasazení.</span><span class="sxs-lookup"><span data-stu-id="7b107-209">This is an alternative to applying [Web.config transformations](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) when you deploy.</span></span> <span data-ttu-id="7b107-210">Další informace najdete v tématu [Store citlivá data v Azure](source-control.md#appsettings) dále v této e knihy.</span><span class="sxs-lookup"><span data-stu-id="7b107-210">For more information, see [Store sensitive data in Azure](source-control.md#appsettings) later in this e-book.</span></span>

<span data-ttu-id="7b107-211">Vytvoření skriptu prostředí uloží v Azure všechny `appSettings` a `connectionStrings` hodnoty, které aplikace potřebuje pro přístup k účtu úložiště a databáze při spuštění v Azure.</span><span class="sxs-lookup"><span data-stu-id="7b107-211">The environment creation script stores in Azure all of the `appSettings` and `connectionStrings` values that the application needs to access the storage account and databases when it runs in Azure.</span></span>

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

<span data-ttu-id="7b107-212">[Nástroj společnosti New Relic](http://newrelic.com/) je architektura telemetrická data, která vám ukážeme v [monitorování a Telemetrie](monitoring-and-telemetry.md) kapitoly.</span><span class="sxs-lookup"><span data-stu-id="7b107-212">[New Relic](http://newrelic.com/) is a telemetry framework that we demonstrate in the [Monitoring and Telemetry](monitoring-and-telemetry.md) chapter.</span></span> <span data-ttu-id="7b107-213">Vytvoření skriptu prostředí se také restartuje, webové aplikace, abyste měli jistotu, že vybere nastavení New Relic.</span><span class="sxs-lookup"><span data-stu-id="7b107-213">The environment creation script also restarts the web app to make sure that it picks up the New Relic settings.</span></span>

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a><span data-ttu-id="7b107-214">Příprava pro nasazení</span><span class="sxs-lookup"><span data-stu-id="7b107-214">Preparing for deployment</span></span>

<span data-ttu-id="7b107-215">Na konci procesu vytváření skript prostředí volá dvě funkce k vytvoření souborů, které se použijí skriptem nasazení.</span><span class="sxs-lookup"><span data-stu-id="7b107-215">At the end of the process, the environment creation script calls two functions to create files that will be used by the deployment script.</span></span>

<span data-ttu-id="7b107-216">Jeden z těchto funkcí vytvoří profil publikování *(&lt;zadaným hodnotám websitename&gt;.pubxml* souboru).</span><span class="sxs-lookup"><span data-stu-id="7b107-216">One of these functions creates a publish profile *(&lt;websitename&gt;.pubxml* file).</span></span> <span data-ttu-id="7b107-217">Kód volá rozhraní Azure REST API pro získání nastavení publikování a uloží informace *.publishsettings* souboru.</span><span class="sxs-lookup"><span data-stu-id="7b107-217">The code calls the Azure REST API to get the publish settings, and it saves the information in a *.publishsettings* file.</span></span> <span data-ttu-id="7b107-218">Potom použije informace z tohoto souboru spolu s soubor šablony (*pubxml.template*) Chcete-li vytvořit *.pubxml* soubor, který obsahuje profil publikování.</span><span class="sxs-lookup"><span data-stu-id="7b107-218">Then it uses the information from that file along with a template file (*pubxml.template*) to create the *.pubxml* file that contains the publish profile.</span></span> <span data-ttu-id="7b107-219">Tento dvoukrokový proces simuluje, co můžete dělat v sadě Visual Studio: Stáhněte si *.publishsettings* souboru a import, který chcete vytvořit profil publikování.</span><span class="sxs-lookup"><span data-stu-id="7b107-219">This two-step process simulates what you do in Visual Studio: download a *.publishsettings* file and import that to create a publish profile.</span></span>

<span data-ttu-id="7b107-220">Další funkce používá jiný soubor šablony (Web environment.template) k vytvoření *webu environment.xml* soubor, který obsahuje nastavení, skript nasazení použije spolu s *.pubxml*souboru.</span><span class="sxs-lookup"><span data-stu-id="7b107-220">The other function uses another template file (website-environment.template) to create a *website-environment.xml* file that contains settings the deployment script will use along with the *.pubxml* file.</span></span>

### <a name="troubleshooting-and-error-handling"></a><span data-ttu-id="7b107-221">Řešení potíží a zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="7b107-221">Troubleshooting and error handling</span></span>

<span data-ttu-id="7b107-222">Skripty jsou jako programy: volání můžou selhat a kdy to dělají budete chtít vědět co nejvíce o selhání a co způsobilo vypršení jejího.</span><span class="sxs-lookup"><span data-stu-id="7b107-222">Scripts are like programs: they can fail, and when they do you want to know as much as you can about the failure and what caused it.</span></span> <span data-ttu-id="7b107-223">Z tohoto důvodu skriptu pro vytváření prostředí změní hodnotu `VerbosePreference` proměnné z `SilentlyContinue` k `Continue` tak, aby se zobrazují všechny podrobné zprávy.</span><span class="sxs-lookup"><span data-stu-id="7b107-223">For this reason, the environment creation script changes the value of the `VerbosePreference` variable from `SilentlyContinue` to `Continue` so that all verbose messages are displayed.</span></span> <span data-ttu-id="7b107-224">Také změní hodnotu `ErrorActionPreference` proměnné z `Continue` k `Stop`tak, aby skript zastaví, i když se setká s neukončujícími chybami:</span><span class="sxs-lookup"><span data-stu-id="7b107-224">It also changes the value of the `ErrorActionPreference` variable from `Continue` to `Stop`, so that the script stops even when it encounters non-terminating errors:</span></span>

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

<span data-ttu-id="7b107-225">Předtím, než ho nemá žádnou práci, ukládá skript tak, aby ho můžete výpočet uplynulého času, po dokončení počáteční čas:</span><span class="sxs-lookup"><span data-stu-id="7b107-225">Before it does any work, the script stores the start time so that it can calculate the elapsed time when it's done:</span></span>

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

<span data-ttu-id="7b107-226">Jakmile se dokončí svou práci, skript zobrazí uplynulý čas:</span><span class="sxs-lookup"><span data-stu-id="7b107-226">After it completes its work, the script displays the elapsed time:</span></span>

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

<span data-ttu-id="7b107-227">A pro všechny klíčové operace skript zapisuje podrobné zprávy, například:</span><span class="sxs-lookup"><span data-stu-id="7b107-227">And for every key operation the script writes verbose messages, for example:</span></span>

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a><span data-ttu-id="7b107-228">Skript nasazení</span><span class="sxs-lookup"><span data-stu-id="7b107-228">Deployment script</span></span>

<span data-ttu-id="7b107-229">Co *AzureWebsiteEnv.ps1 nový* skript provádí pro vytvoření prostředí *publikovat AzureWebsite.ps1* skript provádí pro nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="7b107-229">What the *New-AzureWebsiteEnv.ps1* script does for environment creation, the *Publish-AzureWebsite.ps1* script does for application deployment.</span></span>

<span data-ttu-id="7b107-230">Získá název webové aplikace ze skriptu nasazení *webu environment.xml* soubor vytvořený pomocí skriptu pro vytváření prostředí.</span><span class="sxs-lookup"><span data-stu-id="7b107-230">The deployment script gets the name of the web app from the *website-environment.xml* file created by the environment creation script.</span></span>

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

<span data-ttu-id="7b107-231">Získá heslo uživatele nasazení z *.publishsettings* souboru:</span><span class="sxs-lookup"><span data-stu-id="7b107-231">It gets the deployment user password from the *.publishsettings* file:</span></span>

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

<span data-ttu-id="7b107-232">Je spuštěn [MSBuild](http://msbuildbook.com/) příkaz, který sestaví a nasadí projekt:</span><span class="sxs-lookup"><span data-stu-id="7b107-232">It executes the [MSBuild](http://msbuildbook.com/) command that builds and deploys the project:</span></span>

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

<span data-ttu-id="7b107-233">A pokud jste určili `Launch` parametr příkazového řádku, které volá `Show-AzureWebsite` rutiny a otevřete váš výchozí prohlížeč na adresu URL webu.</span><span class="sxs-lookup"><span data-stu-id="7b107-233">And if you've specified the `Launch` parameter on the command line, it calls the `Show-AzureWebsite` cmdlet to open your default browser to the website URL.</span></span>

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

<span data-ttu-id="7b107-234">Skript nasazení můžete spustit pomocí příkazu podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="7b107-234">You can run the deployment script with a command like this one:</span></span>

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

<span data-ttu-id="7b107-235">A po dokončení, v prohlížeči se otevře se web spuštěný v cloudu, a `<websitename>.azurewebsites.net` adresy URL.</span><span class="sxs-lookup"><span data-stu-id="7b107-235">And when it's done, the browser opens with the site running in the cloud at the `<websitename>.azurewebsites.net` URL.</span></span>

![Oprava aplikace nasazené na platformě Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a><span data-ttu-id="7b107-237">Souhrn</span><span class="sxs-lookup"><span data-stu-id="7b107-237">Summary</span></span>

<span data-ttu-id="7b107-238">S těmito skripty můžete být jisti, že stejný postup bude vždy provést ve stejném pořadí pomocí stejných možností.</span><span class="sxs-lookup"><span data-stu-id="7b107-238">With these scripts you can be confident that the same steps will always be executed in the same order using the same options.</span></span> <span data-ttu-id="7b107-239">To pomáhá zajistit, že každý vývojář v týmu není přijít o něco schválně pokazí něco nebo vlastní na vlastní počítač, nebudou fungovat ve skutečnosti stejným způsobem jako v prostředí jinému členovi týmu nebo v produkčním prostředí něco nasadila.</span><span class="sxs-lookup"><span data-stu-id="7b107-239">This helps ensure that each developer on the team doesn't miss something or mess something up or deploy something custom on his own machine that won't actually work the same way in another team member's environment or in production.</span></span>

<span data-ttu-id="7b107-240">Podobným způsobem můžete automatizovat většinu služeb Azure funkce správy, které můžou provádět na portálu management portal, pomocí rozhraní REST API, skriptů prostředí Windows PowerShell, jazyk rozhraní .NET API nebo prostředí Bash nástroj, který můžete spustit na Linuxu nebo macu.</span><span class="sxs-lookup"><span data-stu-id="7b107-240">In a similar way, you can automate most Azure management functions that you can do in the management portal, by using the REST API, Windows PowerShell scripts, a .NET language API, or a Bash utility that you can run on Linux or Mac.</span></span>

<span data-ttu-id="7b107-241">V [další kapitolu](source-control.md) vytvoříme podívejte se na zdrojový kód a vysvětlit, proč je důležité zahrnout skripty do vašeho úložiště zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="7b107-241">In the [next chapter](source-control.md) we'll look at source code and explain why it's important to include your scripts in your source code repository.</span></span>

## <a name="resources"></a><span data-ttu-id="7b107-242">Prostředky</span><span class="sxs-lookup"><span data-stu-id="7b107-242">Resources</span></span>

- <span data-ttu-id="7b107-243">[Instalace a konfigurace Windows Powershellu pro Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).</span><span class="sxs-lookup"><span data-stu-id="7b107-243">[Install and Configure Windows PowerShell for Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).</span></span> <span data-ttu-id="7b107-244">Vysvětluje, jak nainstalovat nejnovější verzi rutin Azure Powershellu a nainstalovat certifikát, musí v počítači, abyste mohli spravovat Azure započítat i.</span><span class="sxs-lookup"><span data-stu-id="7b107-244">Explains how to install the Azure PowerShell cmdlets and how to install the certificate that you need on your computer in order to manage your Azure account.</span></span> <span data-ttu-id="7b107-245">To je skvělé místo, kde můžete začít pracovat, protože má také odkazy na zdroje informací pro výuku Powershellu samotný.</span><span class="sxs-lookup"><span data-stu-id="7b107-245">This is a great place to get started because it also has links to resources for learning PowerShell itself.</span></span>
- <span data-ttu-id="7b107-246">[Centrum skriptů Azure](https://docs.microsoft.com/azure/automation/automation-runbook-gallery).</span><span class="sxs-lookup"><span data-stu-id="7b107-246">[Azure Script Center](https://docs.microsoft.com/azure/automation/automation-runbook-gallery).</span></span> <span data-ttu-id="7b107-247">Zdroje informací pro vývoj skriptů, které správy služeb Azure, s odkazy na kurzy Začínáme, rutina referenční dokumentaci a zdrojový kód a ukázky skriptů pro portál WindowsAzure.com</span><span class="sxs-lookup"><span data-stu-id="7b107-247">WindowsAzure.com portal to resources for developing scripts that manage Azure services, with links to getting started tutorials, cmdlet reference documentation and source code, and sample scripts</span></span>
- <span data-ttu-id="7b107-248">[Tvůrce skriptů víkendu: Začínáme s Azure a Powershellu](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx).</span><span class="sxs-lookup"><span data-stu-id="7b107-248">[Weekend Scripter: Getting Started with Azure and PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx).</span></span> <span data-ttu-id="7b107-249">Tento příspěvek na blogu vyhrazená pro prostředí Windows PowerShell, poskytuje vynikající Úvod do používání Powershellu pro správu Azure functions.</span><span class="sxs-lookup"><span data-stu-id="7b107-249">In a blog dedicated to Windows PowerShell, this post provides a great introduction to using PowerShell for Azure management functions.</span></span>
- <span data-ttu-id="7b107-250">[Instalace a konfigurace rozhraní příkazového řádku Azure Cross-Platform](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="7b107-250">[Install and Configure the Azure Cross-Platform Command-Line Interface](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).</span></span> <span data-ttu-id="7b107-251">Úvodní kurz pro Azure skriptovací framework, která funguje na Mac a Linux, jakož i Windows systémy.</span><span class="sxs-lookup"><span data-stu-id="7b107-251">Getting-started tutorial for an Azure scripting framework that works on Mac and Linux as well as Windows systems.</span></span>
- <span data-ttu-id="7b107-252">[Nástroje příkazového řádku části tématu stáhnout sady Azure SDK a nástroje](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="7b107-252">[Command-line tools section of the Download Azure SDKs and Tools topic](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="7b107-253">Stránka portálu dokumentace a soubory ke stažení týkající se nástrojů příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="7b107-253">Portal page for documentation and downloads related to command-line tools for Azure.</span></span>
- <span data-ttu-id="7b107-254">[Automatizace všechno, co s knihovnami správy Azure a .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx).</span><span class="sxs-lookup"><span data-stu-id="7b107-254">[Automating everything with the Azure Management Libraries and .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx).</span></span> <span data-ttu-id="7b107-255">Scott Hanselman přináší rozhraní .NET API pro správu Azure.</span><span class="sxs-lookup"><span data-stu-id="7b107-255">Scott Hanselman introduces the .NET management API for Azure.</span></span>
- <span data-ttu-id="7b107-256">[Pomocí skriptů Windows Powershellu k publikování do vývojových a testovacích prostředí](https://msdn.microsoft.com/library/azure/dn642480.aspx).</span><span class="sxs-lookup"><span data-stu-id="7b107-256">[Using Windows PowerShell Scripts to Publish to Dev and Test Environments](https://msdn.microsoft.com/library/azure/dn642480.aspx).</span></span> <span data-ttu-id="7b107-257">Dokumentace MSDN, který vysvětluje, jak použít publikování skripty, které aplikace Visual Studio generuje pro webové projekty.</span><span class="sxs-lookup"><span data-stu-id="7b107-257">MSDN documentation that explains how to use publish scripts that Visual Studio automatically generates for web projects.</span></span>
- <span data-ttu-id="7b107-258">[Nástroje PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597).</span><span class="sxs-lookup"><span data-stu-id="7b107-258">[PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597).</span></span> <span data-ttu-id="7b107-259">Visual Studio rozšíření, které přidává podporu jazyka pro prostředí Windows PowerShell v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7b107-259">Visual Studio extension that adds language support for Windows PowerShell in Visual Studio.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7b107-260">[Předchozí](introduction.md)
> [další](source-control.md)</span><span class="sxs-lookup"><span data-stu-id="7b107-260">[Previous](introduction.md)
[Next](source-control.md)</span></span>
