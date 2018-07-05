---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Vysvětlení souboru projektu | Dokumentace Microsoftu
author: jrjlee
description: Soubory projektu Microsoft Build Engine (MSBuild) leží v srdci procesu sestavení a nasazení. Toto téma začíná koncepční přehled nástroje MSBuild...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 89c5c7906ccfc453195b788cbc6393dc74cda1fb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377201"
---
<a name="understanding-the-project-file"></a>Vysvětlení souboru projektu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Soubory projektu Microsoft Build Engine (MSBuild) leží v srdci procesu sestavení a nasazení. Toto téma začíná koncepční přehled nástroje MSBuild a soubor projektu. Popisuje klíčové komponenty, které budete setkat při práci se soubory projektu, a funguje příklad toho, jak můžete soubory projektu nasazení reálné aplikace.
> 
> Co se dozvíte:
> 
> - Jak nástroj MSBuild používá soubory projektu MSBuild k sestavení projektů.
> - Jak se dá integrovat nástroj MSBuild s technologií nasazení, jako je nástroj pro nasazení Internetové informační služby (IIS) webu (nasazení webu).
> - Jak pochopit klíčové komponenty souboru projektu.
> - Jak vytvářet a nasazovat komplexní aplikace můžete soubory projektu.


## <a name="msbuild-and-the-project-file"></a>Nástroj MSBuild a soubor projektu

Při vytváření a sestavovat řešení v sadě Visual Studio, Visual Studio používá MSBuild k sestavení každý projekt ve vašem řešení. Každý projekt sady Visual Studio obsahuje soubor projektu nástroje MSBuild s příponou souboru odpovídající typu projektu&#x2014;například C# projektu (.csproj), Visual Basic.NET projektu (.vbproj) nebo projektu databáze (.dbproj). Aby bylo možné sestavit projekt, MSBuild musí zpracovat soubor projektu, který je přidružený k projektu. Soubor projektu je dokument XML, který obsahuje všechny informace a pokyny, že nástroj MSBuild potřebuje, aby sestavení projektu, jako je obsah, který chcete zahrnout, požadavky na platformu, informace o verzi, webového serveru nebo nastavení databázového serveru a úkoly, které se musí provádět.

Soubory projektu MSBuild jsou založeny na [schématu MSBuild XML](https://msdn.microsoft.com/library/5dy88c2e.aspx), a díky tomu proces sestavení je zcela otevřený a transparentní. Kromě toho není nutné k instalaci sady Visual Studio, aby bylo možné používat stroji MSBuild engine&#x2014;spustitelný soubor MSBuild.exe je součástí rozhraní .NET Framework, a můžete ji spustit z příkazového řádku. Jako vývojář můžete si vytvořit své vlastní soubory projektu MSBuild pomocí schématu XML nástroje MSBuild, a stanovit propracované a detailní kontrolu nad jak jsou vaše projekty sestavíte a nasadíte. Tyto soubory vlastních projektů pracovat přesně stejně jako soubory projektu, které sada Visual Studio vygeneruje automaticky.

> [!NOTE]
> Soubory projektu MSBuild můžete použít také ke službě Team Build v Team Foundation Server (TFS). Soubory projektu ve scénářích kontinuální integrace (CI) můžete například použít k automatizaci nasazení do testovacího prostředí, když nového kódu se změnami. Další informace najdete v tématu [konfigurace serveru Team Foundation Server pro nasazení webu pomocí automatizované](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).


### <a name="project-file-naming-conventions"></a>Soubor projektu zásady vytváření názvů

Při vytváření souborů projektu, můžete použít libovolnou příponou, který rádi používáte. Usnadnění řešení ostatním uživatelům pochopit, by měly tyto běžné konvence použít:

- Při vytváření souboru projektu, který sestavuje projekty, pomocí rozšíření souborů .proj.
- Použití rozšíření .targets, při vytváření souboru projektu opakovaně použitelné pro import do jiných souborů projektu. Soubory s příponou .targets obvykle nesestavujte všechno sami, jednoduše obsahují pokyny, které lze importovat do souborů .proj souborů.

### <a name="integration-with-deployment-technologies"></a>Integrace s technologií nasazení

Pokud už máte s projekty webových aplikací v sadě Visual Studio 2010, jako jsou webové aplikace ASP.NET a webových aplikacích ASP.NET MVC, budete vědět, že tyto projekty obsahují integrovanou podporu pro balení a nasazení webové aplikace na cílovém prostředí. **Vlastnosti** stránky pro tyto projekty obsahují **balení/publikování webu** a **balení/publikování kódu SQL** karty, které vám umožní nakonfigurovat jak součástí vaší aplikace se zabalit a nasadit. To ukazuje **balení/publikování webu** kartu:

![](understanding-the-project-file/_static/image1.png)

Základní technologie za tyto funkce se označuje jako Web Publishing kanálu (WPP). WPP v podstatě přináší MSBuild a [Webdeploy](https://go.microsoft.com/?linkid=9805122) dohromady a poskytují kompletní proces sestavení, balení a nasazování pro webové aplikace.

Dobrou zprávou je, že můžete využít výhod integrace body, které poskytuje WPP při vytváření vlastních projektů soubory pro webové projekty. V souboru projektu, který vám umožní sestavovat projekty, vytvořit balíčky webového nasazení a instalaci těchto balíčků ve vzdálených serverů pomocí jednoho projektu souborové služby a jedním voláním metody MSBuild může obsahovat pokyny k nasazení. Další spustitelné soubory můžete také volat jako součást procesu sestavení. Můžete například spustit nástroj příkazového řádku VSDBCMD.exe nasazení databáze ze souboru schématu. V průběhu tohoto tématu uvidíte, jak můžete využít tyto funkce pro splnění požadavků vašich podnikových scénářích nasazení.

> [!NOTE]
> Další informace o tom, jak funguje proces nasazení webových aplikací najdete v tématu [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698.aspx).


## <a name="the-anatomy-of-a-project-file"></a>Anatomie souboru projektu

Předtím, než se podíváte na proces sestavení podrobněji, je vhodné trvá několik minut se seznámit s základní strukturu souboru projektu MSBuild. Tato část obsahuje přehled běžných prvky, které se můžete setkat při zobrazit, upravit nebo vytvořit soubor projektu. Konkrétně se dozvíte:

- Jak používat *vlastnosti* ke správě proměnné pro proces sestavení.
- Jak používat *položky* k identifikaci vstupy do procesu sestavení, jako jsou soubory kódu.
- Jak používat *cíle* a *úlohy* pokyny spuštění nástroje MSBuild, pomocí *vlastnosti* a *položky* definovaný jinde v soubor projektu.

To znázorňuje vztah mezi klíčové prvky v souboru projektu MSBuild:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>Element projektu

[Projektu](https://msdn.microsoft.com/library/bcxfsh87.aspx) prvek je kořenovým prvkem každého souboru projektu. Kromě identifikace schéma XML pro soubor projektu **projektu** element můžete zahrnout atributů k určení vstupní body procesu sestavení. Například v [ukázkové řešení Správce kontaktů](the-contact-manager-solution.md), *Publish.proj* souboru Určuje, že sestavení spustit voláním cíl s názvem **FullPublish**.


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>Vlastnosti a podmínky

Soubor projektu je obvykle potřeba poskytují velké množství informací, aby bylo možné úspěšně sestavit a nasadit vaše projekty různé softwarové součásti. Tyto údaje by mohla obsahovat názvy serverů, připojovací řetězce, pověření, konfigurace sestavení, zdrojových a cílových cest k souborům a další informace, které chcete zahrnout podporující přizpůsobení. V souboru projektu, musí být vlastnosti definované v rámci [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) elementu. Vlastnosti nástroje MSBuild obsahovat páry klíč hodnota. V rámci **PropertyGroup** elementu, název elementu definuje klíč vlastnosti a obsah elementu, který definuje hodnoty vlastnosti. Například můžete definovat vlastnosti s názvem **ServerName** a **ConnectionString** k ukládání statických serveru název a připojovací řetězec.


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


Pokud chcete načíst hodnotu vlastnosti, použijte formát <strong>$(</strong><em>PropertyName</em><strong>)</strong><em>.</em> Například pro načtení hodnoty <strong>ServerName</strong> vlastnost, zadali byste:


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> Zobrazí se vám příklady toho, jak a kdy použít hodnoty vlastností dále v tomto tématu.


Vložení informací o jako statických vlastností v souboru projektu není vždy ideální přistupovat ke správě procesu sestavení. V možných scénářů budete chtít získat informace z jiných zdrojů nebo umožněte uživateli zadat informace z příkazového řádku. Nástroj MSBuild umožňuje zadat jakoukoli hodnotu vlastnosti jako parametr příkazového řádku. Například může uživatel zadat hodnotu pro **ServerName** když uživatel spustí z příkazového řádku MSBuild.exe.


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> Další informace o argumenty a přepínače, můžete použít s MSBuild.exe, naleznete v tématu [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).


Podle stejné syntaxe vlastnost slouží k získání hodnot proměnných prostředí a předdefinovaných projektových vlastnosti. Mnoho běžně používaných vlastností jsou definovány pro vás, můžete si je v souborech projektu zahrnutím odpovídající parametr název. Například chcete-li načíst aktuální platforma pro projektovou&#x2014;například **x86** nebo **AnyCpu**&#x2014;můžete zahrnout **$(Platform)** odkaz na vlastnost v váš soubor projektu. Další informace najdete v tématu [Macros for Build Commands and Properties](https://msdn.microsoft.com/library/c02as0cs.aspx), [obecné vlastnosti projektu nástroje MSBuild](https://msdn.microsoft.com/library/bb629394.aspx), a [rezervované vlastnosti](https://msdn.microsoft.com/library/ms164309.aspx).

Vlastnosti jsou často používá ve spojení s *podmínky*. Podpora většinu prvků MSBuild **podmínku** atribut, který vám umožní zadat kritéria, podle kterých by se měl vyhodnotit MSBuild elementu. Zvažte například tuto definici vlastnosti:


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


Když nástroj MSBuild zpracovává tuto definici vlastnosti, ji nejprve zkontroluje, jestli **$(OutputRoot)** hodnota vlastnosti je k dispozici. Pokud hodnota vlastnosti je prázdná&#x2014;jinými slovy, uživatel neposkytl hodnotu pro tuto vlastnost&#x2014;bude podmínka vyhodnocena jako **true** a hodnota vlastnosti je nastavená na **... \Publish\Out**. Pokud uživatel zadal hodnotu pro tuto vlastnost, bude podmínka vyhodnocena jako **false** a hodnota statickou vlastnost se nepoužívá.

Další informace o různých způsobech, ve kterém můžete zadat podmínky najdete v tématu [podmínky nástroje MSBuild](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Položky a skupiny položek

Jednou z důležitých rolí soubor projektu je definování vstupy do procesu sestavení. Obvykle tyto vstupy jsou soubory&#x2014;kódu souborů, konfigurační soubory, soubory příkazů a všechny další soubory, které potřebujete ke zpracování nebo kopírovat jako součást procesu sestavení. Ve schématu projektu nástroje MSBuild, jsou reprezentovány tyto vstupy [položky](https://msdn.microsoft.com/library/ms164283.aspx) elementy. V souboru projektu, musí být položky definované v rámci [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) elementu. Stejně jako **vlastnost** prvků, třeba název **položky** element libovolně. Nicméně je nutné zadat **zahrnout** atribut k identifikaci souboru nebo zástupný znak, který představuje položku.


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


Podle určení více podmínek vyhledávání **položky** elementy se stejným názvem, efektivní vytváření pojmenovaný seznam prostředků. Podívat se do jednoho ze souborů projektu, které sada Visual Studio vytvoří je dobrým způsobem, jak to můžete vidět v akci. Například *ContactManager.Mvc.csproj* soubor v ukázkovém řešení obsahuje velké množství položek skupin, každou s několika stejně jako s názvem **položky** elementy.


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


Tímto způsobem soubor projektu je mu dát pokyn k vytvoření seznamu souborů, které musí být zpracovány stejně jako&#x2014; **odkaz** seznam obsahuje sestavení, která musí být splněné pro úspěšné sestavení  **Kompilace** seznam obsahuje soubory kódu, které musí být kompilovány, a **obsahu** seznam obsahuje prostředky, které musí být zkopírován beze změny. Podíváme se na tom, jak proces sestavení odkazuje na a používá tyto položky později v tomto tématu.

Může také obsahovat prvky položky [itemmetadata –](https://msdn.microsoft.com/library/ms164284.aspx) podřízené prvky. Tyto jsou páry klíč hodnota definované uživatelem a v podstatě představují vlastnosti, které jsou specifické pro danou položku. Například velké množství **kompilaci** obsahovat prvky položky v souboru projektu **DependentUpon** podřízené prvky.


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> Kromě metadata položky vytvořené uživatelem jsou přiřazeny všechny položky při vytváření různých běžná metadata. Další informace najdete v tématu [Metadata známé položky](https://msdn.microsoft.com/library/ms164313.aspx).


Můžete vytvořit **ItemGroup** elementů v rámci na kořenové úrovni **projektu** element nebo v rámci konkrétní **cílové** elementy. **ItemGroup** prvky také podporují **podmínku** atributy, které umožňuje přizpůsobit vstupy do procesu sestavení podle podmínek, jako je konfigurace projektu nebo platformu.

### <a name="targets-and-tasks"></a>Cíle a úlohy

Ve schématu MSBuild [úloh](https://msdn.microsoft.com/library/77f2hx1s.aspx) element představuje jednotlivý build instrukce (nebo úloh). Nástroj MSBuild obsahuje velké množství předdefinované úlohy. Příklad:

- **Kopírování** úloha zkopíruje soubory do nového umístění.
- **Csc** úloha vyvolá kompilátor Visual C#.
- **Vbc –** úloha vyvolá kompilátor jazyka Visual Basic.
- **Exec** úkol spustí zadaný program.
- **Zpráva** úkolu zapíše zprávu do protokolovací nástroj.

> [!NOTE]
> Úplné podrobnosti úlohy, které jsou dostupné ihned najdete v tématu [– referenční dokumentace úlohy nástroje MSBuild](https://msdn.microsoft.com/library/7z253716.aspx). Další informace o úlohách, včetně vytváření vlastních úloh, naleznete v tématu [úlohy nástroje MSBuild](https://msdn.microsoft.com/library/ms171466.aspx).


Úlohy musí být vždy obsažen v rámci [cílové](https://msdn.microsoft.com/library/t50z2hka.aspx) elementy. A **cílové** prvek je sada jeden nebo více úkolů, které jsou spouštěny postupně, a soubor projektu může obsahovat více cílů. Pokud chcete spustit úlohu nebo sadu úloh, vyvoláte cíl, který je obsahuje. Předpokládejme například, že máte jednoduchý projekt soubor, který zaznamená zprávu.


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


Cíle z příkazového řádku lze vyvolat pomocí **/t** přepínač k určení cíle.


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


Alternativně můžete přidat **defaulttargets –** atribut **projektu** element, chcete-li určit cíle, které chcete vyvolat.


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


V takovém případě není nutné zadat cíl z příkazového řádku. Můžete jednoduše zadat soubor projektu, a vyvolá MSBuild **FullPublish** cílové za vás.


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


Cíle a úlohy může obsahovat **podmínku** atributy. V důsledku toho můžete vynechat celý cíle nebo jednotlivé úlohy, pokud jsou splněny určité podmínky.

Obecně řečeno při vytváření užitečných úlohy a cíle, musíte odkazovat na vlastnosti a položky, které jste definovali jinde v souboru projektu:

- Pokud chcete použít hodnotu vlastnosti, zadejte <strong>$(</strong><em>PropertyName</em><strong>)</strong>, kde <em>PropertyName</em> je název <strong>vlastnost</strong> element nebo název parametru.
- Chcete-li použít položku, zadejte <strong>@(</strong><em>ItemName</em><strong>)</strong>, kde <em>ItemName</em> je název <strong>položky</strong> elementu.

> [!NOTE]
> Mějte na paměti, že pokud vytvoříte více položek se stejným názvem, které sestavujete seznamu. Naopak pokud vytvoříte více vlastností se stejným názvem, poslední hodnota vlastnosti je poskytnout přepíše jakékoli předchozí vlastnosti se stejným názvem&#x2014;vlastnost může obsahovat pouze jednu hodnotu.


Například v *Publish.proj* soubor v ukázkovém řešení, podívejte se na **BuildProjects** cíl.


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


V této ukázce můžete sledovat tyto klíčové body:

- Pokud **BuildingInTeamBuild** je zadán parametr a má hodnotu **true**, budou spuštěny žádné úlohy uvnitř tohoto cíle.
- Cíl obsahuje jednu instanci [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) úloh. Tato úloha umožňuje vytvářet jiné projekty MSBuild.
- **ProjectsToBuild** položky je předán úkolu. Tato položka by mohly představovat seznam souborů projektu nebo řešení, všechny určené **ProjectsToBuild** položky elementů v rámci skupiny položek. V takovém případě **ProjectsToBuild** položka odkazuje na soubor jediného řešení.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Hodnoty vlastností, které jsou předány **MSBuild** úloh zahrnují parametry s názvem **OutputRoot** a **konfigurace**. Tyto vlastnosti se nastaví hodnoty parametrů, pokud jsou k dispozici, nebo hodnoty statickou vlastnost Pokud tomu tak není.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

Můžete také zjistit, který **MSBuild** úloha vyvolá cíl s názvem **sestavení**. Toto je jeden z několika předdefinovaných cílů, které hojně používají soubory projektu sady Visual Studio a jsou k dispozici ve vašich vlastních projektů souborech, jako je třeba **sestavení**, **Vyčistit**, **znovu sestavit**, a **publikovat**. Získáte další informace o použití cíle a úlohy pro řízení procesu sestavení a o **MSBuild** úkolů zejména dále v tomto tématu.

> [!NOTE]
> Další informace o cílech naleznete v tématu [cíle nástroje MSBuild](https://msdn.microsoft.com/library/ms171462.aspx).


## <a name="splitting-project-files-to-support-multiple-environments"></a>Rozdělení souborů projektu pro podporu více prostředí

Předpokládejme, že chcete nasadit řešení do různých prostředí, jako produkční prostředí, testovací servery a pracovní platformy. Konfigurace může podstatně liší mezi těmito prostředími&#x2014;ne jenom z hlediska názvy serverů, připojovací řetězce a tak dále, ale také potenciálně z hlediska přihlašovací údaje, nastavení zabezpečení a spoustu dalších faktorů. Pokud je potřeba pravidelně to provést, není ve skutečnosti účelné upravit víc vlastností v souboru projektu pokaždé, když přejdete cílovém prostředí. Ani to není ideální řešení tak, aby vyžadovala nekonečné seznam hodnot vlastností do procesu sestavení.

Naštěstí se o alternativu. Nástroj MSBuild umožňuje rozdělit konfigurace sestavení ve více souborech projektu. Pokud chcete zobrazit, jak to funguje, v ukázkovém řešení, Všimněte si, že existují dva soubory vlastních projektů:

- *Publish.proj*, který obsahuje vlastnosti, položky, a cíle, které jsou společné pro všechna prostředí.
- *Env Dev.proj*, který obsahuje vlastnosti, které jsou specifické pro prostředí pro vývojáře.

Nyní Všimněte si, že *Publish.proj* soubor obsahuje [Import](https://msdn.microsoft.com/library/92x05xfs.aspx) element bezprostředně pod otevírání **projektu** značky.


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


**Importovat** element slouží k importu obsahu souboru jiného souboru projektu nástroje MSBuild do aktuálního souboru projektu MSBuild. V takovém případě **TargetEnvPropsFile** parametr obsahuje název souboru projektu, které chcete importovat. Můžete zadat hodnotu pro tento parametr při spuštění nástroje MSBuild.


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


To efektivně sloučí obsah těchto dvou souborů do jediného souboru projektu. Tento přístup můžete vytvořit jeden projekt soubor, který obsahuje konfigurace univerzální sestavení a více souborů doplňující projekt obsahující vlastnosti specifické pro prostředí. V důsledku toho jednoduše spuštění příkazu s hodnotou jiné parametr vám umožní nasazení řešení do různých prostředí.

![](understanding-the-project-file/_static/image3.png)

Rozdělení souborů projektu tímto způsobem je vhodné provést. Umožňuje vývojářům nasadit do různých prostředí pomocí jediného příkazu při vyloučení duplicitních vlastností univerzální sestavení ve více souborech projektu.

> [!NOTE]
> Pokyny k přizpůsobení souborů projektu specifických pro prostředí pro prostředí serveru, naleznete v tématu [konfigurace vlastnosti nasazení pro cílové prostředí](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="conclusion"></a>Závěr

Toto téma poskytuje obecný úvod do souborů projektu MSBuild a vysvětlení, jak můžete vytvořit vlastní soubory k řízení procesu sestavení. Také představila koncept rozdělení soubory projektu do univerzální sestavení pokyny a vlastnosti specifické pro prostředí sestavení, které umožňují snadno vytvářet a nasazovat projekty do více cílů.

Dalším tématu s názvem [Principy procesu sestavení](understanding-the-build-process.md), poskytuje lepší přehled o použití soubory projektu do ovládacího prvku sestavení a nasazení můžete procházením až po nasazení řešení s realistické úroveň složitosti.

## <a name="further-reading"></a>Další čtení

Podrobnější Úvod do souborů projektu a WPP najdete v tématu [uvnitř the Microsoft Build Engine: použití nástroje MSBuild a Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi a William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Předchozí](setting-up-the-contact-manager-solution.md)
> [další](understanding-the-build-process.md)
