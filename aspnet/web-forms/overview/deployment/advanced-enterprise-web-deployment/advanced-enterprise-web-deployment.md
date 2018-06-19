---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Pokročilé nasazení webu Enterprise | Microsoft Docs
author: jrjlee
description: Tento kurz vám ukáže, jak provádět různé úlohy, které jsou požadované, nebo žádoucí v spoustu podnikové scénáře nasazení. Pro italská translati...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 892e494b6fde994c4d04952382e4d618d73cad5c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879930"
---
<a name="advanced-enterprise-web-deployment"></a>Nasazení webu pokročilé Enterprise
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Tento kurz vám ukáže, jak provádět různé úlohy, které jsou požadované, nebo žádoucí v spoustu podnikové scénáře nasazení.
> 
> Italská překlad tyto kurzy, najdete v článku [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


To je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz řady používá ukázkové řešení&#x2014; [obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) řešení&#x2014;představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3, komunikaci Windows Služba Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md), ve které je řízené procesu sestavení dva soubory projektu&#x2014;jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="scenario-overview"></a>Přehled scénáře

Tento scénář vysoké úrovně pro tyto kurzy je popsaná v [Enterprise nasazení webu: přehled scénářů](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Doporučujeme si přečíst toto téma, než začnete tento kurz.

## <a name="how-to-use-this-tutorial"></a>Jak používat v tomto kurzu

- Každé z témat v tomto kurzu je samostatný a řeší konkrétní problém nebo problém, který se nachází v podnikové scénáře nasazení. Nemusíte fungovat prostřednictvím těchto témat v libovolném pořadí. V tomto kurzu ale zahrnuje některé pokročilé úlohy. Jako takový bez měli Seznamte se s koncepty a techniky, které [nasazení webu v podnikové síti](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) kurz se zaměřuje na aby získal co nejvíce výhod z tohoto obsahu.
- Tento kurz obsahuje následující témata:
- [Provádění nasazení "Co když"](performing-a-what-if-deployment.md). V celá řada možných scénářů budete chtít určit dopad navrhované nasazení v cílovém prostředí nebo existující obsah před skutečně provést žádné změny. Toto téma popisuje, jak můžete spustit nasazení "Co když" ke generování skriptů pro aktualizaci databáze a souborů protokolu, jako v případě, že obsah byl nasazen na cílové prostředí bez provedení změn ve skutečnosti. Analýza tyto prostředky můžete rozpoznat všechny potenciální problémy předem za provozu nasazení.
- [Přizpůsobení nasazení databáze pro prostředí s více](customizing-database-deployments-for-multiple-environments.md). Když nasadíte projekt databáze na více míst, můžete často přizpůsobit vlastnosti nasazení pro každé cílové prostředí. Například v testovacích prostředích je by obvykle znovu vytvořit databázi na každém nasazení, že v pracovním nebo produkčním prostředí by bylo mnohem vyšší pravděpodobnost, chcete-li zachovat data přírůstkové aktualizace. Toto téma popisuje, jak začlenit tyto změny vlastností do logika nasazení tak, že vytvoříte soubor konfigurace (.sqldeployment) konkrétní prostředí nasazení pro každé cílové prostředí.
- Nasazení databáze členství v rolích pro testovací prostředí. Když je znovu vytvořit databázi při každém nasazení&#x2014;například jako součást průběžnou integraci (CI) vytvořit a nasadit do testovacího prostředí&#x2014;obvykle budete muset nakonfigurovat pokaždé, když databáze členství v rolích. Například je obvykle potřeba udělit oprávnění k identitě fondu aplikací přidružené k webové aplikaci. Toto téma popisuje, jak tento proces můžete automatizovat přidáním SQL skriptu po nasazení do logiky nasazení.
- [Nasazení databáze členství do podnikových prostředích](deploying-membership-databases-to-enterprise-environments.md). Databáze členství ASP.NET mají různé vlastnosti, které může zkomplikovat procesu nasazení. Například pouze pro schéma nasazení bude ponechat databázi ve stavu, nefunkční. Ve většině scénářů je vhodnější k vytvoření databáze členství přímo v každém cílovém prostředí. Ale pokud máte nasazení databáze členství, toto téma popisuje některé z přístupů, které můžete použít k nápravě vyplývajících.
- [Vyloučení souborů a složek z nasazení](excluding-files-and-folders-from-deployment.md). V některých scénářích budete chtít přizpůsobit obsah do webového balíčku do určité cílové prostředí. Například můžete chtít zahrnují úplné verze knihoven jazyka JavaScript, při nasazení v testovacím prostředí, podpora ladění na straně klienta, ale minifikovaný verze knihoven můžete použít při nasazování k pracovním nebo produkčním prostředí. Toto téma popisuje, jak můžete vyloučit konkrétní soubory a složky z proces vytvoření balíčku.
- [Nasazení pořízení webových aplikací do offline režimu s Web](taking-web-applications-offline-with-web-deploy.md). Když nasadíte řešení k pracovním nebo produkčním prostředí, budete často chtít využít vaší webové aplikace do režimu offline po dobu trvání procesu nasazení. Toto téma popisuje, jak můžete přidat *aplikace\_offline.htm* souboru k vaší webové aplikaci na začátku procesu nasazení a odeberte jej na konci. Při *aplikace\_offline.htm* soubor je na místě, uživatelé, kteří přejděte do webové aplikace automaticky přesměrováni na *aplikace\_offline.htm* souboru.
- [Spuštěné skripty prostředí PowerShell systému Windows z nástroje MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Mnoho scénářů nasazení vyžadují složitější akce po nasazení, například přidávání zdroje vlastních událostí do registru nebo konfiguraci replikace mezi instancí systému SQL Server. Tyto akce jsou často dosáhnout pomocí skriptů prostředí Windows PowerShell. Toto téma popisuje, jak ke spouštění skriptů prostředí Windows PowerShell ze souboru projektu Microsoft Build Engine (MSBuild) jako součást procesu sestavení a nasazení.
- [Řešení potíží s proces balení](troubleshooting-the-packaging-process.md). Webové publikování kanálu (WPP) definuje MSBuild vlastnost s názvem **EnablePackageProcessLoggingAndAssert** , můžete použít ke generování podrobné informace o procesu vytváření balíčků pro projekty webových aplikací. Toto téma popisuje, co dělají vlastnost a způsobu jeho použití.

## <a name="key-technologies"></a>Klíčové technologie

Tento kurz se zaměřuje na tom, jak používat tyto produkty a technologie pro podporu automatizované sestavení a nasazení webu:

- Visual Studio 2010 a Team Foundation Server (TFS) 2010
- MSBuild a tým Build TFS
- Internetová informační služba (IIS) 7.5
- Nástroj pro nasazení webové služby IIS (Web Deploy) 2.1
- Nástroj pro nasazení databáze VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Další kurzy v této série

To je součástí ze série kurzů pět v podnikovém měřítku nasazení webu. Toto jsou další kurzy v této sérii:

- [Nasazení webové aplikace v podnikové scénáře](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Tento úvodní obsah poskytuje kontextové pozadí pro kurz řady. Popisuje kurz scénář a ukazuje, jak se úlohy a postupy popsané v celé řady začlenit do procesu širší Application Lifecycle Management (ALM).
- [Nasazení v podnikové síti webu](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). V tomto kurzu poskytuje koncepční Úvod do souborů projektu nástroje MSBuild, jako, Web Deploy a další související technologie. Vysvětluje, jak můžete pomocí těchto nástrojů současně ke správě procesů komplexní nasazení.
- [Konfigurace serveru prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Tento kurz popisuje postup konfigurace serverů Windows na podporu různých scénářů nasazení, včetně nasazení balíčku vzdáleném webovém pomocí agenta služba pro nasazení webu (vzdáleného agenta) nebo obslužné rutiny nasazení webu a nasazení vzdálené databáze. Na výběr metody nasazení pro vaše vlastní prostředí obsahuje pokyny a popisuje, jak použít k replikaci nasazených webových aplikací ve všech webových serverech ve farmě serverů webové farmy Framework (WFF).
- [Konfigurace serveru Team Foundation Server pro nasazení webu](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Tento kurz popisuje, jak nakonfigurovat produktu TFS na podporu různých scénářů nasazení, včetně automatického nasazení v rámci procesu položek konfigurace a ručně spustí nasazení konkrétní sestavení.

> [!div class="step-by-step"]
> [Next](performing-a-what-if-deployment.md)
