---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: "Vysvětlení souboru projektu | Microsoft Docs"
author: jrjlee
description: "Soubory projektu Microsoft Build Engine (MSBuild) jsou jádrem procesem sestavení a nasazení. Toto téma začíná koncepční přehled nástroje MSBuild..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 09c3793e9cdddb7c42cf966f2d079245f441540c
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
<a name="understanding-the-project-file"></a>Vysvětlení souboru projektu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Soubory projektu Microsoft Build Engine (MSBuild) jsou jádrem procesem sestavení a nasazení. Toto téma začíná koncepční přehled nástroje MSBuild a soubor projektu. Popisuje klíčové součásti, které budete setkat při práci se soubory projektu a funguje přes příklad použití souborů projektu nasazení reálného aplikací.
> 
> Získáte informace:
> 
> - Jak MSBuild používá soubory projektu nástroje MSBuild k sestavení projektů.
> - Jak MSBuild integruje s technologií nasazení, jako je nástroj pro nasazení Internetové informační služby (IIS) webu (Web Deploy).
> - Jak pochopit klíčové komponenty souboru projektu.
> - Jak můžete použít soubory projektu pro vytváření a nasazení komplexních aplikací.


## <a name="msbuild-and-the-project-file"></a>MSBuild a soubor projektu

Při vytváření a sestavení řešení v sadě Visual Studio, Visual Studio použije nástroje MSBuild vytvořit jednotlivé projekty v řešení. Každý projekt Visual Studio obsahuje soubor projektu nástroje MSBuild s příponou souboru odpovídající typu projektu & #x 2014; například projekt C# (.csproj), Visual Basic.NET projektu (.vbproj) nebo databázového projektu (.dbproj). Chcete-li vytvořit projekt, musí MSBuild zpracovat soubor projektu, který je přidružený k projektu. Soubor projektu je dokument XML, který obsahuje všechny informace a pokyny, že MSBuild potřebuje k vytvoření projektu, jako je obsah, který chcete zahrnout, požadavky na platformu, informace o správě verzí, webového serveru nebo nastavení databázového serveru a úlohy, které je nutné provést.

Soubory projektu nástroje MSBuild jsou založené na [schématu MSBuild XML](https://msdn.microsoft.com/library/5dy88c2e.aspx), a v důsledku procesu sestavení je zcela otevřený a transparentní. Navíc nemusíte instalaci sady Visual Studio, aby bylo možné používat modul MSBuild & #x 2014; spustitelný soubor MSBuild.exe je součástí rozhraní .NET Framework a můžete ji spustit z příkazového řádku. Jako vývojář můžete vytvořit vaše vlastní soubory projektu nástroje MSBuild, pomocí nástroje MSBuild XML schématu, ukládat sofistikovaná a podrobných kontrolu nad projekty jsou vytvořené a nasazené. Tyto soubory vlastních projektů fungovat stejným způsobem jako soubory projektu, které automaticky generuje sada Visual Studio.

> [!NOTE]
> Můžete taky použít soubory projektu nástroje MSBuild službou sestavení týmu v Team Foundation Server (TFS). Například můžete použít soubory projektu ve scénářích průběžnou integraci (CI) k automatizaci nasazení v testovacím prostředí při vrácení se změnami nový kód. Další informace najdete v tématu [konfigurace Team Foundation Server pro nasazení webu automatizované](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).


### <a name="project-file-naming-conventions"></a>Soubor projektu konvence vytváření názvů

Když vytvoříte soubory projektu, můžete použít libovolnou příponou, které chcete. Usnadnění řešení ostatním uživatelům pochopit, musí tyto běžné konvence použít:

- Když vytvoříte soubor projektu, který sestaví projekty, používáte .proj rozšíření.
- Používáte rozšíření .targets při vytváření opakovaně použitelných projektu soubor k importu do jiných souborů projektu. Soubory s příponou .targets obvykle není sestavení nic sami, jednoduše obsahují pokyny, které můžete importovat do .proj souborů.

### <a name="integration-with-deployment-technologies"></a>Integrace s technologií nasazení

Pokud jste pracovali s projekty webových aplikací v sadě Visual Studio 2010, jako jsou webové aplikace ASP.NET a webových aplikací ASP.NET MVC, budete vědět, že tyto projekty zahrnují integrovanou podporu pro balení a nasazení webové aplikace na cílovém prostředí. **Vlastnosti** stránky pro tyto projekty patří **balení/publikování webu** a **balení/publikování kódu SQL** karty, které můžete použít ke konfiguraci způsobu součásti vaší aplikace se zabalí a nasadit. Zobrazí se **balení/publikování webu** karty:

![](understanding-the-project-file/_static/image1.png)

Základní technologii za tyto možnosti se označuje jako webových publikování kanálu (WPP). JAKO v podstatě přináší MSBuild a [Web Deploy](https://go.microsoft.com/?linkid=9805122) společně zajistit dokončení procesu nasazení, balíčků a sestavení pro webové aplikace.

Dobrá zpráva je, že můžete využít výhod integrace bodů, které poskytuje jako při vytváření vlastních projektů souborů pro webové projekty. Pokyny k nasazení lze zahrnout do souboru projektu, který umožňuje vytvářet projekty, vytvořte Balíčky webového nasazení a instalaci těchto balíčků na vzdálených serverech prostřednictvím jediného souboru projektu a jediné volání MSBuild. Také můžete volat další spustitelné soubory jako součást procesu sestavení. Můžete například spustit nástroj příkazového řádku VSDBCMD.exe nasazení databáze ze souboru schématu. V průběhu tohoto tématu uvidíte, jak můžete využít těchto funkcí, které splňují požadavky vaší podnikové scénáře nasazení.

> [!NOTE]
> Další informace o tom, jak proces nasazení webové aplikace funguje, najdete v části [ASP.NET webové aplikace projektu nasazení přehled](https://msdn.microsoft.com/library/dd394698.aspx).


## <a name="the-anatomy-of-a-project-file"></a>Anatomie souboru projektu

Předtím, než se podíváte na procesu sestavení podrobněji, je vhodné věnovat chvíli se seznamte s základní struktura soubor projektu nástroje MSBuild. Tato část obsahuje přehled běžných prvků, které budete dojde při zkontrolovat, upravit nebo vytvořit soubor projektu. Konkrétně se dozvíte:

- Jak používat *vlastnosti* k správě proměnných pro proces sestavení.
- Jak používat *položky* k identifikaci vstupy do procesu sestavení, jako jsou soubory kódu.
- Jak používat *cíle* a *úlohy* umožníte provádění pokyny k MSBuild, pomocí *vlastnosti* a *položky* definované jinde v soubor projektu.

To ukazuje vztah mezi klíčové prvky v souboru projektu nástroje MSBuild:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>Element projektu

[Projektu](https://msdn.microsoft.com/library/bcxfsh87.aspx) element není kořenovým elementem každého souboru projektu. Kromě identifikace schéma XML pro soubor projektu **projektu** element může obsahovat atributy zadat vstupní body procesu sestavení. Například v [obraťte se na správce ukázkové řešení](the-contact-manager-solution.md), *Publish.proj* souboru Určuje spuštění sestavení voláním cíl s názvem **FullPublish**.


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>Vlastnosti a podmínky

Soubor projektu obvykle nutné poskytnout spoustu různých řadu informací, aby bylo možné úspěšně vytvářet a nasazovat vašich projektů. Tyto údaje můžou zahrnovat názvů serverů, připojovací řetězce, pověření, konfigurace sestavení, zdrojové a cílové cesty k souborům a další informace, které chcete zahrnout pro podporu přizpůsobení. V souboru projektu, vlastnosti musí být definovány v rámci [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) elementu. Vlastnosti nástroje MSBuild obsahovat páry klíč hodnota. V rámci **PropertyGroup** elementu, název elementu definuje klíč vlastnosti a obsah elementu definuje hodnotu vlastnosti. Můžete například definovat vlastností s názvem **ServerName** a **ConnectionString** k ukládání statických serveru název a připojovací řetězec.


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


Pokud chcete načíst hodnotu vlastnosti, použijte formát **$(***PropertyName***) ***.* Chcete-li například načíst hodnotu **ServerName** vlastnost, zadali byste:


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> Uvidíte příklady, jak a kdy použít hodnoty vlastností později v tomto tématu.


Vložení informací o jako statické vlastnosti v souboru projektu není vždy ideální přístup pro správu procesu sestavení. V celá řada možných scénářů budete chtít získat informace z jiných zdrojů nebo umožnit uživatelům poskytovat informace z příkazového řádku. MSBuild můžete zadat jakoukoli hodnotu vlastnosti jako parametr příkazového řádku. Například může uživatel zadat hodnotu pro **ServerName** potvrdí spuštění MSBuild.exe z příkazového řádku.


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> Další informace o argumentů a přepínače, které můžete použít se MSBuild.exe najdete v tématu [příkazového řádku MSBuild – Reference](https://msdn.microsoft.com/library/ms164311.aspx).


Stejná syntaxe vlastnost slouží k získání hodnot proměnných prostředí a vlastnosti předdefinované projektu. Velké množství běžně používané vlastnosti jsou definovány pro vás a je použijete v souborech projektu zahrnutím název relevantní parametru. Chcete-li například načíst aktuální platformy projektu & #x 2014; například **x86** nebo **AnyCpu**& #x 2014; můžete zahrnout **$(Platform)** odkaz na vlastnost v soubor projektu. Další informace najdete v tématu [makra pro příkazy sestavení a vlastnosti](https://msdn.microsoft.com/library/c02as0cs.aspx), [běžné vlastnosti projektu nástroje MSBuild](https://msdn.microsoft.com/library/bb629394.aspx), a [rezervované vlastnosti](https://msdn.microsoft.com/library/ms164309.aspx).

Vlastnosti se často používají ve spojení s *podmínky*. Většina elementů MSBuild podporu **podmínku** atribut, který umožňuje zadat kritéria, na kterých by měly vyhodnotit MSBuild elementu. Představte si třeba tuto definici vlastnosti:


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


Když MSBuild zpracovává tuto definici vlastnosti, nejdřív zkontroluje, jestli **$(OutputRoot)** hodnota vlastnosti je k dispozici. Pokud hodnota vlastnosti je prázdné & #x 2014; jinými slovy, uživatel nebyl k dispozici hodnota této vlastnosti & #x 2014; je podmínka vyhodnocena jako **true** a hodnota vlastnosti je nastavena na **... \Publish\Out**. Pokud uživatel zadal hodnotu pro tuto vlastnost, je podmínka vyhodnocena jako **false** a hodnota statické vlastnosti se nepoužívá.

Další informace o různých způsobech, ve kterém můžete zadat podmínky, najdete v části [podmínky nástroje MSBuild](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Položky a skupiny položek

Jedním z důležitých rolí souboru projektu je definovat vstupy do procesu sestavení. Obvykle se tyto vstupní hodnoty jsou soubory & #x 2014; soubory kódu, konfigurační soubory, soubory příkazů a další soubory, které je potřeba zpracovat nebo kopírovat jako část procesu sestavení. Ve schématu projektu nástroje MSBuild, jsou reprezentovány tyto vstupy [položky](https://msdn.microsoft.com/library/ms164283.aspx) elementy. V souboru projektu položky musí být definován v rámci [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) element. Stejně jako **vlastnost** prvky, můžete pojmenovat **položky** element ale chcete. Nicméně je nutné zadat **zahrnout** atribut k identifikaci souboru nebo zástupný znak, který představuje položku.


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


Zadáním více **položky** elementy se stejným názvem, efektivně vytváříte seznam prostředků s názvem. Podívat se uvnitř jeden ze souborů projektu, které sada Visual Studio vytvoří je dobrý způsob, jak je vidět v akci. Například *ContactManager.Mvc.csproj* soubor v ukázkové řešení obsahuje velké množství položek skupin, každý s několik stejně jako s názvem **položky** elementy.


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


Tímto způsobem je instruující soubor projektu nástroje MSBuild vytvořit seznam souborů, které potřebují ke zpracování v stejným způsobem & #x 2014; **odkaz** seznam obsahuje sestavení, které musí být místní pro úspěšné sestavení, **Zkompilovat** seznam obsahuje soubory kódu, které musí být zkompilovány, a **obsahu** seznam obsahuje prostředky, které musí být zkopírovány v nezměněném stavu. Podíváme jak procesu sestavení odkazuje a používá tyto položky později v tomto tématu.

Může také obsahovat elementy položky [itemmetadata –](https://msdn.microsoft.com/library/ms164284.aspx) podřízené elementy. Tyto jsou uživatelem definované páry klíč hodnota a v podstatě představují vlastnosti, které jsou specifické pro danou položku. Například mnoho **zkompilovat** elementy položky v souboru projektu zahrnují **DependentUpon** podřízené elementy.


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> Kromě metadata položky vytvořené uživatelem jsou všechny položky přiřadit různé běžná metadata na vytvoření. Další informace najdete v tématu [Metadata známé položky](https://msdn.microsoft.com/library/ms164313.aspx).


Můžete vytvořit **ItemGroup** elementů v rámci kořenové úrovni **projektu** element nebo v rámci konkrétní **cíl** elementy. **ItemGroup** také podporují elementy **podmínku** atributy, které umožňuje přizpůsobit vstupy do procesu sestavení podle podmínek jako konfigurace projektu nebo platformu.

### <a name="targets-and-tasks"></a>Cílů a úloh

Ve schématu MSBuild [úloh](https://msdn.microsoft.com/library/77f2hx1s.aspx) element reprezentuje jednotlivé sestavení instrukce (nebo úloh). MSBuild zahrnuje velkého množství předdefinované úlohy. Příklad:

- **Kopie** úloh zkopíruje soubory do nového umístění.
- **Csc** úloh vyvolá kompilátor Visual C#.
- **Vbc –** úloh vyvolá kompilátoru jazyka Visual Basic.
- **Exec** úloha spustí zadaný program.
- **Zpráva** úloh zapíše zprávu do protokolovacího nástroje.

> [!NOTE]
> Úplné podrobnosti úlohy, které jsou k dispozici ihned najdete v tématu [MSBuild – Reference úlohy](https://msdn.microsoft.com/library/7z253716.aspx). Další informace o úlohách, včetně toho, jak vytvořit vlastní vlastní úlohy, najdete v části [úlohy nástroje MSBuild](https://msdn.microsoft.com/library/ms171466.aspx).


Úlohy musí být vždy obsažena v [cíl](https://msdn.microsoft.com/library/t50z2hka.aspx) elementy. A **cíl** element je sada jedné nebo více úloh, které jsou prováděny postupně a soubor projektu může obsahovat více cílů. Pokud chcete spustit úlohu nebo sadu úloh, můžete vyvolat cíl, který je obsahuje. Předpokládejme například, že máte soubor Jednoduchý projekt, který zaznamená zprávu.


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


Cíle z příkazového řádku, můžete vyvolat pomocí **/t** přepínač tak, aby zadat cílový.


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


Alternativně můžete přidat **defaulttargets –** atribut **projektu** element k určení cílů, které chcete vyvolat.


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


V takovém případě nepotřebujete zadat cílový z příkazového řádku. Jednoduše můžete určit soubor projektu a bude vyvolání MSBuild **FullPublish** cíl pro vás.


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


Cílů a úloh může obsahovat **podmínku** atributy. Takto můžete vynechat celý cíle nebo jednotlivé úlohy, pokud jsou splněny určité podmínky.

Obecně platí když vytvoříte užitečné úkoly a cíle, budete potřebovat k odkazování na vlastnosti a položky, které jste definovali jinde v souboru projektu:

- Chcete-li použít hodnotu vlastnosti, zadejte **$(***PropertyName***)**, kde *PropertyName* je název **vlastnost** element nebo název parametr.
- Chcete-li použít položku, zadejte **@(***název položky***)**, kde *název položky* je název **položky** elementu.

> [!NOTE]
> Mějte na paměti, že pokud vytvoříte více položek se stejným názvem, kterou sestavujete seznamu. Na rozdíl od Pokud vytvoříte více vlastností se stejným názvem, poslední hodnota vlastnosti, které poskytnete přepíše jakékoli předchozí vlastnosti se stejným názvem & #x 2014; vlastnost může obsahovat pouze jednu hodnotu.


Například v *Publish.proj* souboru v ukázkové řešení, podívejte se na **BuildProjects** cíl.


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


V této ukázce můžete sledovat tyto klíčové body:

- Pokud **BuildingInTeamBuild** je zadán parametr a má hodnotu **true**, budou spuštěny žádné úlohy uvnitř tohoto cíle.
- Cíl obsahuje jednu instanci [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) úloh. Tato úloha umožňuje vytvářet jiné nástroje MSBuild projekty.
- **ProjectsToBuild** položky předaný úlohu. Tato položka by mohl představovat seznam souborů projekt nebo řešení, všechny určené **ProjectsToBuild** položky elementů v rámci skupinu položek. V takovém případě **ProjectsToBuild** položku odkazuje na soubor jediném řešení.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Hodnoty vlastností předaný **MSBuild** úloh zahrnout parametry s názvem **OutputRoot** a **konfigurace**. Tyto jsou nastaveny na hodnoty parametrů, pokud jsou k dispozici nebo statické vlastnosti hodnoty, pokud nejsou.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

Můžete také zjistit, který **MSBuild** úloh vyvolá cíl s názvem **sestavení**. Toto je jedna z několik předdefinovaných cílů, které často používají v souborech projektu sady Visual Studio a jsou k dispozici v souborech vlastních projektů, jako je třeba **sestavení**, **Vyčistit**, **znovu sestavit**, a **publikování**. Získáte informace o používání cíle a úlohy pro řízení procesu sestavení a o informace **MSBuild** úloha v konkrétní později v tomto tématu.

> [!NOTE]
> Další informace o cíle najdete v tématu [cíle MSBuild](https://msdn.microsoft.com/library/ms171462.aspx).


## <a name="splitting-project-files-to-support-multiple-environments"></a>Rozdělení souborů projektu pro podporu prostředí s více

Předpokládejme, že chcete být schopni nasadit řešení do několika prostředích, jako je testovací servery, pracovní platformy a provozní prostředí. Konfigurace může podstatně liší mezi tyto prostředí & #x 2014; ne jen z hlediska názvů serverů, připojovací řetězce a tak dále, ale i potenciálně z hlediska přihlašovací údaje, nastavení zabezpečení a spoustu dalších faktorů. Pokud potřebujete pravidelně to udělat, není skutečně vhodné upravit víc vlastností v souboru projektu pokaždé, když přejdete na cílovém prostředí. Ani je ideální řešení tak, aby vyžadovala nekonečná seznam hodnot vlastností, které mají být poskytovány procesu sestavení.

Naštěstí představuje alternativu. MSBuild umožňuje konfiguraci sestavení rozdělit do více souborech projektu. Chcete-li zobrazit, jak to funguje, v ukázkové řešení, Všimněte si, zda jsou dva soubory vlastních projektů:

- *Publish.proj*, která obsahuje vlastnosti, položky, a cíle, které jsou společné pro všechna prostředí.
- *Env Dev.proj*, která obsahuje vlastnosti, které jsou specifické pro vývojářského prostředí.

Nyní Všimněte si, že *Publish.proj* soubor obsahuje [Import](https://msdn.microsoft.com/library/92x05xfs.aspx) element bezprostředně pod otevření **projektu** značky.


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


**Importovat** element se používá k importu obsahu souboru jiný soubor projektu nástroje MSBuild do aktuální soubor projektu nástroje MSBuild. V takovém případě **TargetEnvPropsFile** parametr obsahuje název souboru projektu, který chcete importovat. Při spuštění nástroje MSBuild, můžete zadat hodnotu tohoto parametru.


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


To efektivně sloučí obsah ze dvou souborů do jediného souboru projektu. Použití tohoto přístupu, můžete vytvořit jeden soubor obsahující konfiguraci universal sestavení a víc souborů doplňující projektu obsahující vlastnosti specifické pro prostředí. V důsledku toho jednoduše spuštěný příkaz s jiným parametrem hodnotu umožňuje nasadit řešení do různých prostředí.

![](understanding-the-project-file/_static/image3.png)

Rozdělení souborů projektu tímto způsobem je vhodné provést. To umožňuje vývojáři k nasazení do prostředí s více spuštěním jednoduchého příkazu, aniž by duplikace vlastností universal sestavení napříč více souborech projektu.

> [!NOTE]
> Pokyny k přizpůsobení souborů projektu specifický pro vaše vlastní prostředí serveru najdete v tématu [konfigurace vlastnosti nasazení pro cílové prostředí](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="conclusion"></a>Závěr

Toto téma poskytuje obecné Úvod do souborů projektu nástroje MSBuild a vysvětlení, jak můžete vytvořit vlastní soubory vlastní projektu k řízení procesu sestavení. Rovněž zaveden koncept rozdělení souborů projektu do univerzální sestavení pokyny a vlastnosti specifické pro prostředí sestavení, můžete snadno vytvořit a nasadit projekty do více cílů.

Dalším tématu [Principy procesu sestavení](understanding-the-build-process.md), poskytuje lepší náhled na tom, jak můžete použít soubory projektu do ovládacího prvku sestavení a nasazení podle návodem nasazení řešení se realistické úrovní složitost.

## <a name="further-reading"></a>Další čtení

Podrobnější Úvod do souborů projektu a jako, najdete v části [uvnitř Microsoft Build Engine: pomocí nástroje MSBuild a Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi a William Bartholomew, ISBN: 978-0-7356-4524-0.

>[!div class="step-by-step"]
[Předchozí](setting-up-the-contact-manager-solution.md)
[další](understanding-the-build-process.md)
