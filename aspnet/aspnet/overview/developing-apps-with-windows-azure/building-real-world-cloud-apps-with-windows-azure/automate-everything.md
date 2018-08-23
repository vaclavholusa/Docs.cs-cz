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
ms.openlocfilehash: 4acf7361bed7e135c73cd46c99d15e12757e4679
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755728"
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatizace všechno, co (sestavování skutečných cloudových aplikací s Azure)
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Úvod do e kniha najdete v tématu [první kapitoly](introduction.md).


První tři vzory, které se podíváme na ve skutečnosti platí do jakéhokoli projektu vývoje softwaru, ale především pro projekty v cloudu. Tento model je o automatizaci úkolů vývoje. Je důležité tématu, protože ruční procesy jsou pomalé a náchylné; automatizace, kolik z nich, je to možné pomáhá nastavit rychlé, spolehlivé a flexibilní pracovní postup. Je jednoznačně důležité pro vývoj pro cloud, protože můžete snadno automatizovat mnoho úloh, které je obtížné či nemožné automatizace v místním prostředí. Například můžete nastavit celou testovací prostředí, včetně nového webovém serveru a back endové virtuální počítače, databáze, blob storage (úložiště souborů), fronty, atd.

## <a name="devops-workflow"></a>Pracovní postup DevOps

Stále uslyšíte termín "DevOps." Výraz vyvinuté mimo rozpoznávání nutné integrovat úlohy vývoje a provozu, abyste mohli efektivně vyvíjet software. Typ pracovního postupu, který chcete povolit je jedna, ve kterém můžete vyvíjet aplikace, nasaďte ji, Učte se od produkční účely v jeho, změňte ji v reakci na co jste se naučili a opakovat cyklus rychle a spolehlivě.

Některé vývojové týmy úspěšní cloudoví nasadit více než jednou za den na živém prostředí. Tým Azure použít k nasazení hlavní aktualizace každé 2 až 3 měsíce, ale teď ji verze menší každé 2 – 3 dny a hlavní verzí každé 2 až 3 týdny. Certifikace do tohoto tempo ve skutečnosti vám pomůže se reagovat na zpětnou vazbu od zákazníků.

Aby bylo možné provést, budete muset povolit cyklu vývoje a nasazení opakovatelným, spolehlivý a předvídatelný a má nízkou cyklu.

![Pracovní postup DevOps](automate-everything/_static/image1.png)

Jinými slovy musí být časový úsek mezi Pokud máte nápad na funkci a když se zákazníci jeho použití a poskytnutí zpětné vazby co nejkratší. První tři vzory – plná, správy zdrojového kódu, automatizace a průběžnou integraci a doručování – se používají osvědčené postupy, které doporučujeme, aby bylo možné povolit tento druh procesu.

## <a name="azure-management-scripts"></a>Skripty pro správu Azure

V [Úvod do e kniha](introduction.md), jste viděli webové konzoly na portálu pro správu Azure. Na portálu pro správu umožňuje monitorovat a spravovat všechny prostředky, které jste nasadili v Azure. Je snadný způsob, jak vytvářet a odstraňovat služeb, jako je webové aplikace a virtuální počítače, tyto služby konfigurovat, monitorovat operace služby a tak dále. Je to skvělý nástroj, ale jeho pomocí provádí ručně. Pokud se chystáte vyvíjet produkční aplikace všech velikostí a obzvláště v prostředí team, doporučujeme projít uživatelského rozhraní, pokud chcete další informace a prozkoumat Azure portal a potom automatizovat procesy, které budete opakovaně dělat.

Téměř vše, co můžete udělat ručně v portálu pro správu nebo ze sady Visual Studio je možné provést pomocí volání rozhraní API pro správu REST. Můžete psát skripty pomocí [prostředí Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), nebo můžete použít open source architektura [Chef](http://www.opscode.com/chef/) nebo [Puppet](http://puppetlabs.com/puppet/what-is-puppet). Můžete také použít nástroj příkazového řádku prostředí Bash v prostředí Mac nebo Linux. Azure nemá skriptovací rozhraní API pro těchto různých prostředích a má [.NET API pro správu](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) v případě, že chcete napsat kód namísto skriptu.

Aplikace Fix It jsme vytvořili několik prostředí Windows PowerShell skripty, které automatizují procesy vytváření testovacího prostředí a nasazení projektu do daného prostředí. proto si probereme některé obsah těchto skriptů.

## <a name="environment-creation-script"></a>Vytvoření skriptu prostředí

První skript podíváme na jmenuje *New-AzureWebsiteEnv.ps1*. Vytvoří prostředí Azure, můžete nasadit Fix It aplikaci pro účely testování. Hlavní úkoly, které tento skript provádí jsou následující:

- Vytvoření webové aplikace.
- Vytvoření účtu úložiště. (Vyžadováno pro objekty BLOB a fronty, jak uvidíte v dalších kapitolách.)
- Vytvoření databáze SQL serveru a dvě databáze: databáze aplikace a databáze členství.
- Store nastavení v Azure, která aplikace bude používat pro přístup k účtu úložiště a databáze.
- Vytvoření souborů s nastavením, které se použijí k automatizaci nasazení.

### <a name="run-the-script"></a>Spusťte skript


> [!NOTE]
> Tato část kapitoly zobrazuje příklady, které zadáte, aby bylo možné je spouštět příkazy a skripty. Tato ukázka a neposkytuje všechno, co potřebujete vědět, chcete-li spustit skripty. Podrobné postupy-k-it pokyny najdete v tématu [příloha: Opravte ji ukázkové aplikace](the-fix-it-sample-application.md#deploybase).


Chcete-li spustit skript prostředí PowerShell, který spravuje služby Azure, které je nutné nainstalovat konzolu prostředí Azure PowerShell a nakonfigurujte ho na spolupráci s vaším předplatným Azure. Po nastavení, můžete spustit Fix It prostředí vytváření skriptu s příkaz podobný následujícímu:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

`Name` Parametr určuje název, který má použít při vytváření účtů databáze a úložišť a `SqlDatabasePassword` parametr určuje heslo pro účet správce, která bude vytvořena pro službu SQL Database. Existují další parametry, které můžete použít, že se podíváme na později.

![Okno prostředí PowerShell](automate-everything/_static/image2.png)

Po dokončení skriptu uvidíte na portálu management portal, co byl vytvořen. Zjistíte dvě databáze:

![Databáze](automate-everything/_static/image3.png)

Účet úložiště:

![Účet úložiště](automate-everything/_static/image4.png)

A webové aplikace:

![Webové stránky](automate-everything/_static/image5.png)

Na **konfigurovat** karta pro webovou aplikaci, uvidíte, že má nastavení účtu úložiště a připojovací řetězce databáze SQL nastavené pro opravu jeho aplikaci.

![appSettings a connectionStrings](automate-everything/_static/image6.png)

*Automatizace* složka teď obsahuje také  *&lt;zadaným hodnotám websitename&gt;.pubxml* souboru. Tento soubor uchovává nastavení, které MSBuild použijete k nasazení aplikace do prostředí Azure, kterou jste právě vytvořili. Příklad:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Jak je vidět, skript vytvořil dokončení testovacího prostředí a celý proces se provádí v přibližně 90 sekund.

Pokud někdo ve vašem týmu chce vytvořit testovací prostředí, lze pouze spustit skript. Nejenže je rychlý, ale také mohli být jistí, že používáte stejný jako ten, který používáte prostředí. Nebylo možné úplně jistí, který pokud všem uživatelům se nastavit věci ručně pomocí portálu pro správu uživatelského rozhraní.

### <a name="a-look-at-the-scripts"></a>Podívejte se na skripty

Nejsou ve skutečnosti tři skripty, které tuto práci. Můžete volat z příkazového řádku a automaticky použije další dvě provést některé úlohy:

- *Nové AzureWebSiteEnv.ps1* je hlavního skriptu.

    - *Nové AzureStorage.ps1* vytvoří účet úložiště.
    - *Nové AzureSql.ps1* vytvoří databáze.

### <a name="parameters-in-the-main-script"></a>Parametry v hlavním skriptu

Hlavní skript *New-AzureWebSiteEnv.ps1*, definuje několik parametrů:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Jsou vyžadovány dva parametry:

- Název webové aplikace, které skript vytvoří. (To se také používá pro adresu URL: `<name>.azurewebsites.net`.)
- Heslo pro nového správce databázového serveru, který vytvoří skript.

Volitelné parametry umožňují určit umístěním datového centra (výchozí nastavení "Západní USA"), správce název databázového serveru (výchozí nastavení "dbuser") a pravidla brány firewall pro databázový server.

### <a name="create-the-web-app"></a>Vytvoření webové aplikace

První věc, kterou skriptu je vytvoření webové aplikace pomocí volání `New-AzureWebsite` rutiny předáním ve webové aplikaci hodnoty název a umístění parametru:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Vytvoření účtu úložiště

Pak hlavní skript se spustí <em>New-AzureStorage.ps1</em> skriptu, určení "<em>&lt;zadaným hodnotám websitename&gt;</em>úložiště" pro název účtu úložiště a stejné datové centrum jako umístění webové aplikace.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*Nové AzureStorage.ps1* volání `New-AzureStorageAccount` rutina pro vytvoření účtu úložiště a vrátí účet klíčové hodnoty názvu a přístup. Aplikace tyto hodnoty budete potřebovat pro přístup k objektům BLOB a fronty v účtu úložiště.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Vždy nebudete chtít vytvořit nový účet úložiště; skript může vylepšit tak, že přidáte parametr, který volitelně přesměruje ho na použití existující účet úložiště.

### <a name="create-the-databases"></a>Vytvoření databází

Pak spustí skript vytvoření databáze, hlavního skriptu *New-AzureSql.ps1*, po nastavení výchozí databáze a názvy pravidel brány firewall:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

Skript vytvoření databáze načte vývojového počítače IP adresu a nastaví pravidlo brány firewall, tak vývojovém počítači můžete připojit k a správě serveru. Vytvoření skriptu databáze dále prochází kroky k několika kroky k nastavení databáze:

- Vytvoří na serveru s použitím `New-AzureSqlDatabaseServer` rutiny.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Vytvoří pravidla brány firewall pro povolení vývojovém počítači ke správě serveru a k tomu, aby webové aplikace k němu připojit. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Vytvoří kontext databáze, která obsahuje název serveru a přihlašovací údaje, pomocí `New-AzureSqlDatabaseServerContext` rutiny.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` je funkce ve skriptu, který volá `ConvertTo-SecureString` rutiny můžete šifrovat hesla a vrátí `PSCredential` objektu stejného typu, který `Get-Credential` rutina vrátí.
- Vytvoří aplikační databázi a databázi členství pomocí `New-AzureSqlDatabase` rutiny.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Volá místně definované funkce tocreates připojovací řetězec pro každou databázi. Aplikace bude používat tyto připojovací řetězce pro přístup k databázím. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString je funkci definovanou ve skriptu, který vytváří připojovací řetězec z hodnoty parametrů zadat do něj.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Vrátí zatřiďovací tabulku s název databázového serveru a připojovací řetězce.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

Aplikace Fix It používá samostatné členství a aplikačních databází. Je také možné umístit data členství a aplikace v jedné databázi.

### <a name="store-app-settings-and-connection-strings"></a>Nastavení aplikací pro Store a připojovací řetězce

Azure nabízí funkce, která umožňuje ukládání nastavení a připojovací řetězce, které automaticky přepsat, co se vrátí do aplikace při pokusu o čtení `appSettings` nebo `connectionStrings` kolekcí v souboru Web.config. Jedná se o alternativu k použití [transformace Web.config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) při nasazení. Další informace najdete v tématu [Store citlivá data v Azure](source-control.md#appsettings) dále v této e knihy.

Vytvoření skriptu prostředí uloží v Azure všechny `appSettings` a `connectionStrings` hodnoty, které aplikace potřebuje pro přístup k účtu úložiště a databáze při spuštění v Azure.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[Nástroj společnosti New Relic](http://newrelic.com/) je architektura telemetrická data, která vám ukážeme v [monitorování a Telemetrie](monitoring-and-telemetry.md) kapitoly. Vytvoření skriptu prostředí se také restartuje, webové aplikace, abyste měli jistotu, že vybere nastavení New Relic.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Příprava pro nasazení

Na konci procesu vytváření skript prostředí volá dvě funkce k vytvoření souborů, které se použijí skriptem nasazení.

Jeden z těchto funkcí vytvoří profil publikování *(&lt;zadaným hodnotám websitename&gt;.pubxml* souboru). Kód volá rozhraní Azure REST API pro získání nastavení publikování a uloží informace *.publishsettings* souboru. Potom použije informace z tohoto souboru spolu s soubor šablony (*pubxml.template*) Chcete-li vytvořit *.pubxml* soubor, který obsahuje profil publikování. Tento dvoukrokový proces simuluje, co můžete dělat v sadě Visual Studio: Stáhněte si *.publishsettings* souboru a import, který chcete vytvořit profil publikování.

Další funkce používá jiný soubor šablony (Web environment.template) k vytvoření *webu environment.xml* soubor, který obsahuje nastavení, skript nasazení použije spolu s *.pubxml*souboru.

### <a name="troubleshooting-and-error-handling"></a>Řešení potíží a zpracování chyb

Skripty jsou jako programy: volání můžou selhat a kdy to dělají budete chtít vědět co nejvíce o selhání a co způsobilo vypršení jejího. Z tohoto důvodu skriptu pro vytváření prostředí změní hodnotu `VerbosePreference` proměnné z `SilentlyContinue` k `Continue` tak, aby se zobrazují všechny podrobné zprávy. Také změní hodnotu `ErrorActionPreference` proměnné z `Continue` k `Stop`tak, aby skript zastaví, i když se setká s neukončujícími chybami:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Předtím, než ho nemá žádnou práci, ukládá skript tak, aby ho můžete výpočet uplynulého času, po dokončení počáteční čas:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Jakmile se dokončí svou práci, skript zobrazí uplynulý čas:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

A pro všechny klíčové operace skript zapisuje podrobné zprávy, například:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Skript nasazení

Co *AzureWebsiteEnv.ps1 nový* skript provádí pro vytvoření prostředí *publikovat AzureWebsite.ps1* skript provádí pro nasazení aplikace.

Získá název webové aplikace ze skriptu nasazení *webu environment.xml* soubor vytvořený pomocí skriptu pro vytváření prostředí.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Získá heslo uživatele nasazení z *.publishsettings* souboru:

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Je spuštěn [MSBuild](http://msbuildbook.com/) příkaz, který sestaví a nasadí projekt:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

A pokud jste určili `Launch` parametr příkazového řádku, které volá `Show-AzureWebsite` rutiny a otevřete váš výchozí prohlížeč na adresu URL webu.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Skript nasazení můžete spustit pomocí příkazu podobný následujícímu:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

A po dokončení, v prohlížeči se otevře se web spuštěný v cloudu, a `<websitename>.azurewebsites.net` adresy URL.

![Oprava aplikace nasazené na platformě Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>Souhrn

S těmito skripty můžete být jisti, že stejný postup bude vždy provést ve stejném pořadí pomocí stejných možností. To pomáhá zajistit, že každý vývojář v týmu není přijít o něco schválně pokazí něco nebo vlastní na vlastní počítač, nebudou fungovat ve skutečnosti stejným způsobem jako v prostředí jinému členovi týmu nebo v produkčním prostředí něco nasadila.

Podobným způsobem můžete automatizovat většinu služeb Azure funkce správy, které můžou provádět na portálu management portal, pomocí rozhraní REST API, skriptů prostředí Windows PowerShell, jazyk rozhraní .NET API nebo prostředí Bash nástroj, který můžete spustit na Linuxu nebo macu.

V [další kapitolu](source-control.md) vytvoříme podívejte se na zdrojový kód a vysvětlit, proč je důležité zahrnout skripty do vašeho úložiště zdrojového kódu.

## <a name="resources"></a>Prostředky

- [Instalace a konfigurace Windows Powershellu pro Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Vysvětluje, jak nainstalovat nejnovější verzi rutin Azure Powershellu a nainstalovat certifikát, musí v počítači, abyste mohli spravovat Azure započítat i. To je skvělé místo, kde můžete začít pracovat, protože má také odkazy na zdroje informací pro výuku Powershellu samotný.
- [Centrum skriptů Azure](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). Zdroje informací pro vývoj skriptů, které správy služeb Azure, s odkazy na kurzy Začínáme, rutina referenční dokumentaci a zdrojový kód a ukázky skriptů pro portál WindowsAzure.com
- [Tvůrce skriptů víkendu: Začínáme s Azure a Powershellu](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). Tento příspěvek na blogu vyhrazená pro prostředí Windows PowerShell, poskytuje vynikající Úvod do používání Powershellu pro správu Azure functions.
- [Instalace a konfigurace rozhraní příkazového řádku Azure Cross-Platform](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Úvodní kurz pro Azure skriptovací framework, která funguje na Mac a Linux, jakož i Windows systémy.
- [Nástroje příkazového řádku části tématu stáhnout sady Azure SDK a nástroje](https://azure.microsoft.com/downloads/). Stránka portálu dokumentace a soubory ke stažení týkající se nástrojů příkazového řádku Azure.
- [Automatizace všechno, co s knihovnami správy Azure a .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman přináší rozhraní .NET API pro správu Azure.
- [Pomocí skriptů Windows Powershellu k publikování do vývojových a testovacích prostředí](https://msdn.microsoft.com/library/azure/dn642480.aspx). Dokumentace MSDN, který vysvětluje, jak použít publikování skripty, které aplikace Visual Studio generuje pro webové projekty.
- [Nástroje PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Visual Studio rozšíření, které přidává podporu jazyka pro prostředí Windows PowerShell v sadě Visual Studio.

> [!div class="step-by-step"]
> [Předchozí](introduction.md)
> [další](source-control.md)
