---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
title: "Určení soubory musí být nasazené (VB) | Microsoft Docs"
author: rick-anderson
description: "Soubory musí být nasazeny z vývojového prostředí do produkčního prostředí závisí částečně na tom, zda technologie ASP.NET byla vytvořena nám..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: ea918f62-c9d6-4a7f-9bc6-e054d3764b2c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
msc.type: authoredcontent
ms.openlocfilehash: 44349b09fdc0de8ad6bd241a4c158d6a198e5d01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="determining-what-files-need-to-be-deployed-vb"></a>Určení soubory musí být nasazené (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_VB.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_vb.pdf)

> Soubory musí být nasazeny z vývojového prostředí do produkčního prostředí závisí částečně na tom, zda technologie ASP.NET byla vytvořena pomocí webu Model nebo Model webových aplikací. Další informace o těchto dvou projektu modelů a jak ovlivňuje projektu modelu nasazení.


## <a name="introduction"></a>Úvod

Nasazení webové aplikace ASP.NET zahrnuje kopírování souborů souvisejících s ASP.NET z vývojového prostředí do produkčního prostředí. Soubory související s ASP.NET zahrnují značek webové stránky ASP.NET a kód a klientské a serverové podpůrných souborů. Podporu na straně klienta jsou soubory odkazuje webové stránky a odeslat přímo do prohlížeče - bitové kopie, soubory šablon stylů CSS a JavaScript soubory, například. Soubory podporu na straně serveru zahrnují ty, které se používají ke zpracování požadavku na straně serveru. To zahrnuje konfigurační soubory, webové služby, soubory třídy, typové datové sady a LINQ souborů SQL, mimo jiné.

Obecně platí by měl být kopírovány všechny soubory podporu na straně klienta z vývojového prostředí do produkčního prostředí, ale kopírovány jaké soubory podporu na straně serveru závisí na tom, jestli jsou explicitně kompilování kódu na straně serveru do sestavení ( `.dll` soubor) nebo pokud máte tyto sestavení automaticky generovaný. V tomto kurzu označuje, co soubory musí být nasazeny, pokud explicitně kompilace kódu do sestavení versus s Tento krok kompilace proběhnou automaticky.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Explicitní kompilace Versus automatickou kompilaci

Rozhraní ASP.NET web pages se dál dělí na deklarativní značkami a zdrojový kód. Obsahuje části deklarativní značky HTML, webové ovládací prvky a syntaxe datové vazby; části kódu obsahuje obslužné rutiny událostí, které jsou napsané v kódu Visual Basic a C#. Části značek a kódu jsou obvykle rozdělené do různých souborů: `WebPage.aspx` obsahuje deklarativní při `WebPage.aspx.vb` ve uložený kód.

Vezměte v úvahu stránku ASP.NET s názvem `Clock.aspx` obsahující ovládací prvek popisek, jehož vlastnost textu je nastavena na aktuální datum a čas při načtení stránky. Deklarativní část (v `Clock.aspx`) by obsahovat značky pro ovládací prvek popisek Web - `<asp:Label runat="server" id="TimeLabel" />` – při části kódu (v `Clock.aspx.vb`) by měla mít `Page_Load` rutinu události s následujícím kódem:

[!code-vb[Main](determining-what-files-need-to-be-deployed-vb/samples/sample1.vb)]

Aby modul ASP.NET pro zpracování požadavku pro tuto stránku, část kódu stránky (  *`WebPage`*  `.aspx.vb` souboru) musí být nejprve kompilovány. Tato kompilace může dojít, explicitně nebo automaticky.

Pokud kompilace se stane explicitně pak bude celá aplikace zdrojový kód je zkompilovat do jednoho nebo více sestavení (`.dll` soubory) nachází v aplikačním `Bin` adresáře. Pokud kompilace se stane automaticky pak výsledná automaticky generovaný je sestavení, ve výchozím nastavení, umístit do `Temporary ASP.NET Files` složky, kterou najdete na `%WINDOWS%\Microsoft.NET\Framework\<version>`, i když toto umístění je lze nakonfigurovat [ &lt; kompilace&gt; element](https://msdn.microsoft.com/en-us/library/s10awwz0.aspx) v `Web.config`. Explicitní kompilace je nutné provést některé akce pro kompilaci kódu aplikace ASP.NET na sestavení, a tento krok dojde před jejich nasazením. S automatickou kompilaci proces kompilace dojde na webovém serveru při prvním přístupu k prostředku.

Bez ohledu na to, jaký model kompilace použijete, část značek všech stránek ASP.NET ( `WebPage.aspx` souborů) je nutné zkopírovat do produkčního prostředí. S explicitní kompilace je nutné zkopírovat do sestavení v `Bin` složka, ale není potřeba kopírovat do části kódu na stránkách ASP.NET ( `WebPage.aspx.vb` soubory). S automatickou kompilaci musíte zkopírovat soubory část kódu, aby kód je k dispozici a mohou být zkompilovány automaticky, když je navštívené stránky. Obsahuje části značek každé webové stránky ASP.NET `@Page` direktivy s atributy, které označuje, zda byl již explicitně kompilovat přidružené kódu stránky, nebo jestli vyžaduje sestavují automaticky. V důsledku toho provozním prostředí můžete bezproblémově pracovat se buď kompilace modelu a není potřeba použít speciální nastavení indikující, že se používá explicitní nebo automatické kompilace.

Tabulka 1 shrnuje různé soubory k nasazení při použití explicitní kompilace versus automatickou kompilaci. Bez ohledu na to kompilace modelu použity jste měli vždycky nasazovat sestavení v `Bin` složky, pokud existuje této složky. `Bin` Složka obsahuje sestavení, specifické pro webové aplikace, mezi které patří kompilované zdrojový kód při použití modelu explicitní kompilace. `Bin` Adresář také obsahuje sestavení z jiných projektů a open source nebo třetích stran sestavení, která se může použít, a to musí být na provozním serveru. Vzhledem k tomu obecné pravidlo, zkopírujte `Bin` složky až produkční při nasazování. (Pokud používáte automatickou kompilaci modelu a nepoužívají žádné externí sestavení, pak nebudete mít `Bin` adresář -, který je v pořádku!)

| **Model kompilace** | **Nasadit soubor část kód?** | **Nasazení souboru zdrojového kódu?** | **Nasazení sestavení v `Bin` adresáře?** |
| --- | --- | --- | --- |
| Explicitní kompilace | Ano | Ne | Ano |
| Automatickou kompilaci | Ano | Ano | Ano (pokud existuje) |

**Tabulka 1: Nasazení co soubory závisí na modelu kompilace použít.**

## <a name="taking-a-trip-down-memory-lane"></a>Pořízení výlet dolů dráhy paměti

Jaký způsob kompilace se používá závisí, částečně na tom, jak je spravován aplikace ASP.NET v sadě Visual Studio. Od. NET na zahájení v roce 2000 byly čtyři různé verze sady Visual Studio – Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 a Visual Studio 2008. Visual Studio .NET 2002 a 2003 spravované aplikace ASP.NET pomocí *projekt webové aplikace* modelu. Klíčové funkce modelu projektu webové aplikace jsou:

- Soubory, že způsob vytvoření projektu jsou definovány v jediného souboru projektu. Všechny soubory, není definován v souboru projektu nejsou považovány za součást webové aplikace Visual Studio.
- Používá explicitní kompilace. Sestavení projektu zkompiluje soubory kódu v projektu do jednoho sestavení, který je umístěn v `Bin` složky.

Společnost Microsoft vydala Visual Studio 2005 vyřadit podporu pro model projektu webové aplikace a nahradit s modelem webový projekt. Webový projekt modelu rozlišené sám sebe z *projekt webové aplikace* modelu následujícími způsoby:

- Místo jediného souboru projektu vidět soubory projektu, se místo toho používá systém souborů. Stručně řečeno všechny soubory ve složce webové aplikace (nebo podsložky) jsou považovány za součást projektu.
- Sestavení projektu v sadě Visual Studio nevytváří sestavení v `Bin` adresáře. Místo toho sestavení projektu webu hlásí chyby kompilace.
- Podpora pro automatickou kompilaci. Webové projekty jsou obvykle nasazují zkopírováním značkami a zdrojový kód do produkčního prostředí, i když kód může být předkompilovaných (explicitní kompilace).

Microsoft obnovená modelu projektu webové aplikace po jeho vydání Visual Studio 2005 Service Pack 1. Visual Web Developer dál však pouze podporovat model webový projekt. Dobrá zpráva je, že toto omezení byl vyřazen s Visual Web Developer 2008 Service Pack 1. Dnes můžete vytvořit aplikace ASP.NET v sadě Visual Studio (a aplikace Visual Web Developer) pomocí modelu projektu webové aplikace nebo model webový projekt. Oba modely mají jejich výhody a nevýhody. Odkazovat na [Úvod do projekty webových aplikací: projekty webových aplikací a porovnávání projekty webů](https://msdn.microsoft.com/en-us/library/aa730880.aspx#wapp_topic5) porovnání obou modelů a se můžete rozhodnout, jaký model projektu nejvhodnější pro vaši situaci.

## <a name="exploring-the-sample-web-application"></a>Zkoumání ukázkovou webovou aplikaci

Ke stažení pro tento kurz zahrnuje názvem recenze adresáře aplikace ASP.NET. Web napodobuje někdo může vytvořit web koníčku sdílet své kniha zkontroluje s online komunita. Tato webová aplikace ASP.NET je velmi jednoduchý a se skládá z těchto prostředků:

- `Web.config`, konfigurační soubor aplikace.
- Stránky předlohy (`Site.master`).
- Sedm různých stránkách ASP.NET:

    - ~/`Default.aspx`-domovské stránky daného webu.
    - ~/`About.aspx`-stránku "O the server".
    - ~/`Fiction/Default.aspx`-stránku se seznamem fiction knihy, které byly.

        - ~/`Fiction/Blaze.aspx`-o nové Richard Bachman *Blaze*.
    - ~/`Tech/Default.aspx`-stránku se seznamem knih technologie, které byly zkontrolovány.

        - ~/`Tech/CYOW.aspx`-o *vytvořit si vlastní web*.
        - ~/`Tech/TYASP35.aspx`-o *naučit sami technologie ASP.NET 3.5 za 24 hodin*.
- Tři různé soubory šablon stylů CSS v `Styles` složky.
- Čtyři bitové kopie souborům - používá technologii ASP.NET logo a bitové kopie zahrnuje tři zkontrolovat Books – všechny v `Images` složky.
- A `Web.sitemap` souboru, který definuje mapy webu a slouží k zobrazení v nabídkách `Default.aspx` stránky v kořenovém adresáři a `Fiction` a `Tech` složek.
- Třída soubor s názvem `BasePage.vb` na základní, který definuje `Page` třídy. Tato třída rozšiřuje funkce `Page` třída automaticky nastavením `Title` vlastnost podle polohy stránky v mapě webu. Stručně řečeno, jsou všechny třídy kódu ASP.NET, rozšiřuje `BasePage` (místo `System.Web.UI.Page`) bude mít její název nastavená na hodnotu v závislosti na pozici v mapy webu. Například při prohlížení ~ /`Tech/CYOW.aspx` název stránky, je nastavena na "Domů: technologie: vytvoření vlastního webu".

Obrázek 1 zobrazuje snímek obrazovky na webu knihy recenze při zobrazení prostřednictvím prohlížeče. V tomto poli se zobrazí stránka ~ / Tech/TYASP35.aspx, která zkontroluje knihy *naučit sami technologie ASP.NET 3.5 za 24 hodin*. S popisem cesty, která zahrnuje horní části stránky a v nabídce v levém sloupci jsou založené na strukturu mapy webu definované v `Web.sitemap`. Bitové kopie v pravém horním rohu je jedním z obálky knihy obrázky umístěné v `Images` složky. Webu vzhled a chování jsou definovány pomocí pravidla šablony kaskádových vypsány soubory šablon stylů CSS v `Styles` složky, když na hlavní stránce je definován zastřešující rozložení stránky `Site.master`.


[![Na webu knihy zkontroluje nabízí recenze na širokou názvy](determining-what-files-need-to-be-deployed-vb/_static/image2.png)](determining-what-files-need-to-be-deployed-vb/_static/image1.png)

**Obrázek 1**: zkontroluje adresáře webu nabízí recenze na širokou názvy ([Kliknutím zobrazit obrázek v plné velikosti](determining-what-files-need-to-be-deployed-vb/_static/image3.png))


Tato aplikace nepoužívá databázi. Každý kontrolní je implementovaný jako samostatné webové stránky v aplikaci. V tomto kurzu (a další kurzy několik) provede procesem nasazení webové aplikace, která nemá databáze. Ale v budoucnu kurzu jsme se zlepšila této aplikace tak, aby recenze, čtečky komentáře a další informace v databázi a bude prozkoumejte, jaké kroky je potřeba provést správně nasazení webových aplikací.

> [!NOTE]
> Tyto kurzy zaměřit na hostování aplikace ASP.NET ke zprostředkovateli webového hostitele a není prozkoumat pomocnou témata jako ASP. NET na mapu serveru nebo pomocí základní třídy stránky. Další informace o těchto technologií a pro další informace o další témata popsaná v tomto kurzu naleznete v části Další čtení na konci každého kurzu.


V tomto kurzu stažení má dvě kopie webové aplikace, každý implementovaný jako jiný typ projektu sady Visual Studio: BookReviewsWAP, projekt webové aplikace a BookReviewsWSP, webový projekt. Oba projekty se vytvořily s Visual Web Developer 2008 SP1 a pomocí technologie ASP.NET 3.5 SP1. Pro práci s těmihle projekty začít rozbalení obsahu na pracovní plochu. Chcete-li spustit projekt webové aplikace (BookReviewsWAP), přejděte na `BookReviewsWAP` složky a poklikejte na soubor řešení `BookReviewsWAP.sln`. Otevřete webový projekt (BookReviewsWSP), spusťte sadu Visual Studio a pak v nabídce Soubor vyberte možnost Otevřít web, přejděte na `BookReviewsWSP` na pracovní ploše a klikněte na tlačítko OK.


Zbývající dvě části tohoto kurzu pohled na jaké soubory, budete muset zkopírovat do produkčního prostředí při nasazení aplikace. Následující dva kurzy - [ *nasazení vaše lokality pomocí protokolu FTP* ](deploying-your-site-using-an-ftp-client-vb.md) a [ *nasazení vaše lokality pomocí sady Visual Studio* ](deploying-your-site-using-visual-studio-vb.md) -různé způsoby, jak zobrazit Zkopírujte tyto soubory zprostředkovatele webového hostitele.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Určení souborů k nasazení pro projekt webové aplikace

Model projektu webové aplikace používá explicitní kompilace – kompilace projektu zdrojového kódu do jednoho sestavení pokaždé, když sestavení aplikace. Tato kompilace zahrnuje soubory kódu na stránkách ASP.NET (~ /`Default.aspx.vb`, ~ /`About.aspx.vb`a tak dále), a taky `BasePage.vb` – třída. Výsledné sestavení jmenuje `BookReviewsWAP.dll` a nachází se v aplikačním `Bin` adresáře.

Obrázek 2 ukazuje soubory, které tvoří projekt kniha recenze webové aplikace.


[![Průzkumníku řešení seznam souborů, které tvoří projekt webové aplikace.](determining-what-files-need-to-be-deployed-vb/_static/image5.png)](determining-what-files-need-to-be-deployed-vb/_static/image4.png)

**Obrázek 2**: Průzkumníku řešení obsahuje soubory, které tvoří projekt webové aplikace


> [!NOTE]
> Jak je vidět na obrázku 2, nejsou zobrazeny soubory kódu na stránkách ASP.NET v Průzkumníku řešení pro projekt webové aplikace Visual Basic. Chcete-li zobrazit třídu kódu pro stránku, na stránce v Průzkumníku řešení klikněte pravým tlačítkem a zvolte kód zobrazení.


K nasazení aplikace ASP.NET vyvinuté pomocí vytvoření aplikace tak, aby explicitně zkompilovat nejnovější zdrojový kód do sestavení modelu spuštění projektu webové aplikace. Zkopírujte následující soubory do produkčního prostředí:

- Stránka soubory, které obsahují deklarativní pro každou technologii ASP.NET, jako například ~ /`Default.aspx`, ~ /`About.aspx`a tak dále. Navíc kopie až deklarativní pro všechny hlavní stránky a ovládací prvky uživatele.
- Sestavení (`.dll` souborů) v `Bin` složky. Není potřeba kopírovat soubory databáze programu (`.pdb`) nebo všechny soubory XML, můžete zjistit v `Bin` adresáře.

Není potřeba kopírovat soubory zdrojového kódu na stránkách ASP.NET do produkčního prostředí, ani je třeba zkopírovat `BasePage.vb` soubor třídy.

> [!NOTE]
> Jak je vidět na obrázku 2, `BasePage` třída je implementovaný jako soubor třídy v projektu umístěny ve složce s názvem `HelperClasses`. Při kompilaci projektu kód `BasePage.vb` soubor zkompilován společně s třídy kódu na pozadí na stránkách ASP.NET do jednoho sestavení `BookReviewsWAP.dll`. Technologie ASP.NET má speciální složky s názvem `App_Code` který je určený k uložení souborů tříd pro webové projekty. Kód `App_Code` složku automaticky kompiluje a proto by neměl být použit s projekty webových aplikací. Místo toho byste měli umístit soubory třídy vaší aplikace do normální složku s názvem `HelperClasses`, nebo `Classes`, nebo něco podobné. Alternativně můžete umístit soubory třídy v samostatných projektu knihovny tříd.


Kromě zkopírování soubory související s ASP.NET revize a sestavení v `Bin` složky, musíte taky zkopírovat soubory podporu na straně klienta - bitové kopie a souborů CSS - i další serverové podpůrné soubory, `Web.config` a `Web.sitemap`. Tyto klienta a serveru straně podporovat potřeba soubory zkopírují do produkčního prostředí bez ohledu na to, jestli použít explicitní nebo automatické kompilace.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Určení souborů k nasazení pro soubory projektu webu

Webový projekt modelu podporuje automatickou kompilaci, funkce není k dispozici při použití modelu projektu webové aplikace. S explicitní kompilace musí kompilace zdrojového kódu vašeho projektu do sestavení a zkopírujte sestavení do produkčního prostředí. Na druhé straně s automatickou kompilaci můžete jednoduše zkopírovat zdrojový kód do produkčního prostředí a je modulem runtime na vyžádání kompilován podle potřeby.

Možnost nabídky sestavení v sadě Visual Studio je v projekty webových aplikací a webové projekty. Vytváření projekty webových aplikací do jednoho sestavení umístěné v zkompiluje projektu zdrojový kód `Bin` directory; vytváření webového projektu kontroluje všechny chyby při kompilaci, ale nevytvoří žádné sestavení. Nasazení aplikace ASP.NET vyvinuté pomocí všechny webový projekt modelu je potřeba udělat je kopie příslušné soubory do produkčního prostředí, ale I by doporučujeme první sestavení projektu zajistit, že nejsou žádné chyby kompilace.

Obrázek 3 ukazuje soubory, které tvoří kniha recenze webový projekt.


[![Průzkumníku řešení seznam souborů, které tvoří webový projekt.](determining-what-files-need-to-be-deployed-vb/_static/image7.png)](determining-what-files-need-to-be-deployed-vb/_static/image6.png)

**Obrázek 3**: Průzkumníku řešení obsahuje soubory, které tvoří webový projekt


Nasazení webového projektu zahrnuje kopírování všech souborů související s ASP.NET do produkčního prostředí - zahrnující značek stránek pro stránky ASP.NET, hlavní stránky a uživatelských ovládacích prvků, spolu s jejich soubory s kódem. Budete muset zkopírovat taky zálohu všech souborů třídy, jako `BasePage.vb`. Všimněte si, že `BasePage.vb` soubor je umístěný ve `App_Code` složky, která je speciální složky ASP.NET používá v projektech webových stránek pro soubory třídy. Speciální složky musí být vytvořen na produkční taky jako soubory třídy v `App_Code` složka na vývojovém prostředí musí být zkopírován do `App_Code` složky na produkční.

Kromě zkopírování souborů ASP.NET značkami a zdrojový kód, musíte taky zkopírovat soubory podporu na straně klienta - bitové kopie a souborů CSS - i další serverové podpůrné soubory, `Web.config` a `Web.sitemap`.

> [!NOTE]
> Webové projekty můžete také použít explicitní kompilace. Budoucí kurzu bude zkontrolujte jak explicitně zkompilovat webový projekt.


## <a name="summary"></a>Souhrn

Nasazení aplikací ASP.NET zahrnuje kopírování potřebné soubory z vývojového prostředí do produkčního prostředí. Přesné sadu souborů, které je třeba synchronizovat, závisí na tom, jestli aplikace ASP.NET explicitně nebo automaticky kompilace kódu. Kompilace strategie těmto nekompatibilitám je ovlivněno jestli je nakonfigurovaný Visual Studio ke správě aplikace ASP.NET pomocí modelu projektu webové aplikace nebo webový projekt modelu.

Model projektu webové aplikace používá explicitní kompilace a kompilovaný kód projektu do jednoho sestavení v `Bin` složky. Při nasazení aplikace, část značek stránek ASP.NET a obsah `Bin` složky musí být posunuta do produkčního prostředí; zdrojového kódu v aplikaci – soubory kódu a kódu třídy, například - není nutné Chcete-li zkopírovat do produkčního prostředí.

Přestože je možné explicitně zkompilovat webový projekt, protože jsme se zobrazí v budoucnosti kurzy, používá model webový projekt ve výchozím nastavení, automatickou kompilaci. Nasazení aplikace ASP.NET, která používá automatickou kompilaci vyžaduje, aby část značek *a* zdrojového kódu, musí se zkopírovat do produkčního prostředí. Kód je automaticky zkompilovány v provozním prostředí, při prvním požadavku.

Jsme zkoumat, jaké soubory je potřeba synchronizovat mezi prostředí vývoje a produkční jsme připraveni k nasazení aplikace recenze adresáře ke zprostředkovateli webového hostitele.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Přehled kompilace technologie ASP.NET](https://msdn.microsoft.com/en-us/library/ms178466.aspx)
- [Uživatelské ovládací prvky ASP.NET](https://msdn.microsoft.com/en-us/library/y6wb1a0e.aspx)
- [Zkoumání ASP. Navigace na webu na NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Úvod do projekty webových aplikací](https://msdn.microsoft.com/en-us/library/aa730880.aspx)
- [Hlavní stránka kurzy](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Sdílení kódu mezi stránkami](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Použití vlastní základní třída pro třídy kódu stránky ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Systém projektu webu Visual Studio 2005: co je to a proč jsme to udělali?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Návod: Převádění webový projekt na projekt webové aplikace v sadě Visual Studio](https://msdn.microsoft.com/en-us/library/aa983476.aspx)

>[!div class="step-by-step"]
[Předchozí](asp-net-hosting-options-vb.md)
[další](deploying-your-site-using-an-ftp-client-vb.md)
