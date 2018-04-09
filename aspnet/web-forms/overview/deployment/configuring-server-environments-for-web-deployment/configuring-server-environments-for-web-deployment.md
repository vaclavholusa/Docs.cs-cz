---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Konfigurace serveru prostředí pro nasazení webu | Microsoft Docs
author: jrjlee
description: Tento kurz vám ukáže, jak nastavit prostředí server pro podporu jedním kliknutím nebo automatizované, nasazení webu a publikování v různých různých scen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: ff6118be618a170ac76d66a9de24a7b5cc2d840a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="configuring-server-environments-for-web-deployment"></a>Konfigurace serveru prostředí pro nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Tento kurz vám ukáže, jak nastavit prostředí server pro podporu jedním kliknutím nebo automatizované, nasazení webu a publikování v různých scénářích různé. Tento kurz obsahuje témata, které vás provede procesem dokončení různé úkoly, jako jsou konfigurace webového serveru pro specifické přístupy k nasazení a nastavení webové farmy Framework (WFF) serverové farmy, společně s přehledy na základě scénáře, které poskytují podporu vyšší úrovně pokyny začátku do konce.
> 
> Tento kurz používá scénář nasazení společnosti Fabrikam, Inc., který je popsaný v [Enterprise nasazení webu: přehled scénářů](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) jako referenční bod příklady a síťové infrastruktury.
> 
> Italská překlad tyto kurzy, najdete v článku [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


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

První téma [výběr práva přístup k nasazení webu](choosing-the-right-approach-to-web-deployment.md), popisuje hlavní přístupy, můžete použít k publikování webových aplikací pomocí Internetové informační služby (IIS) nástroj pro nasazení webu, které je (Web Deploy) 2.0. Také identifikuje scénáře, které jsou mapovány na každý přístup. Z tohoto místa každý scénář téma obsahuje souhrnné informace o úlohách, které potřebujete k dokončení a identifikuje témata, které budete potřebovat fungovat prostřednictvím vám pomohou dokončit tyto úlohy.

Pokud používáte popsaný přístup souboru projektu rozdělení [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md) k vytváření a nasazování řešení konečné tématu [konfigurace vlastnosti nasazení pro cílové prostředí](configuring-deployment-properties-for-a-target-environment.md), popisuje postup konfigurace souborů projektu konkrétní prostředí pro nasazení do jiné cílové prostředí.

## <a name="key-technologies"></a>Klíčové technologie

Tento kurz se zaměřuje na tom, jak používat tyto produkty a technologie pro podporu nasazení webu:

- SLUŽBU IIS 7.5
- Nasazení webu 2.x
- WFF 2.x
- Služba IIS webové správy (WMSvc)

Tento kurz dotykem také na stránce použít Windows Server 2008 R2, SQL Server 2008 R2, technologii ASP.NET 4.0 a ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Další kurzy v této série

To je součástí ze série kurzů pět v podnikovém měřítku nasazení webu. Toto jsou další kurzy v této sérii:

- [Nasazení webové aplikace v podnikové scénáře](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Tento úvodní obsah poskytuje kontextové pozadí pro kurz řady. Popisuje kurz scénář a ukazuje, jak se úlohy a postupy popsané v celé řady začlenit do procesu širší Application Lifecycle Management (ALM).
- [Nasazení v podnikové síti webu](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). V tomto kurzu poskytuje koncepční Úvod k Microsoft Build Engine (MSBuild) soubory projektu, kanálu publikování webové, Web Deploy a další související technologie. Vysvětluje, jak můžete pomocí těchto nástrojů současně ke správě procesů komplexní nasazení.
- [Konfigurace serveru Team Foundation Server pro nasazení webu](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Tento kurz popisuje, jak nakonfigurovat Team Foundation Server (TFS) pro podporu různých scénářů nasazení, včetně automatického nasazení v rámci procesu průběžnou integraci (CI) a ručně spustí nasazení konkrétní sestavení.
- [Pokročilé nasazení webu Enterprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Tento kurz popisuje, jak k provádění různých dalších pokročilých úloh nasazení, jako vlastní nastavení nasazení databáze pro prostředí s více, vyloučení souborů a složek z nasazení a přepnutím do režimu offline webové aplikace během procesu nasazení .

> [!div class="step-by-step"]
> [Next](choosing-the-right-approach-to-web-deployment.md)
