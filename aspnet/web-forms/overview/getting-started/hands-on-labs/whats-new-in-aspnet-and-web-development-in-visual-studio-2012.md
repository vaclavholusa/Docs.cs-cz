---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: "Co je nového v technologii ASP.NET a vývoj webů v sadě Visual Studio 2012 | Microsoft Docs"
author: rick-anderson
description: "Nová verze sady Visual Studio přináší řadu vylepšení, které jsou zaměřené na vylepšení zkušeností a výkon při práci s technologiemi Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: e57f1200aaa207c9109f2832cbf88629ed385bb5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>Co je nového v technologii ASP.NET a vývoj webů v sadě Visual Studio 2012
====================
podle [webové táborech Team](https://twitter.com/webcamps)

> Nová verze sady Visual Studio přináší řadu vylepšení, které jsou zaměřené na vylepšení zkušeností a výkon při práci s technologiemi Web. Visual Studio editory pro šablon stylů CSS, JavaScript a HTML byl zcela renovována zahrnout mnoho podpor kód nejvíce vyžádání, jako je například technologie IntelliSense a Automatické odsazení. Z hlediska výkonu sdružování a minimalizace jsou teď integrované jako doba načítání integrované funkce snadno snížit stránky.
> 
> Visual Studio umožňuje pracovat s nejnovější technologie webu. Fragmenty CSS3 různé prohlížeče vám pomůže Ujistěte se, že váš web funguje bez ohledu na platformu klienta také s využitím nové prvky HTML5 a funkce.
> 
> Zápis a profilování kódu JavaScript by měl být snazší s touto verzí sady Visual Studio. Seznamy IntelliSense, integrované funkce XML dokumentace a navigační jsou nyní k dispozici pro kód jazyka JavaScript. Nyní máte katalogu JavaScript na dosah ruky. Kromě toho můžete zkontrolovat dodržování předpisů ECMAScript5 pomocí skriptů a zjištění chyby syntaxe včas.
> 
> Poslední, ale není alespoň tato verze sady Visual Studio implementuje předdefinované sdružování a minimalizace. Soubory skriptů a stylů bude mnoha funkcemi a komprimované tak, aby lokality provádí rychleji.
> 
> Tato laboratoř vás provede procesem vylepšení a nových funkcí popsaných výše použitím malých změn na ukázkové webové aplikaci ve zdrojové složce zadané.
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V této rukou na testovacího prostředí se dozvíte, jak:

- Použít nové funkce a vylepšení v editoru stylů CSS
- Použít nové funkce a vylepšení v editoru HTML
- Použít nové funkce a vylepšení v editoru jazyka JavaScript
- Konfigurovat a používat sdružování a minimalizace

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

- [Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny o tom, jak ji nainstalovat).
- [Prostředí Windows PowerShell](https://support.microsoft.com/kb/968930/) (pro skripty instalace – již nainstalován ve Windows 8 a Windows Server 2008 R2)
- [Internet Explorer 10](https://windows.microsoft.com/en-US/internet-explorer/products/ie/home) – prohlížeč kompatibilní HTML5 nebo

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Tato rukou na testovacím zahrnuje následující cvičení:

1. [Cvičení 1: Co je nového v editoru stylů CSS](#Exercise1)
2. [Cvičení 2: Co je nového v editoru HTML](#Exercise2)
3. [Cvičení 3: Co je nového v editoru jazyka JavaScript](#Exercise3)
4. [Cvičení 4: Sdružování a Minifikace](#Exercise4)

Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Cvičení 1: Co je nového v editoru stylů CSS

Vývojáři webů by měla být obeznámeni s mnoha problémy související s úpravy šablon stylů CSS. Jeden z největších problémů, o stylu CSS je kompatibility mezi prohlížeči. Často dochází, že po použití stylů na váš web, si všimnete, že vypadá tak různé Pokud otevřete v jiném prohlížeči nebo zařízení. Proto může trávit mnoho času na opravě těchto problémů visual si uvědomí, že pokud provedete nakonec ho fungovat v jedné prohlížeče, ho je rozděleno v jiné.

Visual Studio teď obsahuje funkce, které vývojáři přístup, fungovat a efektivně uspořádání šablony stylů CSS. Během tohoto cvičení bude vyhovovat nové funkce pro organizaci efektivní a edice, a také CSS3 fragmenty kódu pro různé prohlížeče kompatibility.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Úloha 1 – nové funkce editoru

V této úloze bude zjišťovat nové funkce Editor šablon stylů CSS. Tento nový editor vám pomůže zvýšit produktivitu díky nové inteligentní odsazení, komentáře vylepšené kódu a seznamu rozšířené IntelliSense.

1. Spustit **Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení umístěný v **Source\WhatsNewASPNET** složky tohoto testovacího prostředí.
2. V Průzkumníku řešení otevřete **Site.css** soubor umístěný v části **styly** složky. Zajistěte, aby **textového editoru** nástroje se zobrazí na panelu nástrojů. To lze provést, vyberte **zobrazení** | **panely nástrojů** nabídky a zkontrolujte **textového editoru** možnosti. Si všimnete, protože tuto novou verzi **komentář** tlačítko (![komentář – tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) a **zrušte komentáře u** tlačítko (![zrušte komentář – tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) jsou povolená i u editor šablon stylů CSS.

    ![Povolení editoru a šablon stylů CSS nástroje](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "povolení editoru a šablon stylů CSS nástroje")

    *Povolení editoru a šablon stylů CSS nástroje*
3. Posuňte zobrazení kódu a vyberte všechny definice třídy CSS. Klikněte **komentář** (![komentář – tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) tlačítko komentář vybrané řádky. Potom klikněte **zrušte komentáře u** (![zrušte komentář u tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) tlačítko Zrušit změny.
4. Klikněte na tlačítko **sbalit** (![sbalit](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) a **rozbalte** (![rozbalte](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) tlačítka umístěný na levém okraji textu. Všimněte si, že teď můžete skrýt styly, které nepoužívají tak, aby měl čisticí zobrazení.

    ![Sbalení tříd CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "tříd CSS sbalení")

    *Sbalení tříd CSS*
5. Ujistěte se, že je povolena funkce Inteligentní odsazení. Vyberte **nástroje** | **možnosti** možnosti nabídky a potom vyberte **textového editoru** | **šablon stylů CSS**  |  **Formátování** stránky v levém podokně na obrazovce. Zkontrolujte **hierarchické odsazení** možnost.

    ![Povolení hierarchické odsazení](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "povolení hierarchické odsazení")

    *Povolení hierarchické odsazení*
6. Vyhledejte definice hlavní třídy (.main) a připojit stylu pro elementy div. Si všimnete, že kód zarovnaná automaticky, pomoc uživatelům najít nadřazené třídy na první pohled.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Hierarchická zarovnání v jazyce CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "hierarchické zarovnání v jazyce CSS")

    *Hierarchická zarovnání v jazyce CSS*
7. Uvnitř **.main div** třídy, vyhledejte kurzor na konci **ohraničení: 0px;** a stiskněte klávesu **Enter** zobrazíte seznam technologie IntelliSense. Začněte psát **horní** a Všimněte si filtrování seznamu během psaní. V seznamu se zobrazí prvky, které obsahují **horní** v žádné části slova (v předchozích verzích sady Visual Studio, v seznamu se filtrují podle položek, *začít* s označením).

    ![Vylepšení IntelliSense v jazyce CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "rozšíření technologie IntelliSense v jazyce CSS")

    *Vylepšení IntelliSense v jazyce CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Úloha 2 – Výběr barvy

V této úloze zjistíte, že nové volby barev šablon stylů CSS je integrována do Visual Studio IntelliSense.

1. V **Site.css,** vyhledejte definice třídy hlavičky (.header) a umístěte kurzor do **barvu pozadí** atribut mezi &quot;:&quot; a &quot; # &quot; znaky na tento řádek kódu **.**

    ![Vyhledání kurzor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "vyhledání kurzor")

    *Vyhledání kurzor*
2. Odstranit **dvojtečkou** (:) a zapisují znovu zobrazíte volby barev. Všimněte si, že první barev, které se zobrazí jsou nejčastěji se vyskytující barvy vašeho webu. Když kliknete na barvu bílé, jeho kód HTML (#fff) nahradí aktuální kód barvy v šabloně stylů.

    ![Výběr barvy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "volby barev")

    *Výběr barvy*
3. Stiskněte **rozbalte** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) na výběr barvy zobrazovat barvy, a poté přetáhněte přechodu kurzor vybrat barvu tlačítko. Potom klikněte **kapátko** tlačítko a vybrat libovolnou barvu na obrazovce. Všimněte si, že hodnoty barvy pozadí se dynamicky mění při přesunutí kurzoru.

    ![Barva výběru přechodu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "přechod výběr barev")

    *Přechod výběr barev*
4. V **krytí** posuvníku, k centru panelu ke snížení krytí přesunout modulu pro výběr. Všimněte si, že hodnota barvu pozadí nyní změny jeho měřítku RGBA.

    ![Výběr barvy krytí](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "volby barev neprůhlednost.")

    *Výběr barvy neprůhlednost.*

    > [!NOTE]
    > Definice barvy RGBA (červená, zelená, modrá, Alpha) ve specifikaci CSS3 umožňuje definovat hodnota neprůhlednosti barvu pro jednu položku. Na rozdíl od **krytí -** podobné atribut CSS  **-**  RGBA barev je také kompatibilní s nejnovějšími prohlížeči.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Úloha 3 – fragmenty kódu kompatibilní šablon stylů CSS

V této úloze se dozvíte, jak používat různé prohlížeče kompatibilní CSS3 fragmenty kvůli implementaci některé funkce ve vašem webu.

1. V **Site.css** souboru, vyhledejte **záhlaví** šablon stylů CSS třídy definice (.header) a umístěte kurzor níže  **/ \*ohraničení radius\* /**  zástupný symbol pro přidání nové fragment kódu. Stiskněte klávesu **Enter** k zobrazení seznamu IntelliSense a typ **radius** pro filtrování seznamu. Vyberte **border-radius** možnost ze seznamu s klikněte a potom stiskněte klávesu **KARTĚ** klíč vložit fragment. Poté zadejte velikost protokolu radius v pixelech a stiskněte klávesu **Enter**. Například zadejte **15px**.

    Atributy CSS3 přidal fragmentu vykreslí zaokrouhlené ohraničení ve většině prohlížečů dodržování předpisů HTML5, včetně Mozilla a na základě WebKit prohlížeče.

    ![Pomocí fragment border-radius](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "pomocí fragment border-radius")

    *Pomocí fragment border-radius*
2. Použít stejné **ohraničení** fragmenty kódu v styl stránky (.page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Stiskněte klávesu **F5** ke spuštění řešení. Všimněte si, že každé stránce teď má zaokrouhlené ohraničení.

    ![Zaokrouhlí rozích](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "zaokrouhlené rozích")

    *Zaoblenými hranami*
4. Zavřete prohlížeč a vraťte se k sadě Visual Studio.
5. Otevřete **Custom.css** soubor umístěný v části **styly** složky a umístěte kurzor do **div.images ul li img** definici třídy.
6. Zobrazte seznam technologie IntelliSense, typ **stín pole** a stiskněte klávesu **KARTĚ** klíč dvakrát, chcete-li vložit fragment kódu stínové výchozí do definice třídy. Nastavte hodnoty stínové na **10px 10px 5px #888**. Poté zadejte **border-radius** a Vložit fragment kódu. Typ **15px** nastavit velikost protokolu radius a stiskněte klávesu **ENTER**.

    ![Zaokrouhlí rozích s stínové](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "zaokrouhlené rozích s stínové")

    *Zaoblenými hranami s stínové*

    > [!NOTE]
    > V tuto chvíli je atribut stínové Vložit s odpovídající předpony (moz webkit, o) pro podporu standardu Mozilla a prohlížeče Webkit (Chrome, Safari, Konkeror).
7. Vytvořte novou třídu **div.images ul li img:hover** níže **div.images ul li img** definici třídy a umístěte kurzor do hranatých závorkách **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Typ **transformace** a stiskněte klávesu **KARTĚ** klíč dvakrát, aby bylo možné vložit fragment transformace. Potom zadejte **rotate(-15deg)** k provedení změny hodnota úhlu otočení, když se při přechodu myší bitové kopie.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Stiskněte klávesu **F5** ke spuštění řešení a přejděte na stránku CSS3. Všimněte si, že bitové kopie mít zaokrouhlené rozích a pole stínů. Najeďte myší obrázky a jejich otočit sledování.

    ![Transformace fragment otáčení bitovou kopii](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "transformace fragment otáčení obrázku")

    *Transformace fragment otáčení obrázku*

    > [!NOTE]
    > Pokud používáte Internet Explorer 10 a neuvidí stíny, zkontrolujte, zda že dokument režim je nastaven na IE10 standardů. Stiskněte klávesu **F12** otevření nástrojů pro vývojáře aplikace Internet Explorer a klikněte na tlačítko **režim dokumentu** změnit na IE10 standardů.

    ![o-nám](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Cvičení 2: Co je nového v editoru HTML

Visual Studio obsahuje vylepšené editor HTML. Některé z vylepšení obsažené v této verzi jsou inteligentní odsazení v dokumentech HTML, fragmenty HTML5, HTML počáteční a koncové značky odpovídající a ověřování jazyka HTML. Během tohoto cvičení uvidíte, jak tyto změny vylepšit vaše fluency při práci v kód webu.

Jako editor šablon stylů CSS je taky vylepšené HTML editor. Většina tato vylepšení jsou malé ty, které usnadňují životnosti Web developer. Věci jako další fragmenty kódu pro jazyk HTML5, inteligentní odsazení, když úpravy a ověření cílení dokumentu HTML DOCTYPE jsou některé z těchto vylepšení odpovídající počáteční a koncové značky.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Úloha 1 – ověření vylepšené DOCTYPE

HTML editor má teď možnost zkontrolovat DOCTYPE stránky, i když může být definici na hlavní stránce. HTML editor v závislosti na DOCTYPE stránky, vyhodnotí se správnou sadou pravidel a bude filtrovat seznam technologie IntelliSense s DOCTYPE elementy.

V této úloze se změní DOCTYPE stránky, pokud chcete zobrazit, jak se příslušným způsobem změní chování editor HTML.

1. Pokud ještě není otevřené, spusťte **Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení umístěný v **Source\WhatsNewASPNET** složky tohoto testovacího prostředí.
2. Otevřete **Site.Master** stránky.
3. Všimněte si je cílové schéma pro ověření panelu nástrojů. Způsob, jak se chová HTML editor (ověření, IntelliSense atd.) se změní správně podle Doctype vybrané.

    ![Použití Doctype v panelu nástrojů Úpravy zdrojového kódu HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "použití Doctype v panelu nástrojů Úpravy zdrojového kódu HTML")

    *Použití Doctype v panelu nástrojů Úpravy zdrojového kódu HTML*
4. Změňte schéma cílové na HTML 4.01.

    ![Změna Doctype v panelu nástrojů Úpravy zdrojového kódu HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "změna Doctype v panelu nástrojů Úpravy zdrojového kódu HTML")

    *Změna Doctype v panelu nástrojů Úpravy zdrojového kódu HTML*
5. Umístěte kurzor v části **textu** prvek a začněte psát název elementu HTML5 (například **video**). Všimněte si, že element není k dispozici v seznamu IntelliSense.

    ![Elementy jazyka HTML5 nejsou uvedené](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elementy, které nejsou uvedené")

    *Elementy jazyka HTML5 nejsou uvedené*
6. Vrátit zpět změny schématu cílové pro ověření nástrojů výběr DOCTYPE: XHTML5 z rozevíracího seznamu.

    ![Použití Doctype v panelu nástrojů Úpravy zdrojového kódu HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "použití Doctype v panelu nástrojů Úpravy zdrojového kódu HTML")

    *Resetování Doctype v panelu nástrojů Úpravy zdrojového kódu HTML*
7. Umístěte kurzor v části **textu** prvek a začněte psát HTML5 element znovu (například jako **video**). Všimněte si, že HTML5 elementy jsou nyní k dispozici v seznamu IntelliSense.

    ![Elementy jazyka HTML5 se uvedené](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "se uvedené prvky HTML5")

    *Probíhá uvedené prvky HTML5*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Úloha 2 – počáteční nebo koncové značky automatických aktualizací

Visual Studio nyní aktualizuje HTML otevírání nebo při zavření značky elementu, který upravujete vzájemně odpovídaly. Tato nová funkce bude zvýšení produktivity při úpravě značky HTML.

1. Na **Default.aspx** přidejte **H3** element s názvem (například Visual Studio 2012 skály!).


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Změna **H3** značky a typ **H2** nebo **H1.**

    Všimněte si, že koncová značka automaticky aktualizuje. Můžete také upravit koncovou značku, pokud chcete zobrazit, že počáteční značky se aktualizuje příliš.

    ![Automatická aktualizace koncová značka](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "automatické aktualizace koncová značka")

    *Automatická aktualizace koncová značka*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Úloha 3 – nové HTML5 fragmenty kódu

Visual Studio teď obsahuje několik fragmenty kódu jazyka HTML5. V této úloze budete používat některé z těchto fragmenty kódu.

1. Přidejte novou složku s názvem **zvuk** kořenové složky na webovém serveru. Otevřete Průzkumníka Windows a zkopírujte všechny zvukové soubory do **zvuk** složky **WhatsNewASPNET.sln** řešení.
2. V **Default.aspx** stránky, vyhledejte kurzor v části webu 11 skály!! Hlavička. Typ **zvuk** a stiskněte klávesu Tabulátor.

    Nový editor HTML zahrnuje fragmenty kódu pro obsah HTML5. Nezapomeňte použít správné definice DOCTYPE povolit fragmenty HTML5.

    ![Vkládání fragmenty kódu jazyka HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "vkládání HTML5 fragmenty kódu")

    *Vkládání HTML5 fragmenty kódu*
3. Aktualizujte zdroj zvuku tak, aby odkazoval na existující zvukový soubor.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Musíte přidat zvukový soubor k řešení.
4. Stiskněte klávesu **F5** přehrávání zvuku a spuštění tohoto webu.

    ![Spuštěním ovládacího prvku zvuk](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "systémem zvuk ovládacího prvku")

    *Spuštěním zvuk ovládacího prvku*

    > [!NOTE]
    > Můžete také zkusit další fragmenty zahrnuté v sadě Visual Studio, jako je například video, obrázek, atd.
5. Nyní zkuste vložení ovládacího prvku v některé části stránky. Zkuste například vložení **GridView** ovládací prvek, ale místo zadání  **&lt;Gri,** začněte psát  **&lt;GV**. Všimněte si, že IntelliSense seznamu jsou uvedeny **asp: GridView** ovládacího prvku.

    IntelliSense v editoru HTML teď poskytuje vyhledávací název-velká a malá písmena, jakož i částečné odpovídající (načítání všechny elementy, které obsahuje termín).

    ![Vkládání GridView s IntelliSense seznamy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "vkládání GridView pomocí IntelliSense seznamů")

    *Vkládání GridView pomocí IntelliSense seznamů*

    Pokud zadáte  **&lt;mřížky** zobrazí se všechny položky, které odpovídají termín, ale Visual Studio navrhne **gridview** ovládacího prvku:

    ![Vkládání GridView s částečnou shodu a seznamy IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "vkládání GridView s částečnou shodu a seznamy IntelliSense")

    *Vkládání GridView s částečnou shodu a seznamy IntelliSense*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Úloha 4 – HTML Editor inteligentní značky

Jiné zlepšování v editoru HTML je funkce inteligentních značek. Inteligentní značky usnadňují vývoj běžných nebo opakovaných úloh na základě-control. Tato funkce již byla k dispozici v Návrháři HTML, ale ne v editoru jazyka HTML.

1. Otevřete **Site.Master** a najděte **asp: nabídky** elementu. Umístěte kurzor na počáteční značce a Všimněte si, že malé glyfy zobrazí v dolní části elementu - kliknutím ho otevřete nabídce inteligentní úlohy. Všimněte si, že máte rychlý přístup k některé úlohy týkající se řízení nabídky.

    ![Inteligentní úlohy pro ovládací prvek nabídky](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "inteligentní úlohy pro ovládací prvek nabídky")

    *Inteligentní úlohy pro ovládací prvek nabídky*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Úloha 5: inteligentní odsazení

Jedním z doporučených postupů ve formátu HTML je odsazení vnořených elementů zachovat kód čitelné. V sadě Visual Studio 2012 si všimnete, že editor automaticky odsadí elementy při psaní kódu.

> [!NOTE]
> V předchozí verze sady Visual Studio, byl k dispozici inteligentní odsazení v editoru XML, ale není v editoru HTML.


1. Ujistěte se, že konfigurace Indenting na Editor HTML je nastavena na inteligentní odsazení. Chcete-li to udělat, vyberte **nástroje | Možnosti** nabídce a potom vyberte **textového editoru | HTML | Karty** stránky v levém podokně na obrazovce. Vyberte možnost Inteligentní odsazení.

    ![HTML Editor nastavení](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "nastavení editoru HTML")

    *Nastavení editoru HTML*
2. Na **Default.aspx** stránky, odeberte veškerý obsah v části audio element.
3. Umístěte kurzor na konci otevření **zvuk** element a stiskněte klávesu **ENTER**.

    Všimněte si, že nové pozice kurzoru má úroveň další odsazení.

    ![Inteligentní odsazení v editoru HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "inteligentní odsazení v editoru HTML")

    *Inteligentní odsazení v editoru HTML*
4. Obnovení zvuk značky s obsahem jste odstranili, nebo zavřete **Default.aspx** bez uložení změn.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Úloha 6 - Extract do uživatelského ovládacího prvku

Nástroje Refactoring zahrnuté v sadě Visual Studio, jako je například extrahování části kódu pro funkce, se skvělými funkcemi, které usnadňují zlepšení a refaktoring existující kód. Protějšku pro stránky ASP.NET by extrakce kódu HTML do uživatelského ovládacího prvku. Provádění modulu ručně zahrnovat několik kroků, jako je vytvoření uživatelského ovládacího prvku nové, přesunutí části kódu do uživatelského ovládacího prvku, registrace předponu značky uživatelského ovládacího prvku a, nakonec, vytváření instancí uživatelského ovládacího prvku na stránkách. Nyní, nové *extrahovat do uživatelského ovládacího prvku* nástroj automaticky provede tyto kroky pro vás.

V této úloze budete používat nové Extract kontextové operace uživatelský ovládací prvek vygenerujte nový uživatelský ovládací prvek z na vybraný úsek kódu.

1. Na **Default.aspx** vyberte **H2** a **zvuk** elementy.
2. Klikněte pravým tlačítkem a vyberte **extrahovat do uživatelského ovládacího prvku**.

    ![Extrahovat do uživatelského ovládacího prvku nabídky](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "extrahovat do uživatelského ovládacího prvku nabídky")

    *Extrahovat do uživatelského ovládacího prvku nabídky*
3. Zadejte název pro nový uživatelský ovládací prvek. Například **Jukebox.ascx**a potom klikněte na **OK**.

    ![Uložení extrahovaných uživatelského ovládacího prvku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "uložení extrahovaných uživatelského ovládacího prvku")

    *Uložení extrahovaných uživatelského ovládacího prvku*
4. Všimněte si, že jste extrahovali kód vybraný uživatelský ovládací prvek a byl nahrazen původní umístění pro vybraný úsek kódu s instancí nového uživatelského ovládacího prvku.

    ![Stránka automaticky aktualizovaly a používají nový uživatelský ovládací prvek](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "stránky se automaticky aktualizovaly a používají nový uživatelský ovládací prvek")

    *Stránka automaticky aktualizovaly a používají nový uživatelský ovládací prvek*
5. Stiskněte klávesu **F5** spuštění stránky a ověřte, že funguje ovládacího prvku.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Cvičení 3: Co je nového v editoru jazyka JavaScript

Zápis nebo úpravy kódu JavaScript není jednoduchý úkol, zejména v případě, že vaše aplikace spustí zvětšují a sami najít práci s dlouhé soubory a stovky funkce. Skript vývojáři obvykle mají nějakou práci navíc zachovat čitelnost kódu a přejděte v souborech. Se zahrnutím knihovny jazyka JavaScript jako jQuery stala skriptu navigační výzvy sám sebe z důvodu délka kódu.

Visual Studio se obnovuje v editoru jazyka JavaScript s promise, aby režim kódu, dostupné a uspořádány. V editoru jazyka JavaScript jsou teď implementuje řadu funkcí sady Visual Studio, které už existovalo v C# nebo VB editory: Přejít k definici, Automatické odsazení, dokumentace a ověření v případě, že jsou zápis. S prodlouženou platností seznam technologie IntelliSense máte katalogu funkce JavaScript na dosah ruky.

V tomto cvičení se dozvíte některé z nových funkcí a vylepšení JavaScript – editor. Bude procházet ukázkové soubory a zjistit všechny nové charakteristiky, které budou provedeny programování v jazyce JavaScript v rámci sady Visual Studio 2012 efektivnější.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Úloha 1 – nové funkce JavaScript editoru

Tato úloha vás seznámí s některé z nových funkcí editoru jazyka JavaScript, které se zaměřují na uspořádání kódu a přináší lepší uživatelské prostředí.

1. Pokud ještě není otevřené, spusťte **Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení umístěný v **Source\WhatsNewASPNET** složky tohoto testovacího prostředí.
2. Stiskněte klávesu **F5** ke spuštění aplikace, pak klikněte na odkaz JavaScript na navigačním panelu. Aktualizujte stránku několik časy a kontrola jak zvýší čítače.

    ![Čítač stránky](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "čítač stránky")

    *Čítač stránky*
3. Zavřete prohlížeč a přejděte zpátky na Visual Studio.
4. Otevřete **JavaScript.aspx** stránky a najděte  **&lt;skriptu&gt;**  blok (zobrazené dole).

    Následující kód používá místní úložiště HTML5 k uložení *pageLoadCount* proměnné, která ukládá počet, kolikrát má aktuální uživatel navštívil stránky. Místní úložiště je databáze klienta klíč hodnota, zavedl se standardem HTML5. Data budou uložena v místním počítači, do prohlížeče uživatele.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Zkontrolujte, zda že deklarace DOCTYPE nastavena na XHTML5 než budete pokračovat v dalších krocích.
5. Upravit kód a Všimněte si, že obsahuje IntelliSense pro JavaScript HTML5 funkce, jako místní úložiště a jejich vnitřní metody.

    ![Funkce HTML5 JavaScript v jazyce JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "funkce HTML5 JavaScript v jazyce JavaScript")

    *Funkce HTML5 JavaScript v jazyce JavaScript*
6. Klikněte na možnost žádné levá hranatá závorka (**{**) z skriptování kód a Všimněte si, že jsou vyznačené hranatých závorkách.

    ![Závorky jsou vyznačené](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "jsou vyznačené závorky")

    *Jsou vyznačené závorky*
7. Zrušením komentáře u funkce **testAutoAlign()** (Vyberte tři řádky, a můžete použít **CTRL** + **tisíc**; **CTRL** + **U**) a najděte kurzor uvnitř kód funkce. Stisknutím klávesy enter připojit druhý řádek. Všimněte si, že je kód **zarovnán** a **odsazeny automaticky**.

    ![Kód jazyka JavaScript je automaticky zarovnán](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "kódu jazyka JavaScript je automaticky zarovnán")

    *Kód jazyka JavaScript je automaticky zarovnán*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Úloha 2 – ověřování jazyka JavaScript

V této úloze bude zjišťovat nové ověření JavaScript pro standardní ECMAScript5. Tato funkce vám pomůže vytvořit kompatibilní kód JavaScript, při ukládání skriptování problémy před nasazením webu.

> [!NOTE]
> Visual Studio 2010 implementována ECMAStript3 dodržování předpisů, zatímco Visual Studio 2012 poskytuje ECMAScript5 dodržování předpisů.


1. Otevřete **ECMA5script5.js** umístěná **Scripts\custom** složce projektu. Nyní budete testovat ověřování pro ECMAScript5 standardní.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Můžete zkontrolovat &quot; **použití Striktního režimu** &quot; směr v první řádek souboru, který umožňuje ECMAScript5 **striktní režim**. Tento režim se skládá v podmnožině jazyk, který vysvětluje z posledních edici mnohoznačnosti a přidá některé nové funkce, jako je například metod getter a setter a podpora knihovny pro JSON a podrobnější reflexe na vlastnosti objektu.
2. Otevřete **seznam chyb** Pokud ještě není otevřené (**zobrazení** nabídky | **Seznam chyb**). Upozornění **funkce** deklarace jsou podtržené. Je to proto, že v ECMA5 standardní funkce nelze vnořit do struktury jazyka. V chybě bude seznamu můžete zobrazit podrobnosti upozornění.

    ![Chybovou zprávu ověření JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "chybovou zprávu ověření JavaScript")

    *Chybovou zprávu ověření JavaScript*
3. Komentář  **&quot;použití Striktního režimu&quot;**  směr a Všimněte si, že zmizí chyby, ale zůstal upozornění.
4. V poslední řádek souboru, zápis libovolný řetězec jako  **&quot;testování&quot;**  (včetně uvozovek to znamená, je jako řetězec). Zápis období vedle řetězec k zobrazení seznamu IntelliSense a vyberte **trim** možnost.

    Ve verzi ECMAScript5 standard řetězcové hodnoty a proměnné mají řetězcových metod, které jsou definované jako uvolnění dočasné paměti, velká písmena, vyhledávání a nahrazení.

    ![Seznam technologie IntelliSense v jazyce JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "seznam technologie IntelliSense v jazyce JavaScript")

    *Seznam technologie IntelliSense v jazyce JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Úloha 3 – dokumentace XML pro JavaScript

V této úloze zaměříte funkcích nástroje Visual Studio pro dokumentace XML v jazyce JavaScript. Uvidíte, že v seznamu JavaScript IntelliSense zobrazí dokumentace XML každé funkce. Kromě toho bude zjišťovat funkci navigace v jazyce JavaScript.

1. Otevřete **XMLDoc.js** soubor umístěný ve **skripty nebo vlastní** složce projektu. Tento soubor obsahuje dokumentace XML na každém z funkce jazyka JavaScript.

    ![Dokumentace JavaScript XML integrované technologie IntelliSense,](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "integrované dokumentace JavaScript XML IntelliSense")

    *Dokumentace JavaScript XML integrované technologie IntelliSense*
2. Pod **přidat** fungovat v **XMLDoc.js** souboru, vytvořte novou funkci s názvem **testování**.
3. V **testování** fungovat, volání **násobení** funkce, která přijímá dva parametry. Všimněte si pole popisu tlačítka se zobrazuje **násobení** funkce dokumentaci.

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![Dokumentace XML pro funkce jazyka JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "dokumentace XML pro funkce jazyka JavaScript")

    *Dokumentace XML pro funkce jazyka JavaScript*
4. Dokončení příkazu call funkce a typ *tečkou* o otevření seznamu IntelliSense pro vrácené hodnoty. Všimněte si, že je zjišťování sady Visual Studio **vrátit** hodnotu v dokumentaci, považuje hodnotu jako číslo.

    ![Dokumentace XML pro návratové typy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "dokumentace XML pro návratové typy")

    *Dokumentace XML pro návratové typy*
5. Nyní vložte volání pro přidání funkce. Všimněte si, že editoru JavaScript teď podporuje přetížení funkce. Když píšete název funkce, bude moci vyberte některé z dostupných přetížení zadané v dokumentaci.

    ![Dokumentace XML pro přetížení](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "dokumentace XML pro přetížení")

    *Dokumentace XML pro přetížení*
6. Otevřete **GotoDefinition.js** souborů a vyhledejte **$().html()** volání funkce. Vyhledejte kurzor na **html**.
7. Stiskněte klávesu **F12** a přejděte k definici. Všimněte si teď můžete přístup a procházet bez použití kódu jazyka JavaScript **najít** nástroj.
8. Vyhledejte ukazatele na instrukce jQuery před blok signatury na konci souboru kódu. Stiskněte klávesu **F12**. Bude přejděte na soubor knihovny jQuery. Všimněte si můžete taky přejít v souborech jQuery pomocí **F12**.

    ![Navigace na definice, jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "přejdete na definice jQuery")

    *Navigace na definice, jQuery*

> [!NOTE]
> Ujistěte se, že GotoDefinition.js neobsahuje chyby syntaxe před uložením souboru.


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Cvičení 4: Sdružování a Minifikace

Kolikrát své weby obsahují víc než jeden JavaScript nebo šablon stylů CSS souboru? Jde o velmi běžný scénář, kde sdružování a minimalizace může pomoci snížit velikost souboru a proveďte webu provádět rychleji. Nová funkce instalujícího v technologii ASP.NET 4.5 sady určitou sadu souborů JS nebo šablon stylů CSS do jednoho elementu a zmenší velikost podle zmenšování obsah (tj. odebrání není požadováno mezery, odebírání komentářů, snižuje identifikátory).

Sdružování a minimalizace v technologii ASP.NET 4.5 se provádí v době běhu, aby proces můžete identifikovat uživatelský agent (např. aplikace Internet Explorer, Mozilla atd.) a proto komprese zlepšit cílení na uživatele prohlížeče (například odebírání vy, je Mozilla konkrétní Když požadavek pochází z Internet Exploreru).

V tomto cvičení se dozvíte, jak povolit a používat různé typy sdružování a minimalizace v technologii ASP.NET 4.5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Úloha 1 – instalace sdružování a minimalizace balíček NuGet

1. Pokud ještě není otevřené, spusťte **Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení umístěný v **Source\WhatsNewASPNET** složky tohoto testovacího prostředí.
2. Otevřete konzolu Správce balíčků NuGet. Chcete-li to provést, použijte nabídku **zobrazení** | **ostatní okna** | **Konzola správce balíčků**.

    ![Otevírání file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole správce balíčku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "otevřením konzoly Správce balíčků")

    *Otevření konzoly Správce balíčků*
3. V **Konzola správce balíčků** typ **Install-Package Microsoft.Web.Optimization** a stiskněte klávesu **ENTER**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Úloha 2 – výchozí sady

Nejjednodušší způsob, jak používat sdružování a minimalizace je umožnit výchozí sady. Tato metoda používá konvence, abyste mohli odkazovat připojené a minifikovaný verze pro JS a šablon stylů CSS soubory ve složce.

V této úloze se dozvíte, jak povolit odkazovat připojené a minifikovaný soubory JS a šablon stylů CSS a zobrazit výsledný výstup.

1. Pokud ještě není otevřené, spusťte **Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení umístěný v **Source\WhatsNewASPNET** složky tohoto testovacího prostředí.
2. V **Průzkumníku řešení**, rozbalte **styly**, **Scripts\custom** a **Scripts\bundle** složky.

    Všimněte si, že aplikace používá více než jeden šablon stylů CSS a JS souboru.

    ![Soubory více předlohy se styly a JavaScript v aplikaci](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "soubory více předlohy se styly a JavaScript v aplikaci")

    *Více souborů předlohy se styly a JavaScript v aplikaci*
3. Otevřete **Global.asax.cs** souboru.

    Všimněte si, že nové **Microsoft.Web.Optimization** obor názvů je označeno jako komentář na začátku souboru. Zrušením komentáře u použití direktiva k obsahují sdružování a minimalizace funkce.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Vyhledejte **aplikace\_spustit** metoda.

    Tato metoda zrušte komentář u volání EnableDefaultBundles, jak je znázorněno v následujícím fragmentu. To umožňuje nám tak, aby odkazovaly připojené kolekce souborů CSS ve složce pomocí cesty ke složce, a &quot;šablon stylů CSS&quot; nebo &quot;JS&quot; příponu.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Otevřete **Optimization.aspx** souborů a vyhledejte obsahu ovládací prvek pro **HeadContent**.

    Všimněte si, že soubory šablon stylů CSS a soubory JS mít jedinou značku odkazované.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Tento kód je pro účely ukázky. V ideálním případě bude odkazovat sad v souboru Site.Master. V ukázkový kód zjistíte, že některé připojené soubory jsou také se na ně odkazovat soubor Site.Master provádění tohoto odkazu na poslední redundantní.
6. Všimněte si, že odkazy používají instalujícího názvů v **href** atribut k získání souborů CSS nebo JS ze styly a Scripts\custom složku v uvedeném pořadí.

    Cestu můžete použít **skripty nebo vlastní/JS** jak je uvedeno dále sady a všechny soubory JS uvnitř minifikaci **skripty nebo vlastní** složky. Toto je výchozí chování na výchozí sady.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Otevřete **Styles\Site.css** souboru.

    Všimněte si, že původní soubor CSS obsahuje zobrazují odsazené kód, mezery a komentáře, které zvětšit soubor. (Také soubor JavaScript obsahuje mezery a komentáře).

    ![Jeden z původní šablony stylů CSS soubory ve složce skripty](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "jeden z původní šablony stylů CSS soubory ve složce skripty")

    *Jeden z původní šablony stylů CSS souborů ve složce skripty*
8. Stiskněte klávesu **F5** spusťte aplikaci a přejít na **optimalizace** stránky.
9. Klikněte na **šablon stylů CSS sady** odkaz ke stažení a otevřete soubor.

    Projděte si soubor minifikovaný připojené. Si všimněte, že byly odstraněny všechny mezery, komentáře a odsazení znaky, generování menší soubor.

    ![Soubory šablon stylů CSS v sadě](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "soubory dodávat šablon stylů CSS")

    *Připojené soubory šablon stylů CSS*
10. Klikněte na tlačítko **JS sady** odkazu k otevření souboru JavaScript dodávat. Průzkumník upozornění můžete bezpečně ignorovat. Všimněte si, soubory jazyka JavaScript v části **vlastní** složky jsou taky spojeno dohromady a minifikovaný.

    ![JavaScript soubory v sadě](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "soubory dodávat JavaScript")

    *Připojené soubory jazyka JavaScript*

    Povolení komprese pro soubory šablon stylů CSS nebo JS bylo mnohem složitější v předchozí verzi technologie ASP.NET. Nyní, jako jste viděli, stačí přidat jeden řádek v *Global.asax* souboru povolit sdružování a připojené soubory pak odkazovat z vaší lokality.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Úloha 3 – statické sady

Statické sady přístupu umožňuje přizpůsobit sadu souborů sady, odkaz a minimalizaci metoda, která bude použita.

V této úloze nakonfigurujete sady statické definovat určitou sadu souborů sady a minifikaci.

1. Zavřete prohlížeč.
2. Otevřete **Global.asax.cs** souborů a vyhledejte **aplikace\_spustit** metoda.
3. Zrušte komentář kódu statických sady, jak je znázorněno v následujícím kódu.

    Definování statické sadu, která se bude odkazovat s &quot; **~/StaticBundle** &quot; virtuální cestou a použití **JsMinify** pro minimalizaci všechny zadané soubory s **AddFile** metoda. Nakonec přidáváte statické sada, která má **BundleTable** a jeho povolení.

    Všimněte si, že nejsou soubory umístěné na stejném místě; To je Další výhodou přes výchozí sdružování.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Otevřete **Optimization.aspx** souboru.

    Všimněte si, že odkaz na **statické JS sady** je pomocí cesty deklarujete při konfiguraci sady statické v souboru Global.asax.cs: **/StaticBundle**.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Stiskněte klávesu **F5** aplikaci spustit, a potom přejděte na **optimalizace** stránky.
6. Klikněte na **statické JS sady** odkazu k otevření souboru.

    Všimněte si, že minifikovaný dodávat soubor JavaScript je výstup pro všechny soubory JavaScript nakonfigurované v souboru statické sady v cestě &quot;/StaticBundle&quot;.

    ![Statické soubory sady JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "JavaScript statické soubory sady")

    *Statické soubory JavaScript sady*
7. Zavřete prohlížeč a vraťte se k sadě Visual Studio.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Úloha 4 – složka dynamické sady

V této úloze se dozvíte, jak nakonfigurovat dynamické složky sad. Dynamické sdružování je, že můžete zahrnout statické JavaScript, stejně jako ostatní soubory jazyky, které kompilovaný do jazyka JavaScript a proto vyžadovat některé zpracování předtím, než se spustí sdružování.

V tomto příkladu se dozvíte, jak používat **DynamicFolderBundle** třídy za účelem vytvoření dynamického sady pro soubory, které jsou napsané v CofeeScript. CofeeScript je programovací jazyk kompilovaný do jazyka JavaScript a nabízí jednodušší syntaxi pro psaní kódu jazyka JavaScript, rozšíření jako stručný výtah a čitelnost jazyce JavaScript.

1. Otevřete **Global.asax.cs** souborů a vyhledejte **aplikace\_spustit** metoda.
2. Zrušte komentář kódu dynamické sady, jak je znázorněno v následujícím kódu.

    Definování sady dynamické složky, který bude používat **CoffeeMinify** vlastní minimalizace procesor, které se vztahuje pouze na soubory s &quot; **.coffee** &quot; (rozšíření CoffeeScript soubory). Všimněte si, že vzor hledání můžete vybrat soubory sady ve složce, jako je třeba se\*.coffee'.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Otevřete konzolu Správce balíčků NuGet. Chcete-li to provést, použijte nabídku **zobrazení** | **ostatní okna** | **Konzola správce balíčků**.
4. V **Konzola správce balíčků** typ **Install-Package CoffeeSharp** a stiskněte klávesu **ENTER**.
5. Klikněte **zobrazit všechny soubory** tlačítka na **Průzkumníku řešení** okno

    ![Zobrazení všech souborů](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "zobrazení všech souborů")

    *Zobrazení všech souborů*
6. Klikněte pravým tlačítkem **CoffeeMinify.cs** v soubor **Průzkumníku řešení** a vyberte **zahrnout do projektu**

    ![Zahrnout soubor CoffeeMinify.cs v projektu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "zahrnout soubor CoffeeMinify.cs v projektu")

    *Zahrnout soubor CoffeeMinify.cs v projektu*
7. Otevřete **CoffeeMinify.cs** souboru.

    Tato třída dědí od JsMinify k minifikaci vyplývající z CoffeeScript kompilace kódu JavaScript výstup. Zavolá CoffeeScript kompilátoru nejprve generovat kód jazyka JavaScript a pak se odešle do metodu JsMinify.Process k minifikaci výsledný kód.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Otevřete **Script1.coffee** a **Script2.coffee** souborů z **skripty nebo sady** složky.

    Tyto soubory bude obsahovat kód CoffeScript sestavují při provádění sdružování pomocí třídy CoffeeMinify.

    Pro účely jednoduchost jsou soubory CoffeeScript zadat pouze včetně CoffeeScript ukázkový kód. Komentáře jsou vyloučeny JsMinify procesem.

    ![Soubory CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "soubory CoffeeScript")

    *Soubory CoffeeScript*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) nabízí jednodušší syntaxi pro psaní kódu jazyka JavaScript, rozšíření jako stručný výtah a čitelnost jazyce JavaScript, jakož i přidávání dalších funkcí jako pole míru porozumění a porovnávání vzorů.
9. Otevřete **Optimization.aspx** souboru a najděte sady odkazy.

    Všimněte si, že odkaz na **dynamické sady JS** odkazuje **skripty nebo sady** složky pomocí **/kávy** přípon, které jste nakonfigurovali pro sadu složek dynamické.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Stiskněte klávesu **F5** aplikaci spustit, a potom přejděte na **optimalizace** stránky.
11. Klikněte na **dynamické sady JS** odkazu k otevření vygenerovaný soubor.

    Všimněte si, že obsahuje pouze obsah, který je zahrnutý v této sadě **.coffee** soubory. Můžete také zjistí, že kód CoffeeScript byl zkompilován pro jazyk JavaScript a řádky komentované byla odebrána.

    ![Dynamické soubory JS sady](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "dynamické JS soubory sady")

    *Dynamické sady soubory JS*

> [!NOTE]
> Kromě toho můžete nasadit tuto aplikaci do následující weby systému Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu](#AppendixB).


<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Toto testovací prostředí vám umožní pochopit, co je nového v technologii ASP.NET a vývoj webů v sadě Visual Studio 2012 a jak využívat řadu vylepšení v sadě Visual Studio 2012.

Provedením tohoto testovacího prostředí Hands-On mít dozvědět, jak používat nové funkce a vylepšení v aplikaci Visual Studio 2012 editory pro šablon stylů CSS, JavaScript a HTML. Kromě toho mají dozvědět, jak Visual Studio 2012 implementuje předdefinované sdružování a minimalizace.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze  **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; *Visual Studio Express 2012 pro Web se sadou Windows Azure SDK*&quot;.
2. Klikněte na **nyní nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.

    ![Nainstalovat Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "nainstalovat Visual Studio Express")

    *Nainstalovat Visual Studio Express*
4. Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Vyjádření souhlasu s podmínkami licence](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Vyjádření souhlasu s podmínkami licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Průběh instalace*
6. Po dokončení instalace, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.

    ![VS Express pro Web dlaždice](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *VS Express pro Web dlaždice*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu

Tento dodatek vám ukáže, jak vytvořit nový web z portálu Windows Azure Management Portal a publikovat aplikace, kterou jste získali podle testovacím prostředí, využívat výhod nasazení webu publikování funkce poskytované služby Windows Azure.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Úloha 1 – Vytvoření nového webu ze systému Windows Azure Portal

1. Přejděte na [Windows Azure Management Portal](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů společnosti Microsoft, které jsou spojené s vaším předplatným.

    > [!NOTE]
    > S Windows Azure můžete bezplatné hostování 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu. Můžete si zaregistrovat [zde](http://aka.ms/aspnet-hol-azure).

    ![Přihlaste se k portálu Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Přihlaste se k portálu Windows Azure")

    *Přihlaste se k Windows Azure Management Portal*
2. Klikněte na tlačítko **nový** na panelu příkazů.

    ![Vytvoření nového webu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "vytváření nového webu")

    *Vytvoření nového webu*
3. Klikněte na tlačítko **výpočetní** | **webu**. Potom vyberte **rychle vytvořit** možnost. Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvoření webu**.

    > [!NOTE]
    > Web systému Windows Azure je hostitel pro spouštění v cloudu, ve kterém můžete řídit a spravovat webovou aplikaci. Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál. Postup pro nastavení databáze neobsahuje.

    ![Vytvoření nového webu pomocí metody rychlého vytvoření](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "vytváření nového webu pomocí metody rychlého vytvoření")

    *Vytvoření nového webu pomocí metody rychlého vytvoření*
4. Počkejte, dokud nové **webu** je vytvořena.
5. Po vytvoření webu klikněte na odkaz v části **URL** sloupce. Zkontrolujte, zda je funkční nový web.

    ![Procházení na nový web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "procházení na nový web")

    *Procházení na nový web*

    ![Webový server spuštěn](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "webu systémem")

    *Spuštění webu*
6. Přejděte zpět na portál a klikněte na název webu v části **název** sloupec pro zobrazení stránky pro správu.

    ![Otevření stránky Správa webu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "otevření stránek správu webového serveru")

    *Otevření stránek správu webového serveru*
7. V **řídicí panel** v části **rychlého přehledu** klikněte na položku **stažení profilu publikování** odkaz.

    > [!NOTE]
    > *Profilu publikování* obsahuje všechny informace požadované pro publikování webové aplikace na web služby Windows Azure pro každou metodu povoleno publikace. Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a řetězců databází, které jsou potřebné k připojení k a ověřování na základě těchto koncových bodů, pro které je metoda publikace povolena. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pro Web** a **sadu Microsoft Visual Studio 2012** podporu čtení publikační profily k automatické konfiguraci těchto programů pro publikování webové aplikace na weby služby Windows Azure.

    ![Na webu stažení profilu publikování](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "stahování webové stránky profilu publikování")

    *Na webu stažení profilu publikování*
8. Stáhněte si soubor profil publikování do vhodného umístění. Dále v tomto cvičení uvidíte jak používat tento soubor k publikování webové aplikace na weby systému Windows Azure ze sady Visual Studio.

    ![Ukládání souboru profilu publikování](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "ukládání profilu publikování")

    *Ukládání souboru profilu publikování*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Úloha 2 – konfigurování serveru databáze

Pokud vaše aplikace využívá systému SQL Server, databáze, budete muset vytvořit databázi SQL server. Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá systém SQL Server může tuto úlohu přeskočit.

1. Budete potřebovat databázi SQL serveru pro ukládání databázi aplikace. Databáze SQL servery můžete zobrazit ze svého předplatného na portál Windows Azure Management **databází Sql** | **servery** | **serveru Řídicí panel**. Pokud nemáte server vytvořeno, můžete vytvořit jeden pomocí **přidat** tlačítka na panelu příkazů. Poznamenejte si **název serveru a adresa URL, správce přihlašovací jméno a heslo**, jako je použijete v další úkoly. Nevytvářejte databáze ještě, jak bude vytvořen v pozdější fázi.

    ![Řídicí panel serveru databáze SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "řídicího panelu serveru databáze SQL")

    *Řídicí panel serveru databáze SQL*
2. V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio, proto je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**. To lze provést, klikněte na tlačítko **konfigurace**, vyberte IP adresu z **aktuální IP adresa klienta** a vkládání na **počáteční IP adresa** a **Koncová IP adresa** textových polí. Zadejte název pravidla a klikněte ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) tlačítko.

    ![Přidávání IP adresy klienta](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Přidávání IP adresy klienta*
3. Jednou **IP adresa klienta** je povolené IP adresy do seznamu, klikněte na **Uložit** potvrďte změny.

    ![Potvrzení změn](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Potvrzení změn*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu

1. Přejděte zpět na ASP.NET MVC 4 řešení. V **Průzkumníku řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.

    ![Publikování aplikace](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "publikování aplikace")

    *Publikování webu*
2. Umožňuje naimportujte profil publikování, který jste uložili v první úloze.

    ![Import profilu publikování](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "import profilu publikování")

    *Import profilu publikování*
3. Klikněte na tlačítko **ověření připojení**. Po dokončení ověření klikněte na tlačítko **Další**.

    > [!NOTE]
    > Ověření je hotová, jakmile se zobrazí zelené zaškrtnutí zobrazí vedle tlačítko ověřit připojení.

    ![Ověření připojení](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "ověřování připojení")

    *Ověření připojení*
4. V **nastavení** v části **databáze** části, klikněte na tlačítko vedle připojení databáze textové pole (tj. **objekt DefaultConnection**).

    ![Konfigurace nasazení webu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "konfigurace nasazení webu")

    *Konfigurace nasazení webu*
5. Připojení k databázi nakonfigurujte následujícím způsobem:

    - V **název serveru** zadejte vaše databáze SQL serveru adresu URL pomocí *tcp:* předponu.
    - V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.
    - V **heslo** zadejte přihlašovací heslo správce serveru.
    - Zadejte nový název databáze, například: *MVC4SampleDB*.

    ![Konfigurace cílový připojovací řetězec](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "konfigurace cílový připojovací řetězec")

    *Konfigurace cílový připojovací řetězec*
6. Pak klikněte na tlačítko **OK**. Po zobrazení výzvy k vytvoření databáze, klikněte na tlačítko **Ano**.

    ![Vytvoření databáze](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "vytváření řetězec databáze")

    *Vytvoření databáze*
7. Připojovací řetězec, který budete používat pro připojení k databázi SQL v systému Windows Azure je uvedené v rámci textbox výchozí připojení. Pak klikněte na tlačítko **Další**.

    ![Připojovací řetězec odkazující na databázi SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "připojovací řetězec odkazující na databázi SQL")

    *Připojovací řetězec odkazující na databázi SQL*
8. V **Preview** klikněte na tlačítko **publikovat**.

    ![Publikování webové aplikace](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "publikování webové aplikace")

    *Publikování webové aplikace*
9. Jakmile proces publikování dokončí, otevře se výchozí prohlížeč publikované webové stránky.

    ![Aplikace publikována do služby Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "aplikace publikována do služby Windows Azure")

    *Aplikace, které jsou publikovány do služby Windows Azure*
