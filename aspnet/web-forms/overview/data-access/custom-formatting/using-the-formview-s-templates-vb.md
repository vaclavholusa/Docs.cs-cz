---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: Pomocí šablony FormView (VB) | Microsoft Docs
author: rick-anderson
description: Na rozdíl od DetailsView není FormView skládá z pole. Místo toho FormView je vykreslen pomocí šablony. V tomto kurzu podíváme, pomocí F....
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: 16293960f5d8758c93646844bd159547f5e0f38c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875146"
---
<a name="using-the-formviews-templates-vb"></a>Pomocí šablony FormView (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe) nebo [stáhnout PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> Na rozdíl od DetailsView není FormView skládá z pole. Místo toho FormView je vykreslen pomocí šablony. V tomto kurzu podíváme pomocí ovládacího prvku FormView na menší pevné zobrazení dat k dispozici.


## <a name="introduction"></a>Úvod

V posledních dvou kurzech jsme viděli, jak přizpůsobit pomocí TemplateFields výstupy GridView a DetailsView prvky. TemplateFields povolit pro obsah pro konkrétní pole, které chcete být vysoce přizpůsobené, ale v části end GridView i DetailsView mít vzhled místo boxy, mřížky. Pro mnoho scénářů je ideální mřížky rozložení, ale v některých případech je potřeba víc plynulá práce méně rovné a zobrazení. Při zobrazení jeden záznam, je možné pomocí ovládacího prvku FormView plynulá práce rozložení.

Na rozdíl od DetailsView není FormView skládá z pole. Nelze přidat BoundField nebo TemplateField k FormView. Místo toho FormView je vykreslen pomocí šablony. FormView si můžete představit jako DetailsView ovládací prvek, který obsahuje jeden TemplateField. FormView podporuje následujících šablon:

- `ItemTemplate` použít k vykreslení konkrétní záznam zobrazí ve třídě FormView
- `HeaderTemplate` slouží k zadání řádku volitelné záhlaví
- `FooterTemplate` slouží k zadání řádek volitelné zápatí
- `EmptyDataTemplate` Když FormView `DataSource` nemá žádné záznamy `EmptyDataTemplate` slouží místě `ItemTemplate` pro vykreslení značek ovládacího prvku
- `PagerTemplate` slouží k přizpůsobení rozhraní stránkování pro FormViews, které mají povoleno stránkování
- `EditItemTemplate` / `InsertItemTemplate` použít pro přizpůsobení úpravy rozhraní nebo vkládání rozhraní FormViews, která podporuje tyto funkce

V tomto kurzu podíváme pomocí ovládacího prvku FormView k dispozici méně pevné zobrazení produktů. Místo pole pro název, kategorie, dodavatele a tak dále, FormView na `ItemTemplate` zobrazí tyto hodnoty pomocí kombinace header element a `<table>` (viz obrázek 1).


[![FormView dělí rozložení mřížky zobrazená v ovládacím prvku DetailsView.](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**Obrázek 1**: dělí FormView mimo Grid-Like rozložení vidět v ovládacím prvku DetailsView ([Kliknutím zobrazit obrázek v plné velikosti](using-the-formview-s-templates-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Krok 1: Vytvoření vazby dat FormView

Otevřete `FormView.aspx` stránky a přetáhněte ji FormView z panelu nástrojů na návrháře. Při přidávání nejdřív FormView se zobrazí jako šedé pole, instruující nám, `ItemTemplate` je potřeba.


[![FormView nelze vykreslit v návrháři, dokud nebude poskytnuta ItemTemplate](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**Obrázek 2**: The FormView nelze vykreslit v Návrháři dokud `ItemTemplate` je k dispozici ([Kliknutím zobrazit obrázek v plné velikosti](using-the-formview-s-templates-vb/_static/image6.png))


`ItemTemplate` Je možné vytvořit ručně (pomocí deklarativní syntaxe) nebo může být automaticky vytvořené pomocí FormView vytvoření vazby na ovládací prvek zdroje dat pomocí návrháře. Toto automaticky vytvořené `ItemTemplate` obsahuje HTML, který zobrazí název každého pole a popisek ovládacího prvku jehož `Text` vlastnost je vázána na hodnotu pole. Tento přístup také automaticky vytvoří `InsertItemTemplate` a `EditItemTemplate`, které se naplní vstupní ovládací prvky pro každé pole dat vrácených dat zdrojového kódu.

Pokud chcete automaticky vytvořit šablonu, z inteligentních značek FormView přidání nové ovládacího prvku ObjectDataSource vyvolávající `ProductsBLL` třídy `GetProducts()` metoda. Tím se vytvoří FormView s `ItemTemplate`, `InsertItemTemplate`, a `EditItemTemplate`. Odebrat ze zobrazení zdroje `InsertItemTemplate` a `EditItemTemplate` vzhledem k tomu, že ještě nejsme zájem o vytvoření FormView, který podporuje úpravu nebo ještě vkládání. Další, zrušte na kód v rámci `ItemTemplate` tak, aby jsme čistou projektem pracovat na.

Pokud vám vytvořit raději `ItemTemplate` ručně, můžete přidávat a konfigurovat ObjectDataSource přetažením z panelu nástrojů na návrháře. Však není nastavení zdroje dat FormView z návrháře. Místo toho přejděte do zobrazení zdroje a ručně nastavit FormView `DataSourceID` vlastnost, která má `ID` hodnotu ObjectDataSource. V dalším kroku ručně přidat `ItemTemplate`.

Bez ohledu na to, jakou metodu jste se rozhodli využít, v tomto okamžiku vaší FormView deklarativní vzhled:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

Za chvíli zaškrtnutím políčka Povolit stránkování v inteligentních značek FormView; Tím se přidá `AllowPaging="True"` atribut FormView deklarativní syntaxe. Také nastavena `EnableViewState` vlastnost na hodnotu False.

## <a name="step-2-defining-theitemtemplates-markup"></a>Krok 2: Definování`ItemTemplate`na kód

S FormView vázán do ovládacího prvku ObjectDataSource a nakonfigurované pro podporu stránkování je vše připraveno k určení obsahu pro `ItemTemplate`. V tomto kurzu budeme mít název produktu zobrazený v `<h3>` záhlaví. Následující, použijeme HTML `<table>` zobrazíte ostatní vlastnosti produktu v čtyři sloupce tabulky, kde sloupce první a třetí seznam názvů vlastností druhé a čtvrté seznam a jejich hodnoty.

Tento kód lze zadat v prostřednictvím rozhraní úpravy FormView šablona v Návrháři nebo zadat ručně využijte deklarativní syntaxi. Při práci se šablonami I obvykle často rychlejší pracovat přímo s deklarativní syntaxi, ale můžete použít libovolnou techniku jste nejvíce vyhovuje.

Následující kód ukazuje FormView deklarativní po `ItemTemplate`na dokončení strukturu:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

Všimněte si, že syntaxe datové vazby - `<%# Eval("ProductName") %>`pro příklad můžete vložit přímo do výstupu šablony. To znamená, nemusí být přiřazen do ovládacího prvku popisek `Text` vlastnost. Například máme `ProductName` hodnoty zobrazené v `<h3>` element pomocí `<h3><%# Eval("ProductName") %></h3>`, který pro produkt Chai se vykreslují jako `<h3>Chai</h3>`.

`ProductPropertyLabel` a `ProductPropertyValue` tříd CSS slouží k určení styl produktu názvy a hodnoty `<table>`. Tyto třídy CSS jsou definovány v `Styles.css` a způsobit, že názvy vlastností tučné a doprava a přidejte právo, odsazení hodnot vlastností.

Vzhledem k tomu, aby bylo možné zobrazit nejsou k dispozici s FormView, žádné CheckBoxFields `Discontinued` hodnotu jako zaškrtávací políčko jsme musíte přidat vlastní ovládací prvek zaškrtávací políčko. `Enabled` Je nastavena na hodnotu False, takže je jen pro čtení a potom zaškrtněte políčko `Checked` vlastnost je vázána na hodnotu `Discontinued` datové pole.

Pomocí `ItemTemplate` dokončení se zobrazí informace o produktu mnohem víc plynulá práce způsobem. Porovnejte DetailsView výstup z posledního kurzu (obrázek 3) s výstupem vygenerované FormView v tomto kurzu (obrázek 4).


[![Pevné DetailsView výstup](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**Obrázek 3**: pevné DetailsView výstup ([Kliknutím zobrazit obrázek v plné velikosti](using-the-formview-s-templates-vb/_static/image9.png))


[![Plynulá práce FormView výstup](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**Obrázek 4**: výstup FormView kapaliny ([Kliknutím zobrazit obrázek v plné velikosti](using-the-formview-s-templates-vb/_static/image12.png))


## <a name="summary"></a>Souhrn

Při jejich výstup přizpůsobit pomocí TemplateFields mohou mít ovládací prvky GridView a DetailsView, obě stále k dispozici svá data ve formátu mřížky, boxy. Pro případy, kdy jeden záznam musí zobrazit pomocí méně pevné rozložení, FormView je ideálním řešením. Jako DetailsView, FormView vykreslí jeden záznam z jeho `DataSource`, ale na rozdíl od DetailsView se skládá právě z šablony a nepodporuje pole.

Jak jsme viděli v tomto kurzu, FormView umožňuje flexibilnější rozložení při zobrazení jeden záznam. Kurzy v budoucnosti podíváme DataList a opakovače ovládací prvky, které poskytují stejnou úroveň flexibilitu jako FormsView, ale mohou zobrazit více záznamů (např. rutina GridView).

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Pro tento kurz byl E.R. vést kontrolora Gilmore. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](using-templatefields-in-the-detailsview-control-vb.md)
> [další](displaying-summary-information-in-the-gridview-s-footer-vb.md)
