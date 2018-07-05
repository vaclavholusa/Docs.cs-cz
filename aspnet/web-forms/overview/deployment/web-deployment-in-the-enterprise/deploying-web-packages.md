---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Nasazení webových balíčků | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak můžete publikovat balíčky nasazení webu na vzdálený server pomocí Internetové informační služby (IIS) nástroj nasazení webu (Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: a660a9a5593d41064ab099296d4a7a675b206bb5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392855"
---
<a name="deploying-web-packages"></a>Nasazení webových balíčků
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak publikovat balíčky nasazení webu na vzdálený server s použitím Internetové informační služby (IIS) nástroj pro nasazení webu (nasazení webu) 2.0.
> 
> Existují dva hlavní způsoby, ve kterých můžete nasadit webový balíček ke vzdálenému serveru:
> 
> - Můžete použít příkazový řádek MSDeploy.exe přímo.
> - Můžete spustit *[název projektu].deploy.cmd* souboru, který generuje procesu sestavení.
> 
> Konečný výsledek je stejný bez ohledu na to, jaký přístup je použít. V podstatě všech *. deploy.cmd* nemá soubor představuje spuštění MSDeploy.exe se několik předdefinovaných hodnot, takže není nutné poskytnout co nejvíce informací, aby bylo možné nasadit balíček. Tato funkce zjednodušuje proces nasazení. Na druhé straně přímo pomocí MSDeploy.exe poskytuje mnohem větší flexibilitu nad přesně jak váš balíček nasazen.
> 
> Jaký přístup je použít, bude záviset na různých faktorech, včetně jak velkou kontrolu vyžadují nad tímto procesem nasazení a vývoji cílíte služba vzdáleného agenta pro nasazení webu nebo obslužné rutiny nasazení webu. Toto téma vysvětluje, jak používat každý přístup a identifikuje, kdy každý přístup je vhodný.
> 
> Úlohy a názorné postupy v tomto tématu se předpokládá, že:
> 
> - Právě jste vytvořili a zabalené webové aplikace, jak je popsáno v [sestavení a balení projektů webových aplikací](building-and-packaging-web-application-projects.md).
> - Jsme změnili *SetParameters.xml* soubor k poskytování hodnot parametrů pro cílové prostředí, jak je popsáno v [konfigurace parametrů nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md).


Spuštění [*název projektu*]*. deploy.cmd* soubor je nejjednodušší způsob, jak nasadit webový balíček. Zejména pomocí *. deploy.cmd* file nabízí tyto výhody oproti použití MSDeploy.exe přímo:

- Není nutné zadat umístění balíčku pro nasazení webu&#x2014; *. deploy.cmd* souboru již ví, kde je.
- Není nutné zadat umístění *SetParameters.xml* souboru&#x2014; *. deploy.cmd* souboru již ví, kde je.
- Není nutné určit zdroj a cíl MSDeploy zprostředkovatele&#x2014; *. deploy.cmd* soubor už zná hodnot, které chcete použít.
- Není nutné zadat nastavení operace MSDeploy&#x2014; *. deploy.cmd* souboru přidá běžně požadované hodnoty do příkazu MSDeploy.exe automaticky.

Než použijete *. deploy.cmd* uvést pro nasazení webového balíčku, ujistěte se, že:

- *. Deploy.cmd* souboru, [*název projektu*]. *SetParameters.xml* souboru a webového balíčku ([*název projektu*]. *ZIP*) jsou ve stejné složce.
- Na počítači, na kterém je nainstalován nástroj nasazení webu (MSDeploy.exe) *. deploy.cmd* souboru.

*. Deploy.cmd* soubor podporuje různé možnosti příkazového řádku. Když spustíte tento soubor z příkazového řádku, toto je základní syntaxe:


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


Musíte zadat buď **/T** příznak nebo **/Y** příznak označující, zda chcete v uvedeném pořadí provést spuštění zkušební verze nebo živé nasazení (nepoužívejte oba příznaky v jednom příkazu). Tato tabulka vysvětluje účel každého z těchto příznaků.

| Příznak | Popis |
| --- | --- |
| **/T** | Volá MSDeploy.exe s **– whatif** příznak, který označuje spuštění zkušební verze. Namísto nasazení balíčku, vytvoří zprávu o co by mohlo dojít, pokud nasazení balíčku. |
| **/Y** | Volá MSDeploy.exe bez **– whatif** příznak. To nasadí balíček do místního počítače nebo zadaným cílovým serverem. |
| **/M** | Určuje cílový server název nebo adresu URL služby. Další informace o hodnoty, které tady zadáte, najdete v článku **koncový bod aspekty** v tomto tématu. Vynecháte-li **/M** příznak, balíček se nasadí do místního počítače. |
| **/A** | Určuje typ ověřování, který MSDeploy.exe měla použít k provedení nasazení. Možné hodnoty jsou **NTLM** a **základní**. Vynecháte-li **/A** příznak, výchozí typ ověřování **NTLM** pro nasazení do služby vzdáleného agenta pro nasazení webu a **základní** nasazení pro nasazení webu Obslužná rutina. |
| **/U** | Určuje uživatelské jméno. To platí jenom v případě, že používáte základní ověřování. |
| **/P** | Určuje heslo. To platí jenom v případě, že používáte základní ověřování. |
| **/L** | Označuje, že byste měli nasadit balíček do místní instance služby IIS Express. |
| **/G** | Určuje, že balíček je nasazen pomocí [poskytovatele tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Vynecháte-li **/G** příznak, má výchozí hodnotu **false**. |

> [!NOTE]
> Pokaždé, když proces sestavení vytvoří webový balíček, také vytvoří soubor s názvem *[název projektu] .deploy-readme.txt* , který popisuje tyto možnosti nasazení.


Kromě těchto příznaků, můžete zadat nastavení operace nasazení webu jako další *. deploy.cmd* parametry. Veškerá další nastavení, které jste zadali, se jednoduše předává do příslušný základní příkaz MSDeploy.exe. Další informace o těchto nastaveních najdete v tématu [nastavení operace nasazení webu](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Předpokládejme, že chcete nasadit projekt ContactManager.Mvc webové aplikace do testovacího prostředí spuštěním *. deploy.cmd* souboru. Testovací prostředí je nakonfigurován pro použití služby vzdáleného agenta pro nasazení webu, jak je popsáno v [nakonfigurovat webový Server pro nasazení publikování na webu (vzdálený Agent)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Pokud chcete nasadit webové aplikace, budete muset provést další kroky.

**Nasazení webové aplikace s využitím. deploy.cmd souboru**

1. Sestavení a zabalení webové aplikace, jak je popsáno v [sestavení a balení projektů webových aplikací](building-and-packaging-web-application-projects.md).
2. Upravit *ContactManager.Mvc.SetParameters.xml* soubor bude obsahovat hodnoty správné parametrů pro testovací prostředí, jak je popsáno v [konfigurace parametrů nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md).
3. Otevřete okno příkazového řádku a přejděte do umístění *ContactManager.Mvc.deploy.cmd* souboru.
4. Zadejte následující příkaz a stiskněte klávesu Enter:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

V tomto příkladu:

- **/Y** příznak určuje, že chcete skutečném nasazení balíčku, nikoli způsobem zkušební spuštění.
- **/M** příznak určuje, že chcete nasadit balíček do serveru s názvem TESTWEB1. Z této hodnoty MSDeploy.exe se pokus o nasazení balíčku na službu vzdáleného agenta pro nasazení webu na http://TESTWEB1/MSDeployAgentService.
- **/A** příznak určuje, že chcete použít ověřování NTLM. V důsledku toho není nutné zadat uživatelské jméno a heslo.

Pro ilustraci způsob použití *. deploy.cmd* soubor zjednodušuje proces nasazení, podívejte se na, který získá vygenerována a spustit při spuštění příkazu MSDeploy.exe *ContactManager.Mvc.deploy.cmd* pomocí možnosti zobrazené výše.


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


Další informace o používání *. deploy.cmd* soubor pro nasazení webového balíčku naleznete v tématu [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Pomocí MSDeploy.exe

Ačkoli použití *. deploy.cmd* souboru obecně zjednodušuje proces nasazení, existují určité situace, když je vhodnější použít MSDeploy.exe přímo. Příklad:

- Pokud chcete nasadit do obslužné rutiny webu nasadit jako uživatel bez oprávnění správce, nelze použít *. deploy.cmd* souboru. Je to z důvodu chyby v nasazení webu 2.0, jak je popsáno v části **koncový bod aspekty**.
- Pokud chcete ručně přepínat mezi různými *SetParameters.xml* soubory v různých umístěních, možná dáte přednost použití MSDeploy.exe přímo.
- Pokud chcete přepsat několik argumentů příkazového řádku MSDeploy.exe, dáte možná přednost MSDeploy.exe používat přímo.

Při použití MSDeploy.exe, budete muset zadat tři klíčové informace:

- A **– zdroj** parametr, který určuje, odkud pocházejí.
- A **– dest** parametr, který označuje, kde se to vaše data.
- A **– příkaz** parametr, který označuje, [operace](https://technet.microsoft.com/library/dd568989(WS.10).aspx) chcete provést.

MSDeploy.exe spoléhá na [poskytovatele nasazení webu](https://technet.microsoft.com/library/dd569040(WS.10).aspx) k datům zdrojového a cílového procesu. Nástroj nasazení webu obsahuje velké množství poskytovatelů, které představují oblasti aplikace a data zdrojů může fungovat s&#x2014;například existují poskytovatelů pro databáze systému SQL Server, webové servery služby IIS, certifikáty, globální sestavení sestavení cache (GAC), různé různými konfiguračními soubory a spoustu dalších typů dat. Oba **– zdroj** parametr a **– dest** parametr musí určovat poskytovatele, ve formuláři **– zdroj**: [*providerName*] = [*umístění*]. Při nasazení webového balíčku k webu služby IIS, měli byste použít tyto hodnoty:

- **– Zdroj** zprostředkovatele je vždy [balíčku](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Příklad:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- **– Dest** zprostředkovatele je vždy [automaticky](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Příklad:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- **– Příkaz** je vždy **synchronizace**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Kromě toho budete muset zadat různé jiné [nastavení specifické pro zprostředkovatele](https://technet.microsoft.com/library/dd569001(WS.10).aspx) Obecné [operace nastavení](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Předpokládejme například, že chcete nasadit ContactManager.Mvc webovou aplikaci do přípravného prostředí. Nasazení se zaměří na obslužné rutiny nasazení webu a musí používat základní ověřování. Pokud chcete nasadit webové aplikace, budete muset provést další kroky.

**Nasazení webové aplikace s využitím MSDeploy.exe**

1. Sestavení a zabalení webové aplikace, jak je popsáno v [sestavení a balení projektů webových aplikací](building-and-packaging-web-application-projects.md).
2. Upravit *ContactManager.Mvc.SetParameters.xml* soubor bude obsahovat hodnoty správné parametrů pro testovací prostředí, jak je popsáno v [konfigurace parametrů nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md).
3. Otevřete okno příkazového řádku a přejděte do umístění MSDeploy.exe. To je obvykle v %PROGRAMFILES%\IIS\Microsoft V2\msdeploy.exe webové nasazení.
4. Zadejte následující příkaz a stiskněte klávesu Enter (Ignorovat konce řádku):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

V tomto příkladu:

- **– Zdroj** určuje parametr **balíčku** zprostředkovatele a označuje umístění webového balíčku.
- **– Dest** určuje parametr **automaticky** zprostředkovatele. **ComputerName** nastavení poskytuje adresu URL služby položky obslužné rutiny nasazení webu na cílovém serveru. **Typ authtype** nastavení označuje, že chcete používat základní ověřování, a proto je potřeba zadat **uživatelské jméno** a **heslo**. Nakonec **includeAcls = "False"** nastavení znamená, že nechcete kopírovat seznamy řízení přístupu (ACL) souborů ve webové aplikaci zdrojového na cílový server.
- **– Příkaz: synchronizace** argument určuje, že chcete replikovat zdrojový obsah na cílovém serveru.
- **– DisableLink** argumenty znamenat, že nechcete replikovat fondy aplikací, konfigurace virtuálního adresáře nebo certifikáty vrstvy SSL (Secure Sockets) na cílovém serveru. Další informace najdete v tématu [webové nasazení rozšíření odkazu](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- **– SetParamFile** parametr poskytuje umístění *SetParameters.xml* souboru.
- **– AllowUntrusted** přepínač označuje, že nasazení webu by měla přijímat certifikátů SSL, které nebyly vydané důvěryhodnou certifikační autoritou. Pokud nasazujete do obslužné rutiny nasazení webu a jste použili certifikát podepsaný svým držitelem pro zabezpečení adresu URL služby, budete muset zahrnout tento přepínač.

## <a name="automating-web-package-deployment"></a>Automatizace nasazení webového balíčku

V mnoha podnikové scénáře budete chtít nasadit jako součást větší krokování nebo automatizované nasazení webových balíčků. Bez ohledu na to, zda se rozhodnete pro nasazování webových balíčků spuštěním *. deploy.cmd* souborů nebo s použitím MSDeploy.exe přímo, můžete parametrizovat příkazům a volat z cíle v Microsoft Build Engine (MSBuild) soubor projektu.

V ukázkovém řešení Správce kontaktů, podívejte se na **PublishWebPackages** target v *Publish.proj* souboru. Tento cíl se spustí jednou pro každou *. deploy.cmd* soubor určený seznam položek s názvem **PublishPackages**. Cíl používá vlastností a metadat položky k vytvoření kompletní sadu hodnot argumentů pro každý *. deploy.cmd* souboru a následně použije **Exec** úkolů ke spuštění příkazu.


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> Širší přehled modelu soubor projektu v ukázkovém řešení a úvod do projektu vlastní soubory v obecné, najdete v tématu [vysvětlení souboru projektu](understanding-the-project-file.md) a [Principy procesu sestavení](understanding-the-build-process.md).


## <a name="endpoint-considerations"></a>Důležité informace o koncový bod

Bez ohledu na to, jestli nasazení webového balíčku spuštěním *. deploy.cmd* souborů nebo s použitím MSDeploy.exe přímo, je třeba zadat název počítače nebo koncový bod služby pro vaše nasazení.

Pokud cílový webový server je nakonfigurován pro nasazení pomocí služby vzdáleného agenta pro nasazení webu, zadejte cílovou adresu URL služby jako cíl.


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


Alternativně můžete zadat název serveru samostatně jako cíl a nasazení webu se odvodit adresu URL vzdáleného agenta služby.


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Pokud cílový webový server je nakonfigurován k nasazení pomocí obslužné rutiny nasazení webu, musíte zadat adresu koncového bodu služby webové správy (WMSvc) služby IIS jako cíl. Ve výchozím nastavení tato akce trvá formuláře:


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


Můžete cílit na některý z těchto koncových bodů, buď pomocí *. deploy.cmd* souboru nebo přímo MSDeploy.exe. Nicméně pokud chcete do obslužné rutiny nasazení webu nasadit jako uživatel bez oprávnění správce, jak je popsáno v [nakonfigurovat webový Server pro nasazení publikování na webu (nasazení obslužná rutina webových)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), budete muset přidat řetězec dotazu k adrese koncového bodu služby.


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


Důvodem je, že uživatel bez oprávnění správce nemá přístup na úrovni serveru služby IIS; uživatel má přístup pouze k konkrétní web služby IIS. V době psaní textu kvůli chybě v webového publikování kanálu (WPP), nelze spustit *. deploy.cmd* soubor pomocí adresy koncového bodu, který obsahuje řetězec dotazu. V tomto scénáři budete muset nasadit přímo pomocí MSDeploy.exe webového balíčku.

> [!NOTE]
> Další informace o službě vzdáleného agenta pro nasazení webu a obslužné rutiny nasazení webu, naleznete v tématu [výběr právo přístupu k nasazení webu](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Pokyny ke konfiguraci souborů projektu specifických pro prostředí pro nasazení s těmito koncovými body najdete v tématu [nakonfigurovat vlastnosti nasazení pro cílové prostředí](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="authentication-considerations"></a>Důležité informace o ověřování

Bez ohledu na to, jestli nasazení webového balíčku spuštěním *. deploy.cmd* souborů nebo s použitím MSDeploy.exe přímo, je třeba zadat typ ověřování. Nástroj nasazení webu přijímá dva možné hodnoty: **NTLM** nebo **základní**. Pokud zadáte základní ověřování, musíte také zadat uživatelské jméno a heslo. Existují různé faktory, které je třeba vědět, když vyberete typ ověřování:

- Pokud nasazujete službu vzdáleného agenta pro nasazení webu, je potřeba použít ověřování NTLM. Služba vzdáleného agenta nepřijímá přihlašovací údaje pro základní ověřování.
- Pokud nasazení provádíte do obslužné rutiny nasazení webu, můžete použít protokol NTLM nebo základní ověřování. Ve výchozím nastavení je základní ověřování. I když základní ověřování závisí na uživatelská jména a hesla v prostém textu zašifrované, svoje přihlašovací údaje jsou chráněné jako obslužné rutiny nasazení webu vždy používá šifrování pomocí protokolu SSL.
- Pokud webový balíček obsahuje databázi a webovým serverem a serverem databáze jsou samostatné počítače, nebude moct nasadit databázi používat ověřování protokolem NTLM kvůli [NTLM "dvěma segmenty směrování" omezení](https://go.microsoft.com/?linkid=9805120). Budete muset použít přihlašovací údaje SQL serveru v nasazení připojovacího řetězce nebo zadat pověření základního ověřování pro nástroj nasazení webu. Tento problém je popsána podrobněji [nasazení databází členství do podnikových prostředí](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Závěr

Toto téma popisuje, jak nasadit webový balíček buď spuštěním *. deploy.cmd* souboru nebo přímo pomocí MSDeploy.exe. To vysvětlit při každý přístup může být vhodné, a popisuje jak parametrizovat a spusťte příkaz nasazení jako součást procesu větší krokování nebo automatizovaných sestavení.

## <a name="further-reading"></a>Další čtení

Informace o tom, jak vytvořit a parametrizujte balíčku pro nasazení webu najdete v tématu [sestavení a balení projektů webových aplikací](building-and-packaging-web-application-projects.md) a [konfigurace parametrů nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md). Pokyny k vytvoření a nasazení webových balíčků z instance Team Foundation Server (TFS), najdete v článku [konfigurace serveru Team Foundation Server pro nasazení webu pomocí automatizované](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Informace o tom, jak přizpůsobit a řešení potíží s procesem nasazení najdete v tématu [vyloučení souborů a složek z nasazení](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Předchozí](configuring-parameters-for-web-package-deployment.md)
> [další](deploying-database-projects.md)
