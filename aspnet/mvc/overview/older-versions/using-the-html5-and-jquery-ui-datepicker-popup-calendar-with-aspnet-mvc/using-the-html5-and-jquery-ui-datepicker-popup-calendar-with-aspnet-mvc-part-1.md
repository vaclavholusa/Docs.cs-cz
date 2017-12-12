---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: "Použití jazyka HTML5 a kalendáře jQuery UI DatePicker s architekturou ASP.NET MVC – část 1 | Microsoft Docs"
author: Rick-Anderson
description: "V tomto kurzu naučit základní informace o tom, jak pracovat s editor šablon, zobrazení šablon a kalendářem jQuery UI ovládací prvek datepicker v MV ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: 9320c8a2aadb3b3c5bd6cd90b59d8a72db384c0c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Použití jazyka HTML5 a kalendáře jQuery UI DatePicker s architekturou ASP.NET MVC – část 1
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu naučit základní informace o tom, jak pracovat s editor šablon, zobrazení šablon a kalendářem jQuery UI ovládací prvek datepicker v aplikaci ASP.NET MVC Web.


V tomto kurzu naučit, základní informace o tom, jak pracovat s editor šablon, zobrazení šablon a jQuery [uživatelského rozhraní ovládací prvek datepicker kalendářem](http://plugins.jquery.com/project/datepicker) v aplikaci ASP.NET MVC Web. V tomto kurzu můžete použít Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;), což je bezplatnou verzi sady Microsoft Visual Studio, nebo pokud už máte, můžete použít Visual Studio 2010 SP1.

Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadovaný software pomocí následujících odkazů:

- [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Aktualizace nástrojů rozhraní ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)

Pokud používáte Visual Studio 2010 místo Visual Web Developer, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Tento kurz předpokládá, že jste dokončili [Začínáme s MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) kurzu nebo že jste obeznámeni s vývojem pro rozhraní ASP.NET MVC. V tomto kurzu začíná dokončený projekt z [Začínáme s MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) kurzu.

Tento kurz ukazuje kód v jazyce C#. Ale [starter projektu](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) a dokončený projekt jsou taky dostupné v jazyce Visual Basic.

Projekt sady Visual Studio se zdrojovým kódem C# a Visual Basic je k dispozici v tomto tématu: [Stáhnout](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Co budete sestavení

Bude potřeba přidat šablony (konkrétně upravit a zobrazit šablony) pro jednoduchou aplikaci seznamu film, který byl vytvořen v [Začínáme s MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) kurzu. Taky se přidá [ovládací prvek datepicker uživatelského rozhraní jQuery](http://jqueryui.com/demos/datepicker/) kalendářem zjednodušit proces zadávání data. Následující snímek obrazovky ukazuje změně aplikace s jQuery UI ovládací prvek datepicker kalendářem zobrazí.

![dokončení jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Dovedností, které se dozvíte

Zde je, co se dozvíte:

- Použití atributů z [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) obor názvů řízení formátu dat, jakmile se zobrazí a pokud je v režimu úprav.
- Postup vytvoření šablon (Upravit a zobrazit šablony) k řízení formátování data.
- Postup přidání [ovládací prvek datepicker uživatelského rozhraní jQuery](http://jqueryui.com/demos/datepicker/) jako způsob, jak zadejte datová pole.

### <a name="getting-started"></a>Začínáme

Pokud ještě nemáte aplikace film výpis z projektu starter, stáhněte ho pomocí následujícího odkazu: [Stáhnout](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800). V Průzkumníku Windows, klikněte pravým tlačítkem myši *MvcMovie.zip* soubor a vyberte **vlastnosti**. V **MvcMovie.zip vlastnosti** dialogové okno, vyberte **Odblokovat**. (Odblokování brání upozornění zabezpečení, která nastane, když se pokusíte použít *.zip* souboru, který jste stáhli z webu.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Klikněte pravým tlačítkem myši *MvcMovie.zip* soubor a vyberte **Extrahovat vše** soubor rozbalit. V aplikaci Visual Web Developer nebo Visual Studio 2010, otevřete *MvcMovieCS\_TU.sln* souboru.

V **Průzkumníku řešení**, dvakrát klikněte *Views\Shared\\_Layout.cshtml* ho otevřete. Změna `H1` hlavička ze **filmová aplikace MVC** k **film jQuery**. Stiskněte klávesu CTRL + F5 a spusťte aplikaci a klikněte na tlačítko **Domů** karta, která přebírá, abyste `Index` metoda film řadiče. Můžete vyzkoušet na aplikaci, vyberte **upravit** odkaz a **podrobnosti** odkaz pro jednu z videa. Všimněte si, že se v indexu, upravit, a zobrazení podrobností, datum vydání a ceny jsou vhodně formátovaná:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

Formátování kalendářního data a cenu je výsledkem použití [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut u vlastnosti `Movie` třídy.

Otevřete *Movie.cs* souborů a komentář `DisplayFormat` atributu u `ReleaseDate` a `Price` vlastnosti. Výsledná `Movie` třída vypadá takto:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Stisknutím kláves CTRL + F5 znovu spusťte aplikaci a vyberte **Domů** k zobrazení seznamu video. Tentokrát datum vydání se zobrazuje datum a čas a pole Cena přestane zobrazovat symbolu měny. Změny v `Movie` třída zrušila dobrý formátování, které jste předtím viděli, ale budete to opravíme za chvíli.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Pomocí atributu DataAnnotations DataType zadat datový typ

Nahraďte komentované `DisplayFormat` atribut pro `ReleaseDate` vlastnost s [datový typ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) atribut, pomocí `Date` – výčet. Nahradit `DisplayFormat` atribut pro `Price` vlastnost s [datový typ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) atribut znovu, tento současně pomocí `Currency` – výčet. Toto je dokončený kód v vypadá takto:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Spusťte aplikaci. Nyní datum vydání a vlastnosti ceny jsou správně formátovány (které používá odpovídající formáty data a měna). [Datový typ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) atribut poskytuje typ metadat pro předdefinované ASP.NET MVC šablony tak, aby pole vykreslení ve správném formátu. Pomocí `DataType` atribut je vhodnější než použít `DisplayFormat` atribut, který byl původně v kódu, protože `DataType` atribut díky modelu čisticí a flexibilnější pro účely jako mezinárodní prostředí.

V další části se zobrazí, jak vytvořit vlastní šablony zobrazíte datová pole.

>[!div class="step-by-step"]
[Další](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
