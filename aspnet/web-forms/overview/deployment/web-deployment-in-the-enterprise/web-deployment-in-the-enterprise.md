---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Webové nasazení v podniku | Dokumentace Microsoftu
author: jrjlee
description: Tento kurz popisuje, jak splnit řadu výzvy, které se můžete setkat, když spravujete nasazení podnikových webových aplikací na devel...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 2cb8cb12ec5af376b58b672813af8626cb0f95d8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806549"
---
<a name="web-deployment-in-the-enterprise"></a>Nasazení webu v podniku
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Tento kurz popisuje, jak splnit řadu výzvy, které se můžete setkat, když spravujete nasazení podnikových webových aplikací na vývojové, testovací, přípravného a produkčního prostředí. Tento kurz zahrnuje odkaz na řešení spolu s směs koncepčním a orientované na úlohy obsah do vás provedou různými běžné úlohy a postupy.
> 
> Italština překlad těchto kurzů, navštivte web [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Problémy při nasazení Enterprise

Organizace často narazit tyto výzvy při prohlížení ke správě nasazení složitých, podnikových řešení:

- Musíte být schopni nasadit projekty do různých prostředí, jako je pro vývojáře nebo testovací prostředí, pracovní platformy a produkční servery. Řešení musí být nasazený s jinou konfiguraci nastavení pro každé prostředí.
- Je třeba nasadit více závislých projektů současně jako součást jednoho kroku nebo automatizovaného sestavení a nasazení procesu.
- Potřebujete mít možnost pro nasazení jednotky z automatizovaného procesu. Například chcete použít proces průběžné integrace (CI) nasazení webové aplikace do testovacího prostředí při nového kódu se změnami.
- Musíte být schopni řízení procesu nasazení a nastavení nasazení proměnných mimo aplikaci Visual Studio, jak vývojáři je nepravděpodobné, že máte správnou konfiguraci nastavení nebo potřebné přihlašovací údaje pro každé cílové prostředí.
- Budete muset nasadit projekty založené na schématu databáze a zachovat stávající data na další nasazení.
- Potřebujete k nasazení databází členství na základě ad hoc bez nasazení zásady dat účtu uživatele. Také budete muset aktualizovat schéma databáze členství v nasazeném bez ztráty stávajících dat účtu uživatele.
- Je třeba vyloučit určité soubory nebo složky při nasazení obsahu do různých cílových prostředích.

## <a name="overview-of-approach"></a>Přehled přístupu

Tento kurz, společně s další kurzy v této sérii, používá tento podrobný postup čelit výzvám, popsané výše.

- **Celkový proces sestavení a nasazení pomocí vlastní soubory projektu Microsoft Build Engine (MSBuild).**
- To umožňuje vytvářet a nasazovat každý projekt v řešení v rámci jediné operace možné používat skripty.
- Nastavení pro konkrétní prostředí jsou nakonfigurované pomocí souborů Jednoduchý projekt specifických pro prostředí. Na rozdíl od Visual Studio – centrálního přístupu pomocí konfigurace řešení a publikačních profilů konfigurace nasazení pro různá prostředí, tento přístup umožňuje konfigurovat a spravovat proces nasazení mimo aplikaci Visual Studio. To znamená, že vývojáři není nutné Zdokonalte připojovací řetězce, koncové body služby, přihlašovací údaje serveru a další proměnné nasazení pro cílové prostředí.
- Soubory vlastních projektů lze vyvolat pomocí týmových sestavení jako součást pracovního postupu pro Team Foundation Server (TFS). To vám umožní nakonfigurovat automatické nasazení pro CI scénáře.

**Použijte nástroj pro nasazení Internetové informační služby (IIS) webu (nasazení webu) k balení a nasazování projektů webových aplikací.**

- Nástroj nasazení webu poskytuje architekturu, která umožňuje zabalit a nasadit svůj obsah webové aplikace cílový webový server službu IIS, společně se závislosti, nastavení konfigurace, nastavení zabezpečení a další požadavky.
- Můžete řídit celý proces balení a nasazení z v rámci vlastní soubory projektu MSBuild. Můžete také měnit nastavení konfigurace, které nejsou poskytnuty vašeho balíčku pro nasazení webu, jako jsou připojovací řetězce, koncové body služby a podrobnosti o cílovém služby IIS.
- Nástroj nasazení webu, spolu s Web Publishing Pipeline nabízí velké množství bodů rozšiřitelnosti, které umožňují přizpůsobit svá nasazení. Například je snadné vyloučit nepotřebné soubory a složky z vašeho balíčky nasazení webu.

**Použijte nástroj VSDBCMD.exe nasadit a aktualizovat databázových schématech.**

- VSDBCMD umožňuje nasazování databází ze souboru schématu databáze (.dbschema), který je generován při sestavování projektu databáze sady Visual Studio. Naproti tomu funkcí nasazení databází zahrnutých v nasazení webu je vhodnější nasazení existujících databází z místní instanci systému SQL Server.
- Na rozdíl od funkce sady Visual Studio pro nasazení projektu databáze VSDBCMD vám umožní nasadit rozdílové aktualizace do stávající cílové databáze. To umožňuje zachovat stávající data během postupného upgradu schéma databáze.
- Můžete spustit příkazy VSDBCMD z v rámci vlastní soubory projektu MSBuild.

## <a name="content-map"></a>Mapa obsahu

Tento kurz obsahuje témata, která spadají do čtyř hlavních oblastí.

Tato témata zavést řešení odkazu&#x2014;řešení Správce kontaktů&#x2014;a popisují, jak si ho stáhnout a nakonfigurovat na místním počítači:

- [Řešení správce kontaktů](the-contact-manager-solution.md)
- [Nastavení řešení správce kontaktů](setting-up-the-contact-manager-solution.md)

Tato témata zavést soubory projektu MSBuild, popisují, jak můžete vytvářet a používat soubory vlastních projektů a provede procesem nasazení pro řešení Správce kontaktů:

- [Vysvětlení souboru projektu](understanding-the-project-file.md)
- [Vysvětlení procesu sestavení](understanding-the-build-process.md)

Tato témata popisují nasazení webových aplikací, včetně o sestavení a balení procesu, jak proces sestavení integruje do kanálu publikování webové, jak upravit parametry nasazení a nasazení webových balíčků do cílového umístění prostředí:

- [Sestavení a balení projektů webových aplikací](building-and-packaging-web-application-projects.md)
- [Konfigurace parametrů nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md)
- [Nasazení webových balíčků](deploying-web-packages.md)

- [Nasazení projektu databáze](deploying-database-projects.md) popisuje různé postupy můžete použít k nasazení databázové projekty sady Visual Studio, společně s výhody a nevýhody obou těchto přístupů. [Vytvoření a spuštění souboru příkazů k nasazení](creating-and-running-a-deployment-command-file.md) popisuje, jak vytvořit jednoduchý příkaz soubor, který zapouzdří logiku nasazení a umožňuje vám nasazovat složitá řešení jako proces krokování.
- Nakonec [ruční instalace webových balíčků](manually-installing-web-packages.md) je kurz u konce ukazuje, import webových balíčků do služby IIS.

## <a name="key-technologies"></a>Klíčové technologie

Témata v tomto kurzu primárně použití těchto technologií pro správu sestavení a nasazení:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- Nástroj VSDBCMD.exe databáze nasazení

## <a name="other-tutorials-in-this-series"></a>Další kurzy v této sérii

To je součástí série pěti kurzů na podnikové úrovni, nasazení webu. Toto jsou další kurzy v této sérii:

- [Nasazení webové aplikace v podnikových scénářích](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Tento úvodní obsah obsahuje kontextové informace týkající se série kurzů. Popisuje scénář tohoto kurzu a ukazuje, jak se úlohy a názorné postupy popsané v celé řadě vejde do širší proces správy životního cyklu aplikací (ALM).
- [Konfigurace serverového prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Tento kurz popisuje, jak nakonfigurovat servery Windows pro podporu různých scénářů nasazení, včetně nasazení vzdáleného webového balíčku pomocí Služba agenta nasazení webu (vzdálený agent) nebo obslužná rutina webového nasazení a nasazení vzdálené databáze. Poskytuje rady, jak volba metody nasazení pro konkrétní prostředí, a je zde popsán způsob použít webové farmy Framework (WFF) k replikaci nasazených webových aplikací ve všech webových serverů v serverové farmě.
- [Konfigurace serveru Team Foundation Server pro nasazení webu](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Tento kurz popisuje postup konfigurace serveru TFS pro podporu různých scénářů nasazení, včetně automatizovaného nasazení jako součást procesu CI a ručně spustit nasazení konkrétního sestavení.
- [Pokročilé nasazení podnikového webu](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Tento kurz popisuje, jak k provádění různých rozšířené nasazení úkoly, jako je přizpůsobení nasazené databáze pro více prostředí, s výjimkou souborů a složek z nasazení a převedení webové aplikace do offline režimu během procesu nasazení .

> [!div class="step-by-step"]
> [Next](the-contact-manager-solution.md)
