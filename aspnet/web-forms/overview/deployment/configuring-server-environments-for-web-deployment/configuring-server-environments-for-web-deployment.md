---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Konfigurace serverového prostředí pro nasazení webu | Dokumentace Microsoftu
author: jrjlee
description: Tomto kurzu se dozvíte, jak nastavit prostředí server pro podporu jedním kliknutím, nebo automatizované, nasazení webu a publikování v různých různých scen...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: df40cc9d16b9e6b3fe0b659b405106e68cee62b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835560"
---
<a name="configuring-server-environments-for-web-deployment"></a>Konfigurace serverového prostředí pro nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Tomto kurzu se dozvíte, jak nastavit prostředí server pro podporu jedním kliknutím, nebo automatizované, nasazení webu a publikování v různých scénářích různé. Tento kurz zahrnuje témata vás provedl dokončením různé úkoly, jako je konfigurace webového serveru pro specifické přístupy k nasazení a nastavení farmu serverů webové farmy Framework (WFF) společně s přehledy založené na scénářích, které poskytují podporu vyšší úrovně začátku do konce průvodce.
> 
> V tomto kurzu použijete scénář nasazení společnosti Fabrikam, Inc., který je popsaný v [nasazení podnikového webu: přehled scénářů](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) jako referenční příklady a síťovou infrastrukturu.
> 
> Italština překlad těchto kurzů, navštivte web [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Tento kurz obsahuje následující témata:

- [Výběr správného přístupu k nasazení webu](choosing-the-right-approach-to-web-deployment.md)
- [Scénář: Konfigurace testovacího prostředí pro nasazení webu](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Scénář: Konfigurace přípravného prostředí pro nasazení webu](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Scénář: Konfigurace provozního prostředí pro nasazení webu](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Konfigurace webového serveru pro publikování nasazeného webu (vzdálený agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Konfigurace webového serveru pro publikování nasazeného webu (obslužná rutina nasazení webu)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Konfigurace webového serveru pro publikování nasazeného webu (offline nasazení)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Konfigurace databázového serveru pro publikování nasazeného webu](configuring-a-database-server-for-web-deploy-publishing.md)
- [Vytvoření serverové farmy na platformě Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md)
- [Konfigurace vlastností nasazeného cílového prostředí](configuring-deployment-properties-for-a-target-environment.md)

První téma [výběr právo přístupu k nasazení webu](choosing-the-right-approach-to-web-deployment.md), popisuje hlavní přístupy, můžete použít k publikování webových aplikací s použitím Internetové informační služby (IIS) nástroj pro nasazení webu (nasazení webu) 2.0. Také identifikuje scénáře, které se mapují k jednotlivým přístupům. Z tohoto místa každý scénář téma poskytuje základní přehled o úlohách, které potřebujete k dokončení a identifikuje témata, která budete muset projít při dokončení těchto úloh.

Pokud používáte přístup soubor projektu rozdělit podle [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md) k sestavení a nasazení vašeho řešení a poslední téma, [konfigurace vlastnosti nasazení pro cílové prostředí](configuring-deployment-properties-for-a-target-environment.md), popisuje, jak nakonfigurovat soubory projektu pro konkrétní prostředí pro nasazení do jiné cílové prostředí.

## <a name="key-technologies"></a>Klíčové technologie

Tento kurz se zaměřuje na tom, jak používat tyto produkty a technologie pro podporu nasazení webu:

- IIS 7.5
- Nasazení webu 2.x
- WFF 2.x
- Služba IIS služby webové správy (WMSvc)

Tento kurz se dotýká také při použití Windows serveru 2008 R2, SQL Server 2008 R2, technologii ASP.NET 4.0 a ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Další kurzy v této sérii

To je součástí série pěti kurzů na podnikové úrovni, nasazení webu. Toto jsou další kurzy v této sérii:

- [Nasazení webové aplikace v podnikových scénářích](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Tento úvodní obsah obsahuje kontextové informace týkající se série kurzů. Popisuje scénář tohoto kurzu a ukazuje, jak se úlohy a názorné postupy popsané v celé řadě vejde do širší proces správy životního cyklu aplikací (ALM).
- [Webové nasazení v podniku](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Tento kurz obsahuje koncepční Úvod do souborů projektu Microsoft Build Engine (MSBuild), kanálu publikování webové, Web Deploy a dalších souvisejících technologiích. Vysvětluje, jak můžete pomocí těchto nástrojů společně ke správě nasazení komplexních procesů.
- [Konfigurace serveru Team Foundation Server pro nasazení webu](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Tento kurz popisuje, jak nakonfigurovat Team Foundation Server (TFS) pro podporu různých scénářů nasazení, včetně automatizovaného nasazení jako součást procesu kontinuální integrace (CI) a ručně spustit nasazení konkrétního sestavení.
- [Pokročilé nasazení podnikového webu](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Tento kurz popisuje, jak k provádění různých rozšířené nasazení úkoly, jako je přizpůsobení nasazené databáze pro více prostředí, s výjimkou souborů a složek z nasazení a převedení webové aplikace do offline režimu během procesu nasazení .

> [!div class="step-by-step"]
> [Next](choosing-the-right-approach-to-web-deployment.md)
