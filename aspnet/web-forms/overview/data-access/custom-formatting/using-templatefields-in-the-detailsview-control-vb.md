---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
title: Použití vlastností TemplateField v ovládacím prvku DetailsView (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Stejné vlastností TemplateField možnosti dostupné v prvku GridView jsou také k dispozici pomocí ovládacího prvku DetailsView. V tomto kurzu zobrazíme jednoho produktu...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0b91d5f8-127d-4f6a-b204-f2e2b35ef703
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 1c18e8be25369d6e7d1703e71b80e75adb9f15fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752919"
---
<a name="using-templatefields-in-the-detailsview-control-vb"></a>Použití vlastností TemplateField v ovládacím prvku DetailsView (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_13_VB.exe) nebo [stahovat PDF](using-templatefields-in-the-detailsview-control-vb/_static/datatutorial13vb1.pdf)

> Stejné vlastností TemplateField možnosti dostupné v prvku GridView jsou také k dispozici pomocí ovládacího prvku DetailsView. V tomto kurzu zobrazíme jeden produkt v době použití DetailsView s vlastností TemplateField.


## <a name="introduction"></a>Úvod

Pole TemplateField nabízí vyšší míra flexibility v datech pro vykreslení, než vlastnost BoundField, třídě CheckBoxField, HyperLinkField a další ovládací prvky pole data. V [předchozí kurz o službě](using-templatefields-in-the-gridview-control-vb.md) jsme se podívali na použití pole TemplateField v prvku GridView pro:

- Zobrazte více hodnot dat pole do jednoho sloupce. Konkrétně, jak `FirstName` a `LastName` pole byly sloučeny do jednoho sloupce prvku GridView.
- Pomocí prvku alternativní express datovou hodnotu pole. Jsme viděli, jak zobrazit `HiredDate` hodnotu použití ovládacího prvku kalendář.
- Zobrazit informace o stavu, které jsou založené na podkladová data. Zatímco `Employees` tabulka neobsahuje sloupec, který vrátí počet dní, zaměstnanec bylo v úloze, jsme byli schopni zobrazit tyto informace v prvku GridView příklad v předchozím kurzu s použitím TemplateField a metody pro formátování.

Stejné vlastností TemplateField možnosti dostupné v prvku GridView jsou také k dispozici pomocí ovládacího prvku DetailsView. V tomto kurzu zobrazíme jeden produkt v době použití prvku DetailsView, která obsahuje dvě vlastností TemplateField. Sloučí první TemplateField `UnitPrice`, `UnitsInStock`, a `UnitsOnOrder` datová pole do jednoho řádku prvku DetailsView. Druhý TemplateField zobrazí hodnotu `Discontinued` pole, ale použije metoda formátování se zobrazí "Ano", pokud `Discontinued` je `True`a "Ne" jinak.


[![Dvou vlastností TemplateField se používají pro přizpůsobení zobrazení](using-templatefields-in-the-detailsview-control-vb/_static/image2.png)](using-templatefields-in-the-detailsview-control-vb/_static/image1.png)

**Obrázek 1**: dvou vlastností TemplateField se používají pro přizpůsobení zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image3.png))


Pusťme se do práce!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Krok 1: Vytvoření vazby dat na ovládacím prvku DetailsView.

Jak je popsáno v předchozím kurzu, při práci s vlastností TemplateField je často nejjednodušší začněte vytvořením prvku DetailsView, který obsahuje pouze BoundFields a přidání nových vlastností TemplateField nebo převést existující BoundFields vlastností TemplateField jako potřeby . Proto se pustíte do tohoto kurzu DetailsView přidáním na stránku prostřednictvím návrháře a vytvoříte jejich vazbu na ObjectDataSource, který vrátí seznam produktů. Tyto kroky vytvoří DetailsView s BoundFields všech polí hodnot – datový typ Boolean produktu a třídě CheckBoxField pro jedno pole logická hodnota (vyřazeno).

Otevřít `DetailsViewTemplateField.aspx` stránku a přetáhněte z panelu nástrojů na Návrhář DetailsView. V ovládacím prvku DetailsView inteligentních značek zvolte Přidat nový ovládací prvek ObjectDataSource, která volá `ProductsBLL` třídy `GetProducts()` metody.


[![Přidat nový ovládací prvek ObjectDataSource, která volá metodu GetProducts()](using-templatefields-in-the-detailsview-control-vb/_static/image5.png)](using-templatefields-in-the-detailsview-control-vb/_static/image4.png)

**Obrázek 2**: Přidání nového ovládacího prvku ObjectDataSource tohoto volání `GetProducts()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image6.png))


Pro tuto sestavu odstranit `ProductID`, `SupplierID`, `CategoryID`, a `ReorderLevel` BoundFields. V dalším kroku změnit pořadí BoundFields tak, aby `CategoryName` a `SupplierName` BoundFields zobrazovat ihned za `ProductName` Vlastnost BoundField. Můžete upravit `HeaderText` podle potřeby vlastnosti a vlastnosti formátování pro BoundFields jako vy. Stejně jako s použitím prvku GridView, tyto úpravy Vlastnost BoundField úrovni lze provést prostřednictvím dialogového okna pole (přístupný kliknutím na odkaz upravit pole v ovládacím prvku DetailsView inteligentních značek) nebo využijte deklarativní syntaxi. A konečně, smažte ovládacím prvku DetailsView `Height` a `Width` hodnoty vlastností, aby bylo možné povolit ovládacím prvku DetailsView. Rozbalte na základě dat zobrazí pod kontrolou a zaškrtněte políčko Povolit stránkování v inteligentních značek.

Po provedení těchto změn, deklarativní prvku DetailsView by měl vypadat nějak takto:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample1.aspx)]

Pokud chcete zobrazit stránku prostřednictvím prohlížeče chvíli trvat. V tomto okamžiku byste měli vidět uvedené jednoho produktu (Chai) se zobrazuje název produktu, kategorie, Dodavatel, ceny, jednotek v zásobách, jednotky na pořadí a stav ukončená řádky.


[![Jsou uvedeny podrobnosti produktu pomocí řady BoundFields](using-templatefields-in-the-detailsview-control-vb/_static/image8.png)](using-templatefields-in-the-detailsview-control-vb/_static/image7.png)

**Obrázek 3**: jsou uvedeny podrobnosti produktu pomocí řady BoundFields ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Krok 2: Kombinace cena, jednotek v zásobách a jednotky v pořadí jeden řádek

V řádku pro obsahuje ovládacím prvku DetailsView `UnitPrice`, `UnitsInStock`, a `UnitsOnOrder` pole. Jsme můžete zkombinovat tato data pole do jednoho řádku s TemplateField tak, že přidáte nový TemplateField nebo převedením některou existující `UnitPrice`, `UnitsInStock`, a `UnitsOnOrder` BoundFields na pole TemplateField. Zatímco osobně raději převod existující BoundFields, vyzkoušejme tak, že přidáte nový TemplateField.

Začněte tím, že kliknete na odkaz upravit pole v ovládacím prvku DetailsView inteligentních značek zobrazíte dialogové okno pole. V dalším kroku přidejte nové TemplateField a nastavte jeho `HeaderText` vlastnost "A cena inventáře" a přesunutí nové TemplateField tak, že je umístěn nad `UnitPrice` Vlastnost BoundField.


[![Přidat nový TemplateField k ovládacímu prvku DetailsView.](using-templatefields-in-the-detailsview-control-vb/_static/image11.png)](using-templatefields-in-the-detailsview-control-vb/_static/image10.png)

**Obrázek 4**: přidejte nový TemplateField do ovládacího prvku DetailsView ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image12.png))


Protože toto nové TemplateField bude obsahovat hodnoty aktuálně zobrazené v `UnitPrice`, `UnitsInStock`, a `UnitsOnOrder` BoundFields, je Odebereme.

Posledním úkolem v tomto kroku je definování `ItemTemplate` značky ceny a TemplateField inventář, který lze provést buď prostřednictvím úpravy rozhraní v Návrháři nebo ručně pomocí deklarativní syntaxe ovládacího prvku DetailsView šablony. Stejně jako u prvku GridView, je přístupný kliknutím na odkaz Upravit šablony v inteligentních značek rozhraní úprav šablony ovládacím prvku DetailsView. Odtud můžete vybrat šablonu, kterou chcete upravit z rozevíracího seznamu a pak přidejte všechny webové ovládací prvky ze sady nástrojů.

Pro účely tohoto kurzu, začněte přidáním ovládacího prvku popisku na ceny a inventáře TemplateField `ItemTemplate`. V dalším kroku klikněte na odkaz upravit vlastnosti DataBindings z ovládacího prvku popisku webového inteligentních značek a vytvořit vazbu `Text` vlastnost `UnitPrice` pole.


[![Pole UnitPrice Data svázat vlastnost textu popisku](using-templatefields-in-the-detailsview-control-vb/_static/image14.png)](using-templatefields-in-the-detailsview-control-vb/_static/image13.png)

**Obrázek 5**: vytvoření vazby popisku `Text` vlastnost `UnitPrice` datové pole ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>Formátování ceny jako měna

Uveďte ovládací prvek popisek webu ceny a inventáře TemplateField se teď budou zobrazovat jenom cena za vybraný produkt. Obrázek 6 doposud ukazuje snímek obrazovky náš postup při prohlížení prostřednictvím prohlížeče.


[![Ceny a inventáře TemplateField ukazuje cenu](using-templatefields-in-the-detailsview-control-vb/_static/image17.png)](using-templatefields-in-the-detailsview-control-vb/_static/image16.png)

**Obrázek 6**: The ceny a inventáře TemplateField ukazuje cenu ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image18.png))


Všimněte si, že ceny produktu není naformátovaný jako měnu. S Vlastnost BoundField, formátování je možné tak, že nastavíte `HtmlEncode` vlastnost `False` a `DataFormatString` vlastnost `{0:formatSpecifier}`. Pro TemplateField ale žádné formátování pokyny je nutné zadat v syntaxi vázání dat nebo prostřednictvím metody pro formátování někde definovaný v rámci kódu vaší aplikace (například třídy modelu code-behind stránky technologie ASP.NET).

Chcete-li určit formátování syntaxe vázání dat v ovládacím prvku popisek Web, vrátíte kliknutím na odkaz upravit vlastnosti DataBindings z popisku inteligentních značek dialogové okno datové vazby. Můžete zadat pokyny k formátování přímo do formátu rozevíracího seznamu nebo vyberte některou z definovaných formátovací řetězce. Jak je vlastnost BoundField `DataFormatString` vlastnost, formátování je určen pomocí `{0:formatSpecifier}`.

Pro `UnitPrice` pole použijte formátování měny uvedené výběrem hodnoty příslušného rozevíracího seznamu nebo zadáním `{0:C}` ručně.


[![Cena formátu měny](using-templatefields-in-the-detailsview-control-vb/_static/image20.png)](using-templatefields-in-the-detailsview-control-vb/_static/image19.png)

**Obrázek 7**: formátování ceny jako měnu ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image21.png))


Deklarativní, formátování specifikace je označen jako druhý parametr do `Bind` nebo `Eval` metody. Nastavení právě vytvořili prostřednictvím návrháře výsledků v následující výraz datové vazby v deklarativním označení:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Přidání zbývající datových polí pole TemplateField

V tuto chvíli jsme zobrazí a ve formátu `UnitPrice` data pole Price a TemplateField inventáře, ale stále potřebovat zobrazit `UnitsInStock` a `UnitsOnOrder` pole. Umožňuje zobrazit tyto na řádku níže cena a v závorkách. Z rozhraní úprav šablony v Návrháři je možné přidat tyto značky umístěním kurzoru v rámci šablony a jednoduše zadáte text, který se zobrazí. Alternativně je možné zadat Tento kód přímo v deklarativní syntaxe.

Přidání statických značky, ovládacích prvků popisku a syntaxe vázání dat tak, že ceny a inventáře TemplateField zobrazuje informace o ceně a inventáře takto:

*UnitPrice*  
(**v zásobách / On Order:** *UnitsInStock* / *ObjednánoJednotek*)

Po provedení této úlohy prvku DetailsView deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample3.aspx)]

S těmito změnami jsme sloučili informace ceny a inventáře do jednoho řádku prvku DetailsView.


[![Ceny a informace o inventáři se zobrazí v jednoho řádku](using-templatefields-in-the-detailsview-control-vb/_static/image23.png)](using-templatefields-in-the-detailsview-control-vb/_static/image22.png)

**Obrázek 8**: The ceny a informace o inventáři se zobrazí v jednoho řádku ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>Krok 3: Přizpůsobení informace ukončená pole

`Products` Tabulky `Discontinued` sloupec je bitová hodnota, která určuje, zda produkt už není podporován. Při vytváření vazby prvku DetailsView (nebo ovládacího prvku GridView) na ovládací prvek zdroje dat, jako jsou pole logická hodnota `Discontinued`, jsou implementovány jako CheckBoxFields, zatímco – datový typ Boolean hodnotu pole, jako jsou `ProductID`, `ProductName`, atd., jsou implementovány jako BoundFields. Třídě CheckBoxField vykreslí jako zakázané zaškrtávací políčko je zaškrtnuté políčko, pokud je datové pole Hodnota True a nezaškrtnuto jinak.

Místo zobrazení třídě CheckBoxField může být vhodné místo toho zobrazit text označující jestli produktu je přerušeno. K tomu jsme mohli odeberte třídě CheckBoxField v ovládacím prvku DetailsView a pak přidejte vlastnost BoundField jehož `DataField` nastavenou na `Discontinued`. Za chvíli to provést. Po této změně ovládacím prvku DetailsView zobrazí text "True" pro odpojené produkty a "False" pro produkty, které jsou stále aktivní.


[![Řetězce True a False slouží k zobrazení stavu ukončená](using-templatefields-in-the-detailsview-control-vb/_static/image26.png)](using-templatefields-in-the-detailsview-control-vb/_static/image25.png)

**Obrázek 9**: řetězce True a False se používají k zobrazení stavu Vyřazeno ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image27.png))


Představte si, že jsme nechtěli řetězce "True" nebo "False" mají být použity, ale "Ano" a "Žádný", místo toho. Tato vlastní nastavení lze provést s pomocí TemplateField a metody pro formátování. Metody pro formátování můžete provést v libovolném počtu vstupních parametrů, ale musí vracet HTML (jako řetězec) k vložení do šablony.

Přidejte metodu pro formátování `DetailsViewTemplateField.aspx` stránky použití modelu code-behind třídu s názvem `DisplayDiscontinuedAsYESorNO` , který přijme `Northwind.ProductsRow` jako vstupní parametr objekt a vrátí hodnotu typu string. Jak je popsáno v předchozím kurzu, tato metoda *musí* označit jako `Protected` nebo `Public` -li být přístupné ze šablony.


[!code-vb[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample4.vb)]

Tato metoda zkontroluje vstupní parametr (`discontinued`) a vrátí "Ano", pokud je `True`jinak "Ne".

> [!NOTE]
> V metodě formátování zkoumány podle předchozí kurz odvolání, který jsme se předávání v datovém poli, které mohou obsahovat `NULL` s a je proto potřeba a zkontrolujte, zda zaměstnance `HiredDate` hodnota vlastnosti měli nějakou databázi `NULL` hodnota před přístup k `EmployeesRow`společnosti `HiredDate` vlastnost. Tato kontrola není zde vyžadováno, protože `Discontinued` sloupec nikdy nemůžete mít databázi `NULL` hodnoty přiřazené. Kromě toho to je důvod, proč metodu můžete přijímají logickou hodnotu vstupní parametr namísto nutnosti tak, aby přijímal `ProductsRow` instance nebo parametr typu `Object`.


Pomocí této metody pro formátování kompletní, už jen zbývá k jeho volání z TemplateField `ItemTemplate`. Chcete-li vytvořit pole TemplateField buď odstranit `Discontinued` Vlastnost BoundField a přidat nový TemplateField nebo převést `Discontinued` Vlastnost BoundField na pole TemplateField. Pak ze zobrazení deklarativní úpravy pole TemplateField tak, aby obsahoval jenom šablona ItemTemplate, která volá `DisplayDiscontinuedAsYESorNO` metodu hodnotu aktuálního `ProductRow` instance `Discontinued` vlastnost. To je přístupná prostřednictvím `Eval` metody. Konkrétně TemplateField kód by měl vypadat:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample5.aspx)]

To způsobí, že `DisplayDiscontinuedAsYESorNO` metoda má být vyvolána při vykreslování ovládacím prvku DetailsView předávajícího `ProductRow` instance `Discontinued` hodnotu. Protože `Eval` metoda vrátí hodnotu typu `Object`, ale `DisplayDiscontinuedAsYESorNO` metoda očekává, že vstupní parametr typu `Boolean`, jsme přetypovat `Eval` metody vrací hodnotu `Boolean`. `DisplayDiscontinuedAsYESorNO` Metoda pak vrátí "Ano" nebo "Ne" přijímá v závislosti na hodnotě. Vrácená hodnota je, co se zobrazí v prvku DetailsView. Tento řádek (viz obrázek 10).


[![Ano nebo ne hodnoty se teď zobrazují v řádku vyřazeno](using-templatefields-in-the-detailsview-control-vb/_static/image29.png)](using-templatefields-in-the-detailsview-control-vb/_static/image28.png)

**Obrázek 10**: Ano nebo ne hodnoty se teď zobrazují v řádku Vyřazeno ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image30.png))


## <a name="summary"></a>Souhrn

TemplateField v ovládacím prvku DetailsView umožňuje vyšší míra flexibility v zobrazení dat, než je k dispozici s dalšími prvky pole a jsou ideální pro situace, kde:

- Potřebujete více datových polí zobrazeného do jednoho sloupce GridView
- Data je nejlepší vyjádřena pomocí webový ovládací prvek spíše než prostý text
- Výstup závisí na podkladová data, například zobrazení metadat nebo přeformátování dat

Zatímco vlastností TemplateField umožňují vyšší stupeň flexibilitu při vykreslování prvku DetailsView podkladová data, výstup prvku DetailsView. stále pracuje trochu boxy jako jednotlivá pole se vykreslí jako řádek v HTML `<table>`.

Ovládacího prvku FormView nabízí větší míru flexibility při konfiguraci vykresleného výstupu. FormView neobsahuje pole, ale spíš jenom řadu šablon (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`, a tak dále). Uvidíme, jak používat FormView dosáhnout ještě větší kontrolu nad vykreslovaném rozložení v následujícím kurzem.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Jagers daň. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](using-templatefields-in-the-gridview-control-vb.md)
> [další](using-the-formview-s-templates-vb.md)
