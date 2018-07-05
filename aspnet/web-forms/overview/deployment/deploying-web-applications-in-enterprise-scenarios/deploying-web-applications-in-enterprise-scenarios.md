---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Nasazení webové aplikace v podnikové scénáře pomocí sady Visual Studio 2010 | Dokumentace Microsoftu
author: jrjlee
description: Této sérii kurzů popisuje nástroje a techniky, které slouží k nasazování webových aplikací v různých podnikových scénářích. Vysvětluje, jak nejlépe používat...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 8412000e150f59911bb38f0147b1a487bef60c18
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376835"
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Nasazení webové aplikace v podnikové scénáře pomocí sady Visual Studio 2010
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Této sérii kurzů popisuje nástroje a techniky, které slouží k nasazování webových aplikací v různých podnikových scénářích. Vysvětluje, jak nejlépe používat technologie, jako je Visual Studio 2010, Microsoft Build Engine (MSBuild), Internetové informační služby (IIS) 7.5, nástroj nasazení webu služby IIS (Web Deploy), webové farmy Framework (WFF) a nástroje, jako je VSDBCMD.exe do Zjednodušte a spravovat proces nasazení. Obsahuje koncepční přehledy a orientovaných pokyny, které vám pomůžou:
> 
> - Zkontrolujte a stanovit požadavky na nasazení pro webovou aplikaci celého podniku.
> - Konfigurace testu, testovací a produkční prostředí serveru webové pro podporu nasazení webu.
> - Konfigurace procesů průběžné integrace (CI) Team Foundation Server (TFS) pro podporu nasazení automatizovaných webu.
> - Nasazení podnikových webových aplikací do různých serverových prostředích s různými požadavky a omezení.
> - Nasazení změny do webové aplikace, které jsou spuštěny v různých serverových prostředích.
> 
> > [!NOTE]
> > Zatímco tyto kurzy popisují použití TFS jako CI server, pokyny snadno přizpůsobit na libovolný server CI. Není nutné podrobnou znalost TFS k pochopení a využijte kurzy.
> 
> 
> Italština překlad těchto kurzů, navštivte web [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>O autorech

JASON Lee je hlavní vedoucí řízení s [hlavní obsah](http://www.contentmaster.com/) kde kterých pracuje s produkty společnosti Microsoft a technologie, zejména SharePoint a technologie ASP.NET, několik let. JASON obsahuje titul pH.d. ve výpočetním prostředí a je v současnosti MCPD a MCTS certified. Si můžete přečíst na Jason technický blog na [www.jrjlee.com](http://www.jrjlee.com/).

Robert kari je hlavní vedoucí řízení s [hlavní obsah](http://www.contentmaster.com/) kdo během své kariéry zapsala dokumenty White Paper, dokumentace k sadě SDK, Powerpointovými prezentacemi a školení vedených instruktorem a online kurzy. Původní člen týmu dokumentace technologie ASP.NET, pracoval s technologiemi web společnosti Microsoft pro víc než dekádu.

## <a name="target-audience"></a>Cílová skupina

Této sérii kurzů je pro vývojáře webových aplikací ASP.NET a architekty řešení, kteří používají Visual Studio 2010 pro vytváření webových aplikací podnikové úrovni. Chcete-li získat větší hodnotu z obsahu, by měl dobře pracovat pomocí sady Visual Studio 2010 a mít základní znalost sady TFS, společně povědomí o platformu Microsoft technologie jako je ASP.NET MVC 3, Windows Communication Foundation (WCF), služby IIS, SQL Server a databázové projekty sady Visual Studio. Není však nutné znát nástroje a technologie nasazení nebo potřebujete vědět, jak nastavit CI systémy.

## <a name="requirements"></a>Požadavky

Postupujte podle návody a provádět úlohy, které popisují tyto kurzy, budete muset nainstalovat tento software ve svém vývojovém počítači:

- Visual Studio 2010 Premium nebo Ultimate Edition s aktualizací Service Pack 1
- Rozhraní .NET framework 4.0
- Rozhraní .NET framework 3.5 s aktualizací Service Pack 1
- ASP.NET MVC 3.0
- Službu IIS 7.5 Express
- SQL Server Express 2008 R2

Pokud chcete provést nasazení kroků popsaných v rámci těchto návodů, budete potřebovat přístup k ukázkové webové aplikaci nasazení prostředí. Nejlepších výsledků dosáhnete by měly odrážet tato prostředí vaší organizace podnikové deployment vzor. Poté můžete upravit návodech uvedených v této dokumentaci tak, aby odrážely nasazení prostředí a požadavky vaší vlastní organizaci.

## <a name="series-contents"></a>Obsah řady

V této úvodní části skládá ze dvou dalších tématech. Ty jsou navržené k poskytnutí některých širší kontext pro kurzy, které následují:

- [Nasazení podnikového webu: Přehled scénářů](enterprise-web-deployment-scenario-overview.md). Toto téma popisuje scénář, který představuje základní kámen služby každý z kurzů této řady. Tento scénář se zaměřuje na požadavky Application Lifecycle Management (ALM) fiktivní společnosti s názvem společnosti Fabrikam, Inc., jak se vyvíjí webové aplikace podnikové úrovni.
- [Správa životního cyklu aplikace: Od vývoje k ostrému provozu](application-lifecycle-management-from-development-to-production.md). Toto téma obsahuje přehled procesu nasazení vysoké úrovně, začátku do konce. Ukazuje, jak společnosti Fabrikam, Inc. přejde podnikových webovou aplikaci ASP.NET pomocí testu, přípravného a produkčního prostředí jako součást procesu průběžného vývoje.

Obsahuje čtyři kurz sady. Každý se zaměřuje na různé aspekty nasazení webu:

- [Webové nasazení v podniku](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Tento kurz obsahuje koncepční Úvod do souborů projektu MSBuild, kanálu publikování webové, Web Deploy a dalších souvisejících technologiích. Vysvětluje, jak můžete pomocí těchto nástrojů společně ke správě nasazení komplexních procesů.
- [Konfigurace serverového prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Tento kurz popisuje, jak nakonfigurovat servery Windows pro podporu různých scénářů nasazení, včetně nasazení vzdáleného webového balíčku pomocí Služba agenta nasazení webu ("vzdáleného agenta") nebo obslužná rutina webového nasazení a nasazení vzdálené databáze. Poskytuje rady, jak volba metody nasazení pro konkrétní prostředí, a popisuje způsob použití WFF replikovat nasazených webových aplikací ve všech webových serverů v serverové farmě.
- [Konfigurace serveru Team Foundation Server pro nasazení webu](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Tento kurz popisuje postup konfigurace serveru TFS pro podporu různých scénářů nasazení, včetně automatizovaného nasazení jako součást procesu CI a ručně spustit nasazení konkrétního sestavení.
- [Pokročilé nasazení podnikového webu](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Tento kurz popisuje, jak k provádění různých rozšířené nasazení úkoly, jako je přizpůsobení nasazené databáze pro více prostředí, s výjimkou souborů a složek z nasazení a převedení webové aplikace do offline režimu během procesu nasazení .

## <a name="where-to-start"></a>Kde začít

Této sérii kurzů používá ukázkové řešení s realistické úroveň složitosti, společně s scénář nasazení fiktivní organizace, a zadejte referenční implementaci a poskytnout úkoly a návody pro běžné kontextu. Dalším tématu s názvem [nasazení podnikového webu: přehled scénářů](enterprise-web-deployment-scenario-overview.md), představuje scénář a ukázkové řešení. Odtud můžete pracovat prostřednictvím těchto kurzů a témata, které nejvíce odpovídají vašim potřebám.

> [!div class="step-by-step"]
> [Next](enterprise-web-deployment-scenario-overview.md)
