---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Nasazení databáze členství do podnikových prostředích | Microsoft Docs
author: jrjlee
description: Toto téma vysvětluje klíčové faktory a problémy, které budete potřebovat k překonání při zřizování databázích služby pro aplikace ASP.NET (Další běžné...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: b783fcf57759f2a431480eec6902105f6d683408
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892475"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Nasazení databáze členství v podnikových prostředích
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma vysvětluje klíčové faktory a problémy, které že budete muset vyřešit při zřizování aplikace ASP.NET služeb databáze (více obvykle označuje jako databáze členství) v testu, pracovním nebo produkčním prostředí. Popisuje také přístupy, které můžete použít ke splnění tyto problémy.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz řady používá ukázkové řešení&#x2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3, komunikaci Windows Služba Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízené procesu sestavení dva soubory projektu&#x2014;jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Jaké jsou problémy při nasazení databáze členství?

Ve většině případů při navrhnout strategie nasazení pro databázi, je první věcí, kterou je potřeba zvážit jaká data, kterou chcete nasadit. Ve vývojovém nebo testovacím prostředí můžete chtít nasadit data účtu uživatele k usnadnění rychlé a jednoduché testování. V pracovním nebo produkčním prostředí je velmi nepravděpodobné, že chcete nasadit data účtu uživatele.

Bohužel databáze členství technologie ASP.NET znamenat určité výzvy, na které lépe rozhodnout mnohem složitější:

- Pouze pro schéma nasazení ponechá databázi členství v nefunkční stavu. Důvodem je, že databáze členství zahrnuje některé konfigurační data (v **aspnet\_SchemaVersions** tabulky), databáze vyžaduje, aby mohl fungovat. Jako takový Pokud provádíte nasazení pouze pro schéma databáze členství chcete-li vyloučit data účtu uživatele, musíte spustit skript po nasazení základní konfigurační data přidat do.
- V závislosti na konfiguraci databáze členství může zprostředkovatel členství pomocí klíče počítače k šifrování hesla a uložit je do databáze. Účet data uživatelů, které můžete nasadit s databází v takovém případě bude nepoužitelný na cílovém serveru. Z tohoto důvodu nasazení data účtu uživatele není podporováno.

## <a name="choosing-a-membership-database-strategy"></a>Výběr strategie databáze členství

Když zvolíte jak zřídit databáze členství v podnikovém prostředí serveru, použijte tyto pokyny:

- Pokud je to možné, nenasazujte databáze členství. Místo toho vytvořte databázi členství ručně na cílovém serveru databáze. Pokud jste schéma databáze členství nepřizpůsobili, můžete jednoduše vytvořit novou na místě v cílovém pomocí [ASP.NET nástroj pro registraci serveru SQL Server (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Pokud nemáte žádnou možnost, ale chcete nasadit databázi členství&#x2014;například, pokud jste provedli rozsáhlé změny schématu databáze&#x2014;byste měli provést nasazení pouze pro schéma databáze členství, vyloučit data účtu uživatele a potom Spusťte skript po nasazení pro všechny požadované konfigurační data přidat. Obecné pokyny najdete v těchto přístupů [postupy: nasazení ASP.NET členství databáze bez včetně uživatelských účtů](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Je důležité nezapomenout, že *schéma databáze členství je pravděpodobně poměrně statické*. I v případě, že jste přizpůsobili databázi členství, nepravděpodobné, že budete muset aktualizovat schéma v pravidelných intervalech&#x2014;nepůjde změnit často jako kód na webové aplikace nebo projekt databáze. Jako takový neměli byste potřebovat zahrnout do všech procesů nasazení automatizované nebo krokování databáze členství.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Pomocí VSDBCMD a aktualizovat schéma databáze členství

Pokud změníte strukturu databáze členství po prvním nasazení, nemusíte chtít použít nástroj nasazení webu Internetové informační služby (IIS) (Web Deploy) k opětovnému nasazení databáze. Funkce nasazení databáze v nasazení webu nezahrnuje schopnost zajistit rozdílové aktualizace do cílové databáze&#x2014;místo toho musíte nasazení webu vyřadit a znovu vytvořit databázi. To znamená, že přijdete o stávající data účtu uživatele, který je obvykle žádoucí, v pracovním nebo produkčním prostředí.

Alternativou je použití nástroje VSDBCMD aktualizovat schéma cílové databáze. VSDBCMD obsahuje dvě důležité funkce. Nejprve ji můžete importovat do souboru .dbschema schéma existující databázi. Druhý ho můžete nasadit .dbschema souboru k existující databázi jako rozdílové aktualizace, což znamená, že se provádí pouze změny nezbytné k přivedení aktuální cílovou databázi a Neztraťte žádná data.

Chcete-li aktualizovat schéma databáze členství můžete těchto kroků:

1. Použít VSDBCMD **Import** akce generovat soubor .dbschema pro vaše zdrojové databáze členství. Tento postup je popsaný v [postupy: Import schématu z příkazového řádku](https://msdn.microsoft.com/library/dd172135.aspx).
2. Použít VSDBCMD **nasadit** akce k nasazení souboru .dbschema na vaše cílové databáze členství. Tento postup je popsaný v [Reference k příkazovému řádku pro VSDBCMD. EXE (nasazení a Import schématu)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Závěr

Toto téma popisuje některé z problémů, které může být vystaven když potřebujete zřídit databáze členství technologie ASP.NET v různých prostředích cíl. Konkrétně vysvětlit, proč pouze pro schéma nasazení ponechá databázi členství v nefunkční stavu a proč není podporováno nasazení data účtu uživatele. Téma také zobrazí pokyny o tom, jak zřídit, nasazení a aktualizaci databáze členství v různých scénářích.

## <a name="further-reading"></a>Další čtení

Další pokyny a příklady použití VSDBCMD najdete v tématu [Reference k příkazovému řádku pro VSDBCMD. EXE (nasazení a Import schématu)](https://msdn.microsoft.com/library/dd193283.aspx) a [postupy: Import schématu z příkazového řádku](https://msdn.microsoft.com/library/dd172135.aspx). Další informace o používání aspnet\_regsql.exe k vytváření databází členství, najdete v části [ASP.NET nástroj pro registraci serveru SQL Server (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Další obecné pokyny pro nasazení databáze členství, najdete v části [postupy: nasazení ASP.NET členství databáze bez včetně uživatelských účtů](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Předchozí](deploying-database-role-memberships-to-test-environments.md)
> [další](excluding-files-and-folders-from-deployment.md)
