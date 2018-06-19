---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Scénář: Konfigurace produkčním prostředí pro nasazení webu | Microsoft Docs'
author: jrjlee
description: Toto téma popisuje typické webové scénář nasazení pro produkční prostředí a popisuje úlohy, které potřebujete k dokončení pro nastavení podobné...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4de5b1f20f3adcb53765c7cb9765c0d90a80e677
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882943"
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Scénář: Konfigurace produkčním prostředí pro nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje typické webové scénář nasazení pro produkční prostředí a popisuje úlohy, které potřebujete k dokončení pro nastavení prostředí podobné.


V provozním prostředí je konečným cílem pro webové aplikace nebo webu. Pomocí tohoto bodu aplikace byly testování, byla nasazena do pracovního prostředí a je připraven k "přejděte za provozu." Charakteristiky produkčního prostředí můžou široce lišit podle podstatu a účel webového obsahu, velikosti vaší organizace, cílovou skupinu a spoustu dalších faktorů. Ve scénáři s podnikovém měřítku produkčním prostředí může mít tyto vlastnosti:

- V prostředí se skládá z několika Vyrovnávání zatížení webové servery a jeden nebo více databázové servery, často se clustering převzetí služeb při selhání a zrcadlení databáze.
- Pokud prostředí je přístupný z Internetu, je pravděpodobně být oddělené z vaší interní sítě. Může být v jiné podsíti v hraniční síti, může být v jiné doméně a může být na infrastruktuře zcela jinou síť.
- Vývojáři a účty procesu sestavení serveru je vysoce nepravděpodobné, že oprávnění správce na provozních serverech.
- Na základě méně často než testu nebo pracovní nasazení se nasadí změny aplikace.

> [!NOTE]
> Škálování databáze nasazení více serverů je nad rámec tohoto kurzu. Další informace o této oblasti, přečtěte si [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).


Například v našem [kurz scénář](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), server Team Build obsahuje definice sestavení, které umožňují uživatelům sestavte řešení, obraťte se na správce a nasadit ji do pracovního prostředí v jediném kroku. Když je aplikace připravená k nasazení do produkčního prostředí, a to z důvodu omezení způsobené požadavky na zabezpečení a síťové infrastruktury, správce produkčního prostředí musíte ručně zkopírovat webového balíčku na produkční webový server a importovat ji pomocí Správce Internetové informační služby (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Přehled řešení

V tomto scénáři můžete odvodit z analýzu požadavky na nasazení tyto skutečnosti:

- Z důvodu omezení zabezpečení a konfiguraci sítě nelze nakonfigurovat produkčního prostředí pro podporu jedním kliknutím nebo automatické nasazení. Offline nasazení je pouze přijatelná přístup v tomto scénáři.
- V provozním prostředí obsahuje více webových serverů, takže webové farmy Framework (WFF) můžete použít k vytvoření serverové farmy. Použití tohoto přístupu, stačí pouze importovat aplikaci na jednom webovém serveru (primární server) a replikují se tak WFF nasazení na všechny další webové servery v provozním prostředí.

Tato témata poskytují všechny informace, které potřebujete k dokončení těchto úloh:

- [Vytvoření serverové farmy pomocí rozhraní Web Farm Framework](configuring-a-database-server-for-web-deploy-publishing.md). Toto téma popisuje, jak vytvořit a nakonfigurovat serverové farmy používající WFF, takže produkty webové platformy a součásti, nastavení konfigurace a weby a aplikace se replikují napříč více Vyrovnávání zatížení sítě webových serverů.
- [Konfigurace webového serveru pro nasazení webu publikování (Offline nasazení)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Toto téma popisuje, jak sestavit webový server umožňující správci importujte a nasaďte webových balíčků ručně, od čisté sestavení Windows Server 2008 R2.
- [Konfigurovat databázový Server pro publikování nasazení webu](configuring-a-database-server-for-web-deploy-publishing.md). Toto téma popisuje postup konfigurace databázový server pro podporu nasazení od výchozí instalaci systému SQL Server 2008 R2 a vzdáleného přístupu.

## <a name="further-reading"></a>Další čtení

Pokyny týkající se konfigurace typické vývojáře testovacím prostředí najdete v tématu [scénář: Konfigurace testovací prostředí pro nasazení webu](scenario-configuring-a-test-environment-for-web-deployment.md). Pokyny týkající se konfigurace typické pracovní prostředí najdete v tématu [scénář: Konfigurace pracovní prostředí pro nasazení webu](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Předchozí](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [další](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
