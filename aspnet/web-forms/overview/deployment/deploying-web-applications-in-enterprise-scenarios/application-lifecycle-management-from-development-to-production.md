---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: "Správa životního cyklu aplikací: Z vývojového do produkčního prostředí | Microsoft Docs"
author: jrjlee
description: "Toto téma ukazuje, jak fiktivní společnost spravuje nasazení webové aplikace ASP.NET pomocí prostředí testovací, přípravné nebo produkční prostředí jako nominální..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: f7ffff1c3434ce98c70265e4bf64047fd44252d0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="application-lifecycle-management-from-development-to-production"></a>Správa životního cyklu aplikací: Z vývojového do produkčního prostředí
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma ukazuje, jak fiktivní společnost spravuje nasazení webové aplikace ASP.NET prostřednictvím prostředí testovací, přípravné nebo produkční prostředí jako součást procesu průběžné vývoj. V rámci tématu jsou uvedeny odkazy na další informace a návody o tom, jak provádět určité úlohy.
> 
> Téma je určená k poskytnutí přehled pro [série kurzů](deploying-web-applications-in-enterprise-scenarios.md) na nasazení webu v podnikové síti. Nemusíte si dělat starosti, pokud si nejste obeznámeni s některé koncepty, které jsou zde popsané & #x 2014; kurzy, které následují poskytují podrobné informace na všech těchto úloh a techniky.
> 
> > [!NOTE]
> > Forthe zájmu jednoduchost, v tomto tématu se nezabývá aktualizace databáze jako součást procesu nasazení. Ale provedením přírůstkové aktualizace databáze funkce se vyžaduje mnoho podnikových scénářích nasazení a najdete pokyny o tom, jak to provést později z této série kurzu. Další informace najdete v tématu [nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md).


## <a name="overview"></a>Přehled

Proces nasazení, který je zde znázorněný je založena na scénář nasazení společnosti Fabrikam, Inc., který je popsaný v [Enterprise nasazení webu: přehled scénářů](enterprise-web-deployment-scenario-overview.md). Měli byste si přečíst přehled scénářů, než abyste si prostudovali v tomto tématu. V podstatě scénáři prověří, jak organizace spravuje nasazení to bude přiměřeně komplexní webové aplikace [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), prostřednictvím různých fází v běžném podnikovém prostředí.

Na vysoké úrovni obraťte se na správce řešení prochází k těmto fázím jako součást vývoj a proces nasazení:

1. Vývojář kontroluje nějaký kód do Team Foundation Server (TFS) 2010.
2. TFS kód vytvoří a spustí všechny testy jednotek přidružené týmový projekt.
3. TFS nasadí řešení do testovacího prostředí.
4. Týmem vývojářů zkontroluje a ověří řešení v testovacím prostředí.
5. Pracovní prostředí správce provádí nasazení "Co když" pro pracovní prostředí, a stanovit, zda nasazení způsobí potíže.
6. Pracovní prostředí správce provádí za provozu nasazení do fázovacího prostředí.
7. Řešení zde nevyskytlo přijetí uživateli testování v testovacím prostředí.
8. Balíčky pro nasazení webu jsou ručně importovány do produkčního prostředí.

K těmto fázím součástí průběžné vývojového cyklu.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

V praxi proces je trochu složitější než to, jak se zobrazí, když se podíváme na každé fáze podrobněji. Společnost Fabrikam A.s. používá jiný přístup k nasazení pro každé cílové prostředí.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Zbývající část tohoto tématu prozkoumá tyto klíče fázích tohoto životního cyklu nasazení:

- **Požadavky**: jak budete muset nakonfigurovat infrastrukturu serverů, předtím, než začleníte logika nasazení na místě.
- **Počáteční vývoj a nasazení**: co je potřeba udělat před nasazením řešení poprvé.
- **Nasazení otestovat**: jak balíček a nasazení obsahu pro testovací prostředí automaticky při vývojář změnami nový kód.
- **Nasazení pracovní**: sestavení nasazení konkrétní pracovní prostředí a jak provádět "Co když" nasazení zajistit, že nasazení nezpůsobí žádné problémy.
- **Nasazení do produkčního prostředí**: jak importovat webové balíčky do provozního prostředí při vzdálené nasazení brání síťové infrastruktury.

## <a name="prerequisites"></a>Požadavky

První úlohou v žádném scénáři nasazení je zajistit, že serverové infrastruktury splňuje požadavky na nasazení nástroje a techniky. V tomto případě společnost Fabrikam A.s. nakonfiguroval jeho serveru infrastruktury, jako jsou to:

- TFS je nakonfigurována pro zahrnout kolekce týmového projektu, sestavení řadiče a agentů sestavení. V tématu [konfigurace Team Foundation Server pro nasazení webu automatizované](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) Další informace.
- Testovací prostředí je nakonfigurován tak, aby přijímal vzdáleného nasazení pomocí agenta služba pro nasazení webu ("vzdáleného agenta"), jak je popsáno v [scénář: Konfigurace testovací prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) a [ Konfigurace webového serveru pro nasazení webu publikování (vzdáleného agenta)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- Je pracovní prostředí nakonfigurováno tak, aby přijímal vzdáleného nasazení pomocí koncového bodu obslužné rutiny nasazení webu, jak je popsáno v [scénář: Konfigurace pracovní prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) a [konfiguraci webového serveru pro Web nasazení publikování (Web Deploy obslužné rutiny)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- V provozním prostředí je konfigurace umožňuje Správce Ruční import balíčky nasazení webu v Internetové informační služby (IIS), jak je popsáno v [scénář: Konfigurace produkčním prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) a [konfigurace webového serveru pro nasazení webu publikování (Offline nasazení)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Počáteční vývoj a nasazení

Společnost Fabrikam A.s. vývojový tým mohli nasadit řešení obraťte se na správce poprvé, musí se k provedení těchto úloh:

- Vytvoření nového týmového projektu v sadě TFS.
- Vytváření souborů projektu Microsoft Build Engine (MSBuild), které obsahují logiku nasazení.
- Vytvoření definic sestavení sady TFS, které aktivují procesů nasazení.

### <a name="create-a-new-team-project"></a>Vytvoření nového týmového projektu

- Správce sady TFS, Rob Walters vytvoří nového týmového projektu pro aplikaci, jak je popsáno v [vytvoření týmového projektu v sadě TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). V dalším kroku developer realizace, Matt Hink vytvoří "kostra" řešení. Zadá ověří jeho soubory do nového týmového projektu v sadě TFS, jak je popsáno v [přidávání obsahu do správy zdrojového kódu](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Vytvořit logiku nasazení

Matt Hink vytvoří různé vlastní MSBuild soubory projektu, pomocí souboru projektu rozdělení metody uvedené v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt vytvoří:

- Soubor projektu s názvem *Publish.proj* , spustí proces nasazení. Tento soubor obsahuje cíle MSBuild, které vytvářet projekty v řešení, vytvoření webových balíčků a nasazení balíčků do cílového serveru prostředí.
- Soubory projektu specifické pro prostředí s názvem *Env Dev.proj* a *Env Stage.proj*. Ty obsahují nastavení, které jsou specifické pro testovací prostředí a testovací prostředí, jako je připojovací řetězce, koncové body služby a podrobnosti o vzdálené služby, která bude přijímat webového balíčku. Pokyny k výběru správné nastavení pro konkrétní cílové prostředí, najdete v části [nakonfigurovat vlastnosti nasazení pro cílové prostředí](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Pokud chcete spustit nasazení, uživatel provede *Publish.proj* souborů pomocí nástroje MSBuild nebo Team Build a určuje umístění souboru projektu relevantní specifické pro prostředí (*Env Dev.proj* nebo *Env Stage.proj*) jako argument příkazového řádku. *Publish.proj* souboru konkrétní prostředí projektu pro vytvoření kompletní sadu publikování pokyny pro každé cílové prostředí poté import souboru.

> [!NOTE]
> Způsob práce tyto soubory vlastních projektů je nezávislé mechanismu, který používáte pro vyvolání nástroje MSBuild. Například můžete v příkazovém řádku MSBuild přímo, jak je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Soubory projektu můžete spustit z příkazového řádku, jak je popsáno v [vytvořte a spusťte soubor příkazů nasazení](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Alternativně můžete spustit soubory projektu z definice sestavení v sadě TFS, jak je popsáno v [vytváření definici sestavení toto nasazení podporuje](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> V každém případě konečným výsledkem bude stejná & #x 2014; MSBuild provede souboru sloučené projektu a nasadí řešení na cílovém prostředí. To poskytuje značnou flexibilitu v tom, jak aktivovat váš proces publikování.


Po vytvoření vlastních projektů soubory mu má Matt přidá je do složky řešení a zkontroluje je do správy zdrojového kódu.

### <a name="create-build-definitions"></a>Vytvoření definic sestavení

Jako poslední přípravy úlohy, Matt a Rob spolupracují vytvořte tři definice sestavení pro nový týmový projekt:

- **DeployToTest**. Toto sestavení řešení obraťte se na správce a nasadí ho na testovací prostředí pokaždé, když dojde vrácení se změnami.
- **DeployToStaging**. To nasadí prostředky z předchozího zadané sestavení do fázovacího prostředí, když vývojář fronty sestavení.
- **DeployToStaging-WhatIf**. Když vývojář fronty sestavení provede "Co když" nasazení je pracovní prostředí.

Části obsahují další podrobnosti v každém z těchto sestavení definice.

## <a name="deployment-to-test"></a>Nasazení do testu

Vývojový tým společnosti Fabrikam, Inc. udržuje testovací prostředí k provádění různých testování aktivit, jako je ověření a ověření, testování použitelnosti, testování kompatibility a ad hoc nebo nahodilé testování softwaru.

Vývojový tým vytvořil definici sestavení v sadě TFS s názvem **DeployToTest**. Tuto definici sestavení používá aktivační událost průběžnou integraci, což znamená, že proces vytváření spustí pokaždé, když se člen týmu pro vývoj společnosti Fabrikam, Inc. provádí vrácení se změnami. Když se aktivuje build bude definici sestavení:

- Sestavte řešení ContactManager.sln. To zase sestaví všechny projekty v řešení.
- Spusťte všechny testy jednotek ve struktuře řešení složky (Pokud řešení sestavení úspěšně).
- Spusťte vlastní projektu soubory, které řídí proces nasazení (Pokud řešení sestavení úspěšně a předává všechny testy jednotek).

Konečným výsledkem je, že pokud řešení sestavení úspěšně a předá testování částí, webových balíčků a další prostředky pro nasazení se nasadí do testovacího prostředí.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Jak funguje proces nasazení?

**DeployToTest** sestavení tyto argumenty MSBuild definici zdroje:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


**DeployOnBuild = true** a **DeployTarget = balíček** vlastnosti se používají při sestavení Team sestavení projekty v řešení. Pokud projekt je projekt webové aplikace, požádejte tyto vlastnosti MSBuild k vytvoření balíčku pro nasazení webu pro projekt. **TargetEnvPropsFile** vlastnost říká *Publish.proj* souboru kde najít konkrétní prostředí projektu soubor k importu.

> [!NOTE]
> Podrobný návod k vytvoření definice sestavení jako to, najdete v části [vytváření definici sestavení toto nasazení podporuje](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


*Publish.proj* soubor obsahuje cílů, které sestavit každý projekt v řešení. Ale zahrnuje také podmíněnou logiku, přeskočí tyto sestavení cílů Pokud jste spouštění souboru v Team Build. Díky tomu můžete využít výhod funkce další sestavení, která Team Build nabízí, například možnost spouštění testování částí. Pokud sestavení řešení nebo tuto jednotku testy nezdaří, *Publish.proj* soubor nebude spuštěn a aplikace nebude nasazen.

Podmíněnou logiku provádí vyhodnocení **BuildingInTeamBuild** vlastnost. Toto je ve vlastnosti nástroje MSBuild, který se automaticky nastaví na **true** při použití Team Build vytvářet projekty.

## <a name="deployment-to-staging"></a>Nasazení do přípravy

Pokud sestavení splňuje všechny požadavky na týmu pro vývojáře v testovacím prostředí, tým může chtít nasadit ve stejném sestavení do pracovního prostředí. Přípravná prostředí se obvykle konfigurují tak, aby odpovídaly charakteristiky produkční nebo "živé" prostředí jako úzce co nejmenší, například z hlediska specifikace serveru, operační systémy a software a konfiguraci sítě. Přípravná prostředí se často používají pro zátěžové testování, testování přijetí uživateli a širší interní recenze. Sestavení se nasadí do pracovní prostředí přímo ze serveru sestavení.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Definice sestavení, které jsou používány k nasazení řešení pro pracovní prostředí, **DeployToStaging-WhatIf** a **DeployToStaging**, mají tyto vlastnosti:

- Nemáte vytváří skutečně cokoliv. Když Rob nasadí řešení na pracovní prostředí, chce nasadit konkrétní, existující sestavení, který již byl ověřen a ověřit v testovacím prostředí. Definice sestavení jednoduše ke spouštění vlastních projektů souborů, které řídí proces nasazení.
- Když Rob aktivuje build, pomocí sestavení parametry zadejte sestavení, které obsahuje prostředky, které chce nasadit ze serveru sestavení.
- Definice sestavení nejsou automaticky spustí. Rob ručně fronty sestavení, když chce nasadit řešení do fázovacího prostředí.

Toto je proces vysoké úrovně pro nasazení, který je pracovní prostředí:

1. Pracovní prostředí správce Rob Walters fronty sestavení pomocí **DeployToStaging-WhatIf** definice sestavení. Rob používá parametry definice sestavení zadat sestavení, která chce nasadit.
2. **DeployToStaging-WhatIf** sestavení spustí definice vlastních projektů souborů v režimu "Co když". To vytváří soubory protokolů, jako kdyby Rob byl výkon za provozu nasazení, ale není ve skutečnosti díky všechny změny v cílovém prostředí.
3. Rob zkontroluje soubory protokolu a stanovit účinky nasazení v testovacím prostředí. Konkrétně Rob chce zkontrolujte, co bude přidán, co bude aktualizován a co se odstraní.
4. Pokud je Rob splnit, aby nasazení nebude žádné změny nežádoucího existující prostředkům nebo datům, mohl fronty sestavení pomocí **DeployToStaging** definice sestavení.
5. **DeployToStaging** sestavení spustí definice vlastních projektů souborů. Tyto prostředky nasazení publikovat na primární webový server v testovacím prostředí.
6. Řadičem webové farmy Framework (WFF) synchronizuje webové servery v testovacím prostředí. Díky aplikaci k dispozici ve všech webových serverech v serverové farmě.

### <a name="how-does-the-deployment-process-work"></a>Jak funguje proces nasazení?

**DeployToStaging** sestavení tyto argumenty MSBuild definici zdroje:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


**TargetEnvPropsFile** vlastnost říká *Publish.proj* souboru kde najít konkrétní prostředí projektu soubor k importu. **OutputRoot** vlastnost přepíše integrovanou hodnotu a určuje umístění složky sestavení, která obsahuje prostředky, které chcete nasadit. Když Rob fronty sestavení, pomocí **parametry** kartě zajistit aktualizovanou hodnotu pro **OutputRoot** vlastnost.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Další informace o tom, jak vytvořit definici sestavení takto najdete v tématu [nasazení konkrétní sestavení](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).


**DeployToStaging-WhatIf** definici sestavení obsahuje logice nasazení jako **DeployToStaging** definice sestavení. Však obsahuje další argument **WhatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


V rámci *Publish.proj* souboru **WhatIf** vlastnost určuje, zda všechny prostředky nasazení je nutné ji publikovat v režimu "Co když". Jinými slovy soubory protokolu jsou generovány, jako kdyby měl článek zmizel nasazení, ale ve skutečnosti nic se změní v cílovém prostředí. To umožňuje hodnotit dopad navrhované nasazení & #x 2014; zejména, co bude se přidají, co bude aktualizovat a co bude odstraněn & #x 2014; předtím, než je ve skutečnosti provést všechny změny.

> [!NOTE]
> Další informace o tom, jak nakonfigurovat "Co když" nasazení najdete v tématu [provádění nasazení "Co když"](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).


Po nasazení aplikace na primární webový server v testovacím prostředí, WFF automaticky synchronizovat aplikace na všechny servery v serverové farmě.

> [!NOTE]
> Další informace o konfiguraci WFF synchronizovat webových serverů, najdete v části [vytvoření serverové farmy pomocí rozhraní Web Farm Framework](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).


## <a name="deployment-to-production"></a>Nasazení do produkčního prostředí

Po schválení sestavení v testovacím prostředí, týmem společnosti Fabrikam, Inc., můžete publikovat aplikaci do produkčního prostředí. V provozním prostředí je, kde aplikace přejde "živé" a dosáhne jeho cílová skupina koncových uživatelů.

V provozním prostředí je v hraniční síti internetového. Toto je izolovaná od interní síti, která obsahuje server sestavení. Produkční prostředí správce, Lisa Andrews, musíte ručně zkopírovat balíčky pro nasazení webu ze serveru sestavení a importovat do služby IIS na webovém serveru primární produkční.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Toto je proces vysoké úrovně pro nasazení do produkčního prostředí:

1. Týmem vývojářů informuje o tom, Lisa, sestavení je připravený pro nasazení do produkčního prostředí. Tým informuje o tom Lisa umístění balíčky nasazení webu ve složce rozevírací na buildovacím serveru.
2. Lisa shromažďuje webových balíčků ze serveru sestavení a zkopíruje je do primární webový server v provozním prostředí.
3. Lisa Správce služby IIS používá k importu a publikování webových balíčků na hlavním webovém serveru.
4. Řadič WFF synchronizuje webové servery v provozním prostředí. Díky aplikaci k dispozici ve všech webových serverech v serverové farmě.

### <a name="how-does-the-deployment-process-work"></a>Jak funguje proces nasazení?

Správce služby IIS zahrnuje balíček Průvodce aplikací importu, který usnadňuje publikování webových balíčků na web služby IIS. Návod na tom, jak provést tento postup najdete v tématu [ručně instalaci webových balíčků](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Závěr

Toto téma poskytuje ilustraci životního cyklu nasazení pro webovou aplikaci typické podnikovém měřítku.

Toto téma je součástí ze série kurzů, které poskytují informace o různých aspektech nasazení webových aplikací. V praxi jsou v každé fázi procesu nasazení spoustu dalších úloh a důležité informace, a není možné je pokrýt vše v jednom návod. Další informace vám poskytne tyto kurzy:

- [Nasazení v podnikové síti webu](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). V tomto kurzu poskytuje komplexní úvod do webové techniky nasazení pomocí nástroje MSBuild a nástroj nasazení webu služby IIS (Web Deploy).
- [Konfigurace serveru prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Tento kurz obsahuje pokyny ke konfiguraci prostředí Windows server pro podporu různých scénářů nasazení.
- [Konfigurace serveru Team Foundation Server pro automatizované nasazení webu](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Tento kurz obsahuje pokyny o tom, jak integrovat logiku nasazení do sady TFS sestavení procesy.
- [Pokročilé nasazení webu Enterprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Tento kurz obsahuje pokyny o tom, jak splnit některé problémy složitější nasazení vzhled této organizace.

>[!div class="step-by-step"]
[Předchozí](enterprise-web-deployment-scenario-overview.md)
