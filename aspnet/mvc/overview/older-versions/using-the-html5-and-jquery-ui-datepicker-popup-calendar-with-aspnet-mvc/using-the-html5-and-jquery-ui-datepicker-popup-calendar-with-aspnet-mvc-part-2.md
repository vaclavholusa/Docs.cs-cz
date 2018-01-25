---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: "Použití jazyka HTML5 a kalendáře jQuery UI DatePicker s architekturou ASP.NET MVC – část 2 | Microsoft Docs"
author: Rick-Anderson
description: "V tomto kurzu naučit základní informace o tom, jak pracovat s editor šablon, zobrazení šablon a kalendářem jQuery UI ovládací prvek datepicker v MV ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 69fbaa7761c97895ffee770f6feb9ce6b745d186
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Použití jazyka HTML5 a kalendáře jQuery UI DatePicker s architekturou ASP.NET MVC – část 2
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu naučit základní informace o tom, jak pracovat s editor šablon, zobrazení šablon a kalendářem jQuery UI ovládací prvek datepicker v aplikaci ASP.NET MVC Web.


## <a name="adding-an-automatic-datetime-template"></a>Přidání šablonu automatické data a času

V první části tohoto kurzu jste viděli, jak můžete přidat atributů do modelu k explicitnímu zadání formátování a jak explicitně zadat šablonu, která je použita k vykreslení modelu. Například [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) formátování pro Určuje atribut v následujícím kódu výslovně `ReleaseDate` vlastnost.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

V následujícím příkladu [datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atribut, pomocí `Date` výčtu, určuje, že šablona datum měli používat k vykreslení modelu. Pokud neexistuje žádná šablona data ve vašem projektu, se používá integrované datum šablony.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Ale ASP. MVC můžete provádět typ odpovídající pomocí konvencí over konfigurace, tak, že vyhledá šablonu, která odpovídá názvu typu. To vám umožní vytvořit šablonu, která automaticky formáty dat bez použití všechny atributy nebo kód vůbec. Pro tuto část kurzu vytvoříte šablonu, která je automaticky použita pro vlastnosti modelu typu [data a času](https://msdn.microsoft.com/library/system.datetime.aspx). Nebudete muset použít atribut nebo další konfiguraci k určení, že šablona by měla sloužit k vykreslení všechny vlastnosti modelu typu [data a času](https://msdn.microsoft.com/library/system.datetime.aspx).

Taky poznáte způsob, jak přizpůsobit zobrazení jednotlivých vlastností či i jednotlivých polí.

Pokud chcete začít, umožňuje odeberte stávající informace o formátování a zobrazit úplnou kalendářních dat v aplikaci.

Otevřete *Movie.cs* souborů a komentář `DataType` atributu u `ReleaseDate` vlastnost:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Stisknutím kombinace kláves CTRL + F5 a spusťte aplikaci.

Všimněte si, že `ReleaseDate` vlastnost teď zobrazuje datum a čas, protože to je výchozí hodnota je-li žádné informace o formátování.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Přidání pro testování nových šablon stylů CSS

Než vytvoříte šablonu pro formátování kalendářních dat, přidáte několik pravidel stylu CSS, které můžete použít pro nové šablony. To vám pomůže zkontrolujte, zda na vykreslené stránce používá nové šablony.

Otevřete *Content\Site.cs*s souboru a přidejte následující pravidla šablon stylů CSS do spodní části souboru:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Přidání šablony zobrazení data a času

Nyní můžete vytvořit novou šablonu. V *Views\Movies* složky, vytvoření *DisplayTemplates* složky.

V *Views\Shared* složky, vytvoření *DisplayTemplates* složky a *EditorTemplates* složky.

Zobrazení šablony v *Views\Shared\DisplayTemplates* složky budou používat všechny řadiče. Zobrazení šablony v *Views\Movie\DisplayTemplates* složka se použije pouze pomocí `Movie` řadiče. (Pokud šablona se stejným názvem se zobrazí v obě složky šablony *Views\Movie\DisplayTemplates* složky – tedy konkrétnější šablony – má přednost před pro zobrazení vrácené `Movie` řadiče.)

V **Průzkumníku řešení**, rozbalte *zobrazení* složky, rozbalte *sdílené* složku a potom klikněte pravým tlačítkem *Views\Shared\DisplayTemplates* složky.

Klikněte na tlačítko **přidat** a pak klikněte na **zobrazení**. **Přidat zobrazení** se zobrazí dialogové okno.

V **název zobrazení** zadejte `DateTime`. (Chcete-li shodovat s názvem typu musíte použít tento název.)

Vyberte **vytvořit jako částečné zobrazení** zaškrtávací políčko. Ujistěte se, že **použít rozložení stránky předlohy** a **vytvořit zobrazení silného typu** nejsou zaškrtnutá políčka.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Klikněte na tlačítko **přidat**. A *DateTime.cshtml* šablona je vytvořena v *Views\Shared\DisplayTemplates*.

Na následujícím obrázku *zobrazení* složky v **Průzkumníku řešení** po `DateTime` vytváření zobrazení a editor šablon.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Otevřete *Views\Shared\DisplayTemplates\DateTime.cshtml* souboru a přidejte následující kód, který používá [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx) metoda k formátování vlastnost jako datum bez čas. ( `{0:d}` Formát Určuje formát krátkého data.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Opakujte tento krok k vytvoření `DateTime` šablonu *Views\Movie\DisplayTemplates* složky. Použít následující kód v *Views\Movie\DisplayTemplates\DateTime.cshtml* souboru.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

`loud-1` Třída CSS, která způsobí, že data se zobrazení red tučně. Jste přidali `loud-1` třídu CSS stejně jako dočasné opatření, můžete snadno vidět, když se používá tato konkrétní šablonu.

Jaké kroky dokončíte je vytvořena a přizpůsobit šablony, které ASP.NET použije k zobrazení dat. Další obecné šablony (v *Views\Shared\DisplayTemplates* složky) zobrazí jednoduché krátkého data. Šablony, která je speciálně pro `Movie` řadiče (v *Views\Movies\DisplayTemplates* složky) zobrazí s krátkým datem, který je také naformátovaný jako red tučně.

Stisknutím kombinace kláves CTRL + F5 a spusťte aplikaci. V prohlížeči vykreslí zobrazení indexu pro aplikaci.

`ReleaseDate` Vlastnost nyní zobrazuje datum tučným písmem red bez čas. Díky tomu můžete potvrdit, že `DateTime` šablonované Pomocník *Views\Movies\DisplayTemplates* přes je vybrána složka `DateTime` šablonované pomocné rutiny ve sdílené složce (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Nyní přejmenovat *Views\Movies\DisplayTemplates\DateTime.cshtml* do souboru *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Stisknutím kombinace kláves CTRL + F5 a spusťte aplikaci.

Tentokrát `ReleaseDate` vlastnost zobrazí datum bez čas a bez red tučné písmo. To ukazuje, že zadejte šablonu, která obsahuje název dat (v tomto případě `DateTime`) se automaticky používá k zobrazení všechny vlastnosti modelu daného typu. Po je přejmenován *DateTime.cshtml* do souboru *LoudDateTime.cshtml*, technologie ASP.NET již nalézt šablonu *Views\Movies\DisplayTemplates* složky, takže ho použít *DateTime.cshtml* šablony z * Views\Movies\Shared\* složky.

(Odpovídající šablony je malá a velká písmena, takže jste mohli vytvořit název souboru šablony s žádné velká a malá písmena. For example *DATETIME.chstml, datetime.cshtml*, a *DaTeTiMe.cshtml* odpovídá všechny `DateTime` typ.)

Chcete-li zkontrolovat: v tomto okamžiku `ReleaseDate` pole se zobrazuje pomocí *Views\Movies\DisplayTemplates\DateTime.cshtml* šablony, která zobrazí data s využitím formátu krátkého data, ale jinak přidá žádné speciální formátu.

### <a name="using-uihint-to-specify-a-display-template"></a>Pomocí UIHint zadat šablonu zobrazení

Pokud webová aplikace obsahuje hodně `DateTime` pole a ve výchozím nastavení, které chcete zobrazit všechny nebo většinu z nich ve formátu pouze data, *DateTime.cshtml* šablony je dobré přístup. Ale co když máte pár data, ve které chcete zobrazit úplný datum a čas? Žádný problém. Můžete vytvořit další šablonu a použít [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atribut k určení formátování pro úplné datum a čas. Pak můžete selektivně použít dané šablony. Můžete použít [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atribut na úrovni modelu, nebo můžete zadat šablonu uvnitř zobrazení. V této části najdete postup použití `UIHint` atribut selektivně Změna formátování pro některé instance pole data a času.

Otevřete *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* souborů a existujícího kódu nahraďte následujícím kódem:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

To způsobí, že úplné datum a čas, který se má zobrazit a přidá třídu CSS, která lze text zelená a velké.

Otevřete *Movie.cs* souboru a přidejte [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atribut `ReleaseDate` vlastnost, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

To technologii ASP.NET MVC řekne, když se zobrazí `ReleaseDate` vlastnost (konkrétně a ne všem `DateTime` objektu), měla by používat *LoudDateTime.cshtml* šablony.

Stisknutím kombinace kláves CTRL + F5 a spusťte aplikaci.

Všimněte si, že `ReleaseDate` vlastnost teď zobrazuje datum a čas zelená velkými písmeny.

Vraťte se do `UIHint` atribut *Movie.cs* souborů a nastavte komentář u ho proto *LoudDateTime.cshtml* šablony se nepoužijí. Spusťte aplikaci znovu. Datum vydání se nezobrazí, velký a zelená. To ověřuje, že *Views\Shared\DisplayTemplates\DateTime.cshtml* šablona se používá v zobrazení indexu a podrobnosti.

Jak už bylo zmíněno dříve, můžete taky použít šablonu v zobrazení, který vám umožňuje použít šablonu na jednotlivé instance některá data. Otevřete *Views\Movies\Details.cshtml* zobrazení. Přidat `"LoudDateTime"` jako druhý parametr [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) zavolat `ReleaseDate` pole. Dokončený kód v vypadá takto:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

To určuje, že `LoudDateTime` šablony se má použít k zobrazení vlastností modelu, bez ohledu na to, jaké atributy jsou použité pro model.

Stisknutím kombinace kláves CTRL + F5 a spusťte aplikaci.

Zkontrolujte, zda indexovou stránku film používá *Views\Shared\DisplayTemplates\DateTime.cshtml* šablony (červený tučně) a *Movie\Details* stránky je pomocí *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* šablony (velké a zelená).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

V další části vytvoříte šablonu pro komplexního typu.

>[!div class="step-by-step"]
[Předchozí](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
[další](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
