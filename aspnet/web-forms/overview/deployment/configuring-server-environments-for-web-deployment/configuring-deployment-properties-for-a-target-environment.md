---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: Konfigurace vlastnosti nasazení pro cílové prostředí | Microsoft Docs
author: jrjlee
description: Toto téma popisuje, jak konfigurovat vlastnosti specifické pro prostředí k nasazení řešení obraťte se na správce ukázka do určité cílové prostředí...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: ae9e220d1284bcc3aefed09a3f64cceac604fb73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="configuring-deployment-properties-for-a-target-environment"></a>Konfigurace vlastnosti nasazení pro cílové prostředí
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje postup konfigurace vlastností konkrétní prostředí k nasazení řešení obraťte se na správce ukázka do určité cílové prostředí.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz řady používá ukázkové řešení&#x2014; [obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) řešení&#x2014;představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3, komunikaci Windows Služba Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md), ve které je řízené procesu sestavení dva soubory projektu&#x2014;jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="process-overview"></a>Přehled procesu

Soubor projektu, které budete používat k vytváření a nasazování řešení obraťte se na správce je rozdělit do dvou fyzických souborů:

- Ten, který obsahuje universal sestavení pokyny pro nastavení a ( *Publish.proj* souboru).
- Ten, který obsahuje konkrétní prostředí nastavení sestavení (*Env Dev.proj*, *Env Stage.proj*a tak dále).

V okamžiku sestavení souboru projektu příslušné konkrétní prostředí sloučeny do univerzální *Publish.proj* souboru a vytváří kompletní sada pokynů sestavení. Vytvořením nebo přizpůsobení souborů projektu specifické pro prostředí s nastavením, které popisují vlastní scénář nasazení můžete nakonfigurovat nasazení do určité cílové prostředí.

Velké množství tyto hodnoty jsou určeny konfiguraci prostředí cílového&#x2014;konkrétně, jestli je váš cílový webový server nakonfigurované na používání agenta služba pro nasazení webu (vzdáleného agenta) nebo obslužné rutiny nasazení webu. Další informace o těchto přístupů a pokyny k výběru správný přístup pro vaše vlastní prostředí, najdete v části [výběr práva přístup k nasazení webu](choosing-the-right-approach-to-web-deployment.md).

[Scénáře využití manažera kontaktujte](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) vyžaduje dva soubory projektu konkrétní prostředí:

- Nasazení v testovacím prostředí developer (*Env Dev.proj*). Testovací prostředí vývojáře je nakonfigurován tak, aby přijímal vzdáleného nasazení pomocí vzdáleného agenta, jak je popsáno v [scénář: Konfigurace testovací prostředí pro nasazení webu](scenario-configuring-a-test-environment-for-web-deployment.md). Tento soubor musí zajistit vzdáleného agenta adresa koncového bodu, jakož i nastavení pro konkrétní umístění jako koncové body služby a připojovací řetězce.
- Nasazení do pracovního prostředí (*Env Stage.proj*). Je pracovní prostředí nakonfigurováno tak, aby přijímal vzdáleného nasazení pomocí webového nasazení obslužná rutina, jak je popsáno v [scénář: Konfigurace pracovní prostředí pro nasazení webu](scenario-configuring-a-staging-environment-for-web-deployment.md). Tento soubor vyžaduje k poskytování adresa koncového bodu obslužné rutiny nasazení webu, jakož i umístění konkrétní nastavení, jako je připojovací řetězce a koncové body služby.

Je důležité si uvědomit, že nastavení, které nakonfigurujete v souboru projektu konkrétní prostředí nebudou mít vliv na obsah webových balení, sám sebe&#x2014;místo toho řízení nasazení balíčku a jaké hodnoty parametrů jsou dostupné, pokud je balíček extrahovat. Balíček webu do provozního prostředí při importu ručně, jak je popsáno v [scénář: Konfigurace produkčním prostředí pro nasazení webu](scenario-configuring-a-production-environment-for-web-deployment.md) a [ručně instalaci webových balíčků](../web-deployment-in-the-enterprise/manually-installing-web-packages.md), Nezáleží na nastavení, které jste v souboru projektu konkrétní prostředí použili při vygenerování balíčku. Správce Internetové informační služby (IIS) zobrazí výzvu k zadání veškeré parametrizované hodnoty jako koncové body služby a připojovací řetězce při importu balíčku.

K nasazení řešení. Obraťte se na správce cílového prostředí, můžete přizpůsobit tento soubor nebo ho použít jako šablonu a vytvořte svůj vlastní soubor.

**Ke konfiguraci nastavení nasazení konkrétní prostředí pro řešení obraťte se na správce**

1. Otevřete řešení ContactManager WCF v sadě Visual Studio 2010.
2. V **Průzkumníku řešení** okno, rozbalte **publikovat** složky, rozbalte **EnvConfig** složku a pak dvojitým kliknutím **Env-Dev.proj**.

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. Nahraďte hodnoty vlastností v *Env Dev.proj* soubor s správné hodnoty pro testovací prostředí.

    > [!NOTE]
    > Tabulka, která následuje po tomto postupu poskytuje další informace o každé z těchto vlastností.
4. Uložte si práci a pak zavřete *Env Dev.proj* souboru.

## <a name="choosing-the-right-deployment-properties"></a>Výběr vlastností správné nasazení

Tato tabulka popisuje účel každé vlastnosti v ukázkovém souboru projektu specifické pro prostředí, *Env Dev.proj*a obsahuje některé pokyny k hodnoty by měl poskytovat.


|                                                        Název vlastnosti                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        Podrobnosti                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              <strong>MSDeployComputerName</strong> název cílový webový server nebo službu koncového bodu.               |                                                                                                                                                                                                                                              Pokud nasazujete službu vzdáleného agenta na cílový webový server, můžete zadat název cílového počítače (například <strong>TESTWEB1</strong> nebo <strong>TESTWEB1.fabrikam.net</strong>), nebo můžete zadat vzdálených koncový bod agenta (například `http://TESTWEB1/MSDEPLOYAGENTSERVICE`). Nasazení funguje stejným způsobem jako v každém případu. Pokud nasazujete do obslužné rutiny webu nasadit na cílový webový server, měli byste zadat koncový bod služby a obsahovat název webu služby IIS jako parametr řetězce dotazu (například `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`).                                                                                                                                                                                                                                              |
|         <strong>MSDeployAuth</strong> metodu nasazení webu by měl použít k ověření ke vzdálenému počítači.          |                                                                                                                                                                                                                          Musí být nastavena na <strong>NTLM</strong> nebo <strong>základní</strong>. Obvykle použijete <strong>NTLM</strong> Pokud nasazujete ve službě vzdáleného agenta a <strong>základní</strong> Pokud nasazujete do obslužné rutiny nasazení webu. Pokud používáte základní ověřování, budete také muset zadat uživatelské jméno a heslo, které by měla zosobňovat nástroj nasazení webu služby IIS (Web Deploy) Chcete-li provést nasazení. V tomto příkladu jsou tyto hodnoty zajišťováno prostřednictvím <strong>MSDeployUsername</strong> a <strong>MSDeployPassword</strong> vlastnosti. Pokud používáte ověřování NTLM, můžete vynechat tyto vlastnosti nebo zůstat prázdné.                                                                                                                                                                                                                          |
| <strong>MSDeployUsername</strong> Pokud používáte základní ověřování, Web Deploy použije tento účet na vzdáleném počítači.  |                                                                                                                                                                                                                                                                                                                                                                                                                       To by měl být ve formátu <em>domény</em>\*uživatelské jméno * (například <strong>FABRIKAM\matt</strong>). Tato hodnota se používá pouze pokud zadáte základní ověřování. Pokud používáte ověřování NTLM, můžete vynechat vlastnost. Pokud je zadána hodnota, se budou ignorovat.                                                                                                                                                                                                                                                                                                                                                                                                                        |
| <strong>MSDeployPassword</strong> Pokud používáte základní ověřování, Web Deploy použije toto heslo na vzdáleném počítači. |                                                                                                                                                                                                                                                                                                                                                                                                                    Toto je heslo pro uživatelský účet zadaný v <strong>MSDeployUsername</strong> vlastnost. Tato hodnota se používá pouze pokud zadáte základní ověřování. Pokud používáte ověřování NTLM, můžete vynechat vlastnost. Pokud je zadána hodnota, se budou ignorovat.                                                                                                                                                                                                                                                                                                                                                                                                                    |
|     <strong>ContactManagerIisPath</strong> ve službě IIS cestu, na který chcete nasadit aplikace MVC obraťte se na správce.     |                                                                                                                                                                                                                                                                                                                                                                        Měl by to být cesta zobrazí se ve Správci služby IIS ve tvaru [<em>název webu služby IIS</em>] nebo [<em>webové</em><em>název aplikace</em>]. Mějte na paměti, že musí existovat před nasazením aplikace na web služby IIS. Například pokud jste vytvořili webu IIS s názvem DemoSite, budete moci zadat cestu ke službě IIS pro aplikace MVC jako DemoSite nebo ContactManager.                                                                                                                                                                                                                                                                                                                                                                        |
|   <strong>ContactManagerServiceIisPath</strong> ve službě IIS cestu, na který chcete nasadit službu WCF obraťte se na správce.    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                          Pokud jste vytvořili s názvem DemoSite webu IIS, můžete například zadat cestu ke službě IIS pro službu WCF jako <strong>DemoSite/ContactManagerService</strong>.                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|                  <strong>ContactManagerTargetUrl</strong> adresa URL, na které jsou dostupné služby WCF.                   |                                                                                                                                                     To bude mít tvar [<em>adresy URL kořenového adresáře webu služby IIS</em>] nebo [<em>název aplikace služby</em>] nebo [<em>koncový bod služby</em>]. Například pokud vytvoříte webu IIS na portu 85 adresa URL by mít tvar `http://localhost:85/ContactManagerService/ContactService.svc`. Mějte na paměti, že aplikace MVC a služby WCF jsou nasazeny na stejný server. V důsledku toho tato adresa URL přistupuje vždy jen z počítače, na kterém je nainstalována. Z toho důvodu je lepší použít místního hostitele nebo IP adresu, nikoli název počítače nebo hlavičku hostitele v adrese URL. Pokud použijete název počítače nebo hlavičku hostitele, [zpětné smyčky kontrola](https://go.microsoft.com/?linkid=9805131) funkce zabezpečení ve službě IIS může blokovat adresy URL a vrátit <strong>HTTP 401.1 – Neautorizováno</strong> chyby.                                                                                                                                                     |
|                  <strong>CmDatabaseConnectionString</strong> připojovací řetězec pro databázový server.                  | Připojovací řetězec Určuje obou pověření, které budou používat VSDBCMD kontaktovat server, databáze a vytvořit databázi a přihlašovací údaje, které budou používat fondu aplikací webového serveru a obraťte se na serveru databáze komunikovat s databází. V podstatě máte dvě možnosti sem. Můžete zadat <strong>integrované zabezpečení = true</strong>, v takovém případě se používá integrované ověřování systému Windows: <strong>zdroj dat = TESTDB1; Integrated Security = true</strong> v tomto případě databáze se vytvoří pomocí přihlašovací údaje uživatele, který spouští spustitelný soubor VSDBCMD a aplikace bude přístup k databázi pomocí identity účet počítače webového serveru. Alternativně můžete zadat uživatelské jméno a heslo účtu systému SQL Server. V takovém případě jsou pověření systému SQL Server používá VSDBCMD k vytvoření databáze a také fondu aplikací pracovat s databází: <strong>zdroj dat = TESTDB1; Id uživatele = ASqlUser; Heslo = Pa$ $w0rd</strong> názorné postupy v tomto tématu předpokládat, že budete používat integrované ověřování systému Windows. |
|        <strong>CmTargetDatabase</strong> název chcete poskytnout databázi vytvoříte na serveru databáze.        |                                                                                                                                                                                                                                                                                                                                                                                                                                                     Hodnota, kterou tady zadáte, se přidá do příkaz VSDBCMD jako parametr. Slouží také k sestavení úplný připojovací řetězec, který fond aplikací na webovém serveru můžete používat k interakci s databází.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

Tyto příklady ukazují, jak můžete nakonfigurovat tyto vlastnosti pro konkrétní nasazení scénáře.

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a>Příklad 1&#x2014;nasazení pro službu vzdáleného agenta

V tomto příkladu:

- Nasazujete do služby vzdáleného agenta na TESTWEB1.
- Jste instruující, nasazení webu použít ověřování NTLM. Nasazení webu se spustí pomocí přihlašovacích údajů, které jste použili k vyvolání Microsoft Build Engine (MSBuild).
- Integrované ověřování používáte k nasazení **ContactManager** databáze TESTDB1. Databáze se nasadí pomocí přihlašovacích údajů, které jste použili k vyvolání nástroje MSBuild.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a>Příklad 2&#x2014;obslužné rutiny Endpoint nasadit nasazení webu

V tomto příkladu:

- Nasazujete do koncového bodu služby obslužné rutiny nasazení webu na STAGEWEB1.
- Jste instruující, Web Deploy používat základní ověřování.
- Určujete, že nasazení webu by měl zosobnit účet FABRIKAM\stagingdeployer na vzdáleném počítači.
- Ověřování systému SQL Server používáte k nasazení **ContactManager** databáze STAGEDB1.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a>Závěr

Soubory projektu jsou nyní plně konfigurována pro sestavení a nasazení řešení. Obraťte se na správce na jeden nebo více cílové prostředí.

K používání těchto souborů projektu jako součást procesu nasazení krokování, opakovatelných, je nutné provést *Publish.proj* souborů pomocí nástroje MSBuild a předat umístění souboru projektu konkrétní prostředí jako parametr. Můžete provést různými způsoby:

- Přehled nástroje MSBuild a úvod do vlastních projektů soubory, najdete v části [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Informace o tom, jak pomocí nástroje MSBuild příkazu, který provede soubory vlastní projektu formulovali najdete v tématu [nasazení webových balíčků](../web-deployment-in-the-enterprise/deploying-web-packages.md).
- Informace o tom, jak začlenit vaší MSBuild příkazy do příkazového řádku pro jeden krok, opakované nasazení najdete v tématu [vytváření a spouštění soubor příkazů nasazení](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md).
- Informace o tom, jak provést soubory vlastní projektu z nástroje týmové sestavení najdete v tématu [vytváření definici sestavení toto nasazení podporuje](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

> [!div class="step-by-step"]
> [Předchozí](creating-a-server-farm-with-the-web-farm-framework.md)
