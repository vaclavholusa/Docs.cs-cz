---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Použití pomocné rutiny DropDownList s ASP.NET MVC | Dokumentace Microsoftu
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 6c55f37fc74c2aa1c9867abd8c281c88ee36fb92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365747"
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Použití pomocné rutiny DropDownList s ASP.NET MVC
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

V tomto kurzu se seznámíte se základy práce se [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) pomocné rutiny a [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) pomocné rutiny v aplikaci MVC rozhraní ASP.NET Web. Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio chcete postupovat podle tohoto kurzu můžete použít. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:

- [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [ASP.NET MVC 3 nástroje Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)

Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). V tomto kurzu se předpokládá dokončení [Úvod do ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) kurzu nebo[ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) kurzu nebo jste obeznámeni s vývoj pro ASP.NET MVC. Tento kurz pracuje s upravenou projekt z [ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) kurzu. Můžete si stáhnout počáteční projekt s následujícím odkazem [stáhnout verzi C#](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Projekt aplikace Visual Web Developer se dokončení kurzu zdrojový kód C# je k dispozici v tomto tématu. [Stáhněte si](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Co budete vytvářet

Vytvoříte metody akce a zobrazení, které používají [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) pomocné rutiny k výběru kategorie. Budete taky používat **jQuery** Přidat dialog kategorie vložit, který slouží k tomu je potřeba novou kategorii (jako je genre nebo interpreta). Níže je snímek obrazovky zobrazení vytvořit odkazy na přidání nového žánr a přidejte novou interpreta.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Dovednosti, které se dozvíte

Zde je, co se dozvíte:

- Jak používat [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) pomocná rutina pro výběr data v kategoriích.
- Postup přidání **jQuery** dialogové okno pro přidání nové kategorie.

### <a name="getting-started"></a>Začínáme

Začněte tím, že stahování počáteční projekt s následujícím odkazem [Stáhnout](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). V Průzkumníku Windows, klikněte pravým tlačítkem na *DDL\_Starter.zip* a vyberte možnost Vlastnosti. V **DDL\_Starter.zip vlastnosti** dialogové okno, vyberte zrušit blokování.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Klikněte pravým tlačítkem myši DDL\_Starter.zip soubor a vyberte **Extrahovat vše** dekomprimovat soubor. Otevřít *StartMusicStore.sln* soubor s aplikaci Visual Web Developer 2010 Express ("Visual Web Developer" nebo "VWD" zkráceně) nebo Visual Studio 2010.

Stisknutím kláves CTRL + F5 spusťte aplikaci a klikněte na tlačítko **Test** odkaz.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Vyberte **vyberte kategorii Movie (jednoduchý)** odkaz. Zobrazí se seznamu vyberte typ video s komedie vybrané hodnoty.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Klikněte pravým tlačítkem v prohlížeči a vyberte zobrazení zdroje. Kód HTML stránky se zobrazí. Následující kód ukazuje kód HTML pro select element.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Uvidíte, že má každá položka v seznamu vyberte hodnotu (0 pro akci, 1 pro Drama, komedie 2 a 3 pro Sci-fi) a zobrazovaný název (akce, Drama, komedie a Sci-fi). Výše uvedený kód je standard HTML pro vybraného seznamu.

Změňte seznamu příkazu select na Drama a klikněte **odeslat** tlačítko. Adresa URL v prohlížeči je `http://localhost:2468/Home/CategoryChosen?MovieType=1` a na stránce se zobrazí **jste vybrali: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Otevřít *Controllers\HomeController.cs* souboru a zkoumat `SelectCategory` metody.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

[DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) vyžaduje pomocné rutiny použitý k vytvoření seznamu výběru HTML **IEnumerable&lt;SelectListItem &gt;** , explicitně nebo implicitně. To znamená, můžete předat **IEnumerable&lt;SelectListItem &gt;**  explicitně na [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) pomocné rutiny nebo můžete přidat **IEnumerable&lt; SelectListItem &gt;**  k [objekt ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) použití stejného názvu **SelectListItem** jako vlastnost modelu. Při předávání v **SelectListItem** implicitně a explicitně je popsané v další části kurzu. Výše uvedený kód ukazuje nejjednodušší možný způsob, jak vytvořit **IEnumerable&lt;SelectListItem &gt;**  a jeho naplnění text a hodnoty. Poznámka: `Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) má [vybrané](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) vlastnost nastavena na hodnotu **true;** to způsobí, že vykreslený vyberte seznam, aby zobrazoval **komedie** jako vybranou položku v seznamu.

**IEnumerable&lt;SelectListItem &gt;**  vytvořené výše se přidá do [objekt ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) s názvem MovieType. To je, jak můžeme předávat **IEnumerable&lt;SelectListItem &gt;**  implicitně položky [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) pomocné rutiny, které je uvedeno níže.

Otevřít *Views\Home\SelectCategory.cshtml* souboru a prozkoumání značky.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

Na třetím řádku nastavíme na zobrazení/Shared rozložení/\_jednoduché\_Layout.cshtml, což je zjednodušenou verzi souboru standardní rozložení. Této funkce můžete zachovat zobrazení jsme vykreslení HTML jednoduché.

V této ukázce jsme nejsou změnu stavu aplikace, takže jsme se odeslat data s využitím **HTTP GET**, nikoli **HTTP POST**. V části W3C [rychlé kontrolní seznam pro výběr HTTP GET nebo POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Protože jsme se mění aplikace a publikování formuláře, používáme [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) přetížení, která umožňuje zadat metodu akce, kontroleru a formulář – metoda (**HTTP POST** nebo **HTTP GET**). Obvykle obsahují zobrazení [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) přetížení, které nepřijímá žádné parametry. Žádná verze parametr, výchozí hodnota je odesílání dat formuláře POST verzi stejnou metodu akce a kontroler.

Následující řádek

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

předává řetězcový argument pro **DropDownList** pomocné rutiny. Tento řetězec "MovieType" v našem příkladu provede dvě věci:

- Poskytuje klíč pro **DropDownList** pomocná rutina pro vyhledání **IEnumerable&lt;SelectListItem &gt;**  v **objekt ViewBag**.
- To je vázán na data MovieType element formuláře. Pokud je metoda odeslat **HTTP GET**, `MovieType` bude řetězec dotazu. Pokud je metoda odeslat **HTTP POST**, `MovieType` bude přidána v textu zprávy. Následující obrázek ukazuje řetězec dotazu s hodnotou 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Následující kód ukazuje `CategoryChosen` metoda byla odeslána formuláře.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Přejděte zpět na stránku test a vyberte **HTML SelectList** odkaz. HTML stránka vykresluje select element podobný jednoduchá zkušební stránku ASP.NET MVC. Klikněte pravým tlačítkem myši okno prohlížeče a vyberte **zobrazit zdroj**. Kód HTML pro seznamu příkazu select je v podstatě totožné. Zkušební kód HTML stránky, funguje jako metody akce ASP.NET MVC a zobrazení, které jsme testovali dříve.

### <a name="improving-the-movie-select-list-with-enums"></a>Vylepšení seznamu vyberte video s výčty

Pokud kategorie ve vaší aplikaci jsou pevně a nezmění, můžete využít výhod výčty, aby váš kód robustnější a jednodušší pro rozšíření. Když přidáte novou kategorii, správnou kategorii hodnota je generována. Kopírování a vkládání chyb se vyhnete, když přidat novou kategorii, ale nezapomeňte aktualizovat hodnotu kategorie.

Otevřít *Controllers\HomeController.cs* soubor a zkontrolujte následující kód:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

[Výčtu](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` zachycuje film čtyři typy. `SetViewBagMovieType` Metoda vytvoří **IEnumerable&lt;SelectListItem &gt;**  z `eMovieCategories` **výčtu**a nastaví `Selected` vlastnost z `selectedMovie` parametru. `SelectCategoryEnum` Metodu akce pomocí stejného zobrazení jako `SelectCategory` metody akce.

Přejděte na stránku Test a klikněte na `Select Movie Category (Enum)` odkaz. Tentokrát místo hodnotu (číslo) se zobrazí, řetězec představující výčet se zobrazí.

### <a name="posting-enum-values"></a>Účtování hodnot výčtu

Formuláře HTML se obvykle používají k odesílání dat na serveru. Následující kód ukazuje `HTTP GET` a `HTTP POST` verze `SelectCategoryEnumPost` metody.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Předáním `eMovieCategories` výčet `POST` metody jsme extrahovat hodnotu výčtu a řetězec výčtu. Spusťte ukázku a přejděte na stránku testu. Klikněte na `Select Movie Category(Enum Post)` odkaz. Vyberte typ filmů a pak klikněte na tlačítko Odeslat. Zobrazení se zobrazí hodnota a název filmu typu.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Vytvoření více vybrat Element části

[ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) pomocné rutiny HTML, vykreslení HTML `<select>` křížkem `multiple` atribut, který umožňuje uživatelům provést více výběrů. Přejděte na odkaz Test a pak vyberte **více vyberte zemi** odkaz. Vygenerované uživatelské rozhraní vám umožní vybrat více zemí. Na následujícím obrázku jsou vybrané Kanada a Azure China.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Zkoumání kódu MultiSelectCountry

Zkontrolujte následující kód z *Controllers\HomeController.cs* souboru.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

`GetCountries` Metoda vytvoří seznam zemí a předává jej do `MultiSelectList` konstruktoru. `MultiSelectList` Přetížení konstruktoru používané `GetCountries` výše uvedené metody přebírá čtyři parametry:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *položky*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) obsahující položky v seznamu. V příkladu výše, seznam zemí.
2. *dataValueField*: název vlastnosti v **IEnumerable** seznam, který obsahuje hodnotu. V příkladu výše `ID` vlastnost.
3. *dataTextField*: název vlastnosti v **IEnumerable** seznam, který obsahuje informace k zobrazení. V příkladu výše `name` vlastnost.
4. *selectedValues*: seznam vybraných hodnot.

V příkladu výše `MultiSelectCountry` metoda předává `null` hodnotu pro vybrané země, tak žádné země jsou vybrány, pokud se zobrazuje uživatelské rozhraní. Následující kód ukazuje kód Razor použitý k vykreslení `MultiSelectCountry` zobrazení.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

Pomocné rutiny HTML [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) metoda využité nad provést dva parametry, název vlastnosti, která má vazbu modelu a [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) obsahující vyberte možnosti a hodnoty. `ViewBag.YouSelected` Výše uvedený kód slouží k zobrazení hodnot země vybrané při odeslání formuláře. Prozkoumejte přetížení HTTP POST `MultiSelectCountry` metody.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

`ViewBag.YouSelected` Dynamické vlastnosti obsahuje vybraných zemích, získat pro `Countries` položka v kolekci formuláře. V této verzi je předán metodě GetCountries seznam vybraných zemích, takže když `MultiSelectCountry` zobrazení, vybraných zemích, které jsou vybrány v uživatelském rozhraní.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Provádění vybrat Element popisný pomocí modulu plug-in jQuery Harvestu zvolené

Sklizeň [zvolené](http://harvesthq.github.com/chosen/) modulu plug-in jQuery lze přidat do HTML &lt;vyberte&gt; – element pro vytvoření uživatele popisný uživatelského rozhraní. Následující obrázky ukazují sklizeň [zvolené](http://harvesthq.github.com/chosen/) modulu plug-in jQuery s `MultiSelectCountry` zobrazení.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

V níže, dvě image **Kanada** zaškrtnuto.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

Na obrázku výše je vybrán Kanada a obsahuje **x** můžete kliknout na odebrat výběr. Následující obrázek ukazuje Kanada, Číny, a Japonsko vybrali.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Zapojování Harvestu zvolené jQuery modulu plug-in

V následující části se usnadňuje její sledování, pokud máte nějaké zkušenosti s jQuery. Pokud jste jQuery před nikdy nepoužívali, můžete zkusit najít v některém z následujících kurzů jQuery.

- [Jak funguje jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) podle [Resig Jan](http://ejohn.org/)
- [Začínáme s jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) podle [Jörn Zaefferer](http://bassistance.de/)
- [Live příklady jQuery](http://codylindley.com/blogstuff/js/jquery/#) podle [zhruba Lindley](http://codylindley.com/)

Zvolený modul plug-in je součástí starter a dokončené ukázkové projekty, které nejsou poskytnuty v tomto kurzu. Pro účely tohoto kurzu je pouze potřeba použít k připojení do uživatelského rozhraní jQuery. Použití modulu plug-in jQuery Harvestu zvolené v projektu aplikace ASP.NET MVC, musíte mít:

1. Stáhněte si modul plug-in zvolené z [githubu](https://github.com/harvesthq/chosen/). Tento krok se provede za vás.
2. Přidáte vybranou složku do projektu ASP.NET MVC. Přidáte prostředky z zvolený modul plug-in, které jste si stáhli v předchozím kroku, a na vybranou složku. Tento krok se provede za vás.
3. Připojení zvolený modul plug-in, který **DropDownList** nebo **ListBox** pomocné rutiny HTML.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Zapojování zvolený modul plug-in MultiSelectCountry zobrazení.

Otevřít *Views\Home\MultiSelectCountry.cshtml* a přidejte `htmlAttributes` parametr `Html.ListBox`. Parametr přidáte obsahuje název třídy pro výběr seznamu (`@class = "chzn-select"`). Dokončený kód je zobrazena níže:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

Ve výše uvedeném kódu, přidáváme atributu HTML a hodnota atributu `class = "chzn-select"`. Znaku @ před třída nemá nic společného s zobrazovací modul Razor. `class` je [– klíčové slovo jazyka C#](https://msdn.microsoft.com/library/x53a06bb.aspx). Klíčová slova jazyka C# nelze použít jako identifikátory, pokud patří mezi ně jako předponu. V příkladu výše `@class` je platný identifikátor, ale **třídy** není, protože **třídy** je klíčové slovo.

Přidat odkazy *Chosen/chosen.jquery.js* a *Chosen/chosen.css* soubory. *Chosen/chosen.jquery.js* a implementuje funkčně modulu plug-in zvolená. *Chosen/chosen.css* soubor obsahuje stylu. Přidat tyto odkazy do dolní části *Views\Home\MultiSelectCountry.cshtml* souboru. Následující kód ukazuje, jak odkazovat na modul plug-in zvolená.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Aktivace modulu plug-in zvolené pomocí názvu třídy používané **Html.ListBox** kódu. V předchozím příkladu je název třídy `chzn-select`. Přidejte následující řádek do dolní části *Views\Home\MultiSelectCountry.cshtml* zobrazení souboru. Tento řádek aktivuje zvolený modul plug-in.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

Následující řádek je syntaxe pro volání funkce připravené jQuery, který vybere modelu DOM element s názvem třídy `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Zabalená pak nastavte vrácené z volání nahoře použije vybranou metodu (`.chosen();`), který zavěšení do vybraného modulu plug-in.

Následující kód ukazuje dokončenou *Views\Home\MultiSelectCountry.cshtml* zobrazení souboru.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Spusťte aplikaci a přejděte `MultiSelectCountry` zobrazení. Vyzkoušejte přidávání a odstraňování zemích. K dispozici vzorku ke stažení obsahuje také `MultiCountryVM` metody a zobrazení, která implementuje funkce MultiSelectCountry pomocí zobrazení modelu místo **objekt ViewBag**.

V další části uvidíte, jak funguje mechanismu generování uživatelského rozhraní ASP.NET MVC s **DropDownList** pomocné rutiny.

> [!div class="step-by-step"]
> [Next](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
