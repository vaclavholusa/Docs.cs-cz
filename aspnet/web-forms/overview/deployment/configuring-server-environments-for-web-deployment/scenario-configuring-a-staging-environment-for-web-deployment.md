---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: "Scénář: Konfigurace pracovní prostředí pro nasazení webu | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje typické webové scénář nasazení pro pracovní prostředí a popisuje úlohy, které potřebujete k dokončení pro nastavení podobné env..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: b5f223f59a8b222f4f01322d228cf7434e3dfc14
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Scénář: Konfigurace pracovní prostředí pro nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje typické webové scénář nasazení pro pracovní prostředí a popisuje úlohy, které potřebujete k dokončení pro nastavení prostředí podobné.


Mnoha organizací pracovní prostředí slouží k zobrazení náhledu aktualizací do webové aplikace nebo weby. Díky tomu uživatelé v organizaci šanci na zkoumat a zkontrolujte nové funkce nebo obsah před webu "místo za provozu" nebo jinými slovy se nasazení v produkčním prostředí. Je pracovní prostředí je určen k replikovat provozní prostředí co nejblíže, chcete-li provést realistické preview. Tento druh pracovní prostředí obvykle má tyto vlastnosti:

- V prostředí se skládá z několika Vyrovnávání zatížení webové servery a jeden nebo více databázové servery, často se clustering převzetí služeb při selhání a zrcadlení databáze.
- Může být aplikace nasazeny ručně vývojový tým nebo automaticky serverem Team Build.
- Proces účty, které nasazujete, aplikace uživatelů nebo je nepravděpodobné, že oprávnění správce na serveru pracovní.
- Změny aplikace jsou nasazeny na základě časté, tak v prostředí musí podporovat jeden krok nebo automatizované nasazení.

> [!NOTE]
> Škálování databáze nasazení více serverů je nad rámec tohoto kurzu. Další informace o této oblasti, přečtěte si [SQL Server Books Online](https://technet.microsoft.com/en-us/library/ms130214.aspx).


Například v našem [kurz scénář](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) spravuje řešení obraťte se na správce. Správce sady TFS, Rob Walters vytvořil definici sestavení, která umožňuje vývojářům aktivovat nasazení pro pracovní prostředí podle potřeby.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Všimněte si, že ve většině případů nebude chcete nutně nasadit nejnovější sestavení do fázovacího prostředí. Jste místo toho mnohem vyšší pravděpodobnost, který chcete nasadit konkrétní sestavení, která již došlo k ověření a ověření v testovacím prostředí.

## <a name="solution-overview"></a>Přehled řešení

V tomto scénáři můžete odvodit z analýzu požadavky na nasazení tyto skutečnosti:

- Uživatel nebo proces účet, který provádí nasazení nebude mít oprávnění správce na serveru pracovní tak pracovní webové servery musí podporovat nasazení bez oprávnění správce. Jako takový budete muset nakonfigurovat pracovní webových serverů k používání obslužné rutiny nasazení webu, nikoli vzdáleného agenta.
- Pracovní prostředí obsahuje více webových serverů, ale musí pro podporu nasazení jedním kliknutím nebo automatizované, takže budete muset použít webové farmy Framework (WFF) k vytvoření serverové farmy. Tento přístup můžete nasadit aplikace do jednoho webového serveru (primární server) a replikují se tak WFF nasazení na všechny ostatní servery webové v testovacím prostředí.
- Uživatel nebo proces účet, který provádí nasazení musí mít oprávnění k vytváření databází. Jako takový, budete muset přidat účet, který **dbcreator** role serveru na serveru databáze, kromě konfigurace databázový server pro podporu nasazení a vzdálený přístup.

Tato témata poskytují všechny informace, které potřebujete k dokončení těchto úloh:

- [Vytvoření serverové farmy pomocí rozhraní Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md). Toto téma popisuje, jak vytvořit a nakonfigurovat serverové farmy používající WFF, takže produkty webové platformy a součásti, nastavení konfigurace a weby a aplikace se replikují napříč více Vyrovnávání zatížení sítě webových serverů.
- [Konfigurace webového serveru pro publikování nasazení webu (Web Deploy obslužné rutiny)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Toto téma popisuje, jak sestavit webový server, který podporuje nasazení webu publikování, pomocí agenta vzdáleného přístupu od čisté sestavení Windows Server 2008 R2.
- [Konfigurovat databázový Server pro publikování nasazení webu](configuring-a-database-server-for-web-deploy-publishing.md). Toto téma popisuje postup konfigurace databázový server pro podporu nasazení od výchozí instalaci systému SQL Server 2008 R2 a vzdáleného přístupu.

## <a name="further-reading"></a>Další čtení

Pokyny týkající se konfigurace typické vývojáře testovacím prostředí najdete v tématu [scénář: Konfigurace testovací prostředí pro nasazení webu](scenario-configuring-a-test-environment-for-web-deployment.md). Pokyny týkající se konfigurace typické provozním prostředí najdete v tématu [scénář: Konfigurace produkčním prostředí pro nasazení webu](scenario-configuring-a-production-environment-for-web-deployment.md).

>[!div class="step-by-step"]
[Předchozí](scenario-configuring-a-test-environment-for-web-deployment.md)
[další](scenario-configuring-a-production-environment-for-web-deployment.md)
