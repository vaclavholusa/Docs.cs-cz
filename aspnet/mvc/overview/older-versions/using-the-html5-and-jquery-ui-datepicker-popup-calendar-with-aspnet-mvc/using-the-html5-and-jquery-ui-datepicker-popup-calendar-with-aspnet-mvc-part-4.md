---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: "Použití jazyka HTML5 a kalendáře jQuery UI DatePicker s architekturou ASP.NET MVC – část 4 | Microsoft Docs"
author: Rick-Anderson
description: "V tomto kurzu naučit základní informace o tom, jak pracovat s editor šablon, zobrazení šablon a kalendářem jQuery UI ovládací prvek datepicker v MV ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 2b76d2e449d491fd8d808343065b22ba267f1152
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Použití jazyka HTML5 a kalendáře jQuery UI DatePicker s architekturou ASP.NET MVC – část 4
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu naučit základní informace o tom, jak pracovat s editor šablon, zobrazení šablon a kalendářem jQuery UI ovládací prvek datepicker v aplikaci ASP.NET MVC Web.


### <a name="adding-a-template-for-editing-dates"></a>Přidání šablony pro úpravy dat

V této části vytvoříte šablonu pro úpravu dat, která bude použita, pokud rozhraní ASP.NET MVC zobrazí uživatelské rozhraní pro úpravy vlastnosti modelu, které jsou označené **datum** výčet [datový typ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) atribut. Šablona vykreslí pouze datum; čas se nezobrazí. V šabloně budete používat [jQuery UI ovládací prvek Datepicker](http://jqueryui.com/demos/datepicker/) kalendářem poskytnout způsob, jak upravit data.

Chcete-li začít, otevřete *Movie.cs* souboru a přidejte [datový typ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atribut s **datum** výčet `ReleaseDate` vlastnost, jak je znázorněno v následujícím kódu:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Tento kód způsobí, že `ReleaseDate` pole, které chcete zobrazit bez čas v obou šablony zobrazení a úpravě šablony. Pokud vaše aplikace obsahuje *date.cshtml* šablony v *Views\Shared\EditorTemplates* složky nebo *Views\Movies\EditorTemplates* složku, že šablona slouží k vykreslení žádné `DateTime` vlastnost při úpravách. V opačném případě předdefinovaného systémového ukázka ASP.NET se zobrazí vlastnosti jako datum.

Stisknutím kombinace kláves CTRL + F5 a spusťte aplikaci. Vyberte odkaz pro úpravy k ověření, že vstupní pole pro datum vydání se zobrazuje pouze datum.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

V **Průzkumníku řešení**, rozbalte *zobrazení* složky, rozbalte *sdílené* složku a potom klikněte pravým tlačítkem *Views\Shared\EditorTemplates* složky.

Klikněte na tlačítko **přidat**a potom klikněte na **zobrazení**. **Přidat zobrazení** se zobrazí dialogové okno.

V **název zobrazení** zadejte &quot;datum&quot;.

Vyberte **vytvořit jako částečné zobrazení** zaškrtávací políčko. Ujistěte se, že **použít rozložení stránky předlohy** a **vytvořit zobrazení silného typu** nejsou zaškrtnutá políčka.

Klikněte na tlačítko **přidat**. *Views\Shared\EditorTemplates\Date.cshtml* je šablona vytvořena.

Přidejte následující kód, který *Views\Shared\EditorTemplates\Date.cshtml* šablony.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

První řádek deklaruje modelu, který má být `DateTime` typu. I když nemusíte deklarace typu modelu v upravit a zobrazit šablony, je osvědčeným postupem, abyste měli kompilaci kontrola modelu, je předaná do zobrazení. (Další výhodou je, že dostanete pak IntelliSense pro model v zobrazení v sadě Visual Studio.) Pokud není deklarovaný typ modelu, ASP.NET MVC považuje [dynamické](https://msdn.microsoft.com/en-us/library/dd264741.aspx) zadejte a neexistuje žádný kompilaci kontrola typu. Pokud je deklarovat modelu, který má být `DateTime` typu, stane silného typu.

Druhý řádek je právě literálu jazyka HTML, který zobrazí &quot;pomocí šablony datum&quot; před pole pro datum. Použijete tento řádek dočasně ověřit, jestli se používá tato šablona datum.

Další řádek je [Html.TextBox](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.inputextensions.textbox.aspx) pomocné rutiny, který vykreslí `input` pole, které je v textovém poli. Třetí parametr pro pomocníka, který používá nastavení třídy pro textové pole k anonymní typ `datefield` a typ, který má `date`. (Protože `class` je vyhrazená v jazyce C#, budete muset použít `@` znak pro přepnutí `class` atribut analyzátoru jazyka C#.)

`date` Typ je typ vstupu HTML5, umožňující podporou jazyka HTML5 prohlížečů k vykreslení ovládacího prvku Kalendář HTML5. Později přidáte určitého kódu JavaScript spojit ovládací prvek datepicker jQuery k `Html.TextBox` pomocí elementu `datefield` třídy.

Stisknutím kombinace kláves CTRL + F5 a spusťte aplikaci. Ověřte, zda `ReleaseDate` vlastnosti v zobrazení úpravy je pomocí šablony upravit, protože zobrazí šablony &quot;pomocí šablony datum&quot; těsně před `ReleaseDate` text vstupní pole, jak je vidět na tomto obrázku:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

V prohlížeči zobrazte zdroj stránky. (Například pravým tlačítkem myši a vyberte **zobrazit zdroj**.) Následující příklad ukazuje některé z kódu pro stránku, ilustrující `class` a `type` atributy ve vykresleném HTML.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Vraťte se do *Views\Shared\EditorTemplates\Date.cshtml* šablony a odebrat &quot;pomocí šablony datum&quot; značek. Dokončené šablony teď vypadá takto:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Přidání kalendářem jQuery UI ovládací prvek Datepicker pomocí nástroje NuGet

V této části přidáte [ovládací prvek datepicker uživatelského rozhraní jQuery](http://jqueryui.com/demos/datepicker/) kalendářem datum úpravy šablony. [JQuery UI](http://jqueryui.com/) knihovna poskytuje podporu pro animace, pokročilé důsledky a přizpůsobit pomůcky. Je založen na knihovna jQuery JavaScript. Kalendářem ovládací prvek datepicker umožňuje snadno a přirozené zadejte kalendářních dat pomocí kalendáře místo zadávání řetězec. Kalendářem také omezuje uživatelům právní data – položka běžného textu pro datum by umožňují zadejte něco podobného jako `2/33/1999` (únor 33rd, 1999), ale [ovládací prvek datepicker uživatelského rozhraní jQuery](http://jqueryui.com/demos/datepicker/) kalendářem neumožní, který.

Nejprve budete muset nainstalovat knihovny uživatelského rozhraní jQuery. Kvůli tomu budete používat NuGet, který je Správce balíčků, který je součástí verze SP1 Visual Studio 2010 a Visual Web Developer.

V aplikaci Visual Web Developer z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven** a pak vyberte **spravovat balíčky NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Poznámka: Pokud **nástroje** nabídka se nezobrazí **Správce balíčků knihoven** příkaz, musíte nainstalovat NuGet podle pokynů [instalace NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) stránky Web NuGet.   
  
Pokud používáte Visual Studio místo Visual Web Developer, z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven** a pak vyberte **přidat odkaz na balíček knihovny**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

V **MVCMovie - spravovat balíčky NuGet** dialogové okno, klikněte **Online** kartě na levé straně a pak zadejte &quot;jQuery.UI&quot; do vyhledávacího pole. Vyberte j **dotazu uživatelského rozhraní pomůcek: ovládací prvek Datepicker**, vyberte **nainstalovat** tlačítko.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet přidá tyto ladicí verze a minifikovaný verze jQuery základní uživatelské rozhraní a výběr data uživatelského rozhraní jQuery do projektu:

- *jQuery.UI.Core.js*
- *jQuery.UI.Core.min.js*
- *jQuery.UI.DatePicker.js*
- *jQuery.UI.DatePicker.min.js*

Poznámka: Verze ladění (soubory bez *. min.js* rozšíření) jsou užitečné pro ladění, ale v produkční lokality, bude zahrnovat pouze minifikovaný verze.

Chcete-li ve skutečnosti pomocí nástroje pro výběr data jQuery, vytvořte jQuery skript, který bude spojit widgetu kalendáře do šablony upravit. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *skripty* složky a vyberte **přidat**, pak **nová položka**a potom **JScript Soubor**. Název souboru *DatePickerReady.js*.

Přidejte následující kód, který *DatePickerReady.js* souboru:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Pokud si nejste obeznámeni s jQuery, následuje stručné vysvětlení to udělá: první řádek je &quot;jQuery připravené&quot; funkci, která je volána, když máte načíst všechny DOM elementy na stránce. Druhý řádek vybere všechny elementy DOM, které mají název třídy `datefield`, poté vyvolá `datepicker` funkci pro každý z nich. (Mějte na paměti, že jste přidali `datefield` třídy k *Views\Shared\EditorTemplates\Date.cshtml* šablony dříve v tomto kurzu.)

Dále otevřete *Views\Shared\\_Layout.cshtml* souboru. Je nutné přidat odkazy na následující soubory, které jsou všechny požadované, takže můžete použít nástroje pro výběr data:

- *Content/Themes/Base/jQuery.UI.Core.CSS*
- *Content/Themes/Base/jQuery.UI.DatePicker.CSS*
- *Content/Themes/Base/jQuery.UI.theme.CSS*
- *jQuery.UI.Core.min.js*
- *jQuery.UI.DatePicker.min.js*
- *DatePickerReady.js*

Následující příklad ukazuje kód skutečný, měli byste přidat v dolní části `head` element v *Views\Shared\\_Layout.cshtml* souboru.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Kompletní `head` části je znázorněno zde:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

[Adresa URL obsahu pomocná](https://msdn.microsoft.com/en-us/library/system.web.mvc.urlhelper.content.aspx) metoda převede cesta prostředku na absolutní cestu. Je nutné použít `@URL.Content` správně odkazovat na tyto prostředky když aplikace běží ve službě IIS.

Stisknutím kombinace kláves CTRL + F5 a spusťte aplikaci. Vyberte odkaz pro úpravy a pak umístěte kurzor do **ReleaseDate** pole. Zobrazí se kalendářem jQuery uživatelského rozhraní.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Stejně jako většinu jQuery ovládací prvky umožňuje ovládací prvek datepicker hojně přizpůsobit. Informace najdete v tématu [Visual přizpůsobení: navrhování motiv uživatelského rozhraní jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) na [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/) lokality.

### <a name="supporting-the-html5-date-input-control"></a>Podpora jazyka HTML5 datum vstupního ovládacího prvku

Jako další prohlížeče podporují HTML5, budete chtít použít nativní HTML5 vstup, jako `date` elementu input a kalendáře jQuery UI nechcete použít. Můžete přidat logiku do vaší aplikace k automatickému využití ovládacích prvků HTML5, pokud je prohlížeč podporuje. Chcete-li to provést, nahraďte obsah *DatePickerReady.js* soubor s následující:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

První řádek tento skript používá Modernizr k ověřte, zda je vstupní datum HTML5 podporovat. Pokud se nepodporuje, výběr data uživatelského rozhraní jQuery je připojili místo. ([Modernizr](http://www.modernizr.com/docs/) je knihovna JavaScript open source, který zjišťuje dostupnost nativní implementace HTML5 a CSS3. Modernizr je součástí nové projekty ASP.NET MVC, které vytvoříte.)

Po provedení této změny, jej můžete otestovat pomocí prohlížeče, který podporuje HTML5, jako je například Opera 11. Spusťte aplikaci pomocí prohlížeče kompatibilní s HTML5 a upravte film položku. Řízení data HTML5 se používá namísto kalendářem jQuery uživatelského rozhraní:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Vzhledem k tomu, že nové verze prohlížeče jsou postupně implementace HTML5, je dobrý způsob teď přidejte kód na váš web, která bude vyhovovat širokou škálu podporu jazyka HTML5. Například robustnější *DatePickerReady.js* skriptu je zobrazena níže umožňující podporu prohlížeče lokality pouze částečně podporující řízení data HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Tento skript vybere HTML5 `input` elementy typu `date` která plně nepodporují řízení data HTML5. Pro tyto elementy zachytí až kalendářem jQuery uživatelského rozhraní a poté změní `type` atribut z `date` k `text`. Změnou `type` atribut z `date` k `text`, nemá žádný částečné podporu data HTML5. Ještě více robustní *DatePickerReady.js* skriptu najdete na [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Přidávání do šablony s možnou hodnotou Null kalendářních dat

Pokud použijte jednu z existujících šablon datum a předat hodnotu null. datum, získáte Chyba spuštění. Chcete-li šablony datum robustnější, změníte jejich zpracování hodnot null. Pro podporu s možnou hodnotou Null kalendářních dat, změňte kód v *Views\Shared\DisplayTemplates\DateTime.cshtml* pro následující:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Kód vrátí prázdný řetězec, když je model **null**.

Změňte kód v *Views\Shared\EditorTemplates\Date.cshtml* soubor pro následující:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Pokud tento kód používá, pokud model nemá hodnotu null, modelu `DateTime` hodnota se používá. Pokud model má hodnotu null, použije se místo toho aktuální datum.

### <a name="wrapup"></a>Wrapup

V tomto kurzu má popsaná základy objekty se šablonami za ASP.NET a ukazuje, jak používat kalendářem jQuery UI ovládací prvek datepicker v aplikaci ASP.NET MVC. Další informace zkuste tyto prostředky:

- Informace o lokalizaci, najdete v části Rajeesh na blogu [ovládací prvek JQueryUI Datepicker v architektuře ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Informace o jQuery UI najdete v tématu [jQuery UI](http://docs.jquery.com/UI).
- Informace o tom, jak lokalizace ovládacího prvku ovládací prvek datepicker najdete v tématu [uživatelského rozhraní nebo ovládací prvek Datepicker/lokalizace](http://docs.jquery.com/UI/Datepicker/Localization).
- Další informace o šablonách technologie ASP.NET MVC najdete v části řady blogu Brada Wilsona v [ASP.NET MVC 2 šablony](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). I když řady byla zapsána pro ASP.NET MVC 2, materiál stále platí pro aktuální verzi ASP.NET MVC.

>[!div class="step-by-step"]
[Předchozí](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
