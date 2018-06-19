---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Nasazení v podnikové síti webu | Microsoft Docs
author: jrjlee
description: Tento kurz popisuje, jak splnit spoustu problémů, které se můžete setkat, když spravujete nasazení podnikovém měřítku webových aplikací na devel...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: fc463cb689f4f63a12788b80958c9fc8fe20119d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890460"
---
<a name="web-deployment-in-the-enterprise"></a>Nasazení webu v podnikové síti
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Tento kurz popisuje, jak splnit spoustu problémů, které se můžete setkat, když spravujete nasazení podnikovém měřítku webových aplikací na vývoj, testování, pracovní a provozní prostředí. Tento kurz zahrnuje odkaz na řešení společně s směs koncepční a orientované na úlohy obsah vás provedou různé běžné úlohy a postupy.
> 
> Italská překlad tyto kurzy, najdete v článku [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Podnikové nasazení výzvy

Když se podíváte ke správě nasazení komplexní, řešení podnikovém měřítku, organizace často dojde tyto problémy:

- Abyste mohli nasadit projekty do několika prostředích, jako je vývojáře nebo testovací prostředí, musíte, pracovní platformy a produkční servery. Řešení je potřeba nasadit pomocí různých konfiguračních nastavení pro každé prostředí.
- Je třeba nasadit více závislých projektů současně v rámci jednoho kroku nebo automatizované procesu sestavení a nasazení.
- Potřebujete mít možnost pro nasazení jednotky z automatizovaného procesu. Například chcete použít průběžnou integraci (CI) postup nasazení webové aplikace v testovacím prostředí při vrácení se změnami nový kód.
- Musíte být schopni řídit proces nasazení a nastavení proměnných pro nasazení mimo aplikaci Visual Studio, protože vývojáři pravděpodobně nebudou mít správné konfiguraci nastavení nebo potřebná pověření pro každé cílové prostředí.
- Potřebujete nasadit na základě schématu databázové projekty a zachovat stávající data na následné nasazení.
- Je třeba nasadit databáze členství na základě ad hoc bez nasazení data účtu uživatele. Také musíte aktualizovat schéma databáze členství nasazené bez ztráty stávajících dat účtu uživatele.
- Budete muset určité soubory nebo složky vyloučit při nasazení obsahu pro různé cílové prostředí.

## <a name="overview-of-approach"></a>Přehled přístupu

Tento kurz, společně s další kurzy v této série používá tento vysoké úrovně přístup k nápravě popsané výše.

- **K řízení procesu sestavení a nasazení celkové používejte vlastní soubory projektu Microsoft Build Engine (MSBuild).**
- To vám umožní vytvořit a nasadit všechny projekty v řešení v rámci jediné operace Skriptovatelná.
- Nastavení pro konkrétní prostředí jsou konfigurováni pomocí souborů projektu jednoduché konkrétní prostředí. Na rozdíl od použití konfigurace řešení Visual Studio – centrálního přístupu a publikovat profil ke konfiguraci nasazení v různých prostředích, tento přístup umožňuje konfigurovat a spravovat proces nasazení mimo aplikaci Visual Studio. To znamená, že vývojáři není nutné zálohy znalosti připojovací řetězce, koncové body služby, přihlašovací údaje serveru a jiné proměnné nasazení pro cílové prostředí.
- Soubory projektu vlastní může vyvolat Team Build jako součást pracovního postupu Team Foundation Server (TFS). To vám umožňuje nakonfigurovat automatické nasazení pro CI scénáře.

**Použijte nástroj nasazení webu Internetové informační služby (IIS) (Web Deploy) pro zabalení a nasazení projekty webových aplikací.**

- Nasazení webu poskytuje rozhraní, které vám umožní nasazení obsahu webové aplikace do cílový webový server IIS, společně s závislosti, nastavení konfigurace, nastavení zabezpečení a další požadavky a balíček.
- Celý proces balení a nasazení z můžete řídit v rámci vlastní soubory projektu nástroje MSBuild. Můžete také upravit nastavení konfigurace doprovázejících vašeho balíčku pro nasazení webu, jako je připojovací řetězce, koncové body služby a podrobnosti o cílovém služby IIS.
- Nasazení webu, společně s kanálu publikování webové, nabízí spoustu body rozšiřitelnosti, které umožňují přizpůsobit vaše nasazení. Například je snadné vyloučení nepotřebných souborů a složek z vaší balíčky pro nasazení webu.

**Pomocí nástroje VSDBCMD.exe k nasazení a aktualizovat schémata databáze.**

- VSDBCMD umožňuje nasadit databáze ze souboru schématu databáze (.dbschema), který se vygeneruje, když vytvoříte projekt sady Visual Studio databáze. Funkci nasazení databáze, který je součástí nasazení webu spočívá v tom, vhodnější nasazení existující databáze z místní instance systému SQL Server.
- Na rozdíl od funkce sady Visual Studio pro nasazování databázové projekty VSDBCMD umožňuje nasadit rozdílové aktualizace stávající cílovou databázi. To umožňuje zachovat stávající data během postupného upgradu schématu databáze.
- Příkazy VSDBCMD z můžete spustit v rámci vlastní soubory projektu nástroje MSBuild.

## <a name="content-map"></a>Mapa obsahu

Tento kurz obsahuje témata, které spadají do čtyř hlavních oblastí.

Tato část představuje odkaz na řešení&#x2014;řešení obraťte se na správce&#x2014;a popisují, jak ji stáhnout a nakonfigurovat na místním počítači:

- [Řešení správce kontaktů](the-contact-manager-solution.md)
- [Nastavení řešení správce kontaktů](setting-up-the-contact-manager-solution.md)

Tato témata zavést soubory projektu nástroje MSBuild, popisují, jak můžete vytvořit a používat soubory vlastních projektů a proveďte potřebné kroky nasazení pro řešení obraťte se na správce:

- [Vysvětlení souboru projektu](understanding-the-project-file.md)
- [Vysvětlení procesu sestavení](understanding-the-build-process.md)

Tato témata popisují nasazení webových aplikací, včetně jak sestavení a balení zpracování funguje, jak procesu sestavení integruje s kanálu publikování webové, jak upravit parametry nasazení a nasazení webových balíčků do cílového umístění prostředí:

- [Sestavení a balení projektů webových aplikací](building-and-packaging-web-application-projects.md)
- [Konfigurace parametrů nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md)
- [Nasazení webových balíčků](deploying-web-packages.md)

- [Nasazení databázové projekty](deploying-database-projects.md) popisuje různé postupy můžete použít k nasazení databázové projekty sady Visual Studio, společně s výhody a nevýhody obou těchto přístupů. [Vytváření a spouštění soubor příkazů nasazení](creating-and-running-a-deployment-command-file.md) popisuje, jak vytvořit soubor jednoduchý příkaz, který zapouzdřuje logika nasazení a umožňuje nasadit komplexní řešení jako krokování proces.
- Nakonec [ručně instalaci webových balíčků](manually-installing-web-packages.md) je kurz ukončen zobrazením import webových balíčků do služby IIS.

## <a name="key-technologies"></a>Klíčové technologie

Témata v tomto kurzu hlavně pro správu sestavení a nasazení použijte tyto technologie:

- Visual Studio 2010
- MSBuild
- SLUŽBU IIS 7.5
- Web Deploy 2.0
- Nástroj pro nasazení databáze VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Další kurzy v této série

To je součástí ze série kurzů pět v podnikovém měřítku nasazení webu. Toto jsou další kurzy v této sérii:

- [Nasazení webové aplikace v podnikové scénáře](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Tento úvodní obsah poskytuje kontextové pozadí pro kurz řady. Popisuje kurz scénář a ukazuje, jak se úlohy a postupy popsané v celé řady začlenit do procesu širší Application Lifecycle Management (ALM).
- [Konfigurace serveru prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Tento kurz popisuje postup konfigurace serverů Windows na podporu různých scénářů nasazení, včetně nasazení balíčku vzdáleném webovém pomocí agenta služba pro nasazení webu (vzdáleného agenta) nebo obslužné rutiny nasazení webu a nasazení vzdálené databáze. Na výběr metody nasazení pro vaše vlastní prostředí obsahuje pokyny a popisuje, jak použít k replikaci nasazených webových aplikací ve všech webových serverech ve farmě serverů webové farmy Framework (WFF).
- [Konfigurace serveru Team Foundation Server pro nasazení webu](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Tento kurz popisuje, jak nakonfigurovat produktu TFS na podporu různých scénářů nasazení, včetně automatického nasazení v rámci procesu položek konfigurace a ručně spustí nasazení konkrétní sestavení.
- [Pokročilé nasazení webu Enterprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Tento kurz popisuje, jak k provádění různých dalších pokročilých úloh nasazení, jako vlastní nastavení nasazení databáze pro prostředí s více, vyloučení souborů a složek z nasazení a přepnutím do režimu offline webové aplikace během procesu nasazení .

> [!div class="step-by-step"]
> [Next](the-contact-manager-solution.md)
