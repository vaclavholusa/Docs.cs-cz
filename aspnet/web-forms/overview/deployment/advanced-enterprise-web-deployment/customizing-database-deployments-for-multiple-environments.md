---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Přizpůsobení nasazené databáze pro více prostředí | Dokumentace Microsoftu
author: jrjlee
description: 'Toto téma popisuje, jak přizpůsobit vlastnosti databáze pro konkrétní cíl prostředí jako součást procesu nasazení. Poznámka: Tématu předpokládá th...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 52fb2539ef388d129f88aa8aa87088e2d4a41ccf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753761"
---
<a name="customizing-database-deployments-for-multiple-environments"></a>Přizpůsobení nasazené databáze pro více prostředí
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak přizpůsobit vlastnosti databáze pro konkrétní cíl prostředí jako součást procesu nasazení.
> 
> > [!NOTE]
> > V tématu se předpokládá, že nasazujete pomocí MSBuild.exe a VSDBCMD.exe databázový projekt sady Visual Studio 2010. Další informace o proč může zvolíte tuto metodu, najdete v části [nasazení webu v podniku](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) a [nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Když nasadíte databázový projekt do více cílů, budete často chcete přizpůsobit vlastnosti nasazení databáze pro každé cílové prostředí. Například v testovacích prostředích je by obvykle znovu vytvořit databázi při každém nasazení, že v přípravném nebo produkčním prostředí by bylo mnohem větší pravděpodobnost Ujistěte se, přírůstkové aktualizace zachovat data.
> 
> V databázi projektu sady Visual Studio 2010 nastavení nasazení jsou obsaženy v souboru konfigurace (.sqldeployment) nasazení. Jak vytvořit soubory konfigurace nasazení specifických pro prostředí a určit, který chcete použít jako parametr VSDBCMD se zobrazí v tomto tématu.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

Toto téma předpokládá, že:

- Použít rozdělení projektu souboru přístup k nasazení řešení, jak je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Volání VSDBCMD ze souboru projektu k nasazení projektu databáze, jak je popsáno v [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Chcete-li vytvořit nasazení systému, který podporuje různé vlastnosti nasazení databáze mezi cílových prostředí, bude nutné:

- Vytvořte soubor konfigurace (.sqldeployment) nasazení pro každé cílové prostředí.
- Vytvoření VSDBCMD příkazu, který určuje konfigurační soubor nasazení jako přepínač příkazového řádku.
- Parametrizujte VSDBCMD příkaz v souboru projektu Microsoft Build Engine (MSBuild) tak, aby možnosti VSDBCMD jsou vhodné pro cílové prostředí.

Postup pro každý z těchto postupů se zobrazí v tomto tématu.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Vytváří se soubory konfigurace nasazení specifických pro prostředí

Ve výchozím nastavení, databázový projekt obsahuje jedno nasazení konfigurační soubor s názvem *Database.sqldeployment*. Pokud tento soubor otevřít v sadě Visual Studio 2010, zobrazí se různé možnosti nasazení podle, které jsou k dispozici:

- **Nasazení porovnání řazení**. Můžete zvolit, jestli se má používat kolaci databáze vašeho projektu ( *zdroj* kolace) nebo kolaci databáze cílového serveru ( *cílové* řazení). Ve většině případů budete chtít používat kolaci zdroje při nasazení na vývojovém nebo testovacím prostředí. Při nasazování do testovací nebo produkční prostředí budete obvykle chcete ponechat beze změny, aby všechny problémy s interoperabilitou cílové kolaci.
- **Nasazení databáze vlastnosti**. Můžete zvolit, jestli se má použít vlastnosti databáze, jak jsou definovány v *Database.sqlsettings* souboru. Při nasazení databáze poprvé, měli byste nasadit vlastnosti databáze. Pokud aktualizujete existující databázi, vlastnosti by už měla být na místě a obvykle není nutné je znovu nasadit.
- **Vždy znovu vytvořit databázi**. Díky tomu se rozhodujete, jestli chcete znovu vytvořit cílovou databázi vždy, když nasazujete nebo provádět přírůstkové změny do databáze cílové aktuální s schéma. Když znovu vytvoříte databázi, ztratíte všechna data v existující databázi. V důsledku toho by měl obvykle nastavíte **false** pro nasazení pro pracovní nebo produkční prostředí.
- **Blokovat přírůstkové nasazení, pokud může dojít ke ztrátě dat**. To vám umožňuje zvolit, jestli nasazení by se měla zastavit, pokud změna schématu databáze způsobí ztrátu dat. Obvykle zvolíte **true** pro nasazení do produkčního prostředí, získáte možnost zasahovat a chránit všechna důležitá data. Pokud jste nastavili **vždy znovu vytvořit databázi** k **false**, toto nastavení nebude mít žádný efekt.
- **Spouštění nasazení v jednouživatelském režimu**. To většinou není problém v vývojová nebo testovací prostředí. Nicméně byste obvykle nastavíte **true** pro nasazení pro pracovní nebo produkční prostředí. To zabrání uživatelům provádět změny do databáze, zatímco probíhá nasazení.
- **Proveďte zálohu databáze před nasazením**. To obvykle nastavuje na **true** při nasazení v produkčním prostředí, aby se předešlo ztrátě dat. Můžete také nastavit na **true** při nasazení do přípravného prostředí, pokud obsahuje velké množství dat pracovní databáze.
- **Generovat příkazy DROP pro objekty, které jsou v cílové databázi, ale nejsou v databázi projektu**. Ve většině případů to je integrální a základní součástí přírůstkové změny do databáze. Pokud jste nastavili **vždy znovu vytvořit databázi** k **false**, toto nastavení nebude mít žádný efekt.
- **Nepoužívejte příkaz ALTER ASSEMBLY příkazy aktualizovat typy CLR**. Toto nastavení určuje, jak aktualizovat běžné language runtime (CLR) typy na novější verze sestavení systému SQL Server. To by mělo být nastavené **false** ve většině scénářů.

Tato tabulka ukazuje typické nasazení nastavení pro jiné cílové prostředí. Nastavení může být však liší v závislosti na přesné požadavky.

|  | Pro vývojáře a testování | Pracovní/integrace | Produkční |
| --- | --- | --- | --- |
| **Nasazení porovnání řazení** | Zdroj | Cíl | Cíl |
| **Nasazení vlastnosti databáze** | Hodnota TRUE | Pouze první čas | Pouze první čas |
| **Vždy znovu vytvořit databázi** | Hodnota TRUE | False | False |
| **Blokovat přírůstkové nasazení, pokud může dojít ke ztrátě dat.** | False | Možná | Hodnota TRUE |
| **Spusťte skript nasazení v režimu jednoho uživatele** | False | Hodnota TRUE | Hodnota TRUE |
| **Proveďte zálohu databáze před nasazením** | False | Možná | Hodnota TRUE |
| **Generovat příkazy DROP pro objekty, které jsou v cílové databázi, ale nejsou v projektu databáze** | False | Hodnota TRUE | Hodnota TRUE |
| **Nepoužívejte příkaz ALTER ASSEMBLY příkazy aktualizovat typy CLR** | False | False | False |
  

> [!NOTE]
> Další informace o vlastnosti nasazení databáze a důležité informace o prostředí, najdete v části [přehled o nastavení databázového projektu](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [postupy: Konfigurace vlastností pro podrobnosti o nasazení](https://msdn.microsoft.com/library/dd172125.aspx), [ Sestavení a nasazení databáze do izolované vývojové prostředí](https://msdn.microsoft.com/library/dd193409.aspx), a [sestavování a nasazování databází do přípravného nebo produkčního prostředí](https://msdn.microsoft.com/library/dd193413.aspx).


Pro podporu nasazení databázový projekt do více cílů, měli vytvořit konfigurační soubor nasazení pro každé cílové prostředí.

**Chcete-li vytvořit soubor konfigurace specifických pro prostředí**

1. V sadě Visual Studio 2010 v **Průzkumníka řešení** okna, klikněte pravým tlačítkem na váš projekt databáze a pak klikněte na tlačítko **vlastnosti**.
2. Na stránce vlastností projektu databáze na **nasadit** kartě **konfigurační soubor nasazení** řádku, klikněte na tlačítko **nový**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. V **nové konfigurační soubor nasazení** dialogové okno pole, zadejte smysluplný název souboru (například **TestEnvironment.sqldeployment**) a potom klikněte na tlačítko **Uložit**.
4. Na *[název_souboru] *** .sqldeployment** stránky, nastavit vlastnosti nasazení tak, aby odpovídaly požadavkům vaší cílové prostředí a pak soubor uložte.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Všimněte si, že se přidá nový soubor do vlastnosti složky v projektu databáze.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Určení v VSDBCMD konfigurační soubor nasazení

Při použití konfigurace řešení (jako je ladění a vydání) v rámci sady Visual Studio 2010, můžete přidružit každou konfiguraci konfigurační soubor nasazení. Při sestavování konkrétní konfiguraci procesu sestavení generuje soubor manifestu nasazení určených pro konfigurace odkazující na konfigurační soubor nasazení určených pro konfigurace. Jedním z hlavních cílů přístupu k nasazení je popsáno v těchto kurzech ale chcete lidem umožnit řízení procesu nasazení bez použití Visual Studio 2010 a konfigurace řešení. Konfigurace řešení v rámci tohoto přístupu je stejný bez ohledu na cílové nasazení prostředí. Přizpůsobení nasazení databáze tak, aby konkrétní cílové prostředí, můžete použít možnosti příkazového řádku VSDBCMD zadat konfigurační soubor nasazení.

Chcete-li zadat konfigurační soubor nasazení do vašeho VSDBCMD, použijte **p:/DeploymentConfigurationFile** přepnutí a zadejte úplnou cestu k souboru. Tím se přepíše konfigurační soubor nasazení, který identifikuje manifest nasazení. Například můžete použít tento příkaz VSDBCMD k nasazení **ContactManager** databáze do testovacího prostředí:


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> Všimněte si, že proces sestavení může přejmenujte svůj soubor .sqldeployment, když ho zkopíruje soubor do výstupního adresáře.


Pokud používáte proměnných příkazu SQL v SQL skripty před nasazením nebo po nasazení, můžete použít podobný přístup k přidružení souboru .sqlcmdvars specifických pro prostředí s nasazením. V tomto případě použijete **p:/SqlCommandVariablesFile** přepínač k identifikaci souboru .sqlcmdvars.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Spuštění příkazu VSDBCMD ze souboru projektu nástroje MSBuild

Můžete vyvolat příkaz VSDBCMD ze souboru projektu MSBuild pomocí **Exec** úkol v rámci cíl nástroje MSBuild. Ve své nejjednodušší podobě by vypadalo takto:


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- V praxi aby soubory projektu se snadno čte a opakovaně používat, je budete chtít vytvořit vlastnosti, které chcete ukládat různé parametry příkazového řádku. To usnadňuje uživatelům poskytnout hodnoty vlastností v souboru projektu pro konkrétní prostředí nebo přepsat výchozí hodnoty z příkazového řádku MSBuild. Pokud použijete přístup soubor projektu rozdělit podle [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), pokyny pro sestavení a vlastnosti mezi dvěma soubory by měly rozdělit odpovídajícím způsobem:
- Nastavení pro konkrétní prostředí, jako je název souboru konfigurace nasazení, připojovací řetězec databáze a název cílové databáze by měl přejít v souboru projektu pro konkrétní prostředí.
- Cíl nástroje MSBuild, který spouští příkaz VSDBCMD spolu s všechny univerzální vlastnosti jako umístění spustitelného souboru VSDBCMD, by měl přejít v souboru projektu univerzální.

Také se ujistěte, sestavit projekt databáze před vyvolání VSDBCMD tak, aby soubor .deploymanifest je vytvořená a připravená k použití. Zobrazí se úplný příklad tohoto přístupu v tomto tématu [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md), které vás provede soubory projektu ve [ukázkové řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Závěr

Toto téma popisuje, jak můžete přizpůsobit vlastnosti databáze do jiné cílové prostředí při nasazení pomocí nástroje MSBuild a VSDBCMD databázové projekty. Tento přístup je užitečný, když budete chtít nasadit jako součást větší, řešení podnikových databázových projektů. Tato řešení jsou často nasazené do více cílů, jako je vývojové nebo testovací prostředí v izolovaném prostoru, přípravném nebo Integrace platformy a produkční nebo produkčním prostředím. Každá z těchto cílových prostředí obvykle vyžaduje jedinečnou sadu vlastností nasazení databáze.

## <a name="further-reading"></a>Další čtení

Další informace o nasazení pomocí VSDBCMD.exe databázových projektů, naleznete v tématu [nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md). Další informace o používání vlastní soubory projektu MSBuild k řízení procesu nasazení najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Tyto články na webu MSDN poskytují další obecné pokyny v nasazení databáze:

- [Přehled nastavení databázového projektu](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Postupy: Konfigurace vlastností pro podrobnosti o nasazení](https://msdn.microsoft.com/library/dd172125.aspx)
- [Sestavování a nasazování databází do izolované vývojového prostředí](https://msdn.microsoft.com/library/dd193409.aspx)
- [Sestavování a nasazování databází do přípravného nebo produkčního prostředí](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Předchozí](performing-a-what-if-deployment.md)
> [další](deploying-database-role-memberships-to-test-environments.md)
