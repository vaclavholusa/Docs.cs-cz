---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Spuštěné skripty prostředí PowerShell systému Windows ze souborů projektu nástroje MSBuild | Microsoft Docs
author: jrjlee
description: Toto téma popisuje, jak spustit skript prostředí Windows PowerShell jako součást procesu sestavení a nasazení. Můžete spustit skript místně (jinými slovy, na b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: c8ef22cfbba7b3b85944ea4c49f3183e5a6aafbb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890356"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Spuštěné skripty prostředí PowerShell systému Windows ze souborů projektu nástroje MSBuild
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak spustit skript prostředí Windows PowerShell jako součást procesu sestavení a nasazení. Můžete spustit skript místně (jinými slovy, na serveru sestavení) nebo vzdáleně, jako na cílový webový server nebo serveru databáze.
> 
> Existuje mnoho důvodů, proč můžete chtít spustit skript prostředí Windows PowerShell po nasazení. Například můžete chtít:
> 
> - Přidání vlastních událostí zdroje do registru.
> - Generovat adresář systému souborů pro nahrávání.
> - Vyčištění sestavení adresáře.
> - Položky k zápisu do vlastního souboru protokolu.
> - Odesílání e-mailů pozvat uživatele na nově zřízeného webové aplikace.
> - Vytvořte uživatelské účty s příslušnými oprávněními.
> - Konfiguraci replikace mezi instancí systému SQL Server.
> 
> Toto téma vám ukáže, jak ke spouštění skriptů prostředí Windows PowerShell místně i vzdáleně z vlastní cíl v souboru projektu Microsoft Build Engine (MSBuild).


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz řady používá ukázkové řešení&#x2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3, komunikaci Windows Služba Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízené procesu sestavení dva soubory projektu&#x2014;jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

Pokud chcete spustit skript prostředí Windows PowerShell jako součást procesu nasazení automatizované nebo jeden krok, budete potřebovat k dokončení těchto úloh vysoké úrovně:

- Přidejte skript prostředí Windows PowerShell pro vaše řešení a k řízení zdrojů.
- Vytvořte příkaz, který vyvolává skript prostředí Windows PowerShell.
- Escape všechny vyhrazené znaky jazyka XML v příkazu.
- Vytvořit cíl ve vaší vlastní soubor projektu nástroje MSBuild a použít **Exec** úloha pro spuštění příkazu.

Toto téma vám ukáže, jak k provedení těchto postupů. Postupy v tomto tématu a úlohy předpokládají, že jste již obeznámeni s cíle MSBuild a vlastnosti a víte, jak používat vlastní soubor projektu nástroje MSBuild k řízení procesem sestavení a nasazení. Další informace najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Vytvoření a přidání skriptů prostředí PowerShell systému Windows

Úkoly v tomto tématu použijte ukázkový skript prostředí Windows PowerShell s názvem **LogDeploy.ps1** k ukazují, jak spouštět skripty z nástroje MSBuild. **LogDeploy.ps1** skript obsahuje jednoduché funkce, který zapíše položku jeden řádek do souboru protokolu:


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


**LogDeploy.ps1** skriptu přijímá dva parametry. První parametr představuje úplnou cestu k souboru protokolu, do které chcete přidat položku a druhý parametr představuje cíl nasazení, který chcete záznam v souboru protokolu. Když spustíte skript, přidá řádek do souboru protokolu v tomto formátu:


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


Chcete-li **LogDeploy.ps1** skript k dispozici pro MSBuild, budete muset:

- Přidejte skript do správy zdrojového kódu.
- Přidejte skript do řešení v sadě Visual Studio 2010.

Nemusíte nasazovat skriptu pomocí řešení obsah, bez ohledu na to, jestli chcete spusťte tento skript na serveru sestavení nebo na vzdáleném počítači. Jednou z možností je přidat skript do složky řešení. V příkladu obraťte se na správce vzhledem k tomu, že chcete použít skript prostředí Windows PowerShell jako součást procesu nasazení, má smysl přidat skript do složky publikovat řešení.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Obsah složky řešení se zkopírují do sestavení servery jako zdroj materiálu. Tvoří ale žádná z částí žádný výstup projektu.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Provádění skriptu prostředí Windows PowerShell na serveru sestavení

V některých případech můžete spouštět skripty Windows PowerShell v počítači, který sestaví vašich projektů. Můžete například použít skript prostředí Windows PowerShell k vyčištění sestavení složek nebo o zapsání položky do vlastního souboru protokolu.

Z hlediska syntaxe z soubor projektu nástroje MSBuild spustíte skript prostředí Windows PowerShell je stejný jako regulární příkazovém řádku spustíte skript prostředí Windows PowerShell. Je nutné vyvolat spustitelný soubor powershell.exe a použít **– příkaz** přepínač tak, aby poskytují příkazy, které chcete spustit prostředí Windows PowerShell. (V prostředí Windows PowerShell v2, můžete také použít **– soubor** přepínače). Příkaz měla mít tento formát:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


Příklad:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


Pokud cesta k váš skript obsahuje mezery, budete muset uzavřete cestu k souboru v jednoduchých uvozovkách ampersand před sebou. Dvojité uvozovky, nemůžete použít, protože jste je již použit pro uzavřete příkaz:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


Existuje několik další aspekty při vyvolání tento příkaz z nástroje MSBuild. Nejdřív by měla obsahovat **– NonInteractive** příznak, ujistěte se, zda se skript spustí tiše. V dalším kroku by měla obsahovat **– ExecutionPolicy** příznak s hodnotou odpovídající argument. Určuje zásady spouštění, použije se pro váš skript prostředí Windows PowerShell a umožňuje potlačit výchozí zásady spouštění, která může zabránit spuštění skriptu. Můžete vybrat z těchto hodnot argument:

- Hodnota **bez omezení** vám umožní spuštění skriptu, bez ohledu na to, jestli je podepsaný skript prostředí Windows PowerShell.
- Hodnota **RemoteSigned** bude možné spustit nepodepsané skripty, které byly vytvořeny v místním počítači prostředí Windows PowerShell. Ale musí být podepsané skripty, které byly vytvořeny jinde. (V praxi, jste velmi pravděpodobně jste vytvořili skript prostředí Windows PowerShell místně na serveru sestavení).
- Hodnota **AllSigned** vám umožní provést pouze podepsaných skriptů prostředí Windows PowerShell.

Výchozí zásada spouštění je **s omezeným přístupem**, který zabrání spuštění žádné soubory skriptu prostředí Windows PowerShell.

Nakonec je třeba abyste se vyhnuli vyhrazené znaky jazyka XML, které se vyskytují v příkazu prostředí Windows PowerShell:

- Nahrazení jednoduchých uvozovek a být s  **&amp;apos;**
- Nahraďte dvojité uvozovky se  **&amp;potřebná;**
- Nahraďte ampersandy s  **&amp;amp;**

- Pokud provedete tyto změny, bude váš příkaz vypadat takto:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


V rámci vaší vlastní soubor projektu nástroje MSBuild, můžete vytvořit nový cíl a použít **Exec** úlohy ke spuštění tohoto příkazu:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


V tomto příkladu Všimněte si, že:

- Všechny proměnné, jako jsou hodnoty parametrů a umístění spustitelného souboru prostředí Windows PowerShell, jsou deklarovány jako vlastnosti nástroje MSBuild.
- Podmínky jsou zahrnuty umožníte uživatelům přepsat tyto hodnoty z příkazového řádku.
- **MSDeployComputerName** vlastnost je deklarovaná jinde v souboru projektu.

Pokud tento cíl je spuštěn jako součást procesu sestavení, spustí příkazu prostředí Windows PowerShell a zapisovat položky protokolu do souboru, který jste zadali.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Provádění skriptu prostředí Windows PowerShell ve vzdáleném počítači

Prostředí Windows PowerShell je schopen spuštění skriptů na vzdálených počítačích prostřednictvím [Vzdálená správa systému Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). K tomuto účelu, budete muset použít [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) rutiny. Díky tomu můžete spustit skript na jeden nebo více vzdálených počítačů bez kopírování skript do vzdálených počítačů. Výsledky jsou vráceny do místního počítače, z níž jste spustili skript.

> [!NOTE]
> Před použitím **Invoke-Command** rutiny prostředí Windows PowerShell spuštění skriptů ve vzdáleném počítači, je nutné nakonfigurovat naslouchací proces služby WinRM tak, aby přijímal vzdálená zprávy. To provedete spuštěním příkazu **rychlou konfiguraci winrm** na vzdáleném počítači. Další informace najdete v tématu [instalace a konfigurace pro vzdálenou správu systému Windows](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).


V okně prostředí Windows PowerShell by tuto syntaxi používají ke spuštění **LogDeploy.ps1** skriptu ve vzdáleném počítači:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> Existují různé způsoby použití **Invoke-Command** spuštění skriptu souboru, ale tento přístup je nejvíc přímočarý když potřebujete zadat hodnoty parametrů a spravovat cesty s mezerami.


Když to spustíte z příkazového řádku, budete muset vyvolat spustitelný soubor prostředí Windows PowerShell a použít **– příkaz** parametru vytvořit pokyny:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


Jako před, je potřeba zadat některé další přepínače a při spuštění příkazu z nástroje MSBuild vyhnuli všechny vyhrazené znaky jazyka XML:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


Nakonec jako dříve, můžete použít **Exec** úloh v rámci vlastní cíle MSBuild k provedení příkazu:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


Pokud tento cíl je spuštěn jako součást procesu sestavení, prostředí Windows PowerShell se spustí skript na počítači zadaném v **– computername** argument.

## <a name="conclusion"></a>Závěr

Toto téma popisuje, jak spustit skript prostředí Windows PowerShell ze souboru projektu nástroje MSBuild. Tento postup můžete použít ke spuštění skriptu prostředí Windows PowerShell buď místně nebo ve vzdáleném počítači, v rámci procesu automatické nebo krokování sestavení a nasazení.

## <a name="further-reading"></a>Další čtení

Informace o podepisování skriptů prostředí Windows PowerShell a správě zásad spouštění, najdete v části [spuštění skriptů prostředí PowerShell systému Windows](https://technet.microsoft.com/library/ee176949.aspx). Informace o spouštění příkazů prostředí Windows PowerShell ze vzdáleného počítače, najdete v části [spuštění vzdálené příkazy](https://technet.microsoft.com/library/dd819505.aspx).

Další informace o používání vlastních souborů projektu nástroje MSBuild k řízení procesu nasazení najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Předchozí](taking-web-applications-offline-with-web-deploy.md)
> [další](troubleshooting-the-packaging-process.md)
