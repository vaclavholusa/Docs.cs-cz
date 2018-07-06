---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Použití jazyka HTML5 a kalendáře jQuery UI Datepicker s architekturou ASP.NET MVC – část 4 | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy práce pomocí editoru šablon, šablony zobrazení a kalendářem jQuery uživatelského rozhraní prvkem datepicker v MV ASP.NET...
ms.author: aspnetcontent
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: d50ce517df9bd834203cbd026d31db038cd02038
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835729"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Použití jazyka HTML5 a kalendáře jQuery UI Datepicker s architekturou ASP.NET MVC – část 4
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu se seznámíte se základy práce pomocí editoru šablon, šablony zobrazení a kalendářem jQuery UI datepicker v aplikaci MVC rozhraní ASP.NET Web.


### <a name="adding-a-template-for-editing-dates"></a>Přidání šablony pro úpravu dat

V této části vytvoříte šablonu pro úpravu dat, která bude použita, pokud rozhraní ASP.NET MVC zobrazí uživatelské rozhraní pro úpravy vlastnosti modelu, které jsou označeny **datum** výčet [datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atribut. Šablony budou vykreslovat pouze data; čas se nezobrazí. V šabloně použijete [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) překryvný kalendář poskytnout způsob, jak upravit data.

Pokud chcete začít, otevřete *Movie.cs* a přidejte [datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributem **datum** výčet `ReleaseDate` vlastnost, jak je znázorněno v následujícím kódu:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Tento kód způsobí, že `ReleaseDate` pole, který se má zobrazit bez času v obou zobrazit šablony a upravte šablony. Pokud vaše aplikace obsahuje *date.cshtml* šablony v *views\shared\editortemplates za účelem nalezení* složky nebo *Views\Movies\EditorTemplates* složky, tato šablona se použije k vykreslení žádné `DateTime` vlastnost při úpravách. Jinak se zobrazí předdefinovaný systémový šablon ASP.NET vlastnost jako datum.

Stisknutím kláves CTRL + F5 spusťte aplikaci. Vyberte odkaz pro úpravy k ověření, že se vstupní pole pro datum vydání zobrazuje pouze datum.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

V **Průzkumníka řešení**, rozbalte *zobrazení* složky, rozbalte *Shared* složku a potom klikněte pravým tlačítkem *views\shared\editortemplates za účelem nalezení* složky.

Klikněte na tlačítko **přidat**a potom klikněte na tlačítko **zobrazení**. **Přidat zobrazení** zobrazí dialogové okno.

V **název zobrazení** zadejte &quot;datum&quot;.

Vyberte **vytvořit jako částečné zobrazení** zaškrtávací políčko. Ujistěte se, že **použít rozložení stránky předlohy** a **vytvoření zobrazení se silnými typy** nejsou zaškrtnutá políčka.

Klikněte na tlačítko **přidat**. *Views\Shared\EditorTemplates\Date.cshtml* šablona vytvořena.

Přidejte následující kód, který *Views\Shared\EditorTemplates\Date.cshtml* šablony.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

První řádek deklaruje modelu, který má být `DateTime` typu. I když není nutné deklarovat typ modelu v režimu úpravy a zobrazení šablony, je osvědčeným postupem tak, aby získáte kompilace kontroly modelu předávaný do zobrazení. (Další výhodou je, potom získá IntelliSense pro model v zobrazení v sadě Visual Studio.) Pokud není deklarovaný typ modelu, ASP.NET MVC považuje [dynamické](https://msdn.microsoft.com/library/dd264741.aspx) zadejte a neexistuje žádná kompilace kontroly typu. Pokud deklarujete modelu, který má být `DateTime` typ, stane se silnými typy.

Druhý řádek je pouze literál značka jazyka HTML, který zobrazuje &quot;pomocí šablony datum&quot; před pole pro datum. Tento řádek dočasně budete používat k ověření, že tato šablona data používá.

Další řádek je [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) pomocné rutiny, která vykresluje `input` pole, které je textové pole. Třetí parametr pro pomocné rutiny používá nastavení třídy pro textové pole anonymního typu `datefield` a typ, který má `date`. (Protože `class` je vyhrazeným v jazyce C#, budete muset použít `@` znak pro přepnutí `class` atribut v analyzátor jazyka C#.)

`date` Typ je typem vstupu HTML5 umožňující s ohledem na HTML5 prohlížeče k vykreslení ovládacího prvku Kalendář HTML5. Později přidáte určitého kódu JavaScript pro ovládací prvek datepicker jQuery pro připojení `Html.TextBox` pomocí elementu `datefield` třídy.

Stisknutím kláves CTRL + F5 spusťte aplikaci. Můžete ověřit, že `ReleaseDate` vlastnost v zobrazení pro úpravy je pomocí šablony upravit, protože šablona zobrazuje &quot;pomocí šablony datum&quot; těsně před `ReleaseDate` text vstupní pole, jak je znázorněno na tomto obrázku:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

V prohlížeči zobrazte zdroj stránky. (Například, klikněte pravým tlačítkem na stránku a vybrat **zobrazit zdroj**.) Následující příklad ukazuje některé z kódu pro na stránce ilustrující `class` a `type` atributy v zobrazený HTML.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Vraťte se na *Views\Shared\EditorTemplates\Date.cshtml* šablony a odebrat &quot;pomocí šablony datum&quot; značek. Dokončená šablona teď vypadá takto:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Přidání kalendáře jQuery UI Datepicker pomocí nástroje NuGet

V této části přidáte [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) kalendáře datum úpravy šablony. [Uživatelské rozhraní jQuery](http://jqueryui.com/) knihovna poskytuje podporu pro animaci, pokročilé efekty a přizpůsobitelné pomůcky. Je nástavbou knihovna jQuery JavaScript. Překryvný kalendář s prvkem datepicker zajišťuje snadné a přirozené kalendářních pomocí kalendáře nemusí zadávat řetězec. Překryvný kalendář také uživatele omezuje na právní data – běžný text vstupu pro datum by umožňují zadat něco jako `2/33/1999` (únor 33rd, 1999), ale [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) překryvný kalendář, který neumožňuje.

Nejprve je nutné nainstalovat knihovny uživatelského rozhraní jQuery. K tomuto účelu používáte NuGet, který je Správce balíčků, který je součástí verze SP1 Visual Studio 2010 a Visual Web Developer.

V aplikaci Visual Web Developer z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven** a pak vyberte **spravovat balíčky NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Poznámka: Pokud **nástroje** nabídek nezobrazuje **Správce balíčků knihoven** příkaz, je potřeba nainstalovat NuGet podle pokynů v [instalace balíčků NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) stránky na webu NuGet.   
  
Pokud používáte Visual Studio místo aplikace Visual Web Developer z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven** a pak vyberte **přidat odkaz na balíček knihovny**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

V **MVCMovie – spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **Online** kartu na levé straně a pak zadejte &quot;jQuery.UI&quot; do vyhledávacího pole. Vyberte j **dotaz pomůcky: prvku Datepicker uživatelského rozhraní**a pak **nainstalovat** tlačítko.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet do vašeho projektu přidá tyto ladicí verze a minifikovaný verze základní uživatelské rozhraní jQuery a výběr data uživatelské rozhraní jQuery:

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

Poznámka: Verze ladění (soubory bez *. min.js* rozšíření) jsou užitečné pro ladění, ale v produkční lokality, bude zahrnovat pouze minifikovaný verze.

Pro výběr data jQuery skutečně používáte, je potřeba vytvořit jQuery skript, který se bude připojení kalendáře widgetu upravit šablonu. V **Průzkumníka řešení**, klikněte pravým tlačítkem na *skripty* a pak zvolte položku **přidat**, pak **nová položka**a potom **JScript Soubor**. Název souboru *DatePickerReady.js*.

Přidejte následující kód, který *DatePickerReady.js* souboru:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Pokud nejste obeznámeni s jQuery, tady je stručné vysvětlení toho, co to dělá: první řádek je &quot;jQuery připravené&quot; funkce, která je volána, když jste načetli všechny elementy modelu DOM na stránce. Druhý řádek vybere všechny elementy modelu DOM, které mají název třídy `datefield`, potom se vyvolá `datepicker` funkce pro každý z nich. (Mějte na paměti, že jste přidali `datefield` třídu *Views\Shared\EditorTemplates\Date.cshtml* šablony dříve v tomto kurzu.)

Dále otevřete *Views\Shared\\_Layout.cshtml* souboru. Je třeba přidat odkazy na následující soubory, které jsou všechny povinné, takže budete používat výběr data:

- *Content/Themes/Base/jQuery.UI.Core.CSS*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/Themes/Base/jQuery.UI.theme.CSS*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

Následující příklad ukazuje skutečný kód, měli byste přidat v dolní části `head` prvek *Views\Shared\\_Layout.cshtml* souboru.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Kompletní `head` části je znázorněna zde:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

[Obsahu pomocný objekt adresy URL](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) metoda převede cestu prostředku na absolutní cestu. Je nutné použít `@URL.Content` správně odkazovat na tyto prostředky ve službě IIS je spuštěná aplikace.

Stisknutím kláves CTRL + F5 spusťte aplikaci. Vyberte odkaz pro úpravy a umístěte kurzor do **ReleaseDate** pole. Zobrazí se kalendářem jQuery UI.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Stejně jako většinu ovládací prvky jQuery ovládací prvek datepicker vám umožní přizpůsobit neúmyslné. Informace najdete v tématu [Visual přizpůsobení: navrhování motiv uživatelského rozhraní jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) na [uživatelské rozhraní jQuery](http://learn.jquery.com/jquery-ui/getting-started/) lokality.

### <a name="supporting-the-html5-date-input-control"></a>Podpora HTML5 ovládacího prvku vstupní data

Jako další prohlížeče podporují HTML5, budete chtít používat nativní HTML5, jako vstup `date` elementu input a nepoužívat kalendáře jQuery UI. Můžete přidat logiku pro vaši aplikaci automaticky používat ovládací prvky jazyka HTML5, pokud je prohlížeč podporuje. Chcete-li to provést, nahraďte obsah *DatePickerReady.js* souboru následujícím kódem:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

První řádek tento skript používá Modernizr ověřte, že je podporované vstupní data HTML5. Pokud není podporován, výběr data uživatelské rozhraní jQuery připojeno místo toho. ([Modernizr](http://www.modernizr.com/docs/) open source knihovnu JavaScript, který zjišťuje dostupnost nativní implementace jazyka HTML5 a CSS3. Modernizr je zahrnuta v nových projektech ASP.NET MVC, které vytvoříte.)

Po provedení této změny, takže ji můžete otestovat pomocí prohlížeče, který podporuje HTML5, jako je například Opera 11. Spusťte aplikaci pomocí prohlížeče kompatibilní s HTML5 a upravte položku video. Ovládací prvek datum HTML5 se používá namísto kalendářem jQuery UI:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Vzhledem k tomu, že nová verze prohlížečů jsou postupně implementace HTML5, dobrý nápad zatím je přidání kódu na web, který přizpůsobuje širokou škálu podporou HTML5. Například robustní více *DatePickerReady.js* skriptu je uveden níže, která umožňuje vašeho webu podpory prohlížečích jen částečně podpory HTML5 ovládacího prvku datum.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Tento skript vybere HTML5 `input` prvky typu `date` , která plně nepodporují HTML5 ovládacího prvku datum. Pro tyto elementy hooks nahoru kalendářem jQuery UI a pak se změní `type` atribut z `date` k `text`. Změnou `type` atribut z `date` k `text`, částečně se podporuje HTML5 data se odstraní. Ještě více robustní *DatePickerReady.js* skriptu lze nalézt v [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Přidání data s možnou hodnotou NULL do šablon

Pokud použijte jednu z existujících šablon datum a předat hodnotu null datum, zobrazí se vám Chyba za běhu. Chcete-li datum šablony robustnější, změníte, jejich zpracování hodnot null. Pro podporu data s možnou hodnotou Null, změňte kód v *Views\Shared\DisplayTemplates\DateTime.cshtml* takto:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Kód vrátí prázdný řetězec, když je model **null**.

Změňte kód v *Views\Shared\EditorTemplates\Date.cshtml* souboru následující:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Když tento kód se spustí, pokud model nemá hodnotu null, modelu `DateTime` hodnota se používá. Pokud model má hodnotu null, použije se místo toho aktuální datum.

### <a name="wrapup"></a>Wrapup

V tomto kurzu zahrnují základy ASP.NET pomocnými objekty se a ukazuje, jak pomocí kalendářem jQuery UI datepicker v aplikaci ASP.NET MVC. Další informace vyzkoušejte tyto zdroje:

- Informace o lokalizaci, najdete v blogovém společnosti Rajeesh [JQueryUI prvkem Datepicker v architektuře ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Informace o uživatelské rozhraní jQuery najdete v tématu [uživatelské rozhraní jQuery](http://docs.jquery.com/UI).
- Informace o tom, jak lokalizovat ovládací prvek datepicker najdete v tématu [uživatelského rozhraní/ovládací prvek Datepicker/lokalizace](http://docs.jquery.com/UI/Datepicker/Localization).
- Další informace o šablonách technologie ASP.NET MVC, najdete v řadě blogu Brada wilsona v příspěvku na [šablony ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). I když řady napsaný pro ASP.NET MVC 2, materiál je relevantní pro aktuální verzi technologie ASP.NET MVC.

> [!div class="step-by-step"]
> [Předchozí](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
