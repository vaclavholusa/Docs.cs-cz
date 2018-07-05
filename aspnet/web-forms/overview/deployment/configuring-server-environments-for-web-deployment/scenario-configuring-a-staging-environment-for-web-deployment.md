---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Scénář: Konfigurace přípravného prostředí pro nasazení webu | Dokumentace Microsoftu'
author: jrjlee
description: Toto téma popisuje běžné webové scénáře nasazení pro testovací prostředí a vysvětluje úlohy, které potřebujete k dokončení pro nastavení podobné env...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: fda4f056eebb77df3e93c63bdd4fabb071c0a0a9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365224"
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Scénář: Konfigurace přípravného prostředí pro nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje běžné webové scénáře nasazení pro testovací prostředí a vysvětluje úlohy, které potřebujete k dokončení pro nastavení prostředí podobné.


Velké organizace pomocí pracovního prostředí zobrazit náhled aktualizací webové aplikace nebo weby. Díky tomu uživatelé v organizaci možnost prozkoumat a než, přečtěte si nové funkce nebo obsah webu "místo pro live", nebo jinými slovy se nasadí do produkčního prostředí. Testovací prostředí je určen k co nejpřesněji replikovat provozní prostředí negace realistické ve verzi preview. Tento druh přípravné prostředí obvykle má tyto vlastnosti:

- Prostředí se skládá z více s vyrovnáváním zatížení webové servery a jeden nebo více databázových serverů, často a obsahují clustering převzetí služeb při selhání a zrcadlení databáze.
- Aplikace může nasadit ručně vývojový tým nebo automaticky pomocí serveru Team Build.
- Uživatelé nebo účty proces nasazení aplikací je nepravděpodobné, že má oprávnění správce na serverech pracovní.
- Změny aplikací se nasazují na základě časté, takže prostředí musí podporovat krokování nebo automatizovat nasazení.

> [!NOTE]
> Horizontální navýšení kapacity nasazení databáze na více serverech je nad rámec tohoto kurzu. Další informace o této oblasti, obraťte se prosím [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).


Například v našem [kurz scénář](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) spravuje řešení Správce kontaktů. Správce serveru TFS, Rob Walters vytvořil definici sestavení, která umožňuje vývojářům aktivovat nasazení do přípravného prostředí podle potřeby.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Všimněte si, že ve většině případů nebudete chtít nutně nasazení na nejnovější verzi do přípravného prostředí. Jste místo toho mnohem větší pravděpodobnost chcete nasadit konkrétní sestavení, které už provedl ověření a ověření v testovacím prostředí.

## <a name="solution-overview"></a>Přehled řešení

V tomto scénáři můžete odvodit z analýzu požadavky na nasazení tyto skutečnosti:

- Účtu uživatel nebo proces, který provede nasazení nebude mít oprávnění správce na pracovní serverech tak pracovní webové servery musí podporovat nasazení bez oprávnění správce. V důsledku toho budete muset nakonfigurovat pracovní webové servery, které chcete použít obslužné rutiny nasazení webu, nikoli vzdáleného agenta.
- Testovací prostředí obsahuje více webových serverů, ale musí podporovat nasazení jedním kliknutím nebo automatizované, takže budete muset použít webové farmy Framework (WFF) k vytvoření serverové farmy. Tento přístup můžete nasadit aplikaci do jednoho webový server (primární server) a WFF replikuje nasazení na všech ostatních serverech web v přípravném prostředí.
- Uživatel nebo proces účet, který provede nasazení musí mít oprávnění k vytvoření databáze. V důsledku toho budete muset přidat účet, který chcete **dbcreator** role serveru na serveru databáze, kromě konfigurace databázového serveru pro podporu vzdáleného přístupu a nasazení.

Tato témata poskytují všechny informace, které potřebujete k dokončení těchto úloh:

- [Vytvoření serverové farmy pomocí rozhraní Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md). Toto téma popisuje, jak vytvořit a nakonfigurovat serverové farmy používající WFF, takže produkty webové platformy a komponenty, nastavení konfigurace a weby a aplikace se replikují napříč více s vyrovnáváním zatížení webových serverů.
- [Konfigurace webového serveru pro publikování nasazeného webu (Obslužná rutina nasazení webu)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Toto téma popisuje, jak vytvořit web server, který podporuje nasazení webu publikování, pomocí agenta vzdáleného přístupu od čisté sestavení systému Windows Server 2008 R2.
- [Konfigurovat databázový Server pro publikování nasazeného webu](configuring-a-database-server-for-web-deploy-publishing.md). Toto téma popisuje, jak konfigurovat databázový server pro podporu vzdáleného přístupu a nasazení, od výchozí instalaci systému SQL Server 2008 R2.

## <a name="further-reading"></a>Další čtení

Pokyny ke konfiguraci testovacího prostředí typické pro vývojáře najdete v tématu [scénář: Konfigurace testovací prostředí pro nasazení webu](scenario-configuring-a-test-environment-for-web-deployment.md). Pokyny ke konfiguraci typickém produkčním prostředí najdete v tématu [scénář: Konfigurace provozního prostředí pro nasazení webu](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Předchozí](scenario-configuring-a-test-environment-for-web-deployment.md)
> [další](scenario-configuring-a-production-environment-for-web-deployment.md)
