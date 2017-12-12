---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
title: "Pomocí TemplateFields v ovládacím prvku DetailsView (VB) | Microsoft Docs"
author: rick-anderson
description: "K dispozici GridView stejné TemplateFields funkce jsou k dispozici pomocí ovládacího prvku DetailsView. V tomto kurzu zobrazujeme jeden produktu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0b91d5f8-127d-4f6a-b204-f2e2b35ef703
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: b368a651253a569865bb92fa93d3462f88d8935f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-templatefields-in-the-detailsview-control-vb"></a>Pomocí TemplateFields v ovládacím prvku DetailsView (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_13_VB.exe) nebo [stáhnout PDF](using-templatefields-in-the-detailsview-control-vb/_static/datatutorial13vb1.pdf)

> K dispozici GridView stejné TemplateFields funkce jsou k dispozici pomocí ovládacího prvku DetailsView. V tomto kurzu zobrazujeme jeden produktu současně pomocí DetailsView obsahující TemplateFields.


## <a name="introduction"></a>Úvod

TemplateField nabízí vyšší stupeň flexibilitu při vykreslování dat, než BoundField, vlastnost CheckBoxField, HyperLinkField a jiných ovládacích prvků pole data. V [předchozí kurzu](using-templatefields-in-the-gridview-control-vb.md) jsme se podívali na použití TemplateField v GridView na:

- Zobrazte více hodnot polí data v jeden sloupec. Konkrétně i `FirstName` a `LastName` pole měla zkombinované do jednoho sloupce GridView.
- Pomocí prvku alternativní express hodnotu datového pole. Jsme viděli, jak zobrazit `HiredDate` hodnotu pomocí ovládacího prvku kalendář.
- Zobrazit na základě dat základní informace o stavu. Když `Employees` tabulka neobsahuje sloupec, který vrátí počet dní zaměstnanec byl v úloze, jsme byli schopni zobrazit tyto informace v příkladu rutina GridView v předchozí kurzu s použitím TemplateField a metody pro formátování.

K dispozici GridView stejné TemplateFields funkce jsou k dispozici pomocí ovládacího prvku DetailsView. V tomto kurzu zobrazujeme jeden produktu současně pomocí DetailsView, který obsahuje dva TemplateFields. První TemplateField sloučí `UnitPrice`, `UnitsInStock`, a `UnitsOnOrder` datová pole do jednoho řádku DetailsView. Druhý TemplateField se zobrazí hodnota `Discontinued` pole, ale použije formátování metoda zobrazí tlačítko Ano"Pokud `Discontinued` je `True`a"Ne"jinak.


[![Dva TemplateFields slouží k přizpůsobení zobrazení](using-templatefields-in-the-detailsview-control-vb/_static/image2.png)](using-templatefields-in-the-detailsview-control-vb/_static/image1.png)

**Obrázek 1**: dva TemplateFields slouží k přizpůsobení zobrazení ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image3.png))


Můžeme začít!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Krok 1: Vytvoření vazby dat DetailsView

Jak je popsáno v předchozí kurzu při práci s TemplateFields je často nejjednodušší začněte vytvořením DetailsView ovládací prvek, který obsahuje pouze BoundFields a pak přidejte nový TemplateFields nebo převést stávající BoundFields TemplateFields jako potřeby . V tomto kurzu proto Začněte přidáním na stránku prostřednictvím návrháře DetailsView a vytvoření vazby ObjectDataSource, který vrátí seznam produktů. Tyto kroky vytvoří DetailsView s BoundFields pro každé pole hodnot logická hodnota produktu a vlastnost CheckBoxField do jednoho pole logická hodnota (Nákup ukončen).

Otevřete `DetailsViewTemplateField.aspx` stránky a přetáhněte ji DetailsView z panelu nástrojů na návrháře. Z prvku DetailsView inteligentních značek vyberte přidání nové ovládacího prvku ObjectDataSource, která volá `ProductsBLL` třídy `GetProducts()` metoda.


[![Přidání nové ovládacího prvku ObjectDataSource, která volá metodu GetProducts()](using-templatefields-in-the-detailsview-control-vb/_static/image5.png)](using-templatefields-in-the-detailsview-control-vb/_static/image4.png)

**Obrázek 2**: Přidání nové ovládacího prvku ObjectDataSource této vyvolá `GetProducts()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image6.png))


Pro tuto sestavu odebrat `ProductID`, `SupplierID`, `CategoryID`, a `ReorderLevel` BoundFields. V dalším kroku změnit pořadí BoundFields tak, aby `CategoryName` a `SupplierName` BoundFields zobrazí okamžitě po `ProductName` BoundField. Můžete upravit `HeaderText` vlastnosti a vlastnosti formátování pro BoundFields podle svých potřeb. Jako s GridView, tyto úpravy BoundField úrovni lze provést pomocí pole dialogových oken (k dispozici kliknutím na odkaz upravit pole v ovládacím prvku DetailsView inteligentních značek) nebo pomocí deklarativní syntaxe. Nakonec vyčistit prvku DetailsView `Height` a `Width` hodnoty vlastností, aby bylo možné povolit DetailsView řízení rozšiřovat na základě dat zobrazí a zaškrtněte políčko Povolit stránkování v inteligentní značky.

Po provedení těchto změn, deklarativní prvek DetailsView by měl vypadat takto:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample1.aspx)]

Chcete-li zobrazit stránku prostřednictvím prohlížeče chvíli trvat. V tomto okamžiku byste měli vidět uvedené jednoho produktu (Chai) s řádky zobrazuje název produktu, kategorie, dodavatele, ceny, jednotek na skladě, jednotky v pořadí a její stav – starší formáty.


[![Jsou uvedeny podrobnosti produktu pomocí řady BoundFields](using-templatefields-in-the-detailsview-control-vb/_static/image8.png)](using-templatefields-in-the-detailsview-control-vb/_static/image7.png)

**Obrázek 3**: jsou uvedeny podrobnosti produktu pomocí řady BoundFields ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Krok 2: Kombinace ceníku, jednotky v Stock a jednotek v pořadí do jeden řádek

DetailsView má řádek pro `UnitPrice`, `UnitsInStock`, a `UnitsOnOrder` pole. Jsme můžete kombinovat těchto polí dat do jednoho řádku s TemplateField přidáním nové TemplateField nebo převedením jednu z existujících `UnitPrice`, `UnitsInStock`, a `UnitsOnOrder` BoundFields do TemplateField. Při osobní nechci převod existující BoundFields, můžeme přidáním nové TemplateField praxi.

Spusťte kliknutím na odkaz upravit pole v ovládacím prvku DetailsView inteligentní značky se zprovoznit dialogové okno pole. V dalším kroku přidejte TemplateField nový a nastavit jeho `HeaderText` vlastnost "A cena inventáře" a přesunutí nové TemplateField, které je umístěn výše `UnitPrice` BoundField.


[![Přidat nové TemplateField do ovládacího prvku DetailsView](using-templatefields-in-the-detailsview-control-vb/_static/image11.png)](using-templatefields-in-the-detailsview-control-vb/_static/image10.png)

**Obrázek 4**: Přidání nové TemplateField do ovládacího prvku DetailsView ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image12.png))


Vzhledem k tomu, že tento nový TemplateField bude obsahovat hodnoty v aktuálně zobrazený `UnitPrice`, `UnitsInStock`, a `UnitsOnOrder` BoundFields, můžeme je odebrat.

Poslední úloha pro tento krok je určit `ItemTemplate` kód pro cenové a TemplateField inventáře, který lze provést buď pomocí šablony DetailsView úpravy rozhraní v Návrháři nebo ručně pomocí deklarativní syntaxe ovládacího prvku. Stejně jako u GridView, jsou přístupné DetailsView šablony úpravy rozhraní kliknutím na odkaz Upravit šablony v inteligentní značky. Tady můžete vybrat šablonu, kterou chcete upravit v rozevíracím seznamu a poté přidejte všechny ovládací prvky webového z panelu nástrojů.

V tomto kurzu, začněte přidáním ovládací prvek popisek ceny a inventáře TemplateField `ItemTemplate`. Potom klikněte na odkaz Upravit datové vazby z ovládacího prvku popisek webového inteligentních značek a vytvořte vazbu `Text` vlastnost, která má `UnitPrice` pole.


[![Pole UnitPrice dat vytvořit vazbu popisku Text – vlastnost](using-templatefields-in-the-detailsview-control-vb/_static/image14.png)](using-templatefields-in-the-detailsview-control-vb/_static/image13.png)

**Obrázek 5**: vytvoření vazby popisku `Text` vlastnost, která má `UnitPrice` datové pole ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>Formátování cenu jako měny

Pomocí tohoto přidání ovládací prvek popisek webu ceny a inventáře TemplateField nyní zobrazí pouze ceny pro vybrané produktu. Obrázek 6 zobrazuje snímek obrazovky naše průběh doposud, když zobrazit pomocí prohlížeče.


[![Ceny a inventáře TemplateField ukazuje za cenu.](using-templatefields-in-the-detailsview-control-vb/_static/image17.png)](using-templatefields-in-the-detailsview-control-vb/_static/image16.png)

**Obrázek 6**: cenové a inventáře TemplateField zobrazí cenu ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image18.png))


Všimněte si, že není ceny produktu formátu měny. S BoundField, formátování je možné nastavením `HtmlEncode` vlastnost `False` a `DataFormatString` vlastnost `{0:formatSpecifier}`. Pro TemplateField ale žádné formátování pokyny musí být zadané syntaxe vazby dat nebo prostřednictvím metody pro formátování definován někde v kódu aplikace (například v třídy kódu stránky ASP.NET).

K určení, formátování, které se používá v ovládacím prvku popisek webové Syntaxe datové vazby, vrátí dialogové okno datové vazby kliknutím na odkaz Upravit datové vazby ze jmenovky inteligentní značky. Můžete zadat pokyny k formátování přímo v rozevíracím seznamu formát nebo vyberte jednu z řetězce definované formátu. Upozorňujeme BoundField `DataFormatString` vlastnost, formátování, které je zadán pomocí `{0:formatSpecifier}`.

Pro `UnitPrice` pole použití formátování měny zadat tak, že vyberete hodnotu odpovídající rozevíracím seznamu nebo zadáním `{0:C}` ručně.


[![Formátuje cenu jako měny](using-templatefields-in-the-detailsview-control-vb/_static/image20.png)](using-templatefields-in-the-detailsview-control-vb/_static/image19.png)

**Obrázek 7**: formátu cena jako měny ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image21.png))


Deklarativně, formátování specifikace uvedené jako druhý parametr do `Bind` nebo `Eval` metody. Nastavení právě provedená prostřednictvím návrháře výsledků v následujícím výrazu datové vazby v deklarativní:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Přidání do TemplateField zbývající datová pole

V tuto chvíli jsme zobrazí a formátu `UnitPrice` data pole v cenové a TemplateField inventáře, ale stále je třeba zobrazit `UnitsInStock` a `UnitsOnOrder` pole. Umožňuje zobrazit tyto na řádek nižší než cena a v závorkách. Z úpravy rozhraní šablona v Návrháři takové značek přidáte tak, že umístění kurzoru v rámci šablony a jednoduše zadejte text, který se zobrazí. Tento kód Alternativně lze zadat přímo v deklarativní syntaxi.

Přidejte statické značek, ovládací prvky webového popisek a syntaxe vazby dat tak, že cenové a inventáře TemplateField zobrazuje informace o ceny a inventáře takto:

*UnitPrice*  
(**Stock vstupně v pořadí:** *JednotkyNaSkladě* / *ObjednánoJednotek*)

Po provedení této úlohy vaší DetailsView deklarativní by měl vypadat takto:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample3.aspx)]

Tyto změny jsme jste konsolidovat informace o ceny a inventáře do jednoho řádku DetailsView.


[![Ceny a informace o inventáři se zobrazí v jeden řádek](using-templatefields-in-the-detailsview-control-vb/_static/image23.png)](using-templatefields-in-the-detailsview-control-vb/_static/image22.png)

**Obrázek 8**: ceny a informace o inventáři se zobrazí v jeden řádek ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>Krok 3: Přizpůsobení informace – starší formáty pole

`Products` Tabulky `Discontinued` sloupec je bitová hodnota, která určuje, zda již není k dispozici produktu. Při vytváření vazby DetailsView (nebo GridView) pro ovládací prvek zdroje dat, pole logickou hodnotu, jako jsou `Discontinued`, jsou implementované jako CheckBoxFields, zatímco pole hodnot logickou hodnotu, jako například `ProductID`, `ProductName`a tak dále jsou implementované jako BoundFields. Třída CheckBoxField vykreslí jako zakázané zaškrtávací políčko je zaškrtnuto, pokud je datové pole hodnota je True a nezaškrtnuto jinak.

Místo zobrazení třídy CheckBoxField chceme místo toho zobrazí text označující, zda produktu se zastaví. K tomu můžeme může vlastnost CheckBoxField odebrání DetailsView a poté přidejte BoundField jejichž `DataField` byla nastavena na `Discontinued`. Uděláte to chvíli trvat. Po této změně DetailsView zobrazí text "True" pro – starší formáty produkty a "Nepravda" pro produkty, které jsou stále aktivní.


[![Řetězce hodnotu True a False slouží k zobrazení stavu – starší formáty](using-templatefields-in-the-detailsview-control-vb/_static/image26.png)](using-templatefields-in-the-detailsview-control-vb/_static/image25.png)

**Obrázek 9**: řetězce True a False se používají k zobrazení stavu Nákup ukončen ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image27.png))


Představte si, že jsme nechtěli řetězce "True" nebo "Nepravda", "Ano" a "Ne", místo toho použít. Takové přizpůsobení lze provést pomocí TemplateField a metody pro formátování. Metody pro formátování využít libovolný počet vstupních parametrů, ale musí vracet HTML (jako řetězec) se vložit do šablony.

Přidejte metodu k formátování `DetailsViewTemplateField.aspx` třídy kódu stránky s názvem `DisplayDiscontinuedAsYESorNO` který přijme `Northwind.ProductsRow` objektu jako vstupní parametr a vrátí řetězec. Jak je popsáno v předchozí kurzu, tato metoda *musí* označit jako `Protected` nebo `Public` Chcete-li být přístupné ze šablony.


[!code-vb[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample4.vb)]

Tato metoda zkontroluje vstupní parametr (`discontinued`) a vrátí "Ano", pokud je `True`, jinak je "Ne".

> [!NOTE]
> V metodě formátování v předchozí odvolání kurz, který jsme byly předávání v datové pole, která může obsahovat `NULL` s a je proto potřeba zkontrolujte, zda zaměstnance `HiredDate` hodnotu vlastnosti měla databázi `NULL` hodnotu před přístup k `EmployeesRow`na `HiredDate` vlastnost. Tuto kontrolu není zde nutné od `Discontinued` sloupec může mít nikdy databáze `NULL` přiřazených hodnot. Kromě toho z tohoto důvodu metodu může přijmout logickou hodnotu vstupní parametr, místo aby se tak, aby přijímal `ProductsRow` instance nebo parametr typu `Object`.


Pomocí této formátování metody dokončení zbývá volat z TemplateField `ItemTemplate`. Chcete-li vytvořit TemplateField buď odstranit `Discontinued` BoundField a přidat nové TemplateField nebo převést `Discontinued` BoundField do TemplateField. Potom z pohledu deklarativní upravit TemplateField tak, aby obsahoval pouze ItemTemplate, která volá `DisplayDiscontinuedAsYESorNO` metodu předáním v hodnotě aktuální `ProductRow` instance `Discontinued` vlastnost. To je přístupný prostřednictvím `Eval` metoda. Konkrétně TemplateField značek by měl vypadat podobně jako:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample5.aspx)]

To způsobí, že `DisplayDiscontinuedAsYESorNO` metoda má být vyvolána při vykreslování DetailsView, předávání v `ProductRow` instance `Discontinued` hodnotu. Vzhledem k tomu `Eval` metoda vrátí hodnotu typu `Object`, ale `DisplayDiscontinuedAsYESorNO` metoda očekává vstupní parametr typu `Boolean`, jsme přetypovat `Eval` metody vrátit hodnotu `Boolean`. `DisplayDiscontinuedAsYESorNO` Metoda pak vrátí hodnotu "Ano" nebo "Ne" přijme v závislosti na hodnotě. Vrácená hodnota je určeno v této DetailsView řádek (viz obrázek 10).


[![Ano nebo ne hodnoty se nyní zobrazí v řádku Nákup ukončen](using-templatefields-in-the-detailsview-control-vb/_static/image29.png)](using-templatefields-in-the-detailsview-control-vb/_static/image28.png)

**Obrázek 10**: Ano nebo ne hodnoty se nyní zobrazí v řádku Nákup ukončen ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-detailsview-control-vb/_static/image30.png))


## <a name="summary"></a>Souhrn

TemplateField v ovládacím prvku DetailsView umožňuje vyšší stupeň flexibilitu při zobrazení dat, než je k dispozici s dalšími kontrolami pole a jsou ideální pro situace, kde:

- Více datových polí je nutné na jeden sloupec GridView
- Data se nejlépe vyjádřit pomocí ovládacího prvku webového spíše než prostý text
- Výstup závisí na základní data, například zobrazení metadat nebo v přeformátování data

Při TemplateFields povolit pro větší flexibilitu při vykreslování dat prvku DetailsView základní úroveň, výstup DetailsView stále funguje trochu boxy jako každé pole je vykreslen jako řádek v kódu HTML `<table>`.

Ovládací prvek FormView nabízí vyšší stupeň flexibilitu při konfiguraci Vykreslený výstup. FormView neobsahuje pole, ale spíš jenom řadu šablon (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`a tak dále). Postup použití FormView k dosažení ještě větší kontrolu nad vykreslené rozložení v našem kurzu další uvidíme.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Dana Jagers. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](using-templatefields-in-the-gridview-control-vb.md)
[další](using-the-formview-s-templates-vb.md)
