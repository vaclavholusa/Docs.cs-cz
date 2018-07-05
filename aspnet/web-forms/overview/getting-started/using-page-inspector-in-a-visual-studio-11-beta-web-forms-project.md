---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Použití Page Inspectoru pro Visual Studio 2012 v rozhraní ASP.NET Web Forms | Dokumentace Microsoftu
author: rick-anderson
description: Nástroj Page Inspector pro sadu Visual Studio 2012 je nástroj pro vývoj webů pomocí integrovaného prohlížeče. Vyberte jakýkoli element ve integrovaného prohlížeče a nástroj Page Inspector...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 1f1ac1072d33c85ed3e64c493b9cf7970695cea6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384472"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Použití Page Inspectoru pro Visual Studio 2012 ve webových formulářích ASP.NET
====================
podle Tim Ammann

> Nástroj Page Inspector pro sadu Visual Studio 2012 je nástroj pro vývoj webů pomocí integrovaného prohlížeče. Vybrat jakýkoli element ve integrovaného prohlížeče a nástroj Page Inspector okamžitě zvýrazní zdroje a šablon stylů CSS prvku. Můžete procházet všechny stránky v aplikaci, rychle najít zdroje vykreslované značky a použít nástroje prohlížeče přímo v prostředí sady Visual Studio.
> 
> Tento kurz shwos jak povolit režim kontroly a rychle najít a úprava pravidel šablon stylů CSS a text v rámci webového projektu. V tomto kurzu použijete projekt webových formulářů aplikace, ale můžete použít také nástroje Page Inspector pro webové projekty a [MVC](https://go.microsoft.com/?linkid=9802002) aplikací.
> 
> Tento kurz obsahuje následující oddíly:
> 
> [Požadované součásti](#_1_prerequisites)
> 
> [Vytvoření webové aplikace](#_2_creating_a)
> 
> [Zobrazit aplikaci pomocí nástroje Page Inspector](#_3_using_page)
> 
> [Povolit režim kontroly](#_4_inspection_mode)
> 
> [Použití Page Inspectoru provádět změny kódu](#_5_using_page)
> 
> [Režim kontroly a v okně HTML](#_6_inspection_mode)
> 
> [Náhled změn šablon stylů CSS v okně Styly](#_7_previewing_css)
> 
> [Automatická synchronizace šablon stylů CSS](#css_auto_sync)
> 
> [Výběr barvy šablon stylů CSS](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Požadavky

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) nebo [sady Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Chcete-li získat nejnovější verzi nástroje Page Inspector, použijte [instalačního programu webové platformy](https://go.microsoft.com/fwlink/?LinkId=255386) k instalaci sady Azure SDK pro .NET 2.0.


Nástroj Page Inspector je součástí nástroje Microsoft Web Developer Tools. Nejnovější verze je verze 1.3. Zjištění verze máte, spusťte Visual Studio a vyberte **o Microsoft Visual Studio** z **pomáhají** nabídky.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Vytvoření webové aplikace

Nejprve vytvoříte webovou aplikaci, kterou budete používat nástroj Page Inspector se. V sadě Visual Studio, zvolte **souboru** &gt; **nový projekt**. Na levé straně rozbalte **Visual C#** vyberte **webové**a pak vyberte **aplikace webových formulářů ASP.NET**.

![Nová aplikace webových formulářů](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Klikněte na tlačítko **OK**.

Aplikace se otevře v **zdroj** zobrazení.

![Nová aplikace webových formulářů v zobrazení zdroje](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Teď, když máte aplikaci pro práci s, můžete použít nástroj Page Inspector sloužící ke zkoumání a upravte ho.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Zobrazit aplikaci pomocí nástroje Page Inspector

V dalším kroku se zobrazit aplikace s nástrojem Page Inspector. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt a klikněte na tlačítko **zobrazení v nástroje Page Inspector**.

![Zobrazení v nástroje Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Ve výchozím nastavení když poprvé, spustí nástroj Page Inspector je ukotven jako úzké okno na levé straně prostředí sady Visual Studio. Ponechte ukotvena na levé straně a nastavte ho na šířku, který je pro vás naučí, nebo jej v jedné oblasti nástroj ukotvit na nahoru, dolů nebo vpravo:

![Nástroj Page Inspector pozice ukotvení](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Pokud uvolníte okna nástroje Page Inspector, je možné je umístit mimo sadu Visual Studio, nebo dokonce i na druhý monitor pokud nějakou máte. Ale v pořadí ALT + TAB mezi nástroj Page Inspector a sady Visual Studio při okno nástroje Page Inspector je uvolněna, přejděte na **nástroje** &gt; **možnosti** &gt;  **Prostředí** &gt; **karty a Windows**a v části **kartu dobře**zrušte výběr zaškrtávacího políčka volá **plovoucí panely nástrojů trojů zůstávají nad hlavní okno**:

![Zrušte zaškrtnutí políčka s plovoucí desetinnou čárkou windows nástroj ALT + TAB mezi Visual Studio a okno neukotvené panely nástroje Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Horní podokno okna nástroje Page Inspector aktuální stránky zobrazí v okně prohlížeče. V dolním podokně zobrazí na stránce v kódu HTML na levé straně a některé karty na pravé straně, která umožňují kontrolovat různé aspekty stránky. Dolní podokno je podobný [vývojářské nástroje F12 pomáhají](https://msdn.microsoft.com/ie/aa740478) v aplikaci Internet Explorer. (Ale na rozdíl od vývojářských nástrojů, můžete nástroj Page Inspector přímo v sadě Visual Studio.)

![Inspektor stránek](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

V tomto kurzu použijete prohlížeč podokna nástroje Page Inspector a **HTML** a **styly** karty, které vám pomohou rychle přejít a provést změny aplikace.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Povolit režim kontroly

Dále uvidíte, jak funguje režim kontroly nástroje Page Inspector. V okně nástroje Page Inspector klikněte **zkontrolujte, jestli se** tlačítko.

![Zkontrolovat Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Pokud chcete zobrazit režim kontroly v akci, najeďte myší různé části stránky v rámci okna prohlížeče nástroj Page Inspector. Stejně jako, ukazatel myši se změní na velké znaménko plus a zvýrazní prvek pod:

![Ukazatel myši div.content obálky](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Při přesunu ukazatele myši, Všimněte si, že

- Obsah v **zdroj** zobrazení se změní a objeví značka odpovídající vybraný element na stránce. Je zvýrazněn odpovídající značky. Pokud je zdroj do jiného souboru, tento soubor otevřít v zobrazení zdroje se zvýrazněnou relevantní značkami.

- Značky zobrazeny v **HTML** karta v nástroje Page Inspector se také změní tak, aby odpovídaly vybraný element na stránce. V **HTML** kartu, jsou uvedeny odpovídající značky.

- **Styly** karta zobrazuje relevantní pro aktuální výběr pravidel šablon stylů CSS.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Použití Page Inspectoru provádět změny kódu

Nyní uvidíte, jak vám pomůže nástroj Page Inspector najít a provést změny kódu nebo text, jehož umístění nemusí být hned zjevné.

Vložit nástroj Page Inspector v režimu kontroly a potom přejděte do dolní části domovské stránky.

Jakmile zadáte zápatí, otevře se nástroj Page Inspector *Site.Master* soubor rozložení v **zdroje** zobrazení dočasný kartě napravo od druhé karty a zvýrazní části hlavní stránky, které jste Vybrali jste. To se dozvíte, jak najít a zobrazit obsah na stránce, která ve skutečnosti může pocházet z různých souborů než ten, který původně otevřel nástroje Page Inspector.

![Zápatí osvětlení na režim kontroly](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

V okně prohlížeče nástroj Page Inspector, přesuňte ukazatel myši nad řádek s copyrightu <a id="a"> </a>Všimněte si, že.

V *Site.Master* stránce odpovídajícím řádku se zvýrazní.

![Zápatí o autorských právech řádek zvýrazněný](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Přidejte nějaký text do konce řádku v *Site.Master* souboru.

&lt;p&gt;&amp;kopírovat; &lt;%: DateTime.Now.Year %&gt; – My ASP.NET Application Rocks!&lt; /p&gt;

Nyní stiskněte kombinaci kláves Ctrl + Alt + Enter nebo klikněte na panel aktualizace pro zobrazení výsledků v okně prohlížeče nástroj Page Inspector.

![My ASP.NET Application Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Jste možná jste si mysleli, že byla v zápatí je uvedené na *Default.aspx* stránky, ale ukázalo být na stránce předlohy rozložení a nástroj Page Inspector zjistila, že pro vás.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Režim kontroly a v okně HTML

V dalším kroku bude mít rychlý pohled na okno HTML a jak mapuje prvky za vás.

Nástroj Page Inspector převeďte do režimu kontroly.

![Zkontrolovat Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Klepněte na horní část stránky, která říká "zde bude vaše logo". Prohlížené konkrétní element podrobněji, tak zobrazení v okně prohlížeče už mění při přesunutí ukazatele myši.

Nyní přesunutí ukazatele myši **HTML** okna. Při přesunu ukazatele myši, nástroj Page Inspector popisuje element v rámci **HTML** okno a zvýrazní odpovídající element v okně prohlížeče.

![Okno HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Jako předtím, otevře se nástroj Page Inspector *Site.Master* souboru pro vás dočasné kartě. Klikněte na kartu Site.Master a zvýrazní se odpovídající značky ve &lt;záhlaví&gt; části:

![Zvýrazněná značka](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Náhled změn šablon stylů CSS v okně Styly

V dalším kroku se zobrazí, jak můžete použít nástroj Page Inspector **styly** okna Náhled změn do šablony stylů CSS.

Klikněte na tlačítko **zkontrolujte, jestli se** tlačítko Vložit nástroj Page Inspector režim kontroly.

V okně prohlížeče nástroj Page Inspector, přesuňte ukazatel myši nad oddíl "Home Page" až do **div.content obálky** popisek se zobrazí.

![Ukazatel myši přesunout elementy](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Klikněte v části div.content obálky jednou a poté přesuňte ukazatel myši na **styly** okna. V části volič .featured .content obálkové třídy zrušte a zaškrtněte políčko pro vlastnost barvu pozadí.

![Barva pozadí vymazat](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Všimněte si, jak tato změna přináší náhled okamžitě v okně prohlížeče nástroj Page Inspector.

Zaškrtněte toto políčko znovu, klikněte dvakrát na hodnotu vlastnosti a změňte ji na `red`. Změna ukazuje hned:

![Barva pozadí Red](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

**Styly** okno umožňuje snadno test a náhled šablon stylů CSS změní před potvrzením změn na styl listu samotný.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Automatická synchronizace šablon stylů CSS

> [!NOTE]
> Tato funkce vyžaduje verzi 1.3 nástroj Page Inspector.


Funkce Automatická synchronizace šablon stylů CSS lze upravit přímo soubor šablony stylů CSS a podívejte se změny okamžitě v prohlížeči nástroj Page Inspector.

Klikněte na tlačítko **zkontrolujte, jestli se** uvést do režimu kontroly nástroje Page Inspector.

V prohlížeči nástroj Page Inspector, přesuňte ukazatel myši nad oddíl "Home Page" až do **div.content obálky** popisek se zobrazí. Kliknutím vyberte tento element.

**– Styly** okno zobrazuje všechna pravidla šablon stylů CSS u tohoto elementu. Posuňte se dolů najít .featured .content – obálky třídy selektor. Klikněte na ".featured .content obálku". Nástroj Page Inspector otevře soubor šablony stylů CSS, která definuje tento styl (Site.css) a zvýrazní odpovídající stylu CSS.

![Soubor CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Teď změňte hodnotu `background-color` na hodnotu "red". Změny se okamžitě zobrazí v prohlížeči nástroj Page Inspector.

![Nástroj Page Inspector prohlížeče](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Výběr barvy šablon stylů CSS

V dalším kroku se dozvíte, jak používat nástroj Page Inspector rychle najít a změnit šablon stylů CSS pro zvýrazněného textu ve výchozím nastavení aplikace. V tomto příkladu jste se rozhodli, že nemáte, jako jsou modře zvýrazněný a chcete ho změnit na jinou barvu.

Klikněte na tlačítko **zkontrolujte, jestli se** tlačítko.

![Zkontrolovat Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

V okně prohlížeče nástroj Page Inspector, přesuňte ukazatel myši nad zvýrazněnou "videa, kurzy a ukázky" tak, aby CSS "mark" Popisek zobrazí se text.

![Najede myší element značky](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Klepněte na text a vyberte ji. Odpovídající značky selektor šablon stylů CSS se zobrazí v dolní části **styly** okna.

![Označit selektoru v okně Styly](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Klepněte na volič značky. Tím se otevře *Site.css* souboru pro webovou aplikaci. Klikněte na kartu Site.css a zvýrazní se odpovídající šablony stylů CSS pro selektor:

![Označit selektoru v šabloně stylů](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Vyberte a odeberte řádku s vlastností barvu pozadí.

Teď použijete nový výběr barvy šablon stylů CSS 2012 Visual Studio zvolte novou barvu pro **označit** vlastnost barvu pozadí.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Pomocí volby barev služby Visual Studio 2012 šablon stylů CSS

Editor šablon stylů CSS v sadě Visual Studio 2012 obsahuje barvu ovládacího prvku pro výběr, která umožňuje jednoduše vybrat a vložit barvy. Obsahuje jednoduché pruhu barev a "pop dolů" ovládacího prvku pro výběr, který nabízí lepší kontrolu.

Výběr barvy obsahuje standardní paletu barev, podporuje názvy standardních barev, hash kódy, barvy RGB, RGBA, HSL a HSLA a udržuje seznam barvy, které jste naposledy použili v dokumentu.

Na řádku, kde byla vlastnost background-color zadejte "bc" a jednou stisknutím šipky dolů.

Když zadáte první znak každého slova oddělená spojovníkem vlastnost jako "background-color", filtry IntelliSense v seznamu zobrazit pouze vlastnosti, které odpovídají:

![Technologie IntelliSense filtrované hodnoty](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Teď zadejte dvojtečku. Pokud ano, vloží se název vlastnosti plnou barvu pozadí. Typ **#** nebo **rgb (**, a zobrazí se panel pro výběr barev:

![Výběr pruhu barev šablon stylů CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Pokud chcete zobrazit, jak funguje pruhu barev pro výběr, klikněte na jeho barvy s ukazatelem myši nebo stisknutím klávesy se šipkou dolů a pak pomocí kláves šipka doleva a doprava procházení barvy. Při návštěvě barvu, je zobrazen odpovídající hodnota pro vlastnost background-color:

![Hodnota vlastnosti barvu pozadí náhledu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

V tomto okamžiku může stisknutím klávesy Enter vyberte hodnotu a potom středníkem (;) k dokončení položky šablon stylů CSS. Teď přejděte k další části tak, abyste viděli, jak funguje v pop – seznamu pro výběr barvy.

#### <a name="using-the-color-picker-pop-down"></a>Použití barev výběr Pop dolů

Pokud u panelu barev není přesná barva, kterou hledáte, můžete použít výběr barvy pop dolů.

Pokud chcete soubor otevřít, klikněte na dvojitou šipku na pravém konci pruhu barev nebo jednou nebo dvakrát klávesu šipka dolů na klávesnici.

![Výběr barvy šablon stylů CSS Pop dolů](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Klikněte na barvu z svislá čára na pravé straně. V hlavním okně zobrazí přechod pro tuto barvu. Zvolte barvu přímo z svislá čára stisknutím klávesy Enter nebo klikněte na tlačítko kdekoli v hlavním okně Vybrat s větší přesností.

Pokud je na obrazovce počítače, který chcete použít barvu (nemusí být uvnitř uživatelské rozhraní Visual Studia), můžete zachytit její hodnotu s použitím nástroje kapátko vpravo dole.

Neprůhlednost barvu také můžete změnit přesunutím posuvníku v dolní části nástroje pro výběr barvy. Provádí se tak změní se barva hodnot na hodnoty RGBA, protože formát RGBA může představovat neprůhlednosti.

Po výběru barvy, stiskněte klávesu Enter a pak zadejte středníkem k dokončení barvu pozadí položky v *Site.css* souboru.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Kontrola aktualizace posuvník

Nástroj Page Inspector okamžitě zjistí změnu *Site.css* souboru (nebo na libovolný soubor v aplikaci) a zobrazí upozornění na aktualizace panelu.

![Panel aktualizace](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Uložte všechny soubory a aktualizujte prohlížeč, nástroj Page Inspector stisknutím klávesy Ctrl + Alt + Enter nebo klikněte na panel aktualizace. Změna barvy zvýraznění se zobrazí v prohlížeči:

![Změnit barvu zvýraznění](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Všimněte si, že jednoduše aktualizovat prohlížeč nástroj Page Inspector přímo z prostředí sady Visual Studio. Použití Page Inspectoru místo externího prohlížeče umožňuje zůstat v editoru při vývoji webových aplikací.

## <a name="see-also"></a>Viz také

[Představení nástroje Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (video Channel 9)

[Chybové zprávy nástroje Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)
