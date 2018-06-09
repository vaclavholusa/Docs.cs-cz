---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Díky nástroji Page Inspector v architektuře ASP.NET MVC | Microsoft Docs
author: rick-anderson
description: Nástroj Page Inspector v sadě Visual Studio 2012 je nástroj pro vývoj webů s integrované prohlížeče. Vyberte libovolný element v integrovaném prohlížeči a nástroj Page Inspector i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034513"
---
<a name="using-page-inspector-in-aspnet-mvc"></a>Díky nástroji Page Inspector v architektuře ASP.NET MVC
====================
podle Tim Ammann

> Nástroj Page Inspector v sadě Visual Studio 2012 je nástroj pro vývoj webů s integrované prohlížeče. Vyberte libovolný element, v prohlížeči integrované a nástroj Page Inspector okamžitě klade důraz elementu zdroje a šablon stylů CSS. Můžete procházet všechny zobrazení MVC, rychle najít zdroje vykreslované značky a pomocí nástroje prohlížeče přímo v prostředí Visual Studio.
> 
> [Podívejte se Video](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> Tento kurz ukazuje, jak povolit režim kontroly a rychle najít a upravit kód a šablon stylů CSS v rámci svého webového projektu. Tento kurz používá projektu aplikace MVC, ale můžete použít také nástroje Page Inspector pro [webových formulářů](https://go.microsoft.com/?linkid=9802001) a dalších aplikací ASP.NET.
> 
> Tento kurz obsahuje následující části:
> 
> - [Požadavky](#_1_prerequisites)
> - [Vytvoření webové aplikace](#_2_creating_a)
> - [Použijte nástroj Page Inspector na Procházet a zobrazení](#_3_using_page)
> - [Povolit režim kontroly](#_4_inspection_mode)
> - [Použijte nástroj Page Inspector provádět změny kódu](#_5_using_page)
> - [Režim kontroly a okno HTML](#_6_inspection_mode)
> - [Zobrazení náhledu změn šablon stylů CSS v okně Styly](#_7_previewing_css)
> - [Automatická synchronizace šablon stylů CSS](#css_auto_sync)
> - [Výběr barvy šablon stylů CSS](#css_color_picker)
> - [Mapování elementů dynamické stránky JavaScript](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Požadavky

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) nebo [Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Chcete-li získat nejnovější verzi nástroje Page Inspector, použijte [instalačního programu webové platformy](https://go.microsoft.com/fwlink/?LinkId=255386) nainstalovat sadu Windows Azure SDK pro rozhraní .NET 2.0.


Nástroj Page Inspector je instalován s Microsoft Web Developer Tools. Nejnovější verze je 1.3. Zjištění verze máte, spusťte Visual Studio a vyberte **o sadě Microsoft Visual Studio** z **pomoci** nabídky.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Vytvoření webové aplikace

Nejprve vytvořte webovou aplikaci, která nástroj Page Inspector se bude používat. V sadě Visual Studio, vyberte **soubor** &gt; **nový projekt**. Na levé straně, rozbalte položku **Visual C#**, vyberte **webové**a potom vyberte **webové aplikace ASP.NET MVC4**.

![Nové aplikace ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Click **OK**.

V **nový ASP.NET MVC 4 projekt** dialogové okno, vyberte **Internetové aplikace**. Nechte **Razor** jako výchozí modul zobrazení.

![Nový projekt ASP.NET MVC – Internetové aplikace.](using-page-inspector-in-aspnet-mvc/_static/image4.png)

Aplikace se otevře v **zdroj** zobrazení.

![Nové aplikace ASP.NET MVC v zobrazení zdroje](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Teď, když máte aplikaci pro práci s, můžete použít nástroj Page Inspector prozkoumat a upravit ho.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Použijte nástroj Page Inspector na Procházet a zobrazení

V sadě Visual Studio 2012, kliknete pravým tlačítkem všechna zobrazení v projektu, vyberte **zobrazení v nástroj Page Inspector**, a nástroj Page Inspector se zjistit trasy a zobrazení stránky.

V **Průzkumníku řešení**, rozbalte **zobrazení** složku a potom **Domů** složky. Klikněte pravým tlačítkem na soubor Index.cshtml a zvolte **zobrazení v nástroj Page Inspector**.

![Zobrazení Index.cshtml v nástroj Page Inspector](using-page-inspector-in-aspnet-mvc/_static/image8.png)

Ve výchozím nastavení nástroj Page Inspector ukotven jako okno na levé straně prostředí Visual Studio. Pokud dáváte přednost, můžete ho jinde ukotvení nebo zrušení okna ukotvení. V tématu [postupy: rozvržení a dokování oken](https://msdn.microsoft.com/library/z4y0hsax.aspx).

Horní podokno okna nástroje Page Inspector ukazuje aktuální stránky v okně prohlížeče. V dolním podokně zobrazí stránku v značka jazyka HTML, společně s některé karty, která umožňují kontrolovat různé aspekty stránky. Dolní podokno je podobná [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) v aplikaci Internet Explorer.

![Aplikace ASP.NET MVC v nástroj Page Inspector](using-page-inspector-in-aspnet-mvc/_static/image10.png)

V tomto kurzu budete používat **HTML** a **styly** karet procházejte rychle a provést změny aplikace.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>Režim EnableInspection

Chcete-li nástroj Page Inspector uvést do režimu kontroly, klikněte na tlačítko **kontroly** tlačítko. V režimu kontroly když podržíte ukazatel myši nad libovolná součást na vykreslené stránce odpovídající zdrojový kód nebo kód zvýrazní.

![Přepnout režim kontroly](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Nyní přesuňte ukazatel myši nad různé části stránky v rámci nástroje Page Inspector. Jak to uděláte, nezmění na velké znaménko plus a zvýrazní se element pod:

![Ukazatele myši div.content obálky](using-page-inspector-in-aspnet-mvc/_static/image14.png)

Jako ukazatel myši přesunete, Visual Studio označuje odpovídající syntaxi Razor ve zdrojovém souboru. Pokud HTML element pochází z jiného zdrojový soubor, Visual Studio automaticky otevře soubor.

V nástroj Page Inspector **HTML** kartě se zobrazují HTML, který byl vytvořen z syntaxi Razor. Když přesouváte ukazatel myši, jsou vyznačené elementů HTML. **Styly** kartě se zobrazují pravidla šablon stylů CSS pro daný element.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Použijte nástroj Page Inspector provádět změny kódu

Nástroj Page Inspector umožňuje najít značek, jejichž umístění nemusí být zřejmé. Potom můžete upravit kód a zobrazit výsledné změny.

Chcete-li toto zobrazit, klikněte na tlačítko **kontroly** a posuňte se k dolnímu okraji stránky v okně nástroje Page Inspector.

Při přesunutí ukazatele myši do oblasti zápatí, otevře se nástroj Page Inspector \_Layout.cshtml souboru a zvýrazňuje části ke stránce rozložení, které jste vybrali. Jak vidíte, jsou zápatí je definována v souboru rozložení a není zobrazení.

![Zápatí stránky](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Nyní, přesuňte ukazatel myši na řádek s copyrightu <a id="a"> </a>Všimněte si. V \_stránku Layout.cshtml, odpovídající řádek zvýrazněna.

![Zápatí autorským řádek zvýrazněna](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Přidejte na konec řádku v nějaký text \_Layout.cshtml souboru.

&lt;p&gt;&amp;kopírovat; @DateTime.Now.Year – Moje aplikace ASP.NET MVC skály! &lt;/p&gt;

Nyní stiskněte Ctrl + Alt + Enter nebo klikněte na panelu aktualizace a zobrazte výsledky v okně prohlížeče nástroj Page Inspector.

![Moje aplikace skály ASP.NET!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Může mít představit zápatí definované v Index.cshtml, že v ukázalo \_Layout.cshtml a nástroj Page Inspector zjistila, že pro vás.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Režim kontroly a okno HTML

V dalším kroku bude mít rychlý pohled na okno HTML a jak mapuje prvky pro vás.

Klikněte na tlačítko **kontroly** uvést do režimu kontroly nástroje Page Inspector.

Klikněte na horní část stránky, která říká "logohere". Jsou zkoumání určitý element podrobněji tak zobrazení v okně prohlížeče už změní, když ukazatel myši přesunete.

Nyní přesunutí ukazatele myši **HTML** okno. Jako ukazatel myši přesunete, nástroj Page Inspector popisuje element v rámci **HTML** okno a zvýrazňuje odpovídající element v okně prohlížeče.

![Okno HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Jak před, otevře se nástroj Page Inspector \_Layout.cshtml souboru můžete na kartě dočasné. Klikněte na tlačítko \_Layout.cshtml dočasné kartě a odpovídající kód bude mít zvýrazněná v &lt;záhlaví&gt; části pro vás:

![Zvýrazněná značka](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Zobrazení náhledu změn šablon stylů CSS v okně Styly

V dalším kroku použijete nástroj Page Inspector **styly** okna zobrazení náhledu změn do šablon stylů CSS.

Klikněte na tlačítko **kontroly** uvést do režimu kontroly nástroje Page Inspector.

V okně prohlížeče nástroj Page Inspector, přesuňte ukazatel myši nad v části "Home Page", dokud **div.content obálku** se zobrazí popisek.

![Ukazatele myši div.content obálky](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Klikněte v části div.content obálku jednou a poté přesuňte ukazatel myši na **styly** okno. **Syles** v okně se zobrazí všechna pravidla šablon stylů CSS pro tento element. Posuňte se dolů a najít .featured .content obálku třída selektor. Nyní zrušte zaškrtnutí políčka pro vlastnost barvu pozadí.

![Barva pozadí vymazat](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Všimněte si, jak tato změna přináší náhled okamžitě v okně prohlížeče nástroj Page Inspector.

Zaškrtněte políčko znovu, klikněte dvakrát na hodnotu vlastnosti a změňte ji na červený. Změna ukazuje okamžitě:

![Barva pozadí Red](using-page-inspector-in-aspnet-mvc/_static/image30.png)

**Styly** okno díky usnadňuje testování a náhled šablon stylů CSS změní před potvrzením změny styl list sám sebe.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Automatická synchronizace šablon stylů CSS

> [!NOTE]
> Tato funkce vyžaduje 1.3 verzi nástroje Page Inspector.


Automatická synchronizace šablon stylů CSS funkce umožňuje přímo upravit soubor CSS a podívejte se změny okamžitě v prohlížeči nástroj Page Inspector.

Klikněte na tlačítko **kontroly** uvést do režimu kontroly nástroje Page Inspector.

V prohlížeči nástroj Page Inspector, přesuňte ukazatel myši nad v části "Home Page", dokud **div.content obálku** se zobrazí popisek. Klikněte na tlačítko jednou tento element vyberte.

**Syles** v okně se zobrazí všechna pravidla šablon stylů CSS pro tento element. Posuňte se dolů a najít .featured .content obálku třída selektor. Klikněte na ".featured .content obálku". Nástroj Page Inspector otevře soubor CSS, který definuje tento styl (Site.css) a zvýrazňuje odpovídající stylu CSS.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Teď změňte hodnotu `background-color` na "red". Změny se okamžitě zobrazí v prohlížeči nástroj Page Inspector.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>Výběr barvy šablon stylů CSS

Editor šablon stylů CSS v sadě Visual Studio 2012 má výběr barvy, která usnadňuje zvolte a vložit barvy. Výběr barvy obsahuje standardní paletu barev, podporuje názvy standardní barev, kódů hash, barvy RGB, RGBA, HSL a HSLA a udržuje seznam barev, které byly naposledy použity v dokumentu.

V předchozí části, můžete změnit hodnotu `background-color` vlastnost. K vyvolání volby barev, umístěte kurzor po název vlastnosti a typ **#** nebo **rgb (**.

![Na panelu Výběr barev šablon stylů CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Klikněte na barvu, která má vyberte ho a stiskněte klávesu šipka dolů a pak použijte klávesy se šipkami vlevo a vpravo procházení barvy. Při návštěvě barvy s odpovídající hodnotou šestnáctkových zobrazení náhledu:

![Hodnota vlastnosti Barva pozadí zobrazení náhledu](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Pokud pruhu barev nemá požadovanou barvu přesný, můžete na pop nabídku pro výběr barev. Otevřete ho kliknutím na dvojitou šipku na pravém konci pruhu barev nebo stiskněte klávesu šipka dolů jednou nebo dvakrát na klávesnici.

![Výběr rozbalovací Pop barva šablon stylů CSS](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Klikněte na barvu ze svislá čára na pravé straně. Ukazuje to přechodu pro tuto barvu v hlavním okně. Vybrat barvu přímo z panelu svislé stisknutím klávesy Enter nebo klikněte na libovolný bod v hlavním okně zvolit s vyšší přesností.

Pokud je na obrazovce počítače, který chcete použít barvu (nemusí to být v uživatelském rozhraní sady Visual Studio), jeho hodnota můžete zaznamenat pomocí nástroje kapátko vpravo dole.

Také můžete změnit krytí barvu přesunutím posuvníku v dolní části volby barev. V tom změny barvu hodnoty RGBA hodnoty, protože formát RGBA může představovat neprůhlednost.

Po výběru barvy, stiskněte klávesu Enter a pak zadejte středníkem k dokončení barvu pozadí položky v *Site.css* souboru.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Na stránce Inspector aktualizace panelu

Nástroj Page Inspector okamžitě zjistí změnu *Site.css* souboru a zobrazí výstrahu panelu aktualizaci.

![Aktualizace panelu](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Pokud chcete uložit všechny soubory a aktualizujte stránku prohlížeče nástroj Page Inspector, stiskněte Ctrl + Alt + Enter nebo klikněte na panelu aktualizace. Změna v barva zvýraznění se zobrazí v prohlížeči.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Mapování elementů dynamické stránky JavaScript

V moderních webových aplikací elementy na stránce jsou často generována dynamicky v jazyce JavaScript. To znamená, že neexistuje žádná statické značek (HTML nebo Razor), která odpovídá tyto prvky na stránce.

S verzí 1.3 můžete nástroj Page Inspector teď namapovat položky, které byly dynamicky přidat na stránku zpět na odpovídající kód jazyka JavaScript. K předvedení tuto funkci, použijeme [jedné stránky aplikace (SPA) šablony](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> Šablona SPA vyžaduje [ASP.NET a webové nástroje 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) aktualizovat.


V sadě Visual Studio, vyberte **soubor** &gt; **nový projekt**. Na levé straně, rozbalte položku **Visual C#**, vyberte **webové**a potom vyberte **webové aplikace ASP.NET MVC4**. Click **OK**.

V **nový ASP.NET MVC 4 projekt** dialogovém okně, vyberte **jedné stránky aplikace**.

V Průzkumníku řešení rozbalte **zobrazení** složku a potom **Domů** složky. Klikněte pravým tlačítkem na soubor Index.cshtml a zvolte **zobrazení v nástroj Page Inspector**.

Nejprve thing tedy zobrazené v prohlížeči nástroj Page Inspector je přihlašovací stránku. Klikněte na tlačítko "Zaregistrovat" a vytvořte uživatelské jméno a heslo. Po registraci aplikace můžete přihlásí a vytvoří seznam úkolů s některé ukázkové položky.

Klikněte na tlačítko **kontroly** uvést do režimu kontroly nástroje Page Inspector. V prohlížeči nástroj Page Inspector klikněte na jedné z položek seznamu úkolů. Všimněte si, že místo se modře zvýrazněný, element je označený na oranžová s "JS" vedle názvu elementu. To znamená, že element vytvořil dynamicky prostřednictvím skriptu.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Kromě toho se zobrazí oranžové podtržení na **zásobníkem volání** kartě. Znamená to, že **zásobníkem volání** podokno má další informace o elementu.

Klikněte na **zásobníkem volání** kartě. **Zásobníkem volání** podokně se zobrazují zásobníku volání pro volání jazyka JavaScript, který vytvořili elementu. Volá, aby externí knihovny, jako jsou sbaleny jQuery, takže můžete snadno vidět, volání do vaší aplikace skriptu.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Chcete-li zobrazit úplnou zásobníku, včetně volání externí knihovny, můžete rozbalte uzly s označením "Externí knihovny":

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Pokud kliknete na položku v zásobníku volání, Visual Studio otevře soubor kódu a označuje odpovídající skript.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Viz také

[Úvod do architektury ASP.NET MVC 4 pomocí sady Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (webu ASP.net)

[Představení nástroje Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (video na kanálu 9)

[Nástroj Page Inspector chybové zprávy](https://go.microsoft.com/?linkid=9813062) (MSDN)
