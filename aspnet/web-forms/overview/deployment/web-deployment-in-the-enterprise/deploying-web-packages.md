---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Nasazení webových balíčků | Microsoft Docs
author: jrjlee
description: Toto téma popisuje, jak můžete publikovat balíčky nasazení webu na vzdálený server pomocí Internetové informační služby (IIS) nástroj pro nasazení webu (webové...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 5d3af0fdcc6e7ae20194ba658e0cf72ad22c1234
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="deploying-web-packages"></a>Nasazení webových balíčků
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak můžete publikovat balíčky nasazení webu na vzdálený server pomocí Internetové informační služby (IIS) nástroj pro nasazení webu, které je (Web Deploy) 2.0.
> 
> Existují dva hlavní způsoby, ve kterých můžete nasadit webový balíček ke vzdálenému serveru:
> 
> - Můžete použít nástroj příkazového řádku MSDeploy.exe přímo.
> - Můžete spustit *[název projektu].deploy.cmd* souboru, který generuje procesu sestavení.
> 
> Konečný výsledek je stejný bez ohledu na to, jaký přístup je použít. V podstatě všechny *. deploy.cmd* nemá soubor je spuštění MSDeploy.exe s předem určený hodnoty, takže nemáte k poskytování tolik informací k nasazení balíčku. Tato funkce zjednodušuje proces nasazení. Na druhé straně přímo pomocí MSDeploy.exe vám dává mnohem větší flexibilitu přes přesně vašeho balíčku nasazení.
> 
> Jaký přístup použijete, bude záviset na různých faktorech, včetně kolik řízení vyžadujete nad tímto procesem nasazení a zda jste zacílený na vzdáleného agenta nasazení webové služby nebo obslužné rutiny nasazení webu. Toto téma vysvětluje, jak používat každý přístup a určuje, kdy je vhodné každý přístup.
> 
> Úlohy a postupy v tomto tématu předpokládat, že:
> 
> - Jste vytvořené a zabalené webové aplikace, jak je popsáno v [budova a projekty webových aplikací balení](building-and-packaging-web-application-projects.md).
> - Změnili jste *SetParameters.xml* soubor poskytuje hodnot parametrů pro cílové prostředí, jak je popsáno v [parametry konfigurace pro nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md).


Spuštění [*název projektu*]*. deploy.cmd* soubor je nejjednodušší způsob, jak nasadit webový balíček. Konkrétně pomocí *. deploy.cmd* souboru nabízí tyto výhody oproti použití MSDeploy.exe přímo:

- Nemusíte zadejte umístění balíčku pro nasazení webu&#x2014; *. deploy.cmd* souboru již ví, kde je.
- Nemusíte zadejte umístění *SetParameters.xml* soubor&#x2014; *. deploy.cmd* souboru již ví, kde je.
- Nemusíte určit zdrojový a cílový zprostředkovatele MSDeploy&#x2014; *. deploy.cmd* souboru již zná hodnot, které chcete použít.
- Nemusíte určit nastavení operace MSDeploy&#x2014; *. deploy.cmd* souboru přidá nejčastěji požadované hodnoty k příkazu MSDeploy.exe automaticky.

Před použitím *. deploy.cmd* souboru k nasazení webového balíčku, ujistěte se, že:

- *. Deploy.cmd* souboru [*název projektu*]. *SetParameters.xml* soubor a webového balíčku ([*název projektu*]. *ZIP*) jsou ve stejné složce.
- Nasazení webu (MSDeploy.exe) je nainstalována v počítači, který běží *. deploy.cmd* souboru.

*. Deploy.cmd* soubor podporuje různé možnosti příkazového řádku. Pokud tento soubor spustit z příkazového řádku, toto je základní syntaxe:


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


Je nutné zadat buď **/T** příznak nebo **/Y** příznak indikující, zda chcete provést spuštění zkušební verze nebo za provozu nasazení v uvedeném pořadí (Nepoužívejte obě příznaky v jednom příkazu). Tato tabulka vysvětlující účel každé z těchto příznaků.

| Příznak | Popis |
| --- | --- |
| **/T** | Volá MSDeploy.exe s **– whatif** příznak, který označuje spuštění zkušební verze. Místo nasazování balíčku, vytvoří se sestava popisující, co by mohlo dojít, pokud nasazení balíčku. |
| **/Y** | Volá MSDeploy.exe bez **– whatif** příznak. To nasadí balíček do místního počítače nebo zadaným cílovým serverem. |
| **/M** | Určuje cílový server název nebo adresu URL služby. Další informace o hodnoty, které tady zadáte, najdete v článku **koncový bod aspekty** v tomto tématu. V případě vynechání **/M** příznak, balíček se nasadí do místního počítače. |
| **/A** | Určuje typ ověřování, který by měl MSDeploy.exe využít k nasazení. Možné hodnoty jsou **NTLM** a **základní**. V případě vynechání **/A** příznaku, výchozí typ ověřování **NTLM** pro nasazení do vzdáleného agenta nasazení webové služby a na **základní** pro nasazení do nasazení webu Obslužná rutina. |
| **/U** | Určuje uživatelské jméno. To platí jenom v případě, že používáte základní ověřování. |
| **/P** | Určuje heslo. To platí jenom v případě, že používáte základní ověřování. |
| **/L** | Označuje, že byste měli nasadit balíček do místní instance služby IIS Express. |
| **/G** | Určuje, že je balíček nasazen pomocí [poskytovatele tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). V případě vynechání **/G** příznak, má výchozí hodnotu **false**. |

> [!NOTE]
> Pokaždé, když se proces vytváření vytvoří webový balíček, také vytvoří soubor s názvem *[název projektu] .deploy-readme.txt* vysvětlující tyto možnosti nasazení.


Kromě těchto příznaky, můžete zadat nastavení operace nasazení webu jako další *. deploy.cmd* parametry. Veškerá další nastavení, které zadáte, se jednoduše předána do základní příkaz MSDeploy.exe. Další informace o těchto nastaveních najdete v tématu [nastavení operace nasazení webu](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Předpokládejme, že chcete nasadit ContactManager.Mvc projekt webové aplikace v testovacím prostředí tak, že spustíte *. deploy.cmd* souboru. Testovací prostředí je nakonfigurovaný na použití vzdáleného agenta nasazení webové služby, jak je popsáno v [konfigurace webového serveru pro nasazení publikování na webu (vzdáleného agenta)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Chcete-li nasadit webovou aplikaci, proveďte následující kroky.

**Nasazení webové aplikace pomocí. souboru deploy.cmd**

1. Sestavení a balíček projekt webové aplikace, jak je popsáno v [budova a projekty webových aplikací balení](building-and-packaging-web-application-projects.md).
2. Změnit *ContactManager.Mvc.SetParameters.xml* souboru tak, aby obsahovala správný parametr hodnoty pro testovací prostředí, jak je popsáno v [parametry konfigurace pro nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md).
3. Otevřete okno příkazového řádku a přejděte do umístění *ContactManager.Mvc.deploy.cmd* souboru.
4. Zadejte tento příkaz a stiskněte klávesu Enter:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

V tomto příkladu:

- **/Y** příznak určuje, že chcete skutečném nasazení balíčku, spíše než to bezplatnou zkušební verzí spustit.
- **/M** příznak označuje, že chcete nasadit balíček na serveru s názvem TESTWEB1. Od této hodnoty MSDeploy.exe pokusí balíček nasadit do vzdáleného agenta nasazení webové služby v http://TESTWEB1/MSDeployAgentService.
- **/A** příznak určuje, zda chcete použít ověřování NTLM. Nemusíte jako takový, zadejte uživatelské jméno a heslo.

Pro ilustraci jak pomocí *. deploy.cmd* souboru zjednodušuje proces nasazení, podívejte se na příkaz MSDeploy.exe, který získá generovány a provést při spuštění *ContactManager.Mvc.deploy.cmd* pomocí možnosti uvedené výše.


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


Další informace o používání *. deploy.cmd* souboru nasazení webového balíčku, najdete v článku [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Pomocí MSDeploy.exe

I když pomocí *. deploy.cmd* souboru obecně zjednodušuje proces nasazení, existují některé situace, kdy je vhodnější použít MSDeploy.exe přímo. Příklad:

- Pokud chcete nasadit do obslužné rutiny webu nasadit jako uživatel není správcem, nemůžete použít *. deploy.cmd* souboru. To je z důvodu chyby v nasazení webu 2.0, jak je popsáno v části **koncový bod aspekty**.
- Pokud chcete ručně přepnout mezi různými *SetParameters.xml* soubory v různých umístěních, budete možná přednost MSDeploy.exe přímo.
- Pokud chcete přepsat několik argumentů příkazového řádku MSDeploy.exe, dáte možná přednost MSDeploy.exe používat přímo.

Pokud používáte MSDeploy.exe, potřebujete poskytovat tři důležité informace:

- A **– zdroj** parametr, který určuje, kde vaše data pocházejí.
- A **– cíle** parametr, který určuje, kde se bude vaše data.
- A **– příkaz** parametr, který určuje [operace](https://technet.microsoft.com/library/dd568989(WS.10).aspx) chcete provést.

MSDeploy.exe spoléhá na [Web Deploy zprostředkovatelé](https://technet.microsoft.com/library/dd569040(WS.10).aspx) proces zdrojové a cílové data. Nasazení webu obsahuje mnoho poskytovatelů, které představují řadu aplikací a zdrojů dat, můžete pracovat s&#x2014;například existuje poskytovatelů pro databáze systému SQL Server, webové servery služby IIS, certifikáty, sestavení sestavení v globální mezipaměti (aktivit GAC), různé jiné konfigurační soubory a spoustu dalších typů dat. Obě **– zdroj** parametr a **– cíle** parametr musí zprostředkovatele, zadejte ve tvaru **– zdroj**: [*providerName*] = [*umístění*]. Když nasazujete balíček webu na web služby IIS, měli byste použít tyto hodnoty:

- **– Zdroj** zprostředkovatele je vždy [balíček](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Příklad:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- **– Cíle** zprostředkovatele je vždy [automaticky](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Příklad:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- **– Příkaz** je vždy **synchronizace**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Kromě toho budete muset zadat různé další [specifický pro zprostředkovatele nastavení](https://technet.microsoft.com/library/dd569001(WS.10).aspx) a Obecné [operace nastavení](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Předpokládejme například, že chcete nasadit webovou aplikaci ContactManager.Mvc pro pracovní prostředí. Nasazení se zaměří na obslužné rutiny nasazení webu a musí používat základní ověřování. Chcete-li nasadit webovou aplikaci, proveďte následující kroky.

**Nasazení webové aplikace pomocí MSDeploy.exe**

1. Sestavení a balíček projekt webové aplikace, jak je popsáno v [budova a projekty webových aplikací balení](building-and-packaging-web-application-projects.md).
2. Změnit *ContactManager.Mvc.SetParameters.xml* souboru tak, aby obsahovala správný parametr hodnoty pro vaše pracovní prostředí, jak je popsáno v [parametry konfigurace pro nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md).
3. Otevřete okno příkazového řádku a přejděte do umístění souboru MSDeploy.exe. Toto je obvykle v %PROGRAMFILES%\IIS\Microsoft V2\msdeploy.exe nasadit Web.
4. Zadejte tento příkaz a stiskněte klávesu Enter (Ignorovat konce řádků):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

V tomto příkladu:

- **– Zdroj** parametr určuje **balíček** zprostředkovatele a určuje umístění webového balíčku.
- **– Cíle** parametr určuje **automaticky** zprostředkovatele. **ComputerName** nastavení poskytuje adresu URL služby položky obslužné rutiny nasazení webu na cílovém serveru. **Authtype** nastavení znamená, že chcete používat základní ověřování, a jako takový je třeba zadat **uživatelské jméno** a **heslo**. Nakonec **includeAcls = "False"** nastavení znamená, že nechcete kopírovat seznamy řízení přístupu (ACL) souborů ve vaší webové aplikaci zdrojového na cílový server.
- **– Příkaz: synchronizace** argument označuje, že chcete replikovat zdrojový obsah na cílovém serveru.
- **– DisableLink** argumenty znamenat, že nechcete replikovat fondy aplikací, konfigurace virtuálního adresáře nebo certifikáty Secure Sockets Layer (SSL) na cílovém serveru. Další informace najdete v tématu [odkaz rozšíření nasazení webových](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- **– SetParamFile** parametr poskytuje umístění *SetParameters.xml* souboru.
- **– AllowUntrusted** přepínač označuje, že nasazení webu musí přijmout certifikátů SSL, které nebyly vydané důvěryhodnou certifikační autoritou. Pokud nasazujete do obslužné rutiny nasazení webu a certifikát podepsaný svým držitelem jste použili k zabezpečení adresu URL služby, musíte zahrnout tento přepínač.

## <a name="automating-web-package-deployment"></a>Automatizace balíček nasazení webu

V spoustu podnikové scénáře budete chtít nasadit jako součást automatického nasazení nebo větší krokování webových balíčků. Bez ohledu na to, zda chcete provést nasazení webových balíčků spuštěním *. deploy.cmd* souboru nebo pomocí přímo MSDeploy.exe, můžete Parametrizace příkazech a volat z cíle v Microsoft Build Engine (MSBuild) soubor projektu.

V ukázkové řešení obraťte se na správce, podívejte se na **PublishWebPackages** target v *Publish.proj* souboru. Tento cíl se spustí jednou pro každou *. deploy.cmd* souboru se identifikovanou pomocí seznamu položek s názvem **PublishPackages**. Cíl používá vybudovat úplnou sadu argumentů hodnot pro všechny vlastnosti a metadata položky *. deploy.cmd* soubor a pak používá **Exec** úlohy ke spuštění příkazu.


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> Komplexnější přehled model souboru projektu v ukázkové řešení a úvod do vlastních projektů soubory v obecné, najdete v tématu [vysvětlení souboru projektu](understanding-the-project-file.md) a [Principy procesu sestavení](understanding-the-build-process.md).


## <a name="endpoint-considerations"></a>Aspekty koncový bod

Bez ohledu na to, jestli nasazení webového balíčku spuštěním *. deploy.cmd* souboru nebo pomocí přímo MSDeploy.exe, je třeba zadat název počítače nebo koncový bod služby pro nasazení.

Pokud cílový webový server je nakonfigurován pro nasazení pomocí vzdáleného agenta nasazení webové služby, je třeba zadat cílová adresa URL služby jako cíl.


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


Alternativně můžete zadat název serveru samostatně jako cíl a nasazení webu odvodí adresu URL služby vzdáleného agenta.


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Pokud cílový webový server je nakonfigurován pro nasazení pomocí obslužné rutiny nasazení webu, budete muset zadat adresu koncového bodu služby webové správy (WMSvc) služby IIS jako cíl. Ve výchozím nastavení a to má formát:


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


Můžete vybrat některý z těchto koncových bodů buď pomocí *. deploy.cmd* soubor nebo MSDeploy.exe přímo. Ale pokud budete chtít nasadit do obslužné rutiny nasazení webu jako uživatel není správcem, jak je popsáno v [konfigurace webového serveru pro nasazení publikování na webu (webové nasazení obslužné rutiny)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), je nutné přidat řetězec dotazu do adresa koncového bodu služby.


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


Důvodem je, že uživatel není správcem nemá přístup na úrovni serveru pro službu IIS; pouze má přístup ke konkrétní web služby IIS. V době psaní z důvodu chyby ve webové publikování kanálu (WPP), nelze spustit *. deploy.cmd* souboru pomocí adresu koncového bodu, který obsahuje řetězec dotazu. V tomto scénáři budete muset nasazení webového balíčku pomocí MSDeploy.exe přímo.

> [!NOTE]
> Další informace o vzdáleného agenta nasazení webové služby a obslužné rutiny nasazení webu, najdete v části [výběr práva přístup k nasazení webu](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Pokyny ke konfiguraci soubory specifické pro prostředí projektu nasazení s těmito koncovými body najdete v tématu [nakonfigurovat vlastnosti nasazení pro cílové prostředí](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="authentication-considerations"></a>Důležité informace o ověřování

Bez ohledu na to, jestli nasazení webového balíčku spuštěním *. deploy.cmd* souboru nebo pomocí přímo MSDeploy.exe, budete muset zadat typ ověřování. Nasazení webu přijímá dvě možné hodnoty: **NTLM** nebo **základní**. Pokud zadáte základní ověřování, musíte taky zadat uživatelské jméno a heslo. Existují různé faktory, které potřebujete znát když vyberete typ ověřování:

- Pokud nasazujete do vzdáleného agenta nasazení webové služby, musíte použít ověřování NTLM. Služba vzdáleného agenta nebude přijímat pověření základního ověřování.
- Pokud nasazujete do obslužné rutiny nasazení webu, můžete použít protokol NTLM nebo základní ověřování. Výchozí nastavení je základní ověřování. Přestože základní ověřování závisí na uživatelská jména a hesla přenesená v prostém textu, přihlašovací údaje jsou chráněny jako obslužné rutiny nasazení webu vždy používá šifrování SSL.
- Pokud váš webový balíček obsahuje databázi a webový server a databázový server je samostatných počítačů, nebudete moct nasadit databázi pomocí ověřování protokolem NTLM kvůli [NTLM "dvěma segmenty směrování" omezení](https://go.microsoft.com/?linkid=9805120). Budete muset použít pověření systému SQL Server v nasazení připojovací řetězec nebo zadat základní ověřovací pověření pro nástroj nasazení webu. Tento problém je podrobně popsaná v další v [nasazení databáze členství v podnikových prostředích](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Závěr

Toto téma popisuje, jak nasadit webový balíček buď spuštěním *. deploy.cmd* souboru nebo pomocí MSDeploy.exe přímo. Je vysvětlit, když každý přístup může být vhodné a je popsané, jak Parametrizace a spustit příkaz nasazení v rámci většího sestavení krokování nebo automatizované procesu.

## <a name="further-reading"></a>Další čtení

Informace o tom, jak vytvořit a Parametrizace balíčku pro nasazení webu, najdete v části [budova a projekty webových aplikací balení](building-and-packaging-web-application-projects.md) a [parametry konfigurace pro nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md). Pokyny k vytváření a nasazování webových balíčků z instance Team Foundation Server (TFS) najdete v tématu [konfigurace Team Foundation Server pro nasazení webu automatizované](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Informace o tom, jak přizpůsobit a řešení potíží s procesu nasazení najdete v tématu [vyloučení souborů a složek z nasazení](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Předchozí](configuring-parameters-for-web-package-deployment.md)
> [další](deploying-database-projects.md)
