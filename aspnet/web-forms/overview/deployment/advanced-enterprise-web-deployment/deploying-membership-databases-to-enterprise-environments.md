---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Nasazení databází členství do podnikových prostředí | Dokumentace Microsoftu
author: jrjlee
description: Toto téma vysvětluje klíčové aspekty a výzvy, které je potřeba vyřešit při zřízení databáze služby aplikace technologie ASP.NET (Další běžné...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 432951b54fc7cc6b0384dfb4dbd255b16a546e76
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370323"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Nasazení databází členství do podnikových prostředí
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma vysvětluje klíčové aspekty a problémy, které že je potřeba vyřešit při zřízení aplikace ASP.NET služby databáze (více obvykle označuje jako databáze členství) v testovacím, přípravném nebo produkčním prostředí. Popisuje také přístupů, které vám umožní splnit tyto výzvy.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Co jsou problémy při nasazení databáze členství?

Ve většině případů navrhnout strategie nasazení pro databázi, první věc, kterou je potřeba zvážit při jaká data chcete nasadit. V prostředí vývoj nebo testování můžete chtít nasadit dat účtu uživatele k usnadnění rychlé a jednoduché testování. V testovací nebo produkční prostředí je velmi pravděpodobné, že chcete nasadit dat účtu uživatele.

Bohužel databází členství technologie ASP.NET představují některé konkrétní běžné problémy, které lépe rozhodnout mnohem složitější:

- Nasazení pouze se schématy ponechá databázi členství v nefunkčním stavu. Důvodem je, že databáze členství zahrnuje některé konfigurační data (v **aspnet\_SchemaVersions** tabulky), která vyžaduje databázi mohl fungovat. V důsledku toho pokud provádíte nasazení jen schéma databáze členství aby bylo možné vyloučit dat účtu uživatele, bude potřeba spustit skript po nasazení můžete přidat data základní konfigurace.
- V závislosti na konfiguraci databáze členství může zprostředkovatel členství použít klíč počítače můžete šifrovat hesla a uložit je v databázi. V takovém případě dat účtu uživatele, které nasazujete s databází už nepůjdou použít na cílovém serveru. Z tohoto důvodu nasazení dat účtu uživatele není podporováno.

## <a name="choosing-a-membership-database-strategy"></a>Výběr strategie databáze členství

Když zvolíte, jak zřídit databáze členství v podnikovém prostředí serveru, použijte tyto pokyny:

- Kdykoli je to možné, nenasazujte databáze členství. Místo toho vytvořte databázi členství ručně na cílovém serveru databáze. Pokud jste nepřizpůsobili schéma databáze členství, můžete jednoduše vytvořit nový na místě v cílovém pomocí [nástroj pro registraci serveru SQL technologie ASP.NET (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Pokud nemáte žádnou možnost, ale chcete-li nasadit databázi členství&#x2014;například, pokud jste provedli rozsáhlé změny schématu databáze&#x2014;byste měli provést nasazení jen schéma databáze členství pro vyloučení dat účtu uživatele a pak Spusťte skript po nasazení můžete přidat všechny požadované konfigurace. Široký pokyny najdete v těchto přístupů v [postupy: nasazení ASP.NET členství databáze bez včetně uživatelských účtů](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Je důležité nezapomenout, že *schéma databáze členství by mohla být poměrně statické*. I když jste přizpůsobili databáze členství, je pravděpodobné, že je potřeba aktualizovat schéma v pravidelných intervalech&#x2014;nepůjde změnit s stejně často jako kód v webovou aplikaci nebo projekt databáze. Jako takové neměli byste potřebovat zahrnout všechny procesy nasazování automatických nebo krokování databáze členství.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Pomocí VSDBCMD, a aktualizovat schéma databáze členství

Pokud změníte strukturu databáze členství po prvním nasazení, nemusí chtít použít nástroj pro nasazení Internetové informační služby (IIS) webu (nasazení webu) k opětovnému nasazení databáze. Funkce nasazení databází v nasazení webu nezahrnuje schopnost zajistit rozdílové aktualizace do cílové databáze&#x2014;místo toho musíte Webdeploy vyřadit a znovu vytvořit databázi. To znamená, že ztratíte žádná existující účet data uživatelů, což je obvykle nežádoucí v přípravném nebo produkčním prostředí.

Alternativou je použití nástroje VSDBCMD aktualizovat schéma cílové databáze. VSDBCMD obsahuje dvě důležité funkce. Nejprve ho můžete importovat schéma z existující databáze do souboru .dbschema. Za druhé je možné nasadit .dbschema souboru k existující databázi jako rozdílové aktualizace, což znamená, že umožňuje pouze změny požaduje, aby aktuální cílovou databázi a neztratila žádná data.

Postup vysoké úrovně můžete aktualizovat schéma databáze členství:

1. Použít VSDBCMD **Import** akce pro generování .dbschema soubor pro zdrojové databáze členství. Tento postup je popsaný v [postupy: Import schématu z příkazového řádku](https://msdn.microsoft.com/library/dd172135.aspx).
2. Použít VSDBCMD **nasadit** akce k nasazení souboru .dbschema na vaše cílové databáze členství. Tento postup je popsaný v [Reference k příkazovému řádku pro VSDBCMD. Soubor EXE (nasazení a Import schématu)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Závěr

Toto téma popisuje některé výzvy, které mohou nastat, když je potřeba zřídit databáze členství technologie ASP.NET v různých cílových prostředích. Zejména vysvětlit, proč nasazení pouze se schématy ponechá databázi členství v nefunkčním stavu a proč nasazení dat účtu uživatele se nepodporuje. Téma také uvedené pokyny o tom, jak zřizovat, nasazovat a aktualizovat členství databáze v různých scénářích.

## <a name="further-reading"></a>Další čtení

Další pokyny a příklady, jak používat VSDBCMD najdete v tématu [Reference k příkazovému řádku pro VSDBCMD. Soubor EXE (nasazení a Import schématu)](https://msdn.microsoft.com/library/dd193283.aspx) a [postupy: Import schématu z příkazového řádku](https://msdn.microsoft.com/library/dd172135.aspx). Další informace o používání aspnet\_regsql.exe k vytváření databází členství, naleznete v tématu [nástroj pro registraci serveru SQL technologie ASP.NET (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Obecnější informace o nasazení databází členství najdete v tématu [postupy: nasazení ASP.NET členství databáze bez včetně uživatelských účtů](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Předchozí](deploying-database-role-memberships-to-test-environments.md)
> [další](excluding-files-and-folders-from-deployment.md)
