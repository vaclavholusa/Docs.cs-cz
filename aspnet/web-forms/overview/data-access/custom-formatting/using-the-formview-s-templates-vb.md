---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: Pomocí šablony ovládacího prvku FormView (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Na rozdíl od ovládacím prvku DetailsView FormView se skládá z polí. Místo toho FormView je vykreslen pomocí šablony. V tomto kurzu prozkoumáme pomocí F....
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: 66ad79c0c385afed75c1888b81328220ee048d19
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389911"
---
<a name="using-the-formviews-templates-vb"></a>Pomocí šablony ovládacího prvku FormView (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe) nebo [stahovat PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> Na rozdíl od ovládacím prvku DetailsView FormView se skládá z polí. Místo toho FormView je vykreslen pomocí šablony. V tomto kurzu, kterou prozkoumáme pomocí ovládacího prvku FormView prezentovat méně od rigidních zobrazovat data.


## <a name="introduction"></a>Úvod

V posledních dvou kurzech jsme viděli, jak přizpůsobit výstupy GridView a DetailsView řízení použití vlastností TemplateField. Vlastností TemplateField povolit obsah pro určité pole do vysoce přizpůsobené, ale nakonec GridView a DetailsView mít vzhled spíše boxy, mřížky. V mnoha scénářích je ideální rozložení mřížky, ale v některých případech je potřeba méně od rigidních, více zážitků zobrazení. Při zobrazení jeden záznam, je možné, pomocí ovládacího prvku FormView proměnlivé rozložení.

Na rozdíl od ovládacím prvku DetailsView FormView se skládá z polí. Vlastnost BoundField nebo TemplateField nelze přidat k FormView. Místo toho FormView je vykreslen pomocí šablony. Berte jako ovládací prvek DetailsView, který obsahuje jeden TemplateField třídy FormView. FormView podporuje následujících šablon:

- `ItemTemplate` použije k vykreslení konkrétní záznam zobrazí ve třídě FormView
- `HeaderTemplate` používá se k určení řádku volitelné záhlaví
- `FooterTemplate` používá se k určení řádku volitelné zápatí
- `EmptyDataTemplate` Když ovládacího prvku FormView `DataSource` nemá žádné záznamy `EmptyDataTemplate` je použito místo `ItemTemplate` pro vykreslení značek ovládacího prvku
- `PagerTemplate` Umožňuje přizpůsobit rozhraní stránkování pro FormViews, které mají povoleno stránkování
- `EditItemTemplate` / `InsertItemTemplate` používané k úpravám úpravy rozhraní nebo vkládání rozhraní pro FormViews, které podporují tyto funkce

V tomto kurzu, kterou prozkoumáme pomocí ovládacího prvku FormView prezentovat méně od rigidních zobrazení produktů. Místo generování pole pro název, kategorie, dodavatele a tak dále, třídě FormView společnosti `ItemTemplate` zobrazí tyto hodnoty pomocí kombinace header element a `<table>` (viz obrázek 1).


[![Dělí FormView rozložení mřížky v ovládacím prvku DetailsView.](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**Obrázek 1**: FormView rozdělí Grid-Like rozložení viděli v ovládacím prvku DetailsView ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-formview-s-templates-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Krok 1: Vytvoření vazby dat na třídě FormView

Otevřít `FormView.aspx` stránku a přetáhněte z panelu nástrojů do návrháře FormView. Při prvním přidání FormView se zobrazí jako šedé pole, že nám, které `ItemTemplate` je potřeba.


[![FormView nelze vytvořit v návrháři, dokud nebude poskytnuta šablona ItemTemplate](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**Obrázek 2**: třídy FormView nelze vytvořit v Návrháři až `ItemTemplate` je k dispozici ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-formview-s-templates-vb/_static/image6.png))


`ItemTemplate` Lze vytvořit ručně (pomocí deklarativní syntaxe) nebo může být automaticky vytvořený ve třídě FormView vytvoření vazby na ovládací prvek zdroje dat pomocí návrháře. To automaticky vytvořený `ItemTemplate` obsahuje kód HTML, zobrazí název jednotlivých polí a popisek ovládací prvek, jehož `Text` vlastnost je vázána na hodnotu pole. Tento přístup také automaticky vytvoří `InsertItemTemplate` a `EditItemTemplate`, které plní vstupní ovládací prvky pro každé pole dat vrácených ovládací prvek zdroje dat.

Pokud chcete automaticky vytvořit šablonu, z ovládacího prvku FormView inteligentních značek přidat nový ovládací prvek ObjectDataSource, která volá `ProductsBLL` třídy `GetProducts()` metody. Tím se vytvoří FormView pomocí `ItemTemplate`, `InsertItemTemplate`, a `EditItemTemplate`. Odebrat ze zobrazení zdroje `InsertItemTemplate` a `EditItemTemplate` protože nemáme zájem o vytvoření FormView, který podporuje úpravy nebo ještě vkládání. Vymazání značek v rámci další `ItemTemplate` tak, aby máme čisté břidlicová pracovat.

Pokud byste raději `ItemTemplate` ručně, můžete přidat a nakonfigurovat ObjectDataSource jeho přetažením z panelu nástrojů do návrháře. Však nemají nastavený zdroj dat třídy FormView z návrháře. Místo toho přejděte na zobrazení zdroje a ručně nastavte ovládacího prvku FormView `DataSourceID` vlastnost `ID` hodnotu ObjectDataSource. V dalším kroku ručně přidejte `ItemTemplate`.

Bez ohledu na to, jaký přístup jste se rozhodli využít, v tuto chvíli vašeho ovládacího prvku FormView deklarativní by měl vypadat:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

Za chvíli zaškrtnout políčko Povolit stránkování ve třídě FormView inteligentních značek; Tím se přidá `AllowPaging="True"` atribut ovládacího prvku FormView deklarativní syntaxe. Nastavit také, `EnableViewState` vlastnost na hodnotu False.

## <a name="step-2-defining-theitemtemplates-markup"></a>Krok 2: Definování`ItemTemplate`vaší značky

Pomocí třídy FormView svázán s ovládacím prvkem ObjectDataSource a nakonfigurována pro podporu stránkování jsme připraveni k určení obsahu pro `ItemTemplate`. Pro účely tohoto kurzu, dopřejeme si produktu název zobrazovaný v `<h3>` záhlaví. Pod použijeme HTML `<table>` zobrazit zbývající vlastnosti produkt v tabulce čtyři sloupce, kde první a třetí sloupec seznam názvů vlastností a druhá a čtvrtá vypisovat jejich hodnoty.

Tento kód lze zadat v rozhraní úprav ovládacího prvku FormView šablona v Návrháři nebo ručně zadat využijte deklarativní syntaxi. Při práci se šablonami můžu obvykle často rychlejší pracovat přímo s deklarativní syntaxe, ale můžete použít jakýkoli technika vám bude vyhovovat nejvíce.

Následující kód ukazuje FormView deklarativní po `ItemTemplate`pro strukturu se dokončila:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

Všimněte si, že syntaxe databinding – `<%# Eval("ProductName") %>`pro příklad můžete vloží přímo do výstupu šablony. To znamená, nemusí být přiřazené na ovládací prvek Label `Text` vlastnost. Například máme `ProductName` hodnoty zobrazené v `<h3>` prvku pomocí `<h3><%# Eval("ProductName") %></h3>`, které pro produkt se zobrazí takto Chai `<h3>Chai</h3>`.

`ProductPropertyLabel` a `ProductPropertyValue` tříd CSS se používají pro určení stylu produktu názvy vlastností a hodnot v `<table>`. Tyto šablony stylů CSS třídy jsou definovány v `Styles.css` a způsobit, že názvy vlastností normálním a tučným písmem zarovnaný doprava a přidejte pravé odsazení, aby hodnoty vlastností.

Vzhledem k tomu, aby bylo možné zobrazit nejsou k dispozici s FormView, žádné CheckBoxFields `Discontinued` hodnotu jako zaškrtávací políčko jsme musíte přidat vlastní ovládací prvek zaškrtávací políčko. `Enabled` Je nastavena na hodnotu False, takže jen pro čtení a na zaškrtávací políčko `Checked` vlastnost je vázána na hodnotu `Discontinued` datové pole.

S `ItemTemplate` dokončení se zobrazí informace o produktu mnohem více plynulé způsobem. Porovnejte prvku DetailsView. výstup z poslední kurz (obrázek 3) s výstupem ve třídě FormView generována v tomto kurzu (obrázek 4).


[![Od Rigidních výstup prvku DetailsView.](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**Obrázek 3**: The od Rigidních výstup prvku DetailsView ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-formview-s-templates-vb/_static/image9.png))


[![Plynulé výstup FormView](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**Obrázek 4**: The dynamiky FormView výstup ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-formview-s-templates-vb/_static/image12.png))


## <a name="summary"></a>Souhrn

Zatímco ovládací prvky GridView a DetailsView můžou mít svůj výstup přizpůsobili použití vlastností TemplateField, obě stále k dispozici svá data ve formátu tabulky, která vypadá jako boxy. Pro situace, když je potřeba jeden záznam zobrazí pomocí méně od rigidních rozložení, třídě FormView je ideálním řešením. Stejně jako ovládacím prvku DetailsView FormView vykreslí jeden záznam z jeho `DataSource`, ale na rozdíl od ovládacím prvku DetailsView se skládá právě z šablony a nepodporuje pole.

Jak jsme viděli v tomto kurzu, třídě FormView umožňuje flexibilnější rozložení při zobrazení jeden záznam. V budoucích kurzech prozkoumáme ovládacích prvcích DataList a Repeater ovládací prvky, které poskytují stejnou úroveň jako FormsView flexibilitu, ale budou moct zobrazit několik záznamů (např. prvku GridView).

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla E.R. Gilmore. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](using-templatefields-in-the-detailsview-control-vb.md)
> [další](displaying-summary-information-in-the-gridview-s-footer-vb.md)
