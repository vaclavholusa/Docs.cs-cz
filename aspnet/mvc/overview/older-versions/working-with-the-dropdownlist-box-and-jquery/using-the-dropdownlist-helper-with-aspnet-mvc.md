---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Využitím pomocné rutiny rozevírací seznam s architekturou ASP.NET MVC | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 21373deeded801c5cea9e89f6dac0f3542a55ca5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Využitím pomocné rutiny rozevírací seznam s architekturou ASP.NET MVC
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

V tomto kurzu se naučit se základy práce s [rozevírací seznam](https://msdn.microsoft.com/library/dd492948.aspx) pomocné rutiny a [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) pomocné rutiny v aplikaci ASP.NET MVC Web. Můžete použít Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio k postupovat v kurzu. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:

- [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [Aktualizace nástrojů rozhraní ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)

Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). Tento kurz předpokládá, že jste dokončili [Úvod do architektury ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) kurzu nebo[ASP.NET MVC Hudba úložiště](../mvc-music-store/mvc-music-store-part-1.md) kurzu nebo jste se seznámili s ASP.NET MVC vývoj. V tomto kurzu začíná upravené projekt z [ASP.NET MVC Hudba úložiště](../mvc-music-store/mvc-music-store-part-1.md) kurzu. Projekt starter se na následující odkaz můžete stáhnout [stáhnout verzi jazyka C#](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Projekt Visual Web Developer dokončené kurzu zdrojový kód C# je k dispozici v tomto tématu. [Stáhněte si](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Co budete sestavení

Vytvoříte metody akce a zobrazení, které používají [rozevírací seznam](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) pomocná rutina pro vyberte kategorii. Budete taky používat **jQuery** Přidat dialog kategorie vložení, který lze použít, pokud je potřeba novou kategorii (jako je genre nebo umělcem). Níže je snímek obrazovky zobrazení vytvořit odkazy na přidejte nové genre a přidejte nový umělcem.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Dovedností, které se dozvíte

Zde je, co se dozvíte:

- Postup použití [rozevírací seznam](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) pomocné rutiny, vyberte kategorii data.
- Postup přidání **jQuery** dialogové okno pro přidání nové kategorie.

### <a name="getting-started"></a>Začínáme

Začněte tím, že stahování starter projekt se na následující odkaz [Stáhnout](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). V Průzkumníku Windows, klikněte pravým tlačítkem na *DDL\_Starter.zip* soubor a vyberte možnost Vlastnosti. V **DDL\_Starter.zip vlastnosti** dialogové okno, vyberte odblokovat.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Klikněte pravým tlačítkem na DDL\_Starter.zip soubor a vyberte **Extrahovat vše** soubor rozbalit. Otevřete *StartMusicStore.sln* soubor s Visual Web Developer 2010 Express ("Visual Web Developer" nebo "VWD" pro zkrácení) nebo Visual Studio 2010.

Stiskněte klávesu CTRL + F5 a spusťte aplikaci a klikněte na tlačítko **Test** odkaz.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Vyberte **vyberte kategorii Movie (jednoduchý)** odkaz. Zobrazí se seznam vyberte typ film s komedie vybrané hodnoty.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Klikněte pravým tlačítkem v prohlížeči a vyberte zobrazení zdroje. Kód HTML stránky se zobrazí. Následující kód ukazuje HTML pro select element.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Uvidíte, že každá položka v seznamu select má hodnotu (0 pro typ akce, 1 pro obrázkům, pro komedie 2 a 3 pro vědecké účely Fiction) a zobrazované jméno (akci, obrázkům, komedie a vědecké účely Fiction). Výše uvedený kód je standardní HTML pro v seznamu select.

Změna seznamu příkazu select k obrázkům a stiskněte klávesu **odeslání** tlačítko. Adresa URL v prohlížeči je `http://localhost:2468/Home/CategoryChosen?MovieType=1` a na stránce se zobrazuje **jste vybrali: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Otevřete *Controllers\HomeController.cs* soubor a zkontrolujte `SelectCategory` metoda.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

[Rozevírací seznam](https://msdn.microsoft.com/library/dd492738.aspx) vyžaduje Pomocník použít k vytvoření seznamu výběru HTML **rozhraní IEnumerable&lt;SelectListItem &gt;** , explicitně nebo implicitně. To znamená, můžete předat **rozhraní IEnumerable&lt;SelectListItem &gt;**  explicitně na [rozevírací seznam](https://msdn.microsoft.com/library/dd492738.aspx) pomocné rutiny, nebo můžete přidat **rozhraní IEnumerable&lt; SelectListItem &gt;**  k [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) použití stejného názvu **SelectListItem** jako vlastnost modelu. Předávání v **SelectListItem** implicitně a explicitně je popsaná v další části kurzu. Výše uvedený kód ukazuje nejjednodušší možný způsob, jak vytvořit **rozhraní IEnumerable&lt;SelectListItem &gt;**  a jeho naplnění hodnoty a text. Poznámka: `Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) má [vybrané](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) vlastnost nastavena na hodnotu **true;** to způsobí, že vykreslené seznamu výběru zobrazit **komedie** jako vybraná položka v seznamu.

**Rozhraní IEnumerable&lt;SelectListItem &gt;**  vytvořit vyšší je přidán do [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) s názvem MovieType. Toto je, jak jsme předat **rozhraní IEnumerable&lt;SelectListItem &gt;**  implicitně na [rozevírací seznam](https://msdn.microsoft.com/library/dd492738.aspx) pomocná vidíte níže.

Otevřete *Views\Home\SelectCategory.cshtml* soubor a zkontrolujte kód.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

Na třetí řádek nastaví rozložení na zobrazení/Shared/\_jednoduché\_Layout.cshtml, což je zjednodušenou verzi souboru standardní rozložení. Jsme to zachovat zobrazení a vykreslení HTML jednoduché.

V této ukázce jsme nejsou změny stavu aplikace, takže jsme budou odesílat data pomocí **HTTP GET**, nikoli **HTTP POST**. Najdete v části W3C [rychlé kontrolní seznam pro výběr HTTP GET nebo POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Protože jsme nejsou změna aplikace a publikování formuláře, použijeme [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) přetížení, které umožňuje zadejte metodu akce, kontroleru a formuláře – metoda (**HTTP POST** nebo **HTTP GET**). Obvykle obsahovat zobrazení [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) přetížení, které nepřijímá žádné parametry. Žádná parametr verze výchozí publikování dat formuláře na verzi POST stejnou metodu akce a kontroler.

Následující řádek

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

předá argument řetězce k **rozevírací seznam** pomocné rutiny. Tento řetězec "MovieType" v našem příkladu provádí dvě věci:

- Poskytuje klíč pro **rozevírací seznam** pomocná rutina pro vyhledání **rozhraní IEnumerable&lt;SelectListItem &gt;**  v **ViewBag**.
- Ho je vázané na data pro MovieType form element. Pokud je metoda odeslání **HTTP GET**, `MovieType` bude řetězec dotazu. Pokud je metoda odeslání **HTTP POST**, `MovieType` bude přidán v textu zprávy. Následující obrázek znázorňuje řetězec dotazu s hodnotou 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Následující kód ukazuje `CategoryChosen` metoda byla odeslána formuláře.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Přejděte zpět na zkušební stránku a vyberte **HTML SelectList** odkaz. Vykreslí stránku HTML vybrat element podobná jednoduché zkušební stránku ASP.NET MVC. Klikněte pravým tlačítkem na okno prohlížeče a vyberte **zobrazit zdroj**. Kód HTML pro seznamu příkazu select je v podstatě stejné. Zkušební kód HTML stránky, funguje jako metodu akce ASP.NET MVC a zobrazení, které jsme testovali dříve.

### <a name="improving-the-movie-select-list-with-enums"></a>Vylepšení seznamu vyberte video s výčty

Pokud kategorie v aplikaci umístěny pevně a nedojde ke změně, můžete využít výhod výčty, aby váš kód více robustní a jednodušší pro rozšíření. Když přidáte novou kategorii, je generována hodnota správné kategorie. Zabraňuje kopírování a vkládání chyby při přidání nové kategorie ale zapomenete aktualizovat hodnotu kategorie.

Otevřete *Controllers\HomeController.cs* soubor a zkontrolujte následující kód:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

[Výčtu](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` zaznamená film čtyři typy. `SetViewBagMovieType` Metoda vytvoří **rozhraní IEnumerable&lt;SelectListItem &gt;**  z `eMovieCategories` **výčtu**a nastaví `Selected` vlastnost z `selectedMovie` parametr. `SelectCategoryEnum` Akce metoda používá stejný zobrazení jako `SelectCategory` metody akce.

Přejděte na stránku Test a klikněte na `Select Movie Category (Enum)` odkaz. Tentokrát místo hodnotu (number) se zobrazuje, řetězec představující je výčet se zobrazí.

### <a name="posting-enum-values"></a>Publikování hodnot výčtu

Formuláře HTML jsou obvykle používány k odesílání dat na serveru. Následující kód ukazuje `HTTP GET` a `HTTP POST` verzích `SelectCategoryEnumPost` metoda.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Předáním `eMovieCategories` výčtu k `POST` metoda, jsme můžete rozbalit hodnotou výčtu a řetězec výčtu. Spuštění vzorového a přejděte na stránku testu. Klikněte na `Select Movie Category(Enum Post)` odkaz. Vyberte typ film a poté stiskněte tlačítko pro odeslání. Zobrazení zobrazuje hodnota a název typu film.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Vytvoření více vybrat Element části

[ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) pomocné rutiny HTML vykreslí HTML `<select>` element s `multiple` atribut, který umožňuje uživatelům provést více výběrů. Přejděte na odkaz Test a pak vyberte **více vyberte zemi** odkaz. Rozhraní vykreslené vám umožní vybrat více zemích. Na následujícím obrázku jsou vybrány Kanady a Číny.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Zkoumání kódu MultiSelectCountry

Zkontrolujte následující kód *Controllers\HomeController.cs* souboru.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

`GetCountries` Metoda vytvoří seznam zemí, a potom předává jej do `MultiSelectList` konstruktor. `MultiSelectList` Přetížení konstruktoru, které jsou používány `GetCountries` metoda výše má čtyři parametry:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *položky*: [rozhraní IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) obsahující položky v seznamu. V příkladu výše, seznamu zemích.
2. *dataValueField*: název vlastnosti v **rozhraní IEnumerable** seznamu, který obsahuje hodnotu. V příkladu nahoře `ID` vlastnost.
3. *dataTextField*: název vlastnosti v **rozhraní IEnumerable** seznamu, který obsahuje informace k zobrazení. V příkladu nahoře `name` vlastnost.
4. *selectedValues*: seznam vybraných hodnot.

V příkladu nahoře `MultiSelectCountry` metoda předává `null` hodnotu pro vybrané země, takže žádná zemích jsou vybrané, když se zobrazí uživatelské rozhraní. Následující kód ukazuje kód Razor použitý k vykreslení `MultiSelectCountry` zobrazení.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

Pomocné rutiny HTML [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) metodu použít výše proveďte dva parametry, název vlastnosti, která má vazby modelu a [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) obsahující vyberte možnosti a hodnoty. `ViewBag.YouSelected` Výše uvedený kód se používá k zobrazení hodnot zemí jste vybrali při odeslání formuláře. Zkontrolujte přetížení HTTP POST `MultiSelectCountry` metoda.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

`ViewBag.YouSelected` Dynamických vlastností obsahuje vybrané země, získat pro `Countries` položku v kolekci formuláře. V této verzi je předán metodu GetCountries seznam vybraných zemích, takže když `MultiSelectCountry` se zobrazí, vybrané zemí jsou vybrány v uživatelském rozhraní.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Vytváření vyberte Element popisný s jQuery sklizně zvolený modulu plug-in

Sklizeň [zvolený](http://harvesthq.github.com/chosen/) modulu plug-in jQuery mohou být přidány do HTML &lt;vyberte&gt; elementu, který chcete vytvořit uživatele popisný uživatelského rozhraní. Následující obrázky ukazují sklizeň [zvolený](http://harvesthq.github.com/chosen/) modulu plug-in jQuery s `MultiSelectCountry` zobrazení.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

V níže, dvě bitové kopie **Kanada** je vybrána.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

Na obrázku výše je vybrána Kanada, která obsahuje **x** můžete kliknout na odebrat výběr. Následující obrázek ukazuje Kanada, Čína, a vybraná Japonsku.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Zapojování jQuery sklizně zvolený modulu plug-in

V následující části je snazší řídí, pokud máte nějaké zkušenosti s jQuery. Pokud jste použili nikdy jQuery před, můžete chtít použijte jeden z následujících kurzů jQuery.

- [Jak funguje jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) podle [Resig Jan](http://ejohn.org/)
- [Začínáme s jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) podle [Jörn Zaefferer](http://bassistance.de/)
- [Live příklady jQuery](http://codylindley.com/blogstuff/js/jquery/#) podle [Codymu Lindley](http://codylindley.com/)

Modul plug-in zvolený je součástí starter a dokončené ukázkové projekty doprovázejících tohoto kurzu. V tomto kurzu bude pouze muset použít jQuery napojit na rozhraní. Chcete-li používat modul plug-in jQuery sklizně zvolená v projektu ASP.NET MVC, postupujte takto:

1. Stáhnout modul plug-in zvolený z [githubu](https://github.com/harvesthq/chosen/). Tento krok bylo provedeno za vás.
2. Přidejte zvolenou složku do projektu ASP.NET MVC. Přidáte prostředky z zvolenou modul plug-in, které jste stáhli v předchozím kroku do složky, vybrat. Tento krok bylo provedeno za vás.
3. Propojte vybraný modul plug-in, který **rozevírací seznam** nebo **ListBox** pomocné rutiny HTML.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Zapojování modulu plug-in zvolený MultiSelectCountry zobrazení.

Otevřete *Views\Home\MultiSelectCountry.cshtml* souboru a přidejte `htmlAttributes` parametru `Html.ListBox`. Parametr přidáte obsahuje název třídy pro seznamu pro výběr (`@class = "chzn-select"`). Dokončený kód je zobrazena níže:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

Ve výše uvedeném kódu, přidáváme atribut HTML a hodnota atributu `class = "chzn-select"`. Znak @ předcházející třída nijak nesouvisí s zobrazovací modul Razor. `class` je [C# – klíčové slovo](https://msdn.microsoft.com/library/x53a06bb.aspx). Klíčová slova jazyka C# nelze použít jako identifikátory, pokud patří mezi ně jako předponu. V příkladu nahoře `@class` je platný identifikátor ale **třída** není, protože **třída** je klíčové slovo.

Přidejte odkazy na *Chosen/chosen.jquery.js* a *Chosen/chosen.css* soubory. *Chosen/chosen.jquery.js* a implementuje funkčně z modulu plug-in zvolená. *Chosen/chosen.css* soubor poskytuje stylu. Přidat tyto odkazy v dolní části *Views\Home\MultiSelectCountry.cshtml* souboru. Následující kód ukazuje, jak chcete-li vybrat modulu plug-in.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Aktivovat zvolený modul plug-in, použít název třídy, které jsou používány **Html.ListBox** kódu. V předchozím příkladu název třídy je `chzn-select`. Přidejte následující řádek na konec *Views\Home\MultiSelectCountry.cshtml* zobrazení souboru. Tento řádek aktivuje zvolený modulu plug-in.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

Následující řádek je syntaxe pro volání funkce připravené jQuery, který vybere elementu DOM s názvem třídy `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Zabalená nastavit vrácená z výše uvedených volání pak použije vybranou metodu (`.chosen();`), který zachytí až zvolený modulu plug-in.

Následující kód ukazuje dokončené *Views\Home\MultiSelectCountry.cshtml* zobrazení souboru.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Spusťte aplikaci a přejděte do `MultiSelectCountry` zobrazení. Zkuste přidávání a odstraňování zemích. Stažení ukázkové poskytuje také obsahuje `MultiCountryVM` metoda a zobrazení, která implementuje funkce MultiSelectCountry pomocí zobrazení modelu místo **ViewBag**.

V další části se zobrazí, jak funguje mechanismus generování uživatelského rozhraní ASP.NET MVC pomocí **rozevírací seznam** pomocné rutiny.

> [!div class="step-by-step"]
> [Next](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
