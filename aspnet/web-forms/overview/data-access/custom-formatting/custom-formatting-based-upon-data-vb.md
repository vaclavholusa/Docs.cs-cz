---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: Vlastní formátování založené na datech (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Úprava formát ovládacího prvku GridView, DetailsView nebo FormView na základě něj navázaná data lze provést několika různými způsoby. V tomto kurzu jsme vám l...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: d55a9ece22805d7f5d248e81a01d059dbde0ee18
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809586"
---
<a name="custom-formatting-based-upon-data-vb"></a>Vlastní formátování založené na datech (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe) nebo [stahovat PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> Úprava formát ovládacího prvku GridView, DetailsView nebo FormView na základě něj navázaná data lze provést několika různými způsoby. V tomto kurzu podíváme na tom, jak provádět data vázaná formátování prostřednictvím datové vazby a RowDataBound obslužné rutiny událostí.


## <a name="introduction"></a>Úvod

Vzhled ovládacích prvků ovládacího prvku GridView, DetailsView a FormView je možné přizpůsobit pomocí velkého počtu vlastnosti stylu. Vlastnosti, jako je `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, a `Height`, mimo jiné, určovat celkový vzhled ovládacího prvku vykresleného. Vlastnosti včetně `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, a ostatní povolit nastavení stylu uplatňovat na konkrétní části. Nastavení stylu, lze použít na úrovni pole.

V mnoha scénářích však formátování požadavky závisí na hodnotě zobrazená data. K přitažení pozornosti ke z zásob produktů, třeba sestavy obsahující informace o produktu může nastavit barvu pozadí na žlutou pro tyto produkty, jejichž `UnitsInStock` a `UnitsOnOrder` pole se rovná 0. Chcete-li zvýraznit dražší, produkty, můžeme zobrazit ceny pro tyto produkty ocenění více než 75.00 $ tučným písmem.

Úprava formát ovládacího prvku GridView, DetailsView nebo FormView na základě něj navázaná data lze provést několika různými způsoby. V tomto kurzu podíváme na tom, jak provádět data vázaná formátování prostřednictvím `DataBound` a `RowDataBound` obslužných rutin událostí. V dalším kurzu se podíváme alternativním přístupem.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Použití ovládacího prvku DetailsView`DataBound`obslužné rutiny události

Když data svázaná s DetailsView, z ovládacího prvku zdroje dat nebo prostřednictvím kódu programu přiřazená dat na ovládací prvek `DataSource` vlastnosti a volání jeho `DataBind()` dojít k následující sled kroků metody:

1. Data webový ovládací prvek `DataBinding` dojde k aktivaci události.
2. Data je vázán na data webový ovládací prvek.
3. Data webový ovládací prvek `DataBound` dojde k aktivaci události.

Vlastní logiky můžete vložený okamžitě po provedení kroků 1 a 3 prostřednictvím obslužné rutiny události. Vytvořením obslužná rutina události `DataBound` události můžeme programově určit data, která byla vázaná na ovládací prvek webových dat a formátování podle potřeby upravit. Pro znázornění Pojďme vytvořit DetailsView, zobrazí se seznam obecných informací o produktu, která se zobrazí `UnitPrice` hodnota v ***písmo tučné písmo, kurzívu*** při překročení 75.00 $.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Krok 1: Zobrazení informací o produktu v DetailsView

Otevřít `CustomColors.aspx` stránku `CustomFormatting` složky, přetáhněte ovládací prvek DetailsView z panelu nástrojů na Návrhář, nastavte jeho `ID` hodnoty vlastnosti `ExpensiveProductsPriceInBoldItalic`a jeho vazbu na nový ovládací prvek ObjectDataSource, která volá `ProductsBLL` třídy `GetProducts()` metody. Podrobné pokyny k provádění to jsou tady jsme ji vynechali pro zkrácení vzhledem k tomu, že nám prozkoumat podrobněji v předchozích kurzech.

Když prvku ObjectDataSource vázaný na ovládacím prvku DetailsView, věnujte chvíli upravit seznam polí. Jste se rozhodli odebrat `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, a `Discontinued` BoundFields přejmenovat a zbývající BoundFields přeformátovali. Jsem také odstraněné `Width` a `Height` nastavení. Protože ovládacím prvku DetailsView zobrazí jenom jeden záznam, musíme povolit stránkování, aby koncový uživatel Chcete-li zobrazit všechny produkty. To tak, že zaškrtnete políčko Povolit stránkování v ovládacím prvku DetailsView inteligentních značek.


[![Obrázek 1: Zaškrtněte políčko Povolit stránkování v ovládacím prvku DetailsView inteligentních značek](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**Obrázek 1**: obrázek 1: Zaškrtněte políčko Povolit stránkování v ovládacím prvku DetailsView inteligentních značek ([kliknutím ji zobrazíte obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image3.png))


Po provedení těchto změn bude DetailsView značky:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

Využijte k otestování této stránky v prohlížeči.


[![Ovládací prvek DetailsView zobrazí jeden produkt v čase](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**Obrázek 2**: prvku DetailsView. ovládací prvek zobrazí jeden produkt najednou ([kliknutím ji zobrazíte obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Krok 2: Prostřednictvím kódu programu určující hodnotu Data v obslužné rutině události datové vazby

Pokud chcete zobrazit ceny pro tyto produkty písmeny tučné písmo, kurzívu jehož `UnitPrice` hodnota přesahuje $75.00, musíme nejprve moct určení prostřednictvím kódu programu `UnitPrice` hodnotu. Pro ovládacím prvku DetailsView. To lze provést v `DataBound` obslužné rutiny události. Vytvořit událost obslužné rutiny kliknete na ovládacím prvku DetailsView. v Návrháři pak přejděte do okna Vlastnosti. Stisknutím klávesy F4 zobrazíte ji, pokud není viditelný, nebo přejděte do nabídky zobrazení a vyberte možnost nabídky okna Vlastnosti. V okně Vlastnosti klikněte na na ikonu blesku na seznamu událostí v ovládacím prvku DetailsView. V dalším kroku buď poklepejte `DataBound` události nebo zadejte název obslužné rutiny události, kterou chcete vytvořit.


![Vytvořte obslužnou rutinu události pro událost datové vazby](custom-formatting-based-upon-data-vb/_static/image7.png)

**Obrázek 3**: vytvořit obslužná rutina události `DataBound` událostí


> [!NOTE]
> Můžete také vytvořit obslužnou rutinu události z část kódu stránky ASP.NET. Najdete zde dva rozevírací seznamy v horní části stránky. Vyberte objekt v levém seznamu rozevíracího seznamu a události, kterou chcete vytvořit obslužnou rutinu pro přímo rozevíracího seznamu a sady Visual Studio vytvoří automaticky obslužnou rutinu události.


Tím se automaticky vytvořit obslužnou rutinu události a můžete přejít k části kódu ve kterém byl přidán. V tomto okamžiku se zobrazí:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

Data vázaná na ovládacím prvku DetailsView je přístupná prostřednictvím `DataItem` vlastnost. Připomínáme, že jsme naše ovládací prvky vazbu s DataTable silného typu, který se skládá z kolekce instancí DataRow silného typu. Pokud objekt DataTable je vázán na ovládacím prvku DetailsView, první řádek dat v objektu DataTable je přiřazen do ovládacího prvku DetailsView `DataItem` vlastnost. Konkrétně `DataItem` je vlastnosti přiřazen `DataRowView` objektu. Můžeme použít `DataRowView`společnosti `Row` vlastnost k získání přístupu k podkladový objekt DataRow, který je ve skutečnosti `ProductsRow` instance. Když máme takovou `ProductsRow` instance bychom mohli naše rozhodnutí jednoduše zkontrolováním hodnoty vlastností objektu.

Následující kód ukazuje, jak určit, zda `UnitPrice` je vázán na ovládacím prvku DetailsView hodnota větší než $75.00:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> Protože `UnitPrice` může mít `NULL` hodnota v databázi, doporučujeme nejdřív zkontrolujte, abyste měli jistotu, že jsme se zabývají `NULL` hodnotu před přístupem k `ProductsRow`společnosti `UnitPrice` vlastnost. Tato kontrola je důležité protože jsme pokusu o přístup k `UnitPrice` vlastnosti, když má `NULL` hodnotu `ProductsRow` vyvolá objekt [strongtypingexception – výjimka](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Krok 3: Formátování hodnoty UnitPrice v ovládacím prvku DetailsView.

V tuto chvíli můžeme určit, zda `UnitPrice` hodnotu hranice do ovládacího prvku DetailsView má hodnotu, která překračuje $75.00, ale My jsme dosud se dozvíte, jak programově upravit ovládacím prvku DetailsView formátování odpovídajícím způsobem. Pokud chcete upravit, formátování v ovládacím prvku DetailsView celý řádek, programový přístup k řádku pomocí `DetailsViewID.Rows(index)`; Chcete-li upravit konkrétní buňky, přístup k použití `DetailsViewID.Rows(index).Cells(index)`. Jakmile budeme mít odkaz na řádek nebo buňku jsme pak můžete upravit jeho vzhled nastavením jeho vlastností stylu.

Programově přístup k řádku je nutné znát indexu na řádek, který začíná hodnotou 0. `UnitPrice` Řádek je pátý řádek v ovládacím prvku DetailsView předá index 4 a zpřístupňování prostřednictvím kódu programu pomocí `ExpensiveProductsPriceInBoldItalic.Rows(4)`. V tuto chvíli jsme může mít celý řádek obsah zobrazit v písmo tučné písmo, kurzívu, můžete pomocí následujícího kódu:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

Nicméně díky tomu budou *obě* popisek (cena) a hodnota tučné písmo a kurzíva. Pokud Chceme mít pouze hodnotu tučné písmo a kurzíva musíme použít formátování na druhý buňky v řádku, což lze provést pomocí následujícího:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

Protože našich kurzů pro jaký jste použili šablony stylů udržovat čisté oddělení mezi vykreslované značky a informace týkající se stylu, místo nastavení vlastnosti konkrétního stylu, jak je znázorněno výše můžeme místo toho použijte třídu šablony stylů CSS. Otevřít `Styles.css` šablony stylů a přidejte novou třídu šablony stylů CSS s názvem `ExpensivePriceEmphasis` s následující definice:


[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

Potom v `DataBound` obslužná rutina události, nastavte na buňku `CssClass` vlastnost `ExpensivePriceEmphasis`. Následující kód ukazuje `DataBound` obslužné rutiny události v celém rozsahu:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

Při prohlížení Chai, která stojí méně než 75.00 $, zobrazí se cena uvedená normálním písmem (viz obrázek 4). Ale při prohlížení Niku Kobe Mishi, jehož cena $97.00 cena se zobrazí v písmo tučné písmo, kurzívu (viz obrázek 5).


[![Ceny za méně než $75.00 jsou zobrazeny v normální písmo](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**Obrázek 4**: ceny za méně než $75.00 jsou zobrazeny v normální písmo ([kliknutím ji zobrazíte obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image10.png))


[![Ceny nákladné produkty, které se zobrazují v tučné, kurzíva písma](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**Obrázek 5**: ceny nákladné produkty, které se zobrazují v tučné, kurzíva písma ([kliknutím ji zobrazíte obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>Použití ovládacího prvku FormView`DataBound`obslužné rutiny události

Postup určení podkladová data vázaná FormView jsou stejné jako pro vytvoření DetailsView `DataBound` obslužná rutina události, přetypování `DataItem` vázány na ovládací prvek vlastnosti pro příslušný typ objektu a zjistit, jak pokračovat dál. FormView a DetailsView liší, ale v způsob, jakým aktualizuje vzhled uživatelského rozhraní.

FormView neobsahuje žádné BoundFields a proto nemá `Rows` kolekce. Místo toho FormView se skládá z šablon, které může obsahovat kombinaci statický kód HTML, webové ovládací prvky a datové vazby syntaxe. Úprava stylu FormView obvykle zahrnuje nastavení stylu jednoho nebo víc webových ovládacích prvků v rámci šablony ovládacího prvku FormView.

Pro znázornění, Pojďme FormView seznam produktů, jako je v předchozím příkladu, ale umožňuje použití zobrazení pouze název produktu a jednotky v zásobách s jednotkami v zásobách zobrazí červený písmem v případě, že je menší než nebo rovno 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Krok 4: Zobrazení informací o produktu v FormView

Přidat FormView k `CustomColors.aspx` stránce pod DetailsView a nastavte jeho `ID` vlastnost `LowStockedProductsInRed`. FormView svázat s ovládacím prvkem ObjectDataSource vytvořili v předchozím kroku. Tím se vytvoří `ItemTemplate`, `EditItemTemplate`, a `InsertItemTemplate` pro FormView. Odeberte `EditItemTemplate` a `InsertItemTemplate` a zjednodušit `ItemTemplate` mají zahrnout jenom `ProductName` a `UnitsInStock` hodnoty, každá do své vlastní ovládací prvky popisek příslušně pojmenovaných. Stejně jako u prvku DetailsView z předchozího příkladu, také zaškrtnutím políčka Povolit stránkování ve třídě FormView inteligentních značek.

Po tyto úpravy vašeho ovládacího prvku FormView značek by měl vypadat nějak takto:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

Všimněte si, `ItemTemplate` obsahuje:

- **Statický kód HTML** text "Product:" a "jednotek v zásobách:" spolu s `<br />` a `<b>` elementy.
- **Ovládací prvky webových** dva ovládací prvky Label `ProductNameLabel` a `UnitsInStockLabel`.
- **Datová vazba syntaxe** `<%# Bind("ProductName") %>` a `<%# Bind("UnitsInStock") %>` syntaxe, která přiřadí hodnoty z těchto polí k ovládacím prvkům popisku `Text` vlastnosti.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Krok 5: Prostřednictvím kódu programu určující hodnotu Data v obslužné rutině události datové vazby

Pomocí značky ovládacího prvku FormView kompletní, dalším krokem je prostřednictvím kódu programu určete, jestli `UnitsInStock` hodnota je menší než nebo rovno 10. Toho dosahuje přesně stejným způsobem s FormView jako byl s ovládacím prvku DetailsView. Začněte vytvořením obslužnou rutinu události pro ovládacího prvku FormView `DataBound` událostí.


![Vytvořte obslužnou rutinu události datové vazby](custom-formatting-based-upon-data-vb/_static/image14.png)

**Obrázek 6**: vytvořit `DataBound` obslužné rutiny události


Události přetypování obslužná rutina ovládacího prvku FormView `DataItem` vlastnost `ProductsRow` instance a určit, jestli `UnitsInPrice` hodnotu tak, že musíme zobrazení red písmem.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Krok 6: Formátování ovládacího prvku popisku UnitsInStockLabel ve třídě FormView ItemTemplate

Posledním krokem je pro formátování zobrazených `UnitsInStock` hodnotu red písmeny, pokud hodnota je 10 nebo méně. K tomu potřebujeme k programovému přístupu ke službě `UnitsInStockLabel` v ovládacím prvku `ItemTemplate` a nastavit jeho vlastnosti stylu tak, aby jeho textu se zobrazí červeně. Chcete-li získat přístup k webové řízení v šabloně, použijte `FindControl("controlID")` metoda takto:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

V našem příkladu chceme, aby pro přístup k popisku ovládací prvek, jehož `ID` hodnotu `UnitsInStockLabel`, takže bychom použili:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

Jakmile budeme mít programový odkaz na ovládací prvek, jsme jde upravit její vlastnosti stylu podle potřeby. Jako u předchozího příkladu, jsem vytvořil třídu šablony stylů CSS v `Styles.css` s názvem `LowUnitsInStockEmphasis`. Stylu na ovládacím prvku popisek Web, nastavte jeho `CssClass` vlastnost odpovídajícím způsobem.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> Syntaxe pro formátování šablonu programově přístup k ovládacímu prvku pomocí webové `FindControl("controlID")` a nastavením jeho vlastnosti související se stylem lze také při použití [vlastností TemplateField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) v prvku DetailsView nebo GridView ovládací prvky. Prozkoumáme vlastností TemplateField v následujícím kurzem.


Obrázky 7 znázorňuje FormView při prohlížení produktu jehož `UnitsInStock` hodnota je větší než 10, zatímco v produktu na obrázku 8 je jeho hodnota menší než 10.


[![Pro produkty s dostatečně velké jednotky v zásobách ne vlastní formátování](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**Obrázek 7**: se použije pro produkty s dostatečně velké jednotky v zásobách, ne vlastní formátování ([kliknutím ji zobrazíte obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image17.png))


[![Jednotky v zásobách číslo se zobrazí červeně pro tyto produkty s hodnoty 10 nebo méně](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**Obrázek 8**: The jednotky v zásobách číslo se zobrazí červeně pro tyto produkty s hodnoty 10 nebo méně ([kliknutím ji zobrazíte obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Formátování pomocí prvku GridView`RowDataBound`událostí

Dříve jsme se zaměřili na posloupnost kroků ovládacím prvku DetailsView. a FormView řídí průběh prostřednictvím během datové vazby. Pojďme se podívat přes tyto kroky ještě jednou jako aktualizačního programu.

1. Data webový ovládací prvek `DataBinding` dojde k aktivaci události.
2. Data je vázán na data webový ovládací prvek.
3. Data webový ovládací prvek `DataBound` dojde k aktivaci události.

Těchto třech jednoduchých krocích jsou dostačující pro DetailsView a FormView, protože se zobrazí jenom jeden záznam. Pro prvku GridView, který zobrazuje *všechny* záznamy vázán k němu (nikoli pouze první), je trochu složitější kroku 2.

V kroku 2 prvku GridView zobrazí zdroj dat a pro každý záznam, vytvoří `GridViewRow` instance a vazba k aktuální záznam. Pro každou `GridViewRow` přidány do prvku GridView, dvě události jsou vyvolány:

- **`RowCreated`** Aktivuje se po `GridViewRow` se vytvořil
- **`RowDataBound`** Aktivuje se po aktuální záznam byl vázán `GridViewRow`.

Pro prvku GridView potom datová vazba je přesněji popsán následující sled kroků:

1. Prvku GridView `DataBinding` dojde k aktivaci události.
2. Data je vázán na prvku GridView.   
  
   Pro každý záznam ve zdroji dat. 

    1. Vytvoření `GridViewRow` objektu
    2. Oheň `RowCreated` událostí
    3. Záznam, který chcete vytvořit vazbu `GridViewRow`
    4. Oheň `RowDataBound` událostí
    5. Přidat `GridViewRow` k `Rows` kolekce
3. Prvku GridView `DataBound` dojde k aktivaci události.

Chcete-li přizpůsobit formát jednotlivých záznamů prvku GridView, pak potřebujeme vytvořit obslužná rutina události `RowDataBound` událostí. Pro znázornění, přidáme GridView k `CustomColors.aspx` stránka, která obsahuje název, kategorie a cen pro jednotlivé produkty, zvýraznění produkty, jejichž cena je nižší než 10,00 USD barvou žlutým pozadím.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Krok 7: Zobrazení informací o produktu v GridView

Přidejte prvek GridView ve třídě FormView z předchozího příkladu a nastavte jeho `ID` vlastnost `HighlightCheapProducts`. Vzhledem k tomu, že už máme ObjectDataSource, který vrátí všechny produkty na stránce, vytvořit vazbu s, který v prvku GridView. Nakonec upravte prvku GridView BoundFields právě produktů názvy, kategorie a ceny. Za tyto úpravy prvku GridView značek by měl vypadat jako:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

Obrázek 9 ukazuje náš postup do této chvíle při prohlížení prostřednictvím prohlížeče.


[![Název, kategorie a ceny pro každý produkt obsahuje seznam prvku GridView.](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**Obrázek 9**: prvku GridView uvádí název, kategorie a ceny pro každý produkt ([kliknutím ji zobrazíte obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Krok 8: Prostřednictvím kódu programu určující hodnotu Data v obslužné rutině události RowDataBound

Když `ProductsDataTable` je vázán na prvku GridView. jeho `ProductsRow` instance jsou výčtové a pro každou `ProductsRow` `GridViewRow` se vytvoří. `GridViewRow`Společnosti `DataItem` vlastnost je přiřazena zvláštní `ProductRow`, po jejímž uplynutí prvku GridView `RowDataBound` je vyvolána obslužná rutina události. Chcete-li zjistit `UnitPrice` hodnotu pro každý produkt vázán na prvku GridView a pak je nutné vytvořit obslužnou rutinu události pro prvku GridView `RowDataBound` událostí. V této obslužné rutiny události jsme mohli prohlédnout `UnitPrice` hodnotu pro aktuální `GridViewRow` a formátování rozhodnutí pro tento řádek.

Tato obslužná rutina události je možné vytvořit pomocí stejného postupu jako FormView a prvku DetailsView.


![Vytvořte obslužnou rutinu události pro událost RowDataBound prvku GridView.](custom-formatting-based-upon-data-vb/_static/image24.png)

**Obrázek 10**: vytvořit obslužnou rutinu události pro prvku GridView `RowDataBound` událostí


Vytváření obslužnou rutinu události tímto způsobem způsobí, že následující kód, který automaticky přidá do části kódu stránky ASP.NET:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

Když `RowDataBound` událost je aktivována, obslužná rutina události je předána jako druhý parametr objektu typu `GridViewRowEventArgs`, který má vlastnost s názvem `Row`. Tato vlastnost vrátí odkaz na `GridViewRow` , který byl pouze data vázaná. Pro přístup `ProductsRow` instance je vázán na `GridViewRow` používáme `DataItem` vlastnost takto:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

Při práci s `RowDataBound` obslužná rutina události je důležité mít na paměti, že prvku GridView se skládá z různých typů řádků a že tato událost se aktivuje například pro *všechny* řádek typy. A `GridViewRow`od typu se dají určit pomocí jeho `RowType` vlastnost a může mít jednu z možných hodnot:

- `DataRow` řádek, který je vázán na záznam z prvku GridView. `DataSource`
- `EmptyDataRow` řádek zobrazí v případě prvku GridView `DataSource` nic neobsahuje
- `Footer` řádek zápatí. Pokud je vidět prvku GridView `ShowFooter` je nastavena na `True`
- `Header` řádek záhlaví; Zobrazí-li prvku GridView ShowHeader je nastavena na `True` (výchozí)
- `Pager` prvku GridView, které implementují, stránkování, řádek, který zobrazí rozhraní stránkování
- `Separator` není možné použít u prvku GridView, ale používá `RowType` dvěma datovými webových ovládacích prvcích probereme v budoucích kurzech ovládací prvky vlastnosti ovládacích prvků DataList a Repeater

Protože `EmptyDataRow`, `Header`, `Footer`, a `Pager` řádků nejsou přidružené k `DataSource` záznam, že bude mít vždy hodnotu `Nothing` pro jejich `DataItem` vlastnost. Z tohoto důvodu, než se pokusíte o práci s aktuální `GridViewRow`společnosti `DataItem` vlastnost, nejprve musíte zajišťujeme, že jsme pracujete s `DataRow`. Toho můžete docílit tak, že zkontrolujete `GridViewRow`společnosti `RowType` vlastnost takto:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Krok 9: Zvýraznění žlutý při the UnitPrice hodnota řádku je menší než 10,00 USD

Posledním krokem je prostřednictvím kódu programu zvýraznit celý `GridViewRow` Pokud `UnitPrice` hodnota řádku je menší než 10,00 USD. Syntaxe pro přístup k prvku GridView řádků nebo buňky, které je stejná jako u ovládacím prvku DetailsView. `GridViewID.Rows(index)` pro přístup k celým řádkem, `GridViewID.Rows(index).Cells(index)` pro přístup k určité buňky. Nicméně, když `RowDataBound` obslužná rutina události je vyvoláno vázaných dat `GridViewRow` ještě musí být přidán do prvku GridView `Rows` kolekce. Proto nemůže přistupovat k aktuální `GridViewRow` z instance `RowDataBound` obslužné rutiny události pomocí kolekce řádků.

Místo `GridViewID.Rows(index)`, nám může odkazovat na aktuální `GridViewRow` instance v `RowDataBound` pomocí obslužné rutiny události `e.Row`. To znamená, v pořadí zvýrazněte aktuálního `GridViewRow` z instance `RowDataBound` bychom použili obslužné rutiny události:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

Místo nastavení `GridViewRow`společnosti `BackColor` vlastnost přímo, můžeme zůstat u pomocí tříd šablon stylů CSS. Vytvořil jsem třídu šablony stylů CSS s názvem `AffordablePriceEmphasis` , který nastavuje barvu pozadí na žlutou. Dokončené `RowDataBound` následuje obslužné rutiny události:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]


[![Největší dostupnou produkty jsou zvýrazněn žlutou](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**Obrázek 11**: jsou cenově dostupné produkty v většinu zvýrazněn žlutou ([kliknutím ji zobrazíte obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image27.png))


## <a name="summary"></a>Souhrn

V tomto kurzu jsme viděli, jak formátovat ovládacího prvku GridView, DetailsView a podle data vázaná na ovládacím prvku FormView. K tomu jsme vytvořili obslužnou rutinu události pro `DataBound` nebo `RowDataBound` událostí, kde byla podkladová data prozkoumat spolu s změny formátování, v případě potřeby. Pro přístup k datům vázán k prvku DetailsView nebo FormView, používáme `DataItem` vlastnost v `DataBound` obslužné rutiny události; prvku GridView, každý `GridViewRow` instance `DataItem` vlastnost obsahuje data vázaná na daném řádku, který je k dispozici v `RowDataBound` obslužné rutiny události.

Syntaxe pro programově úpravu formátování ovládacího prvku webových dat závisí na ovládací prvek a jak se zobrazí data, která mají být ve formátu. Prvek DetailsView a GridView ovládací prvky, řádky a buňky je přístupný podle pořadového čísla indexu. Pro třídy FormView, který používá šablony, `FindControl("controlID")` metoda je běžně používaná k nalezení webové ovládacího prvku z v rámci šablony.

V dalším kurzu podíváme na použití šablony s ovládacími prvky GridView a prvku DetailsView. Kromě toho ukážeme Další možností, jak vlastní formátování založené na podkladová data.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly E.R. Gilmore, Dennis Patterson a Jagers daň. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [další](using-templatefields-in-the-gridview-control-vb.md)
