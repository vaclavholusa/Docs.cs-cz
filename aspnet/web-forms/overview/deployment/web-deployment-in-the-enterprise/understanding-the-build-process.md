---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Vysvětlení procesu sestavení | Dokumentace Microsoftu
author: jrjlee
description: Toto téma obsahuje návod procesu sestavení a nasazení podnikové úrovni. Tento přístup je popsáno v tomto tématu používá Engin vytvářet vlastní Microsoft...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 9df145b281b086f546c55d0b26a8b0e44e896bb0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755485"
---
<a name="understanding-the-build-process"></a>Vysvětlení procesu sestavení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma obsahuje návod procesu sestavení a nasazení podnikové úrovni. Poskytují detailní kontrolu nad všechny aspekty procesu pomocí postupu popsaného v tomto tématu vlastní soubory projektu Microsoft Build Engine (MSBuild). V souborech projektu vlastní cíle nástroje MSBuild slouží ke spouštění nástroje nasazení jako nástroj nasazení webu (MSDeploy.exe) Internetové informační služby (IIS) a nástroj pro nasazení databáze VSDBCMD.exe.
> 
> > [!NOTE]
> > V předchozím tématu [vysvětlení souboru projektu](understanding-the-project-file.md), popsané klíčové komponenty souboru projektu MSBuild a představil nový koncept rozdělte soubory projektu pro podporu nasazení na víc cílových prostředí. Pokud již nejste obeznámeni s těchto konceptů, měli byste zkontrolovat [vysvětlení souboru projektu](understanding-the-project-file.md) předtím, než začnete Procházet v tomto tématu.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](understanding-the-project-file.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="build-and-deployment-overview"></a>Sestavení a nasazení – přehled

V [řešení Správce kontaktů](the-contact-manager-solution.md), tři soubory řízení procesu sestavení a nasazení:

- A *univerzální projekt soubor* (*Publish.proj*). Tato položka obsahuje sestavení a nasazení pokynů neměňte mezi cílové prostředí.
- *Soubor projektu specifických pro prostředí* (*Env Dev.proj*). Tato položka obsahuje sestavení a nasazení nastavení, které jsou specifické pro konkrétní cílové prostředí. Například můžete použít *Env Dev.proj* soubor k poskytování nastavení pro vývojářské nebo testovací prostředí a vytvořit alternativní soubor s názvem *Env Stage.proj* k poskytování nastavení Příprava prostředí.
- A *souboru příkazů* (*publikovat Dev.cmd*). Jedná se MSBuild.exe chcete spustit příkaz, který určuje, který projekt soubory, které jste. Můžete vytvořit soubor příkazů pro každé cílové prostředí, kde každý soubor obsahuje pomocí příkazu MSBuild.exe, který určuje soubor jiného projektu pro konkrétní prostředí. To umožní vývojářům jednoduše spuštěním souboru příslušný příkaz nasazení do různých prostředí.

V ukázkovém řešení najdete tyto tři soubory ve složce publikování řešení.

![](understanding-the-build-process/_static/image1.png)

Předtím, než se podíváte na tyto soubory podrobněji, Pojďme se podívat, jak funguje celkový proces sestavení při použití tohoto přístupu. Na vysoké úrovni proces sestavení a nasazení vypadá například takto:

![](understanding-the-build-process/_static/image2.png)

První věc, která se stane, je, že dva soubory projektu&#x2014;jeden obsahující univerzální pokyny k sestavení a nasazení a jeden obsahuje nastavení pro konkrétní prostředí&#x2014;jsou sloučeny do jediného souboru projektu. MSBuild pak pracuje pomocí pokynů v souboru projektu. Sestavuje všechny projekty v řešení, pomocí souboru projektu pro každý projekt. Poté volá jiné nástroje, jako je nasazení webu (MSDeploy.exe) a nástroj VSDBCMD nasazení webového obsahu a databází do cílového prostředí.

Od začátku do konce proces sestavení a nasazení provádí tyto úlohy:

1. Odstraní obsah výstupní adresář, v rámci přípravy pro nové sestavení.
2. Sestaví jednotlivých projektů v řešení:

    1. Pro webové projekty&#x2014;v takovém případě webová aplikace ASP.NET MVC a WCF webové služby&#x2014;proces sestavení vytvoří pro každý projekt balíčku pro nasazení webu.
    2. Proces sestavení pro databázové projekty, vytvoří manifest nasazení (soubor .deploymanifest) pro každý projekt.
3. Používá nástroj VSDBCMD.exe nasazení každý databázový projekt v řešení, pomocí různé vlastnosti ze souborů projektu&#x2014;cílový připojovací řetězec a název databáze&#x2014;spolu s .deploymanifest souboru.
4. Pomocí nástroje MSDeploy.exe nasazení každého webového projektu v řešení, pomocí různé vlastnosti ze souborů projektu k řízení procesu nasazení.

Ukázkové řešení můžete použít pro sledování tohoto procesu podrobněji.

> [!NOTE]
> Pokyny k přizpůsobení souborů projektu specifických pro prostředí pro prostředí serveru, naleznete v tématu [nakonfigurovat vlastnosti nasazení pro cílové prostředí](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="invoking-the-build-and-deployment-process"></a>Vyvolání sestavení a proces nasazení

K nasazení řešení Správce kontaktů do testovacího prostředí pro vývojáře, vývojář běží *publikovat Dev.cmd* souboru příkazů. Tím se vyvolá MSBuild.exe, určení *Publish.proj* jako souboru projektu pro spuštění a *Env Dev.proj* jako hodnotu parametru.


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> **/Fl** přepnout (zkratka pro **/fileLogger**) protokoluje výstup sestavení do souboru s názvem *msbuild.log* v aktuálním adresáři. Další informace najdete v tématu [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).


V tomto okamžiku MSBuild spustí, načte *Publish.proj* soubor a spustí zpracování pokyny v něm. První instrukce říká MSBuild pro import projektu souboru, který **TargetEnvPropsFile** parametr určuje.


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


**TargetEnvPropsFile** určuje parametr *Env Dev.proj* souboru, takže MSBuild sloučí obsah *Env Dev.proj* soubor do  *Publish.proj* souboru.

Další prvky, které MSBuild zjistí v souboru sloučeného projektu jsou vlastnosti skupiny. Vlastnosti se zpracovávají v pořadí, v jakém jsou uvedeny v souboru. Nástroj MSBuild vytvoří pár klíč hodnota pro každou vlastnost zajištění, že jsou splněny všechny zadané podmínky. Vlastnosti definované dále v souboru se přepíšou všechny vlastnosti se stejným názvem definovali dříve v souboru. Představme si třeba, **OutputRoot** vlastnosti.


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


Když MSBuild zpracovává první **OutputRoot** elementu, poskytuje s podobným názvem parametr nebyl zadán, nastaví hodnotu vlastnosti **OutputRoot** vlastnost **... \Publish\Out**. Pokud se setká druhý **OutputRoot** element, pokud je podmínka vyhodnocena jako **true**, přepíše hodnotu **OutputRoot** vlastnost s hodnotou **OutDir** parametru.

Další prvek, který zjistí MSBuild je skupina jedné položky, který obsahuje položku s názvem **ProjectsToBuild**.


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


Nástroj MSBuild zpracovává tento pokyn vytvořením seznamu položek s názvem **ProjectsToBuild**. V takovém případě seznamu položek obsahuje pouze jednu hodnotu&#x2014;cestu a název souboru řešení.

V tomto okamžiku zbývající prvky jsou cíle. Cíle jsou zpracována jinak z vlastností a položek&#x2014;v podstatě se zpracuje cílů, pokud jsou buď explicitně zadaná uživatelem nebo vyvolat jiný konstruktor v rámci souboru projektu. Vzpomeňte si, že otevírání **projektu** značka zahrnuje **defaulttargets –** atribut.


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


Toto dá pokyn MSBuild, který má být vyvolán **FullPublish** zadán cíl, není-li cíle při vyvolání MSBuild.exe. **FullPublish** cíl neobsahuje žádné úkoly; místo toho ji jednoduše určuje seznam závislostí.


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


Tato závislost říká MSBuild uvedeném v pořadí ke spuštění **FullPublish** cíl, potřebuje k vyvolání tento seznam cílů v uvedeném pořadí:

1. Musí vyvolat **Vyčistit** cíl.
2. Musí vyvolat **BuildProjects** cíl.
3. Musí vyvolat **GatherPackagesForPublishing** cíl.
4. Musí vyvolat **PublishDbPackages** cíl.
5. Musí vyvolat **PublishWebPackages** cíl.

### <a name="the-clean-target"></a>Cíl vyčistit

**Vyčistit** cíl v podstatě odstraní výstupnímu adresáři a veškerý jeho obsah jako příprava pro nové sestavení.


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


Všimněte si, že obsahuje cíl **ItemGroup** elementu. Při definování vlastností nebo položek v rámci **cílové** elementu, kterou vytváříte *dynamické* vlastností a položek. Jinými slovy vlastnosti nebo položky nebudou zpracovány dokud je cíl proveden. Výstupní adresář nemusí existovat nebo obsahovat všechny soubory až do zahájení procesu sestavení, takže nemůže vytvořit  **\_soubory FilesToDelete** jako statické položky seznamu, budete muset počkat, dokud probíhá spuštění. V důsledku toho sestavení seznamu jako dynamické položky v rámci cíle.

> [!NOTE]
> V tomto případě vzhledem k tomu, **Vyčistit** cíl je první provádět, není nutné skutečné na použití skupiny dynamické položky. Je však vhodné použít dynamické vlastnosti a položky v tomto typu scénáře, protože můžete chtít provést cíle v jiném pořadí v určitém okamžiku.  
> Také by měl cílem vyhnout se deklarace položky, které se nikdy nepoužívá. Pokud máte položky, které budou použity pouze konkrétní cíl, zvažte umístění uvnitř cíl, který chcete odebrat všechny zbytečnou režii v procesu sestavení.


Dynamické položky jste si poznamenali, **Vyčistit** cíl je celkem jasné a využívá integrovanou **zpráva**, **odstranit**, a **removedir –** úlohy:

1. Odešlete zprávu protokolovače.
2. Vytvoření seznamu souborů určených k odstranění.
3. Odstraňte soubory.
4. Odeberte výstupní adresář.

### <a name="the-buildprojects-target"></a>Cíl BuildProjects

**BuildProjects** cíl v podstatě sestavení všech projektů v ukázkovém řešení.


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


Tento cíl se podrobně popsány v některých v předchozím tématu [vysvětlení souboru projektu](understanding-the-project-file.md), pro ilustraci, jak úlohy a cíle odkazovat vlastností a položek. V tuto chvíli vás zajímají hlavně **MSBuild** úloh. Tento úkol můžete použít k sestavení více projektů. Úloha nevytvoří nové instance nástroje MSBuild.exe; používá aktuální spuštěné instance pro každý projekt sestavit. Klíčové body zájmu v tomto příkladu jsou vlastnosti nasazení:

- **DeployOnBuild** vlastnost dá pokyn, aby MSBuild spouštěl žádné pokyny k nasazení v nastavení projektu po dokončení sestavení každého projektu.
- **DeployTarget** vlastnost určuje cíl, který chcete vyvolat po sestavení projektu. V tomto případě **balíčku** cílové vytvoří výstup projektu do nasazení webového balíčku.

> [!NOTE]
> **Balíčku** cílové vyvolá webového publikování kanálu (WPP), který poskytuje integraci nástroje MSBuild a nasazení webu. Pokud chcete podívejte se na předdefinované cíle, které poskytuje WPP kontrolu *Microsoft.Web.Publishing.targets* souboru ve složce % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


### <a name="the-gatherpackagesforpublishing-target"></a>Cíl GatherPackagesForPublishing

Pokud jste studovat **GatherPackagesForPublishing** cíl, můžete si všimnout, že neobsahuje ve skutečnosti žádné úlohy. Místo toho obsahuje skupinu jednu položku, která definuje tři dynamické položky.


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


Tyto položky najdete balíčky pro nasazení, které byly vytvořeny při **BuildProjects** byl cíl spuštěn. Můžete nelze definovat tyto položky staticky v souboru projektu, protože soubory, na které odkazují na položky neexistují až **BuildProjects** je cíl proveden. Místo toho musíte položky definované dynamicky v rámci cíl, který není vyvolána, dokud nebude po **BuildProjects** je cíl proveden.

Položky nejsou použity uvnitř tohoto cíle&#x2014;tento cíl sestavení jednoduše položky a budou metadata spojená s každou hodnotu položky. Jakmile tyto prvky se zpracovávají, **PublishPackages** položka bude obsahovat dvě hodnoty a cesta k *ContactManager.Mvc.deploy.cmd* souboru a cestu k  *ContactManager.Service.deploy.cmd* souboru. Nástroj nasazení webu vytvoří tyto soubory součástí webového balíčku pro každý projekt a jsou to soubory, které je nutné vyvolat na cílovém serveru, aby bylo možné nasadit balíčky. Když otevřete jednu z těchto souborů uvidíte v podstatě příkaz MSDeploy.exe s různými hodnotami parametrů specifických pro sestavení.

**DbPublishPackages** položka bude obsahovat jedinou hodnotu, cesta k *ContactManager.Database.deploymanifest* souboru.

> [!NOTE]
> Soubor .deploymanifest se vygeneruje, když vytváříte projekt databáze a používá stejné schéma jako soubor projektu MSBuild. Obsahuje všechny informace potřebné k nasazení databáze, včetně umístění schéma databáze (.dbschema) a podrobnosti o skripty před a po nasazení. Další informace najdete v tématu [přehled z databáze sestavení a nasazení](https://msdn.microsoft.com/library/aa833165.aspx).


Získáte další informace o způsobu nasazení balíčků a manifesty nasazení databáze vytváří a používají v [sestavení a balení projektů webových aplikací](building-and-packaging-web-application-projects.md) a [nasazení databázové projekty](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>Cíl PublishDbPackages

Stručně vzato **PublishDbPackages** cílové vyvolá VSDBCMD nástroj pro nasazení **ContactManager** databáze do cílové prostředí. Konfigurace nasazení databáze vyžaduje spoustu rozhodnutích a drobné rozdíly, a získáte další informace najdete v [nasazení databázové projekty](deploying-database-projects.md) a [přizpůsobení nasazené databáze pro více prostředí](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). V tomto tématu se zaměříme na jak skutečně funguje tento cíl.

Napřed si všimněte, že obsahuje počáteční značka **výstupy** atribut.


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


Toto je příklad *dávkování cíle*. V souborech projektu MSBuild dávkování je technika pro iterace přes kolekce. Hodnota **výstupy** atribut, **"% (DbPublishPackages.Identity)"**, odkazuje **Identity** vlastností metadat **DbPublishPackages**  seznam položek. Tento typ notation **Outputs=%***(ItemList.ItemMetadataName)*, je přeložen jako:

- Rozdělit položky v **DbPublishPackages** do dávek položek, které obsahují stejné **Identity** hodnota metadat.
- Spusťte cíl jednou za služby batch.

> [!NOTE]
> **Identita** je jedním z [integrovaná metadata hodnoty](https://msdn.microsoft.com/library/ms164313.aspx) , která je přiřazená každé položce při vytvoření. Odkazuje na hodnotu **zahrnout** atribut **položky** element&#x2014;jinými slovy, cesta a název souboru položky.


V takovém případě vzhledem k tomu, že by neměla existovat více než jednu položku se stejným cestu a název souboru, v podstatě spolupracujeme s velikostí dávky jednoho. Cílem je provedena jednou pro každý balíček databáze.

Zobrazí se podobná zápis ve  **\_Cmd** vlastnost, která vytvoří příkaz VSDBCMD pomocí příslušných přepínačů.


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


V takovém případě **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, a **%(DbPublishPackages.FullPath)** všechny najdete metadata hodnot **DbPublishPackages** kolekci položek. **\_Cmd** vlastnost používá **Exec** úkol, který vyvolá příkaz.


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


V důsledku tohoto zápisu **Exec** úloh vytvoří na základě jedinečných kombinací dávky **DatabaseConnectionString**, **TargetDatabase**a **FullPath** hodnoty metadat a tato úloha spustí jednou pro každé dávky. Toto je příklad *dávkování úloh*. Nicméně, protože cílové úrovni dávkování má již rozdělen naše kolekce položek do dávek jedné položky **Exec** úkol se spustí jednou a jen jednou pro každou iteraci cíle. Jinými slovy tato úloha vyvolá nástroj VSDBCMD jednou pro každý balíček databáze v rámci řešení.

> [!NOTE]
> Další informace o cíl a dávkové zpracování úloh, naleznete v části nástroje MSBuild [dávkování](https://msdn.microsoft.com/library/ms171473.aspx), [Metadata položek v dávkování cíle](https://msdn.microsoft.com/library/ms228229.aspx), a [Metadata položek v dávkování úloh](https://msdn.microsoft.com/library/ms171474.aspx).


### <a name="the-publishwebpackages-target"></a>Cíl PublishWebPackages

Už v tomto okamžiku jste vyvolat **BuildProjects** cíl, který generuje balíčku pro nasazení webu pro každý projekt v ukázkovém řešení. Doprovodná každý balíček je *deploy.cmd* soubor, který obsahuje příkazy MSDeploy.exe potřebné k nasazení balíčku na cílovém prostředí, a *SetParameters.xml* soubor, který určuje nezbytné podrobnosti cílového prostředí. Jsme také vyvolat **GatherPackagesForPublishing** cíl, který generuje kolekci položek obsahujících *deploy.cmd* soubory, které vás zajímají. V podstatě **PublishWebPackages** target provádí tyto funkce:

- Zpracovává *SetParameters.xml* souboru pro každý balíček zahrnuje správné podrobnosti pro cílové prostředí pomocí **xmlpoke –** úloh.
- Vyvolá *deploy.cmd* souboru pro každý balíček pomocí příslušných přepínačů.

Stejně jako **PublishDbPackages** cíl, **PublishWebPackages** target používá k zajištění, že cíl je provede jednou pro každý balíček webové dávkování cíle.


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


V rámci cíle **Exec** úkolů se používá ke spuštění *deploy.cmd* souboru pro každý web balíček.


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Další informace o konfiguraci nasazení webových balíčků naleznete v tématu [sestavení a balení projektů webových aplikací](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Závěr

Toto téma poskytuje návod, jak rozdělit projekt soubory se používají k řízení procesu sestavení a nasazení od začátku do konce pro ukázkové řešení Správce kontaktů. Tento přístup umožňuje spuštění komplexní, celopodniková nasazení v jedné a opakovatelným kroku, jednoduše spuštěním souboru příkazu specifických pro prostředí.

## <a name="further-reading"></a>Další čtení

Podrobnější Úvod do souborů projektu a WPP najdete v tématu [uvnitř the Microsoft Build Engine: použití nástroje MSBuild a Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi a William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Předchozí](understanding-the-project-file.md)
> [další](building-and-packaging-web-application-projects.md)
