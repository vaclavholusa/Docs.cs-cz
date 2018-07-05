---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Konfigurace oprávnění pro tým nasazení sestavení | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak nakonfigurovat oprávnění, které umožňují sestavení serveru nasazení obsahu do webové servery a databázové servery jako součást automatizovaného b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: f84e72bd5991b0407008ccdaff5243979cbb986e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384659"
---
<a name="configuring-permissions-for-team-build-deployment"></a>Konfigurace oprávnění pro tým nasazení sestavení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak nakonfigurovat oprávnění, které umožňují sestavení serveru nasazení obsahu do webové servery a databázové servery jako součást automatizovaného procesu sestavení.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

Při instalaci služby Team Foundation Server (TFS) 2010 sestavení, zadejte identitu, se kterým se má službu spustit. Ve výchozím nastavení je to účet síťové služby. Alternativně můžete nakonfigurovat službu sestavení ke spuštění pomocí účtu domény.

Úkoly nasazení, které vyžadují ověřování Windows a plánujete automatizovat pomocí Team Build, bude spuštěna s použitím identity služby sestavení. V důsledku toho budete muset udělit identitu služby sestavení jakékoli požadovaná oprávnění pro vaše webové servery a databázové servery.

> [!NOTE]
> Účet Network Service používá účet počítače pro ověření do jiných počítačů. Účty počítače mít podobu * [název domény]\[název počítače] ***$**&#x2014;například **FABRIKAM\TFSBUILD$**. V důsledku toho pokud je sestavovací služba spuštěná, používat identitu síťové služby, byste měli udělit libovolné požadovaná oprávnění pro identitu účtu počítače pro váš server sestavení.


## <a name="configuring-web-server-permissions"></a>Konfigurace oprávnění webového serveru

Jak je popsáno v [výběr právo přístupu k nasazení webu](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), existují dva hlavní přístupy, které můžete použít, pokud chcete nasadit webových balíčků na vzdáleném webovém serveru:

- Nasadit aplikaci ze vzdáleného umístění pomocí cílení *webová služba agenta nasazení* (označované také jako vzdálený agent) na cílovém serveru.
- Nasadit aplikaci ze vzdáleného umístění pomocí cílení *Internetová informační služba* (*služby IIS) obslužné rutiny webu nasadit* na cílovém serveru.

Vzdálený agent v tomto případě má dva klíče omezení:

- Vzdálený agent podporuje pouze ověřování NTLM. Jinými slovy, nasazení musí používat identitu služby sestavení&#x2014;nelze zosobnit jiného účtu.
- Použití vzdáleného agenta, musí být účet, který nasazení provádí správce na cílovém serveru.

Společně tyto dvě omezení Ujistěte se, vzdálený agent přístup nežádoucí pro automatické nasazení Team Build. Chcete-li tuto metodu použijte, je třeba službu sestavení účtu správce na všechny cílové webové servery.

Naproti tomu obslužná rutina nasazení webového přístupu nabízí různé výhody:

- Obslužné rutiny nasazení webu podporuje základní ověřování přes protokol HTTPS, který umožňuje předání přihlašovacích údajů účtu alternativní nástroj nasazení webu služby IIS (Web Deploy).
- Můžete nakonfigurovat cílové webové servery umožňuje uživatelům bez oprávnění správce pro nasazení obsahu pro konkrétní weby služby IIS pomocí obslužné rutiny nasazení webu.

V důsledku toho je vhodnější jasně cílit obslužné rutiny nasazení webu při automatizaci nasazení webového balíčku z nástroje týmové sestavení. Toto je doporučený postup:

1. Vytvořte účet domény s nízkým oprávněním, který chcete použít pro nasazení.
2. Nakonfigurujte obslužné rutiny nasazení webu a účtu udělit požadovaná oprávnění pro nasazení obsahu pro konkrétní web služby IIS, jak je popsáno v [konfigurace webového serveru pro nasazení publikování na webu (nasazení obslužná rutina webových)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Vyvolání Web Deploy a cílové obslužné rutiny nasazení webu, použití základního ověřování a poskytnutí přihlašovacích údajů účtu domény právě vytvořili, a provést nasazení.

V [Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) ukázkové řešení, určete typ ověřování (základní nebo NTLM), přihlašovací údaje pro nasazení webu a adresu koncového bodu (vzdálený agent nebo obslužné rutiny nasazení webu) v souboru projektu pro konkrétní prostředí. Tyto hodnoty slouží k formulování a spusťte příkaz Webdeploy při spuštění souboru projektu. Další informace najdete v tématu [nasazení webových balíčků](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Další informace o konfiguraci webové nasazení obslužnou rutinu, včetně postupu konfigurace oprávnění, najdete v části [konfigurace webového serveru pro nasazení publikování na webu (nasazení obslužná rutina webových)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Další informace o konfiguraci vzdáleného agenta najdete v tématu [konfigurace webového serveru pro nasazení publikování na webu (vzdálený Agent)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Konfigurace databáze serveru oprávnění

Nasazení databáze na SQL Server, musíte mít:

- Vytvořte přihlašovací údaje pro nasazení účtu v instanci systému SQL Server.
- Udělit přihlášení **DBCreator** oprávnění v instanci systému SQL Server.
- Po počátečním nasazení přidat přihlášení **db\_vlastníka** role na cílové databázi. To je povinné, protože na další nasazení, můžete se úprava existující databázi místo vytvoření nové databáze.

Můžete se ověřit do instance SQL serveru pomocí ověřování protokolem NTLM nebo ověřování serveru SQL Server:

- Pokud používáte ověřování protokolem NTLM, budete muset udělit oprávnění k účtu sestavovací služby je popsáno výše.
- Pokud používáte ověřování SQL serveru, musíte udělit oprávnění je popsáno výše pro účet systému SQL Server. Také musíte zahrnout připojovací řetězec, který používáte k nasazení databáze SQL serveru uživatelské jméno a heslo.

Podrobné informace o tom, jak nakonfigurovat oprávnění pro nasazení databáze, najdete v části [konfigurace databázového serveru pro publikování nasazení na webu](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Závěr

V tomto okamžiku byste měli rozumět oprávnění požadovaná spolu s open, možnosti ověřování při automatizaci nasazení webové aplikace a databáze z nástroje týmové sestavení. Také by měl být schopni implementovat potřebná oprávnění na webové servery služby IIS a servery databázového serveru SQL Server.

## <a name="further-reading"></a>Další čtení

Další informace o konfiguraci prostředí Windows serveru pro podporu vzdáleného nasazení najdete v tématu [konfigurace serveru prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Předchozí](deploying-a-specific-build.md)
