---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: "Automatizovat vše (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs"
author: MikeWasson
description: "Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: cf1cb7b07ffe8750724e58e4fb66854c9a033a54
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatizovat vše (vytváření reálných cloudových aplikací s Azure)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud. Úvod do e adresáře, najdete v části [první kapitoly](introduction.md).


Prvních tří vzorů, které podíváme ve skutečnosti použít k žádným projektem vývoj softwaru, ale hlavně pro projekty cloudových. Tento vzor je o automatizaci úloh vývoj. Je důležité tématu, protože manuální procesy pomalé a náchylný; automatizace, kolik z nich možné pomáhá nastavit rychlé, spolehlivé a agilní pracovní postup. Je jednoznačně důležité pro vývoj cloudové protože snadno můžete automatizovat řadu úkolů, které je obtížné nebo dokonce znemožňují k automatizaci v místním prostředí. Například můžete nastavit celou testovací prostředí, včetně nového webového serveru a back-end virtuálních počítačů, databází, blob storage (úložiště souborů), fronty, atd.

## <a name="devops-workflow"></a>Pracovní postup DevOps

Stále uslyšíte termín "DevOps." Termín vyvinuté mimo rozpoznávání, budete muset integrovat vývoj a provoz úloh a vývoji softwaru efektivně. Druh pracovního postupu, který chcete povolit je taková, ve kterém můžete vyvíjet aplikace, nasazení, dozvědět se od produkční použití ho, změnit v reakci na co když jste se naučili a opakujte cyklus rychle a spolehlivě.

Některé vývojové týmy úspěšné cloudu nasadit několikrát za den za provozu prostředí. Tým Azure použít k nasazení hlavní každé 2 – 3 měsíce, ale nyní jej aktualizovat verze dílčími aktualizacemi každé 2 až 3 dny a hlavní verze každé 2 až 3 týdny. Získávání do této cadence skutečně vám pomůže se reaguje na názory zákazníků.

Aby bylo možné provést, je třeba povolit cyklus vývoj a nasazení, který je opakovatelných, spolehlivé, předvídatelný a nízkou cyklus doby.

![Pracovní postup DevOps](automate-everything/_static/image1.png)

Doba mezi Pokud máte představu pro funkci a když jsou zákazníci použití a poskytování zpětné vazby jinými slovy, musí být co nejkratší. První tři vzory – automatizovat vše, Správa zdrojového kódu a průběžnou integraci a doručení – jsou všechny informace o doporučených postupech, které doporučujeme, aby bylo možné povolit tento druh procesu.

## <a name="azure-management-scripts"></a>Skripty pro správu Azure

V [Úvod do této e knihy](introduction.md), jste viděli webové konzoly, portálu pro správu Azure. Portálu pro správu umožňuje sledovat a spravovat všechny prostředky, které jste nasadili v Azure. Je snadný způsob, jak vytvořit a odstranění služeb jako třeba webové aplikace a virtuálních počítačů, tyto služby konfigurovat, sledovat operace služby a tak dále. Je skvělý nástroj, ale jeho použití je ruční proces. Pokud budete vyvíjet produkční aplikace libovolnou velikost, a hlavně v prostředí team, doporučujeme, aby přejděte prostřednictvím portálu uživatelského rozhraní, chcete-li další informace a seznamte se s Azure a poté automatizaci procesů, které jste budete by opakovaně.

Téměř vše, co můžete udělat ručně v portálu pro správu nebo ze sady Visual Studio můžete také provést volání rozhraní API REST správy. Můžete napsat skripty s použitím [prostředí Windows PowerShell](https://msdn.microsoft.com/en-us/library/windowsazure/jj156055.aspx), nebo můžete použít otevřeným zdrojem framework [Chef](http://www.opscode.com/chef/) nebo [Puppet](http://puppetlabs.com/puppet/what-is-puppet). Můžete také použít nástroj příkazového řádku Bash v prostředí Mac nebo Linux. Azure má skriptování rozhraní API pro tyto různých prostředích, která je [rozhraní API pro správu .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) v případě, že chcete napsat kód místo skriptu.

Aplikaci, opravte ji vytvořili jsme některé skripty prostředí Windows PowerShell, které automatizují procesy vytváření testovacího prostředí a nasazení projektu do prostředí a přečtěte část obsah těchto skriptů.

## <a name="environment-creation-script"></a>Vytvoření skriptu prostředí

Je název první skriptu podíváme *New-AzureWebsiteEnv.ps1*. Vytvoří prostředí Azure, můžete nasadit opravte ji aplikaci pro testování. Hlavní úlohy, které tento skript provede jsou následující:

- Vytvoření webové aplikace.
- Vytvoření účtu úložiště. (Požadováno pro objekty BLOB a fronty, jak uvidíte v dalších kapitolách.)
- Vytvoření databáze SQL serveru a dvě databáze: aplikační databáze a databáze členství.
- Ukládání nastavení v Azure, která aplikace bude používat pro přístup k účtu úložiště a databáze.
- Vytvořte soubory nastavení, které se použijí k automatizaci nasazení.

### <a name="run-the-script"></a>Spusťte skript


> [!NOTE]
> Tato část kapitoly jsou uvedeny příklady skripty a příkazy, které zadáte, aby bylo možné spustit. Tuto ukázku a neposkytuje všechno, co potřebujete vědět, aby bylo možné spustit skripty. Podrobné pokyny how-k-proveďte it, najdete v [příloha: Opravte ji ukázka Application](the-fix-it-sample-application.md#deploybase).


Chcete-li spustit skript prostředí PowerShell, který spravuje služby Azure, budete muset nainstalovat konzoly Azure PowerShell a nakonfigurovat ho na spolupráci s předplatným Azure. Po nastavení, můžete spustit opravte ji prostředí vytvoření skriptu pomocí příkazu podobné následujícímu:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

`Name` Parametr určuje název, který se má použít při vytváření databáze a úložiště účtů a `SqlDatabasePassword` parametr určuje heslo pro účet správce, který se vytvoří pro databázi SQL. Existují další parametry, které můžete použít, podíváme později.

![Okno prostředí PowerShell](automate-everything/_static/image2.png)

Po dokončení skriptu uvidíte na portálu management portal, co byl vytvořen. Zjistíte dvě databáze:

![Databáze](automate-everything/_static/image3.png)

Účet úložiště:

![Účet úložiště](automate-everything/_static/image4.png)

A webové aplikace:

![Webový server](automate-everything/_static/image5.png)

Na **konfigurace** kartě pro webovou aplikaci, uvidíte, že má nastavení účtu úložiště a připojovací řetězce SQL databáze nastavení pro opravu jeho aplikace.

![appSettings a connectionStrings](automate-everything/_static/image6.png)

*Automatizace* složka teď také obsahuje  *&lt;zadaným hodnotám websitename&gt;.pubxml* souboru. Tento soubor uchovává nastavení, která MSBuild použijete k nasazení aplikace do prostředí Azure, kterou jste právě vytvořili. Příklad:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Jak vidíte, skript vytvořil dokončení testovacího prostředí a celý proces se provádí v přibližně 90 sekund.

Pokud někdo na váš tým chce vytvořit testovací prostředí, mohou pouze spouštět skript. Jenom je rychlý, ale také může být jistý, že používají stejný jako ten, který používáte prostředí. Nepodařilo se poměrně jako jisti této, pokud všichni byla nastavení věcí ručně pomocí portálu pro správu uživatelského rozhraní.

### <a name="a-look-at-the-scripts"></a>Podívejte se na tyto skripty

Existují ve skutečnosti tři skripty, které tuto práci. Můžete volat z příkazového řádku a automaticky použije další dvě udělat některé úlohy:

- *Nové AzureWebSiteEnv.ps1* je hlavního skriptu.

    - *Nové AzureStorage.ps1* vytvoří účet úložiště.
    - *Nové AzureSql.ps1* vytvoří databáze.

### <a name="parameters-in-the-main-script"></a>Parametry v hlavního skriptu

Hlavní skriptu *New-AzureWebSiteEnv.ps1*, definuje několik parametrů:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Jsou vyžadovány dva parametry:

- Název webové aplikace, kterou vytvoří skript. (Používá se také pro adresu URL: `<name>.azurewebsites.net`.)
- Heslo pro nového správce databáze serveru, který vytvoří skript.

Volitelné parametry umožňují určit umístění center dat (výchozí hodnota je "Západní USA"), správce název databázového serveru (výchozí hodnota je "dbuser") a pravidla brány firewall pro server databáze.

### <a name="create-the-web-app"></a>Vytvoření webové aplikace

První věc skript nemá, je vytvoření webové aplikace pomocí volání `New-AzureWebsite` rutiny předávání v k němu webovou aplikaci název a umístění hodnoty parametrů:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Vytvořit účet úložiště

Potom hlavní skript se spustí *New-AzureStorage.ps1* skriptu, zadání "*&lt;zadaným hodnotám websitename&gt;*úložiště" pro název účtu úložiště a stejná data center umístění jako webové aplikace.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*Nové AzureStorage.ps1* volání `New-AzureStorageAccount` vytvořte účet úložiště a vrátí účet názvem a přístupovým klíčové hodnoty. Aplikace bude nutné tyto hodnoty pro přístup k objektům BLOB a fronty v účtu úložiště.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Nemusí se vždy chcete vytvořit nový účet úložiště; skript může zvýšit tak, že přidáte parametr, který volitelně přesměruje ho na použití existující účet úložiště.

### <a name="create-the-databases"></a>Vytvoření databáze

Hlavního skriptu pak spustí skript vytvoření databáze, *New-AzureSql.ps1*, po nastavení výchozí databáze a názvy pravidel brány firewall:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

Vytvoření databázového skriptu načte vývojářském počítači IP adresu a nastaví pravidlo brány firewall tak, aby vývojářském počítači můžete připojit k a správě serveru. Vytvoření databázového skriptu pak prochází nastavte databáze několik kroků:

- Vytvoří server pomocí `New-AzureSqlDatabaseServer` rutiny.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Vytvoří pravidla brány firewall pro povolení vývojářském počítači ke správě serveru a umožňuje připojení k tomuto webové aplikace. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Vytvoří kontext databáze, která obsahuje název serveru a přihlašovací údaje, pomocí `New-AzureSqlDatabaseServerContext` rutiny.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText`je funkce ve skriptu, který volá `ConvertTo-SecureString` rutiny k šifrování hesla a vrátí `PSCredential` objektu, stejný typ, který `Get-Credential` rutina vrátí.
- Vytvoří aplikační databázi a databázi členství pomocí `New-AzureSqlDatabase` rutiny.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Volá tocreates místně definované funkce připojovací řetězec pro každou databázi. Aplikace bude používat tyto připojovací řetězce pro přístup k databázím. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString je funkci definovanou v skript, který vytvoří připojovací řetězec z hodnoty parametrů použité k němu.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Vrátí tabulku hash se název databázového serveru a připojovací řetězce.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

Aplikace opravte ji používá samostatné členství a databáze aplikace. Je také možné uvést dat členství a aplikací do jedné databáze.

### <a name="store-app-settings-and-connection-strings"></a>Ukládání nastavení aplikace a připojovacích řetězců

Azure nabízí funkce, která vám umožní uložit nastavení a připojovací řetězce, které automaticky přepsání, které jsou vraceny aplikace při pokusu o čtení `appSettings` nebo `connectionStrings` kolekcí v souboru Web.config. Jde o alternativu k použití [transformace Web.config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) při nasazení. Další informace najdete v tématu [ukládat citlivá data v Azure](source-control.md#appsettings) dál v této příručce e.

Vytvoření skriptu prostředí ukládá v Azure všechny `appSettings` a `connectionStrings` hodnoty, které aplikace potřebuje přístup k účtu úložiště a databáze při spuštění v Azure.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/) telemetrie rozhraní, které jsme předvedení v [monitorování a Telemetrie](monitoring-and-telemetry.md) kapitoly. Skript pro vytvoření prostředí se také restartuje, webové aplikace, abyste měli jistotu, že vybere nastavení New Relic.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Příprava pro nasazení

Na konci procesu vytvoření skriptu prostředí volá dvě funkce postup vytvoření souborů, které se použijí skript nasazení.

Jednu z těchto funkcí vytvoří profil publikování *(&lt;zadaným hodnotám websitename&gt;.pubxml* souboru). Kód zavolá rozhraní API REST Azure pro získání nastavení publikování a ukládá informace v *.publishsettings* souboru. Pak používá informace z tohoto souboru spolu s souboru šablony (*pubxml.template*) k vytvoření *.pubxml* soubor, který obsahuje profil publikování. Tento dvoukrokový proces simuluje, co se děje v sadě Visual Studio: stažení *.publishsettings* souboru a import, který chcete vytvořit profil publikování.

Další funkce používá jiný soubor šablony (Web environment.template) k vytvoření *webu environment.xml* soubor, který obsahuje nastavení skript nasazení použije spolu s *.pubxml*souboru.

### <a name="troubleshooting-and-error-handling"></a>Řešení potíží a zpracování chyb

Skripty se podobně jako programy: můžou selhat a pokud se tak chcete vědět, co nejvíce můžete o selhání, a zda ji způsobil. Z toho důvodu vytvoření skriptu prostředí změní hodnotu `VerbosePreference` proměnnou z `SilentlyContinue` k `Continue` tak, aby všechny podrobné zprávy se zobrazují. Také změní hodnotu `ErrorActionPreference` proměnnou z `Continue` k `Stop`tak, aby skript zastaví, i když zjistí neukončující chyby:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Před jeho veškerou práci, ukládá skript tak, aby ho můžete vypočítat uplynulý čas, pokud se provádí čas spuštění:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Až se dokončí svojí práci, skript zobrazí uplynulý čas:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

A pro každé klíčové operace skript zapisuje podrobné zprávy, například:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Skript nasazení

Co *New-AzureWebsiteEnv.ps1* nemá skript pro vytvoření prostředí, *publikovat AzureWebsite.ps1* nemá skriptu pro nasazení aplikace.

Skript nasazení získá název webové aplikace z *webu environment.xml* soubor vytvořený pomocí skriptu pro vytváření prostředí.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Získá heslo uživatele nasazení z *.publishsettings* souboru:

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Se provede [MSBuild](http://msbuildbook.com/) příkaz, který vytvoří a nasadí projekt:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

A pokud jste určili `Launch` parametr na příkazovém řádku, zavolá `Show-AzureWebsite` rutiny otevře výchozí prohlížeč na adresu URL webu.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Skript nasazení můžete spustit pomocí příkazu podobné následujícímu:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

A až skončíte, prohlížeči se otevře s web spuštěný v cloudu v `<websitename>.azurewebsites.net` adresy URL.

![Automatická oprava aplikace nasazené do systému Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>Souhrn

Pomocí těchto skriptů máte jistotu, že se shoduje s kroky bude vždy provést ve stejném pořadí pomocí stejných možností. To pomáhá zajistit, že každý vývojář v týmu není neproběhly něco nebo zřeteli něco a nasazení něco vlastní na vlastním počítači, který nebude fungovat ve skutečnosti stejným způsobem jako v prostředí druhý člen seskupení, nebo v produkčním prostředí.

Podobným způsobem můžete automatizovat nejvíce Azure funkce správy, které můžete provést v portálu pro správu pomocí rozhraní REST API, skriptů prostředí Windows PowerShell, jazyk rozhraní .NET API nebo Bash nástroj, který můžete spustit na platformě Linux nebo Mac.

V [další kapitoly](source-control.md) jsme budete podívejte se na zdrojový kód a vysvětlit, proč je důležité zahrnout skripty do vašeho úložiště zdrojového kódu.

## <a name="resources"></a>Prostředky

- [Instalace a konfigurace prostředí Windows PowerShell pro Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Popisuje postup instalace rutin prostředí Azure PowerShell a jak nainstalovat certifikát, zda je nutné v počítači, abyste mohli spravovat vaše Azure účtu. Toto je skvělým místem začít pracovat, protože má také odkazy na zdroje informací prostředí PowerShell, sám sebe.
- [Centra skriptů Azure](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). Portál WindowsAzure.com prostředky pro vývoj skriptů, které spravují služby Azure, s odkazy na získávání kurzy Začínáme, rutina referenční dokumentaci a zdrojový kód a ukázkové skripty
- [Tvůrce skriptů víkendu: Začínáme s Azure a prostředí PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). V blogu vyhrazený pro prostředí Windows PowerShell poskytuje tento příspěvek Skvělý úvod pomocí prostředí PowerShell pro Azure správy funkcí.
- [Instalace a konfigurace rozhraní příkazového řádku Azure napříč platformami](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Úvodní kurz pro Azure skriptování framework, který funguje na Mac a Linux, jakož i Windows systémy.
- [Nástroje příkazového řádku části tématu stáhnout sady Azure SDK a nástroje](https://azure.microsoft.com/downloads/). Stránka portálu dokumentace a produkty ke stažení související s nástroje příkazového řádku pro Azure.
- [Automatizace všechno, co se knihovny správy Azure a rozhraní .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman zavádí .NET správy rozhraní API pro Azure.
- [Pomocí skriptů prostředí PowerShell systému Windows k publikování pro vývojáře a testovací prostředí](https://msdn.microsoft.com/library/azure/dn642480.aspx). Dokumentace MSDN, která vysvětluje, jak používat publikovat skripty, které automaticky generuje sada Visual Studio pro webové projekty.
- [Prostředí PowerShell nástroje pro sadu Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Visual Studio rozšíření, která přidá podporu pro prostředí Windows PowerShell v sadě Visual Studio.

>[!div class="step-by-step"]
[Předchozí](introduction.md)
[další](source-control.md)
