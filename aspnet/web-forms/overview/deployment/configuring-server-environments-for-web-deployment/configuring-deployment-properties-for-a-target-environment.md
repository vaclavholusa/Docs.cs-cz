---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: Konfigurace vlastností nasazeného cílového prostředí | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje postup konfigurace vlastností specifických pro prostředí k nasazení ukázkové řešení Správce kontaktů do určité cílové prostředí...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: 8e0a050b2fec272fec3922d1c2f3afff4aee7fca
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387683"
---
<a name="configuring-deployment-properties-for-a-target-environment"></a>Konfigurace vlastností nasazeného cílového prostředí
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje postup konfigurace vlastností specifických pro prostředí k nasazení ukázkové řešení Správce kontaktů do určité cílové prostředí.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) řešení&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="process-overview"></a>Přehled procesu

Soubor projektu, který použijete k vytvoření a nasazení řešení Správce kontaktů je rozdělený do dvou fyzických souborů:

- Ten, který obsahuje univerzální sestavení nastavení a pokyny ( *Publish.proj* souboru).
- Ten, který obsahuje konkrétní prostředí nastavení sestavení (*Env Dev.proj*, *Env Stage.proj*, a tak dále).

V okamžiku sestavení souboru projektu odpovídající specifických pro prostředí se sloučí do univerzální *Publish.proj* souboru a vytvoří kompletní sadu pokynů sestavení. Vytvoření nebo přizpůsobení souborů projektu specifických pro prostředí s nastavením, které popisují vašemu scénáři nasazení můžete nakonfigurovat nasazení u konkrétního cílového prostředí.

Mnoho z těchto hodnot se určují podle konfigurace cílového prostředí&#x2014;zejména, určuje, zda váš cílový webový server konfigurován pro použití služby agenta nasazení webu (vzdálený agent) nebo obslužné rutiny nasazení webu. Další informace o těchto přístupů a pokyny pro výběr správného přístupu pro konkrétní prostředí, najdete v části [výběr právo přístupu k nasazení webu](choosing-the-right-approach-to-web-deployment.md).

[Scénáře využití manažera kontaktu](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) vyžaduje dva soubory projektu pro konkrétní prostředí:

- Nasazení do testovacího prostředí pro vývojáře (*Env Dev.proj*). Testovací prostředí pro vývojáře je nakonfigurovaný tak, aby přijímal vzdálené nasazení s využitím vzdáleného agenta, jak je popsáno v [scénář: Konfigurace testovací prostředí pro nasazení webu](scenario-configuring-a-test-environment-for-web-deployment.md). Tento soubor je potřeba zadat vzdálený agent adresa koncového bodu, jakož i nastavení specifická pro umístění jako připojovací řetězce a koncových bodů služby.
- Nasazení do přípravného prostředí (*Env Stage.proj*). Pracovní prostředí je nakonfigurované tak, aby přijímal vzdálená nasazení pomocí webové nasazení obslužné rutiny, jak je popsáno v [scénář: Konfigurace pracovní prostředí pro nasazení webu](scenario-configuring-a-staging-environment-for-web-deployment.md). Tento soubor je potřeba zadat adresu koncového bodu obslužné rutiny webu nasadit, stejně jako konkrétní umístění nastavení podobné připojovacím řetězcům a koncové body služby.

Je důležité si uvědomit, že nastavení, které nakonfigurujete v souboru projektu specifických pro prostředí nemají vliv na obsah webového balíčku samotného&#x2014;místo toho, jakým se řídí způsob nasazení balíčku a jaké hodnoty parametrů jsou k dispozici po balíček extrahovat. Webový balíček do produkčního prostředí při importu ručně, jak je popsáno v [scénář: Konfigurace provozního prostředí pro nasazení webu](scenario-configuring-a-production-environment-for-web-deployment.md) a [ruční instalace webových balíčků](../web-deployment-in-the-enterprise/manually-installing-web-packages.md), takže nezáleží na nastavení, které jste použili v souboru projektu specifických pro prostředí při generování balíčku. Správce Internetové informační služby (IIS) zobrazí výzvu pro jakékoli parametrizovaného hodnotu, například připojovací řetězce a koncové body služby, při importu balíčku ho.

Pokud chcete nasadit řešení Správce kontaktů na cílovém prostředí, můžete tento soubor upravit, nebo použít jako šablonu a vytvořte svůj vlastní soubor.

**Konfigurace nastavení specifických pro prostředí nasazení pro řešení Správce kontaktů**

1. Otevřete řešení ContactManager WCF v sadě Visual Studio 2010.
2. V **Průzkumníku řešení** okna, rozbalte **publikovat** složky, rozbalte **EnvConfig** složku a poté dvojitým kliknutím **Env-Dev.proj**.

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. Nahraďte hodnoty vlastností v *Env Dev.proj* soubor s správné hodnoty pro testovací prostředí.

    > [!NOTE]
    > V tabulce, které následuje po tomto postupu poskytuje další informace o každé z těchto vlastností.
4. Uložte si práci a pak zavřete *Env Dev.proj* souboru.

## <a name="choosing-the-right-deployment-properties"></a>Výběr vlastnosti správné nasazení

Tato tabulka popisuje účel každé vlastnosti v ukázkovém souboru projektu pro konkrétní prostředí, *Env Dev.proj*a najdete pokyny k hodnoty by měly poskytnout.


|                                                        Název vlastnosti                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        Podrobnosti                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              <strong>MSDeployComputerName</strong> název cílového webového serveru nebo službě koncového bodu.               |                                                                                                                                                                                                                                              Pokud nasazujete službu vzdáleného agenta na cílový webový server, můžete zadat název cílového počítače (například <strong>TESTWEB1</strong> nebo <strong>TESTWEB1.fabrikam.net</strong>), nebo můžete zadat vzdálený koncový bod agenta (například `http://TESTWEB1/MSDEPLOYAGENTSERVICE`). Nasazení funguje stejně v každém případě. Pokud nasazení provádíte do obslužné rutiny webu nasadit na cílový webový server, musí zadat koncový bod služby a obsahovat název webu služby IIS jako parametru řetězce dotazu (například `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`).                                                                                                                                                                                                                                              |
|         <strong>MSDeployAuth</strong> metodu nasazení webu by měl používat k ověření na vzdáleném počítači.          |                                                                                                                                                                                                                          To by mělo být nastavené <strong>NTLM</strong> nebo <strong>základní</strong>. Obvykle budete používat <strong>NTLM</strong> Pokud nasazení provádíte do služby vzdálený agent a <strong>základní</strong> Pokud nasazení provádíte do obslužné rutiny nasazení webu. Pokud používáte základní ověřování, musíte také zadat uživatelské jméno a heslo, které nástroj nasazení webu služby IIS (Web Deploy) by měly vydávat se za účelem nasazení. V tomto příkladu je zajišťována tyto hodnoty <strong>MSDeployUsername</strong> a <strong>MSDeployPassword</strong> vlastnosti. Pokud používáte ověřování protokolem NTLM, můžete vynechat tyto vlastnosti nebo nechat prázdné.                                                                                                                                                                                                                          |
| <strong>MSDeployUsername</strong> Pokud používáte základní ověřování, nasazení webu bude používat tento účet na vzdáleném počítači.  |                                                                                                                                                                                                                                                                                                                                                                                                                       To by měla mít podobu <em>domény</em>\*uživatelské jméno * (například <strong>FABRIKAM\matt</strong>). Tato hodnota se používá jenom při zadání základní ověřování. Pokud používáte ověřování protokolem NTLM, vlastnost lze vynechat. Pokud je zadána hodnota, bude se ignorovat.                                                                                                                                                                                                                                                                                                                                                                                                                        |
| <strong>MSDeployPassword</strong> Pokud používáte základní ověřování, nasazení webu bude používat toto heslo na vzdáleném počítači. |                                                                                                                                                                                                                                                                                                                                                                                                                    Jedná se o heslo pro uživatelský účet, který jste zadali v <strong>MSDeployUsername</strong> vlastnost. Tato hodnota se používá jenom při zadání základní ověřování. Pokud používáte ověřování protokolem NTLM, vlastnost lze vynechat. Pokud je zadána hodnota, bude se ignorovat.                                                                                                                                                                                                                                                                                                                                                                                                                    |
|     <strong>ContactManagerIisPath</strong> ve službě IIS cesta, na kterém chcete nasadit aplikace MVC Správce kontaktů.     |                                                                                                                                                                                                                                                                                                                                                                        Je třeba cesta, jak se zobrazuje ve Správci služby IIS ve formátu [<em>název webu služby IIS</em>] / [<em>webové</em><em>název_aplikace</em>]. Mějte na paměti, že web služby IIS musí existovat před nasazením aplikace. Například pokud jste vytvořili na webu služby IIS s názvem DemoSite, budete moci zadat cestu ke službě IIS pro aplikaci MVC jako DemoSite/ContactManager.                                                                                                                                                                                                                                                                                                                                                                        |
|   <strong>ContactManagerServiceIisPath</strong> ve službě IIS cesta, na kterém chcete nasadit službu WCF Správce kontaktů.    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                          Například pokud jste vytvořili na webu služby IIS s názvem DemoSite, budete moci zadat cestu ke službě IIS pro službu WCF jako <strong>DemoSite/ContactManagerService</strong>.                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|                  <strong>ContactManagerTargetUrl</strong> adresa URL, na které se dá kontaktovat službu WCF.                   |                                                                                                                                                     To bude mít podobu [<em>adresu URL kořenového webu služby IIS</em>] / [<em>název aplikace služby</em>] / [<em>koncový bod služby</em>]. Například pokud jste vytvořili na webu služby IIS na portu 85, adresa URL by mít podobu `http://localhost:85/ContactManagerService/ContactService.svc`. Mějte na paměti, že aplikace MVC a služby WCF se nasadí na stejný server. V důsledku toho tato adresa URL vždy jen přistupovat z počítače, na kterém je nainstalována. Z toho důvodu je vhodnější použít místního hostitele nebo IP adresu, nikoli název počítače nebo hlavičku hostitele v adrese URL. Pokud použijete název počítače nebo hlavičku hostitele [vrácení zpětné smyčky](https://go.microsoft.com/?linkid=9805131) funkce zabezpečení ve službě IIS může blokovat adresy URL a vrátit <strong>HTTP 401.1 – Neautorizováno</strong> chyby.                                                                                                                                                     |
|                  <strong>CmDatabaseConnectionString</strong> připojovací řetězec pro databázový server.                  | Připojovací řetězec Určuje i přihlašovací údaje, které budou používat VSDBCMD kontaktovat server databáze a vytvořit databázi a přihlašovací údaje fondu aplikací webového serveru pomocí spojení se serverem databáze a interakci s databází. V podstatě máte dvě možnosti zde. Můžete zadat <strong>Integrated Security = true</strong>, v takovém případě se používá integrované ověřování Windows: <strong>zdroj dat = TESTDB1; Integrated Security = true</strong> v tomto případě databáze se vytvoří pomocí přihlašovací údaje uživatele, který spouští spustitelný soubor VSDBCMD a aplikace bude přístup k databázi pomocí identity účet počítače webového serveru. Alternativně můžete zadat uživatelské jméno a heslo účtu systému SQL Server. V takovém případě se používají přihlašovací údaje SQL serveru podle VSDBCMD vytvoříte databázi a fond aplikací pro interakci s databází: <strong>zdroj dat = TESTDB1; Id uživatele = ASqlUser; Heslo = Pa$ $w0rd</strong> názorné postupy v tomto tématu se předpokládá, že budete používat integrované ověřování Windows. |
|        <strong>CmTargetDatabase</strong> název, kterému chcete udělit databázi vytvoříte na serveru databáze.        |                                                                                                                                                                                                                                                                                                                                                                                                                                                     Hodnota, kterou tady zadáte, se přidá do příkazu VSDBCMD jako parametr. Používá se také sestavit úplný připojovací řetězec, fond aplikací na webovém serveru můžete použít k interakci s databází.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

Tyto příklady ukazují, jak můžete konfigurovat tyto vlastnosti pro konkrétní scénáře nasazení.

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a>Příklad 1&#x2014;nasazení tak, aby služba vzdáleného agenta

V tomto příkladu:

- Nasazujete do služby vzdálený agent na TESTWEB1.
- Máte instruující, nasazení webu pro použití ověřování NTLM. Nástroj nasazení webu se spustí pomocí přihlašovacích údajů, které jste použili k vyvolání Microsoft Build Engine (MSBuild).
- Integrované ověřování používáte k nasazení **ContactManager** databáze TESTDB1. Databáze se nasadí pomocí přihlašovacích údajů, které jste použili k vyvolání MSBuild.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a>Příklad 2&#x2014;nasazení na webu nasadit koncový bod obslužné rutiny

V tomto příkladu:

- Nasazujete do koncového bodu obslužná rutina nasazení webové služby v STAGEWEB1.
- Máte instruující, Web Deploy používat základní ověřování.
- Určujete, že nasazení webu by měl zosobnit účet FABRIKAM\stagingdeployer na vzdáleném počítači.
- Ověřování serveru SQL Server používáte k nasazení **ContactManager** databáze STAGEDB1.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a>Závěr

V tomto okamžiku soubory projektu jsou plně nakonfigurované k vytvoření a nasazení řešení Správce kontaktů na jeden nebo více cílové prostředí.

Pokud chcete použít tyto soubory projektu jako součást procesu krokování a opakovatelným nasazení, je nutné provést *Publish.proj* soubor pomocí nástroje MSBuild a předejte mu umístění souboru projektu pro konkrétní prostředí jako parametr. Můžete to provést různými způsoby:

- Přehled nástroje MSBuild a úvod do projektu vlastní soubory, naleznete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Informace o tom, jak formulovat pomocí příkazu MSBuild, který provede vlastní soubory, naleznete v tématu [nasazení webových balíčků](../web-deployment-in-the-enterprise/deploying-web-packages.md).
- Informace o tom, jak začlenit MSBuild příkazů do souboru příkazů krokování a opakovatelným nasazení najdete v tématu [vytvořením a spuštěním souboru příkazů k nasazení](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md).
- Informace o tom, jak provést vlastní projektu soubory z nástroje týmové sestavení, naleznete v tématu [vytvoření definice sestavení nasazení podporuje](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

> [!div class="step-by-step"]
> [Předchozí](creating-a-server-farm-with-the-web-farm-framework.md)
