---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Použití jazyka HTML5 a kalendáře jQuery UI Datepicker s architekturou ASP.NET MVC – část 2 | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy práce pomocí editoru šablon, šablony zobrazení a kalendářem jQuery uživatelského rozhraní prvkem datepicker v MV ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9b27ccc6ce26e8266947c531d299ba69bbec4fde
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577194"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Použití jazyka HTML5 a kalendáře jQuery UI Datepicker s architekturou ASP.NET MVC – část 2
====================
Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))

> V tomto kurzu se seznámíte se základy práce pomocí editoru šablon, šablony zobrazení a kalendářem jQuery UI datepicker v aplikaci MVC rozhraní ASP.NET Web.


## <a name="adding-an-automatic-datetime-template"></a>Přidání automatické šablony data a času

V první části tohoto kurzu jste viděli, jak můžete přidat atributy na model s ohledem na formátování a jak můžete explicitně zadat šablonu, která se používá k vygenerování modelu. Například [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributů v následující kód explicitně určuje formátování `ReleaseDate` vlastnost.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

V následujícím příkladu [datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atribut, pomocí `Date` výčet, určuje, že datum šablona by měla sloužit k vygenerování modelu. Pokud neexistuje žádná šablona data ve vašem projektu, se používá předdefinovanou šablonu.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Ale ASP. MVC můžete provádět odpovídající typ pomocí konvence over konfigurace, tím, že hledají šablonu, která odpovídá názvu typu. To vám umožní vytvořit šablonu, která automaticky formáty dat bez použití vůbec žádné atributy nebo kódu. Pro tuto část kurzu vytvoříte šablonu, která se automaticky využije na vlastnosti modelu typu [data a času](https://msdn.microsoft.com/library/system.datetime.aspx). Nebudete muset zadat, že šablona má být použita k vykreslení všech vlastností modelu s typem pomocí atributu nebo jiná konfigurace [data a času](https://msdn.microsoft.com/library/system.datetime.aspx).

Dozvíte se víc i způsob, jak přizpůsobit zobrazení jednotlivých vlastností či i jednotlivá pole.

Pokud chcete začít, můžeme odebrat stávající informace o formátování a zobrazí úplné datum v aplikaci.

Otevřít *Movie.cs* souboru a nastavte komentář `DataType` atribut na `ReleaseDate` vlastnost:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Stisknutím kláves CTRL + F5 spusťte aplikaci.

Všimněte si, že `ReleaseDate` vlastnost nyní zobrazí datum a čas, protože je to výchozí hodnota, když je k dispozici žádné informace o formátování.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Přidání pro testování nových šablon stylů CSS

Než vytvoříte šablonu pro formátování kalendářních dat, přidáte několik pravidel stylu CSS, které můžete použít pro nové šablony. Ty vám pomohou ověřit, že je na vykreslené stránce pomocí nové šablony.

Otevřít *Content\Site.cs*s a přidejte následující pravidla šablon stylů CSS do spodní části souboru:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Přidání šablony zobrazení data a času

Nyní můžete vytvořit novou šablonu. V *Views\Movies* složku, vytvořte *DisplayTemplates* složky.

V *Views\Shared* složku, vytvořte *DisplayTemplates* složky a *EditorTemplates* složky.

Zobrazení šablony v *views\shared\displaytemplates za účelem nalezení* složku budou používat všechny řadiče. Zobrazení šablony v *Views\Movie\DisplayTemplates* složka se použije pouze `Movie` kontroleru. (Pokud šablona se stejným názvem se zobrazí v obě složky šablony *Views\Movie\DisplayTemplates* složky – tedy konkrétnější šablony – má přednost před pro zobrazení vrácených `Movie` kontroler.)

V **Průzkumníka řešení**, rozbalte *zobrazení* složky, rozbalte *Shared* složku a potom klikněte pravým tlačítkem *views\shared\displaytemplates za účelem nalezení* složky.

Klikněte na tlačítko **přidat** a potom klikněte na tlačítko **zobrazení**. **Přidat zobrazení** zobrazí dialogové okno.

V **název zobrazení** zadejte `DateTime`. (Aby bylo možné shodovat s názvem typu musíte použít tento název.)

Vyberte **vytvořit jako částečné zobrazení** zaškrtávací políčko. Ujistěte se, že **použít rozložení stránky předlohy** a **vytvoření zobrazení se silnými typy** nejsou zaškrtnutá políčka.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Klikněte na tlačítko **přidat**. A *DateTime.cshtml* vytvoření šablony v *views\shared\displaytemplates za účelem nalezení*.

Na následujícím obrázku *zobrazení* složky v **Průzkumníka řešení** po `DateTime` vytváření zobrazení a editor šablon.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Otevřít *Views\Shared\DisplayTemplates\DateTime.cshtml* a přidejte následující kód, který používá [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx) metody pro formátování vlastnosti jako datum bez času. ( `{0:d}` Určuje formát krátkého formátu data.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Opakujte tento krok k vytvoření `DateTime` šablony *Views\Movie\DisplayTemplates* složky. Použijte následující kód *Views\Movie\DisplayTemplates\DateTime.cshtml* souboru.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

`loud-1` Třídu šablony stylů CSS způsobí, že datum je zobrazení red tučně. Jste přidali `loud-1` třídu šablony stylů CSS, stejně jako dočasné opatření, takže můžete snadno zobrazit, když se používá tato konkrétní šablonu.

Co jste provedli se vytvoří a přizpůsobit šablony, které technologie ASP.NET použije k zobrazení data. Další obecné šablony (v *views\shared\displaytemplates za účelem nalezení* složky) zobrazí jednoduché krátkého formátu data. Šablonu, která je upravena výslovně pro `Movie` kontroler (v *Views\Movies\DisplayTemplates* složky) zobrazí krátkého formátu data, která je také formátovaný jako červený text tučné.

Stisknutím kláves CTRL + F5 spusťte aplikaci. Prohlížeče vykreslí zobrazení indexu pro aplikaci.

`ReleaseDate` Vlastnosti nyní zobrazí datum v tučné červeně, nezahrnuje dobu. Díky tomu můžete potvrdit, že `DateTime` bez vizuálního vzhledu Pomocník *Views\Movies\DisplayTemplates* přes je vybrána složka `DateTime` bez vizuálního vzhledu pomocné rutiny ve sdílené složce (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Přejmenovat *Views\Movies\DisplayTemplates\DateTime.cshtml* do souboru *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Stisknutím kláves CTRL + F5 spusťte aplikaci.

Tentokrát `ReleaseDate` vlastnost zobrazí datum bez času a tučným písmem červené. To ukazuje, že zadejte šablonu, která má název data (v tomto případě `DateTime`) se automaticky používá k zobrazení tohoto typu všechny vlastnosti modelu. Poté co přejmenovat *DateTime.cshtml* do souboru *LoudDateTime.cshtml*, již nenachází šablony v ASP.NET *Views\Movies\DisplayTemplates* složky, aby se použít *DateTime.cshtml* šablonu z * Views\Movies\Shared\* složky.

(Šablony odpovídající velká a malá písmena, takže jste mohli vytvořit název souboru šablony s jakékoli velká a malá písmena. For example *DATETIME.chstml, datetime.cshtml*, a *DaTeTiMe.cshtml* by všechny odpovídaly `DateTime` typu.)

Ke kontrole: v tomto okamžiku `ReleaseDate` pole se zobrazí pomocí *Views\Movies\DisplayTemplates\DateTime.cshtml* šablonu, která zobrazí data s využitím formátu krátkého data, ale jinak přidá žádný zvláštní formát.

### <a name="using-uihint-to-specify-a-display-template"></a>Použití UIHint k určení zobrazení šablony

Pokud vaše webová aplikace má mnoho `DateTime` pole a ve výchozím nastavení, které chcete zobrazit všechny nebo většina z nich ve formátu pouze *DateTime.cshtml* šablona je dobrý nápad. Ale co kdybyste měli několik kalendářních dat, ve které chcete zobrazit úplné datum a čas? Žádný problém. Můžete vytvořit další šablony a použít [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributy formátování pro úplné datum a čas. Pak můžete selektivně použít tuto šablonu. Můžete použít [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atribut na úrovni modelu nebo je můžete určit, která šablona uvnitř zobrazení. V této části, uvidíte, jak používat `UIHint` atribut selektivně Změna formátování pro některé instance pole data a času.

Otevřít *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* souboru a nahraďte existující kód následujícím kódem:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

To způsobí, že úplné datum a čas, který se má zobrazit a přidává třídu šablony stylů CSS, která umožňuje text zelené a velké.

Otevřít *Movie.cs* a přidejte [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atribut `ReleaseDate` vlastnost, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

To technologii ASP.NET MVC řekne, který poznat `ReleaseDate` vlastnosti (konkrétně a není to jen tak nějaký `DateTime` objektu), měla by používat *LoudDateTime.cshtml* šablony.

Stisknutím kláves CTRL + F5 spusťte aplikaci.

Všimněte si, že `ReleaseDate` vlastnost nyní zobrazí datum a čas zelené velkými písmeny.

Vraťte se na `UIHint` atribut v *Movie.cs* souboru a komentáře ji proto *LoudDateTime.cshtml* šablony se nepoužije. Spusťte aplikaci znovu. Datum vydání se nezobrazí zelená i velké. Ověří, *Views\Shared\DisplayTemplates\DateTime.cshtml* šablona se používá v zobrazeních Index a podrobnosti o.

Jak už bylo zmíněno dříve, můžete také použít šablony v zobrazení, které vám umožní použít šablonu pro jednotlivé instance nějaká data. Otevřít *Views\Movies\Details.cshtml* zobrazení. Přidat `"LoudDateTime"` jako druhý parametr [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) zavolat `ReleaseDate` pole. Dokončený kód vypadá takto:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Určuje, který `LoudDateTime` šablony by se použít k zobrazení vlastnosti modelu, bez ohledu na to, jaké atributy jsou použité pro model.

Stisknutím kláves CTRL + F5 spusťte aplikaci.

Ověřte, zda byla filmu indexovou stránku používá *Views\Shared\DisplayTemplates\DateTime.cshtml* šablony (červená bold) a *Movie\Details* pomocí stránky *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* šablony (velké a zelená).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

V další části vytvoříte šablonu pro komplexního typu.

> [!div class="step-by-step"]
> [Předchozí](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [další](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
