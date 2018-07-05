---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Nasazení členství v databázových rolích do testovacího prostředí | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak přidat uživatelské účty do databázových rolí jako součást nasazení řešení do testovacího prostředí. Když nasadíte řešení obsahující...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 42ceaa654c7f690f8455c0569e347cd5e9fccf7a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401857"
---
<a name="deploying-database-role-memberships-to-test-environments"></a>Nasazení členství v databázových rolích do testovacího prostředí
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak přidat uživatelské účty do databázových rolí jako součást nasazení řešení do testovacího prostředí.
> 
> Když nasadíte řešení obsahující databázový projekt do testovací nebo produkční prostředí, obvykle nechcete pro vývojáře k automatizaci přidání uživatelských účtů do databázové role. Ve většině případů vývojář nepoznáte uživatelské účty, které je potřeba přidat do které databázových rolí a tyto požadavky může kdykoli změnit. Při nasazení řešení obsahujícího projekt databáze na vývojovém nebo testovacím prostředí, situace se však obvykle raději liší:
> 
> - Vývojáři obvykle znovu nasadí řešení v pravidelných intervalech, často několikrát za den.
> - Databáze je obvykle znovu vytvořena při každém nasazení, což znamená, že uživatelé databáze musí být vytvořen a přidán do role po každém nasazení.
> - Vývojáři obvykle má plnou kontrolu nad cílové vývojové nebo testovací prostředí.
> 
> V tomto scénáři je často výhodné a automaticky vytvořte uživatele databáze s členstvím v databázových rolích přiřadit jako součást procesu nasazení.
> 
> Klíčovým faktorem je, že tuto operaci musí být podmíněné podle cílového prostředí. Pokud nasazení provádíte pracovní nebo produkční prostředí, budete chtít tuto operaci přeskočit. Pokud nasazení provádíte do vývojář nebo testovacího prostředí, budete chtít nasadit členství v rolích bez dalšího zásahu. Toto téma popisuje jeden z přístupů, které můžete použít k řešení těchto problémů.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

Toto téma předpokládá, že:

- Použít rozdělení projektu souboru přístup k nasazení řešení, jak je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Volání VSDBCMD ze souboru projektu k nasazení projektu databáze, jak je popsáno v [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Vytvoření uživatelů databáze a přiřazení členství v rolích při nasazení projektu databáze do testovacího prostředí, bude nutné:

- Vytvořte skript Transact Structured Query Language (Transact-SQL), který provede změny nezbytné databáze.
- Vytvořte cíl Microsoft Build Engine (MSBuild), který používá nástroje sqlcmd.exe spusťte skript SQL.
- Konfigurovat soubory projektu, který má být vyvolán cíl při nasazení řešení do testovacího prostředí.

Postup pro každý z těchto postupů se zobrazí v tomto tématu.

## <a name="scripting-the-database-role-memberships"></a>Skriptování členství Role databáze

Vytvoříte třeba skript Transact-SQL v mnoha různými způsoby, a na libovolném místě zvolíte. Nejjednodušší způsob je postup vytvoření skriptu v rámci vašeho řešení v sadě Visual Studio 2010.

**Chcete-li vytvořit skript SQL**

1. V **Průzkumníka řešení** okna, rozbalte uzel projektu databáze.
2. Klikněte pravým tlačítkem **skripty** složku, přejděte na **přidat**a potom klikněte na **novou složku**.
3. Typ **Test** jako název složky a potom stiskněte klávesu Enter.
4. Klikněte pravým tlačítkem na **testovací** složku, přejděte na příkaz **přidat**a potom klikněte na tlačítko **skript**.
5. V **přidat novou položku** dialogové okno pole, dejte smysluplný název vašeho skriptu (například **AddRoleMemberships.sql**) a potom klikněte na tlačítko **přidat**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. V *AddRoleMemberships.sql* přidejte příkazů jazyka Transact-SQL, který:

    1. Vytvořte uživatele databáze pro přihlášení serveru SQL Server, který bude přístup k databázi.
    2. Přidejte uživatele databáze do rolí databáze.
7. Soubor by měl vypadat takto:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Uložte soubor.

## <a name="executing-the-script-on-the-target-database"></a>Spuštění skriptu na cílové databázi

V ideálním případě by spustíte všechny požadované skripty jazyka Transact-SQL jako součást skriptu po nasazení při nasazení projektu databáze. Skripty po nasazení není však umožňují spouštět logiky podmíněně na základě konfigurace řešení nebo vlastností sestavení. Alternativou je spuštění skriptů SQL přímo ze souboru projektu MSBuild, tak, že vytvoříte **cílové** element, který provede příkaz sqlcmd.exe. Použijte tento příkaz pro spuštění skriptu na cílové databázi:


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Další informace o možnostech příkazového řádku sqlcmd najdete v tématu [Nástroj sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).


Před vložením tento příkaz v cíli MSBuild, je potřeba zvážit, za jakých podmínek chcete skript ke spuštění:

- Cílová databáze musí existovat před změnou jeho členství v rolích. V důsledku toho je potřeba spustit tento skript *po* nasazení databáze.
- Je potřeba zahrnout podmínku tak, aby skript se spustí pouze pro testovací prostředí.
- Pokud používáte nasazení "what if"&#x2014;jinými slovy, pokud jste generování skriptů nasazení ale nejsou ve skutečnosti spouštěním&#x2014;byste neměli spouštět skript SQL.

Pokud používáte přístup soubor projektu rozdělit podle [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), jak je ukázáno v ukázkovém řešení Správce kontaktů, můžete rozdělit pokynů sestavení vašeho skriptu SQL takto:

- Veškeré požadované vlastnosti specifické pro prostředí, spolu s vlastností, která určuje, jestli se má nasadit oprávnění, by měly patřit v souboru projektu specifických pro prostředí (například *Env Dev.proj*).
- Cíl nástroje MSBuild, společně s všechny vlastnosti, které nedojde ke změně mezi prostředími cíl by měl přejít v souboru projektu univerzální (například *Publish.proj*).

V souboru projektu specifických pro prostředí musíte zadat název databázového serveru, název cílové databáze a logická vlastnost, která umožňuje uživatelům určit, jestli se má nasadit členství v rolích.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


V souboru projektu univerzální budete muset zadat umístění sqlcmd spustitelný soubor a umístění, které chcete spustit skript jazyka SQL. Tyto vlastnosti zůstane stejný bez ohledu na cílovém prostředí. Je také potřeba vytvořit cíl nástroje MSBuild k provedení příkazu sqlcmd.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Všimněte si, přidejte umístění testovaného sqlcmd spustitelného souboru jako statická vlastnost, protože to může být užitečné další cíle. Naproti tomu definujete umístění skriptu SQL a syntaxe příkazu sqlcmd jako dynamických vlastností v cíli, jako nebudou požadované předtím, než je cíl proveden. V takovém případě **DeployTestDBPermissions** cíl bude spuštěn pouze pokud jsou splněny tyto podmínky:

- **DeployTestDBRoleMemberships** je nastavena na **true**.
- Nebyl zadán uživatel **WhatIf = true** příznak.

Nakonec se nezapomeňte vyvolat cíl. V *Publish.proj* souboru, provedete tak, že přidáte do seznamu závislostí pro výchozí cíl **FullPublish** cíl. Je potřeba zajistit, aby **DeployTestDBPermissions** target není spuštěn až **PublishDbPackages** byl cíl spuštěn.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Závěr

Toto téma popisuje jeden ze způsobů, ve kterém můžete přidat databázové uživatele a členství v rolích jako akci po nasazení při nasazení projektu databáze. To je obvykle užitečné, když budete pravidelně znovu vytvořit databázi v testovacím prostředí, ale je obvykle třeba zabránit, při nasazování databází do pracovní nebo produkční prostředí. V důsledku toho se ujistěte, že používáte nezbytné podmíněnou logiku tak, aby databáze uživatele a členství v rolích se vytvoří pouze v případě je to vhodné.

## <a name="further-reading"></a>Další čtení

Další informace o použití VSDBCMD nasazení databázových projektů, naleznete v tématu [nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md). Pokyny k přizpůsobení nasazené databáze pro jiné cílové prostředí najdete v tématu [přizpůsobení nasazené databáze pro více prostředí](customizing-database-deployments-for-multiple-environments.md). Další informace o používání vlastní soubory projektu MSBuild k řízení procesu nasazení najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Další informace o možnostech příkazového řádku sqlcmd najdete v tématu [Nástroj sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Předchozí](customizing-database-deployments-for-multiple-environments.md)
> [další](deploying-membership-databases-to-enterprise-environments.md)
