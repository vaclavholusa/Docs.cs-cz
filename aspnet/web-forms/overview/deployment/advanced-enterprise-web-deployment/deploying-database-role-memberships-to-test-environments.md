---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: "Nasazení databáze členství v rolích do testovacího prostředí | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje postup přidání uživatelských účtů do role databáze v rámci řešení pro nasazení v testovacím prostředí. Při nasazení řešení obsahující..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 226c28622f76e866fba1fc33cf9b9b7a01e5295b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="deploying-database-role-memberships-to-test-environments"></a>Nasazení databáze členství v rolích do testovacího prostředí
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje postup přidání uživatelských účtů do role databáze v rámci řešení pro nasazení v testovacím prostředí.
> 
> Když nasadíte řešení obsahující projekt databáze k pracovním nebo produkčním prostředí, obvykle nechcete, aby vývojáři automatizovat přidání uživatelských účtů do role databáze. Ve většině případů vývojář nebude vědět, které uživatelské účty musí být přidán do které databázové role a tyto požadavky může kdykoli změnit. Při nasazení řešení obsahující projekt databáze na vývojovém nebo testovacím prostředí, situaci se však obvykle místo liší:
> 
> - Vývojáři obvykle znovu nasadí řešení v pravidelných intervalech, často několikrát za den.
> - Databáze se obvykle znovu vytvoří při každém nasazení, což znamená, že uživatelé databáze musí být vytvořen a přidán do role po každé nasazení.
> - Vývojáři obvykle má plnou kontrolu nad cíl vývojovém nebo testovacím prostředí.
> 
> V tomto scénáři je často užitečné k automatickému vytvoření databáze uživatele a přiřadit členství v rolích databázi jako součást procesu nasazení.
> 
> Klíčovým faktorem je, že tato operace musí být podmíněného založené na cílovém prostředí. Pokud nasazujete do pracovním nebo produkčním prostředí, chcete přeskočit operaci. Pokud jste nasazení do vývojář nebo testovací prostředí, budete chtít nasadit členství v rolích bez dalšího zásahu. Toto téma popisuje jeden z přístupů, které můžete použít, chcete-li vyřešit tento problém.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz series používá ukázkové řešení & #x 2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3 systému Windows Komunikační služby Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízené procesu sestavení dva projektu soubory & #x 2014; jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

Toto téma předpokládá, že:

- Použít přístup souboru projektu rozdělení k nasazení řešení, jak je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Volání VSDBCMD ze souboru projektu nasadit projekt databáze, jak je popsáno v [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Vytvořit databázi uživatele a přiřadit členství v rolích při nasazování databázového projektu do testovacího prostředí, musíte:

- Vytvořte skript Transact Structured Query Language (Transact-SQL), který provede změny potřebné databáze.
- Vytvořte cíl Microsoft Build Engine (MSBuild), který používá ke spuštění skriptu SQL nástroje sqlcmd.exe.
- Konfigurovat soubory projektu, který má být vyvolán cíl, pokud nasazujete řešení v testovacím prostředí.

Toto téma vám ukáže, jak provést všechny tyto postupy.

## <a name="scripting-the-database-role-memberships"></a>Skriptování členství Role databáze

Skript jazyka Transact-SQL můžete vytvořit v mnoha různými způsoby, a v žádném z umístění zvolíte. Nejjednodušší způsob je vytvořit skript v rámci vašeho řešení v sadě Visual Studio 2010.

**Chcete-li vytvořit skript SQL**

1. V **Průzkumníku řešení** okno, rozbalte uzel vaší databáze projektu.
2. Klikněte pravým tlačítkem myši **skripty** složku, přejděte na příkaz **přidat**a potom klikněte na **novou složku**.
3. Typ **Test** jako název složky a potom stiskněte klávesu Enter.
4. Klikněte pravým tlačítkem myši **Test** složku, přejděte na příkaz **přidat**a potom klikněte na **skriptu**.
5. V **přidat novou položku** dialogové okno pole, zadejte smysluplný název vašeho skriptu (například **AddRoleMemberships.sql**) a potom klikněte na **přidat**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. V *AddRoleMemberships.sql* soubor, přidejte příkazy jazyka Transact-SQL, který:

    1. Vytvořte uživatele databáze pro přihlášení systému SQL Server, který bude přistupovat k databázi.
    2. Přidejte uživatele databáze k žádné roli požadovaná databáze.
7. Soubor by měl vypadat takto:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Uložte soubor.

## <a name="executing-the-script-on-the-target-database"></a>Provádění skriptu na cílovou databázi

V ideálním případě by spustit všechny požadované skripty jazyka Transact-SQL v rámci skriptu po nasazení při nasazení projektu databáze. Skripty po nasazení však Nepovolit můžete pro zpracování logiky podmíněně na základě konfigurace řešení nebo vlastnosti sestavení. Alternativou je spustit skripty SQL přímo ze souboru projektu nástroje MSBuild, vytvořte **cíl** element, který spouští příkaz sqlcmd.exe. Tento příkaz můžete použít ke spuštění skriptu na cílovou databázi:


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Další informace o možnostech příkazového řádku sqlcmd najdete v tématu [Nástroj sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).


Před cíl MSBuild vložíte tento příkaz, je třeba vzít v úvahu, za jakých podmínek chcete spuštění skriptu:

- Cílová databáze musí existovat změnit jeho členství v rolích. Jako takový, budete muset spustit tento skript *po* nasazení databáze.
- Je nutné zahrnout podmínku tak, aby skript se spustí, pouze pro testovací prostředí.
- Pokud používáte "Co když" nasazení & #x 2014; jinými slovy, pokud jste generování skriptů nasazení, ale ve skutečnosti není spuštěna je & #x 2014; nesmí spuštěním skriptu SQL.

Pokud používáte popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), jak je předvedeno ukázkové řešení obraťte se na správce, můžete rozdělit sestavení pokyny pro váš skript SQL takto:

- Veškeré požadované vlastnosti specifické pro prostředí, společně s vlastnost, která určuje, zda chcete nasadit oprávnění, by měli přejít v souboru projektu konkrétní prostředí (například *Env Dev.proj*).
- Cíle nástroje MSBuild, společně s všechny vlastnosti, které nedojde ke změně mezi cílové prostředí, by měli přejít v souboru projektu univerzální (například *Publish.proj*).

V souboru projektu specifické pro prostředí musíte zadat název databázového serveru, název cílové databáze a vlastnost typu Boolean, který uživateli umožňuje zadat, jestli se má nasadit členství v rolích.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


V souboru projektu univerzální budete muset zadat umístění sqlcmd spustitelného souboru a umístění, které chcete spustit skript SQL. Tyto vlastnosti zůstane stejný bez ohledu na cílovém prostředí. Také musíte vytvořit cíl MSBuild ke spuštění příkazu sqlcmd.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Všimněte si, přidejte umístění spustitelného souboru sqlcmd jako pomocí statické vlastnosti, protože to může být užitečné k jiné cíle. Naproti tomu definujte umístění souboru skriptu SQL a syntaxe příkazu sqlcmd jako dynamické vlastnosti v rámci cíle, jako nebudou požadované před provedením cíl. V takovém případě **DeployTestDBPermissions** cíl bude spuštěn pouze v případě splnění těchto podmínek:

- **DeployTestDBRoleMemberships** je nastavena na **true**.
- Nebyl zadán uživatel **WhatIf = true** příznak.

Nakonec nezapomeňte vyvolání cíl. V *Publish.proj* souboru, můžete k tomu tak, že přidáte do seznamu závislostí pro výchozí cíl **FullPublish** cíl. Musíte zajistit, aby **DeployTestDBPermissions** cíl není provést, dokud **PublishDbPackages** cíl provedl.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Závěr

Toto téma popisuje jeden ze způsobů, ve kterém můžete přidat uživatele databáze a členství v rolích jako akce po nasazení při nasazení projektu databáze. To je obvykle užitečné, když je pravidelně znovu vytvořit databázi v testovacím prostředí, ale je obvykle třeba zabránit, když nasazujete databáze k pracovním nebo produkčním prostředí. Zkontrolujte jako takový pomocí potřebné logiky podmíněného, tak, aby uživatelé databáze a členství v rolích pouze vytvářejí, když je to vhodné.

## <a name="further-reading"></a>Další čtení

Další informace o použití VSDBCMD pro databázové projekty nasazení najdete v tématu [nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md). Pokyny k přizpůsobení nasazení databáze pro různé cílové prostředí najdete v tématu [přizpůsobení nasazení databáze pro prostředí s více](customizing-database-deployments-for-multiple-environments.md). Další informace o používání vlastních souborů projektu nástroje MSBuild k řízení procesu nasazení najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Další informace o možnostech příkazového řádku sqlcmd najdete v tématu [Nástroj sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

>[!div class="step-by-step"]
[Předchozí](customizing-database-deployments-for-multiple-environments.md)
[další](deploying-membership-databases-to-enterprise-environments.md)
