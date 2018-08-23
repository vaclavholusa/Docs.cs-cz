---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Scénář: Konfigurace provozního prostředí pro nasazení webu | Dokumentace Microsoftu'
author: jrjlee
description: Toto téma popisuje běžné webové scénář nasazení pro produkční prostředí a vysvětluje úlohy, které potřebujete k dokončení pro nastavení podobná...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 3627d18e1b102c574726282780239464be04c04b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753022"
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Scénář: Konfigurace provozního prostředí pro nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje běžné webové scénář nasazení pro produkční prostředí a vysvětluje úlohy, které potřebujete k dokončení pro nastavení prostředí podobné.


Produkčním prostředí je konečný cíl pro webovou aplikaci nebo Web. Pomocí tohoto bodu aplikace prošel testování, nasazení do přípravného prostředí a je připravená "za provozu." Charakteristiky produkčním prostředí může značně lišit podle povahu a účel webového obsahu, velikost vaší organizace, cílovou skupinu a spoustu dalších faktorů. Ve scénáři podnikové úrovni produkčním prostředí může mít tyto charakteristiky:

- Prostředí se skládá z více s vyrovnáváním zatížení webové servery a jeden nebo více databázových serverů, často a obsahují clustering převzetí služeb při selhání a zrcadlení databáze.
- Pokud prostředí je přístupem k Internetu, bude pravděpodobně odděleni od vaší interní síti. To může být v jiné podsíti v hraniční síti, může být v jiné doméně a může být na zcela jiné síťové infrastruktury.
- Vývojáři a účty sestavovací server procesu jsou vysoce nepravděpodobné, že máte oprávnění správce na provozních serverech.
- Změny aplikace se nasadí na základě méně často než testů nebo přípravných nasazení.

> [!NOTE]
> Horizontální navýšení kapacity nasazení databáze na více serverech je nad rámec tohoto kurzu. Další informace o této oblasti, obraťte se prosím [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).


Například v našem [kurz scénář](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Build server zahrnuje definice sestavení, které umožňují uživateli vytvořit řešení Správce kontaktů a nasazení do přípravného prostředí v jediném kroku. Když je aplikace připravená k nasazení do produkčního prostředí, z důvodu omezení stanovené požadavky na zabezpečení a síťové infrastruktury, musíte ručně zkopírovat webový balíček do produkční webový server a importovat správce produkčního prostředí ji pomocí Správce Internetové informační služby (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Přehled řešení

V tomto scénáři můžete odvodit z analýzu požadavky na nasazení tyto skutečnosti:

- Z důvodu omezení zabezpečení a konfiguraci sítě nelze nakonfigurovat produkčního prostředí pro podporu nasazení jedním kliknutím nebo automatizované. Offline nasazení je jediné možné přístup v tomto scénáři.
- V provozním prostředí obsahuje více webových serverů, takže webové farmy Framework (WFF) můžete použít k vytvoření serverové farmy. Tento přístup správce jenom je potřeba importovat aplikaci na jednom webovém serveru (primární server) a WFF replikuje nasazení na všech ostatních serverech web v provozním prostředí.

Tato témata poskytují všechny informace, které potřebujete k dokončení těchto úloh:

- [Vytvoření serverové farmy pomocí rozhraní Web Farm Framework](configuring-a-database-server-for-web-deploy-publishing.md). Toto téma popisuje, jak vytvořit a nakonfigurovat serverové farmy používající WFF, takže produkty webové platformy a komponenty, nastavení konfigurace a weby a aplikace se replikují napříč více s vyrovnáváním zatížení webových serverů.
- [Konfigurace webového serveru pro nasazení webu (Offline nasazení) publikování](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Toto téma popisuje, jak vytvořit webový server, která umožňuje správcům importovat a nasazení webových balíčků ručně, počínaje čisté sestavení systému Windows Server 2008 R2.
- [Konfigurovat databázový Server pro publikování nasazeného webu](configuring-a-database-server-for-web-deploy-publishing.md). Toto téma popisuje, jak konfigurovat databázový server pro podporu vzdáleného přístupu a nasazení, od výchozí instalaci systému SQL Server 2008 R2.

## <a name="further-reading"></a>Další čtení

Pokyny ke konfiguraci testovacího prostředí typické pro vývojáře najdete v tématu [scénář: Konfigurace testovací prostředí pro nasazení webu](scenario-configuring-a-test-environment-for-web-deployment.md). Pokyny k nastavení typické přípravné prostředí, najdete v části [scénář: Konfigurace pracovní prostředí pro nasazení webu](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Předchozí](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [další](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
