---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: "Nasazení sestavení konfigurace oprávnění pro Team | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje, jak nakonfigurovat oprávnění, aby umožňuje serveru sestavení pro nasazení obsahu pro webové servery a servery databázi jako součást automatizované b..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: cb3d013d69e36f97335ea31dd6e4997772ba2d8e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="configuring-permissions-for-team-build-deployment"></a>Konfigurace oprávnění pro tým nasazení sestavení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak nakonfigurovat oprávnění pro povolení vašem serveru sestavení pro nasazení obsahu pro webové servery a servery databázi jako součást procesu automatizované sestavení.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz series používá ukázkové řešení & #x 2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3 systému Windows Komunikační služby Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízené procesu sestavení dva projektu soubory & #x 2014; jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

Při instalaci služby Team Foundation Server (TFS) 2010 sestavení, zadejte identitu, se kterým chcete, aby služba běžela. Ve výchozím nastavení je to účet síťové služby. Alternativně můžete nakonfigurovat službu sestavení běžely pomocí účtu domény.

Všechny úlohy nasazení, které vyžadují ověřování systému Windows a plánujete automatizovat pomocí Team Build, se spustí pomocí identity služby sestavení. Jako takový budete muset udělit identita služby sestavení všechny požadované oprávnění pro databázové servery a webové servery.

> [!NOTE]
> Účet Network Service používá účet počítače k ověřování na jiné počítače. Účty počítače ve formě *[název domény]\[název počítače]***$**& #x 2014, například **FABRIKAM\TFSBUILD$**. Jako takový Pokud vaše sestavení služba spustí, používá identitu síťové služby, byste měli udělit žádná požadovaná oprávnění k identitě účet počítače pro váš server sestavení.


## <a name="configuring-web-server-permissions"></a>Konfigurace oprávnění webového serveru

Jak je popsáno v [výběr práva přístup k nasazení webu](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), existují dva hlavní přístupy, pokud chcete nasadit webových balíčků na vzdáleném webovém serveru, můžete použít:

- Nasaďte aplikaci ze vzdáleného umístění cílením *Služba agenta pro nasazení webu* (také označované jako vzdáleného agenta) na cílovém serveru.
- Nasaďte aplikaci ze vzdáleného umístění cílením *Internetová informační služba* (*IIS) obslužné rutiny nasazení webu* na cílovém serveru.

Vzdáleného agenta v tomto případě má dva klíče omezení:

- Vzdáleného agenta podporuje pouze ověřování NTLM. Jinými slovy nasazení musí používat identitu služby sestavení & #x 2014; nemůže zosobnit jiný účet.
- Pokud chcete používat vzdáleného agenta, musí být účet, který provádí nasazení správce na cílovém serveru.

Společně tyto dvě omezení zkontrolujte přístup vzdáleného agenta nežádoucího automatické vytvoření týmu nasazení. Pro tento postup, bude třeba, aby službu sestavení účtu správce na všechny cílové webové servery.

Obslužné rutiny nasazení webu přístup, nabízí různé výhody:

- Obslužné rutiny nasazení webu podporuje základní ověřování prostřednictvím protokolu HTTPS, které umožňuje předat přihlašovací údaje účtu alternativní nástroj nasazení webu služby IIS (Web Deploy).
- Můžete nakonfigurovat cílové webové servery do povolit uživatelům bez oprávnění správce pro nasazení obsahu pro konkrétní weby služby IIS pomocí obslužné rutiny nasazení webu.

V důsledku toho je vhodnější jasně cíle obslužné rutiny nasazení webu při automatizaci balíček nasazení webu z nástroje týmové sestavení. Toto je doporučený postup:

1. Vytvořte účet domény s nízkými oprávněními chcete použít pro nasazení.
2. Nakonfigurujte obslužné rutiny nasazení webu a udělte účtu požadovaná oprávnění pro nasazení obsahu pro konkrétní web služby IIS, jak je popsáno v [konfigurace webového serveru pro nasazení publikování na webu (webové nasazení obslužné rutiny)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Vyvolání nástroje nasazení webu a cílové obslužné rutiny nasazení webu, používá ověřování basic a poskytnutí pověření účtu domény, jste vytvořili, k provedení nasazení.

V [obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) ukázkové řešení, zadejte typ ověřování (základní nebo NTLM), přihlašovací údaje pro nasazení webu a adresa koncového bodu (vzdáleného agenta nebo obslužné rutiny nasazení webu) v souboru projektu konkrétní prostředí. Tyto hodnoty se používají k formulovali a spustit příkaz nástroje nasazení webu, když se spustí soubor projektu. Další informace najdete v tématu [nasazení webových balíčků](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Další informace o konfiguraci nasazení obslužné rutiny webu, včetně toho, jak nakonfigurovat oprávnění, najdete v části [konfigurace webového serveru pro nasazení publikování na webu (webové nasazení obslužné rutiny)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Další informace o konfiguraci vzdáleného agenta najdete v tématu [konfigurace webového serveru pro nasazení publikování na webu (vzdáleného agenta)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Konfigurace oprávnění databáze serveru

Chcete-li nasadit databázi na serveru SQL Server, postupujte takto:

- Vytvořte přihlašovací údaje pro nasazení účet na instanci serveru SQL.
- Udělit přihlášení **DBCreator** oprávnění na instanci serveru SQL.
- Po počátečním nasazení, přidejte přihlášení k **db\_vlastníka** role na cílovou databázi. To je potřeba, protože na následné nasazení, můžete se úprava existující databázi místo vytvoření nové databáze.

Můžete ověřovat pomocí ověřování protokolem NTLM nebo ověřování serveru SQL Server instanci systému SQL Server:

- Pokud používáte ověřování NTLM, budete muset udělit oprávnění k účtu služby sestavení popsané výše.
- Pokud používáte ověřování systému SQL Server, budete muset udělit oprávnění k účtu systému SQL Server popsané výše. Musíte taky zahrnout do připojovacího řetězce, který používáte pro nasazení databáze systému SQL Server uživatelské jméno a heslo.

Podrobné informace o tom, jak nakonfigurovat oprávnění pro nasazení databáze najdete v tématu [konfigurace databázový Server pro nasazení webového publikování](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Závěr

V tomto okamžiku byste měli porozumět oprávnění potřebná společně s otevřené, možnosti ověřování při automatizaci nasazení webové aplikace a databáze z nástroje týmové sestavení. Taky by měl být schopen implementovat potřebná oprávnění na webové servery služby IIS a na serverech databáze systému SQL Server.

## <a name="further-reading"></a>Další čtení

Další informace o konfiguraci serveru prostředí Windows pro podporu vzdáleného nasazení najdete v tématu [konfigurace serveru prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

>[!div class="step-by-step"]
[Předchozí](deploying-a-specific-build.md)
