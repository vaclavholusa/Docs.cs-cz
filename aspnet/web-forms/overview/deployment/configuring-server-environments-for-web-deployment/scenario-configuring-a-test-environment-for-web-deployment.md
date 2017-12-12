---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: "Scénář: Konfigurace testovacího prostředí pro nasazení webu | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje scénář nasazení typické web pro vývojáře nebo testovací prostředí a popisuje úlohy, které potřebujete k dokončení, aby bylo možné nastavit serveru..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 008b9cd081152e6a378d0fa2e08497a6771fd9b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Scénář: Konfigurace testovacího prostředí pro nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje scénář nasazení typické web pro vývojáře nebo testovací prostředí a popisuje úlohy, které potřebujete k dokončení pro nastavení prostředí podobné.


Když vývojáři pracovat na webové aplikace, budou se často poskytnut přístup k prostředí serveru, který můžete použít k testování změny svých aplikací v reálných nastavení. Tento druh vývojovém nebo testovacím prostředí obvykle má tyto vlastnosti:

- Prostředí se skládá z jedné databáze serveru jednom webovém serveru.
- Vývojáři obvykle oprávnění správce na serverech, aby mohly nakonfigurovat prostředí tak, aby požadavky aplikací.
- Změny aplikace jsou nasazeny na základě časté, tak v prostředí musí podporovat jeden krok nebo automatizované nasazení.

Například v našem [kurz scénář](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink je vývojář společnosti Fabrikam, Inc. Matt pracuje na řešení obraťte se na správce a pravidelně je potřeba nasadit změny v testovacím prostředí. Matt je správce na serveru webových testů a testovací databázový server. Na začátku Matt musí být schopný nasadit řešení přímo do testovacího prostředí.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

Jako pracovní připojí postupuje a další vývojáři týmu, obraťte se na správce, řešení je nakonfigurován pro nepřetržitou integraci (CI) v Team Foundation Server (TFS). Vždy, když vývojář zkontroluje v obsahu, Team Build by měl sestavte řešení, spustit všechny testy jednotek a automaticky nasadit řešení do testovacího prostředí.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Přehled řešení

Testovací prostředí musí podporovat krokování nebo automatizované nasazení ze vzdáleného počítače, takže máte na výběr dva hlavní přístupy. Můžeš:

- Konfigurace serveru webových testů pro podporu nasazení pomocí agenta služba pro nasazení webu ("vzdáleného agenta").
- Konfigurace serveru webových testů pro podporu nasazení pomocí obslužná rutina nasazení webu.

> [!NOTE]
> Můžete také použít [nasazení webu na vyžádání](https://technet.microsoft.com/en-us/library/ee517345(WS.10).aspx) ("temp agent"). Toto je podobná přístup vzdáleného agenta z hlediska požadavky a omezení.


V takovém případě vývojáři mít oprávnění správce na cílovém serveru a testovací prostředí není předmětem omezení přísných bezpečnostních, tak, aby logické volbou konfigurace serveru webového testu pro podporu nasazení pomocí vzdáleného agenta. To je méně složitých a vyžaduje menší počáteční konfiguraci než přístup obslužné rutiny nasazení webu. Také budete muset konfigurovat databázový server pro podporu nasazení a vzdálený přístup.

Tato témata poskytují všechny informace, které potřebujete k dokončení těchto úloh:

- [Konfigurace webového serveru pro nasazení webu publikování (vzdáleného agenta)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Toto téma popisuje, jak sestavit webový server, který podporuje nasazení webu publikování, pomocí agenta vzdáleného přístupu od čisté sestavení Windows Server 2008 R2.
- [Konfigurovat databázový Server pro publikování nasazení webu](configuring-a-database-server-for-web-deploy-publishing.md). Toto téma popisuje postup konfigurace databázový server pro podporu nasazení od výchozí instalaci systému SQL Server 2008 R2 a vzdáleného přístupu.

## <a name="further-reading"></a>Další čtení

Pokyny týkající se konfigurace typické pracovní prostředí najdete v tématu [scénář: Konfigurace pracovní prostředí pro nasazení webu](scenario-configuring-a-staging-environment-for-web-deployment.md). Pokyny týkající se konfigurace typické provozním prostředí najdete v tématu [scénář: Konfigurace produkčním prostředí pro nasazení webu](scenario-configuring-a-production-environment-for-web-deployment.md).

>[!div class="step-by-step"]
[Předchozí](choosing-the-right-approach-to-web-deployment.md)
[další](scenario-configuring-a-staging-environment-for-web-deployment.md)
