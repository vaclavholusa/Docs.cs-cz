---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: "Konfigurace serveru Team Foundation Server pro nasazení webu | Microsoft Docs"
author: jrjlee
description: "Tento kurz vám ukáže, jak nakonfigurovat Team Foundation Server (TFS) 2010 sestavení řešení a nasazení webového obsahu na různé cílové prostředí. To..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 72f60841a1381380c0ea6167077420f960180dc7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Konfigurace serveru Team Foundation Server pro nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Tento kurz vám ukáže, jak nakonfigurovat Team Foundation Server (TFS) 2010 sestavení řešení a nasazení webového obsahu na různé cílové prostředí. To zahrnuje scénáře průběžnou integraci (CI), kde nasadíte obsah automaticky pokaždé, když vývojář provede změny. Může také obsahovat ruční aktivační událost scénáře, kde může správce pro aktivaci nasazení konkrétní sestavení do pracovního prostředí po sestavení byl ověřen a ověřit v testovacím prostředí. Témata v tomto kurzu vás provede proces celé konfigurace, včetně:
> 
> - Postup vytvoření nového týmového projektu v sadě TFS.
> - Postup přidání obsahu do správy zdrojového kódu.
> - Postup konfigurace serveru sestavení pro podporu položek konfigurace a nasazení.
> - Jak vytvořit definici sestavení, která obsahuje logiku nasazení.
> - Jak nakonfigurovat oprávnění pro automatické nasazení.
> 
> Italská překlad tyto kurzy, najdete v článku [http://www.lucamorelli.it](http://www.lucamorelli.it).


Tento kurz předpokládá, že máte nainstalované sady TFS 2010 a vytvořit kolekci týmových projektů v rámci procesu počáteční konfigurace. [Team Foundation Průvodce instalací pro Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) poskytuje komplexní pokyny v těchto úloh.

## <a name="context"></a>Kontext

To je součástí ze série kurzů v závislosti na požadavcích nasazení enterprise fiktivní společnosti s názvem Fabrikam, Inc. Tento kurz series používá ukázkové řešení & #x 2014; [obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) řešení & #x 2014; představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3 systému Windows Komunikační služby Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md), ve které je řízené procesu sestavení dva projektu soubory & #x 2014; jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="scenario-overview"></a>Přehled scénáře

Tento scénář vysoké úrovně pro tyto kurzy je popsaná v [Enterprise nasazení webu: přehled scénářů](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Doporučujeme si přečíst toto téma, než začnete tento kurz.

## <a name="how-to-use-this-tutorial"></a>Jak používat v tomto kurzu

Pokud je to poprvé, které jste provedli úkoly popsané v tomto kurzu, nebo pokud chcete použít postup popsaný v příkladech použití ukázkové řešení, měli nejdřív projít kurz témata v pořadí. Alternativně můžete použít jednotlivé témata jako pokyny pro konkrétní úlohy. Tento kurz obsahuje následující témata:

- [Vytvoření týmového projektu v sadě TFS](creating-a-team-project-in-tfs.md). Týmového projektu je základní jednotkou správy zdrojového kódu, Správa procesů a sestavení v sadě TFS. Potřebujete k vytvoření týmového projektu, než budete moct přidat obsahu ovládací prvek zdroje nebo vytvoření definice sestavení.
- [Přidávání obsahu do správy zdrojového kódu](adding-content-to-source-control.md). Po vytvoření týmového projektu, můžete začít přidávat obsah do správy zdrojového kódu. Budete muset přidat své projekty a řešení, společně s žádné externí závislosti, před zahájením konfigurace sestavení.
- [Konfigurace sady TFS sestavení serveru pro nasazení webu](configuring-a-tfs-build-server-for-web-deployment.md). Pokud chcete k vytvoření týmového projektu obsahu, musíte nakonfigurovat server sestavení. Ve většině případů to by měl být na samostatný počítač z instalace sady TFS. Chcete-li nakonfigurovat server sestavení, je potřeba nainstalovat a konfigurovat službu sestavení sady TFS, nainstalujte Visual Studio 2010, vytvořit sestavení řadiče a agentů sestavení, nainstalovat všechny produkty nebo součásti, které potřebuje váš kód, aby sestavení úspěšně a nainstalovat Internetová informační služba (IIS) webové nástroje pro nasazení (Web Deploy).
- [Vytvoření definice sestavení, který podporuje nasazení](creating-a-build-definition-that-supports-deployment.md). Před zahájením služby Řízení front nebo spuštění sestavení v sadě TFS, musíte vytvořit alespoň jednu sestavení definice pro týmový projekt. Definici sestavení definuje všechny aspekty sestavení, včetně věcí, které by měl být součástí sestavení, co by měly aktivovat sestavení a kde Team Build by měli poslat výstupy sestavení. Můžete nakonfigurovat definice sestavení ke spuštění vlastní soubory projektu Microsoft Build Engine (MSBuild), který umožňuje zahrnout nasazení logiku v automatizovaných sestaveních.
- [Nasazení konkrétní sestavení](deploying-a-specific-build.md). V celá řada možných scénářů budete chtít nasadit konkrétní sestavení, nikoli na nejnovější verzi, na cílovém prostředí. V takovém případě můžete nakonfigurovat definice buildu, která nasadí obsah ze složky konkrétní vyřaďte.
- [Konfigurace oprávnění pro tým nasazení sestavení](configuring-permissions-for-team-build-deployment.md). Pokud službu sestavení pro nasazení obsahu v rámci procesu automatizované sestavení, budete muset udělit různé oprávnění k účtu služby sestavení na všechny cílové webové servery a servery databáze.

## <a name="key-technologies"></a>Klíčové technologie

Tento kurz se zaměřuje na tom, jak používat tyto produkty a technologie pro podporu automatizované sestavení a nasazení webu:

- Visual Studio Team Foundation Server 2010
- Týmovém sestavení a nástroje MSBuild
- Web Deploy

Tento kurz dotykem také na stránce použít Windows Server 2008 R2, IIS 7.5, SQL Server 2008 R2, technologii ASP.NET 4.0 a ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Další kurzy v této série

To je součástí ze série kurzů pět v podnikovém měřítku nasazení webu. Toto jsou další kurzy v této sérii:

- [Nasazení webové aplikace v podnikové scénáře](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Tento úvodní obsah poskytuje kontextové pozadí pro kurz řady. Popisuje kurz scénář a ukazuje, jak se úlohy a postupy popsané v celé řady začlenit do procesu širší Application Lifecycle Management (ALM).
- [Nasazení v podnikové síti webu](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). V tomto kurzu poskytuje koncepční Úvod do souborů projektu nástroje MSBuild, webových publikování kanálu (WPP), nasazení webu a další související technologie. Vysvětluje, jak můžete pomocí těchto nástrojů současně ke správě procesů komplexní nasazení.
- [Konfigurace serveru prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Tento kurz popisuje postup konfigurace serverů Windows na podporu různých scénářů nasazení, včetně nasazení balíčku vzdáleném webovém pomocí agenta služba pro nasazení webu (vzdáleného agenta) nebo obslužné rutiny nasazení webu a nasazení vzdálené databáze. Na výběr metody nasazení pro vaše vlastní prostředí obsahuje pokyny a popisuje, jak použít k replikaci nasazených webových aplikací ve všech webových serverech ve farmě serverů webové farmy Framework (WFF).
- [Pokročilé nasazení webu Enterprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Tento kurz popisuje, jak k provádění různých dalších pokročilých úloh nasazení, jako vlastní nastavení nasazení databáze pro prostředí s více, vyloučení souborů a složek z nasazení a přepnutím do režimu offline webové aplikace během procesu nasazení .

>[!div class="step-by-step"]
[Další](creating-a-team-project-in-tfs.md)
