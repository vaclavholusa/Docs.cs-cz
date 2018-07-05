---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Správa životního cyklu aplikace: Od vývoje k ostrému provozu | Dokumentace Microsoftu'
author: jrjlee
description: Toto téma ukazuje, jak fiktivní společnost spravuje nasazení webové aplikace ASP.NET prostřednictvím testu, přípravného a produkčního prostředí jako pamětích...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: fdd51d2b6836c7fed04132f7c05bbede772d21e0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362807"
---
<a name="application-lifecycle-management-from-development-to-production"></a>Správa životního cyklu aplikace: Od vývoje k ostrému provozu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma ukazuje, jak fiktivní společnost spravuje nasazení webové aplikace ASP.NET prostřednictvím testu, přípravného a produkčního prostředí jako součást procesu průběžného vývoje. V rámci tématu jsou uvedeny odkazy na další informace a návody o tom, jak provádět konkrétní úlohy.
> 
> Téma je určené k představují základní popis pro [sérii kurzů](deploying-web-applications-in-enterprise-scenarios.md) na nasazení webu v podniku. Nedělejte si starosti, pokud nejste obeznámeni s některé koncepty popsané&#x2014;kurzech, které následují poskytují podrobné informace o všech těchto úlohách a techniky.
> 
> > [!NOTE]
> > Forthe zájmu jednoduchosti, toto téma se nezabývá aktualizace databáze jako součást procesu nasazení. Však provedením přírůstkové aktualizace funkcí databáze je požadavek mnoho podnikových scénářích nasazení, a můžete najít pokyny o tom, jak to provést později v této řadě kurzů. Další informace najdete v tématu [nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md).


## <a name="overview"></a>Přehled

Proces nasazení, která je znázorněna zde je založen na scénář nasazení společnosti Fabrikam, Inc., který je popsaný v [nasazení podnikového webu: přehled scénářů](enterprise-web-deployment-scenario-overview.md). Přehled scénářů přečtěte si před studovat v tomto tématu. V podstatě scénář prozkoumá způsob, jakým organizace spravuje nasazení poměrně složité webovou aplikaci, [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), prostřednictvím různých fází v typickém podnikovém prostředí.

Na vysoké úrovni prochází řešení Správce kontaktů těchto fází v rámci vývoje a proces nasazení:

1. Vývojář kontroluje kód do Team Foundation Server (TFS) 2010.
2. TFS vytvoří kód a spustí všechny testy jednotek, které jsou asociované s týmovým projektem.
3. TFS se řešení nasadí do testovacího prostředí.
4. Tým vývojářů zkontroluje a ověří řešení v testovacím prostředí.
5. Pracovní prostředí správce provádí "what if" nasazení do přípravného prostředí stanovit, zda nasazení způsobí, že jakékoli problémy.
6. Pracovní prostředí správce provádí za nasazení do přípravného prostředí.
7. Řešení projde akceptační testování v testovacím prostředí.
8. Balíčky nasazení webu se ručně importují do produkčního prostředí.

Tyto fáze je součástí průběžné vývojového cyklu.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

V praxi proces je o něco složitější než to, jak se zobrazí, když se podíváme v každé fázi podrobněji. Společnost Fabrikam, Inc. používá jiný přístup při nasazení pro každé cílové prostředí.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Zbývající část tohoto tématu zkontroluje tyto klíče fázích tohoto životního cyklu nasazení:

- **Požadavky**: jak budete muset nakonfigurovat serverové infrastruktury předtím, než začleníte logiky nasazení na místě.
- **Počáteční vývoj a nasazení**: co je potřeba udělat před nasazením řešení poprvé.
- **Nasazení testování**: zabalení a nasazení obsahu do testovacího prostředí automaticky když vývojář vrátí nový kód.
- **Nasazení do přípravného prostředí**: jak nasadit konkrétní sestavení do přípravného prostředí a jak provádět "what if" nasazení zajistí, že nasazení nebude způsobovat problémy.
- **Nasazení do produkčního prostředí**: jak naimportovat webových balíčků do produkčního prostředí při vzdálené nasazení brání síťové infrastruktury.

## <a name="prerequisites"></a>Požadavky

První úkol ve všech scénářích nasazení je zajistit, že serverové infrastruktury splňuje požadavky na nasazení nástroje a techniky. V takovém případě společnosti Fabrikam, Inc. má nakonfigurovanou jeho serverové infrastruktury následujícím způsobem:

- TFS je nakonfigurována k zahrnout kolekce týmových projektů, kontrolery sestavení a agenty sestavení. Zobrazit [konfigurace serveru Team Foundation Server pro nasazení webu pomocí automatizované](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) Další informace.
- Testovací prostředí je nakonfigurované tak, aby přijímal vzdálené nasazení s využitím služby agenta nasazení webu ("vzdálený agent"), jak je popsáno v [scénář: Konfigurace testovací prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) a [ Konfigurace webového serveru pro nasazení webu (vzdálený Agent) publikování](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- Pracovní prostředí je nakonfigurované tak, aby přijímal vzdálené nasazení s využitím koncového bodu obslužné rutiny nasazení webu, jak je popsáno v [scénář: Konfigurace pracovní prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) a [nakonfigurovat webový Server Publikování nasazeného webu (Obslužná rutina nasazení webu)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- V provozním prostředí nakonfigurována, aby umožňovala správcem k ručnímu importu balíčky nasazení webu do Internetové informační služby (IIS), jak je popsáno v [scénář: Konfigurace provozního prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) a [konfiguraci webového serveru pro publikování (Offline nasazení) nasazeného webu](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Počáteční vývoj a nasazení

Vývojový tým společnosti Fabrikam, Inc. mohli nasadit řešení Správce kontaktů poprvé, je potřeba provést tyto úlohy:

- Vytvořte nový týmový projekt v sadě TFS.
- Vytvoření souborů projektu Microsoft Build Engine (MSBuild), které obsahují logiku nasazení.
- Vytvoření definic sestavení TFS, které aktivuje procesům nasazení.

### <a name="create-a-new-team-project"></a>Vytvořte nový týmový projekt

- Správce serveru TFS, Rob Walters vytvoří nový týmový projekt pro aplikaci, jak je popsáno v [vytvořením týmového projektu v TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). V dalším kroku vedoucí vývojář, Matt Hink vytvoří kostru řešení. Má ověří jeho soubory do nového týmového projektu v TFS, jak je popsáno v [přidávání obsahu do správy zdrojových kódů](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Vytvoření logiky nasazení

Matt Hink vytváří různé vlastní soubory projektu MSBuild, přístup soubor projektu rozdělené podle [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt vytvoří:

- Soubor projektu s názvem *Publish.proj* , který spouští proces nasazení. Tento soubor obsahuje cíle nástroje MSBuild, sestavení projektů v řešení, vytváření webových balíčků a nasazení balíčků do cílového serveru prostředí.
- Soubory projektu specifických pro prostředí s názvem *Env Dev.proj* a *Env Stage.proj*. Ty obsahují nastavení, která jsou specifická pro testovací prostředí a testovací prostředí, jako jsou připojovací řetězce, koncové body služby a podrobnosti o vzdálené služby, která bude dostávat webového balíčku. Informace o volbě správné nastavení pro konkrétní cílové prostředí najdete v tématu [nakonfigurovat vlastnosti nasazení pro cílové prostředí](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Spustit nasazení, uživatel provede *Publish.proj* soubor pomocí nástroje MSBuild nebo Team Build a určí umístění souboru projektu odpovídající specifických pro prostředí (*Env Dev.proj* nebo *Env Stage.proj*) jako argument příkazového řádku. *Publish.proj* souboru poté importuje soubor projektu specifických pro prostředí vytvořit úplnou sadu pokynů pro každé cílové prostředí pro publikování.

> [!NOTE]
> Způsob, jakým tyto soubory vlastních projektů práce je nezávislá mechanismus, který používáte k vyvolání MSBuild. Například můžete použít příkazový řádek MSBuild přímo, jak je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Soubory projektu můžete spustit z příkazového souboru, jak je popsáno v [vytvoření a spuštění souboru příkazů k nasazení](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Alternativně můžete spustit soubory projektu z definice sestavení v TFS, jak je popsáno v [vytvoření definice sestavení nasazení podporuje](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> V každém případě konečný výsledek je stejný&#x2014;MSBuild spustí soubor sloučené projektu a řešení nasadí do cílového prostředí. To vám poskytuje značnou flexibilitu v tom, jak aktivovat váš proces publikování.


Jakmile vytvořil soubory vlastních projektů, Matt se přidají do složky řešení a vrací do správy zdrojového kódu.

### <a name="create-build-definitions"></a>Vytvoření definice sestavení

Jako konečné přípravný úkol Matt a Rob společně vytvářejí tři definice sestavení pro nový týmový projekt:

- **DeployToTest**. Toto řešení Správce kontaktů sestaví a nasadí do testovacího prostředí při každém vrácení se změnami vyvolá.
- **DeployToStaging**. To nasadí prostředků ze zadaného předchozí sestavení do přípravného prostředí, když vývojář vloží sestavení do fronty.
- **DeployToStaging-WhatIf**. To provede "what if" nasazení do přípravného prostředí, když vývojář vloží sestavení do fronty.

Následující části poskytují další podrobnosti na každém z nich definice sestavení.

## <a name="deployment-to-test"></a>Nasazení do prostředí Test

Vývojový tým společnosti Fabrikam, Inc. udržuje testovací prostředí provádět různé aktivity, jako jsou ověřování a ověřování, testování, testování kompatibility a ad hoc nebo průzkumného testování pro testování softwaru.

Vývojový tým vytvořil definici sestavení na serveru TFS s názvem **DeployToTest**. Tato definice sestavení používá trigger průběžné integrace, což znamená, že proces sestavení spustí vždy, když člen týmu společnosti Fabrikam, Inc. vývoj provede vrácení se změnami. Když se aktivuje sestavení, definice sestavení bude:

- Sestavte řešení ContactManager.sln. To zase sestavení všech projektů v rámci řešení.
- Spusťte všechny testy jednotek ve struktuře složek řešení (je-li řešení je sestaveno úspěšně).
- Spusťte vlastní soubory, které řídí proces nasazení (Pokud řešení je sestaveno úspěšně a předává všechny testy jednotek).

Konečným výsledkem bude, že pokud řešení je sestaveno úspěšně a úspěšně projde testy jednotek, webových balíčků a všechny další prostředky, nasazení se nasadí do testovacího prostředí.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Jak funguje proces nasazení?

**DeployToTest** sestavení definice poskytuje tyto argumenty nástroje MSBuild:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


**DeployOnBuild = true** a **DeployTarget = balíček** vlastnosti jsou používány při vytváření týmového sestavení projektů v rámci řešení. Pokud je projekt webové aplikace, tyto vlastnosti dáte pokyn, aby nástroj MSBuild vytvoří balíček pro nasazení webového projektu. **TargetEnvPropsFile** vlastnost říká *Publish.proj* kde najít soubor projektu specifických pro prostředí pro import souboru.

> [!NOTE]
> Podrobný návod o tom, jak vytvořit definici sestavení tímto způsobem, naleznete v tématu [vytvoření definice sestavení nasazení podporuje](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


*Publish.proj* soubor obsahuje cíle, které každý projekt v řešení sestavit. Ale také obsahuje podmíněnou logiku, přeskočí těchto sestavení cíle Pokud provedete souboru ve službě Team Build. Díky tomu můžete využít výhod funkce další sestavení, které Team Build nabízí, jako je možnost spouštět testy jednotek. Pokud sestavení řešení nebo jednotky testů selhání, *Publish.proj* soubor nebude provedeno a aplikace nebude nasazena.

Podmíněnou logiku provádí vyhodnocení **BuildingInTeamBuild** vlastnost. Toto je vlastnost MSBuild, který se automaticky nastaví na **true** při použití Team Build pro sestavování vašich projektů.

## <a name="deployment-to-staging"></a>Nasazení do přípravného prostředí

Při sestavení splňuje všechny požadavky vývojovému týmu v testovacím prostředí, tým může chtít stejné sestavení nasazení do přípravného prostředí. Pracovní prostředí jsou obvykle konfigurované tak, aby odpovídala vlastnosti produkční nebo "živé" prostředí jako úzce co nejvíc, například z hlediska serveru specifikace, operační systémy a software a konfiguraci sítě. Pracovní prostředí se často používají pro zátěžové testování, testování přijetí u zákazníků a širší interní kontroly. Sestavení se nasadí do přípravného prostředí přímo z serveru sestavení.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Definice sestavení použít k nasazení řešení do přípravného prostředí **DeployToStaging-WhatIf** a **DeployToStaging**, sdílet tyto charakteristiky:

- Není vytváří skutečně cokoliv. Když Rob řešení nasadí do přípravného prostředí, chce nasadit konkrétní existující sestavení, který již byl ověřen a ověření v testovacím prostředí. Definic sestavení, které stačí ke spuštění vlastních projektů soubory, které řídí proces nasazení.
- Když Rob aktivuje sestavení, používá parametry sestavení k sestavení, které obsahuje prostředky, které chce nasadit ze serveru sestavení.
- Definice sestavení se automaticky spustí. Rob ruční zařadí do fronty sestavení při chce nasadit řešení do přípravného prostředí.

Toto je souhrnný proces pro nasazení do přípravného prostředí:

1. Pracovní prostředí správce, Rob Walters zařadí do fronty sestavení pomocí **DeployToStaging-WhatIf** definice sestavení. Rob používá parametry definice sestavení k určení sestavení, které chce nasadit.
2. **DeployToStaging-WhatIf** sestavení definice spustí vlastní soubory v režimu "what if". To vytváří soubory protokolů, jako kdyby Rob prováděl živé nasazení, ale to není ve skutečnosti žádné změny cílového prostředí.
3. Rob zkontroluje soubory protokolu pro ověření efekty nasazení do přípravného prostředí. Zejména Rob chce zkontrolujte, co se přidají, co se aktualizuje a co se má odstranit.
4. Pokud je Rob splněny, nasazení neprovede žádné nežádoucí změny na stávající prostředky nebo data, která zařadí do fronty sestavení pomocí **DeployToStaging** definice sestavení.
5. **DeployToStaging** sestavení definice spuštění vlastních projektů souborů. Tyto materiály pro nasazení publikovat primární webový server v testovacím prostředí.
6. Kontroleru webové farmy Framework (WFF) synchronizuje webové servery v testovacím prostředí. Díky tomu aplikace k dispozici na všech webových serverů v serverové farmě.

### <a name="how-does-the-deployment-process-work"></a>Jak funguje proces nasazení?

**DeployToStaging** sestavení definice poskytuje tyto argumenty nástroje MSBuild:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


**TargetEnvPropsFile** vlastnost říká *Publish.proj* kde najít soubor projektu specifických pro prostředí pro import souboru. **OutputRoot** vlastnost přepíše integrovanou hodnotu a označuje umístění složky sestavení, která obsahuje prostředky, které chcete nasadit. Při Rob zařadí do fronty sestavení, použije **parametry** kartu k poskytování aktualizovanou hodnotu pro **OutputRoot** vlastnost.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Další informace o tom, jak vytvořit definici sestavení tímto způsobem, naleznete v tématu [nasadit konkrétní sestavení](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).


**DeployToStaging-WhatIf** definice sestavení obsahuje stejnou logiku nasazení, jako **DeployToStaging** definice sestavení. Však zahrnuje další argument **WhatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


V rámci *Publish.proj* souboru **WhatIf** vlastnost indikuje, že všechny prostředky nasazení by se měly zveřejňovat v režimu "what if". Soubory protokolu jsou generovány jinými slovy, jako kdyby nasazení bývali dopředu, ale nic se ve skutečnosti nezmění v cílovém prostředí. Díky tomu můžete vyhodnotit její dopad navrhované nasazení&#x2014;v konkrétní, co bude nechejte se přidat, co se budou aktualizovat, a co bude odstraněn&#x2014;předtím, než ve skutečnosti provedete nějaké změny.

> [!NOTE]
> Další informace o tom, jak nakonfigurovat "what if" nasazení najdete v tématu [provádění nasazení "What If"](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).


Po nasazení aplikace na primární webový server v testovacím prostředí, WFF aplikace automaticky synchronizovat na všechny servery v serverové farmě.

> [!NOTE]
> Další informace o konfiguraci WFF synchronizovat webových serverů, najdete v části [vytvoření serverové farmy pomocí rozhraní Web Farm Framework](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).


## <a name="deployment-to-production"></a>Nasazení do produkčního prostředí

Po schválení sestavení v testovacím prostředí můžete týmu společnosti Fabrikam, Inc. publikujte aplikaci do produkčního prostředí. Produkčním prostředí je kde aplikace přejde "živé" a dosáhne jeho cílová skupina koncových uživatelů.

Produkčním prostředí je v hraniční síti přístupem k Internetu. Toto je izolovaná od interní síti, která obsahuje server sestavení. Produkční prostředí správce, Lisa Andrews, musíte ručně zkopírovat balíčky nasazení webu ze serveru sestavení a naimportovat do služby IIS na produkční primární webový server.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Toto je souhrnný proces pro nasazení do produkčního prostředí:

1. Vývojovému týmu vás informuje o tom Lisa, že sestavení připravený k nasazení do produkčního prostředí. Tým vás informuje o tom Lisa umístění balíčky nasazení webu v odkládací složce na serveru sestavení.
2. Lisa shromažďuje webových balíčků ze serveru sestavení a zkopíruje je do primární webový server v provozním prostředí.
3. Lisa Správce služby IIS používá pro import a publikování webových balíčků na hlavním webovém serveru.
4. Kontroler WFF synchronizuje webové servery v provozním prostředí. Díky tomu aplikace k dispozici na všech webových serverů v serverové farmě.

### <a name="how-does-the-deployment-process-work"></a>Jak funguje proces nasazení?

Správce služby IIS obsahuje Průvodce importem aplikace balíčku, který usnadňuje publikování webových balíčků na webu služby IIS. Návod, jak tento postup provést, najdete v části [ruční instalace webových balíčků](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Závěr

Toto téma poskytuje ilustraci životní cyklus nasazení pro webovou aplikaci typické podnikové úrovni.

Toto téma je součástí série kurzů, které poskytují pokyny na různé aspekty nasazení webových aplikací. V praxi existuje mnoho dalších úlohy a důležité informace v každé fázi procesu nasazení a není možné pokrývají vše na jeden návodu. Další informace naleznete v těchto kurzech:

- [Webové nasazení v podniku](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Tento kurz poskytuje ucelený Úvod do služby web techniky nasazení pomocí nástroje MSBuild a nástroj nasazení webu služby IIS (Web Deploy).
- [Konfigurace serverového prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Tento kurz obsahuje pokyny ke konfiguraci prostředí Windows serveru pro podporu různých scénářů nasazení.
- [Konfigurace serveru Team Foundation Server pro nasazení webu pomocí automatizované](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Tento kurz obsahuje pokyny o tom, jak integrovat do procesů sestavení TFS logiky nasazení.
- [Pokročilé nasazení podnikového webu](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Tento kurz obsahuje pokyny o tom, jak splnit některé ze složitějších problémy při nasazení této organizace potýkají.

> [!div class="step-by-step"]
> [Předchozí](enterprise-web-deployment-scenario-overview.md)
