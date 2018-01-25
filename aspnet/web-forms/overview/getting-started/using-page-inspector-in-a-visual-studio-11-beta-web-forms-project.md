---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: "Díky nástroji Page Inspector pro sadu Visual Studio 2012 v rozhraní ASP.NET Web Forms | Microsoft Docs"
author: rick-anderson
description: "Nástroj Page Inspector pro sadu Visual Studio 2012 je nástroj pro vývoj webů s integrované prohlížeče. Vyberte libovolný element v integrovaném prohlížeči a nástroj Page Inspector..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: ca8a3c194577766e56d0604323fef567d539316c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Díky nástroji Page Inspector pro sadu Visual Studio 2012 v webové formuláře ASP.NET
====================
podle Tim Ammann

> Nástroj Page Inspector pro sadu Visual Studio 2012 je nástroj pro vývoj webů s integrované prohlížeče. Vyberte libovolný element, v prohlížeči integrované a nástroj Page Inspector okamžitě klade důraz elementu zdroje a šablon stylů CSS. Můžete procházet všechny stránky v aplikaci, rychle najít zdroje vykreslované značky a pomocí nástroje prohlížeče přímo v prostředí Visual Studio.
> 
> Tento kurz shwos povolení režimu kontroly a rychle najít a upravit pravidla šablon stylů CSS a text v rámci svého webového projektu. Tento kurz používá projekt webové formuláře aplikace, ale můžete použít také nástroje Page Inspector pro webové projekty a [MVC](https://go.microsoft.com/?linkid=9802002) aplikace.
> 
> Tento kurz obsahuje následující části:
> 
> [Požadavky](#_1_prerequisites)
> 
> [Vytvoření webové aplikace](#_2_creating_a)
> 
> [Chcete-li zobrazit aplikaci použít nástroj Page Inspector](#_3_using_page)
> 
> [Povolit režim kontroly](#_4_inspection_mode)
> 
> [Použijte nástroj Page Inspector provádět změny kódu](#_5_using_page)
> 
> [Režim kontroly a okno HTML](#_6_inspection_mode)
> 
> [Zobrazení náhledu změn šablon stylů CSS v okně Styly](#_7_previewing_css)
> 
> [Automatická synchronizace šablon stylů CSS](#css_auto_sync)
> 
> [Výběr barvy šablon stylů CSS](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Požadavky

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) nebo [Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Chcete-li získat nejnovější verzi nástroje Page Inspector, použijte [instalačního programu webové platformy](https://go.microsoft.com/fwlink/?LinkId=255386) nainstalovat sadu Azure SDK pro rozhraní .NET 2.0.


Nástroj Page Inspector je instalován s Microsoft Web Developer Tools. Nejnovější verze je 1.3. Zjištění verze máte, spusťte Visual Studio a vyberte **o sadě Microsoft Visual Studio** z **pomoci** nabídky.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Vytvoření webové aplikace

Nejdřív vytvoříte webovou aplikaci, která nástroj Page Inspector se bude používat. V sadě Visual Studio, vyberte **soubor** &gt; **nový projekt**. Na levé straně, rozbalte položku **Visual C#**, vyberte **webové**a potom vyberte **aplikaci webových formulářů ASP.NET**.

![Novou aplikaci Web Forms](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Click **OK**.

Aplikace se otevře v **zdroj** zobrazení.

![Novou aplikaci Web Forms v zobrazení zdroje](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Teď, když máte aplikaci pro práci s, můžete použít nástroj Page Inspector prozkoumat a upravit ho.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Chcete-li zobrazit aplikaci použít nástroj Page Inspector

V dalším kroku se zobrazí aplikace s nástrojem Page Inspector. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a potom zvolte **zobrazení v nástroj Page Inspector**.

![Zobrazení v nástroj Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Ve výchozím nastavení kdy nástroj Page Inspector spouští poprvé, je ukotven jako úzké okno na levé straně prostředí Visual Studio. Ponechte ukotven na levé straně a nastavte ji na šířku, který je možnost pro vás, nebo ho v jednom z oblastí, které nástroj ukotvení na nahoru, dolů nebo vpravo:

![Nástroj Page Inspector pozice ukotvení](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Pokud uvolníte okno nástroj Page Inspector, můžete umístit ji mimo Visual Studio, nebo i na druhém monitoru Pokud nemáte. Však v pořadí, v jakém ALT + TAB mezi nástroj Page Inspector a Visual Studio při okno nástroj Page Inspector není ukotvený, přejděte do **nástroje** &gt; **možnosti** &gt;  **Prostředí** &gt; **karty a okna**a v části **kartě dobře**, zrušte zaškrtnutí políčka názvem **plovoucí nástroj windows vždy zůstat na hlavní okno**:

![Zrušením zaškrtnutí plovoucí windows nástroj ALT + TAB mezi odpojenou okno nástroj Page Inspector a Visual Studio](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Horní podokno okna nástroje Page Inspector ukazuje aktuální stránky v okně prohlížeče. V dolním podokně zobrazí stránku v značka jazyka HTML na levé straně a některé karty na pravé straně, která umožňují kontrolovat různé aspekty stránky. Dolní podokno je podobná [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) v aplikaci Internet Explorer. (Ale na rozdíl od nástrojů pro vývojáře, můžete nástroj Page Inspector přímo v sadě Visual Studio.)

![Inspektor stránek](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

V tomto kurzu budete používat v podokně Prohlížeč nástroje Page Inspector a **HTML** a **styly** karty vám pomohou rychle přejděte a provést změny aplikace.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Povolit režim kontroly

V dalším kroku uvidíte, jak funguje nástroj Page Inspector kontroly režimu. V okně nástroje Page Inspector klikněte na **kontroly** tlačítko.

![Kontrola – Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Informace o režimu kontroly v akci, přesuňte ukazatel myši přes různé části stránky v okně prohlížeče nástroj Page Inspector. Jak to uděláte, nezmění na velké znaménko plus a zvýrazní se element pod:

![Ukazatele myši div.content obálky](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Jako ukazatel myši přesunete, Všimněte si, že

- Obsah **zdroj** zobrazit změny zobrazit značku odpovídající vybraný element na stránce. Je označený relevantní značek. Pokud zdroj je v jiném souboru, je tento soubor otevřít v zobrazení zdroje se zvýrazněnou relevantní značkami.

- Kód zobrazí v **HTML** ve nástroj Page Inspector také změní tak, aby odpovídaly vybraný element na stránce. V **HTML** kartě popsané relevantní značek.

- **Styly** kartě se zobrazují pravidla šablon stylů CSS relevantní pro aktuální výběr.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Použijte nástroj Page Inspector provádět změny kódu

Nyní uvidíte, jak můžete použít nástroj Page Inspector k vyhledání a provést změny kódu nebo text, jehož umístění nemusí být hned zjevné.

Nástroj Page Inspector uvést do režimu kontroly a potom přejděte k dolnímu okraji domovské stránce.

Jakmile zadáte oblasti zápatí, otevře se nástroj Page Inspector *Site.Master* rozložení souboru v **zdroje** zobrazení na dočasné kartě napravo od dalších karty a zvýrazňuje části hlavní stránky, které Vybrali jste. To ukazuje, jak nástroj Page Inspector můžete najít a zobrazit obsah na stránku, která ve skutečnosti může pocházet z jiného souboru než ten, který původně otevřel.

![Označuje zápatí v režimu kontroly](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

V okně prohlížeče nástroj Page Inspector, přesuňte ukazatel myši nad řádek s copyrightu <a id="a"> </a>Všimněte si.

V *Site.Master* stránky, odpovídající řádek zvýrazněna.

![Zápatí autorským řádek zvýrazněna](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Přidejte na konec řádku v nějaký text *Site.Master* souboru.

&lt;p&gt;&amp;kopírovat; &lt;%: DateTime.Now.Year %&gt; -Moje aplikace skály ASP.NET!&lt; /p&gt;

Nyní stiskněte Ctrl + Alt + Enter nebo klikněte na panelu aktualizace a zobrazte výsledky v okně prohlížeče nástroj Page Inspector.

![Moje aplikace skály ASP.NET!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Vám může mít chápat, že zápatí byla na *Default.aspx* stránky, ale ukázalo v hlavní rozložení stránky a nástroj Page Inspector zjistila, že pro vás.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Režim kontroly a okno HTML

V dalším kroku bude mít rychlý pohled na okno HTML a jak mapuje prvky pro vás.

Nástroj Page Inspector uvést do režimu kontroly.

![Kontrola – Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Klikněte na horní část stránky, která říká "zde bude vaše logo". Jsou zkoumání určitý element podrobněji tak zobrazení v okně prohlížeče už změní, když ukazatel myši přesunete.

Nyní přesunutí ukazatele myši **HTML** okno. Jako ukazatel myši přesunete, nástroj Page Inspector popisuje element v rámci **HTML** okno a zvýrazňuje odpovídající element v okně prohlížeče.

![HTML Window](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Jak před, otevře se nástroj Page Inspector *Site.Master* souboru můžete na kartě dočasné. Klikněte na kartu Site.Master a zvýrazní se odpovídající kód v &lt;záhlaví&gt; části:

![Zvýrazněná značka](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Zobrazení náhledu změn šablon stylů CSS v okně Styly

V dalším kroku uvidíte, jak můžete použít nástroj Page Inspector **styly** okna zobrazení náhledu změn do šablon stylů CSS.

Klikněte **kontroly** tlačítko uvést do režimu kontroly nástroje Page Inspector.

V okně prohlížeče nástroj Page Inspector, přesuňte ukazatel myši nad v části "Home Page", dokud **div.content obálku** se zobrazí popisek.

![Ukazatele myši na elementy](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Klikněte v části div.content obálku jednou a poté přesuňte ukazatel myši na **styly** okno. V části volič třídy .featured .content obálky zrušte a zaškrtněte políčko pro vlastnost barvu pozadí.

![Barva pozadí vymazat](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Všimněte si, jak tato změna přináší náhled okamžitě v okně prohlížeče nástroj Page Inspector.

Zaškrtněte políčko znovu, klikněte dvakrát na hodnotu vlastnosti a změňte ji na `red`. Změna ukazuje okamžitě:

![Barva pozadí Red](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

**Styly** okno díky usnadňuje testování a náhled šablon stylů CSS změní před potvrzením změny styl list sám sebe.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Automatická synchronizace šablon stylů CSS

> [!NOTE]
> Tato funkce vyžaduje 1.3 verzi nástroje Page Inspector.


Automatická synchronizace šablon stylů CSS funkce umožňuje přímo upravit soubor CSS a podívejte se změny okamžitě v prohlížeči nástroj Page Inspector.

Klikněte na tlačítko **kontroly** uvést do režimu kontroly nástroje Page Inspector.

V prohlížeči nástroj Page Inspector, přesuňte ukazatel myši nad v části "Home Page", dokud **div.content obálku** se zobrazí popisek. Klikněte na tlačítko jednou tento element vyberte.

**Syles** v okně se zobrazí všechna pravidla šablon stylů CSS pro tento element. Posuňte se dolů a najít .featured .content obálku třída selektor. Klikněte na ".featured .content obálku". Nástroj Page Inspector otevře soubor CSS, který definuje tento styl (Site.css) a zvýrazňuje odpovídající stylu CSS.

![Soubor CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Teď změňte hodnotu `background-color` na "red". Změny se okamžitě zobrazí v prohlížeči nástroj Page Inspector.

![Nástroj Page Inspector prohlížeče](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Výběr barvy šablon stylů CSS

Dále budete Další informace o použití nástroje Page Inspector rychle najít a změnit šablon stylů CSS v výchozí aplikace zvýrazněný text. V tomto příkladu jste se rozhodli, že nemáte jako modře zvýrazněný příklad a chcete ho změnit na jinou barvu.

Klikněte **kontroly** tlačítko.

![Kontrola – Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

V okně prohlížeče nástroj Page Inspector, přesuňte ukazatel myši nad zvýrazněnou "videa, kurzy a ukázky" text tak, aby CSS "označením" Popisek se zobrazí.

![Ukazatele myši značky elementu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Kliknutím na text vyberte ho. V dolní části se zobrazí odpovídající značky selektor šablon stylů CSS **styly** okno.

![Označit pro výběr v okně Styly](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Klikněte na označit selektor. Tím se otevře *Site.css* souboru pro webovou aplikaci. Klikněte na kartu Site.css a zvýrazní se odpovídající šablon stylů CSS pro selektor:

![Označit selektor v šabloně stylů.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Vyberte a odeberte řádku s vlastností barvu pozadí.

Teď použijete nové volby barev Visual Studio 2012 CSS vybrat nový barvu pro **označit** vlastnost barvu pozadí.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Pomocí volby barev služby Visual Studio 2012 šablon stylů CSS

Editor šablon stylů CSS v sadě Visual Studio 2012 má výběr barvy, která usnadňuje zvolte a vložit barvy. Obsahuje výběr "pop rozbalovací", která nabízí jemnějšího ovládání a jednoduchý pruhu barev.

Výběr barvy obsahuje standardní paletu barev, podporuje názvy standardní barev, kódů hash, barvy RGB, RGBA, HSL a HSLA a udržuje seznam barev, které byly naposledy použity v dokumentu.

V řádku, kde byla vlastnost Barva pozadí zadejte "bc" a stiskněte jednou na šipku dolů.

Když zadáte první znak každého slova ve vlastnosti oddělené pomlčkou jako "background-color", filtry IntelliSense v seznamu zobrazit pouze vlastnosti, které odpovídají:

![IntelliSense filtrované hodnoty](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Nyní zadejte středník. Když to uděláte, vloží se název vlastnosti úplné barvu pozadí. Typ  **#**  nebo **rgb (**, a zobrazí se na panelu Výběr barev:

![Na panelu Výběr barev šablon stylů CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Pokud chcete zjistit, jak funguje pruhu barev výběr, klikněte na jeho barvy s ukazatelem myši nebo stiskněte klávesu šipka dolů a pak použijte klávesy se šipkami vlevo a vpravo procházení barvy. Při návštěvě barvu, která je zobrazen náhled s odpovídající hodnotou pro vlastnost background-color:

![Hodnota vlastnosti Barva pozadí zobrazení náhledu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

V tomto okamžiku by mohla stisknutím klávesy Enter vyberte hodnotu a pak středníkem (;) k dokončení položka šablon stylů CSS. Teď přejděte k další části, aby mohli zobrazit, jak funguje barva výběru pop nižší.

#### <a name="using-the-color-picker-pop-down"></a>Pomocí Pop nižší výběr barev

Když pruhu barev nemá přesný barvu, která hledáte, můžete pro výběr barvy rozbalovací pop.

Otevřete ho kliknutím na dvojitou šipku na pravém konci pruhu barev nebo stiskněte klávesu šipka dolů jednou nebo dvakrát na klávesnici.

![Výběr rozbalovací Pop barva šablon stylů CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Klikněte na barvu ze svislá čára na pravé straně. Ukazuje to přechodu pro tuto barvu v hlavním okně. Vybrat barvu přímo z panelu svislé stisknutím klávesy Enter nebo klikněte na libovolný bod v hlavním okně zvolit s vyšší přesností.

Pokud je na obrazovce počítače, který chcete použít barvu (nemusí to být v uživatelském rozhraní sady Visual Studio), jeho hodnota můžete zaznamenat pomocí nástroje kapátko vpravo dole.

Také můžete změnit krytí barvu přesunutím posuvníku v dolní části volby barev. V tom změny barvu hodnoty RGBA hodnoty, protože formát RGBA může představovat neprůhlednost.

Po výběru barvy, stiskněte klávesu Enter a pak zadejte středníkem k dokončení barvu pozadí položky v *Site.css* souboru.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Na stránce Inspector aktualizace panelu

Nástroj Page Inspector okamžitě zjistí změnu *Site.css* souboru (nebo všechny soubory v aplikaci) a zobrazí upozornění na aktualizace panelu.

![Aktualizace panelu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Pokud chcete uložit všechny soubory a aktualizujte stránku prohlížeče nástroj Page Inspector, stiskněte Ctrl + Alt + Enter nebo klikněte na panelu aktualizace. Změna v barva zvýraznění se zobrazí v prohlížeči:

![Barva zvýraznění změnit](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Všimněte si, nemusí aktualizovat prohlížeč nástroje Page Inspector přímo na v prostředí Visual Studio. Díky nástroji Page Inspector místo externího prohlížeče umožňuje zůstat v editoru při vývoji webových aplikací.

## <a name="see-also"></a>Viz také

[Představení nástroje Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (video na kanálu 9)

[Nástroj Page Inspector chybové zprávy](https://go.microsoft.com/?linkid=9813062) (MSDN)
