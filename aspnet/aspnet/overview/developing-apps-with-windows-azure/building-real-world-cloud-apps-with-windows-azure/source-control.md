---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: "Zdroj ovládacího prvku (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs"
author: MikeWasson
description: "Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/23/2015
ms.topic: article
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: e3ce68b949199db35c18a09771d99d38562b74e9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Správa zdrojového kódu (vytváření reálných cloudových aplikací s Azure)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud. Informace o elektronická kniha najdete v tématu [první kapitoly](introduction.md).


Správa zdrojového kódu je nezbytné pro všechny projekty vývoje cloudu, nejen team prostředí. Domníváte nebude úprav zdrojový kód nebo i dokument aplikace Word bez funkci zpět a automatické zálohování a Správa zdrojového kódu nabízí tyto funkce na úrovni projektu, kde můžete uložit i další čas, kdy dojde k chybě. Cloudové služby správy zdrojového kódu již nemusíte si dělat starosti o složité nastavení a můžete použít Visual Studio Online zdrojového kódu zdarma až 5 uživatelů.

První část této kapitoly vysvětluje tři klíčové osvědčené postupy pro mějte na paměti:

- [Skripty pro automatizaci považovat za zdrojový kód](#scripts) a verze je spolu s kódu aplikace.
- [Nikdy se změnami tajné klíče](#secrets) (citlivých dat jako pověření) do úložiště zdrojového kódu.
- [Nastavit zdroj větví](#devops) povolit DevOps pracovního postupu.

Zbývající část kapitoly dává některé ukázkové implementace tyto vzory v sadě Visual Studio, Azure a Visual Studio Online:

- [Přidat skripty ke správě zdrojového kódu v sadě Visual Studio](#vsscripts)
- [Ukládat citlivá data v Azure](#appsettings)
- [Pomocí Git v sadě Visual Studio a Visual Studio Online](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Skripty pro automatizaci považovat za zdrojového kódu

Při práci na projektu cloudové často měníte věcí a chcete mít možnost rychle reagovat na problémech ohlášených zákazníky. Rychle reagovat, která využívá skripty pro automatizaci, jak je popsáno v [automatizovat všechno](automate-everything.md) kapitoly. Všechny skripty, které můžete použít k vytvoření prostředí, nasaďte do něj škálování ho, atd., musí být synchronizované se zdrojovým kódem vaší aplikace.

Aby skripty synchronizovaná s kódem, uložte je do systému správy zdrojů. Pak pokud byste někdy potřebovali chcete vrátit zpět změny nebo provést rychlou opravu produkčním kódu, který je odlišný od kódu vývoj, nemáte ztrácet čas pokusu sledovat nastavení, které se změnily nebo které členové týmu mít kopie verze, které potřebujete. Jste zajištěná, které jsou synchronizované s základu kódu, který je pro potřebujete, a jste zajištěná, že všichni členové týmu funguje stejné skripty, skripty, které potřebujete. Jestli potřebujete automatizovat testování a nasazování oprava výrobní nebo vývoj nových funkcí, pak budete mít správné skript pro kód, který je třeba aktualizovat.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Nekontrolovat v tajných klíčů

Úložiště zdrojového kódu je obvykle přístupné pro příliš mnoho uživatelů pro něj být odpovídajícím způsobem bezpečné místo pro citlivá data, jako jsou hesla. Pokud se skripty spoléhají na tajné klíče, jako jsou hesla, Parametrizace tato nastavení tak, že nemáte ukládány ve zdrojovém kódu a uložit vaše tajné klíče někde jinde.

Například se Azure umožňuje stáhnout soubory, které obsahují nastavení publikování k automatizaci tvorby profilů publikování. Tyto soubory zahrnují uživatelská jména a hesla, které mají oprávnění ke správě služeb Azure. Pokud použijete tuto metodu pro vytvoření publikovat profily, a pokud se tyto soubory do správy zdrojového kódu se změnami, kdo má přístup do úložiště můžete zobrazit těchto uživatelských jmen a hesel. Je možné bezpečně uložit heslo v samotném profilu publikování protože je šifrovaný a je v *. pubxml.user* soubor, který ve výchozím nastavení není součástí zdrojového kódu.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Struktura zdroje větví usnadňuje DevOps pracovního postupu

Jak implementovat větve v úložišti se týká také možnosti k vývoji nových funkcí a opravte problémy v produkčním prostředí. Tady je vzor, že mnoho střední velikosti týmy použití:

![Struktura zdroje firemní pobočky](source-control/_static/image1.png)

Hlavní větve vždy odpovídá kód, který je v produkčním prostředí. Větvemi hlavní odpovídají různých fázích životního cyklu vývoj. Vývoj větve je, kde budete implementovat nové funkce. Pro malý tým právě máte hlavní a vývoj, ale často doporučujeme, aby uživatelé měli pracovní větev mezi vývojovým týmem a hlavní server. Můžete vytvořit pracovní pro konečné integrace testování předtím, než aktualizace se přesune do produkčního prostředí.

Pro velký týmy může být samostatné větve pro každou novou funkci; pro menší tým může mít každý přihlašuje na větev vývoj.

Pokud máte větev pro jednotlivé funkce, když funkce A je připravený sloučení změny jeho zdrojového kódu až do vývoj větev a dolů na další funkce větve. Tento zdrojový kód slučování proces může být časově náročná a nechcete svou práci při zachování funkce samostatné, některé týmy používají jinou názvem  *[funkci přepínačů](http://en.wikipedia.org/wiki/Feature_toggle)*  (označované taky jako *funkci příznaky*). To znamená, že všechny kód pro všechny funkce je ve stejné pobočce, ale povolit nebo zakázat jednotlivých funkcí s použitím přepínače v kódu. Předpokládejme například, funkce A je nové pole pro úlohy aplikace opravte ji a funkce B přidává funkce ukládání do mezipaměti. Kód pro obě funkce může být v pobočce vývoj, ale pouze zobrazení bude aplikace pole nové proměnné je nastavena na hodnotu true a bude používat jenom ukládání do mezipaměti při různých proměnná je nastavená na hodnotu true. Pokud funkci A není připraven chcete zvýšit, ale funkce B je připraveno, můžete zvýšit úroveň všech kódu do produkčního prostředí s přepínač funkcí A vypnout a B funkce přepínač na. Potom můžete dokončit A funkce a povyšte ho později, všechny s slučování žádný zdrojový kód.

Jestli používáte větví nebo přepínačů pro funkce, větvení struktura jako to umožňuje toku kódu z vývojového do provozního agilní a opakovatelných způsobem.

Tato struktura také umožňuje rychle reagovat na připomínky zákazníků. Pokud potřebujete provést rychlou opravu do produkčního prostředí, můžete provést také která efektivně agilní způsobem. Můžete vytvořit větev z hlavní nebo pracovní a až to bude hotové sloučení až do hlavní a dolů na vývoj a funkce větve.

![Oprava hotfix větve](source-control/_static/image2.png)

Bez větvení struktura takto s jeho oddělení produkce a vývoj větví by mohlo provozní problém v pozici museli povýšíte nový kód funkce společně s vaše produkční opravte je. Nový kód funkce nemusí být plně otestované a připravena k produkční a možná budete muset udělat velké množství pracovních zálohování na změny, které nejsou připravené. Nebo možná budete muset zpoždění vaší opravu, aby bylo možné testovat změny a jejich přípravu k nasazení.

Zobrazí se další příklady, jak implementovat tyto tři vzory v sadě Visual Studio, Azure a Visual Studio Online. Toto jsou příklady spíše než podrobné pokyny krok za krokem how-k-proveďte it; podrobné pokyny, které poskytují veškeré potřebné kontextu, najdete v článku [prostředky](#resources) části na konci kapitoly.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Přidat skripty ke správě zdrojového kódu v sadě Visual Studio

Ke správě zdrojového kódu v sadě Visual Studio můžete přidat skripty, včetně jejich ve složce řešení sady Visual Studio (za předpokladu, že je váš projekt ve správě zdrojového kódu). Zde je možné ho provést.

Vytvořte složku pro skripty ve složce řešení (stejné složce, která má vaše *.sln* souboru).

![Automatizace složky](source-control/_static/image3.png)

Zkopírujte soubory skriptů do složky.

![Obsah složky automatizace](source-control/_static/image4.png)

V sadě Visual Studio přidejte do projektu složku řešení.

![Výběr nového řešení složky nabídky](source-control/_static/image5.png)

A přidejte soubory skriptu ke složce řešení.

![Přidat existující položku nabídky Výběr](source-control/_static/image6.png)

![Přidat existující položku – dialogové okno](source-control/_static/image7.png)

Soubory skriptů jsou nyní zahrnuty ve vašem projektu a Správa zdrojového kódu je sledování změn jejich verze spolu s odpovídající změny zdrojového kódu.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Ukládat citlivá data v Azure

Pokud spustíte aplikaci webu v Azure, jeden způsob, jak zabránit ukládání přihlašovacích údajů ve správě zdrojového kódu je uložit je do Azure.

Například aplikace opravte ukládá v jeho Web.config soubor dva připojovací řetězce, které budou mít hesla v produkční a klíč, který poskytuje přístup k účtu úložiště Azure.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Pokud vložíte skutečné produkční hodnoty těchto nastavení v vaše *Web.config* souboru, nebo pokud jste zadali v *Web.Release.config* soubor k transformaci Web.config vložení je během nasazení, konfigurace budete být uložen v úložišti zdrojů. Pokud zadáte databázové připojovací řetězce do provozní profil publikování, heslo bude v vaše *.pubxml* souboru. (Může vyloučit *.pubxml* souboru od správy zdrojového kódu, ale pak ztratíte Výhodou sdílení všechna ostatní nastavení nasazení.)

Azure vám dává alternativu pro **appSettings** a připojovací řetězce části *Web.config* souboru. Tady je příslušné části **konfigurace** kartu pro webovou stránku v portálu správy Azure:

![appSettings a connectionStrings portálu](source-control/_static/image8.png)

Při nasazení projektu na tento web a spouštět aplikace, ať hodnoty jsou uloženy v Azure přepsat, ať hodnoty jsou v souboru Web.config.

Tyto hodnoty lze nastavit v Azure pomocí portálu pro správu nebo skripty. V prostředí vytvoření skriptu pro automatizaci jste viděli v [automatizovat všechno](automate-everything.md) kapitoly vytvoří Azure SQL Database, získá úložiště a připojovací řetězce SQL Database a ukládá těchto tajných klíčů v nastavení pro svůj web.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Všimněte si, že tyto skripty jsou parametry tak, aby skutečnými hodnotami nemáte získat formou zdrojové úložiště.

Při spuštění místně ve vašem vývojovém prostředí, aplikace načte místního souboru Web.config a připojení body řetězec k databázi serveru SQL LocalDB v *aplikace\_Data* složky webového projektu. Při spuštění aplikace v Azure a aplikace se pokusí načíst tyto hodnoty ze souboru Web.config, získá zpět a používá jsou hodnoty uložené pro webový server, není co je skutečně v souboru Web.config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>Pomocí Git v sadě Visual Studio a Visual Studio Online

Všechny zdrojové prostředí pro řízení můžete použít k implementaci větvení struktura DevOps uvedené výše. Pro distribuované týmy [distribuované systém správy verzí](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) může být nejvhodnější; pro ostatními týmy [centralizované systému](http://en.wikipedia.org/wiki/Revision_control) může lépe pracovat.

[Git](http://git-scm.com/) je DVCS, který se stane velmi oblíbenou. Při použití Git pro řízení zdrojů, máte úplnou kopii úložiště se všemi jeho historie v místním počítači. Spousta lidí přednost tomu, že vzhledem k tomu je snazší pokračovat v práci, když nejsou připojené k síti – můžete nadále se potvrdí a odvolání, vytvářet a přepnutí větví a tak dále. I když jste připojení k síti, je jednodušší a rychlejší vytvoření větví a přepnutí větví, když všechno, co je místní. Také můžete provést místní potvrzení a odvolání bez nutnosti vliv na ostatní vývojáři. A můžete dávky potvrzení před jejich odesláním na server.

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO), dříve se označuje jako Team Foundation Service, nabízí i Git a [správy verzí Team Foundation](https://msdn.microsoft.com/library/ms181237(v=vs.120).aspx) (TFVC; centralizované správy zdrojového kódu). Zde ve společnosti Microsoft ve skupině Azure pomocí některé týmy centralizované zdrojového kódu, některé použití distribuována, a některé použít kombinaci (centralizované pro některé projekty a distribuován pro další projekty). Službu VSO je zdarma pro uživatele, až 5. Můžete si zaregistrovat pro plánu free [zde](https://go.microsoft.com/fwlink/?LinkId=307137).

Visual Studio 2013 zahrnuje integrovanou prvotřídní [podporu Git](https://msdn.microsoft.com/library/hh850437.aspx); tady je rychlý ukázku jak to funguje.

S projektem otevřeným v nástroji Visual Studio 2013, klikněte pravým tlačítkem na řešení v **Průzkumníku řešení**a zvolte **přidat řešení správy zdrojového kódu**.

![Přidat řešení do správy zdrojového kódu](source-control/_static/image9.png)

Visual Studio zobrazí, pokud chcete použít TFVC (centralizované správy verzí) nebo Git.

![Zvolte zdrojového kódu](source-control/_static/image10.png)

Když vyberete Git a klikněte na tlačítko **OK**, Visual Studio vytvoří nové místní úložiště Git ve složce řešení. Nové úložiště nejsou uloženy žádné soubory ještě; je nutné je přidat do úložiště pomocí tohoto postupu potvrzení Git. Pravým tlačítkem na řešení v **Průzkumníku řešení**a potom klikněte na **potvrdit**.

![Potvrzení](source-control/_static/image11.png)

Visual Studio automaticky zpracuje všechny soubory projektu pro potvrzení a zobrazí je v **Team Explorer** v **změny zahrnuté** podokně. (Pokud byly některé nebyla chcete zahrnout do potvrzení, můžete třeba vybrat, klikněte pravým tlačítkem a klikněte na tlačítko **vyloučit**.)

![Team Explorer](source-control/_static/image12.png)

Zadejte komentář potvrzení a klikněte na **potvrzení**, a Visual Studio provádí potvrzení a zobrazí ID potvrzení.

![Team Explorer změny](source-control/_static/image13.png)

Teď Pokud změníte nějaký kód tak, aby se liší od co je v úložišti, můžete snadno zobrazit rozdíly. Klikněte pravým tlačítkem na soubor, který jste změnili, vyberte **porovnání s Unmodified**, a získat porovnání zobrazení, který ukazuje nepotvrzené změny.

![Porovnání s beze změny](source-control/_static/image14.png)

![Diff zobrazuje změny](source-control/_static/image15.png)

Zobrazí se snadno jaké změny prováděné a vrátit se změnami.

Předpokládejme, že budete muset udělat větev – můžete to udělat v sadě Visual Studio příliš. V **Team Explorer**, klikněte na tlačítko **nové větve**.

![Nové větve Team Explorer](source-control/_static/image16.png)

Zadejte název větve, klikněte na tlačítko **vytvoření větve**, a pokud jste vybrali **větve najdete v článku věnovaném**, Visual Studio automaticky rezervuje nové větve.

![Nové větve Team Explorer](source-control/_static/image17.png)

Teď můžete provádět změny souborů a jejich změnami na tuto větev. A můžete snadno přepínat mezi pobočkami a Visual Studio automaticky synchronizace, který si vyhradili soubory podle toho, co jste větev. V tomto příkladu title webové stránky v  *\_Layout.cshtml* byla změněna na "Oprava hotfix 1" v HotFix1 větev.

![Hotfix1 větve](source-control/_static/image18.png)

Pokud přepnete zpět do hlavní pobočky, obsah  *\_Layout.cshtml* souboru automaticky obnovit jsou v hlavní pobočce.

![Hlavní větve](source-control/_static/image19.png)

Tento jednoduchý příklad jak můžete rychle vytvořit větev a překlopit přepínat mezi větve. Tato funkce umožňuje vysoce agilní pracovní postup pomocí strukturu větve a skripty pro automatizaci zobrazovat v [automatizovat všechno](automate-everything.md) kapitoly. Například může být práce v pobočce vývoj, vytvořte větev oprava hotfix z hlavní, přejděte do nové větve, provedené změny existuje a potvrdit a pak přejděte zpátky do větve vývoj a pokračovat, co jste dělali.

Co jste se seznámili s tady je způsob práce s místní úložiště Git v sadě Visual Studio. V prostředí team je obvykle také doručte změny do běžné úložiště. Nástroje sady Visual Studio také umožňují bodu do vzdáleného úložiště Git. K tomuto účelu můžete použít webu GitHub.com, nebo můžete použít [Git v prostředí Visual Studio Online](https://msdn.microsoft.com/library/hh850437.aspx) integrovat všechny ostatní sady Visual Studio Online funkce jako je například pracovní položkou a sledování chyb.

Tato akce není jediným způsobem, jak můžete implementovat agilní větvení strategie, samozřejmě. Můžete povolit stejné agilní pracovní postup pomocí úložiště správy centralizované zdrojů.

## <a name="summary"></a>Souhrn

Měřit její úspěšnost systému správy zdrojů na základě jak rychle provést změnu a získat tak bezpečné a předvídatelné za provozu. Pokud se přistihnete Vystrašené provedení změny, protože je nutné provést jeden nebo dva ruční testování v něm, vám může položte otázku, co musíte udělat process-wise nebo test-wise tak, aby tato změna může provést, v minutách, nebo na nejhorší už než hodinu. Jednou z možných strategií pro tento krok je implementace průběžnou integraci a nastavené průběžné doručování, který vám nabídneme [další kapitoly](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Prostředky

[Visual Studio Online](https://www.visualstudio.com/) portál poskytuje dokumentace a podpora služby, a můžete si zaregistrovat účet. Pokud máte Visual Studio 2012 a chcete použít Git, přečtěte si téma [Visual Studio Tools for Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c).

Další informace o TFVC (centralizované správy verzí) a Git (distribuované verzí) najdete v následujících zdrojích informací:

- [Které systém správy verzí použít: TFVC nebo Git?](https://msdn.microsoft.com/library/vstudio/ms181368.aspx#tfvc_or_git_summary) Dokumentace MSDN, obsahuje tabulku shrnutí rozdíly mezi TFVC a Git.
- [Dobře je dobré Team Foundation Server a je dobré Git, ale který je lepší?](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Porovnání Git a TFVC.

Další informace o větvení strategie najdete v následujících zdrojích informací:

- [Vytvoření kanálu verze sadou Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Dokumentace k Microsoft Patterns and Practices. V kapitole 6 diskuzi o větvení strategie. Funkce obranou přepíná prostřednictvím funkce větve a pokud se používají větve pro funkce, obhajuje zachování jejich krátkodobou (hodin nebo dnů maximálně).
- [Průvodce řízení verze](https://aka.ms/vsarsolutions). Průvodce větvení strategie podle ALM Rangers. Na kartě soubory ke stažení najdete v části Vytvoření větve Strategies.pdf.
- [Vývoj softwaru pomocí funkce přepínačů](https://msdn.microsoft.com/magazine/dn683796.aspx). Článek v časopise MSDN.
- [Funkce přepínání](http://martinfowler.com/bliki/FeatureToggle.html). Úvod k funkci přepíná / funkce flags na blogu Martin Fowler.
- [Funkce přepínačů vs funkce větví](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Jiné příspěvku na blogu o funkce přepínačů, podle Dylan Smith.

Další informace o způsobu zpracování citlivé informace, které by neměl být zachovány v zdrojová ovládací prvek úložiště najdete v následujících zdrojích informací:

- [Osvědčené postupy pro nasazování hesel a dalších citlivých dat do ASP.NET a službě Azure App Service](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Weby Azure: Jak řetězců aplikace a připojovací řetězce pracovní](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Vysvětluje funkci Azure, který přepíše `appSettings` a `connectionStrings` data *Web.config* souboru.
- [Vlastní nastavení konfigurace a aplikací v Azure weby - Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Informace o dalších metodách pro zachování citlivých informací mimo zdrojového kódu najdete v tématu [ASP.NET MVC: zachovat privátní nastavení ze zdrojového kódu](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

>[!div class="step-by-step"]
[Předchozí](automate-everything.md)
[další](continuous-integration-and-continuous-delivery.md)
