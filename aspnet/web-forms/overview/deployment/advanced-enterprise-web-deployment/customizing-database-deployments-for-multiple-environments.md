---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: "Přizpůsobení nasazení databáze pro prostředí s více | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje, jak můžete nastavit vlastnosti databáze pro konkrétní cíl prostředí jako součást procesu nasazení. Poznámka: Tématu předpokládá tý..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: f3ca344c2466d9d538f55cd8ff0a5bf5b7bac808
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
<a name="customizing-database-deployments-for-multiple-environments"></a>Přizpůsobení nasazení databáze pro prostředí s více
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak můžete nastavit vlastnosti databáze pro konkrétní cíl prostředí jako součást procesu nasazení.
> 
> > [!NOTE]
> > Téma předpokládá, že nasazujete pomocí MSBuild.exe a VSDBCMD.exe databázového projektu sady Visual Studio 2010. Další informace o proč můžete zvolit tento přístup, najdete v části [nasazení webu v podnikové síti](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) a [nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Když nasadíte databázového projektu do více cílů, budete často chcete přizpůsobit vlastnosti nasazení databáze pro každé cílové prostředí. Například v testovacích prostředích je by obvykle znovu vytvořit databázi na každém nasazení, že v pracovním nebo produkčním prostředí by bylo mnohem vyšší pravděpodobnost, chcete-li zachovat data přírůstkové aktualizace.
> 
> V projektu sady Visual Studio 2010 databáze jsou nastavení nasazení obsažené v souboru konfigurace (.sqldeployment) nasazení. Toto téma vám ukáže, jak vytvořit soubory konfigurace nasazení konkrétní prostředí a určit, které chcete použít jako parametr VSDBCMD.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz series používá ukázkové řešení & #x 2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3 systému Windows Komunikační služby Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízené procesu sestavení dva projektu soubory & #x 2014; jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

Toto téma předpokládá, že:

- Použít přístup souboru projektu rozdělení k nasazení řešení, jak je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Volání VSDBCMD ze souboru projektu nasadit projekt databáze, jak je popsáno v [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Chcete-li vytvořit nasazení systém, který podporuje různých vlastnosti nasazení databáze mezi prostředími cíl, musíte:

- Vytvořte soubor konfigurace (.sqldeployment) nasazení pro každé cílové prostředí.
- Vytvořte VSDBCMD příkaz, který určuje konfigurační soubor nasazení nástroje jako přepínač příkazového řádku.
- Parametrizace příkaz VSDBCMD v souboru projektu Microsoft Build Engine (MSBuild) tak, aby VSDBCMD možnosti jsou vhodné pro cílové prostředí.

Toto téma vám ukáže, jak provést všechny tyto postupy.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Vytváření soubory konfigurace nasazení specifické pro prostředí

Výchozí nastavení projektu databáze obsahuje jedno nasazení konfiguračního souboru s názvem *Database.sqldeployment*. Pokud tento soubor otevřete v sadě Visual Studio 2010, zobrazí se možnosti jiné nasazení, které jsou k dispozici:

- **Nasazení porovnání kolace**. Tímto způsobem můžete vybrat, jestli se má používat kolaci databáze projektu ( *zdroj* kolace) nebo kolaci databáze na cílovém serveru ( *cíl* kolace). Ve většině případů budete chtít použít zdroj kolace při nasazení na vývojovém nebo testovacím prostředí. Když nasadíte k pracovním nebo produkčním prostředí, budete obvykle chcete opustit cílové kolaci beze změny, aby se zabránilo žádné problémy s interoperabilitou.
- **Nasazení vlastnosti databáze**. Tímto způsobem můžete vybrat, jestli se má použít vlastnosti databáze, jak jsou definovány v *Database.sqlsettings* souboru. Při prvním nasazení databáze, měli byste nasadit vlastnosti databáze. Pokud aktualizujete existující databázi, vlastnosti musí již být k dispozici a není třeba je znovu nasadit.
- **Vždy znovu vytvořit databázi**. Díky tomu se rozhodujete, jestli se znovu vytvořit cílová databáze při každém nasazení nebo zkontrolujte aktuální přírůstkové změny, aby cílová databáze s schéma. Pokud znovu vytvořit databázi, ztratíte všechna data v existující databázi. Jako takový by měl obvykle nastavíte **false** pro nasazení do pracovním nebo produkčním prostředí.
- **Blokovat přírůstkové nasazení, pokud může dojít ke ztrátě dat**. Tímto způsobem můžete vybrat, jestli by se měla zastavit nasazení, pokud změnu schématu databáze způsobí ztrátu dat. To obvykle nastavuje na **true** pro nasazení v produkčním prostředí, získáte možnost zasáhnout a chránit všechny důležitá data. Pokud jste nastavili **vždy znovu vytvořit databázi** k **false**, toto nastavení nebude mít žádný vliv.
- **Spouštění nasazení v režimu jednoho uživatele**. To obvykle není problém v prostředí pro vývoj nebo testování. Ale by měl obvykle nastavíte **true** pro nasazení do pracovním nebo produkčním prostředí. To uživatelům bránit v provádění změn do databáze během nasazení.
- **Zálohování databáze před nasazením**. To obvykle nastavuje na **true** při nasazení v produkčním prostředí, aby se předešlo ztrátě dat. Můžete také nastavit na **true** při nasazení pro pracovní prostředí, pokud vaše pracovní databáze obsahuje velké množství dat.
- **Generovat příkazy DROP pro objekty, které jsou v cílové databázi, ale nejsou v databázi projektu**. Ve většině případů to je nedílnou a podstatnou část provedením přírůstkové změny databáze. Pokud jste nastavili **vždy znovu vytvořit databázi** k **false**, toto nastavení nebude mít žádný vliv.
- **Nepoužívejte příkaz ALTER ASSEMBLY příkazy aktualizace typů CLR**. Toto nastavení určuje, jak aktualizovat běžné typy language runtime (CLR) na novější verze sestavení systému SQL Server. Musí být nastavena na **false** ve většině scénářů.

Tato tabulka ukazuje nastavení typické nasazení pro jiné cílové prostředí. Nastavení však může být liší v závislosti na požadavků na přesný.

|  | Vývojáře/testování | Pracovní nebo integrace | Produkční |
| --- | --- | --- | --- |
| **Kolace porovnání nasazení** | Zdroj | cíl | cíl |
| **Nasazení vlastnosti databáze** | True | Pouze první přihlášení | Pouze první přihlášení |
| **Vždy znovu vytvořit databázi** | True | False | False |
| **Blokovat přírůstkové nasazení, pokud může dojít ke ztrátě dat.** | False | Možná | True |
| **Spustit skript nasazení v režimu jednoho uživatele** | False | True | True |
| **Zálohování databáze před nasazením** | False | Možná | True |
| **Generovat příkazy DROP pro objekty, které jsou v cílové databázi, ale nejsou v databázi projektu** | False | True | True |
| **Nepoužívejte příkaz ALTER ASSEMBLY příkazy aktualizace typů CLR** | False | False | False |
  

> [!NOTE]
> Další informace o vlastnosti nasazení databáze a důležité informace o prostředí, najdete v části [přehled o nastavení projektu databáze](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [postupy: Konfigurace vlastností Podrobnosti nasazení](https://msdn.microsoft.com/library/dd172125.aspx), [ Sestavení a nasazení databáze do prostředí izolované vývoj](https://msdn.microsoft.com/library/dd193409.aspx), a [sestavení a nasazení databází k pracovním nebo produkčním prostředí](https://msdn.microsoft.com/library/dd193413.aspx).


Pro podporu nasazení databázového projektu do více cílů, měli vytvořit konfigurační soubor nasazení pro každé cílové prostředí.

**Vytvořte soubor konfigurace specifické pro prostředí**

1. V sadě Visual Studio 2010 v **Průzkumníku řešení** oken, klikněte pravým tlačítkem na projekt databáze a pak klikněte na tlačítko **vlastnosti**.
2. Na stránce vlastností projektu databáze na **nasadit** ve **konfigurační soubor nasazení** řádek, klikněte na tlačítko **nový**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. V **nový soubor konfigurace nasazení** dialogové okno pole, zadejte smysluplný název souboru (například **TestEnvironment.sqldeployment**) a potom klikněte na **Uložit**.
4. Na *[Filename] *** .sqldeployment** stránky, nastavte vlastnosti nasazení pro splnění požadavků na cílové prostředí a pak soubor uložte.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Všimněte si, že nový soubor přidán do složky vlastnosti ve vašem projektu databáze.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Určení konfigurační soubor nasazení nástroje v VSDBCMD

Při použití konfigurace řešení (jako je ladění a vydání) v rámci sady Visual Studio 2010, můžete přidružit každé konfiguraci konfigurační soubor nasazení. Při sestavování konkrétní konfigurací procesu sestavení generuje soubor manifestu specifické konfigurace nasazení, který odkazuje na konfigurační soubor nasazení specifické konfigurace. Jedním z hlavních cílů tohoto přístupu k nasazení, které jsou popsané v těchto kurzech je však přidělíte uživatelům možnost řídit proces nasazení bez použití sady Visual Studio 2010 a konfigurace řešení. Konfigurace řešení v tento přístup je stejný bez ohledu na cílové nasazení prostředí. Podle nasazení databáze do určité cílové prostředí, můžete použít možnosti příkazového řádku VSDBCMD k určení konfiguračního souboru nasazení.

Pokud chcete zadat konfigurační soubor nasazení ve vaší VSDBCMD, použijte **p:/DeploymentConfigurationFile** přepínače a zadat úplnou cestu k souboru. Tím se přepíše konfiguračního souboru nasazení, který identifikuje manifest nasazení. Například můžete použít tento příkaz VSDBCMD k nasazení **ContactManager** databáze v testovacím prostředí:


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> Všimněte si, že procesu sestavení může přejmenování souboru .sqldeployment při kopírování souboru do výstupního adresáře.


Pokud používáte proměnných příkazu SQL v SQL skriptů před nasazením nebo po nasazení, můžete podobný postup přiřazení souboru .sqlcmdvars specifické pro prostředí s nasazením. V takovém případě použijete **p:/SqlCommandVariablesFile** přepínače k identifikaci vašeho .sqlcmdvars souboru.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Spuštěním příkazu VSDBCMD ze souboru projektu nástroje MSBuild

Můžete vyvolat příkaz VSDBCMD ze souboru projektu nástroje MSBuild pomocí **Exec** úloh v rámci cíle MSBuild. Ve své nejjednodušší podobě by vypadat přibližně takto:


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- V praxi aby soubory projektu snadno načíst a znovu použít, budete chtít vytvořit vlastností pro ukládání různé parametry příkazového řádku. To usnadňuje uživatelům poskytnout hodnoty vlastností v souboru projektu konkrétní prostředí nebo přepsat výchozí hodnoty z příkazového řádku nástroje MSBuild. Pokud použijete popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), pokyny pro sestavení a vlastnosti mezi těmito dvěma soubory by měl rozdělení odpovídajícím způsobem:
- Nastavení pro konkrétní prostředí, jako je název souboru konfigurace nasazení, připojovací řetězec databáze a cílová databáze má zobrazit v souboru projektu konkrétní prostředí.
- MSBuild cíl, který spouští příkaz VSDBCMD společně s jakékoli universal vlastnosti jako umístění spustitelného souboru VSDBCMD, by měli přejít v souboru projektu univerzální.

Také se ujistěte, sestavení projektu databáze před vyvolání VSDBCMD tak, aby soubor .deploymanifest je vytvořená a připravená k použití. Zobrazí úplný příklad tohoto přístupu v tématu [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md), který vás provede soubory v projektu [obraťte se na správce ukázkové řešení](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Závěr

Toto téma popisuje, jak můžete přizpůsobit vlastnosti databáze do jiné cílové prostředí při nasazení pomocí nástroje MSBuild a VSDBCMD databázové projekty. Tento přístup je užitečné, když potřebujete nasadit databázové projekty jako součást řešení větší, podnikovém měřítku. Tato řešení se často nasadí do více míst, jako je v izolovaném prostoru prostředí pro vývoj nebo testování, pracovní nebo Integrace platformy a produkční nebo prostředí za provozu. Každý z těchto cílové prostředí obvykle vyžaduje jedinečnou sadu vlastností nasazení databáze.

## <a name="further-reading"></a>Další čtení

Další informace o nasazení databázové projekty pomocí VSDBCMD.exe najdete v tématu [nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md). Další informace o používání vlastních souborů projektu nástroje MSBuild k řízení procesu nasazení najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Tyto články na webu MSDN poskytovat další obecné informace o nasazení databáze:

- [Přehled nastavení projektu databáze](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Postupy: Konfigurace vlastností Podrobnosti nasazení](https://msdn.microsoft.com/library/dd172125.aspx)
- [Sestavení a nasazení databází do prostředí izolované vývoj](https://msdn.microsoft.com/library/dd193409.aspx)
- [Sestavení a nasazení databází k pracovním nebo produkčním prostředí](https://msdn.microsoft.com/library/dd193413.aspx)

>[!div class="step-by-step"]
[Předchozí](performing-a-what-if-deployment.md)
[další](deploying-database-role-memberships-to-test-environments.md)
