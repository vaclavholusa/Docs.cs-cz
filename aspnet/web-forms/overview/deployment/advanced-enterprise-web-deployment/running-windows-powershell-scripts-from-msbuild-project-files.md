---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Spouštění skriptů Windows Powershellu ze souborů projektu MSBuild | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak spustit skript prostředí Windows PowerShell jako součást procesu sestavení a nasazení. Skript můžete spustit místně (jinými slovy na b...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: 018a962c3bac774a770b83b2fd1f44f72b6f5b09
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752602"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Spouštění skriptů Windows Powershellu ze souborů projektu MSBuild
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak spustit skript prostředí Windows PowerShell jako součást procesu sestavení a nasazení. Můžete spustit skript místně (jinými slovy, na serveru sestavení) nebo vzdáleně, třeba na cílový webový server nebo databázový server.
> 
> Existuje mnoho důvodů, proč můžete chtít spustit skript prostředí Windows PowerShell po nasazení. Například můžete chtít:
> 
> - Přidání zdroje vlastní událost do registru.
> - Generovat adresář systému souborů pro nahrávání.
> - Čištění adresáře sestavení.
> - Zápis položky do vlastního souboru protokolu.
> - Odesílání e-mailů pozvání uživatelů do nově zřízeného webové aplikace.
> - Vytvořte uživatelské účty s příslušnými oprávněními.
> - Konfigurace replikace mezi instancí systému SQL Server.
> 
> Toto téma se zobrazí spuštění skriptů Windows Powershellu z vlastního cíle v souboru projektu Microsoft Build Engine (MSBuild) místně i vzdáleně.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

Chcete-li spustit skript prostředí Windows PowerShell jako součást procesu krokování nebo automatizované nasazení, budete potřebovat k dokončení těchto úloh:

- Přidáte skript prostředí Windows PowerShell do svého řešení a do správy zdrojového kódu.
- Vytvořte příkaz, který vyvolává skript prostředí Windows PowerShell.
- Řídicí vyhrazené znaky XML ve svých rukou.
- Vytvořit cíl v souboru projektu MSBuild vlastní a použít **Exec** úkolů ke spuštění příkazu.

Toto téma se ukazují, jak provést tyto postupy. Úlohy a názorné postupy v tomto tématu se předpokládá, že jste již obeznámeni s MSBuild cíle a vlastnosti, a že vám pochopit, jak můžete použít vlastní soubor projektu MSBuild k řízení procesu sestavení a nasazení. Další informace najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Vytváření a přidávání skriptů Windows Powershellu

Úlohy v tomto tématu použijte ukázkový skript prostředí Windows PowerShell s názvem **LogDeploy.ps1** si ukážeme, jak spouštět skripty MSBuild. **LogDeploy.ps1** skript obsahuje jednoduchou funkci, která zapíše položku jedním řádkem do souboru protokolu:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]


**LogDeploy.ps1** skript přijímá dva parametry. První parametr představuje úplnou cestu k souboru protokolu, do které chcete přidat položku, a druhý parametr představuje cíl nasazení, který chcete nahrát do souboru protokolu. Když spustíte skript, přidá nový řádek do souboru protokolu v tomto formátu:


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


Chcete-li **LogDeploy.ps1** skript k dispozici pro MSBuild, budete muset:

- Přidáte skript do správy zdrojového kódu.
- Přidáte skript do svého řešení v sadě Visual Studio 2010.

Není nutné k nasazení skriptu s obsahem vašeho řešení, bez ohledu na to, zda máte v plánu spuštění skriptu na serveru sestavení nebo ve vzdáleném počítači. Jednou z možností je přidat skript do složky řešení. V příkladu správce kontaktů vzhledem k tomu, že chcete použít skript prostředí Windows PowerShell jako součást procesu nasazení je vhodné přidat skript do složky řešení publikovat.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Obsah složky řešení se zkopírují na servery sestavení jako zdrojový materiál. Nicméně jsou součástí žádné žádný výstup projektu.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Provádění skriptu prostředí Windows PowerShell na serveru sestavení

V některých případech můžete chtít spustit skripty Windows Powershellu na počítači, který sestaví vaše projekty. Například můžete použít skript prostředí Windows PowerShell pro vyčištění sestavení složky nebo zápis položky do vlastního souboru protokolu.

Z hlediska syntaxi skriptu Windows Powershellu ze souboru projektu MSBuild je stejný jako regulární příkazovém řádku spustíte skript prostředí Windows PowerShell. Je nutné vyvolat powershell.exe spustitelný soubor a použít **– příkaz** přepínač k poskytování příkazů Windows Powershellu, aby fungoval. (V prostředí Windows PowerShell verze 2, můžete také použít **– soubor** přepínače). Příkaz by měl provést tento formát:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


Příklad:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


Pokud cesta ke skriptu obsahuje mezery, budete muset uzavřít cesta k souboru v jednoduchých uvozovkách předchází znak ampersand. Dvojité uvozovky, nemůžete použít, protože jste už použili k uzavření příkazu:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


Existuje několik dalších důležitých informací při vyvolání tento příkaz MSBuild. Nejprve, měli byste zahrnout **– NonInteractive** příznak Ujistěte se, že se skript spustí tiše. V dalším kroku, měli byste zahrnout **– ExecutionPolicy** příznak s hodnotou odpovídající argument. Toto nastavení určuje zásady spouštění, že bude vztahovat na váš skript prostředí Windows PowerShell a umožňuje potlačit výchozí zásadu spouštění, což může zabránit spuštění skriptu. Můžete vybrat z těchto argumentů:

- Hodnota **Unrestricted** vám umožní prostředí Windows PowerShell pro spuštění vašeho skriptu, bez ohledu na to, jestli je skript podepsán.
- Hodnota **RemoteSigned** vám umožní prostředí Windows PowerShell ke spouštění nepodepsaných skriptů, které byly vytvořeny na místním počítači. Však musí být podepsané skripty, které byly vytvořeny jinde. (V praxi, jste velmi nepravděpodobné, že jste vytvořili skript prostředí Windows PowerShell místně na serveru sestavení).
- Hodnota **AllSigned** vám umožní spustit jenom podepsané skripty Windows Powershellu.

Je výchozí zásadu spouštění **s omezeným přístupem**, která zabrání spuštění jakékoli soubory skriptů prostředí Windows PowerShell.

Nakonec musíte řídicí vyhrazené znaky XML, ke kterým dochází v příkazu Windows Powershellu:

- Nahraďte jednoduchých uvozovek a být s  **&amp;apos;**
- Nahraďte dvojité uvozovky se  **&amp;quot;**
- Nahraďte tyto znaky s  **&amp;amp;**

- Pokud provedete tyto změny, bude příkaz vypadat takto:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


V rámci vlastního souboru projektu MSBuild, můžete vytvořit nový cíl a použít **Exec** úkolů ke spuštění tohoto příkazu:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


V tomto příkladu Všimněte si, že:

- Všechny proměnné, jako jsou hodnoty parametrů a umístění ke spustitelnému souboru prostředí Windows PowerShell jsou deklarovány jako vlastnosti nástroje MSBuild.
- Podmínky jsou zahrnuty umožňující uživatelům přepsat tyto hodnoty z příkazového řádku.
- **MSDeployComputerName** vlastnost je deklarován jinde v souboru projektu.

Při spouštění tohoto cíle jako součást procesu sestavení spustí příkazu prostředí Windows PowerShell a zapisovat položky protokolu do souboru, který jste zadali.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Provádění skriptu prostředí Windows PowerShell ve vzdáleném počítači

Prostředí Windows PowerShell je schopen spuštění skriptů ve vzdálených počítačích prostřednictvím [Vzdálená správa Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). K tomuto účelu, budete muset použít [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) rutiny. To umožňuje spuštění skriptu na jeden nebo více vzdálených počítačů bez kopírování skript na vzdálených počítačích. Všechny výsledky jsou vráceny do místního počítače, ze kterého jste spustili skript.

> [!NOTE]
> Než použijete **Invoke-Command** rutiny prostředí Windows PowerShell spouštět skripty ve vzdáleném počítači, je nutné nakonfigurovat naslouchací proces služby WinRM tak, aby přijímal vzdálená zprávy. Můžete to provést spuštěním příkazu **winrm quickconfig** na vzdáleném počítači. Další informace najdete v tématu [instalace a konfigurace pro Windows Vzdálená správa](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).


V okně Windows Powershellu, můžete využít tuto syntaxi pro spuštění **LogDeploy.ps1** skriptů ve vzdáleném počítači:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> Existují různé způsoby použití **Invoke-Command** ke spuštění skriptu je nejvíc přímočarý soubor, ale tento přístup při je potřeba zadat hodnoty parametrů a spravovat cesty obsahující mezery.


Pokud při spuštění z příkazového řádku, je potřeba vyvolat spustitelný soubor Windows Powershellu a použít **– příkaz** parametr vaše pokyny:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


Jako dříve, budete muset zadat nějaké další přepínače a řídicí vyhrazené znaky XML, když spustíte příkaz MSBUILD:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


A konečně, stejně jako dříve, můžete použít **Exec** úkol v rámci vlastního cíle nástroje MSBuild pro provedení příkazu:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


Při spouštění tohoto cíle jako součást procesu sestavení prostředí Windows PowerShell se spustí skript na počítači zadaném v **– computername** argument.

## <a name="conclusion"></a>Závěr

Toto téma popisuje, jak spustit skript prostředí Windows PowerShell ze souboru projektu MSBuild. Tento přístup můžete použít ke spuštění skriptu prostředí Windows PowerShell buď místně nebo ve vzdáleném počítači, jako součást krokování nebo automatizovaného sestavení a nasazení procesu.

## <a name="further-reading"></a>Další čtení

Pokyny k podepisování skriptů prostředí Windows PowerShell a správou zásad v provádění, naleznete v tématu [spouštění skriptů Windows Powershellu](https://technet.microsoft.com/library/ee176949.aspx). Informace o spouštění příkazů prostředí Windows PowerShell ze vzdáleného počítače najdete v tématu [spouštění vzdálených příkazů](https://technet.microsoft.com/library/dd819505.aspx).

Další informace o používání vlastní soubory projektu MSBuild k řízení procesu nasazení najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Předchozí](taking-web-applications-offline-with-web-deploy.md)
> [další](troubleshooting-the-packaging-process.md)
