---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
title: Formátování DataList a opakovače na základě údajů o (C#) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu jsme projdete kroky příklady, jak jsme formátování vzhledu DataList ovládacích prvků a opakovače, buď pomocí formátování funkce s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 83e3d759-82b8-41e6-8d62-f0f4b3edec41
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 00dac460ad905d34632bca3249e019ddc626e440
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876212"
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-c"></a>Formátování DataList a opakovače na základě údajů o (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_CS.exe) nebo [stáhnout PDF](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/datatutorial30cs1.pdf)

> V tomto kurzu jsme projdete kroky příklady, jak jsme formátování vzhledu DataList ovládacích prvků a opakovače, buď pomocí formátování funkce v rámci šablony nebo zpracování události vycentrovat.


## <a name="introduction"></a>Úvod

Jak jsme viděli v předchozím kurzu, DataList nabízí řadu vlastnosti související se styly, které ovlivňují její vzhled. Konkrétně jsme viděli, jak přiřadit výchozí tříd CSS DataList s `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, a `SelectedItemStyle` vlastnosti. Kromě těchto čtyř vlastností prvku DataList obsahuje několik jiných vlastností související se styly, jako `Font`, `ForeColor`, `BackColor`, a `BorderWidth`, a další. Ovládacím prvku Repeater neobsahuje žádné vlastnosti související se styly. Taková nastavení stylu musí být provedeny přímo v kódu v šablonách s opakovače.

Často ale způsob formátování data, závisí na samotná data. Například při výpisu produkty jsme chtít zobrazit informace o produktu v Barva světla šedé písma, pokud se zastaví, nebo může chceme, abyste měli na očích `UnitsInStock` hodnotu, pokud je nula. Jak jsme viděli v předchozí kurzy, rutina GridView, DetailsView a FormView nabízejí dvou různých způsobů, jak formátu jejich vzhled na základě jejich dat:

- **`DataBound` Událostí** vytvoření obslužné rutiny události pro příslušné `DataBound` událost, která aktivuje se po data byla svázána na jednotlivé položky (pro GridView bylo `RowDataBound` událostí; DataList a opakovače je `ItemDataBound`událostí). V takovém případě může být prověřen obslužnou rutinu, pouze data vázaná a formátování rozhodnutí udělali. Jsme se zaměřili na tato technika v [vlastní formátování dat na základě po](../custom-formatting/custom-formatting-based-upon-data-cs.md) kurzu.
- **Formátování funkce v šablonách** při použití TemplateFields v ovládacích prvcích DetailsView nebo GridView nebo šablony v ovládacím prvku FormView, jsme formátování funkci přidat do třídy kódu stránky s ASP.NET, vrstvu obchodní logiky nebo všechny Další knihovny tříd, které je dostupné z webové aplikace. Tato funkce formátování lze zadat libovolný počet vstupních parametrů, ale musí vracet HTML pro vykreslení v šabloně. Formátování funkce byly prozkoumány nejprve v [pomocí TemplateFields v ovládacím prvku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) kurzu.

Obě tyto formátování techniky jsou k dispozici s ovládacími prvky DataList a opakovače. V tomto kurzu jsme projdete kroky příklady použití obě metody pro oba ovládací prvky.

## <a name="using-theitemdataboundevent-handler"></a>Pomocí`ItemDataBound`obslužné rutiny události

Data je vázána k DataList, buď z prvku zdroje dat, nebo prostřednictvím prostřednictvím kódu programu přiřazování dat k ovládacímu prvku s `DataSource` vlastnost a volání jeho `DataBind()` metoda DataList s `DataBinding` aktivuje událost, výčet, zdroj dat a každý záznam dat je vázána DataList. Pro každý záznam ve zdroji dat prvku DataList vytvoří [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) objektu, který je pak vázána na aktuální záznam. Během tohoto procesu DataList vyvolává dvě události:

- **`ItemCreated`** Aktivuje se po `DataListItem` byla vytvořena
- **`ItemDataBound`** Aktivuje se po záznam na aktuální záznam byla svázána se `DataListItem`

Následující kroky popisují proces vytváření vazby dat pro ovládací prvek DataList.

1. DataList s [ `DataBinding` událostí](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) aktivuje
2. Data je vázána DataList  
  
   Pro každý záznam ve zdroji dat. 

    1. Vytvoření `DataListItem` objektu
    2. Ještě efektivněji [ `ItemCreated` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Záznam, který chcete vytvořit vazbu `DataListItem`
    4. Ještě efektivněji [ `ItemDataBound` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Přidat `DataListItem` k `Items` kolekce

Při vytváření vazby dat k ovládacím prvku Repeater, prochází přesné stejné pořadí kroků. Jediným rozdílem je, že místo `DataListItem` vytváří instance, používá opakovače [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Astute čtečky jste si nejspíše všimli mírné anomálií mezi pořadí kroků, které transpire při DataList a opakovače je vázána na data a když GridView vázané na data. Na konci procesu vázání dat tail GridView vyvolá `DataBound` událostí; ale ovládací prvek DataList ani opakovače mít takové události. Je to proto, že ovládací prvky DataList a opakovače byly vytvořeny zpět v určeném časovém rozmezí ASP.NET 1.x před vzoru obslužná rutina události před a po úrovně stalo běžné.


Jako s GridView, je vytvoření obslužné rutiny události pro jednu možnost pro formátování na základě dat `ItemDataBound` událostí. Tuto obslužnou rutinu události by kontrolovat data, která měla právě vázána na `DataListItem` nebo `RepeaterItem` a ovlivnit formátování ovládacího prvku podle potřeby.

Pro ovládací prvek DataList změny formátování pro celou položku lze implementovat pomocí `DataListItem` s vlastnosti související se styly, mezi které patří standardní `Font`, `ForeColor`, `BackColor`, `CssClass`a tak dále. Pokud chcete ovlivnit formátování konkrétní ovládací prvky webového v rámci šablony s DataList, musíme prostřednictvím kódu programu přístup a úpravy stylu tyto ovládací prvky webového. Jsme viděli, jak provést tuto zpět v *vlastní formátování dat na základě po* kurzu. Ovládacím prvku Repeater, jako `RepeaterItem` třída nemá žádné vlastnosti související s styl; proto provedeny všechny změny související se styly `RepeaterItem` v `ItemDataBound` obslužné rutiny události musí provést programově přístup k a aktualizace ovládacích prvků v rámci Šablona.

Vzhledem k tomu `ItemDataBound` formátování technika pro jsou téměř stejné, našem příkladu se soustředí na pomocí prvku DataList DataList a opakovače.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Krok 1: Zobrazení informací o produktu v prvku DataList

Než budeme starosti formátování, umožňují s vytvořit stránky, která používá k zobrazení informací o produktu DataList. V [předchozí kurzu](displaying-data-with-the-datalist-and-repeater-controls-cs.md) jsme vytvořili DataList jejichž `ItemTemplate` zobrazí každý název produktu s, kategorie, dodavatele, množství jednotky a cena. Umožní s opakováním zde tato funkce v tomto kurzu. K tomu, můžete buď znovu vytvořit DataList a jeho ObjectDataSource od nuly nebo můžete zkopírovat přes tyto ovládací prvky na stránce vytvořili v předchozí kurzu (`Basics.aspx`) a vložit je na stránce pro účely tohoto kurzu (`Formatting.aspx`).

Jakmile replikace funkce DataList a ObjectDataSource `Basics.aspx` do `Formatting.aspx`, pozorně změnit DataList s `ID` vlastnost z `DataList1` k více popisný `ItemDataBoundFormattingExample`. V dalším kroku zobrazte DataList v prohlížeči. Jak ukazuje obrázek 1, formátování jediným rozdílem mezi každý produkt je, že střídavě barvu pozadí.


[![Produkty, které jsou uvedeny v ovládacím prvku DataList](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image1.png)

**Obrázek 1**: produkty jsou uvedeny v ovládacím prvku DataList ([Kliknutím zobrazit obrázek v plné velikosti](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image3.png))


V tomto kurzu mohli s formátu prvku DataList tak, aby všechny produkty s cenu nižší než 20,00 bude mít i jeho název a zvýrazněná žlutý cena jednotky.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Krok 2: Určení prostřednictvím kódu programu hodnotu Data v obslužná rutina události ItemDataBound

Vzhledem k tomu jenom produkty s cenou pod 20,00 bude mít vlastní formátování, které se použijí, jsme musí být schopní určit, každý s cena produktu. Při vytváření vazby dat k DataList, prvku DataList zobrazí záznamy v zdrojem dat a pro každý záznam vytvoří `DataListItem` instance, vazba záznamu zdroje dat `DataListItem`. Po konkrétní záznam s dat byla svázána se aktuální `DataListItem` objekt, DataList s `ItemDataBound` událost je aktivována. Můžeme vytvořit obslužnou rutinu události pro tuto událost, chcete-li zkontrolovat hodnoty dat pro aktuální `DataListItem` a na základě těchto hodnot, proveďte změny formátování nezbytné.

Vytvoření `ItemDataBound` událost pro prvku DataList a přidejte následující kód:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample1.cs)]

Při koncept a sémantiku za DataList s `ItemDataBound` obslužné rutiny události jsou stejné jako v případě rutina GridView s `RowDataBound` obslužné rutiny událostí v *vlastní formátování dat na základě při* se liší syntaxe kurzu mírně. Při `ItemDataBound` aktivuje událost `DataListItem` jenom vazby na data je předána do odpovídající obslužné rutiny události prostřednictvím `e.Item` (místo `e.Row`, stejně jako u GridView s `RowDataBound` obslužné rutiny události). DataList s `ItemDataBound` aktivuje obslužné rutiny události pro *každý* řádek přidán do DataList, včetně řádky záhlaví, zápatí řádků a Oddělovač řádků. Informace o produktu je však vázaný jenom na řádky data. Proto při použití `ItemDataBound` událost a kontrolovat data vázaná na prvku DataList, je potřeba nejdřív zkontrolujte, zda jsme re práce s datovou položku. To lze provést pomocí kontroly `DataListItem` s [ `ItemType` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), která může mít jednu z [následující hodnoty osm](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Obě `Item` a `AlternatingItem``DataListItem` s způsob vytvoření DataList s datových položek. Za předpokladu, že jsme re práce `Item` nebo `AlternatingItem`, jsme získat přístup skutečnou `ProductsRow` instance, která byla vázána na aktuální `DataListItem`. `DataListItem` s [ `DataItem` vlastnost](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) obsahuje odkaz na `DataRowView` objekt, jehož `Row` vlastnost poskytuje odkaz na skutečnou `ProductsRow` objektu.

Dále zkontrolujte jsme `ProductsRow` instance s `UnitPrice` vlastnost. Od tabulky produktů s `UnitPrice` pole umožňuje `NULL` hodnoty, před pokusem o přístup `UnitPrice` vlastnost jsme měli nejdřív zkontrolovat, zjistěte, zda má `NULL` hodnotu pomocí `IsUnitPriceNull()` metoda. Pokud `UnitPrice` hodnota není `NULL`, jsme potom zkontrolujte, zda ji s menší než 20,00. Pokud je skutečně v části 20,00, potřebujeme poté použijte vlastní formátování.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Krok 3: Zvýraznění s názvem produktu a ceny

Jakmile víme, že je cena produktu s méně než 20,00, zbývá zvýrazněte jeho název a cena. K tomu, jsme musí nejprve prostřednictvím kódu programu odkazovat ovládací prvky popisek v `ItemTemplate` , zobrazení, název produktu s a ceny. Dále je potřeba je lze zobrazit žlutý pozadí. Informace o formátování lze použít přímou úpravou popisků `BackColor` vlastnosti (`LabelID.BackColor = Color.Yellow`); v ideálním případě by ale všechny záležitosti související s zobrazení by měla být vyjádřena prostřednictvím šablony stylů CSS. Ve skutečnosti jsme už máte šablonu stylů, která obsahuje požadovanou formátování definované v `Styles.css`  -  `AffordablePriceEmphasis`, který byl vytvořen a zabývá *vlastní formátování dat na základě po* kurzu.

Chcete-li použít formátování, jednoduše nastavte dva ovládací prvky popisek webového `CssClass` vlastnosti, které chcete `AffordablePriceEmphasis`, jak je znázorněno v následujícím kódu:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample2.cs)]

S `ItemDataBound` obslužné rutiny události dokončen, pokroku `Formatting.aspx` stránku v prohlížeči. Jak ukazuje obrázek 2, tyto produkty s cenou pod 20,00 mít jejich název a cena zvýrazněná.


[![Tyto produkty nižší než 20,00 jsou vyznačené](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image4.png)

**Obrázek 2**: těch, které produkty nižší než 20,00 jsou vyznačené ([Kliknutím zobrazit obrázek v plné velikosti](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image6.png))


> [!NOTE]
> Vzhledem k tomu, že DataList se vykresluje jako HTML `<table>`, jeho `DataListItem` instance mít vlastnosti související se styly, které lze nastavit chcete konkrétní styl celého položkou provést. Například, pokud jsme chtěli zvýrazněte *celý* položky žlutý při jeho cena byla menší než 20,00 jsme může nahradit kód, který odkazuje štítky a nastavte jejich `CssClass` vlastnosti s následující kód: `e.Item.CssClass = "AffordablePriceEmphasis"` (viz obrázek 3).


`RepeaterItem` S, který vytváří ovládacím prvku Repeater však tento t nabízí takové úrovni styl vlastnosti. Proto vlastní formátování opakovače vyžaduje použití vlastnosti stylu pro ovládací prvky webového v rámci šablony opakovače s, stejně jako jsme provedli na obrázku 2.


[![Celý položka produktu je označený pro produkty v rámci 20,00](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image7.png)

**Obrázek 3**: položka celé produktu je označený pro produkty v rámci 20,00 ([Kliknutím zobrazit obrázek v plné velikosti](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Pomocí formátování funkcí z v rámci šablony

V *pomocí TemplateFields v ovládacím prvku GridView* kurzu jsme viděli, jak použít vlastní formátování na základě dat pomocí formátování funkce v rámci GridView TemplateField vázaným na řádky s GridView. Formátování funkce je metoda, která můžete vyvolat z šablony a vrátí kód HTML pro vypuštění na příslušné místo. Formátování funkce mohou být uloženy ve třídě kódu stránky s ASP.NET, nebo můžete centralizované do třídy souborů v `App_Code` složku nebo v samostatné projektu knihovny tříd. Přesunutí formátování funkce mimo třídy s kódu stránky ASP.NET je ideální, pokud máte v úmyslu používat funkci stejné formátování na více stránkách ASP.NET nebo v jiných webových aplikací ASP.NET.

K předvedení funkcí formátování, umožňují s mít informace o produktu, které zahrnují text [nákup UKONČEN] vedle názvu produktu s, pokud ji s zastaveny. Navíc umožňují s mít zvýrazněná žlutý pokud cena ho s menší než 20,00 (jako jsme to udělali `ItemDataBound` příklad obslužná rutina události); Pokud cenu je 20,00 nebo vyšší, umožňují s nezobrazí skutečná cena, ale místo toho volat text, prosím cena uvozovky. Obrázek 4 ukazuje snímek obrazovky s produkty zobrazení seznamu pomocí těchto pravidel formátování použít.


[![Nákladné produktů ceny se nahradí Text, volejte uvozovky cena](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image10.png)

**Obrázek 4**: nákladné produktů, ceny se nahradí Text, volejte cena uvozovky ([Kliknutím zobrazit obrázek v plné velikosti](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>Krok 1: Vytvoření formátování funkce

V tomto příkladu budeme potřebovat dvě funkce formátování, ten, který zobrazí název produktu spolu s textem [nákup UKONČEN], v případě potřeby a jiné zobrazující zvýrazněná ceny, pokud ji s menší než 20,00 nebo text, volejte o cenovou nabídku, jinak hodnota. Umožní s vytvořte tyto funkce ve třídě kódu stránky s ASP.NET a pojmenujte je `DisplayProductNameAndDiscontinuedStatus` a `DisplayPrice`. Obě metody musíte vrátit k vykreslení jako řetězec HTML a obě musí být označen `Protected` (nebo `Public`) Chcete-li být volána z části ASP.NET stránky s deklarativní syntaxe. Následuje kód pro tyto dvě metody:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample3.cs)]

Všimněte si, že `DisplayProductNameAndDiscontinuedStatus` metoda přijímá hodnoty `productName` a `discontinued` data polí jako skalárních hodnot, zatímco `DisplayPrice` metoda přijímá `ProductsRow` instance (ne `unitPrice` skalární hodnotu). Bude fungovat buď přístup; ale pokud formátování funkce pracuje s skalárních hodnot, které mohou obsahovat databáze `NULL` hodnoty (například `UnitPrice`; ani `ProductName` ani `Discontinued` povolit `NULL` hodnoty), musí dát zvláštní pozor při zpracování těchto Skalární vstupy.

Konkrétně musí být vstupní parametr typu `Object` vzhledem k tomu může být hodnota příchozí `DBNull` instance namísto očekávaného datového typu. Kromě toho musí být provedena kontrola k určení, zda je hodnota příchozí databáze `NULL` hodnotu. To znamená pokud jsme chtěli `DisplayPrice` metoda tak, aby přijímal cenu jako skalární hodnota, jsme d muset použít následující kód:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample4.cs)]

Všimněte si, že `unitPrice` vstupní parametr není typu `Object` a která byla upravena podmíněného příkaz pro zjištění, pokud `unitPrice` je `DBNull` nebo ne. Kromě toho od `unitPrice` vstupní parametr, je předaná jako `Object`, musí být převedena na desítkovou hodnotu.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Krok 2: Volání funkce formátování z vlastnosti s ItemTemplate

S formátování funkce přidané do Naše třída kódu stránky s ASP.NET, zbývá k vyvolání těchto formátování funkce z DataList s `ItemTemplate`. K volání funkce formátování ze šablony, umístíte volání funkce v rámci datové vazby syntaxe:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample5.aspx)]

V DataList s `ItemTemplate` `ProductNameLabel` ovládací prvek popisek webu aktuálně zobrazuje název produktu s přiřazením jeho `Text` vlastnost výsledek z `<%# Eval("ProductName") %>`. Aby bylo možné používat, se zobrazí název a text [nákup UKONČEN], v případě potřeby, aktualizujte deklarativní syntaxi tak, aby místo toho přiřadí `Text` hodnotu vlastnosti o `DisplayProductNameAndDiscontinuedStatus` metoda. Pokud to uděláte, jsme musí předat název produktu s a – starší formáty hodnot pomocí `Eval("columnName")` syntaxe. `Eval` Vrátí hodnotu typu `Object`, ale `DisplayProductNameAndDiscontinuedStatus` metoda očekává vstupní parametry typu `String` a `Boolean`; proto jsme hodnot vrácených přetypovat `Eval` metodu pro typy očekávané vstupní parametr, například takto:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample6.aspx)]

Chcete-li zobrazit cenu, můžeme jednoduše nastavit `UnitPriceLabel` štítek s `Text` vlastnost na hodnotu vrácenou `DisplayPrice` metoda, stejně jako jsme to udělali pro zobrazení názvu produktu s a [ZASTAVÍ] text. Ale namísto předávání `UnitPrice` jako skalární vstupní parametr jsme místo předat celý `ProductsRow` instance:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample7.aspx)]

Pomocí volání formátování funkce zavedené pozorně naše postup zobrazit v prohlížeči. Obrazovce měla vypadat podobně jako obrázek 5 – starší formáty produkty, včetně textu [nákup UKONČEN] a tyto produkty výpočet nákladů více než 20,00 s jejich cena nahrazen textem prosím volání cena uvozovky.


[![Nákladné produktů ceny se nahradí Text, volejte uvozovky cena](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image13.png)

**Obrázek 5**: nákladné produktů, ceny se nahradí Text, volejte cena uvozovky ([Kliknutím zobrazit obrázek v plné velikosti](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image15.png))


## <a name="summary"></a>Souhrn

Formátování obsah ovládacího prvku DataList nebo opakovače na základě dat, lze provést pomocí dvě techniky. První způsob je vytvoření obslužné rutiny události pro `ItemDataBound` událost, která aktivuje jako jednotlivé záznamy ve zdroji dat je vázána na nový `DataListItem` nebo `RepeaterItem`. V `ItemDataBound` obslužná rutina události, může být prověřen aktuální položka s data a pak formátování lze použít pro obsah šablony, nebo pro `DataListItem` s na celý samotnou položku.

Vlastní formátování Alternativně může být dosaženo pomocí formátování funkce. Formátování funkce je metoda, která se může vyvolat z prvku DataList nebo opakovače s šablony, které vrátí kód HTML pro vydávání na příslušné místo. Často vrácené funkcí formátování HTML je určen podle hodnoty je svázán s aktuální položkou. Tyto hodnoty mohou být předány do formátování funkce, jako skalárních hodnot nebo pomocí předávání celý objekt je vázána na položku (například `ProductsRow` instance).

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Yaakov Ellis Randy Schmidt a Liz Shulok. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](displaying-data-with-the-datalist-and-repeater-controls-cs.md)
> [další](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
