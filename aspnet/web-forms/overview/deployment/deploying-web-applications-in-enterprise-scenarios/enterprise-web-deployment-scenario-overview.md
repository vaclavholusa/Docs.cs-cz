---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Nasazení podnikového webu: Přehled scénářů | Dokumentace Microsoftu'
author: jrjlee
description: Této sérii kurzů používá ukázkové řešení s realistické úroveň složitosti, společně s scénáři nasazení fiktivní organizace k poskytování ref...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 47bd0a0f7b71a6069d573f1454583cdb832f6b4c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366074"
---
<a name="enterprise-web-deployment-scenario-overview"></a>Nasazení podnikového webu: Přehled scénářů
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Této sérii kurzů používá ukázkové řešení s realistické úroveň složitosti, společně s scénář nasazení fiktivní organizace, a zadejte referenční implementaci a poskytnout úkoly a návody pro běžné kontextu. Toto téma popisuje scénář tohoto kurzu a zavádí ukázkové řešení.


## <a name="scenario-description"></a>Popis scénáře

Fiktivní společnosti Fabrikam, Inc., je vytváření řešení, které umožňuje vzdálené prodejní týmy, ukládání a načítání informací z webového rozhraní.

Procesy správy životního cyklu aplikací (ALM) společnosti Fabrikam, Inc. vyžadovat, aby řešení k nasazení do tří serverových prostředích v různých fázích procesu vývoje softwaru:

- Test nebo "izolovaném prostoru" prostředí pro vývojáře.
- Intranetoví pracovní prostředí.
- Produkční prostředí, přístupem k Internetu.

Každá z těchto prostředí obsahuje různé konfigurace a požadavky na zabezpečení a každá představuje problémy při nasazení jedinečný.

### <a name="the-fabrikam-inc-server-infrastructure"></a>Společnost Fabrikam, Inc. Server infrastruktury

Toto je základní infrastruktury vývoje a nasazení společnosti Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Vývojářské pracovní stanice, zdrojové infrastruktuře ovládacího prvku, testovací prostředí pro vývojáře a testovací prostředí, které se všechny nacházejí v síti intranet v rámci domény Fabrikam.net. Produkčním prostředí se nachází v hraniční síti (označované také jako DMZ, demilitarizovaná zóna a monitorovaná podsíť), což je izolovaná od sítě intranet bránou firewall. Toto je běžný scénář nasazení: obvykle izolovat vaše internetové webové servery z vaší interní server infrastruktury prostřednictvím brány firewall nebo serverů bran.

V tomto příkladu:

- Server Team Foundation Server (TFS) 2010 se serverem samostatné sestavení poskytuje správy zdrojového kódu a funkce kontinuální integrace (CI).
- Testovací prostředí pro vývojáře zahrnuje webového serveru Internetové informační služby (IIS) 7.5 a databázový server SQL Server 2008 R2.
- V provozním prostředí obsahuje více webových serverů služby IIS 7.5 synchronizovány se serverem kontroleru webové farmy Framework (WFF), společně s databázový server SQL Server 2008 R2. V praxi serveru databáze pomocí clusteringu nebo zrcadlení zlepšit škálovatelnost a dostupnost.
- Pracovní prostředí slouží k replikaci konfigurace provozního prostředí co nejpřesněji.
- Zásady izolace sítě a brány firewall nedovolují s přímým přístupem a automatizované nasazení z intranetu do hraniční sítě.

Konfigurace každého z těchto prostředí je podrobněji popsaný v druhé části kurzu [konfigurace serveru prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Týmové role pro ALM

Tito uživatelé jsou součástí vytváření, správa, vytváření a publikování řešení Správce kontaktů:

- Matt Hink je webový vývojář aplikace společnosti Fabrikam, Inc. Je součástí týmu, který řešení Správce kontaktů vyvinuté pomocí sady Visual Studio 2010. Matt má úplná práva správce na serverech v testovacím prostředí pro vývojáře, které vám umožní ho nakonfigurujte prostředí tak, aby podle svých potřeb. Má také přístup uživatelů k instanci Visual Studio 2010 TFS, kde uloží zdrojový kód pro řešení Správce kontaktů.
- Rob Walters je správce serveru pro vývojový tým společnosti Fabrikam, Inc. Rob má přístup správce na serveru TFS tak, aby mohl konfigurovat všechny aspekty TFS a týmového sestavení. Rob také má přístupová oprávnění k testování a pracovní webové servery a funguje jako správce databáze (DBA) pro databázové servery v testovací a přípravná prostředí. Rob nakonfiguroval Team Build na serveru TFS k provádění těchto úkolů:

    - Sestavte a spusťte testy jednotek v aplikaci pokaždé, když se uživatel vrátí do souboru na server TFS. Tomu se říká CI.
    - Nasazení aplikace Správce kontaktů do testovacího prostředí automaticky, jakmile se aplikace úspěšně projde testy jednotek. Jedná se o publikování databáze na testovací servery na počáteční nasazení a všechny aktualizace databáze po počátečním nasazení.
    - Nasaďte aplikaci Správce kontaktů do přípravného prostředí do jednoho kroku procesu.
    - Vytvořte webový balíček, který správce webového serveru a DBA lze použít k publikování aplikace do produkčního prostředí.
- Lisa Andrews je zodpovědná za nasazení aplikací na provozních serverech společnosti Fabrikam, Inc. správce serveru. Uživatel má přístup pro čtení ke sdílené složce, kam TFS Team Build ukládá balíčku pro nasazení webu po sestavení aplikace Správce kontaktů. Uživatel má také přístup správce pro provozní webové servery tak, aby si můžete nasadit aplikaci do produkčního prostředí. Kromě toho uživatel funguje jako DBA, kdo nasazuje databází a aktualizace databáze pro databázový server v provozním prostředí.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>Řešení Správce kontaktů

Obraťte se na správce řešení je navržena tak, aby registrovaná, přihlášeného uživatele přidat nebo upravit kontaktní údaje prostřednictvím webového rozhraní. Řešení Správce kontaktů skládá ze čtyř jednotlivé projekty:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Toto je projekt webové aplikace ASP.NET MVC3, která představuje vstupní bod pro řešení. Nabízí některé funkce základní webové aplikace, jako jsou zároveň uživatelům poskytují možnost vytvářet a zobrazovat kontaktní údaje. Aplikace se spoléhá na službu Windows Communication Foundation (WCF) pro správu kontaktů a databázi služeb aplikaci ASP.NET ke správě ověřování a autorizace.
- **ContactManager.Database**. Toto je databázový projekt sady Visual Studio 2010. Projekt definuje schéma pro databázi, že úložiště kontaktní údaje.
- **ContactManager.Service**. Toto je projekt webové služby WCF. WCF zpřístupňuje koncový bod, který umožňuje volajícím provádět vytvořit, načíst, aktualizace a odstranění (CRUD) operací v databázi Správce kontaktů. Služba závisí na databázi Správce kontaktů a ContactManager.Common.dll sestavení.
- **ContactManager.Common**. Toto je projekt knihovny tříd. Služba WCF spoléhá na typy definované v tomto sestavení.

Z prvního kurzu této série se poskytuje úplný přehled řešení a jeho požadavky na nasazení [nasazení webu v podniku](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Úlohy nasazení

Existují několik různých úloh potřebných k nasazení aplikací do různých prostředí ve velké organizaci. Toto jsou klíčové úkoly, které jsou určené kurzy:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Tady je seznam jednotlivých kroků v procesu nasazení z pohledu uživatele je popsáno dříve v tomto dokumentu:

1. Všichni členové týmu, projděte si řešení Správce kontaktů v sadě Visual Studio 2010 k určení klíče nasazení požadavky a problémy.
2. Matt Hink můžou nasadit do testovacího prostředí pro vývojáře, provádět počáteční test logiky nasazení řešení Správce kontaktů přímo z pracovní stanice pro vývojáře.
3. Matt Hink přidá aplikaci do správy zdrojového kódu v sadě TFS.
4. Rob Walters vytváří různé definice sestavení pro řešení Správce kontaktů v Team Build. K nasazení řešení do testovacího prostředí pro vývojáře, pokaždé, když se uživatel vrátí nový kód používá jednu definici sestavení CI. Jiné definice sestavení umožňuje uživatelům aktivační události nasazení do přípravného prostředí podle potřeby.
5. Pokaždé, když uživatel vrátí nový kód, Team Build automaticky vytvoří komponenty řešení, spustí testy jednotek a nasadí řešení pro testovací prostředí pro vývojáře je-li sestavení úspěšné a testy jednotek prošly.
6. Když uživatel spustí nasazení do přípravného prostředí, řešení zabalit a nasadit v procesu krokování. Tento proces také vygeneruje balíček pro ruční nasazení do produkčního prostředí.
7. Lisa Andrews nasadí aplikaci do produkčního prostředí ručně importováním webového balíčku vytvořili v kroku 6.

### <a name="key-deployment-issues"></a>Problémy s nasazením klíče

Řešení Správce kontaktů a scénář společnosti Fabrikam, Inc. zvýraznění různé běžné problémy a výzvy, které mohou nastat, když nasazujete složitá řešení na podnikové úrovni. Příklad:

- Musíte být schopni nasadit projekty do různých prostředí, jako je pro vývojáře nebo testovací prostředí, pracovní platformy a produkční servery. Řešení musí být nasazený s jinou konfiguraci nastavení pro každé prostředí.
- Je třeba nasadit více závislých projektů současně jako součást jednoho kroku nebo automatizovaného sestavení a nasazení procesu.
- Potřebujete mít možnost pro nasazení jednotky z automatizovaného procesu. Například chcete použít procesu CI k nasazení webové aplikace do přípravného prostředí při nového kódu se změnami.
- Musíte být schopni řízení procesu nasazení a nastavení nasazení proměnných mimo aplikaci Visual Studio, jak vývojáři je nepravděpodobné, že máte správnou konfiguraci nastavení nebo potřebné přihlašovací údaje pro každé cílové prostředí.
- Budete muset nasadit projekty založené na schématu databáze a zachovat stávající data na další nasazení.
- Potřebujete k nasazení databází členství na základě ad hoc bez nasazení zásady dat účtu uživatele. Také budete muset aktualizovat schéma databáze členství v nasazeném bez ztráty stávajících dat účtu uživatele.
- Je třeba vyloučit určité soubory nebo složky při nasazení obsahu do různých cílových prostředích.

Kromě toho Správa nasazení jsou časté a přírůstkové aktualizace vyvolá si některé další běžné problémy. Příklad:

- Pokaždé, když vývojář vrátí nový kód pro spuštění testů jednotek. Chcete nasadit řešení, pokud kód projde testy jednotek.
- Když nasadíte webovou aplikaci pro testovací nebo produkční prostředí, kterou chcete přesměrovat uživatele na *aplikace\_offline.htm* soubor po dobu trvání procesu nasazení.
- Chcete se přihlásit aktivity nasazení. Proces nasazení by měli poslat e-mailová oznámení o úspěšném nebo neúspěšném nasazení do určeným příjemcům.
- Pokud automatizované nasazení se nezdaří, procesu nasazení by měl pokusem o aktuálním nasazení nebo místo toho nasadit předchozí webového balíčku.

> [!div class="step-by-step"]
> [Předchozí](deploying-web-applications-in-enterprise-scenarios.md)
> [další](application-lifecycle-management-from-development-to-production.md)
