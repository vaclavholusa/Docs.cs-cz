---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Nasazení projektu databáze | Dokumentace Microsoftu
author: jrjlee
description: 'Poznámka: V mnoha scénářích nasazení enterprise, potřebujete mít možnost publikovat přírůstkové aktualizace nasazené databáze. Alternativou je znovu vytvořit...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 43fa197a1d5a3cf521f4d2202754ff0d121cebe3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756373"
---
<a name="deploying-database-projects"></a>Nasazení projektu databáze
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> V mnoha scénářích nasazení enterprise potřebujete mít možnost publikovat přírůstkové aktualizace nasazené databáze. Alternativou je znovu vytvořit databázi na každé nasazení, což znamená, že ztratíte všechna data v existující databázi. Pokud pracujete se sadou Visual Studio 2010, pomocí VSDBCMD je doporučený postup pro přírůstkové publikování databáze. Další verze sady Visual Studio a webové publikování kanálu (WPP) však bude obsahovat nástroje, které podporuje přírůstkové publikování přímo.


Pokud otevřete ukázkové řešení Správce kontaktů v sadě Visual Studio 2010, uvidíte, že databázový projekt obsahuje vlastnosti složky, která obsahuje čtyři soubory.

![](deploying-database-projects/_static/image1.png)

Spolu s souboru projektu (*ContactManager.Database.dbproj* v tomto případě), tyto soubory určují různé aspekty procesu sestavení a nasazení:

- *Database.sqlcmdvars* soubor obsahuje hodnoty pro všechny proměnné SQLCMD používat při nasazování projektu. Každé konfigurace řešení (například debug a release) můžete určit různé .sqlcmdvars souboru.
- *Database.sqldeployment* soubor obsahuje nastavení specifické pro nasazení, například, jestli se má používat kolaci definovaný v projektu nebo kolaci cílovém serveru, jestli se má znovu vytvořit cílové databáze každých čas nebo jednoduše změnit existující databázi a přenést až do data a tak dále. Každé konfigurace řešení můžete určit různé .sqldeployment souboru.
- *Database.sqlpermissions* soubor je dokument XML, který můžete použít k definování všechna oprávnění, které chcete přidat do cílové databáze. Všechny konfigurace řešení sdílet stejný soubor .sqlpermissions.
- *Database.sqlsettings* soubor Určuje vlastnosti databáze pro použití při vytváření databáze, jako je kolaci na použití chování operátory porovnání a tak dále. Všechny konfigurace řešení sdílet stejný soubor .sqlsettings.

Je které stojí za to, že si tyto soubory lze otevřít v sadě Visual Studio a seznamte se s obsahem.

Když vytvoříte projekt databáze, proces sestavení vytvoří dva soubory:

- A *schéma databáze* (.dbschema souboru). Popisuje schéma databáze, kterou chcete vytvořit ve formátu XML.
- A *manifest nasazení* (.deploymanifest souboru). Tato položka obsahuje všechny informace potřebné k vytvoření a nasazení vaší databáze. Odkazuje na soubor .dbschema společně s další prostředky, jako jsou pokyny k nasazení (souboru .sqldeployment) a skripty SQL před nasazením nebo po nasazení.

To znázorňuje vztah mezi tyto prostředky:

![](deploying-database-projects/_static/image2.png)

Jak je vidět, .sqlsettings soubor a soubor .sqlpermissions jsou vstupy do procesu sestavení. Chcete se souborem projektu databáze tyto soubory se používají k vytvoření souboru schématu databáze. .Sqldeployment soubor a soubor .sqlcmdvars prochází proces sestavení beze změny. Manifest nasazení označuje umístění schématu databáze, .sqldeployment souboru, soubor .sqlcmdvars a skripty SQL před nasazením nebo po nasazení.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Proč používat VSDBCMD nasadit projekt databáze?

Existují různé různé přístupy k nasazení projektu databáze. Ale ne všechny jsou vhodné pro nasazení projektu databáze na vzdálené servery v podnikovém prostředí. Zvažte, co chcete z nasazení projektu databáze. V podnikových scénářích nasazení které budete pravděpodobně vhodné:

- Možnost nasadit projekt databáze ze vzdáleného umístění.
- Možnost provádět přírůstkové aktualizace existující databáze.
- O schopnost zahrnovat skripty před nasazením nebo skripty po nasazení.
- Schopnost přizpůsobit nasazení do různých cílové prostředí.
- Možnost nasadit projekt databáze v rámci větší, obvykle skripty, krokování řešení nasazení.

Existují tři hlavní přístupy, které lze použít k nasazení projektu databáze:

- Nasazení funkce můžete použít s typem databáze projektu v sadě Visual Studio 2010. Při vytváření a nasazování databázový projekt v sadě Visual Studio 2010, procesu nasazení používá ke generování specifické pro konfiguraci sestavení nasazení na základě SQL souboru manifest nasazení. Tím se vytvoří databáze, pokud není již existuje nebo proveďte potřebné změny do databáze, pokud již existuje. Vám pomůže SQLCMD.exe spusťte tento soubor na cílový server, nebo můžete nastavit Visual Studio k vytvoření a spuštění souboru. Nevýhody tohoto přístupu je, že máte jenom omezenou kontrolu přes nastavení nasazení. Často také budete muset upravit soubor nasazení SQL k poskytování hodnot proměnných pro konkrétní prostředí. Můžete použít pouze tento přístup z počítače se sadou Visual Studio 2010 nainstalovaný a vývojář potřebuje vědět a zadejte připojovací řetězce a přihlašovací údaje pro všechny cílové prostředí.
- Vám pomůže nástroj pro nasazení Internetové informační služby (IIS) webu (nasazení webu) [nasadit databázi projektu webové aplikace v rámci](https://msdn.microsoft.com/library/dd465343.aspx). Tento přístup je však mnohem složitější, pokud chcete nasadit projekt databáze, spíše než replikovat stávající místní databázi na cílovém serveru. Můžete nakonfigurovat nasazení webu pro spuštění skriptu pro nasazení SQL, který generuje databázového projektu, ale pokud to chcete udělat, je potřeba vytvořit vlastní soubor cílů WPP pro váš projekt webové aplikace. Tento postup přidá vyžadovat značné množství složitost procesu nasazení. Kromě toho Web Deploy přímo nepodporuje přírůstkové aktualizace na stávajících databází. Další informace o tomto přístupu najdete v tématu [rozšíření Web Publishing Pipeline na databázový projekt balíčku nasazeného souboru SQL](https://go.microsoft.com/?linkid=9805121).
- Nástroj VSDBCMD můžete použít k nasazení databáze pomocí schématu databáze nebo manifest nasazení. VSDBCMD.exe můžete volat z cíle nástroje MSBuild, který umožňuje publikování databáze jako součást procesu větší a skriptované nasazení. Proměnné v souboru .sqlcmdvars a spoustu dalších vlastností databáze z VSDBCMD příkaz, který vám umožní přizpůsobit nasazení pro různá prostředí bez vytvoření několika konfiguracích sestavení můžete přepsat. VSDBCMD poskytuje funkce rozlišení, což znamená, že se provede pouze nezbytné změny zarovnání cílové databáze se schéma vaší databáze. VSDBCMD také nabízí širokou škálu možnosti příkazového řádku, které vám poskytnou přesnou kontrolu nad procesu nasazení.

Z tohoto přehledu uvidíte, že pomocí nástroje MSBuild VSDBCMD je nejvhodnější pro typické podnikové scénáře nasazení:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Podporuje vzdálené nasazení? | Ano | Ano | Ano |
| Podporuje přírůstkové aktualizace? | Ano | Ne | Ano |
| Podporuje skripty před/po-deployment? | Ano | Ano | Ano |
| Podporuje nasazení více prostředí? | Limited | Limited | Ano |
| Podporuje nasazení? | Limited | Ano | Ano |

Zbývající část tohoto tématu popisuje použití VSDBCMD pomocí nástroje MSBuild k nasazení databázové projekty.

## <a name="understanding-the-deployment-process"></a>Principy procesu nasazení

Nástroj VSDBCMD vám umožní nasadit databázi pomocí schéma databáze (soubor .dbschema) nebo manifestu nasazení (souboru .deploymanifest). V praxi téměř vždy použijete manifestu nasazení, protože manifest nasazení umožňuje zadat výchozí hodnoty pro různé vlastnosti nasazení a identifikovat všechny skripty SQL před nasazením nebo po nasazení, které chcete spustit. Třeba tento příkaz VSDBCMD slouží k nasazování **ContactManager** databáze pro databázový server v testovacím prostředí:


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


V tomto případě:

- **/A** (nebo **/Action**) přepínač určuje, co chcete VSDBCMD udělat. Tento parametr můžete nastavit **Import** nebo **nasadit**. **Import** možnost se používá ke generování souboru .dbschema z existující databáze a **nasadit** možnost se používá k nasazení souboru .dbschema do cílové databáze.
- **/Manifest** (nebo **manifestfile**) identifikuje přepínač .deploymanifest soubor, který chcete nasadit. Pokud chcete místo toho použijte soubor .dbschema, můžete využít **/model** (nebo **/ModelFile**) přepnutí.
- **Protokolovacímu** (nebo **/ConnectionString**) přepínač obsahuje připojovací řetězec pro cílový server databáze. Všimněte si, že to nebude obsahovat název databáze&#x2014;VSDBCMD potřebuje pro připojení k serveru a vytvořte databázi. není nutné se připojit k jednotlivé databáze. Pokud váš soubor .deploymanifest obsahuje připojovací řetězec, můžete vynechat tento přepínač. Pokud je i přesto použít přepínač, hodnota přepínače přepíše .deploymanifest hodnotu.
- <strong>/P:TargetDatabase</strong> vlastnosti obsahuje název, kterou chcete přiřadit k cílové databázi vytvoření. Tím se přepíše hodnotu <strong>TargetDatabase</strong> vlastnost v souboru .deploymanifest. Můžete použít <strong>/p:</strong> <em>[název vlastnosti]</em>řiďte se syntaxí nastavit celou řadu vlastností nasazení k přepsání proměnných SQLCMD deklarované v souboru .sqlcmdvars.
- **/Dd+** (nebo **/DeployToDatabase+**) přepínač označuje, že chcete nasazení taky můžete vytvořit a nasadit ho do cílového prostředí. Pokud zadáte **/dd-**, nebo vynechejte přepínač, VSDBCMD bude generovat skript nasazení, ale nebudou ji nasadit do cílového prostředí. Tento přepínač je často zdroji záměny a je vysvětlené podrobněji v další části.
- **/Script** (nebo **/DeploymentScriptFile**) přepínač určuje, ve které chcete generovat skript nasazení. Tato hodnota nemá vliv na proces nasazení.

Další informace o VSDBCMD najdete v tématu [Reference k příkazovému řádku pro VSDBCMD. Soubor EXE (nasazení a Import schématu)](https://msdn.microsoft.com/library/dd193283.aspx) a [postupy: Příprava databáze pro nasazení z příkazového řádku s použitím VSDBCMD. Soubor EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Příklad použití VSDBCMD ze souboru projektu MSBuild, naleznete v tématu [Principy procesu sestavení](understanding-the-build-process.md). Příklady, jak nakonfigurovat nastavení nasazení databáze pro více prostředí, najdete v článku [přizpůsobení nasazené databáze pro více prostředí](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Principy DeployToDatabase přepínače

Chování **/dd** nebo **/DeployToDatabase** přepínač závisí na tom, jestli používáte VSDBCMD s .dbschema soubor nebo soubor .deploymanifest. Pokud používáte soubor .dbschema, je poměrně jednoduché chování:

- Pokud zadáte **/dd+** nebo **/dd**, VSDBCMD vygenerovat skript nasazení a nasazení databáze.
- Pokud zadáte **/dd-** nebo vynechejte přepínač, VSDBCMD vygeneruje jenom skript nasazení.

Pokud používáte soubor .deploymanifest, chování je mnohem složitější. Důvodem je, že soubor .deploymanifest obsahuje název vlastnosti **DeployToDatabase** , která také určuje, zda je nasazení databáze.


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


Hodnota této vlastnosti je nastavena podle vlastnosti projektu databáze. Pokud nastavíte **nasazení akce** k **vytvořit nasazení skriptů (.sql)**, bude hodnota **False**. Pokud nastavíte **nasazení akce** k **nasazení skriptů (.sql) vytvořit a nasadit do databáze**, bude hodnota **True**.

> [!NOTE]
> Tato nastavení jsou spojeny s konkrétní sestavení konfigurace a platforma. Například, pokud nakonfigurujete nastavení **ladění** konfigurace a potom publikovat pomocí **vydání** konfigurace, nastavení se nepoužijí.


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> V tomto scénáři **nasazení akce** by měla být vždy nastavená na **vytvořit nasazení skriptů (.sql)**, protože nechcete, aby Visual Studio 2010 pro nasazení vaší databáze. Jinými slovy **DeployToDatabase** vlastnost by měla být vždy **False**.


Když **DeployToDatabase** je zadána vlastnost, **/dd** přepínači pouze přepíše vlastnost, pokud je hodnota vlastnosti **false**:

- Pokud **DeployToDatabase** je vlastnost **False**, a zadáte **/dd+** nebo **/dd**, přepíše VSDBCMD  **DeployToDatabase** vlastnost a nasazení databáze.
- Pokud **DeployToDatabase** vlastnost **False**, a zadáte **/dd-** nebo vynechejte přepínač, VSDBCMD nenasadí databáze.
- Pokud **DeployToDatabase** vlastnost **True**, VSDBCMD bude ignorovat přepínač a nasazení databáze.
- V každém případě bez ohledu na to, jestli nasazujete databáze také se vygeneruje skript nasazení.

## <a name="conclusion"></a>Závěr

Toto téma poskytuje přehled o procesu sestavení a nasazení pro databázové projekty v sadě Visual Studio 2010. Také popsáno, jak můžete VSDBCMD.exe pomocí nástroje MSBuild pro podporu nasazení databáze celého podniku.

Další informace o tom, jak to funguje v praxi, najdete v části [přizpůsobení nasazené databáze pro více prostředí](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Další čtení

Informace o tom, jak přizpůsobit tak, že vytvoříte konfigurační soubor samostatného nasazení pro jednotlivá prostředí nasazení databází najdete v tématu [přizpůsobení nasazené databáze pro více prostředí](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Pokyny ke konfiguraci členství v databázových rolích spuštěním skriptu po nasazení najdete v tématu [nasazení členstvím v databázových rolích do testovacího prostředí](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Pokyny týkající se správy některé jedinečné výzvy členství databáze uložit naleznete v tématu [nasazení databází členství do podnikových prostředí](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Tato témata na webu MSDN poskytují širší pokyny a základní informace o databázových projektů sady Visual Studio a proces nasazení databáze:

- [Visual Studio 2010 Server projekty databází systému SQL](https://msdn.microsoft.com/library/ff678491.aspx)
- [Správa změn databáze](https://msdn.microsoft.com/library/aa833404.aspx)
- [Postupy: Příprava databáze pro nasazení z příkazového řádku s použitím VSDBCMD. SOUBOR EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Přehled databáze sestavení a nasazení](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Předchozí](deploying-web-packages.md)
> [další](creating-and-running-a-deployment-command-file.md)
