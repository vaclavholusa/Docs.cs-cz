---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Nasazení databázové projekty | Microsoft Docs
author: jrjlee
description: 'Poznámka: V mnoha podnikové scénáře nasazení, je třeba možnost publikovat přírůstkové aktualizace nasazené databáze. Alternativou je znovu vytvořit...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: a0b3871ea098b549271bce2b9d5f0c24f9ca8a9c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882498"
---
<a name="deploying-database-projects"></a>Databázové projekty nasazení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> V mnoha podnikové scénáře nasazení je třeba možnost publikovat přírůstkové aktualizace nasazené databáze. Alternativou je znovu vytvořit databázi na každém nasazení, což znamená, že přijdete o všechna data v existující databázi. Při práci s Visual Studio 2010 pomocí VSDBCMD je doporučeným přístupem k přírůstkové publikování databáze. Další verze sady Visual Studio a webové publikování kanálu (WPP) však bude obsahovat nástrojů, který podporuje přírůstkové publikování přímo.


Pokud otevřete obraťte se na správce ukázkové řešení v sadě Visual Studio 2010, uvidíte, že k databázovému projektu obsahuje vlastnosti složky, která obsahuje čtyři soubory.

![](deploying-database-projects/_static/image1.png)

Společně s souboru projektu (*ContactManager.Database.dbproj* v tomto případě), tyto soubory ovládat různé aspekty procesu sestavení a nasazení:

- *Database.sqlcmdvars* soubor poskytuje hodnoty pro všechny proměnné SQLCMD použít při nasazení projektu. Každé řešení konfiguraci (například ladění a vydání) můžete zadat různé .sqlcmdvars souboru.
- *Database.sqldeployment* soubor poskytuje nastavení pro konkrétní nasazení, jako jestli se má používat kolaci definované v projektu nebo kolaci na cílovém serveru, jestli se má znovu vytvořit cílové databázi každých čas nebo jednoduše upravit existující databázi a převeďte ho do aktuální a tak dále. Každé řešení konfiguraci můžete zadat různé .sqldeployment souboru.
- *Database.sqlpermissions* soubor je dokument XML, který můžete použít k definování všechna oprávnění, které chcete přidat na cílovou databázi. Všechny konfigurace řešení sdílet stejný soubor .sqlpermissions.
- *Database.sqlsettings* soubor Určuje úroveň databáze vlastnosti, které chcete použít při vytváření databáze, jako je kolaci na použití chování relační operátory a tak dále. Všechny konfigurace řešení sdílet stejný soubor .sqlsettings.

Je vhodné věnovat chvíli k otevření těchto souborů v sadě Visual Studio a seznamte se s obsahem.

Při sestavování projektu databáze procesu sestavení vytvoří dva soubory:

- A *schéma databáze* (.dbschema soubor). Popisuje schéma databáze, kterou chcete vytvořit ve formátu XML.
- A *– manifest nasazení* (.deploymanifest soubor). Tato položka obsahuje všechny informace požadované pro vytvoření a nasazení databáze. Odkazuje na soubor .dbschema spolu s jiným prostředkům, jako jsou pokyny k nasazení (soubor .sqldeployment) a všechny skripty SQL před nasazením nebo po nasazení.

To ukazuje vztah mezi tyto prostředky:

![](deploying-database-projects/_static/image2.png)

Jak vidíte, .sqlsettings soubor a soubor .sqlpermissions jsou vstupy do procesu sestavení. Projekt souborem databáze jsou tyto soubory použít k vytvoření souboru schématu databáze. .Sqldeployment soubor a soubor .sqlcmdvars předávat procesu sestavení beze změny. Manifest nasazení určuje umístění schématu databáze, .sqldeployment souboru, soubor .sqlcmdvars a všechny skripty SQL před nasazením nebo po nasazení.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Proč používat VSDBCMD nasadit projekt databáze?

Existují různé různé přístupy k nasazení databázové projekty. Ne všechny z nich jsou však vhodné pro nasazení databáze projektu na vzdálených serverech v podnikovém prostředí. Zvažte, co chcete z projektu nasazení databáze. V podnikové scénáře nasazení budete pravděpodobně chcete:

- Schopnost nasadit projekt databáze ze vzdáleného umístění.
- Schopnost provádět přírůstkové aktualizace existující databázi.
- Možnost zahrnout skripty před nasazením nebo skripty po nasazení.
- Možnost přizpůsobit pro prostředí s více cílové nasazení.
- Schopnost nasadit projekt databáze jako součást větší, obvykle skriptován, krokování řešení nasazení.

Existují tři hlavní přístupy, které můžete nasadit projekt databáze:

- Můžete použít funkci nasazení s typem databáze projektu v sadě Visual Studio 2010. Vytvořit a nasadit projekt databáze v sadě Visual Studio 2010, proces nasazení používá ke generování specifická pro konfiguraci sestavení na základě SQL nasazení souboru manifest nasazení. Tím se vytvoří databáze, pokud není již neexistuje nebo proveďte potřebné změny do databáze, pokud již existuje. SQLCMD.exe můžete použít ke spuštění tohoto souboru na cílovém serveru, nebo můžete nastavit sady Visual Studio vytvořte a spusťte soubor. Nevýhodou tohoto přístupu je, že máte jenom omezený kontrolu nad nastavení nasazení. Často také musíte upravit soubor nasazení nástroje SQL k zadání hodnot proměnných konkrétní prostředí. Tento přístup z počítače lze použít pouze s Visual Studio 2010 nainstalovaný a vývojář by bylo potřeba vědět a zadejte připojovací řetězce a přihlašovací údaje pro všechny cílové prostředí.
- Nástroj pro nasazení Internetové informační služby (IIS) webu (Web Deploy) můžete [nasadit databázi jako součást projektu webové aplikace](https://msdn.microsoft.com/library/dd465343.aspx). Tento přístup je však mnohem složitější, pokud chcete nasadit projekt databáze než jednoduše replikovat existující místní databázi na cílovém serveru. Můžete nakonfigurovat nasazení webu pro spuštění skriptu nasazení SQL, který generuje k databázovému projektu, ale pokud to chcete provést, musíte vytvořit vlastní soubor jako cíle pro váš projekt webové aplikace. Tento postup přidá vyžadovat značné množství složitost do procesu nasazení. Kromě toho Web Deploy přímo nepodporuje přírůstkové aktualizace existující databáze. Další informace o tento přístup, najdete v části [rozšíření kanálu publikování webové do projektu balíček databáze nasazeném souboru SQL](https://go.microsoft.com/?linkid=9805121).
- Můžete použít nástroj VSDBCMD pro nasazení databáze, pomocí schématu databáze nebo manifest nasazení. VSDBCMD.exe můžete volat z nástroje MSBuild cíl, který vám umožňuje publikování databází v rámci procesu nasazení větší, pomocí skriptu. Proměnné v souboru .sqlcmdvars a spoustu dalších vlastnosti databáze z VSDBCMD příkaz, který můžete vytvořit vlastní nastavení nasazení pro různá prostředí bez vytvoření konfigurací s více sestavení můžete přepsat. VSDBCMD poskytuje rozdílů mezi funkce, což znamená, že budou pouze potřebné změny, chcete-li zarovnat cílové databáze s svého schématu databáze. VSDBCMD také nabízí širokou škálu možností příkazového řádku, které poskytují jemně odstupňovanou kontrolu nad procesu nasazení.

Z tohoto přehledu můžete zjistit, že pomocí nástroje MSBuild VSDBCMD je nejvhodnější pro použití v typické podnikové nasazení:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Podporuje vzdálené nasazení? | Ano | Ano | Ano |
| Podporuje přírůstkové aktualizace? | Ano | Ne | Ano |
| Podporuje skripty před nebo po-deployment? | Ano | Ano | Ano |
| Podporuje nasazení s více prostředí? | Omezené | Omezené | Ano |
| Podporuje skriptování nasazení? | Omezené | Ano | Ano |

Zbývající část tohoto tématu popisuje použití VSDBCMD pomocí nástroje MSBuild nasazení databázové projekty.

## <a name="understanding-the-deployment-process"></a>Principy procesu nasazení

Nástroj VSDBCMD umožňuje nasadit databázi pomocí schématu databáze (soubor .dbschema) nebo nasazení manifestu (soubor .deploymanifest). V praxi budete téměř vždy používat manifest nasazení jako manifest nasazení, můžete zadat výchozí hodnoty pro různé vlastnosti nasazení a identifikovat všechny před nasazením nebo po nasazení skripty SQL, které chcete spustit. Například tento příkaz VSDBCMD se používá k nasazení **ContactManager** databáze pro databázový server v testovacím prostředí:


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


V tomto případě:

- **/A** (nebo **/Action**) přepínač určuje, co chcete udělat VSDBCMD. Můžete nastavit na **Import** nebo **nasadit**. **Import** možnost se používá ke generování souboru .dbschema z existující databáze a **nasadit** možnost se používá k nasazení souboru .dbschema na cílovou databázi.
- **/Manifest** (nebo **/ManifestFile**) přepínač identifikuje .deploymanifest soubor, který chcete nasadit. Pokud chcete místo toho použijte soubor .dbschema, byste použili **/modelu** (nebo **/ModelFile**) přepínače.
- **/Cs** (nebo **/ConnectionString**) přepínač poskytuje připojovací řetězec pro cílový server databáze. Všimněte si, že to neobsahuje název databáze&#x2014;VSDBCMD potřebuje připojit k serveru k vytvoření databáze; není třeba připojení k databázi jednotlivých. Pokud vaše .deploymanifest soubor obsahuje připojovací řetězec, můžete vynechat tento přepínač. Pokud chcete přesto použít přepínač, hodnota přepínače přepíše hodnotu .deploymanifest.
- <strong>/P:TargetDatabase</strong> vlastnost poskytuje název, kterou chcete přiřadit k cílové databázi na vytvoření. Přepíše hodnotu <strong>TargetDatabase</strong> vlastnost v souboru .deploymanifest. Můžete použít <strong>/p:</strong> <em>[název vlastnosti]</em>syntaxe nastavit celou řadu vlastností nasazení a přepsat všechny proměnné SQLCMD deklarován v souboru .sqlcmdvars.
- **/Dd+** (nebo **/DeployToDatabase+**) přepínač označuje, že chcete vytvořit nasazení a nasadíte ho do cílové prostředí. Pokud zadáte **/dd-**, nebo vynechejte přepínač, VSDBCMD vygeneruje skript nasazení, ale nebude nasazení na cílovém prostředí. Tento přepínač je často zdroji záměny a je vysvětlené podrobněji v další části.
- **/Script** (nebo **/DeploymentScriptFile**) přepínač určuje, kde chcete vygenerovat skript nasazení. Tato hodnota nemá vliv na proces nasazení.

Další informace o VSDBCMD najdete v tématu [Reference k příkazovému řádku pro VSDBCMD. EXE (nasazení a Import schématu)](https://msdn.microsoft.com/library/dd193283.aspx) a [postupy: Příprava databáze pro nasazení z příkazového řádku pomocí VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Příklad použití VSDBCMD ze souboru projektu nástroje MSBuild, naleznete v části [Principy procesu sestavení](understanding-the-build-process.md). Příklady, jak nakonfigurovat nastavení nasazení databáze pro prostředí s více najdete v tématu [přizpůsobení nasazení databáze pro prostředí s více](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Seznámení s přepínačem DeployToDatabase

Chování **/dd** nebo **/DeployToDatabase** přepínač závisí na tom, jestli používáte VSDBCMD s .dbschema soubor nebo soubor .deploymanifest. Pokud používáte soubor .dbschema, toto chování je přímočará:

- Pokud zadáte **/dd+** nebo **/dd**, VSDBCMD vygenerovat skript nasazení a nasazení databáze.
- Pokud zadáte **/dd-** nebo vynechejte přepínač, VSDBCMD vydá skript nasazení.

Pokud používáte soubor .deploymanifest, chování je mnohem složitější. Důvodem je, že soubor .deploymanifest obsahuje název vlastnosti **DeployToDatabase** , také určuje, zda je nasazení databáze.


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


Hodnota této vlastnosti je nastavena podle vlastnosti k databázovému projektu. Pokud nastavíte **nasazení akce** k **vytvořit skript nasazení (.sql)**, bude hodnota **False**. Pokud nastavíte **nasazení akce** k **vytvořit skript nasazení (.sql) a nasadit do databáze**, bude hodnota **True**.

> [!NOTE]
> Tato nastavení jsou přidružené k určité sestavení konfigurace a platformy. Například pokud nakonfigurujete nastavení pro **ladění** konfigurace a pak publikovat pomocí **verze** konfigurace, nastavení se nepoužijí.


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> V tomto scénáři **nasazení akce** musí být vždy nastavená na **vytvořit skript nasazení (.sql)**, protože nechcete, aby Visual Studio 2010 nasazení vaší databáze. Jinými slovy **DeployToDatabase** vlastnost by měla být vždy **False**.


Při **DeployToDatabase** vlastnost zadána, **/dd** přepínač pouze přepíše vlastnost, pokud je hodnota vlastnosti **false**:

- Pokud **DeployToDatabase** vlastnost je **False**, a zadáte **/dd+** nebo **/dd**, přepíše VSDBCMD  **DeployToDatabase** vlastnost a nasazení databáze.
- Pokud **DeployToDatabase** vlastnost je **False**, a zadáte **/dd-** nebo vynechejte přepínači, nebudou VSDBCMD nasazení databáze.
- Pokud **DeployToDatabase** vlastnost je **True**, VSDBCMD bude ignorovat přepínač a nasazení databáze.
- Skript nasazení se generuje v každém případě bez ohledu na to, jestli nasazujete také databáze.

## <a name="conclusion"></a>Závěr

Toto téma poskytuje přehled procesu sestavení a nasazení pro databázové projekty v sadě Visual Studio 2010. Také popisuje, jak používat VSDBCMD.exe pomocí nástroje MSBuild pro podporu nasazení databáze podnikovém měřítku.

Další informace o tom, jak to funguje v praxi, najdete v části [přizpůsobení nasazení databáze pro prostředí s více](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Další čtení

Informace o tom, jak přizpůsobit tak, že vytvoříte samostatné nasazení konfiguračního souboru pro jednotlivá prostředí nasazení databáze najdete v tématu [přizpůsobení nasazení databáze pro prostředí s více](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Pokyny ke konfiguraci členství v rolích databáze spuštěním skriptu po nasazení najdete v tématu [nasazení členství Role databáze pro testovací prostředí](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Pokyny týkající se správy některé jedinečné výzvy členství databází chce použít, najdete v části [nasazení databáze členství v podnikových prostředích](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Tato témata na webu MSDN poskytují širší pokyny a obecné informace o sadě Visual Studio databázové projekty a proces nasazení databáze:

- [Visual Studio 2010 SQL Server Database Projects](https://msdn.microsoft.com/library/ff678491.aspx)
- [Správa změn databáze](https://msdn.microsoft.com/library/aa833404.aspx)
- [Postupy: Příprava databáze pro nasazení z příkazového řádku pomocí VSDBCMD. SOUBOR EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Přehled databáze sestavení a nasazení](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Předchozí](deploying-web-packages.md)
> [další](creating-and-running-a-deployment-command-file.md)
