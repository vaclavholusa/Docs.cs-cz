---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Scénář: Konfigurace testovacího prostředí pro nasazení webu | Dokumentace Microsoftu'
author: jrjlee
description: Toto téma popisuje běžné webové scénáře nasazení pro vývojáře nebo testovací prostředí a vysvětluje úlohy, které potřebujete k dokončení pro nastavení incidentech...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f8636fab82b63edab50fc13ae32f4dd536f133ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382491"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Scénář: Konfigurace testovacího prostředí pro nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje běžné webové scénář nasazení pro vývojáře nebo testovací prostředí a vysvětluje úlohy, které potřebujete k dokončení pro nastavení prostředí podobné.


Když vývojáři pracují na webové aplikace, jsou často budete poskytnut přístup k prostředí serveru, který můžete použít k testování změny do svých aplikací v reálné nastavení. Tento druh vývojové nebo testovací prostředí obvykle má tyto vlastnosti:

- Prostředí se skládá z jediného webového serveru a jednoho databázového serveru.
- Vývojáři obvykle oprávnění správce na serverech, aby mohly nakonfigurujte prostředí tak, aby požadavkům jejich aplikací.
- Změny aplikací se nasazují na základě časté, takže prostředí musí podporovat krokování nebo automatizovat nasazení.

Například v našem [kurz scénář](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink je pro vývojáře společnosti Fabrikam, Inc. Matt pracuje na řešení Správce kontaktů a pravidelně je potřeba nasadit změny do testovacího prostředí. Matt je správcem na webovém serveru testu a testovací databázový server. Matt na začátku musí být možné přímo nasadit řešení na testovací prostředí.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

Jako práce postupuje a další vývojáři připojit k týmu, kontaktujte správce řešení je nakonfigurovaný pro průběžnou integraci (CI) v Team Foundation Server (TFS). Pokaždé, když vývojář vrátí obsah, Team Build by měl sestavte řešení, spuštění všech testů jednotek a automaticky nasadit řešení na testovací prostředí.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Přehled řešení

Testovací prostředí musí podporovat krokování nebo automatizovat nasazení ze vzdáleného počítače, takže máte na výběr dva hlavní přístupy. Můžeš:

- Konfigurace serveru webových testů pro podporu nasazení pomocí Služba agenta nasazení webu ("vzdáleného agenta").
- Konfigurace serveru webových testů pro podporu nasazení pomocí obslužné rutiny pro nasazení webu.

> [!NOTE]
> Můžete také použít [nasazení webu na vyžádání](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("temp agent"). Toto je podobný přístup vzdáleného agenta z hlediska požadavky a omezení.


V tomto případě vývojáři oprávnění správce na cílových serverech a testovacího prostředí není předmětem pravidla striktní bezpečnostní omezení, tedy logickou volbou konfigurace serveru webového testu pro podporu nasazení pomocí vzdáleného agenta. To není tak složitý a vyžaduje menší počáteční nastavení, než obslužná rutina nasazení webového přístupu. Také budete muset konfigurovat databázový server pro podporu vzdáleného přístupu a nasazení.

Tato témata poskytují všechny informace, které potřebujete k dokončení těchto úloh:

- [Konfigurace webového serveru pro nasazení webu (vzdálený Agent) publikování](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Toto téma popisuje, jak vytvořit web server, který podporuje nasazení webu publikování, pomocí agenta vzdáleného přístupu od čisté sestavení systému Windows Server 2008 R2.
- [Konfigurovat databázový Server pro publikování nasazeného webu](configuring-a-database-server-for-web-deploy-publishing.md). Toto téma popisuje, jak konfigurovat databázový server pro podporu vzdáleného přístupu a nasazení, od výchozí instalaci systému SQL Server 2008 R2.

## <a name="further-reading"></a>Další čtení

Pokyny k nastavení typické přípravné prostředí, najdete v části [scénář: Konfigurace pracovní prostředí pro nasazení webu](scenario-configuring-a-staging-environment-for-web-deployment.md). Pokyny ke konfiguraci typickém produkčním prostředí najdete v tématu [scénář: Konfigurace provozního prostředí pro nasazení webu](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Předchozí](choosing-the-right-approach-to-web-deployment.md)
> [další](scenario-configuring-a-staging-environment-for-web-deployment.md)
