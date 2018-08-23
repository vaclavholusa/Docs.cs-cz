---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Konfigurace serveru Team Foundation Server pro nasazení webu | Dokumentace Microsoftu
author: jrjlee
description: Tomto kurzu se dozvíte, jak nakonfigurovat Team Foundation Server (TFS) 2010 k sestavení řešení a nasazení webového obsahu do různých cílových prostředích. To...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2b668f2e9bdf73632b8b076416c9ccc5cf34a7ed
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752104"
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Konfigurace serveru Team Foundation Server pro nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Tomto kurzu se dozvíte, jak nakonfigurovat Team Foundation Server (TFS) 2010 k sestavení řešení a nasazení webového obsahu do různých cílových prostředích. To zahrnuje scénáře kontinuální integrace (CI), kde nasadíte obsah automaticky pokaždé, když vývojář provádí změny. Můžou také zahrnovat ruční aktivační události scénáře, ve kterém může správce k aktivaci nasazení konkrétního sestavení do přípravného prostředí po sestavení byl ověřen a ověření v testovacím prostředí. Témata v tomto kurzu vás provede celou konfiguraci procesu, včetně:
> 
> - Postup vytvoření nového týmového projektu v TFS.
> - Postup přidání obsahu do správy zdrojového kódu.
> - Jak nakonfigurovat server sestavení pro podporu CI a nasazení.
> - Jak vytvořit definici sestavení, která obsahuje logiku pro nasazení.
> - Jak nakonfigurovat oprávnění pro automatické nasazení.
> 
> Italština překlad těchto kurzů, navštivte web [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Tento kurz předpokládá, že jste nainstalovali TFS 2010 a vytvoření kolekce týmových projektů jako součást počáteční konfigurace procesu. [Team Foundation Průvodce instalací pro Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) poskytuje komplexní pokyny pro tyto úlohy.

## <a name="context"></a>Kontext

To je součástí série kurzů na základě požadavků organizace nasazení fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) řešení&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="scenario-overview"></a>Přehled scénářů

Základní scénář pro tyto kurzy je popsaný v [nasazení podnikového webu: přehled scénářů](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Doporučujeme, abyste si toto téma před zahájením práce v tomto kurzu.

## <a name="how-to-use-this-tutorial"></a>Jak používat v tomto kurzu

Pokud je to první úkoly popsané v tomto kurzu jste provedli, nebo pokud chcete postupovat podle příkladů použití ukázkové řešení, měli nejdřív projít témata kurzu v pořadí. Alternativně můžete použít jednotlivých tématech jako pokyny pro konkrétní úlohy. Tento kurz obsahuje následující témata:

- [Vytvoření týmového projektu v TFS](creating-a-team-project-in-tfs.md). Týmový projekt je základní jednotkou správy zdrojového kódu, řízení procesů a sestavení v TFS. Potřebujete vytvořit týmový projekt, než budete moct přidat obsah do správy zdrojového kódu nebo vytváření definice sestavení.
- [Přidávání obsahu do správy zdrojových kódů](adding-content-to-source-control.md). Po vytvoření týmového projektu, můžete začít přidávat obsah do správy zdrojového kódu. Bude potřeba přidat projekty a řešení, společně s externích závislostí, před zahájením konfigurace sestavení.
- [Konfigurace serveru TFS Server sestavení pro nasazení webu](configuring-a-tfs-build-server-for-web-deployment.md). Pokud chcete k sestavení obsahu vašeho týmu projektu, budete potřebovat ke konfiguraci serveru sestavení. Ve většině případů by měl být na samostatném počítači z instalace TFS. Ke konfiguraci serveru sestavení, je potřeba nainstalovat a nakonfigurovat službu sestavení TFS, nainstalujte Visual Studio 2010, vytvořit kontrolery sestavení a agenty sestavení, nainstalovat všechny produkty nebo komponenty, které váš kód potřebuje, aby bylo možné úspěšně sestavit a nainstalovat Internetová informační služba (IIS) webu nástroj pro nasazení (Web Deploy).
- [Vytvoření definice sestavení, která podporuje nasazení](creating-a-build-definition-that-supports-deployment.md). Před zahájením zařazování do fronty a budou spouštět buildy na serveru TFS, je potřeba vytvořit definice alespoň jedno sestavení pro týmový projekt. Definice sestavení definuje každý aspekt sestavení, včetně věci, které by měl být součástí sestavení, co by měly aktivovat sestavení a kam by měl Team Build poslat výstupy sestavení. Můžete nakonfigurovat definici sestavení spouštět vlastní soubory projektu Microsoft Build Engine (MSBuild), která umožňuje zahrnout logiky nasazení v automatizovaných sestaveních.
- [Nasazení konkrétního sestavení](deploying-a-specific-build.md). V možných scénářů budete chtít nasazení konkrétního sestavení, nikoli na nejnovější verzi na cílovém prostředí. V takovém případě můžete nakonfigurovat definici sestavení, která nasadí obsah z konkrétní odkládací složky.
- [Nasazení sestavení konfigurace oprávnění pro tým](configuring-permissions-for-team-build-deployment.md). Pokud je sestavovací služba pro nasazení obsahu jako součást automatizovaného procesu sestavení, budete muset poskytnout různá oprávnění k účtu sestavovací služby na všechny cílové webové servery a databázové servery.

## <a name="key-technologies"></a>Klíčové technologie

Tento kurz se zaměřuje na tom, jak používat tyto produkty a technologie pro podporu automatické sestavení a nasazení webu:

- Visual Studio Team Foundation Server 2010
- Týmové sestavení a nástroje MSBuild
- Web Deploy

Tento kurz se dotýká také při použití Windows serveru 2008 R2, IIS 7.5, SQL Server 2008 R2, technologii ASP.NET 4.0 a ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Další kurzy v této sérii

To je součástí série pěti kurzů na podnikové úrovni, nasazení webu. Toto jsou další kurzy v této sérii:

- [Nasazení webové aplikace v podnikových scénářích](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Tento úvodní obsah obsahuje kontextové informace týkající se série kurzů. Popisuje scénář tohoto kurzu a ukazuje, jak se úlohy a názorné postupy popsané v celé řadě vejde do širší proces správy životního cyklu aplikací (ALM).
- [Webové nasazení v podniku](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Tento kurz obsahuje koncepční Úvod do souborů projektu MSBuild, webových publikování kanálu (WPP), nasazení webu a dalších souvisejících technologiích. Vysvětluje, jak můžete pomocí těchto nástrojů společně ke správě nasazení komplexních procesů.
- [Konfigurace serverového prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Tento kurz popisuje, jak nakonfigurovat servery Windows pro podporu různých scénářů nasazení, včetně nasazení vzdáleného webového balíčku pomocí Služba agenta nasazení webu (vzdálený agent) nebo obslužná rutina webového nasazení a nasazení vzdálené databáze. Poskytuje rady, jak volba metody nasazení pro konkrétní prostředí, a je zde popsán způsob použít webové farmy Framework (WFF) k replikaci nasazených webových aplikací ve všech webových serverů v serverové farmě.
- [Pokročilé nasazení podnikového webu](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Tento kurz popisuje, jak k provádění různých rozšířené nasazení úkoly, jako je přizpůsobení nasazené databáze pro více prostředí, s výjimkou souborů a složek z nasazení a převedení webové aplikace do offline režimu během procesu nasazení .

> [!div class="step-by-step"]
> [Next](creating-a-team-project-in-tfs.md)
