---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Co je nového v ASP.NET a webového vývoje v sadě Visual Studio 2012 | Dokumentace Microsoftu
author: rick-anderson
description: Nová verze sady Visual Studio přináší celou řadu vylepšení, zaměřuje na vylepšení možnosti a výkon při práci s technologiemi Web...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 474df5c8e2cee820a3bdd80ba45e6504a025cdf1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823605"
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>Co je nového v ASP.NET a webového vývoje v sadě Visual Studio 2012
====================
podle [Campy Web týmu](https://twitter.com/webcamps)

> Nová verze sady Visual Studio přináší celou řadu vylepšení, zaměřuje na vylepšení možnosti a výkon při práci s technologiemi Web. Visual Studio editory pro šablony stylů CSS, JavaScript a HTML byl zcela přepracované zahrnout mnoho nejvíce vyžádání kód podpory, jako je například technologie IntelliSense a Automatické odsazení. Z hlediska výkonu sdružování a minifikace jsou teď integrované jako dobu načítání integrované funkce, které chcete jednoduše redukovat stránky.
> 
> Visual Studio umožňuje pracovat s nejnovějšími technologiemi Web. Abyste měli jistotu, že váš web fungovat bez ohledu na platformu klienta a využití nové funkce a prvky HTML5 můžete fragmenty CSS3 prohlížečů.
> 
> Psaní a profilování kódu JavaScript by mělo být jednodušší s touto verzí sady Visual Studio. Seznamy IntelliSense, integrované funkce XML dokumentace a navigace jsou teď k dispozici pro kód jazyka JavaScript. Teď máte katalogu JavaScript na dosah ruky. Kromě toho můžete zkontrolovat dodržování předpisů ECMAScript5 pomocí skriptů a zjistit chyby syntaxe v rané fázi.
> 
> Poslední, ale ne alespoň tato verze sady Visual Studio implementuje integrované sdružování a minifikace. Soubory skriptů a stylů budou balené a komprimované tak, aby lokality provádí rychleji.
> 
> Tato laboratoř vás provede vylepšení a nových funkcí popsaných dříve použitím menší změny na ukázkovou webovou aplikaci ve zdrojové složce k dispozici.
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto praktická cvičení se dozvíte, jak:

- Používat nové funkce a vylepšení v editoru stylů CSS
- Používat nové funkce a vylepšení v editoru HTML
- Používat nové funkce a vylepšení v editoru jazyka JavaScript
- Konfigurace a používání sdružování a minifikace

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

- [Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny k jeho instalaci).
- [Prostředí Windows PowerShell](https://support.microsoft.com/kb/968930/) (pro skripty instalace – na Windows 8 a Windows Server 2008 R2 nainstalovaná)
- [Aplikace Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - nebo prohlížeč kompatibilní HTML5

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Tato praktická cvičení zahrnuje následující praktická cvičení:

1. [Cvičení 1: Co je nového v editoru stylů CSS](#Exercise1)
2. [Cvičení 2: Co je nového v editoru HTML](#Exercise2)
3. [Cvičení 3: Co je nového v editoru jazyka JavaScript](#Exercise3)
4. [Cvičení 4: Sdružování a Minifikace](#Exercise4)

Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Cvičení 1: Co je nového v editoru stylů CSS

Weboví vývojáři měli seznámit s mnoha problémy související s úpravy šablon stylů CSS. Jedním z největších problémů stylu CSS se kompatibility prohlížečů. Často dochází, že po aplikování stylů na váš web, si všimnete, že vypadá jinak Pokud otevřete v jiném prohlížeči nebo zařízení. Proto může trávit mnoho času na řešení těchto problémů visual si uvědomit, že pokud provedete nakonec pracovat v jeden prohlížeč, to je v fungovat, ostatní.

Visual Studio nyní zahrnuje funkce, které pomáhá vývojářům přístup, práce a efektivní uspořádání šablony stylů CSS. Během tohoto cvičení bude vyhovovat nové funkce pro organizaci efektivní a edition, stejně jako CSS3 fragmenty kódu pro kompatibilitu prohlížečů.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Úloha 1 – nové funkce editoru

V této úloze bude zjišťovat nové funkce editoru šablon stylů CSS. Tento nový editor vám pomůže zvýšit produktivitu s využitím novou inteligentní odsazení, komentáře vylepšení kódu a vylepšené technologie IntelliSense seznam.

1. Spustit **sady Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení nachází v **Source\WhatsNewASPNET** složka tohoto testovacího prostředí.
2. V Průzkumníku řešení otevřete **Site.css** soubor umístěný **styly** složky. Ujistěte se, že **textový Editor** nástroje jsou viditelné na panelu nástrojů. Chcete-li to mohli udělat, vyberte **zobrazení** | **panely nástrojů** možnost nabídky a zkontrolujte **textový Editor** možnosti. Uvidíte, že od této nové verzi **komentář** tlačítko (![komentář – tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) a **zrušit komentář** tlačítko (![zrušte komentář – tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)), také dostupných pro editor šablon stylů CSS.

    ![Povolení nástroje šablon stylů CSS a Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "povolení editoru a nástroje pro šablony stylů CSS")

    *Povolení nástroje šablon stylů CSS a Editor*
3. Posunout kód a vybrat všechny definice třídy šablony stylů CSS. Klikněte na tlačítko **komentář** (![komentář – tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) tlačítko vyjádřit vybrané řádky. Klikněte **zrušit komentář** (![Odkomentujte tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) tlačítko pro vrácení změn.
4. Klikněte na tlačítko **sbalit** (![sbalit](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) a **Rozbalit** (![rozbalte](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) tlačítka umístěny na levý okraj textu. Všimněte si, že teď můžete skrýt styly, které nepoužíváte čisticího modulu zobrazení.

    ![Sbalení tříd CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "tříd CSS sbalení")

    *Sbalení třídy šablony stylů CSS*
5. Ujistěte se, že je povolena funkce Inteligentní odsazení. Vyberte **nástroje** | **možnosti** nabídky a pak vyberte **textový Editor** | **šablon stylů CSS**  |  **Formátování** stránky v levém podokně na obrazovce. Zkontrolujte, **hierarchické odsazení** možnost.

    ![Povolení hierarchické odsazení](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "povolení hierarchické odsazení")

    *Povolení hierarchické odsazení*
6. Vyhledejte definici hlavní třídy (.main) a doplňovací style pro prvky elementů div. Můžete si všimnout, že kód zarovná automaticky, kterým pomůže uživatelům najít nadřazené třídy na první pohled.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Hierarchické zarovnání v jazyce CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "hierarchické zarovnání v jazyce CSS")

    *Hierarchické zarovnání v jazyce CSS*
7. Uvnitř **.main div** třídy, vyhledejte kurzor na konci **ohraničení: 0px;** a stiskněte klávesu **Enter** zobrazíte seznam technologie IntelliSense. Začněte psát **horní** a Všimněte si, jak v seznamu vyfiltrují během psaní. V seznamu budou uvedeny prvky, které obsahují **horní** v jakékoli části slova (v předchozích verzích sady Visual Studio, v seznamu vyfiltrují podle položky, které *začít* s označením).

    ![Vylepšení technologie IntelliSense v jazyce CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "vylepšení technologie IntelliSense v jazyce CSS")

    *Vylepšení technologie IntelliSense v jazyce CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Úloha 2 – Výběr barvy

V této úloze bude zjišťovat, že nový výběr barvy šablon stylů CSS integrována s IntelliSense ve Visual Studio.

1. V **Site.css,** vyhledejte definici třídy záhlaví (.header) a umístěte kurzor vedle **barvu pozadí** atribut mezi &quot;:&quot; a &quot; # &quot; znaků na daném řádku kódu **.**

    ![Vyhledání kurzor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "umístění kurzoru")

    *Umístění kurzoru*
2. Odstranit **dvojtečka** (:) a zapište ho znovu zobrazíte volby barev. Všimněte si, že první barvy, které se nejčastěji se vyskytujících barvy vašeho webu. Pokud kliknete na bílou barvu, jeho kód HTML (#fff) nahradí aktuální kód barvy v šabloně stylů.

    ![Výběr barvy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "výběr barvy")

    *Výběr barvy*
3. Stisknutím klávesy **Rozbalit** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) tlačítko pro výběr barvy a zobrazte barvy, přesuňte kurzor přechodu na jinou barvu. Potom klikněte na tlačítko **kapátko** tlačítko a vyberte libovolnou barvu z obrazovky. Všimněte si, že hodnota barvy pozadí se dynamicky mění při přesunutí kurzoru.

    ![Přechod pro výběr barev](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "přechod pro výběr barev")

    *Přechod pro výběr barev*
4. V **krytí** posuvník, přesunout do středu panelu ke snížení krytí modulu pro výběr. Všimněte si, že hodnota barvy pozadí nyní změní měřítko na RGBA.

    ![Výběr barvy krytí](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "výběr barvy krytí")

    *Výběr barvy krytí*

    > [!NOTE]
    > Definice barvy RGBA (červená, zelená, modrá, alfa) ve specifikaci CSS3 umožňuje definovat hodnotu neprůhlednosti barvy pro jednu položku. Na rozdíl od **krytí -** podobně jako atribut CSS **-** RGBA barvy budou také kompatibilní s nejnovější prohlížeče.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Úloha 3 – fragmenty kódu kompatibilní šablon stylů CSS

V této úloze se dozvíte, jak používat napříč prohlížeči kompatibilní CSS3 fragmenty kvůli implementaci některé funkce na vašem webu.

1. V **Site.css** souboru, vyhledejte **záhlaví** šablon stylů CSS třídy definice (.header) a umístěte kurzor na slovo níže **/ \*poloměr ohraničení\* /** zástupný symbol pro přidání nové fragment kódu. Stisknutím klávesy **Enter** zobrazíte seznam technologie IntelliSense a typ **radius** pro filtrování seznamu. Vyberte **border-radius** možnost ze seznamu poklikejte na a pak stiskněte klávesu **kartu** klíče pro vložení fragmentu kódu. Zadejte velikost protokolu radius v pixelech a stiskněte klávesu **Enter**. Například zadejte **15px**.

    Vykreslí zakulacený ohraničení u většiny prohlížečů HTML5 dodržování předpisů, včetně, Mozilla a na základě WebKit prohlížeče se CSS3 atributy přidané aplikací fragmentu kódu.

    ![Pomocí fragmentu kódu border-radius](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "pomocí fragmentu kódu border-radius")

    *Pomocí fragmentu kódu border-radius*
2. Použít stejné **ohraničení** fragmenty kódu ve stylu stránky (.page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Stisknutím klávesy **F5** ke spuštění řešení. Všimněte si, že každá stránka nyní zaoblené ohraničení.

    ![Zaoblené rohy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "zaoblené rohy")

    *Zaoblení rohů*
4. Zavřete prohlížeč a vraťte se do sady Visual Studio.
5. Otevřít **Custom.css** soubor umístěný **styly** složky a umístěte kurzor mezi **div.images ul li img** definici třídy.
6. Stisknutím klávesy enter zobrazte seznam technologie IntelliSense, typ **box-shadow** a stiskněte klávesu **kartu** klíč dvakrát pro vložení fragmentu kódu stínové výchozí uvnitř definice třídy. Nastavte hodnoty stínové na **10px 10px 5px #888**. Potom zadejte **border-radius** a Vložit fragment kódu. Typ **15px** nastavit velikost protokolu radius a stiskněte klávesu **ENTER**.

    ![Zaoblené rohy se stínovou](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "zaoblené rohy se stínovou")

    *Zaoblené rohy se stínovou*

    > [!NOTE]
    > V tomto okamžiku je vložen atribut stínové s odpovídající předpony (moz komponenty webkit, o) pro podporu Mozilla a prohlížeče komponenty Webkit (Chrome, Safari, Konkeror).
7. Vytvořte novou třídu **div.images ul li img:hover** níže **div.images ul li img** definici třídy a umístěte kurzor dovnitř závorek **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Typ **transformace** a stiskněte klávesu **kartu** klíč dvakrát, aby bylo možné vložit fragment kódu transformace. Potom zadejte **rotate(-15deg)** změnit hodnota úhlu otočení při bitové kopie jsou při přechodu myší.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Stisknutím klávesy **F5** ke spuštění řešení a přejděte na stránku CSS3. Všimněte si, že mají zaoblené rohy a pole stíny imagí. Najeďte myší obrázky a podívejte se na nich otočit.

    ![Transformace fragmentu otočení obrázku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "fragment kódu transformace otočení obrázku")

    *Transformace fragmentu otočení obrázku*

    > [!NOTE]
    > Pokud používáte Internet Explorer 10 a nevidí stínů, ujistěte se, že je nastaven režimu dokumentů standardy aplikace Internet Explorer 10. Stisknutím klávesy **F12** otevřete vývojářské nástroje aplikace Internet Explorer a klikněte na tlačítko **režim dokumentu** změnit standardy aplikace Internet Explorer 10.

    ![o-USA](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Cvičení 2: Co je nového v editoru HTML

Visual Studio poskytuje Vylepšený editor HTML. Některá vylepšení zahrnutá v této verzi jsou inteligentní odsazení dokumentů HTML, fragmenty kódu HTML5, HTML start a odpovídající koncové značky a ověřováním HTML. Během tohoto cvičení uvidíte, jak tyto změny vylepšit vaše fluency při práci v kódu na webu.

Také jsme vylepšili editoru HTML, jako editor šablon stylů CSS. Většina tato vylepšení jsou malé ty, které usnadňují života Web developer. Věci jako další fragmenty kódu pro HTML5, inteligentní odsazení při úpravách a ověření cílení na dokument DOCTYPE v HTML jsou některá z těchto vylepšení odpovídající počáteční a koncovou značku.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Úloha 1 – vylepšené DOCTYPE ověření

HTML editor má teď možnost zkontrolovat deklarace DOCTYPE stránky, i když definice může být na stránce předlohy. HTML editor v závislosti na deklarace DOCTYPE stránky, ověří se správnou sadou pravidel a bude filtrovat seznam technologie IntelliSense, vzhledem k tomu prvky DOCTYPE.

V této úloze se změní deklarace DOCTYPE stránky, pokud chcete zobrazit, jak se příslušným způsobem mění chování editoru HTML.

1. Pokud ještě není otevřen, začněte **sady Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení nachází v **Source\WhatsNewASPNET** složka tohoto testovacího prostředí.
2. Otevřít **Site.Master** stránky.
3. Všimněte si, že na cílové schéma pro ověření nástrojů. Deklarace Doctype vybrané se k správně změní způsob, jakým se chová editoru HTML (ověření, technologie IntelliSense atd.).

    ![Používat typ Doctype v úpravy zdrojového kódu HTML nástrojů](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "použití Doctype v panelu nástrojů Úpravy zdrojového kódu HTML")

    *Používat typ Doctype v panelu nástrojů Úpravy zdrojového kódu HTML*
4. Změna cílové schéma do formátu HTML 4.01.

    ![Změna Doctype v úpravy zdrojového kódu HTML nástrojů](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "změna Doctype v panelu nástrojů Úpravy zdrojového kódu HTML")

    *Změna Doctype v panelu nástrojů Úpravy zdrojového kódu HTML*
5. Umístěte kurzor na slovo v rámci **tělo** prvek a začněte psát název HTML5 elementu (například **videa**). Všimněte si, že prvek není k dispozici v seznamu technologie IntelliSense.

    ![Nejsou uvedeny prvky HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "nejsou uvedeny prvky HTML5")

    *Nejsou uvedeny prvky HTML5*
6. Vrátit zpět změny na cílové schéma pro ověření nástrojů vybrat typ dokumentu: XHTML5 z rozevíracího seznamu.

    ![Používat typ Doctype v úpravy zdrojového kódu HTML nástrojů](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "použití Doctype v panelu nástrojů Úpravy zdrojového kódu HTML")

    *Resetovat Doctype v panelu nástrojů Úpravy zdrojového kódu HTML*
7. Umístěte kurzor na slovo v rámci **tělo** prvek a začněte psát HTML5 elementu znovu (třeba jako **videa**). Všimněte si, že prvky HTML5, jsou teď dostupné v seznamu technologie IntelliSense.

    ![Prvky HTML5 zobrazovaly](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "zobrazovaly prvky HTML5")

    *Prvky HTML5 zobrazovaly*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Úloha 2 – začátek/konec značky automatických aktualizací

Visual Studio nyní aktualizuje HTML otevření nebo zavření značky elementu, který upravujete vzájemně odpovídaly. Tato nová funkce budou zvyšovat produktivitu při úpravách značky HTML.

1. Na **Default.aspx** stránce, přidejte **H3** element s názvem (například Visual Studio 2012 Rocks!).

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Změnit **H3** značku a typ **H2** nebo **H1.**

    Všimněte si, že automaticky aktualizuje koncová značka. Můžete také upravit koncovou značku, pokud chcete zobrazit, že počáteční značce odpovídajícím způsobem aktualizuje příliš.

    ![Automatická aktualizace koncovou značku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "automatických aktualizací koncová značka")

    *Automatická aktualizace koncová značka*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Úloha 3 – nové fragmenty kódu HTML5

Visual Studio teď obsahuje několik fragmentů kódu HTML5. V této úloze budete používat některé fragmenty kódu.

1. Přidat novou složku s názvem **zvuku** do kořenové složky webu. Otevřete Průzkumníka Windows a zkopírujte do jakékoli zvukový soubor **zvuku** složky **WhatsNewASPNET.sln** řešení.
2. V **Default.aspx** stránky, vyhledejte kurzor v rámci webu 11 Rocks! Záhlaví. Typ **zvuku** a stiskněte klávesu Tabulátor.

    Nový editor HTML obsahuje fragmenty kódu pro HTML5 obsah. Nezapomeňte použít správné definice DOCTYPE umožňující fragmenty kódu HTML5.

    ![Vkládají se fragmenty kódu HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "vkládají se fragmenty kódu HTML5")

    *Vkládají se fragmenty kódu HTML5*
3. Aktualizace zdroje zvuku tak, aby odkazovala na existující zvukový soubor.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Je potřeba přidat zvukový soubor k řešení.
4. Stisknutím klávesy **F5** spuštění tohoto webu a přehrávání zvuku.

    ![Spuštěním ovládacího prvku zvuk](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "spuštěním ovládacího prvku zvuku")

    *Spuštěním ovládacího prvku zvuku*

    > [!NOTE]
    > Můžete také zkusit víc fragmentů kódu, které jsou zahrnuty v sadě Visual Studio, jako jsou videa, obrázek, atd.
5. Teď si vyzkoušejte vložíte ovládací prvek v některé části stránky. Například, pokuste se vložit **GridView** ovládacího prvku, ale místo zadání  **&lt;mřížky,** začněte psát  **&lt;GV**. Všimněte si, že v seznamu technologie IntelliSense se zobrazí **asp: GridView** ovládacího prvku.

    Technologie IntelliSense v editoru HTML nyní poskytuje vyhledávání – název malých a velkých písmen, stejně jako částečné odpovídající (načítání všechny elementy, které obsahuje výraz).

    ![Vložení prvku GridView se seznamy IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "vkládání prvek GridView s seznamy IntelliSense")

    *Vložení prvku GridView se seznamy IntelliSense*

    Pokud zadáte  **&lt;mřížky** se zobrazí všechny položky, které odpovídají termín, ale Visual Studio navrhne **gridview** ovládacího prvku:

    ![Vkládání prvek GridView s částečnou shodu a seznamy IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "vkládání prvek GridView s částečnou shodu a seznamy IntelliSense")

    *Vkládání prvek GridView s částečnou shodu a seznamy IntelliSense*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Úloha 4 – HTML Editor inteligentní značky

Další vylepšení v editoru HTML je funkce inteligentních značek. Inteligentní značky umožňují snadno provádět úlohy běžné nebo opakované vývoje na základě-control. Tato funkce už je k dispozici v Návrháři HTML, ale ne v editoru jazyka HTML.

1. Otevřít **Site.Master** a vyhledejte **nabídku asp:** elementu. Umístěte kurzor na počáteční značce a Všimněte si, že malé piktogram zobrazí v dolní části elementu - kliknutím ho otevřete v nabídce inteligentních úlohy. Všimněte si, že máte rychlý přístup k některé úkoly související s ovládací prvek nabídky.

    ![Inteligentní úlohy pro ovládací prvek nabídky](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "inteligentní úlohy pro ovládací prvek nabídky")

    *Chytré úkoly pro ovládací prvek nabídky*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Úloha 5: inteligentní odsazení

Jeden z osvědčených postupů ve formátu HTML je odsazení vnořené prvky, které mají být kód čitelné. V sadě Visual Studio 2012 můžete si všimnout, že v editoru automaticky odsadí prvky při psaní kódu.

> [!NOTE]
> V předchozí verzi sady Visual Studio, inteligentní odsazení byla k dispozici v editoru XML, ale ne v editoru jazyka HTML.


1. Ujistěte se, že konfigurace Indenting v editoru HTML je nastavena na inteligentní odsazení. Chcete-li to mohli udělat, vyberte **nástroje | Možnosti** nabídky a pak vyberte **textový Editor | HTML | Karty** stránky v levém podokně na obrazovce. Vyberte možnost Inteligentní odsazení.

    ![Nastavení editoru HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "nastavení editoru HTML")

    *Nastavení editoru HTML*
2. Na **Default.aspx** stránce, odstranit veškerý obsah v rámci elementu zvuku.
3. Umístěte kurzor na konec otevírání **zvuku** elementu a stiskněte klávesu **ENTER**.

    Všimněte si, že nová pozice kurzoru má úroveň další odsazení.

    ![Inteligentní odsazení v editoru HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "inteligentní odsazení v editoru HTML")

    *Inteligentní odsazení v editoru HTML*
4. Obnovit značku pro zvuk s obsahem jste odstranili, nebo zavřete **Default.aspx** bez uložení změn.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Krok 6 – extrahovat do uživatelského ovládacího prvku

Refactoring nástroje zahrnuté v sadě Visual Studio, jako je například extrahování část kódu na funkci, jsou skvělé funkce, které usnadňují zlepšení závorek a refaktoring stávajícího kódu. Protějšek pro stránky ASP.NET by extrakce kódu HTML do uživatelského ovládacího prvku. Teď už ručně by vyžadovalo několik kroků, jako vytvoření nového uživatelského ovládacího prvku, přesunutí části kódu do uživatelského ovládacího prvku, registrace předponu značky uživatelského ovládacího prvku a, nakonec vytvoření instance uživatelského ovládacího prvku na stránkách. Nyní, nové *extrahovat do uživatelského ovládacího prvku* nástroj všechny tyto kroky automaticky provede za vás.

Nové extrahovat do uživatelského ovládacího prvku kontextové operace v této úloze, použije k vygenerování nového uživatelského ovládacího prvku z vybraného kódu.

1. Na **Default.aspx** stránky, vyberte **H2** a **zvuku** elementy.
2. Klikněte pravým tlačítkem a vyberte **extrahovat do uživatelského ovládacího prvku**.

    ![Extrahovat do uživatelského ovládacího prvku nabídky](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "extrahovat do uživatelského ovládacího prvku nabídky")

    *Extrahovat do uživatelského ovládacího prvku nabídky*
3. Zadejte název nového uživatelského ovládacího prvku. Například **Jukebox.ascx**a potom klikněte na tlačítko **OK**.

    ![Uložení extrahovaných uživatelského ovládacího prvku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "uložení extrahovaných uživatelského ovládacího prvku")

    *Uložení extrahovaných uživatelského ovládacího prvku*
4. Všimněte si, že vybraný kód byl extrahován do uživatelského ovládacího prvku a byl nahrazen původní umístění vybraném kódu s instancí nového uživatelského ovládacího prvku.

    ![Stránka automaticky aktualizována na používání nového uživatelského ovládacího prvku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "stránky automaticky aktualizována na používání nového uživatelského ovládacího prvku")

    *Stránka automaticky aktualizována na používání nového uživatelského ovládacího prvku*
5. Stisknutím klávesy **F5** spusťte a ověřte, zda ovládací prvek funguje.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Cvičení 3: Co je nového v editoru jazyka JavaScript

Zápis nebo úpravy kódu jazyka JavaScript není snadný úkol, zejména v případě, že aplikace se spustí v velikost a zjistíte, se zabývá dlouhé soubory a stovky funkce. Skript vývojáři obvykle nemusí provést další úkony a zachovat čitelnost kódu a navigace mezi soubory. Díky zahrnutí knihovny jazyka JavaScript, jako je jQuery skript navigace přestal samotné složité kvůli délka kódu.

Visual Studio obnovil editor jazyka JavaScript s velcí přístupné a uspořádané režimu kódu. Mnoho funkcí sady Visual Studio, které existovaly už v editoru jazyka C# nebo VB je nyní implementována v JavaScript editoru: přechod na definici, Automatické odsazení, dokumentace a ověření v případě, že píšete. Pomocí seznamu obnovené IntelliSense máte katalogu funkce jazyka JavaScript na dosah ruky.

V tomto cvičení se dozvíte některé nové funkce a vylepšení editoru jazyka JavaScript. Budete procházet ukázkové soubory a každé nové vlastnosti, které způsobí, že programování v jazyce JavaScript v sadě Visual Studio 2012 efektivnější zjišťování.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Úloha 1 – nové funkce editoru jazyka JavaScript

Tato úloha vás seznámí s některé nové funkce editoru jazyka JavaScript, která se zaměřují na uspořádání kódu a přináší lepší uživatelské prostředí.

1. Pokud ještě není otevřen, začněte **sady Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení nachází v **Source\WhatsNewASPNET** složka tohoto testovacího prostředí.
2. Stisknutím klávesy **F5** ke spuštění aplikace, pak klikněte na odkaz JavaScript na navigačním panelu. Aktualizujte stránku několik časy a kontrola jak zvýší čítač.

    ![Čítač stránky](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "čítač stránky")

    *Čítač stránky*
3. Zavřete prohlížeč a přejděte zpět do sady Visual Studio.
4. Otevřít **JavaScript.aspx** stránky a vyhledejte **&lt;skript&gt;** blok (viz dole).

    Následující kód používá místní úložiště HTML5 pro ukládání *pageLoadCount* proměnná, která ukládá počet, kolikrát má aktuální uživatel navštívil stránky. Místní úložiště je databáze na straně klienta klíč hodnota, zavedl se standardem HTML5. Data uložená na místním počítači uvnitř webového prohlížeče.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Ujistěte se, že deklarace DOCTYPE je nastaven na XHTML5 než budete pokračovat s dalšími kroky.
5. Upravit kód a Všimněte si, že obsahuje IntelliSense pro JavaScript HTML5 funkce, jako je místní úložiště a jejich vnitřní metody.

    ![Funkce jazyka JavaScript HTML5 v jazyce JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript funkce v jazyce JavaScript")

    *Funkce jazyka JavaScript HTML5 v jazyce JavaScript*
6. Klikněte na libovolné levá hranatá závorka (**{**) z skriptování kódu a Všimněte si, že jsou zvýrazněny uvedených v závorkách.

    ![Závorky jsou zvýrazněny](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "závorky jsou zvýrazněné.")

    *Závorky jsou zvýrazněné.*
7. Zrušením komentáře u funkce **testAutoAlign()** (můžete si vybrat tři řádky **CTRL** + **K**; **CTRL** + **U**) a vyhledejte kurzor myši do kódu funkce. Stisknutím klávesy enter přidat druhý řádek. Všimněte si, že kód je nyní **zarovnané** a **automaticky odsazený**.

    ![Kód jazyka JavaScript je automaticky zarovnány](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "kód jazyka JavaScript je automaticky zarovnání")

    *Kód jazyka JavaScript je automaticky zarovnání*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Úloha 2 – ověření jazyka JavaScript

V této úloze bude zjišťovat nové ověření jazyka JavaScript pro standardní ECMAScript5. Tato funkce vám pomůže psát kompatibilní kód jazyka JavaScript, při brání problémy skriptování před nasazením webu.

> [!NOTE]
> Visual Studio 2010 implementované ECMAStript3 dodržování předpisů, zatímco služba Visual Studio 2012 poskytuje ECMAScript5 dodržování předpisů.


1. Otevřít **ECMA5script5.js** umístěna ve složce **Scripts\custom** složky projektu. Teď budete testovat ověřování pro ECMAScript5 standard.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Si můžete prohlédnout &quot; **použití Striktního režimu** &quot; směr v první řádek souboru, který umožňuje ECMAScript5 **přísném režimu**. Tento režim se skládá z podmnožinu jazyka, který vysvětluje nejednoznačnosti z posledních edice a přidá některé nové funkce, jako jsou gettery a settery, podpora knihovny pro JSON a úplnější reflexe na vlastnosti objektu.
2. Otevřít **seznam chyb** Pokud ještě není otevřené (**zobrazení** nabídky | **Seznam chyb**). Všimněte si, že **funkce** podtržené deklarace. Je to proto, že v ECMA5 standardní funkce nelze vnořit do struktury jazyka. V chybě bude seznam níže můžete zobrazit podrobnosti upozornění.

    ![Chybovou zprávu ověření jazyka JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "chybovou zprávu ověření jazyka JavaScript")

    *Chybovou zprávu ověření jazyka JavaScript*
3. Okomentujte **&quot;použití Striktního režimu&quot;** směr a Všimněte si, že chyby zmizí, ale zůstávají upozornění.
4. Napište libovolný řetězec jako v poslední řádek souboru **&quot;testování&quot;** (včetně uvozovek označuje jako řetězec). Zápis období vedle řetězec, který se zobrazí seznam technologie IntelliSense a vyberte **trim** možnost.

    Ve standardním ECMAScript5 řetězcové hodnoty a proměnné nemají řetězcových metod, které jsou definované jako oříznout, velká písmena, vyhledávání a nahrazení.

    ![Seznam technologie IntelliSense v jazyce JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "seznam technologie IntelliSense v jazyce JavaScript")

    *Seznam technologie IntelliSense v jazyce JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Úloha 3 – dokumentace XML pro jazyk JavaScript

V této úloze bude prozkoumání funkcí sady Visual Studio pro dokumentaci XML v jazyce JavaScript. Zobrazí se že seznam technologie IntelliSense jazyka JavaScript nyní zobrazuje dokumentace XML jednotlivých funkcí. Kromě toho bude zjišťovat navigační funkce v jazyce JavaScript.

1. Otevřít **XMLDoc.js** soubor umístěný ve **skripty a vlastní** složky projektu. Tento soubor obsahuje dokumentaci XML na všech funkcí jazyka JavaScript.

    ![Dokumentace JavaScript XML integrované technologie IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "dokumentace JavaScript XML integrované technologie IntelliSense")

    *Dokumentace JavaScript XML integrované technologie IntelliSense*
2. Pod **přidat** fungovat v **XMLDoc.js** soubor, vytvořte novou funkci s názvem **testování**.
3. V **testování** fungovat, zavolejte **vynásobit** funkce, která přijímá dva parametry. Všimněte si, že pole Popis se zobrazuje **vynásobit** dokumentaci k funkci.

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![Dokumentace XML pro funkce jazyka JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "dokumentace XML pro funkce jazyka JavaScript")

    *Dokumentace XML pro funkce jazyka JavaScript*
4. Dokončení příkazu call funkce a typ *tečkou* o otevření seznamu technologie IntelliSense pro vrácené hodnoty. Všimněte si, že se detekuje aplikace Visual Studio **vrátit** hodnotu v dokumentaci, zpracuje hodnotu jako číslo.

    ![Dokumentace XML pro typy vrácených hodnot](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "dokumentace XML pro návratové typy")

    *Dokumentace XML pro návratové typy*
5. Nyní vložte volání pro přidání funkce. Všimněte si, že editor JavaScriptu teď podporuje přetížení funkce. Při zápisu název funkce, budou moct vyberte některou z dostupných přetížení zadané v dokumentaci.

    ![Dokumentace XML pro přetížení](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "dokumentace XML pro přetížení")

    *Dokumentace XML pro přetížení*
6. Otevřít **GotoDefinition.js** souboru a vyhledejte **$().html()** volání funkce. Najít kurzor na **html**.
7. Stisknutím klávesy **F12** a přejít k definici. Všimněte si, že teď můžete používat a procházet kód JavaScriptu bez použití **najít** nástroj.
8. Najděte kurzor na instrukci jQuery před blok signatury na konci souboru kódu. Stisknutím klávesy **F12**. Bude přejděte na soubor knihovny jQuery. Všimněte si, že se můžete dostat také jQuery souborů pomocí **F12**.

    ![Přejdete na definice jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "přejdete na definice jQuery")

    *Přejděte na definice jQuery*

> [!NOTE]
> Ujistěte se, že GotoDefinition.js neobsahuje žádné chyby syntaxe před uložením tohoto souboru.


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Cvičení 4: Sdružování a Minifikace

Kolikrát websites obsahují více než jeden JavaScript nebo šablon stylů CSS soubor? Jde o velmi běžný scénář, kde sdružování a minifikace může pomoct snížit velikost souboru a nastavte lokality rychlejší. Nová funkce pro vytváření prostředků v ASP.NET 4.5 sady sadu soubory JS a CSS do jednoho elementu a snižuje jeho velikost pomocí minifikace obsah (to znamená odebrání mezery nejsou vyžadovány, odebírání komentářů, snížení identifikátory).

Sdružování a minifikace v technologii ASP.NET 4.5 je provést v době běhu, tak, aby proces identifikace uživatelský agent (např. aplikace Internet Explorer, Mozilla atd.) a proto komprese zlepšit cílení na uživatele prohlížeče (například odebrání položky, který je Mozilla konkrétní Když požadavek pochází z aplikace Internet Explorer).

V tomto cvičení se dozvíte, jak povolit a použít různé typy sdružování a minifikace v technologii ASP.NET 4.5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Úloha 1 – instalace sdružování a Minifikace balíčku od Nugetu

1. Pokud ještě není otevřen, začněte **sady Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení nachází v **Source\WhatsNewASPNET** složka tohoto testovacího prostředí.
2. Otevřete konzolu Správce balíčků NuGet. K tomuto účelu použijte nabídku **zobrazení** | **ostatní Windows** | **Konzola správce balíčků**.

    ![Otevření správce file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole balíčku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "otevřete konzoly Správce balíčků")

    *Otevření konzole Správce balíčků*
3. V **Konzola správce balíčků** typ **Install-Package Microsoft.Web.Optimization** a stiskněte klávesu **ENTER**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Úloha 2 – výchozí sady

Nejjednodušší způsob, jak používat sdružování a minifikace je umožnit výchozí sady. Tato metoda používá konvence umožňuje odkazovat na připojené a minifikovaný verze pro soubory JS a CSS ve složce.

V této úloze se dozvíte, jak povolit a odkazovat na připojené a minifikovaný soubory JS a CSS a zobrazit výsledný výstup.

1. Pokud ještě není otevřen, začněte **sady Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení nachází v **Source\WhatsNewASPNET** složka tohoto testovacího prostředí.
2. V **Průzkumníka řešení**, rozbalte **styly**, **Scripts\custom** a **Scripts\bundle** složek.

    Všimněte si, že aplikace používá více než jeden šablon stylů CSS a JS souboru.

    ![Soubory více šablon stylů a jazyka JavaScript v aplikaci](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "soubory více šablon stylů a jazyka JavaScript v aplikaci")

    *Více souborů šablon stylů a jazyka JavaScript v aplikaci*
3. Otevřít **Global.asax.cs** souboru.

    Všimněte si, že nový **Microsoft.Web.Optimization** obor názvů je označené jako komentář na začátku souboru. Zrušte komentář na pomocí směrnice obsahují sdružování a minifikace funkce.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Vyhledejte **aplikace\_Start** metody.

    V této metodě zrušte komentář u volání EnableDefaultBundles, jak je znázorněno v následujícím fragmentu kódu. Umožňuje nám to odkazuje na připojené kolekci souborů šablon stylů CSS ve složce pomocí cesty ke složce, plus &quot;šablon stylů CSS&quot; nebo &quot;JS&quot; příponu.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Otevřít **Optimization.aspx** souboru a vyhledejte ovládací prvek obsahu pro **HeadContent**.

    Všimněte si, že soubory šablon stylů CSS a JS mít jednu značku odkazované.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Tento kód je pro účely ukázky. V ideálním případě bude odkazovat sad v souboru Site.Master. V tomto ukázkovém kódu zjistíte, že některé soubory v sadě jsou také se na ně odkazovat soubor Site.Master odkazu poslední redundantní.
6. Všimněte si, že odkazy používají konvence vytváření prostředků v **href** atribut zobrazíte všechny šablony stylů CSS a JS soubory z stylů a Scripts\custom složky v uvedeném pořadí.

    Může používat cestu **skripty/vlastní/JS** jak je znázorněno níže, k vytvoření balíčku a minifikace všechny soubory JS v **skripty a vlastní** složky. Toto je výchozí chování sady výchozí.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Otevřít **Styles\Site.css** souboru.

    Všimněte si, že původní soubor CSS obsahuje kód odsazený, mezery a komentáře, které zvětšení souboru. (Také soubor jazyka JavaScript obsahuje mezery a komentáře).

    ![Jeden z původní šablony stylů CSS soubory ve složce Scripts](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "jeden z původní šablony stylů CSS soubory ve složce Scripts")

    *Jeden z původní šablony stylů CSS souborů ve složce Scripts*
8. Stisknutím klávesy **F5** spusťte aplikaci a přejděte **optimalizace** stránky.
9. Klikněte na **sady šablon stylů CSS** odkaz ke stažení a otevřete soubor.

    Projděte si soubor minifikovaný jako součást balíčku. Můžete si všimnout, že byly odebrány všechny mezery, komentáře a odsazení znaků, generování menší soubor.

    ![Soubory šablon stylů CSS v sadě](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "soubory balíčků šablon stylů CSS")

    *Soubory šablon stylů CSS v sadě*
10. Nyní klikejte na příkaz **JS sady** odkaz k otevření souboru JavaScriptu spojeny. V Průzkumníku upozornění můžete bezpečně ignorovat. Všimněte si, že soubory jazyka JavaScript v rámci **vlastní** složky jsou taky spojeny a minifikovaný.

    ![Soubory jazyka JavaScript v sadě](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "soubory balíčků jazyka JavaScript")

    *Soubory jazyka JavaScript v sadě*

    Povolení komprese pro soubory CSS a JS bylo mnohem složitější v předchozí verzi technologie ASP.NET. Nyní, jak jste viděli, stačí přidat jeden řádek *Global.asax* soubor povolit sdružování a odkázat na připojené soubory z vašeho webu.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Úloha 3 – statické sady

Statická sada přístupu umožňuje přizpůsobit sadu souborů sady, odkaz a připravenost k minifikaci metodu, která se použije.

V této úlohy můžete nakonfigurovat statické sady definovat konkrétní sadu souborů pro vytvoření balíčku a minifikace.

1. Zavřete prohlížeč.
2. Otevřít **Global.asax.cs** souboru a vyhledejte **aplikace\_Start** metoda.
3. Zrušením komentáře u statické sady kódu, jak je znázorněno v následujícím kódu.

    Definování statickou sadu, která se bude odkazovat &quot; **~/StaticBundle** &quot; virtuální cesty a použití **JsMinify** pro minimalizaci všechny zadané soubory s **AddFile** metody. Nakonec přidáte statická sada, která má **BundleTable** a že ho povolíte.

    Všimněte si, že soubory nejsou umístěny na stejném místě; Toto je Další výhodou nad výchozí sdružování.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Otevřít **Optimization.aspx** souboru.

    Všimněte si, že odkaz na **statické JS sady** je pomocí cesty je deklarován při konfiguraci sady statických v souboru Global.asax.cs: **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Stisknutím klávesy **F5** ke spuštění aplikace a potom přejděte na **optimalizace** stránky.
6. Klikněte na **statické JS sady** odkaz k otevření souboru.

    Všimněte si, že minifikovaný dodávat soubor jazyka JavaScript je výstup pro všechny soubory jazyka JavaScript nakonfigurovaný v souboru statické sady v rámci cesty &quot;/StaticBundle&quot;.

    ![Statické soubory sady JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "sady JavaScript statické soubory")

    *Seskupit statické soubory jazyka JavaScript*
7. Zavřete prohlížeč a vraťte se do sady Visual Studio.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Úloha 4 – dynamické složku sady

V této úloze se dozvíte, jak nakonfigurovat dynamické složku sady. Výkon dynamické sdružování je, že můžete zahrnout statické JavaScript, jakož i jiné soubory v jazycích, které se kompiluje do jazyka JavaScript a proto nevyžadují nějaké zpracování předtím, než se spustí sdružování.

V tomto příkladu se dozvíte, jak používat **DynamicFolderBundle** třídy za účelem vytvoření dynamického sada prostředků pro soubory, které jsou napsané v CofeeScript. CofeeScript je programovací jazyk, který kompiluje do jazyka JavaScript a nabízí jednodušší syntaxí pro psaní kódu jazyka JavaScript, lepší čitelnost a zkrácení v jazyce JavaScript.

1. Otevřít **Global.asax.cs** souboru a vyhledejte **aplikace\_Start** metoda.
2. Zrušením komentáře u dynamických sady kódu, jak je znázorněno v následujícím kódu.

    Definujete sadu dynamické složku, která bude používat **CoffeeMinify** připravenost k minifikaci vlastní procesor, který se bude vztahovat jenom na soubory s &quot; **.coffee** &quot; (rozšíření Soubory CoffeeScript). Všimněte si, že můžete použít vzor hledání pro výběr souborů pro vytvoření balíčku ve složce, jako je třeba "\*.coffee".

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Otevřete konzolu Správce balíčků NuGet. K tomuto účelu použijte nabídku **zobrazení** | **ostatní Windows** | **Konzola správce balíčků**.
4. V **Konzola správce balíčků** typ **Install-Package CoffeeSharp** a stiskněte klávesu **ENTER**.
5. Klikněte na tlačítko **zobrazit všechny soubory** tlačítko **Průzkumníka řešení** okna

    ![Zobrazení všech souborů](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "zobrazující všechny soubory")

    *Zobrazení všech souborů*
6. Klikněte pravým tlačítkem myši **CoffeeMinify.cs** soubor **Průzkumníku řešení** a vyberte **zahrnout do projektu**

    ![Zahrnout do projektu soubor CoffeeMinify.cs](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "CoffeeMinify.cs soubor zahrnout projektu")

    *Zahrnout soubor CoffeeMinify.cs do projektu*
7. Otevřít **CoffeeMinify.cs** souboru.

    Tato třída dědí z JsMinify k minifikaci Javascriptového výstupu plynoucí z kompilování kódu CoffeeScript. Volá CoffeeScript kompilátor nejprve generovat kód jazyka JavaScript, a potom ho odešle JsMinify.Process metody k minifikaci výsledný kód.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Otevřít **Script1.coffee** a **Script2.coffee** souborů z doručené pošty **skripty/sady** složky.

    Tyto soubory bude obsahovat CoffeScript kód se zkompiluje při provádění sdružování s třídou CoffeeMinify.

    V zájmu jednoduchosti soubory CoffeeScript k dispozici jsou pouze včetně CoffeeScript ukázkový kód. Komentáře jsou vyloučeny JsMinify procesem.

    ![Soubory CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "soubory CoffeeScript")

    *Soubory CoffeeScript*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) nabízí jednodušší syntaxi pro psaní kódu jazyka JavaScript, zkrácení v jazyce JavaScript a čitelnost, stejně jako přidávání jiných funkcí, jako je obsah pole a porovnávání vzorů.
9. Otevřít **Optimization.aspx** souboru a vyhledejte odkazy sady.

    Všimněte si, že odkaz na **dynamické JS sady** odkazuje **skripty/sady** složky pomocí **/kávy** přípony, které jste nakonfigurovali pro sadu složek dynamické.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Stisknutím klávesy **F5** ke spuštění aplikace a potom přejděte na **optimalizace** stránky.
11. Klikněte na **dynamické JS sady** odkaz k otevření generovaného souboru.

    Všimněte si, že obsah, který byl součástí Tato sada obsahuje pouze **.coffee** soubory. Můžete také zobrazit, že kód CoffeeScript byla zkompilována do jazyka JavaScript a komentovaná řádky se odebrala.

    ![Dynamické soubory JS sady](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "dynamické JS sady souborů")

    *Dynamické sady soubory JS*

> [!NOTE]
> Kromě toho můžete nasadit tuto aplikaci následující weby Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu](#AppendixB).


<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Toto testovací prostředí vám umožní pochopit, co je nového v ASP.NET a vývoj v sadě Visual Studio 2012 pro Web a jak využívat řadu vylepšení v sadě Visual Studio 2012.

Po dokončení tohoto praktického testovacího prostředí, mají zkušenosti použití nových funkcí a vylepšení v aplikaci Visual Studio 2012 editory pro šablony stylů CSS, JavaScript a HTML. Kromě toho mají zkušenosti, jak Visual Studio 2012 implementuje integrované sdružování a minifikace.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.

    ![Instalace sady Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "instalace sady Visual Studio Express")

    *Instalace sady Visual Studio Express*
4. Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Přijetí podmínek licence](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Přijetí podmínek licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Průběh instalace*
6. Až instalace skončí, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.

    ![VS Express for Web dlaždice](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *VS Express for Web dlaždice*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Příloha B: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu

Tento dodatek se ukazují, jak vytvořit nový web z portálu správy Windows Azure a publikovat aplikace, kterou jste získali podle testovacího prostředí, využít Webdeploy funkce publikování ve Windows Azure k dispozici.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Úloha 1 – Vytvoření nového webu z Windows webu Azure Portal

1. Přejděte [Windows Azure Management Portal](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů Microsoft spojených s vaším předplatným.

    > [!NOTE]
    > Windows Azure můžete zadarmo hostovat 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu. Můžete se zaregistrovat [tady](http://aka.ms/aspnet-hol-azure).

    ![Přihlaste se k portálu Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Přihlaste se k portálu Windows Azure")

    *Přihlaste se k portálu pro správu Azure Windows*
2. Klikněte na tlačítko **nový** na panelu příkazů.

    ![Vytvoření nového webu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "vytváření nového webu")

    *Vytvoření nového webu*
3. Klikněte na tlačítko **Compute** | **webu**. Potom vyberte **rychlé vytvoření** možnost. Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvořit web**.

    > [!NOTE]
    > Hostitel pro webovou aplikaci spuštěnou v cloudu, který může řídit a spravovat je web Windows Azure. Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál. Nezahrnuje kroky pro vytvoření databáze.

    ![Vytvoření nového webu pomocí metody rychlého vytvoření](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "vytváření nového webu pomocí metody rychlého vytvoření")

    *Vytvoření nového webu pomocí metody rychlého vytvoření*
4. Počkejte, dokud nové **webu** se vytvoří.
5. Po vytvoření webové stránky, klikněte na odkaz v části **URL** sloupce. Zkontrolujte, jestli funguje nový web.

    ![Na nový web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "přechodu na nový web")

    *Procházení na nový web*

    ![Spuštění webu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "spuštění webové stránky")

    *Spuštění webové stránky*
6. Přejděte zpět na portál a klikněte na název webové stránky v části **název** sloupec, který se zobrazí na stránkách správy.

    ![Otevřete správu webových stránek](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "otevřete správu webových stránek")

    *Otevřete správu webových stránek*
7. V **řídicí panel** stránce v části **rychlý přehled** klikněte na tlačítko **stáhnout profil publikování** odkaz.

    > [!NOTE]
    > *Profil publikování* obsahuje všechny informace požadované pro publikování webových aplikací na webu Windows Azure pro každou metodu povoleno publikování. Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a databázové řetězce požadované k připojení a ověřování na základě jednotlivých koncových bodů, u kterých je povolena metoda publikace. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** a **Microsoft Visual Studio 2012** podporují čtení publikační profily k automatizaci konfigurace z těchto programů pro publikování webových aplikací na Windows Azure websites.

    ![Stahování webové stránky publikovat profil](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "stahování webové stránky profil publikování")

    *Na webu stažení profilu publikování*
8. Stáhněte si soubor profilu publikování do vhodného umístění. Dále v tomto cvičení uvidíte jak publikovat webovou aplikaci na weby Windows Azure ze sady Visual Studio pomocí tohoto souboru.

    ![Ukládání souboru profilu publikování](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "ukládá se profil publikování")

    *Ukládá se profil publikování*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Úloha 2 – konfigurování serveru databáze

Pokud vaše aplikace využívá SQL Server databáze, budete muset vytvořit server služby SQL Database. Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá SQL Server může tuto úlohu přeskočit.

1. Pro uložení databáze aplikace budete potřebovat databázi SQL serveru. Servery SQL Database můžete zobrazit ze svého předplatného na portálu správy Windows Azure na **databází Sql** | **servery** | **serveru Řídicí panel**. Pokud nemáte server vytvořili, můžete vytvořit jednu **přidat** tlačítko na panelu příkazů. Poznamenejte si **název serveru a adresu URL, správce přihlašovací jméno a heslo**, jak je použijete v další úkoly. Nevytvářet databáze, protože vytvoří se v pozdější fázi.

    ![Řídicí panel serveru SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "řídicího panelu serveru SQL Database")

    *Řídicí panel serveru SQL Database*
2. V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio z tohoto důvodu je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**. Chcete-li to mohli udělat, klikněte na tlačítko **konfigurovat**, vyberte IP adresu z **aktuální IP adresa klienta** a vložte ho na **počáteční IP adresa** a **Koncová IP adresa** textová pole. Zadejte název pravidla a klikněte na tlačítko ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) tlačítko.

    ![Přidat IP adresu klienta](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Přidat IP adresu klienta*
3. Jednou **IP adresa klienta** je přidat do povolených IP adres klikněte na tlačítko na **Uložit** potvrďte provedené změny.

    ![Potvrzení změn](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Potvrzení změn*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nasazení webu

1. Vraťte se do řešení ASP.NET MVC 4. V **Průzkumníka řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.

    ![Publikování aplikace](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "publikování aplikace")

    *Publikování na webu*
2. Importujte profil publikování, který jste uložili v první úloze.

    ![Import profilu publikování](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "import profilu publikování")

    *Import publikačního profilu*
3. Klikněte na tlačítko **ověřit připojení**. Po dokončení ověření klikněte na tlačítko **Další**.

    > [!NOTE]
    > Ověření bylo dokončeno, jakmile se zobrazí zelené zaškrtnutí vedle tlačítka ověřit připojení.

    ![Ověřuje se připojení](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "ověřuje se připojení")

    *Ověřuje se připojení*
4. V **nastavení** stránce v části **databází** klikněte na tlačítko vedle textového pole připojení k databázi (to znamená **objekt DefaultConnection**).

    ![Konfigurace nasazení webu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "konfigurace nasazení webu")

    *Konfigurace nasazení webu*
5. Konfigurace připojení k databázi následujícím způsobem:

   - V **název serveru** zadejte vaše pomocí adresy URL databáze SQL serveru *tcp:* předponu.
   - V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.
   - V **heslo** zadejte heslo pro přihlašovací jméno správce serveru.
   - Zadejte nový název databáze, například: *MVC4SampleDB*.

     ![Konfigurace cílový připojovací řetězec](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "konfigurace cílový připojovací řetězec")

     *Konfigurace cílový připojovací řetězec*
6. Pak klikněte na tlačítko **OK**. Po zobrazení výzvy k vytvoření databáze klikněte na **Ano**.

    ![Vytvoření databáze](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "vytvoření řetězce databáze")

    *Vytvoření databáze*
7. Připojovací řetězec, který budete používat pro připojení k databázi SQL ve službě Windows Azure se zobrazí v textovém poli výchozí připojení. Pak klikněte na tlačítko **Další**.

    ![Připojovací řetězec odkazující na SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "připojovací řetězec odkazující na SQL Database")

    *Připojovací řetězec odkazující na SQL Database*
8. V **ve verzi Preview** klikněte na **publikovat**.

    ![Publikování webové aplikace](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "publikování webové aplikace")

    *Publikování webové aplikace*
9. Až se proces publikování dokončí, otevře se váš výchozí prohlížeč publikovaného webu.

    ![Publikování aplikace do Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "publikování aplikace do Windows Azure")

    *Aplikace publikovaná do Windows Azure*
