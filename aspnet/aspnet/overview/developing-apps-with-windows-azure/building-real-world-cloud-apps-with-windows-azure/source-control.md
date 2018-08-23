---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Zdrojový ovládací prvek (vytváření skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: riande
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 8402b73f5f9d063d958df39f98267468e4aef746
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755035"
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Správy zdrojového kódu (vytváření skutečných cloudových aplikací s Azure)
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Informace o e kniha najdete v tématu [první kapitoly](introduction.md).


Je nezbytné pro všechny projekty vývoje pro cloud, ne jenom Týmová prostředí správy zdrojového kódu. Nebylo by si představit úpravy zdrojového kódu nebo dokonce Wordový dokument bez funkce zpět a automatické zálohování a Správa zdrojového kódu nabízí tyto funkce na úrovni projektu, kde se dají ušetřit ještě víc času, kdy dojde k chybě. S cloudovými službami zdrojového ovládacího prvku už nemusíte dělat starosti o složité nastavování a můžete použít Visual Studio Online správy zdrojového kódu až 5 uživatelů zdarma.

První část této kapitole popisuje tři klíčové osvědčené postupy a mějte na paměti:

- [Skripty pro automatizaci považovat za zdrojový kód](#scripts) a verze je společně s kódu aplikace.
- [Nikdy se změnami tajných kódů](#secrets) (citlivá data, jako je například přihlašovací údaje) do úložiště zdrojového kódu.
- [Nastavit zdrojové větve](#devops) umožňující pracovních postupů DevOps.

Zbývající část kapitola obsahuje některé ukázky implementace tyto vzory v sadě Visual Studio, Azure a Visual Studio Online:

- [Přidat skripty do správy zdrojového kódu v sadě Visual Studio](#vsscripts)
- [Store citlivá data v Azure](#appsettings)
- [Pomocí Gitu ve Visual Studio a Visual Studio Online](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Skripty pro automatizaci považovat za zdrojového kódu

Když pracujete na projektu cloudu často měníte věci a chtějí mít možnost rychle reagovat na problémy, které nahlásili ho vaši zákazníci. Rychle reagovat zahrnuje použití skripty pro automatizaci, jak je vysvětleno v [automatizovat všechno](automate-everything.md) kapitoly. Všechny skripty, které použijete k vytvoření prostředí, nasaďte do ní, škálování, atd., musí být synchronizované se zdrojovým kódem vaší aplikace.

Aby skripty synchronizovaná s kódem, uložte je v systému správy zdrojů. Pak pokud byste někdy potřebovali chcete vrátit zpět změny nebo provést rychlou opravu produkční kód, který se liší od vývoje kódu, nemusíte plýtvat vaším časem při sledování nastavení, které byly změněny nebo které členové týmu mají kopie verze, které potřebujete. Budete jistí, které jsou synchronizované s základu kódu, který je pro budete potřebovat a budete jistí, že všichni členové týmu pracujete s stejné skripty skripty, které potřebujete. Potom, jestli je potřeba automatizovat testování a nasazování opravu hotfix do produkčního prostředí nebo vývoj nových funkcí, budete mít správný skript pro kód, který je potřeba aktualizovat.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Nezaškrtávejte políčko v tajných kódů

Úložiště zdrojového kódu je obvykle přístupné pro příliš mnoho lidí pro ni bude odpovídajícím způsobem zabezpečené místo pro citlivá data, jako jsou hesla. Pokud skripty využívají tajné kódy jako jsou hesla, parametrizujte tato nastavení tak, že nejsou uloženy ve zdrojovém kódu a ukládejte tajné klíče někde jinde.

Azure umožňuje stáhnout soubory, které obsahují například publikovat nastavení k automatizaci vytváření profilů publikování. Tyto soubory obsahují uživatelská jména a hesla, které mají oprávnění ke správě služeb Azure. Pokud použijete tuto metodu pro vytvoření publikačních profilů, a pokud odešlete těchto souborů do správy zdrojových kódů, každý, kdo má přístup k úložišti můžete zobrazit tyto uživatelská jména a hesla. Můžete bezpečně uložit heslo v profilu publikování samotné protože je šifrovaný a je *. pubxml.user* soubor, který ve výchozím nastavení není zahrnut ve správě zdrojového kódu.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Struktura větví zdroje usnadnit pracovních postupů DevOps

Jak implementovat větve v úložišti ovlivňuje vaši schopnost jak vyvíjet nové funkce a opravy problémů v produkčním prostředí. Toto je vzor, spoustu středně velké týmy používají:

![Struktura větve zdroje](source-control/_static/image1.png)

Hlavní větve vždy odpovídá kód, který je v produkčním prostředí. Větvemi hlavní odpovídají různých fázích životního cyklu vývoje. Vývoj větev je, kde implementují nové funkce. Pro malý tým může stačí master a vývoj, ale často doporučujeme, aby uživatelé měli pracovní větev mezi vývojem a hlavní. Můžete použít pracovní pro konečné integrační testování před aktualizace se přesune do produkčního prostředí.

Pro velké objemy týmy, které mohou být samostatných větvích pro každé nové vlastnosti; pro menší tým může mít každý vracet se změnami do větve vývoje.

Pokud máte větev pro každou funkci, pokud funkce A je připraven sloučení změny jeho zdrojového kódu až do vývoje větve a dolů do jiné větve funkcí. Tento zdrojový kód sloučení procesu může být časově náročné a pokud chcete vyhnout, které pracují přitom zachovat samostatné funkce, některé týmy implementovat alternativu volá *[přepínání funkcí](http://en.wikipedia.org/wiki/Feature_toggle)* (známou taky jako *příznaků funkcí*). To znamená, že veškerý kód pro všechny funkce je ve stejné pobočce, ale povolit nebo zakázat jednotlivých funkcí s použitím přepínače v kódu. Předpokládejme například, funkce A je nové pole pro úlohy aplikace Fix It a funkce B přidává funkce ukládání do mezipaměti. Kód pro obě funkce může být ve větvi vývoje, ale pouze zobrazení aplikace budou nové pole, když je proměnná nastavena na hodnotu true a použije jenom ukládání do mezipaměti při různých proměnná je nastavená na hodnotu true. Pokud není funkce A by se dala zvýšit, ale funkce B je připraven, můžete zvýšit úroveň veškerý kód do produkčního prostředí s přepínačem funkce A vypnout a zapnout funkci B. Potom můžete dokončit A funkce a zvyšte jeho úroveň později, všechny se nesloučí zdrojového kódu.

Jestli větve nebo přepíná používali pro funkce, větvení struktury tímto způsobem umožňuje toku kódu z vývojového do produkčního prostředí agile a opakovatelné způsobem.

Tato struktura také vám umožní rychle reagovat na zpětnou vazbu od zákazníků. Pokud potřebujete provést rychlou opravu do produkčního prostředí, můžete provést také, která efektivně pružně. Můžete vytvořit větev mimo hlavní nebo testovacího a až to bude hotové jeho sloučení do hlavní větve nahoru a dolů do větve vývoje a funkce.

![Větev hotfixu](source-control/_static/image2.png)

Bez větvení struktury tímto způsobem s jeho oddělení produkce a vývojových větvích by mohlo problém produkčního prostředí v pozici by bylo nutné podporovat nové funkce kódu společně s jste kód opravili správně produkčního prostředí můžete. Nový kód funkce nemusí být plně otestovaný a připravený pro produkční a možná budete muset udělat spoustu práce zálohování Zavádíme změny, které ještě nejsou připravené. Nebo může být nutné zpoždění jste kód opravili správně, pokud chcete otestovat změny a jejich přípravu pro nasazení.

Dále uvidíte příklady toho, jak implementovat tyto tři vzory v sadě Visual Studio, Azure a Visual Studio Online. Toto jsou příklady spíše než podrobné postupy-k-it pokyny; podrobné pokyny, které poskytují veškeré potřebné kontextu, najdete v článku [prostředky](#resources) části na konci kapitoly.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Přidat skripty do správy zdrojového kódu v sadě Visual Studio

Skripty můžete přidat do správy zdrojového kódu v sadě Visual Studio včetně ve složce řešení sady Visual Studio (za předpokladu, že váš projekt je ve správě zdrojového kódu). Tady je jeden způsob, jak to udělat.

Vytvořte složku pro skripty ve složce řešení (stejné složce, která má vaše *.sln* souboru).

![Složka služby Automation](source-control/_static/image3.png)

Zkopírujte soubory skriptů do složky.

![Obsah složky služby Automation](source-control/_static/image4.png)

V sadě Visual Studio přidejte do projektu složku řešení.

![Výběr nabídky novou složku řešení](source-control/_static/image5.png)

A přidejte soubory skriptů do složky řešení.

![Přidat existující položku nabídky Výběr](source-control/_static/image6.png)

![Přidat existující položku – dialogové okno](source-control/_static/image7.png)

Soubory skriptů jsou teď součástí vašeho projektu a správy zdrojového kódu se ke sledování jejich změny verze společně s odpovídající změny zdrojového kódu.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Store citlivá data v Azure

Pokud spustíte svou aplikaci na webu v Azure, je jeden způsob, jak zabránit ukládání přihlašovacích údajů ve správě zdrojového kódu k jejich uložení v Azure.

Například aplikace Fix It ukládá v jeho souboru Web.config souborů dva připojovací řetězce, které se mají hesla v produkčním prostředí a klíč, který poskytuje přístup k vašemu účtu úložiště Azure.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Pokud přesunete samotnou produkci hodnoty pro tato nastavení v vaše *Web.config* souboru, nebo pokud je vložíte do *Web.Release.config* souboru nakonfigurujte transformaci Web.config pro vložení během nasazení budete se ukládají v úložišti zdroje. Pokud zadáte databázové připojovací řetězce do produkce profil publikování, heslo bude v vaše *.pubxml* souboru. (Může vyloučit *.pubxml* souborů ze správy zdrojových kódů, ale pak ztratíte výhodu, že všechna ostatní nastavení nasazení pro sdílení obsahu.)

Azure nabízí alternativu pro **appSettings** a připojovací řetězce oddíly *Web.config* souboru. Tady je odpovídající část **konfigurace** karta pro webovou stránku v portálu správy Azure:

![appSettings a connectionStrings portálu](source-control/_static/image8.png)

Při nasazení projektu na tomto webu a spouštět aplikace přepsat libovolné hodnoty jsou uložené v Azure, libovolné hodnoty jsou v souboru Web.config.

Tyto hodnoty můžete nastavit v Azure pomocí portálu pro správu nebo skripty. Automatizační skript vytváření prostředí jste viděli v [automatizovat všechno](automate-everything.md) kapitoly vytvoří databázi SQL Azure, získá storage a připojovací řetězce SQL Database a ukládá těchto tajných kódů v nastavení pro webový server.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Všimněte si, že tyto skripty jsou parametrizovány tak, aby skutečnými hodnotami nechcete získat trvale uložena do úložiště zdrojového kódu.

Při spuštění místně ve vašem vývojovém prostředí aplikace načte váš místní soubor Web.config a připojení body řetězec k databázi systému SQL Server LocalDB ve *aplikace\_Data* složce webového projektu. Při spuštění aplikace v Azure a aplikace se pokusí načíst tyto hodnoty ze souboru Web.config, získá zpět a používá jsou hodnoty uložené pro webový server není co je skutečně v souboru Web.config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>Pomocí Gitu ve Visual Studio a Visual Studio Online

Všechny zdrojové prostředí pro ovládací prvek můžete implementovat DevOps větvení struktury, které jsou uvedené výše. Distribuovaných týmů [distribuovaný systém řízení verze](http://en.wikipedia.org/wiki/Distributed_revision_control) (systém DVCS) může být nejvhodnější; pro jiné týmy [centralizované systému](http://en.wikipedia.org/wiki/Revision_control) může být vhodnější.

[Git](http://git-scm.com/) je DVCS, který se má stát velmi populární. Pokud používáte Git pro správu zdrojového kódu, máte úplnou kopii úložiště s celou její historií v místním počítači. Mnoho lidí dáváte přednost, protože je to snazší pokračovat v práci, když nejste připojení k síti – můžou dál vykonávat potvrzení změn a vrácení zpět, vytvářet a přepnutí větví a tak dále. I v případě, že jste připojeni k síti, je jednodušší a rychlejší vytváření větví a přepínání větví všechno, co je místní. Můžete také provést místních potvrzení změn, vrácení zpět bez nutnosti vliv na ostatní vývojáři. A může hromadně potvrzení před odesláním do serveru.

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO), dříve známé jako Team Foundation Service, nabízí i Git a [Team Foundation Version Control](https://msdn.microsoft.com/library/ms181237(v=vs.120).aspx) (TFVC; centralizované správy zdrojového kódu). Tady v Microsoftu v Azure group některé týmy používají centralizovanou správu zdrojového kódu, používat distribuována, a některé používá kombinaci (centralizovaná u některých projektů a distribuován pro jiné projekty). Služby VSO je zdarma až pro 5 uživatelů. Si můžete zaregistrovat bezplatný plán [tady](https://go.microsoft.com/fwlink/?LinkId=307137).

Visual Studio 2013 zahrnuje prvotřídní integrované [podporu úložiště Git](https://msdn.microsoft.com/library/hh850437.aspx); zde je rychlý ukázku toho, jak to funguje.

S projektem otevřeným v sadě Visual Studio 2013, klikněte pravým tlačítkem na řešení v **Průzkumníka řešení**a zvolte **přidat řešení do správy zdrojových kódů**.

![Přidat řešení do správy zdrojového kódu](source-control/_static/image9.png)

Visual Studio výzvu, pokud chcete používat TFVC (centralizované správy verzí) nebo Git.

![Zvolte možnost správy zdrojového kódu](source-control/_static/image10.png)

Když vyberete Git a klikněte na tlačítko **OK**, sada Visual Studio vytvoří nové místní úložiště Git ve složce řešení. Nové úložiště ještě; neobsahuje žádné soubory je nutné je přidat do úložiště Git commit způsobem. Klikněte pravým tlačítkem na řešení v **Průzkumníka řešení**a potom klikněte na tlačítko **potvrzení**.

![Potvrzení změn](source-control/_static/image11.png)

Visual Studio automaticky zpracuje všechny soubory projektu pro potvrzení a uvádí jejich v **Team Exploreru** v **zahrnuté změny** podokně. (Pokud byly některé nechcete zahrnout do potvrzení změn, můžete vybrat, klikněte pravým tlačítkem a klikněte na tlačítko **vyloučit**.)

![Team Explorer](source-control/_static/image12.png)

Zadejte komentář potvrzení a klikněte na tlačítko **potvrzení**, a sady Visual Studio spustí potvrzení a zobrazí ID potvrzení změn.

![Team Explorer změny](source-control/_static/image13.png)

Teď Pokud nějaký kód změnit tak, aby se liší od co je v úložišti, můžete snadno zobrazit rozdíly. Klikněte pravým tlačítkem na soubor, který jste změnili, vyberte **porovnat s Unmodified**, zobrazí se zobrazení porovnání, která zobrazuje nepotvrzené změny.

![Porovnat s verzí bez úprav](source-control/_static/image14.png)

![Změny zobrazení rozdílu](source-control/_static/image15.png)

Můžete snadno zobrazit změny teď a vrátit je se změnami.

Předpokládejme, že budete muset udělat větev – můžete tak učinit v sadě Visual Studio příliš. V **Team Exploreru**, klikněte na tlačítko **novou větev**.

![Team Explorer novou větev](source-control/_static/image16.png)

Zadejte název větve, klikněte na tlačítko **vytvořit větev**, a pokud jste vybrali **rezervovat větev**, Visual Studio automaticky rezervuje novou větev.

![Team Explorer novou větev](source-control/_static/image17.png)

Teď můžete provádět změny souborů a vrácení se změnami do této větve. A můžete snadno přepínat mezi větve a sady Visual Studio automaticky synchronizuje soubory na větvi, které byly rezervovány. V tomto příkladu nadpisu webové stránky v  *\_Layout.cshtml* byl změněn na "Opravy hotfix 1" v HotFix1 větve.

![Hotfix1 větve](source-control/_static/image18.png)

Pokud přepnete zpět do hlavní větve, obsah  *\_Layout.cshtml* soubor automaticky obnovit jsou v hlavní větvi.

![Hlavní větev](source-control/_static/image19.png)

Tento jednoduchý příklad toho, jak můžete rychle vytvořit větev a překlopit vpřed a zpět mezi větvemi. Tato funkce umožňuje vysoce agilní pracovní postup pomocí strukturu větví a skripty pro automatizaci uvedené v [automatizovat všechno](automate-everything.md) kapitoly. Například může být práci ve větvi vývoj, vytvořte větev opravy hotfix z hlavní, přepnout na novou větev, proveďte požadované změny existuje a potvrdíte je a pak přepněte zpět do větve vývoje a pokračovat, co jste dělali.

Co už víte, zde je způsob práce s místním úložištěm Git v sadě Visual Studio. V prostředí team obvykle také vložíte změny běžné úložiště. Nástroje sady Visual Studio také umožňují tak, aby odkazoval do vzdáleného úložiště Git. K tomuto účelu můžete použít webu GitHub.com, nebo můžete použít [Git ve Visual Studiu Online](https://msdn.microsoft.com/library/hh850437.aspx) integrovaná všechny ostatní sady Visual Studio Online funkce jako je například pracovní položky a sledování chyb.

Tato akce není jediným způsobem je možné implementovat agilní strategie větvení, samozřejmě. Můžete povolit stejného agilní pracovního postupu pomocí úložiště centralizovanou správu zdrojového ovládacího prvku.

## <a name="summary"></a>Souhrn

Měřit její úspěšnost systému správy zdrojů na základě toho, jak rychle můžete provést změnu a získat za bezpečné a předvídatelným způsobem. Pokud se děsili toho k provedení změny, protože je nutné provést jeden nebo dva ručního testování v něm, můžete pokládat sami co musíte udělat process-wise nebo test-wise tak, že můžete provést tuto změnu v minutách, nebo na nejhorší ne déle než hodinu. Jednou z možných strategií pro způsobem, který je implementace průběžné integrace a průběžné doručování, což si probereme [další kapitolu](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Prostředky

[Visual Studio Online](https://www.visualstudio.com/) portál poskytuje dokumentace a podpora služby a můžete se zaregistrovat pro účet. Pokud máte sadu Visual Studio 2012 a chcete použít Git, přečtěte si téma [Visual Studio Tools for Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c).

Další informace o TFVC (centralizované správy verzí) a Git (distribuovanou správu verzí) naleznete na následujících odkazech:

- [Který systém správy verzí mám použít: TFVC nebo Git?](https://msdn.microsoft.com/library/vstudio/ms181368.aspx#tfvc_or_git_summary) Dokumentace MSDN obsahuje tabulku se souhrnem rozdílech mezi TFVC a Git.
- [Dobře chci Team Foundation Server a jsem, jako je Git, ale které je lepší?](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Porovnání Git a TFVC.

Další informace o strategie větvení naleznete na následujících odkazech:

- [Vytváření procesních toků pro verzi serveru Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Dokumentace ke službě Microsoft Patterns and Practices. Viz kapitola 6 diskuzi o strategie větvení. Funkce pomocníků přepíná přes větve funkcí, pokud větve pro funkce se používají, nejzávažnějších a jejich krátkodobou (hodiny nebo i dny maximálně).
- [Verze ovládacího prvku průvodce](https://aka.ms/vsarsolutions). Příručka k strategie větvení pomocí ALM Rangers. Na kartě soubory ke stažení najdete v článku Strategies.pdf větvení.
- [Vývoj softwaru s funkcí přepíná](https://msdn.microsoft.com/magazine/dn683796.aspx). Článek v časopise MSDN Magazine.
- [Funkce přepínání](http://martinfowler.com/bliki/FeatureToggle.html). Úvod k funkci přepíná / na blog Martina Fowlera příznaky funkcí.
- [Funkce přepíná vs větve funkcí](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Jiné blogový příspěvek o funkci přepínačů, podle Dylan Smith.

Další informace o tom, jak zpracovat citlivé informace, které by neměly být udržovány v úložišť správy zdrojového kódu naleznete na následujících odkazech:

- [Osvědčené postupy pro nasazení hesel a dalších citlivých dat do ASP.NET a službě Azure App Service](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Model weby Azure: Jak řetězců aplikace a připojení fungují řetězce](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Vysvětluje funkci Azure, která přepíše `appSettings` a `connectionStrings` data *Web.config* souboru.
- [Vlastní nastavení konfigurace a aplikace na webech Azure – s Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Informace o dalších metodách pro uchování citlivých informací ze správy zdrojového kódu, naleznete v tématu [ASP.NET MVC: zachovat privátní nastavení ze správy zdrojového kódu](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Předchozí](automate-everything.md)
> [další](continuous-integration-and-continuous-delivery.md)
