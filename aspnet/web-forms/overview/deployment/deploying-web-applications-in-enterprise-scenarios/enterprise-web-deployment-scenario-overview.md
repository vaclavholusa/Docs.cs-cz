---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Nasazení webu Enterprise: Přehled scénářů | Microsoft Docs'
author: jrjlee
description: Tato sada kurzy používá ukázkové řešení s úrovní realistické složitější, společně s scénář nasazení fiktivních enterprise, k poskytování ref...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 20f6e206d6aa4bebb4936246468f5ada0e213236
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890018"
---
<a name="enterprise-web-deployment-scenario-overview"></a>Podnikového nasazení webu: Přehled scénáře
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Tato sada kurzy používá ukázkové řešení s úrovní realistické složitější, společně s scénář nasazení fiktivních enterprise, a zadejte odkaz na implementaci a dát úlohy a návody pro běžné kontextu. Toto téma popisuje kurz scénář a zavádí ukázkové řešení.


## <a name="scenario-description"></a>Popis scénáře

Společnost Fabrikam, Inc., fiktivní společnosti, je vytvoření řešení, které umožňuje vzdálené týmy prodeje, ukládání a načítání kontaktních informací z webové rozhraní.

Procesy Application Lifecycle Management (ALM) společnosti Fabrikam, Inc. vyžadovat řešení pro nasazení do tří serverových prostředích v různých fázích vývoje softwaru:

- Vývojáře testu nebo "izolovaného prostoru" prostředí.
- Na základě intranetu pracovní prostředí.
- Internetový produkčního prostředí.

Každý z těchto prostředí má jinou konfiguraci a požadavky na zabezpečení a každý z čeho výzvy jedinečný nasazení.

### <a name="the-fabrikam-inc-server-infrastructure"></a>Společnost Fabrikam, Inc. Server infrastruktury

Toto je podrobný vývoj a nasazení infrastruktury společnosti Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Pracovní stanice developer, zdrojové infrastruktuře ovládací prvek, developer testovacího prostředí a pracovní prostředí, ve kterém jsou všechny umístěny v síti intranet v rámci domény Fabrikam.net. Produkčním prostředí se nachází na hraniční síti (označované také jako DMZ, demilitarizovaná zóna a monitorovaná podsíť), která je izolovaná od sítě intranet bránou firewall. Toto je běžný scénář nasazení: obvykle izolovat internetového webových serverů z vaší interní server infrastruktury prostřednictvím brány firewall nebo servery brány.

V tomto příkladu:

- Na server Team Foundation Server (TFS) 2010 s samostatné sestavení serveru poskytuje zdrojového kódu a funkce průběžnou integraci (konfigurace).
- Testovací prostředí vývojáře zahrnuje webového serveru Internetové informační služby (IIS) 7.5 a databázový server SQL Server 2008 R2.
- V provozním prostředí obsahuje více webových serverů služby IIS 7.5 synchronizovány se serverem webové farmy Framework (WFF) řadiče, společně s databázový server SQL Server 2008 R2. V praxi může databázový server použít clustering nebo zrcadlení ke zlepšení škálovatelnosti a dostupnosti.
- Je pracovní prostředí je určena pro co nejblíže replikovat konfiguraci produkčního prostředí.
- Zásad izolace sítě a brány firewall nepovoluje direct, automatické nasazení z intranetu na hraniční síti.

Konfiguraci každé z těchto prostředí je podrobně popsaná v další v druhé kurzu [konfigurace serveru prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Tým role pro ALM

Tito uživatelé se účastní vytváření, správa, vytváření a publikování řešení obraťte se na správce:

- Matt Hink je webový vývojář aplikace společnosti Fabrikam, Inc. Je součástí týmu, který vyvinuté pomocí sady Visual Studio 2010 řešení obraťte se na správce. Matt má úplná práva správce na serverech v testovacím prostředí vývojáře, která umožní mu nakonfigurovat prostředí tak, aby podle svých potřeb. Má také přístup uživatelů k Visual Studio 2010 TFS instance, kde uloží zdrojového kódu pro řešení obraťte se na správce.
- Rob Walters je správce serveru pro společnost Fabrikam A.s. vývojový tým. Rob má přístup pro správu na serveru TFS, takže může nakonfigurovat všechny aspekty sady TFS a Team Build. Rob také má přístupová oprávnění k testovací a pracovní webové servery a funguje jako správce databáze (DBA) pro servery databáze testovacích a přípravných prostředí. Rob nakonfiguroval Team Build na serveru TFS k vykonávání těchto úkolů:

    - Sestavení a spouštění testování částí v aplikaci vždy, když uživatel zkontroluje v souboru do sady TFS. Tomu se říká položek konfigurace.
    - Obraťte se na správce aplikace pro testovací prostředí nasadíte automaticky, jakmile se aplikace úspěšně projde testy jednotek. To zahrnuje publikování databáze na testovací servery v původním nasazení a všechny aktualizace do databáze po počátečním nasazení.
    - Nasazení obraťte se na správce aplikace pro pracovní prostředí v procesu krokování.
    - Vytvořte webový balíček, který správce webového serveru a správce databáze můžete použít k publikování aplikace do produkčního prostředí.
- Lisa Andrews je zodpovědná za nasazení aplikací na produkční servery společnosti Fabrikam, Inc., správce serveru. Uživatel má přístup pro čtení ke sdílené složce, kde TFS Team Build ukládá balíčku pro nasazení webu po sestavení aplikace obraťte se na správce. Uživatel má také přístup správce pro provozní webové servery tak, aby Jana můžete nasadit aplikace do produkčního prostředí. Kromě toho uživatel funguje jako DBA, kdo nasazuje databáze a aktualizace databáze na serveru databáze v provozním prostředí.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>Obraťte se na správce řešení

Obraťte se na správce řešení je navrženo tak, aby uživatelé registrovaný, přihlášený přidávání a úprava kontaktních informací prostřednictvím webového rozhraní. Řešení obraťte se na správce se skládá ze čtyř jednotlivých projektů:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Toto je projekt webové aplikace technologie ASP.NET MVC3, která představuje vstupní bod pro řešení. Nabízí některé funkce základní webové aplikace, jako jsou nabízí uživatelům možnost vytvářet a prohlížet kontaktní údaje. Aplikace spoléhá na službu Windows Communication Foundation (WCF) ke správě kontakty a databázi služeb aplikace ASP.NET ke správě ověřování a autorizace.
- **ContactManager.Database**. Toto je databázového projektu sady Visual Studio 2010. Projekt definuje schéma pro databázi, aby úložiště kontaktní údaje.
- **ContactManager.Service**. Toto je projekt webové služby WCF. Zpřístupňuje WCF koncový bod, který umožňuje volajícím provádět vytvořit, získat, aktualizovat a odstranit operace v databázi obraťte se na správce. Služba závisí na obraťte se na správce databáze a ContactManager.Common.dll sestavení.
- **ContactManager.Common**. Toto je projektu knihovny tříd. Služby WCF spoléhá na typy definované v tomto sestavení.

Úplný přehled o řešení a jeho požadavky na nasazení je součástí z prvního kurzu této série [nasazení webu v podnikové síti](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Úlohy nasazení

Existuje několik různých úloh potřebných k nasazení aplikací do různých prostředí ve velkých organizacích. Toto jsou klíčové úlohy, které se týkají kurzů k:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Tady je seznam jednotlivých kroků v procesu nasazení z pohledu uživatelů popsané dříve v tomto dokumentu:

1. Všichni členové týmu, projděte si obraťte se na správce řešení v sadě Visual Studio 2010 k určení klíče nasazení požadavky a problémy.
2. Matt Hink může nasadit řešení obraťte se na správce přímo z pracovní stanice vývojáře pro vývojáře testovací prostředí, které mají proveďte test počáteční logiku nasazení.
3. Matt Hink přidá aplikaci do správy zdrojového kódu v sadě TFS.
4. Rob Walters vytvoří různé definice sestavení, obraťte se na správce řešení v Team Build. Jednu definici sestavení používá CI k nasazení řešení do testovacího prostředí vývojáře vždy, když uživatel zkontroluje v nový kód. Jinou definici sestavení umožňuje uživatelům aktivační událost nasazení pro pracovní prostředí podle potřeby.
5. Pokaždé, když uživatel vrátí nový kód, Team Build automaticky vytvoří komponenty řešení, spustí testy jednotek a nasadí řešení do testovacího prostředí vývojáře Pokud sestavení bylo úspěšné a předejte jí testů jednotek.
6. Když uživatel spustí nasazování do pracovní prostředí, je řešení zabalené a nasazení v jedné kroku procesu. Tento proces vytvoří také balíček pro ruční nasazení do produkčního prostředí.
7. Lisa Andrews nasadí aplikaci do produkčního prostředí ručně importováním webového balíčku vytvořili v kroku 6.

### <a name="key-deployment-issues"></a>Problémy při nasazení klíče

Obraťte se na správce řešení a scénář společnosti Fabrikam, Inc., zvýrazněte různé běžné problémy a výzvy, které se můžete setkat při nasazování komplexní řešení podnikovém měřítku. Příklad:

- Abyste mohli nasadit projekty do několika prostředích, jako je vývojáře nebo testovací prostředí, musíte, pracovní platformy a produkční servery. Řešení je potřeba nasadit pomocí různých konfiguračních nastavení pro každé prostředí.
- Je třeba nasadit více závislých projektů současně v rámci jednoho kroku nebo automatizované procesu sestavení a nasazení.
- Potřebujete mít možnost pro nasazení jednotky z automatizovaného procesu. Například chcete použít CI postup nasazení webové aplikace do pracovního prostředí při vrácení se změnami nový kód.
- Musíte být schopni řídit proces nasazení a nastavení proměnných pro nasazení mimo aplikaci Visual Studio, protože vývojáři pravděpodobně nebudou mít správné konfiguraci nastavení nebo potřebná pověření pro každé cílové prostředí.
- Potřebujete nasadit na základě schématu databázové projekty a zachovat stávající data na následné nasazení.
- Je třeba nasadit databáze členství na základě ad hoc bez nasazení data účtu uživatele. Také musíte aktualizovat schéma databáze členství nasazené bez ztráty stávajících dat účtu uživatele.
- Budete muset určité soubory nebo složky vyloučit při nasazení obsahu pro různé cílové prostředí.

Kromě toho Správa nasazení, když jsou často a přírůstkové aktualizace vyvolá si některé další běžné problémy. Příklad:

- Pokaždé, když vývojář změnami nový kód pro spuštění testů jednotek. Chcete nasadit řešení, pokud kód projde testy jednotek.
- Když nasadíte webovou aplikaci k pracovním nebo produkčním prostředí, které chcete přesměrovat uživatele do *aplikace\_offline.htm* soubor po dobu trvání procesu nasazení.
- Chcete protokolovat aktivity nasazení. Proces nasazení by měli poslat e-mailové oznámení o úspěšném nebo neúspěšném nasazení určeným příjemcům.
- Pokud automatického nasazení selže, proces nasazení by měl opakovat aktuální nasazení nebo místo toho nasadit předchozí webového balíčku.

> [!div class="step-by-step"]
> [Předchozí](deploying-web-applications-in-enterprise-scenarios.md)
> [další](application-lifecycle-management-from-development-to-production.md)
