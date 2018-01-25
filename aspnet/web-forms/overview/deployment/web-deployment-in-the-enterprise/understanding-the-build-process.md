---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: "Principy procesu sestavení | Microsoft Docs"
author: jrjlee
description: "Toto téma obsahuje návod sestavení a nasazení procesu podnikovém měřítku. Postup popsaný v tomto tématu používá vlastní Microsoft sestavení Engin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 3efcefc40dc135ff42f55911036f8b38b5aa13b1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="understanding-the-build-process"></a>Principy procesu sestavení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma obsahuje návod sestavení a nasazení procesu podnikovém měřítku. Postup popsaný v tomto tématu používá vlastní soubory projektu Microsoft Build Engine (MSBuild) k poskytnutí jemně odstupňovanou kontrolu nad všechny aspekty procesu. V souborech projektu vlastní cíle MSBuild slouží ke spuštění nástroje pro nasazení jako nástroj nasazení webu (MSDeploy.exe) Internetové informační služby (IIS) a nástroj pro nasazení databáze VSDBCMD.exe.
> 
> > [!NOTE]
> > Předchozí téma [vysvětlení souboru projektu](understanding-the-project-file.md), popsané klíčové komponenty soubor projektu nástroje MSBuild a zavádí koncept rozdělení souborů projektu pro podporu nasazení na několika cílové prostředí. Pokud už nejste obeznámeni s následujícími základními pojmy, měli byste zkontrolovat [vysvětlení souboru projektu](understanding-the-project-file.md) než začnete pracovat prostřednictvím tohoto tématu.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz series používá ukázkové řešení & #x 2014; [řešení obraťte se na správce](the-contact-manager-solution.md)& #x 2014; představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3 systému Windows Komunikační služby Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](understanding-the-project-file.md), ve které je řízené procesu sestavení dva projektu soubory & #x 2014; jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="build-and-deployment-overview"></a>Sestavení a nasazení – přehled

V [řešení obraťte se na správce](the-contact-manager-solution.md), tři soubory řízení procesu sestavení a nasazení:

- A *soubor projektu univerzální* (*Publish.proj*). Tato položka obsahuje sestavení a nasazení pokyny, které se nemění mezi cílové prostředí.
- *Soubor projektu konkrétní prostředí* (*Env Dev.proj*). Tato položka obsahuje sestavení a nasazení nastavení, které jsou specifické pro konkrétní cílové prostředí. Například můžete použít *Env Dev.proj* souboru k zadání nastavení pro vývojáře nebo testovací prostředí a vytvořit alternativní soubor s názvem *Env Stage.proj* zadání nastavení pro pracovní prostředí.
- A *soubor příkazů* (*publikovat Dev.cmd*). Tato položka obsahuje MSBuild.exe příkaz, který určuje, které projektu souborů, které jste chcete provést. Můžete vytvořit soubor příkazů pro každé cílové prostředí, kde každý soubor obsahuje příkaz MSBuild.exe, který určuje soubor jiný projekt konkrétní prostředí. To umožňuje vývojáři nasadit do různých prostředí jednoduše spuštěním souboru příslušný příkaz.

Tyto tři soubory ve složce publikování řešení můžete najít v ukázkové řešení.

![](understanding-the-build-process/_static/image1.png)

Předtím, než se podíváte na tyto soubory podrobněji, Podívejme se na fungování celkové procesu sestavení při použití tohoto přístupu. Na vysoké úrovni procesem sestavení a nasazení vypadat třeba takto:

![](understanding-the-build-process/_static/image2.png)

První věc, kterou se stane, je, že jsou obě projektu soubory & #x 2014; jeden obsahující universal sestavení a pokyny k nasazení a jeden obsahující nastavení pro konkrétní prostředí & #x 2014; jsou sloučeny do jediného souboru projektu. MSBuild funguje pak pomocí pokynů v souboru projektu. Sestaví všechny projekty v řešení pomocí souboru projektu pro každý projekt. Potom zavolá jiných nástrojů, jako je nasazení webu (MSDeploy.exe) a nástroj VSDBCMD k nasazení webového obsahu a databází na cílovém prostředí.

Proces sestavení a nasazení od začátku do konce, provádí tyto úlohy:

1. Odstraní obsah výstupního adresáře v rámci přípravy čerstvé sestavení.
2. Sestavuje jednotlivých projektů v řešení:

    1. Pro webové projekty & #x 2014; v takovém případě webová aplikace ASP.NET MVC a službou WCF webové služby & #x 2014; procesu sestavení vytvoří balíčku pro nasazení webu pro každý projekt.
    2. Proces sestavení pro databázové projekty, vytvoří nasazení manifestu (soubor .deploymanifest) pro každý projekt.
3. Nástroj VSDBCMD.exe používá k nasazení jednotlivé databáze projekty v řešení, pomocí různé vlastnosti z soubory projektu & #x 2014; cílový připojovací řetězec a název databáze & #x 2014; společně s .deploymanifest soubor.
4. Nástroj MSDeploy.exe používá pro nasazení jednotlivých webového projektu v řešení pomocí různých vlastností ze souborů projektu k řízení procesu nasazení.

Ukázkové řešení můžete použít ke sledování tohoto procesu podrobněji.

> [!NOTE]
> Pokyny k přizpůsobení souborů projektu specifický pro vaše vlastní prostředí serveru najdete v tématu [nakonfigurovat vlastnosti nasazení pro cílové prostředí](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="invoking-the-build-and-deployment-process"></a>Vyvolání sestavení a proces nasazení

K nasazení řešení. Obraťte se na správce v testovacím prostředí developer, spustí Vývojář *publikovat Dev.cmd* příkazového řádku. Tím se spustí MSBuild.exe, zadání *Publish.proj* jako soubor projektu provést a *Env Dev.proj* jako hodnotu parametru.


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> **/Fl** přepínače (zkratka pro **/fileLogger**) protokoluje jeho výstup do souboru s názvem *msbuild.log* v aktuálním adresáři. Další informace najdete v tématu [příkazového řádku MSBuild – Reference](https://msdn.microsoft.com/library/ms164311.aspx).


V tomto okamžiku MSBuild spuštění, načte *Publish.proj* soubor a spustí zpracování podle pokynů v něm. První pokyn informuje MSBuild Importovat projekt souboru, který **TargetEnvPropsFile** parametrem.


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


**TargetEnvPropsFile** parametr určuje *Env Dev.proj* souboru, takže MSBuild sloučí obsah *Env Dev.proj* soubor do  *Publish.proj* souboru.

Další prvky, které MSBuild zaznamená v souboru projektu sloučené jsou vlastnosti skupiny. Vlastnosti se zpracovávají v pořadí, ve kterém se zobrazí v souboru. MSBuild vytvoří pár klíč hodnota pro každou vlastnost zajištění, že jsou splněny všechny zadané podmínky. Vlastnosti později definované v souboru přepíše jakékoli vlastnosti se stejným názvem dříve definované v souboru. Představte si třeba **OutputRoot** vlastnosti.


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


Když MSBuild zpracovává první **OutputRoot** elementu poskytování s podobným názvem parametr nebyl zadán, nastaví hodnotu **OutputRoot** vlastnost **... \Publish\Out**. Když zjistí druhý **OutputRoot** elementu, pokud je podmínka vyhodnocena jako **true**, přepíše hodnotu **OutputRoot** vlastnosti s hodnotou **OutDir** parametr.

Na další prvek, který zjistí MSBuild je skupina jednu položku, obsahující položka s názvem **ProjectsToBuild**.


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild zpracuje tento pokyn podle budovy seznamu položek s názvem **ProjectsToBuild**. V takovém případě seznamu položek obsahuje jednu hodnotu & #x 2014; cestu a název souboru, řešení.

V tomto okamžiku zbývající prvky jsou cíle. Cíle jsou zpracovávány jinak z vlastností a položek & #x 2014; v podstatě nejsou zpracovány cíle, pokud jsou buď explicitně specifikovaných uživatelem nebo vyvolané jiné konstrukce v souboru projektu. Odvolat, otevření **projektu** zahrnuje značky **defaulttargets –** atribut.


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


Tím se nastaví MSBuild má být vyvolán **FullPublish** cíl, pokud cíle nejsou zadané, když je volána MSBuild.exe. **FullPublish** cíl neobsahuje žádné úlohy; místo toho jednoduše určuje seznam závislosti.


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


Tuto závislost informuje MSBuild tomto v pořadí provést **FullPublish** cíl, je nutné vyvolat tento seznam cílů, v uvedeném pořadí:

1. Je nutné vyvolat **Vyčistit** cíl.
2. Je nutné vyvolat **BuildProjects** cíl.
3. Je nutné vyvolat **GatherPackagesForPublishing** cíl.
4. Je nutné vyvolat **PublishDbPackages** cíl.
5. Je nutné vyvolat **PublishWebPackages** cíl.

### <a name="the-clean-target"></a>Vyčištění cíl

**Vyčistit** cíl v podstatě odstraní výstupního adresáře a veškerý jeho obsah jako příprava pro novou sestavení.


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


Všimněte si, že cíl zahrnuje **ItemGroup** element. Když definujete vlastnosti nebo položky v rámci **cíl** elementu, kterou vytváříte, *dynamické* vlastností a položek. Jinými slovy vlastnosti nebo položky nejsou zpracovány cíl je proveden. Výstupní adresář nemusí existovat nebo obsahovat žádné soubory, až do zahájení procesu sestavení, takže nemůže vytvořit  **\_soubory FilesToDelete** seznamu jako statické položka; budete muset Počkejte, dokud provádění. Takto vytvoříte seznamu jako dynamické položky v rámci cíle.

> [!NOTE]
> V takovém případě protože **Vyčistit** cíl je první spouštění, není nutné skutečné použít skupinu dynamické položky. Je však vhodné pro použití dynamických vlastností a položek v takové situaci, jak můžete chtít provést cíle v jiném pořadí v určitém okamžiku.  
> Měly také směřovat předejdete deklarace položky, které se nikdy používat. Pokud máte položky, které budou používat jenom konkrétní cíl, zvažte umístění je uvnitř cíl, který má odeberte všechny nepotřebné režijní náklady na proces sestavení.


Dynamické položky z produkce, **Vyčistit** cíl je poměrně jednoduché a využívá integrované **zpráva**, **odstranit**, a **removedir –**úkolů:

1. Odešle zprávu do protokolovacího nástroje.
2. Vytvoříte seznam souborů k odstranění.
3. Odstraňte soubory.
4. Odeberte výstupnímu adresáři.

### <a name="the-buildprojects-target"></a>Cíl BuildProjects

**BuildProjects** cíl v podstatě vytvoří všechny projekty v řešení ukázka.


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


Tento cíl se některé podrobně popsány v předchozí tématu [vysvětlení souboru projektu](understanding-the-project-file.md), k objasnění, jak úlohy a cíle odkazovat vlastností a položek. V tuto chvíli vás zajímají hlavně **MSBuild** úloh. Tento úkol můžete použít k vytvoření více projektů. Úloha nevytvoří novou instanci třídy MSBuild.exe; používá aktuální spuštěné instance pro každý projekt sestavit. Klíčové body zájmu v tomto příkladu jsou vlastnosti nasazení:

- **DeployOnBuild** přikazuje MSBuild spouštět žádné pokyny k nasazení v nastavení projektu po dokončení sestavení každý projekt.
- **DeployTarget** vlastnost identifikuje cíl, který chcete vyvolat po sestavení projektu. V takovém případě **balíček** cíl sestavení projektu výstup do balíčku nasadit webové.

> [!NOTE]
> **Balíček** cíl vyvolá webové publikování kanálu (WPP), která poskytuje integraci mezi MSBuild a nasazení webu. Pokud chcete podívejte se na předdefinované cíle, které poskytuje jako, zkontrolujte *Microsoft.Web.Publishing.targets* soubor ve složce % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


### <a name="the-gatherpackagesforpublishing-target"></a>Cíl GatherPackagesForPublishing

Pokud, abyste si prostudovali **GatherPackagesForPublishing** cíl, můžete si všimnout, že neobsahuje ve skutečnosti žádné úlohy. Místo toho obsahuje skupinu jednu položku, která definuje tři dynamické položky.


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


Tyto položky odkazovat na balíčky nasazení, které byly vytvořeny při zpracování **BuildProjects** target byla spuštěna. Nelze definujete tyto položky staticky v souboru projektu, protože soubory, na které odkazují položky neexistují až **BuildProjects** cíl se spustí. Místo toho položky musí být definován dynamicky v rámci cíl, který není vyvolána, dokud nebude po **BuildProjects** cíl se spustí.

Nepoužívají se položky v rámci této cílové & #x 2014; tento cíl jednoduše vytvoří položky a budou metadata spojená s každou hodnotu položky. Po zpracování tyto prvky jsou **PublishPackages** položka bude obsahovat dvě hodnoty, cesta k *ContactManager.Mvc.deploy.cmd* souboru a cesta k  *ContactManager.Service.deploy.cmd* souboru. Nasazení webu vytvoří tyto soubory jako součást webového balíčku pro každý projekt, a jsou to soubory, které je nutné vyvolat na cílovém serveru, aby bylo možné nasadit balíčky. Pokud otevřete si některý z těchto souborů uvidíte v podstatě příkaz MSDeploy.exe s různými hodnotami parametrů specifické pro sestavení.

**DbPublishPackages** položka bude obsahovat jednu hodnotu, cesta k *ContactManager.Database.deploymanifest* souboru.

> [!NOTE]
> Soubor .deploymanifest se vygeneruje, když vytváříte projekt databáze a jako soubor projektu nástroje MSBuild používá stejné schéma. Obsahuje všechny informace potřebné k nasazení databáze, včetně umístění schéma databáze (.dbschema) a podrobnosti o všech skriptů, před nasazením a po nasazení. Další informace najdete v tématu [přehled z databáze sestavení a nasazení](https://msdn.microsoft.com/library/aa833165.aspx).


Dozvíte informace o tom, jak balíčky pro nasazení a manifesty nasazení databáze vytvořit a použít v [budova a projekty webových aplikací balení](building-and-packaging-web-application-projects.md) a [nasazení databázové projekty](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>Cíl PublishDbPackages

Stručně platí, že **PublishDbPackages** cíl vyvolá nástroj VSDBCMD k nasazení **ContactManager** databáze na cílovém prostředí. Konfigurace nasazení databáze zahrnuje mnoho rozhodnutí a drobné odlišnosti a budete Další informace o to [nasazení databázové projekty](deploying-database-projects.md) a [přizpůsobení nasazení databáze pro prostředí s více](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). V tomto tématu budete zaměříme na tom, jak skutečně funguje tento cíl.

První, Všimněte si, že počáteční značka obsahuje **výstupy** atribut.


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


Toto je příklad *dávkování cíle*. V souborech projektu nástroje MSBuild dávkování je technika pro iterování přes kolekce. Hodnota **výstupy** atribut, **"% (DbPublishPackages.Identity)"**, odkazuje **Identity** metadata vlastnosti **DbPublishPackages**  seznamu položek. Tento zápis **Outputs=%***(ItemList.ItemMetadataName)*, je přeložená jako:

- Rozdělit položky v **DbPublishPackages** do dávek položek, které obsahují stejné **Identity** hodnota metadat.
- Spusťte cíl jednou na jednu dávku.

> [!NOTE]
> **Identity** je jedním z [hodnoty předdefinovaných metadat](https://msdn.microsoft.com/library/ms164313.aspx) přiřazené každá položka na vytvoření. Odkazuje na hodnotu **zahrnout** atribut **položky** element & #x 2014; jinými slovy, cestu a název položky.


V takovém případě nikdy by měl být více než jednu položku se stejným cesta a název souboru, v podstatě Pracujeme s velikostí batch jedné. Cíl se spustí jednou pro každý balíček databáze.

Zobrazí se podobné zápis ve  **\_Cmd** vlastnosti, která vytvoří příkaz VSDBCMD s příslušné přepínače.


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


V takovém případě **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, a **%(DbPublishPackages.FullPath)** všechny odkazovat na metadata hodnoty **DbPublishPackages** kolekce položek. **\_Cmd** vlastnost je používána **Exec** úkol, který volá příkaz.


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


V důsledku tohoto zápisu **Exec** úloh vytvoří dávek podle jedinečných kombinací **DatabaseConnectionString**, **TargetDatabase**a **FullPath** hodnoty metadat a tato úloha spustí jednou pro každou dávku. Toto je příklad *dávkování úloh*. Nicméně, protože cílové úrovni dávkování má již rozdělen naše kolekce položek do jedné položky dávek, **Exec** úloha spustí jednou a jen jednou u každé iteraci cíle. Jinými slovy tato úloha vyvolá nástroj VSDBCMD jednou pro každý balíček databáze v řešení.

> [!NOTE]
> Další informace o cíle a dávkování úloh najdete v tématu MSBuild [Batching](https://msdn.microsoft.com/library/ms171473.aspx), [Metadata položek v dávkování cíle](https://msdn.microsoft.com/library/ms228229.aspx), a [Metadata položek v dávkování úloh](https://msdn.microsoft.com/library/ms171474.aspx).


### <a name="the-publishwebpackages-target"></a>Cíl PublishWebPackages

Pomocí tohoto bodu, jste vyvolat **BuildProjects** cíl, který generuje balíčku pro nasazení webu pro každý projekt v ukázkové řešení. Doplňujícími každý balíček je *deploy.cmd* souboru, který obsahuje MSDeploy.exe příkazy potřebné k nasazení balíčku na cílovém prostředí, a *SetParameters.xml* soubor, který určuje nezbytné podrobnosti o cílovém prostředí. Také jste vyvolat **GatherPackagesForPublishing** cíl, který generuje kolekce položek obsahující *deploy.cmd* soubory, které vás zajímají. V podstatě **PublishWebPackages** target provádí tyto funkce:

- Zpracovává *SetParameters.xml* souboru pro každý balíček zahrnout správné informace pro cílové prostředí pomocí **xmlpoke –** úloh.
- Vyvolá *deploy.cmd* souboru pro každý balíček, příslušné přepínače.

Stejně jako **PublishDbPackages** cíl, **PublishWebPackages** cíl používá k zajištění, že cíl se spustí jednou pro každý balíček webové dávkování cíle.


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


V rámci cíle **Exec** úkolů se používá ke spuštění *deploy.cmd* souboru pro každý web balíček.


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Další informace o konfiguraci nasazení webových balíčků, najdete v části [budova a projekty webových aplikací balení](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Závěr

Toto téma poskytuje návod, jak se soubory projektu rozdělení slouží k řízení procesu sestavení a nasazení od začátku do konce pro ukázkové řešení obraťte se na správce. Tento přístup umožňuje spouštět komplexní, podnikovém měřítku nasazení v jedné opakovatelné kroku, jednoduše tak, že spustíte soubor příkaz konkrétní prostředí.

## <a name="further-reading"></a>Další čtení

Podrobnější Úvod do souborů projektu a jako, najdete v části [uvnitř Microsoft Build Engine: pomocí nástroje MSBuild a Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi a William Bartholomew, ISBN: 978-0-7356-4524-0.

>[!div class="step-by-step"]
[Předchozí](understanding-the-project-file.md)
[další](building-and-packaging-web-application-projects.md)
