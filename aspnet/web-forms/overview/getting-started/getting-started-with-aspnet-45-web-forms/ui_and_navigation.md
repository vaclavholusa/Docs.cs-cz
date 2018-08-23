---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Uživatelské rozhraní a navigace | Dokumentace Microsoftu
author: Erikre
description: V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 pro jsme...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: 55c659cbaf48dbb02dc34e013242443d4fbd8845
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756663"
---
<a name="ui-and-navigation"></a>Uživatelské rozhraní a navigace
====================
podle [Erik Reitan](https://github.com/Erikre)

[Stáhněte si ukázkový projekt Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronickou knihu (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu se zdrojovým kódem jazyka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) je k dispozici v této sérii kurzů.


V tomto kurzu se upraví uživatelského rozhraní z výchozí webové aplikace k podpoře funkce úložiště front-aplikace Wingtip Toys. Také budete přidávat jednoduché a data svázaná navigace. V tomto kurzu vychází z předchozí kurz o službě "Vytvoření the vrstvy přístupu k datům" a je součástí série kurzů na adresář Wingtip Toys.

## <a name="what-youll-learn"></a>Co se dozvíte:

- Postup změny uživatelského rozhraní pro podporu funkcí úložiště front-aplikace Wingtip Toys.
- Jak nakonfigurovat HTML5 elementu zahrnout navigace po stránkách.
- Jak vytvořit ovládací prvek s daty pro navigaci k datům konkrétního produktu.
- Postup zobrazení dat z databáze vytvořené pomocí platformy Entity Framework Code First.

Webové formuláře ASP.NET umožňují vytvářet dynamický obsah pro vaši webovou aplikaci. Způsobem, který je podobný jako statická stránka HTML Web (stránka, která nezahrnuje zpracování na serveru) se vytvoří každou webovou stránku ASP.NET, ale dodatečné prvky, které technologie ASP.NET rozezná a spravuje pro generují kód HTML při spuštění stránky obsahuje webovou stránku ASP.NET.

S jako statická stránka HTML (*.htm* nebo *.html* souboru), server splňuje `Web` žádosti o čtení souboru a odešlete ho jako-je v prohlížeči. Naopak když uživatel požádá o webovou stránku ASP.NET (*.aspx* souboru), stránka běží jako aplikace na webovém serveru. Je spuštěn na stránce, může provádět všechny úlohy, které vyžaduje web, včetně výpočet hodnot, čtení nebo zápis informací o databázi nebo volání jiných programů. Na stránce jako výstup, dynamicky vytváří značky (například elementy ve formátu HTML) a odešle tento dynamického výstupu do prohlížeče.

## <a name="modifying-the-ui"></a>Úprava uživatelského rozhraní

Změnou budete pokračovat v této sérii kurzů *Default.aspx* stránky. Upraví uživatelského rozhraní, které je již vytvořeno pomocí výchozí šablony použité k vytvoření aplikace. Typ změny, které budete používat jsou běžně při vytváření všech aplikací webových formulářů. Můžete udělat tak, že změna názvu, nahraďte část obsahu a odebírá nepotřebné výchozí obsah.

1. Otevřete nebo přejděte *Default.aspx* stránky.
2. Pokud se zobrazí v **návrhu** zobrazit, přepněte na **zdroj** zobrazení.
3. V horní části stránky `@Page` direktiv, změnit `Title` atribut "Vítejte," jak je znázorněno zvýrazněné žlutou barvou níže. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Také na *Default.aspx* stránka, nahraďte všechna výchozí obsahu součástí `<asp:Content>` označit tak, aby se zobrazí jako značky níže. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Uložit *Default.aspx* stránky tak, že vyberete **uložit Default.aspx** z **souboru** nabídky.

   Výsledná *Default.aspx* stránka bude vypadat následovně: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

V tomto příkladu jste nastavili `Title` atribut `@Page` směrnice. Když se objeví HTML v prohlížeči do kódu serveru `<%: Page.Title %>` přeloží na obsah součástí `Title` atribut.

Příklad stránky obsahuje základní prvky, které tvoří webovou stránku ASP.NET. Na stránce obsahuje statický text může mít na stránce HTML, spolu s prvky, které jsou specifické pro technologii ASP.NET. Součástí obsahu *Default.aspx* stránky, bude se integrovat s obsahem stránky předlohy, která budou vysvětlena dále v tomto kurzu.

### <a name="page-directive"></a>@Page – Direktiva

Webové formuláře ASP.NET obvykle obsahovat direktivy, které vám umožňují určit informace stránky vlastností a konfigurace pro stránku. Direktivy jsou použitý technologií ASP.NET jako pokyny, jak proces na stránce, ale nejsou vykreslit jako součást značky, které je odesláno prohlížeči.

Nejčastěji používané direktiva je `@Page` směrnice, které vám umožní určit mnoho možností konfigurace pro stránku, včetně následujících:

1. Server programovací jazyk pro kód ve stránce, jako je C#.
2. Určuje, zda stránka je stránka s serverový kód přímo na stránce, kterému se říká stránku jedním souborem, nebo zda je stránka s kódem v samostatné třídě souboru, který se nazývá použití modelu code-behind stránka.
3. Určuje, zda stránka má související stránky předlohy a proto by měl být považován za stránku obsahu.
4. Možnosti ladění a trasování.

Pokud není zadána `@Page` direktiv na stránce nebo pokud direktivu neobsahuje konkrétní nastavení, nastavení se budou dědit z *Web.config* konfiguračního souboru nebo z *Machine.config* konfigurační soubor. *Machine.config* soubor poskytuje další konfiguraci nastavení pro všechny aplikace spuštěné na počítači.

> [!NOTE] 
> 
> *Machine.config* také obsahuje podrobné informace o nastavení všechny možné konfigurace.


### <a name="web-server-controls"></a>Ovládací prvky webového serveru

Ve většině aplikací webových formulářů ASP.NET přidejte ovládací prvky, které uživateli umožňují interakci s stránky, například tlačítka, textová pole, seznamy a tak dále. Tyto ovládací prvky webového serveru jsou podobné tlačítka HTML a vstupních prvků. Nicméně se zpracovávají na serveru, abyste mohli používat kódu serveru můžete nastavit jejich vlastnosti. Tyto ovládací prvky také vyvolávají události, které dokáže zpracovat v serverovém kódu.

Serverové ovládací prvky pomocí speciální syntaxe, která rozpozná technologie ASP.NET při spuštění stránky. Název značky pro ovládací prvky ASP.NET serveru začíná `asp:` předponu. To umožňuje technologii ASP.NET nerozpozná a nenahraje a zpracování těchto ovládacích prvků serveru. Předpona, která může lišit, pokud ovládací prvek není součástí rozhraní .NET Framework. Kromě `asp:` předponu, serverové ovládací prvky ASP.NET také zahrnout `runat="server"` atribut a `ID` , že vám pomůže odkazovat na ovládací prvek v serverovém kódu.

Při spuštění stránky technologie ASP.NET identifikuje serverové ovládací prvky a spouští kód, který je spojen s těmito ovládacími prvky. Mnoho ovládacích prvků vykreslovat kód HTML nebo jiné značky do stránky, jakmile se zobrazí v prohlížeči.

### <a name="server-code"></a>Kód serveru

Většina aplikací webových formulářů ASP.NET zahrnuje kód, který běží na serveru při zpracování stránky. Jak je uvedeno výše, serverový kód slouží k provádění různých věcí, například přidání dat do ovládacího prvku ListView. Technologie ASP.NET podporuje řadu jazyků pro spouštění na serveru, včetně C#, Visual Basic, J# a dalších.

Technologie ASP.NET podporuje dva modely pro psaní kódu serveru pro webové stránky. V modelu s jedním souborem kódu stránky je v prvku skriptu zahrnujícím počáteční značka `runat="server"` atribut. Alternativně můžete vytvořit kód pro stránku v samostatném souboru třídy, což se označuje jako model použití modelu code-behind. V takovém případě stránky webových formulářů ASP.NET obecně neobsahuje žádný kód serveru. Místo toho `@Page` – direktiva obsahuje informace, které odkazuje *.aspx* stránku s jeho souboru přidruženého kódu na pozadí.

`CodeBehind` Obsažené v atributu `@Page` direktiva Určuje název souboru samostatné třídy a `Inherits` atribut určuje název třídy v souboru kódu na pozadí, která odpovídá na stránku.

### <a name="updating-the-master-page"></a>Aktualizace stránky předlohy

Stránky předlohy webových formulářů ASP.NET, umožňují vytvoření konzistentního rozložení pro stránky v aplikaci. Jedna stránka předlohy definuje vzhled a chování a standardní chování, které chcete použít pro všechny stránky (nebo skupinu stránek) ve vaší aplikaci. Potom můžete vytvořit jednotlivé stránky obsahu, které obsahují obsah, který chcete zobrazit, jak je vysvětleno výše. Při vyžádání obsahu stránky, budou ASP.NET sloučí s hlavní stránkou vytvořit výstup, který kombinuje rozložení stránky předlohy s obsahem ze stránky obsahu.

Nový web potřebuje jeden logo, které se zobrazí na každé stránce. Pokud chcete přidat toto logo, můžete upravit HTML na stránce předlohy.

1. V **Průzkumníka řešení**, najít a otevřít **Site.Master** stránky.
2. Pokud je stránka ve **návrhu** zobrazit, přepněte na **zdroj** zobrazení.
3. Aktualizovat hlavní stránku **úpravy nebo přidání** zvýrazněné žlutou barvou značky: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Tento kód HTML zobrazí image s názvem *logo.jpg* z *Imagí* složce webové aplikace, které přidáte později. Pokud v prohlížeči se zobrazí stránka, která používá stránku předlohy, zobrazí se loga. Pokud uživatel klikne na logo, uživatel přejde zpět *Default.aspx* stránky. Značka HTML anchor `<a>` obaluje ovládací prvek image serveru a umožňuje obrázek, který se má zahrnutý jako součást odkazu. `href` Atributů pro značku ukotvení Určuje kořenový "`~/`" webu jako umístění propojení. Ve výchozím nastavení *Default.aspx* stránky se zobrazí, když uživatel přejde na kořen webu. **Image** `<asp:Image>` serverový ovládací prvek obsahuje další vlastnosti, jako například `BorderStyle`, vykreslí jako HTML při zobrazení v prohlížeči.

### <a name="master-pages"></a>Stránky předlohy

Hlavní stránka je soubor s příponu .master (například *Site.Master*) pomocí předdefinovaných rozložení, který může obsahovat statický text, HTML elementů a ovládacích prvků serveru. Hlavní stránka je označena speciální `@Master` směrnice, který nahrazuje `@Page` direktiva, která se používá pro běžné *.aspx* stránky.

Kromě `@Master` direktiv, stránky předlohy také obsahuje všechny nejvyšší úrovně elementy HTML stránky, jako například `html`, `head`, a `form`. Například na hlavní stránce jste přidali výše, použijete HTML `table` rozložení, `img` – element pro logo společnosti, statický text a serverových ovládacích prvků pro zpracování běžných členství pro svůj web. Veškeré kódování HTML a všechny prvky technologie ASP.NET můžete použít jako součást stránky předlohy.

Kromě statický text a ovládací prvky, které se zobrazí na všech stránkách stránky předlohy také zahrnuje jednu nebo více **ContentPlaceHolder** ovládacích prvků. Tyto ovládací prvky zástupný symbol definovat oblastech, kde se objeví replaceable obsah. Zase replaceable obsah je definována v obsahu stránky, jako například *Default.aspx*, použije **obsah** serverový ovládací prvek.

#### <a name="adding-image-files"></a>Přidání souborů obrázků

Obrázek loga, která je popsána výše, spolu s všechny obrázků produktů, musíte přidat do webové aplikace tak, že je můžete vidět, když je projekt zobrazen v prohlížeči.

#### <a name="download-from-msdn-samples-site"></a>Stáhněte z webu MSDN ukázky:

[Začínáme s webovými formuláři ASP.NET 4.5 a Visual Studio 2013 – na adresář Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

Soubor ke stažení obsahuje prostředky v *Northwind prostředky* složku, která se použijí k vytvoření ukázkové aplikace.

1. Pokud jste tak již neučinili, stáhněte si komprimovaný ukázkové soubory pomocí na výše uvedeném odkazu z webu MSDN ukázky.
2. Po stažení otevřete soubor .zip a zkopírujte obsah do místní složky na svém počítači.
3. Vyhledání a otevření *Northwind prostředky* složky.
4. Přetahováním myší, zkopírujte *katalogu* složku z vaší místní složky do kořenového adresáře projektu webové aplikace v **Průzkumníka řešení** sady Visual Studio. 

    ![Uživatelské rozhraní a navigace – kopírování souborů](ui_and_navigation/_static/image1.png)
5. Dále vytvořte novou složku s názvem *image* kliknutím pravým tlačítkem myši **Northwind** projekt **Průzkumníka řešení** a vyberete **přidat**  - &gt; **Novou složku**.
6. Kopírování *logo.jpg* soubor *Northwind prostředky* složky v **Průzkumníka souborů** k *Imagí* složce webové aplikace projekt **Průzkumníka řešení** sady Visual Studio.
7. Klikněte na tlačítko **zobrazit všechny soubory** možnosti v horní části **Průzkumníka řešení** k aktualizaci seznamu souborů, pokud se nové soubory.  
  
    **Průzkumník řešení** teď zobrazuje aktualizovaný projekt soubory. 

    ![Uživatelské rozhraní a navigace – Průzkumník řešení](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Přidání stránek

Před přidáním navigace k webové aplikaci, budete nejprve přidat dvě nové stránky, které budete přejděte do. Později v této řadě kurzů budete zobrazovat produktů a podrobnosti o produktu na tyto nové stránky.

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na **Northwind**, klikněte na tlačítko **přidat**a potom klikněte na tlačítko **nová položka**.   
 **Přidat novou položku** se zobrazí dialogové okno.
2. Vyberte **Visual C#**  - &gt; **webové** šablony skupiny na levé straně. Vyberte **webové formuláře se stránkou předlohy** uprostřed seznamu a pojmenujte ho *ProductList.aspx*. 

    ![Uživatelské rozhraní a navigace – přidání nové položky dialogového okna](ui_and_navigation/_static/image3.png)
3. Vyberte **Site.Master** připojení na nově vytvořený na hlavní stránce *.aspx* stránky. 

    ![Uživatelské rozhraní a navigace - vybrat hlavní stránku](ui_and_navigation/_static/image4.png)
4. Přidání další stránky s názvem *ProductDetails.aspx* následujícím postupem stejné.

### <a name="updating-bootstrap"></a>Aktualizuje se Bootstrap

Použijte šablony projektů Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), rozložení a motivy rozhraní vytvořené Twitter. K zajištění přizpůsobivý návrh, což znamená, že rozložení můžete dynamicky přizpůsobit velikosti okna jiný prohlížeč používá Bootstrap CSS3. Funkce motivů Bootstrap můžete také snadno provést změnu v aplikačním vzhled a chování. Výchozí šablony webové aplikace ASP.NET v sadě Visual Studio 2013 obsahuje Bootstrap jako balíček NuGet.

V tomto kurzu se změní vzhled a chování aplikace Wingtip Toys nahrazením soubory CSS Bootstrapu.

1. V **Průzkumníka řešení**, otevřete *obsahu* složky.
2. Klikněte pravým tlačítkem myši *bootstrap.css* souboru a přejmenujte ho na *bootstrap original.css*.
3. Přejmenovat *bootstrap.min.css* k *bootstrap original.min.css*.
4. V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *obsahu* a pak zvolte položku **otevřít složku v Průzkumníku souborů**.  
   Zobrazí se v Průzkumníku souborů. Do tohoto umístění se uloží stažené bootstrap soubory šablon stylů CSS.
5. V prohlížeči přejděte na [ https://bootswatch.com/3/ ](https://bootswatch.com/3/).
6. Přejděte okno prohlížeče, dokud se nezobrazí Cerulean motiv. 

    ![Uživatelské rozhraní a navigace - Cerulean motiv](ui_and_navigation/_static/image5.png)
7. Stáhněte si oba *bootstrap.css* souboru a *bootstrap.min.css* do souboru *obsahu* složky. Použijte cestu ke složce, která se zobrazí v obsahu **Průzkumníka souborů** okno, které jste dříve otevřeli.
8. V **sady Visual Studio** v horní části **Průzkumníka řešení**, vyberte **zobrazit všechny soubory** možnosti se zobrazí nové soubory ve složce obsahu. 

    ![Uživatelské rozhraní a navigace – Průzkumník řešení](ui_and_navigation/_static/image6.png)

   Zobrazí se dva nové soubory šablon stylů CSS v **obsahu** složka, ale Všimněte si, že na ikonu vedle názvu každého souboru je zobrazena šedě. To znamená, že soubor ještě nejsou přidané do projektu.
9. Klikněte pravým tlačítkem myši *bootstrap.css* a *bootstrap.min.css* souborů a vyberte **zahrnout do projektu**.   
   Když spustíte aplikaci Wingtip Toys později v tomto kurzu, zobrazí se nové uživatelské rozhraní.

> [!NOTE] 
> 
> Používá šablony webové aplikace ASP.NET *Bundle.config* souboru v kořenovém adresáři projektu pro uložení této cesty souborů CSS Bootstrapu.


### <a name="modifying-the-default-navigation"></a>Změna výchozí navigace

Výchozí navigaci na každé stránce v aplikaci můžete upravit změnou prvek seznamu Neseřazený navigace, který je v *Site.Master* stránky.

1. V **Průzkumníka řešení**, najděte a otevřete *Site.Master* stránky.
2. Přidejte další navigační odkaz zvýrazněné žlutou barvou na Neseřazený seznam je uvedeno níže:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Jak je vidět ve výše uvedené ve formátu HTML můžete upravit každé položky na řádku `<li>` obsahující značku ukotvení `<a>` s odkazem `href` atribut. Každý `href` odkazuje na stránku ve webové aplikaci. V prohlížeči, když uživatel klikne na jednu z těchto odkazů (například **produkty**), bude přejděte na stránku obsažené v `href` (například **ProductList.aspx**). Na konci tohoto kurzu budete spouštět aplikace.

> [!NOTE] 
> 
> Tilda (`~`) znak se používá k určení, která `href` cesta začíná v kořenovém adresáři projektu.


### <a name="adding-a-data-control-to-display-navigation-data"></a>Přidání ovládacího prvku dat k zobrazení dat navigace

V dalším kroku přidáte ovládací prvek pro zobrazení všech kategorií z databáze. Každá kategorie bude fungovat jako odkaz *ProductList.aspx* stránky. Když uživatel klikne na odkaz kategorii v prohlížeči, budou přejděte na stránku produktů a zobrazit pouze produkty, které jsou spojené s vybranou kategorii.

Budete používat **ListView** ovládací prvek pro zobrazení všech kategorií obsaženým v databázi. Chcete-li přidat **ListView** ovládacího prvku k hlavní stránce:

1. V *Site.Master* stránce, přidejte následující zvýrazněný `<div>` element **po** `<div>` element obsahující `id="TitleContent"` , který jste přidali dříve:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Tento kód zobrazí všechny kategorie z databáze. **ListView** ovládací prvek zobrazí názvy jednotlivých kategorií jako text odkazu a zahrnuje odkaz *ProductList.aspx* stránky obsahující hodnotu řetězce dotazu `ID` kategorie. Nastavením `ItemType` vlastnost **ListView** řídit vazbový výraz `Item` je k dispozici v rámci `ItemTemplate` uzlu a ovládací prvek stane silného typu. Můžete vybrat podrobnosti `Item` pomocí technologie IntelliSense, jako je například určení `CategoryName`. Tento kód se nachází v kontejneru `<%#: %>` , který označuje vazbový výraz. Přidáním (:) na konec objektu `<%#` předponu, výsledek výrazu datové vazby je kódovaný jazykem HTML. Pokud je výsledek kódovaný jazykem HTML, vaše aplikace je líp chráněný proti webů vkládání (mezi weby XSS) a HTML útoky prostřednictvím injektáže skriptu.

> [!NOTE] 
> 
> **Tip**
> 
> Když přidáte tak, že zadáte během vývoje kódu, můžete byste si být jisti, že platným členem objektu je najít, protože se silnými typy, ovládací prvky dat zobrazit dostupné členy podle technologie IntelliSense. Technologie IntelliSense nabízí možnosti odpovídající kontext kódu při psaní kódu, jako jsou vlastnosti, metody a objekty.


V dalším kroku budete implementovat `GetCategories` metoda načíst data.

### <a name="linking-the-data-control-to-the-database"></a>Propojení ovládacího prvku dat do databáze

Předtím, než můžou zobrazovat data v ovládacím prvku dat, budete muset propojit ovládací prvek dat do databáze. Chcete-li na odkaz, můžete upravit kód za *Site.Master.cs* souboru.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Site.Master* stránce a potom klikněte na tlačítko **zobrazit kód**. *Site.Master.cs* soubor je otevřen v editoru.
2. Poblíž začátku *Site.Master.cs* přidejte dva další obory názvů tak, aby zahrnuté obory názvů vypadat následovně:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Přidat zvýrazněnou `GetCategories` za `Page_Load` obslužná rutina události následujícím způsobem:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

Výše uvedený kód je spuštěn při načtení každou stránku, která používá hlavní stránku v prohlížeči. `ListView` Ovládací prvek (s názvem "categoryList"), který jste přidali dříve v tomto kurzu používá vazbu modelu vyberte data. Ve značkách `ListView` ovládacího prvku můžete nastavit u tohoto prvku `SelectMethod` vlastnost `GetCategories` metody uvedené výše. `ListView` Řídit volání `GetCategories` metoda v příslušnou dobu života stránky cyklu a automaticky vytvoří vazbu vrácená data. Naučíte se další informace o vázání dat v dalším kurzu.

### <a name="running-the-application-and-creating-the-database"></a>Spuštění aplikace a vytvoření databáze

Dříve v této sérii kurzů jste vytvořili třídu inicializátorů (s názvem "ProductDatabaseInitializer") a zadat tuto třídu v *global.asax.cs* souboru. Entity Framework, databázi vygeneruje, když aplikace je poprvé spustit, protože `Application_Start` metoda součástí *global.asax.cs* soubor bude volat třída inicializátoru. Inicializátor třídy použije tříd modelu (`Category` a `Product`), že jste přidali dříve v této sérii kurzů k vytvoření databáze.

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *Default.aspx* stránku a vybrat **nastavit jako úvodní stránku**.
2. Ve Visual Studiu stisknutím klávesy **F5**.   
 Bude trvat nějakou dobu nastavit všechno během této při prvním spuštění.   
    ![Uživatelské rozhraní a navigace – Windows prohlížeče](ui_and_navigation/_static/image7.png)  
 Při spuštění aplikace, aplikace bude zkompilována a databáze s názvem *wingtiptoys.mdf* budou vytvořeny v *aplikace\_Data* složky. V prohlížeči se zobrazí kategorie navigační nabídky. Tato nabídka se vygeneroval načtením kategorií z databáze. V dalším kurzu budete implementovat navigaci.
3. Ukončením prohlížeče zastavte spuštěnou aplikaci.

### <a name="reviewing-the-database"></a>Kontrola databáze

Otevřít *Web.config* soubor a podívejte se na část řetězce připojení. Vidíte, že `AttachDbFilename` hodnotu v připojovacím řetězci odkazuje `DataDirectory` pro projekt webové aplikace. Hodnota `|DataDirectory|` je rezervovanou hodnotu, která představuje *aplikace\_Data* složky v projektu. Tato složka je, kde se nachází databáze, který byl vytvořen z tříd entit.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Pokud *aplikace\_Data* složka není viditelný, nebo pokud tato složka je prázdný, vyberte **aktualizovat** ikonu a pak **zobrazit všechny soubory** ikonu v horní části **Průzkumníka řešení** okna. Rozbalení šířku **Průzkumníka řešení** windows může být nutné zobrazení všech dostupných ikon.


Teď si můžete prohlédnout data obsažená v *wingtiptoys.mdf* databázový soubor s použitím **Průzkumníka serveru** okna.

1. Rozbalte *aplikace\_Data* složky. Pokud *aplikace\_Data* složka není viditelný, najdete v poznámce výše.
2. Pokud *wingtiptoys.mdf* databázový soubor není viditelný, vyberte **aktualizovat** ikonu a pak **zobrazit všechny soubory** ikonu v horní části **Průzkumníka řešení**  okna.
3. Klikněte pravým tlačítkem myši *wingtiptoys.mdf* databázový soubor a vyberte **otevřít**.  
    **Průzkumník serveru** se zobrazí. 

    ![Uživatelské rozhraní a navigace – Průzkumník serveru](ui_and_navigation/_static/image8.png)
4. Rozbalte *tabulky* složky.
5. Klikněte pravým tlačítkem myši **produkty**tabulce a vybrat **zobrazit Data tabulky**.  
 **Produkty** tabulky se zobrazí. 

    ![Uživatelské rozhraní a navigace – tabulka produktů](ui_and_navigation/_static/image9.png)
6. Toto zobrazení umožňuje zobrazit a upravovat data v **produkty** tabulky ručně.
7. Zavřít **produkty** okno tabulky.
8. V **Průzkumníka serveru**, klikněte pravým tlačítkem myši **produkty** tabulku znovu a vyberte **Otevřít definici tabulky**.  
 Návrh dat pro **produkty** tabulky se zobrazí. 

    ![Uživatelské rozhraní a navigace – návrh produkty](ui_and_navigation/_static/image10.png)
9. V **T-SQL** kartě, zobrazí se příkaz SQL DDL, která byla použita k vytvoření této tabulky. Můžete také použít uživatelské rozhraní v **návrhu** kartu k úpravě schématu.
10. V **Průzkumníka serveru**, klikněte pravým tlačítkem na **Northwind** databáze a vyberte **ukončení připojení**.   
 Odpojení databáze ze sady Visual Studio, schéma databáze bude moci změnit později v této řadě kurzů.
11. Vraťte se na **Průzkumníka řešení**tak, že vyberete **Průzkumníka řešení** karta v dolní části **Průzkumníka serveru** okna.

## <a name="summary"></a>Souhrn

V tomto kurzu této série jste přidali nějaké základní uživatelské rozhraní, grafiky, stránky a navigace. Kromě toho jste spustili webové aplikace, který vytvoří databázi z datových tříd, které jste přidali v předchozím kurzu. Můžete také zobrazit obsah *produkty* tabulky databáze zobrazením databázi přímo. V dalším kurzu budete zobrazovat data položky a informace z databáze.

## <a name="additional-resources"></a>Další prostředky

[Úvod do programování webových stránek ASP.NET](https://msdn.microsoft.com/library/ms178125.aspx)   
[Přehled ovládacích prvků webového serveru technologie ASP.NET](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[Kurz šablon stylů CSS](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Předchozí](create_the_data_access_layer.md)
> [další](display_data_items_and_details.md)
