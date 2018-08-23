---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Pokročilé nasazení podnikového webu | Dokumentace Microsoftu
author: jrjlee
description: Tomto kurzu se dozvíte, jak k provádění různých úloh, které jsou požadované, nebo žádoucí řadu podnikových scénářích nasazení. Pro Itálii translati...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 34f1bf3bc2c37afc66f458a60a29fe5ce8f6c018
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756856"
---
<a name="advanced-enterprise-web-deployment"></a>Pokročilé nasazení podnikového webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Tomto kurzu se dozvíte, jak k provádění různých úloh, které jsou požadované, nebo žádoucí řadu podnikových scénářích nasazení.
> 
> Italština překlad těchto kurzů, navštivte web [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


To je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) řešení&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="scenario-overview"></a>Přehled scénářů

Základní scénář pro tyto kurzy je popsaný v [nasazení podnikového webu: přehled scénářů](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Doporučujeme, abyste si toto téma před zahájením práce v tomto kurzu.

## <a name="how-to-use-this-tutorial"></a>Jak používat v tomto kurzu

- Každé z témat v tomto kurzu je samostatný a řeší konkrétní problém nebo problém, který se nachází v podnikových scénářích nasazení. Není nutné pro seznámení se základními těchto témat v libovolném pořadí. Tento kurz se zabývá však některé pokročilé úlohy. V důsledku toho můžete byste se seznámit s koncepty a techniky, které [nasazení webu v podniku](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) kurz se zabývá cílem získat co nejvíce výhod z tohoto obsahu.
- Tento kurz obsahuje následující témata:
- [Provádění nasazení "What If"](performing-a-what-if-deployment.md). V možných scénářů budete chtít určit dopad navrhované nasazení v cílovém prostředí nebo jakýkoli existující obsah předtím, než ve skutečnosti provedete nějaké změny. Toto téma popisuje, jak spustit nasazení "what if" ke generování skriptů pro aktualizaci databáze a soubory protokolu, jako kdyby měl nasazení obsahu do cílového prostředí, bez provedení změn, ve skutečnosti. Analýza těchto prostředků může pomoci odhalit potenciální problémy s předstihem živé nasazení.
- [Přizpůsobení nasazené databáze pro více prostředí](customizing-database-deployments-for-multiple-environments.md). Když nasadíte databázový projekt do více cílů, budete často chcete přizpůsobit vlastnosti nasazení pro každé cílové prostředí. Například v testovacích prostředích je by obvykle znovu vytvořit databázi při každém nasazení, že v přípravném nebo produkčním prostředí by bylo mnohem větší pravděpodobnost Ujistěte se, přírůstkové aktualizace zachovat data. Toto téma popisuje, jak začlenit tyto změny vlastností do logiky nasazení tak, že vytvoříte soubor konfigurace (.sqldeployment) nasazení specifických pro prostředí pro každé cílové prostředí.
- Nasazení členství v databázových rolích do testovacího prostředí. Když znovu vytvoříte databázi při každém nasazení&#x2014;například kontinuální integrace (CI) v rámci sestavení a nasazení do testovacího prostředí&#x2014;obvykle budete muset nakonfigurovat pokaždé, když se členstvím v databázových rolích. Například budete obvykle musíte udělit oprávnění k identitě fondu aplikací, které jsou spojené s vaší webovou aplikací. Toto téma popisuje, jak tento proces automatizovat tak, že přidáte skript SQL po nasazení do logiky nasazení.
- [Nasazení databází členství do podnikových prostředí](deploying-membership-databases-to-enterprise-environments.md). Databáze členství technologie ASP.NET mají různé vlastnosti, které může zkomplikovat proces nasazení. Například pouze se schématy nasazení bude ponechat databázi v nefunkčním stavu. Ve většině případů je vhodnější přímo v každém cílovém prostředí vytvořit databázi členství. Ale pokud máte nasazení databáze členství, toto téma popisuje některé z přístupů, které můžete použít k nápravě přináší.
- [Vyloučení souborů a složek z nasazení](excluding-files-and-folders-from-deployment.md). V některých případech budete chtít přizpůsobit obsah váš webový balíček do určité cílové prostředí. Například můžete chtít zahrnout úplnou verzí knihoven jazyka JavaScript, při nasazení do testovacího prostředí, podpora ladění na straně klienta, ale použijte minifikovaný verze knihoven při nasazování do testovací nebo produkční prostředí. Toto téma popisuje, jak vyloučit určité soubory a složky z proces vytvoření balíčku.
- [Převedení webových aplikací Offline s webovým nasazení](taking-web-applications-offline-with-web-deploy.md). Když nasadíte řešení pro testovací nebo produkční prostředí, budete často chtít využít vaše webové aplikace v režimu offline po dobu trvání procesu nasazení. Toto téma popisuje, jak můžete přidat *aplikace\_offline.htm* souborů do webové aplikace při zahájení procesu nasazení a odeberte na konci. Zatímco *aplikace\_offline.htm* je soubor na místě, všichni uživatelé, kteří přejděte do webové aplikace se automaticky přesměrují na *aplikace\_offline.htm* souboru.
- [Spouštění skriptů Windows Powershellu MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Mnoho scénářů nasazení vyžaduje složitější akce po nasazení, jako jsou přidání vlastní zdroje událostí do registru nebo konfiguraci replikace mezi instancí systému SQL Server. Tyto akce jsou často proveden prostřednictvím skriptů prostředí Windows PowerShell. Toto téma popisuje, jak spouštět skripty Windows Powershellu ze souboru projektu Microsoft Build Engine (MSBuild) jako součást procesu sestavení a nasazení.
- [Řešení potíží s procesem vytváření balíčku](troubleshooting-the-packaging-process.md). Web pro publikování kanálu (WPP) definuje vlastnost MSBuild s názvem **EnablePackageProcessLoggingAndAssert** , můžete použít ke generování podrobné informace o procesu balení projektů webových aplikací. Toto téma popisuje, co vlastnost dělá a jak ji používat.

## <a name="key-technologies"></a>Klíčové technologie

Tento kurz se zaměřuje na tom, jak používat tyto produkty a technologie pro podporu automatické sestavení a nasazení webu:

- Visual Studio 2010 a Team Foundation Server (TFS) 2010
- Nástroj MSBuild a týmové sestavení TFS
- Internetová informační služba (IIS) 7.5
- Nástroj pro nasazení webové služby IIS (Web Deploy) 2.1
- Nástroj VSDBCMD.exe databáze nasazení

## <a name="other-tutorials-in-this-series"></a>Další kurzy v této sérii

To je součástí série pěti kurzů na podnikové úrovni, nasazení webu. Toto jsou další kurzy v této sérii:

- [Nasazení webové aplikace v podnikových scénářích](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Tento úvodní obsah obsahuje kontextové informace týkající se série kurzů. Popisuje scénář tohoto kurzu a ukazuje, jak se úlohy a názorné postupy popsané v celé řadě vejde do širší proces správy životního cyklu aplikací (ALM).
- [Webové nasazení v podniku](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Tento kurz obsahuje koncepční Úvod do souborů projektu MSBuild, WPP, Web Deploy a dalších souvisejících technologiích. Vysvětluje, jak můžete pomocí těchto nástrojů společně ke správě nasazení komplexních procesů.
- [Konfigurace serverového prostředí pro nasazení webu](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Tento kurz popisuje, jak nakonfigurovat servery Windows pro podporu různých scénářů nasazení, včetně nasazení vzdáleného webového balíčku pomocí Služba agenta nasazení webu (vzdálený agent) nebo obslužná rutina webového nasazení a nasazení vzdálené databáze. Poskytuje rady, jak volba metody nasazení pro konkrétní prostředí, a je zde popsán způsob použít webové farmy Framework (WFF) k replikaci nasazených webových aplikací ve všech webových serverů v serverové farmě.
- [Konfigurace serveru Team Foundation Server pro nasazení webu](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Tento kurz popisuje postup konfigurace serveru TFS pro podporu různých scénářů nasazení, včetně automatizovaného nasazení jako součást procesu CI a ručně spustit nasazení konkrétního sestavení.

> [!div class="step-by-step"]
> [Next](performing-a-what-if-deployment.md)
