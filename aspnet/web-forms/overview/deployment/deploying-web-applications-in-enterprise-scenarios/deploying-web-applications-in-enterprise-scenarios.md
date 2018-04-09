---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Nasazení webové aplikace v podnikové scénáře použití sady Visual Studio 2010 | Microsoft Docs
author: jrjlee
description: Tato sada kurzů popisuje nástroje a techniky, které můžete použít k nasazení webové aplikace v různých podnikových scénářích. Vysvětluje, jak nejlépe použít...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 921b1ccd8a1f2109a51f3f75149588422fefb91d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Nasazení webové aplikace v podnikové scénáře použití sady Visual Studio 2010
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Tato sada kurzů popisuje nástroje a techniky, které můžete použít k nasazení webové aplikace v různých podnikových scénářích. Vysvětluje, jak využívat nejlépe technologií, jako je Visual Studio 2010, Microsoft Build Engine (MSBuild), Internetové informační služby (IIS) 7.5, nástroj nasazení webu služby IIS (Web Deploy), webové farmy Framework (WFF) a nástroje, jako je VSDBCMD.exe do zjednodušit a spravovat proces nasazení. Obsahuje koncepční přehled a pokyny k orientované na úlohy, které vám pomůže:
> 
> - Zkontrolujte a vytvořit požadavky na nasazení pro webovou aplikaci podnikovém měřítku.
> - Konfigurace testovací, přípravné nebo produkční prostředí webové prostředí server pro podporu nasazení webu.
> - Konfigurace procesů průběžnou integraci (CI) Team Foundation Server (TFS) pro podporu nasazení automatizované webu.
> - Nasaďte podnikovém měřítku webových aplikací na jiný server prostředí s různými požadavky a omezení.
> - Nasaďte změny do webové aplikace, které běží v prostředí jiný server.
> 
> > [!NOTE]
> > Při tyto kurzy popisují použití sady TFS jako CI server, je snadno přizpůsobit pokyny k žádnému serveru položek konfigurace. Nepotřebujete podrobné údaje o TFS pochopit a využívat kurzů k.
> 
> 
> Italská překlad tyto kurzy, najdete v článku [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>O autorů

JASON Lee je hlavní technologist s [obsahu hlavní](http://www.contentmaster.com/) kde kterých pracuje s produkty společnosti Microsoft a technologie, zejména služby SharePoint a technologii ASP.NET, několik let. JASON obsahuje aplikaci PhotoDraw v oblasti výpočetních a je aktuálně MCPD a MCTS certifikaci. Si můžete přečíst na Jason technické blog na [www.jrjlee.com](http://www.jrjlee.com/).

Robert kari je hlavní technologist s [obsahu hlavní](http://www.contentmaster.com/) kdo během jeho kariérní zápisem dokumenty White Paper, dokumentaci k sadě SDK, Powerpointové prezentace a kurzů vedených lektorem a online školení. Původní člen týmu dokumentace pro technologie ASP.NET, že pracoval s technologiemi web společnosti Microsoft pro přes deset.

## <a name="target-audience"></a>Cílové skupiny

Tato sada kurzy je pro vývojáře webových aplikací ASP.NET a řešení architektům, kteří používají Visual Studio 2010 vytvářet podnikovém měřítku webové aplikace. Chcete-li získat maximální hodnotu z obsahu, by měla být možnost pomocí sady Visual Studio 2010 a mít základní znalost sady TFS, společně s je třeba znát Microsoft webové platformy technologie ASP.NET MVC 3, Windows Communication Foundation (WCF), služby IIS, SQL Server a Visual Studio databázové projekty. Však není nutné znát nástroje a technologie nasazení nebo potřebujete vědět, jak nastavit CI systémy.

## <a name="requirements"></a>Požadavky

Postupujte podle názorné postupy a provádět úlohy, které popisují tyto kurzy, budete potřebovat k instalaci tohoto softwaru na vašem vývojovém počítači:

- Visual Studio 2010 Premium nebo Ultimate Edition s aktualizací Service Pack 1
- Rozhraní .NET framework 4.0
- Rozhraní .NET framework 3.5 s aktualizací Service Pack 1
- ASP.NET MVC 3.0
- Služba IIS 7.5 Express
- SQL Server Express 2008 R2

Pokud chcete provést nasazení kroků popsaných v rámci tyto postupy, musíte mít přístup k ukázkové webové aplikace nasazení prostředí. Nejlepších výsledků dosáhnete by měla odpovídat těchto prostředích vzor nasazení enterprise vaší organizace. Poté můžete upravit návody uvedených v této dokumentaci tak, aby odrážela nasazení prostředí a požadavky vaší vlastní organizaci.

## <a name="series-contents"></a>Obsah řady

Tato úvodní část zahrnuje dva další témata. Tyto jsou určená k poskytnutí některých širší kontext pro kurzy, které následují:

- [Podnikového nasazení webu: Přehled scénáře](enterprise-web-deployment-scenario-overview.md). Toto téma popisuje scénář, který základem každý z této série kurzů. Tento scénář se zaměřuje na požadavky Application Lifecycle Management (ALM) fiktivní společnost s názvem Fabrikam, Inc. vývoji situace webovou aplikaci podnikovém měřítku.
- [Správa životního cyklu aplikací: Z vývojového do produkčního prostředí](application-lifecycle-management-from-development-to-production.md). Toto téma obsahuje přehled vysoké úrovně, začátku do konce procesu nasazení. Ukazuje, jak společnost Fabrikam A.s. přesune podnikovém měřítku webovou aplikaci ASP.NET pomocí prostředí testovací, přípravné nebo produkční prostředí v rámci procesu průběžné vývoj.

Řady zahrnuje čtyři kurz sady. Každý se zaměřuje na různé aspekty nasazení webu:

- [Nasazení v podnikové síti webu](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). V tomto kurzu poskytuje koncepční Úvod do souborů projektu nástroje MSBuild, kanálu publikování webové, Web Deploy a další související technologie. Vysvětluje, jak můžete pomocí těchto nástrojů současně ke správě procesů komplexní nasazení.
- [Konfigurace serveru prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Tento kurz popisuje postup konfigurace serverů Windows na podporu různých scénářů nasazení, včetně nasazení balíčku vzdáleného webu pomocí agenta služba pro nasazení webu ("vzdáleného agenta") nebo obslužné rutiny nasazení webu a nasazení vzdálené databáze. Poskytuje pokyny na Vybrat metodu nasazení, která pro vaše vlastní prostředí, a popisuje postup použití WFF k replikaci nasazených webových aplikací mezi všechny webové servery ve farmě serverů.
- [Konfigurace serveru Team Foundation Server pro nasazení webu](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Tento kurz popisuje, jak nakonfigurovat produktu TFS na podporu různých scénářů nasazení, včetně automatického nasazení v rámci procesu položek konfigurace a ručně spustí nasazení konkrétní sestavení.
- [Pokročilé nasazení webu Enterprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Tento kurz popisuje, jak k provádění různých dalších pokročilých úloh nasazení, jako vlastní nastavení nasazení databáze pro prostředí s více, vyloučení souborů a složek z nasazení a přepnutím do režimu offline webové aplikace během procesu nasazení .

## <a name="where-to-start"></a>Kde začít

Tato sada kurzy používá ukázkové řešení s úrovní realistické složitější, společně s scénář nasazení fiktivních enterprise, a zadejte odkaz na implementaci a dát úlohy a návody pro běžné kontextu. Dalším tématu [Enterprise nasazení webu: přehled scénářů](enterprise-web-deployment-scenario-overview.md), představuje scénář a ukázkové řešení. Odtud můžete pracovat prostřednictvím kurzy a témata, které nejvíce odpovídají vašim potřebám.

> [!div class="step-by-step"]
> [Next](enterprise-web-deployment-scenario-overview.md)
