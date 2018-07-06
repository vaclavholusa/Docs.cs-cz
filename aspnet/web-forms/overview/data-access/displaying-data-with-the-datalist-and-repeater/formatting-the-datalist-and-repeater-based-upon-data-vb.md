---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
title: Formátování ovládacích prvků DataList a Repeater na základě dat (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu jsme projdete kroky příklady, jak jsme formát vzhled ovládacích prvků DataList a Repeater ovládací prvky, buď pomocí funkce formátování se...
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: e2f401ae-37bb-4b19-aa97-d6b385d40f88
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 428438b2bae062c09d13c002f4729c3c394975a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807707"
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-vb"></a>Formátování ovládacích prvků DataList a Repeater na základě dat (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_VB.exe) nebo [stahovat PDF](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/datatutorial30vb1.pdf)

> V tomto kurzu jsme projdete kroky příklady, jak jsme formát vzhled ovládacích prvků DataList a Repeater ovládací prvky, buď s použitím formátování funkcí v rámci šablony nebo zpracování událostí datové vazby.


## <a name="introduction"></a>Úvod

Jak jsme viděli v předchozím kurzu, prvku DataList nabízí celou řadu vlastnosti stylu, které ovlivňují její vzhled. Konkrétně jsme viděli, jak chcete přiřadit výchozí šablony stylů CSS třídy DataList s `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, a `SelectedItemStyle` vlastnosti. Kromě tyto čtyři vlastnosti prvku DataList zahrnuje celou řadu dalších vlastností stylu například `Font`, `ForeColor`, `BackColor`, a `BorderWidth`, pár. Ovládací prvek Repeater neobsahuje žádné vlastnosti stylu. Tato nastavení stylu se musí provádět přímo v kódu v šablonách opakovače s.

Často ale formátování dat závisí na vlastní data. Například při výpisu produkty budeme chtít zobrazení informací o produktu v barvu písma světle šedá, pokud je přerušeno nebo nám chcete zvýraznit `UnitsInStock` hodnotu, pokud je nula. Jak jsme viděli v předchozích kurzech, ovládacího prvku GridView, DetailsView a FormView nabízí dva různé způsoby, jak formátovat jejich vzhled na základě svých dat:

- **`DataBound` Události** vytvořit obslužnou rutinu události pro příslušné `DataBound` událost, která aktivuje se po data byla svázána se jednotlivé položky (pro prvek GridView byla `RowDataBound` události; u ovládacích prvků DataList a Repeater je`ItemDataBound`událostí). V takovém případě se dají prozkoumat obslužnou rutinu, pouze data vázaná a formátování rozhodnutí provedli. Jsme se zaměřili na tuto techniku ve [vlastní formátování založené na Data](../custom-formatting/custom-formatting-based-upon-data-vb.md) kurzu.
- **Formátování funkcí v šablonách** při použití vlastností TemplateField v prvku DetailsView nebo GridView ovládacích prvků, nebo v šabloně v ovládacím prvku FormView, můžeme přidat do třídy modelu code-behind stránky s ASP.NET, vrstvy obchodní logiky nebo některý formátování – funkce Další knihovny tříd, který je přístupný z webové aplikace. Tato funkce formátování může přijmout libovolný počet vstupních parametrů, ale musí vrátit kód HTML pro vykreslení v šabloně. Formátování funkce byly nejprve kontrolován [použití vlastností TemplateField v ovládacím prvku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) kurzu.

Obě tyto metody formátování jsou k dispozici s ovládacími prvky DataList a Repeater. V tomto kurzu jsme projdete kroky příklady použití obě tyto metody pro oba ovládací prvky.

## <a name="using-theitemdataboundevent-handler"></a>Použití`ItemDataBound`obslužné rutiny události

Když data svázaná s a v prvku DataList, z ovládacího prvku zdroje dat nebo prostřednictvím kódu programu přidělením data do ovládacího prvku s `DataSource` vlastnosti a volání jeho `DataBind()` metoda DataList s `DataBinding` aktivuje události, zdroj dat, výčtu, a každý záznam dat je vázán na ovládacím prvku DataList. Pro každý záznam ve zdroji dat, vytvoří prvku DataList [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) objektu, který je pak vázána na aktuální záznam. Během tohoto procesu prvku DataList vyvolává dvě události:

- **`ItemCreated`** Aktivuje se po `DataListItem` se vytvořil
- **`ItemDataBound`** Aktivuje se po aktuální záznam byl vázán `DataListItem`

Následující kroky popisují proces vytváření vazby dat pro ovládací prvek DataList.

1. DataList s [ `DataBinding` události](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) aktivována
2. Data je vázán na ovládacím prvku DataList  
  
   Pro každý záznam ve zdroji dat. 

    1. Vytvoření `DataListItem` objektu
    2. Oheň [ `ItemCreated` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Záznam, který chcete vytvořit vazbu `DataListItem`
    4. Oheň [ `ItemDataBound` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Přidat `DataListItem` k `Items` kolekce

Při vytváření vazby dat k ovládacím prvku opakovače, prochází přesně stejnou posloupnost kroků. Jediným rozdílem je, že místo `DataListItem` vytváří instance, používá opakovače [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Bystří čtenáři možná jste si všimli mírné anomálií mezi posloupnost kroků, které probíhají, když v prvku DataList a Repeater jsou vázány na data a po prvku GridView je vázán na data. Na konci ocáskem proces vytváření vazby dat prvku GridView vyvolá `DataBound` události; ale ovládací prvek DataList ani Repeater mít takovou událost. Je to proto ovládacích prvků DataList a Repeater byly vytvořeny v časovém rámci 1.x technologie ASP.NET, před vzor obslužné rutiny události před instrumentací a po ní úrovně stalo běžné.


Například s použitím prvku GridView, je vytvořit obslužnou rutinu události pro jednu z možností pro formátování na základě dat `ItemDataBound` událostí. Tato obslužná rutina události by kontrolovat, který má právě vázán na data `DataListItem` nebo `RepeaterItem` a ovlivňuje formátování ovládacího prvku podle potřeby.

Pro ovládací prvek DataList změny formátování pro celou položku je možné implementovat pomocí `DataListItem` s stylu vlastnosti, které zahrnují standardní `Font`, `ForeColor`, `BackColor`, `CssClass`, a tak dále. Ovlivňuje formátování konkrétní webové ovládací prvky v rámci šablony ovládacích prvků DataList s potřebujeme pro programově přístup a úpravy stylu těchto ovládacích prvků. Jsme viděli, jak provést tuto znovu v *vlastní formátování založené na Data* kurzu. Jako ovládacím prvku Repeater `RepeaterItem` třída nemá žádné vlastnosti stylu; proto všechny související se stylem změny provedené `RepeaterItem` v `ItemDataBound` obslužné rutiny události musí provést programově přístup k a aktualizace webové ovládací prvky v rámci Šablona.

Vzhledem k tomu, `ItemDataBound` formátování techniku pro prvky DataList a Repeater jsou téměř identické, náš příklad se zaměřuje na pomocí prvku DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Krok 1: Zobrazení informací o produktu v ovládacím prvku DataList

Předtím, než jsme se starat o formátování, umožňují s nejprve vytvořit stránku, která se používá k zobrazení informací o produktu a v prvku DataList. V [předchozí kurz o službě](displaying-data-with-the-datalist-and-repeater-controls-vb.md) jsme vytvořili, a v prvku DataList jehož `ItemTemplate` zobrazí každý produkt s názvem, kategorie, Dodavatel, množství na jednotku a ceny. Umožní s opakujte tuto funkci tady v tomto kurzu. K tomu můžete buď vytvořit znovu ovládacích prvcích DataList a jeho ObjectDataSource úplně od začátku, nebo můžete zkopírovat tyto ovládací prvky na stránce vytvořili v předchozím kurzu (`Basics.aspx`) a vložte je do stránky pro účely tohoto kurzu (`Formatting.aspx`).

Jakmile zreplikovali funkce ovládacích prvků DataList a ObjectDataSource `Basics.aspx` do `Formatting.aspx`, věnujte chvíli změnit DataList s `ID` vlastnost z `DataList1` do více popisné `ItemDataBoundFormattingExample`. V dalším kroku prvku DataList zobrazte v prohlížeči. Jak ukazuje obrázek 1 pouze formátování rozdíl mezi jednotlivé produkty je, že barvu pozadí alternativy.


[![Produkty jsou uvedené v ovládacím prvku DataList](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image1.png)

**Obrázek 1**: The produkty jsou uvedené v ovládacím prvku DataList ([kliknutím ji zobrazíte obrázek v plné velikosti](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image3.png))


Pro účely tohoto kurzu nechte s formátování prvku DataList tak, aby žádné produkty s méně než 20,00 $za cenu budou mít i její název a jednotky cen zvýrazněné žlutou barvou.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Krok 2: Prostřednictvím kódu programu určující hodnotu Data v obslužné rutině události ItemDataBound

Protože pouze tyto produkty s cenou pod 20,00 $za bude mít vlastní formátování použité, jsme musí být schopní určit, každý s cena produktu. Při vytváření vazby dat k a v prvku DataList, prvku DataList zobrazí záznamy ve zdroji dat a pro každý záznam, vytvoří `DataListItem` instance záznam zdroje dat k vytvoření vazby `DataListItem`. Po konkrétní záznam s dat byla svázána se aktuální `DataListItem` objekt DataList s `ItemDataBound` událost se aktivuje. Můžeme vytvořit obslužnou rutinu události pro tuto událost ke kontrole datových hodnot pro aktuální `DataListItem` a na základě těchto hodnot, proveďte změny formátování nezbytné.

Vytvoření `ItemDataBound` události ovládacích prvcích DataList a přidejte následující kód:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample1.vb)]

Zatímco koncept a sémantika DataList s `ItemDataBound` obslužnou rutinu události jsou stejné jako používaným v nástroji GridView s `RowDataBound` obslužné rutině událostí ve *vlastní formátování založené na Data* kurzu syntaxe se liší mírně. Při `ItemDataBound` dojde k aktivaci události, `DataListItem` právě vázané na data je předána do odpovídající obslužnou rutinu události prostřednictvím `e.Item` (místo `e.Row`, stejně jako u ovládacího prvku GridView s `RowDataBound` obslužná rutina události). DataList s `ItemDataBound` vyvolá obslužnou rutinu události pro *každý* řádky přidané do prvku DataList, včetně řádky záhlaví, zápatí řádky a Oddělovač řádků. Informace o produktu je však vázaný jenom na řádky dat. Proto při použití `ItemDataBound` události a kontrolovat data vázaná na ovládacím prvku DataList, musíme nejprve ujistěte se, že jsme k práci s datovou položku. Toho můžete docílit tak, že zkontrolujete `DataListItem` s [ `ItemType` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), což může mít jednu z [osm následujícího](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Obě `Item` a `AlternatingItem``DataListItem` s strukturu prvku DataList s datovými položkami. Za předpokladu, že můžeme znovu pracovat `Item` nebo `AlternatingItem`, přístupu k skutečnou `ProductsRow` instanci, která byla vázána na aktuální `DataListItem`. `DataListItem` s [ `DataItem` vlastnost](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) obsahuje odkaz na `DataRowView` objekt, jehož `Row` vlastnost obsahuje odkaz na skutečnou `ProductsRow` objektu.

V dalším kroku zkontrolujeme `ProductsRow` instance s `UnitPrice` vlastnost. Od tabulky produktů s `UnitPrice` pole `NULL` hodnoty, než se pokusíte o přístup k `UnitPrice` vlastnost jsme měli by nejdřív zkontrolovat k zjištění, zda má `NULL` hodnotu pomocí `IsUnitPriceNull()` metoda. Pokud `UnitPrice` hodnota není `NULL`, jsme potom zkontrolujte, jestli ho s méně než 20,00 $za. Pokud je ve skutečnosti v části 20,00 $za, pak musíme použít vlastní formátování.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Krok 3: Zvýraznění s názvem produktu a ceny

Jakmile víme, že je cena produkt s méně než 20,00 $, už jen zbývá její název a ceny. K tomu musíte nejdřív prostřednictvím kódu programu odkazujeme ovládací prvky popisku v `ItemTemplate` , který zobrazí název produktu s a ceny. Dále musíme nechat na nich zobrazovat žlutým pozadím. Informace o formátování můžete použít přímou úpravou popisky `BackColor` vlastnosti (`LabelID.BackColor = Color.Yellow`); v ideálním případě by však všechny záležitosti související s zobrazení by měl být vyjádřen prostřednictvím šablony stylů CSS. Ve skutečnosti už máme šablony stylů, která poskytuje požadovanou formátování definované v `Styles.css`  -  `AffordablePriceEmphasis`, který byl vytvořen a zabývá *vlastní formátování založené na Data* kurzu.

K formátování použít, stačí nastavit dva ovládací prvky popisek webového `CssClass` vlastností `AffordablePriceEmphasis`, jak je znázorněno v následujícím kódu:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample2.vb)]

S `ItemDataBound` obslužná rutina události dokončení, opakování `Formatting.aspx` stránku v prohlížeči. Jak znázorňuje obrázek 2 mají tyto produkty s cenami za 20,00 $za své jméno a cena zvýrazní.


[![Tyto produkty méně než 20,00 $za zvýrazněné](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image4.png)

**Obrázek 2**: tyto produkty méně než 20,00 $za jsou zvýrazněny ([kliknutím ji zobrazíte obrázek v plné velikosti](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image6.png))


> [!NOTE]
> Protože prvku DataList se vykreslí jako HTML `<table>`, jeho `DataListItem` instance mají stylu vlastnosti, které je možné nastavit konkrétní stylu na celé položky. Například, pokud chceme, abyste měli na očích *celý* položky žlutý při jeho cena byla menší než 20,00 $ jsme by mohly nahradit kód, na který odkazuje popisky a nastavte jejich `CssClass` vlastnosti s následujícím řádkem kódu: `e.Item.CssClass = "AffordablePriceEmphasis"` (viz obrázek 3).


`RepeaterItem` , Které tvoří ovládacím prvku opakovače však don t nabízejí tyto vlastnosti stylu úrovni. Proto použití vlastního formátování u Opakovači vyžaduje použití vlastnosti stylu webové ovládací prvky v rámci šablon opakovače s stejným způsobem, jako jsme to udělali na obrázku 2.


[![Celý položka produktu je zvýrazněn pro produkty v rámci 20,00 $](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image7.png)

**Obrázek 3**: celý produkt položky je zvýrazněn pro produkty v rámci 20,00 $ ([kliknutím ji zobrazíte obrázek v plné velikosti](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Použití formátování funkcí z v rámci šablony

V *použití vlastností TemplateField v ovládacím prvku GridView* kurzu jsme viděli, jak použít vlastní formátování založené na datech pomocí formátování funkci uvnitř ovládacího prvku GridView TemplateField vázaným na řádky GridView s. Formátování funkce je metoda, která může být vyvolána z šablony a vrátí kód HTML, aby byly vypuštěny na příslušné místo. Formátování funkce mohou být umístěné ve třídě ASP.NET stránky s kódem na pozadí nebo může být centralizované do třídy souborů v `App_Code` složce nebo v samostatném projektu knihovny tříd. Přesun formátování funkce mimo třídu s kódem na pozadí stránky technologie ASP.NET je ideální, pokud máte v úmyslu používat stejnou funkci formátování více stránek v ASP.NET, nebo v jiných webových aplikací ASP.NET.

Abychom si předvedli formátování funkcí, umožňují s máte produktové informace obsahují text [VYŘAZENO] vedle názvu produktu s, pokud ho s ukončena. Navíc umožňují s mají zvýrazněné žlutou if cena ho s méně než 20,00 $za (jako jsme to udělali `ItemDataBound` příklad obslužná rutina události); Pokud je 20,00 $za cenu nebo s vyšší, vám umožňují zobrazit skutečná cena, ale místo toho volat text prosím pro cenové nabídky. Obrázek 4 ukazuje snímek obrazovky produkty výpisu se tato formátování pravidla platila.


[![Nákladné produktů cena je nahrazena textem, vyvolejte funkci pro cenové nabídky](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image10.png)

**Obrázek 4**: nákladné produktů, cena je nahrazena textem, vyvolejte funkci pro cenové nabídky ([kliknutím ji zobrazíte obrázek v plné velikosti](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>Krok 1: Vytvoření formátování funkce

V tomto příkladu budeme potřebovat dvě funkce formátování, ten, který se zobrazí spolu s textem [VYŘAZENO], název produktu v případě potřeby a jiné zobrazující zvýrazněný cena, pokud ho s méně než 20,00 $za nebo textu, vyvolejte funkci pro Cenová nabídka jinak. Umožní s tyto funkce můžete vytvářet v třídě modelu code-behind stránky s ASP.NET a pojmenujte je `DisplayProductNameAndDiscontinuedStatus` a `DisplayPrice`. Obě metody musíte vrátí kód HTML pro vykreslení jako řetězec a oba musí být označen `Protected` (nebo `Public`) aby bylo možné vyvolat z část deklarativní syntaxe stránky s ASP.NET. Následující kód pro tyto dvě metody:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample3.vb)]

Všimněte si, že `DisplayProductNameAndDiscontinuedStatus` metoda přijímá hodnoty `productName` a `discontinued` data polí jako skalární hodnoty, zatímco `DisplayPrice` metoda přijímá `ProductsRow` instance (spíše než `unitPrice` skalární hodnota). Bude fungovat oběma; však-li funkci formátování pracuje s skalárních hodnot, které mohou obsahovat databáze `NULL` hodnoty (například `UnitPrice`; ani `ProductName` ani `Discontinued` povolit `NULL` hodnoty), musí být přijata zvláštní pozornost při zpracování těchto Skalární vstupy.

Konkrétně se vstupní parametr musí být typu `Object` příchozí hodnoty. může se stát, že `DBNull` instanci místo očekávaným datovým typem. Kromě toho musí být provedena kontrola k určení, zda je hodnota příchozí databáze `NULL` hodnotu. To znamená pokud chceme `DisplayPrice` metodu tak, aby přijímal cena jako skalární hodnota, jsme d muset pomocí následujícího kódu:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample4.vb)]

Všimněte si, že `unitPrice` vstupní parametr je typu `Object` a zjistit, pokud byla změněna podmíněný příkaz `unitPrice` je `DBNull` nebo ne. Kromě toho od `unitPrice` vstupní parametr předaný jako `Object`, musí být přetypovat na desítkovou hodnotu.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Krok 2: Volání z ovládacích prvků DataList s ItemTemplate formátování – funkce

S formátování funkce přidané do třídy použití modelu code-behind stránky s naší technologie ASP.NET, už jen zbývá k vyvolání těchto funkcí z prvku DataList s formátování `ItemTemplate`. Volat funkci formátování ze šablony, umístíte volání funkce v rámci Syntaxe datové vazby:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample5.aspx)]

V ovládacím prvku DataList s `ItemTemplate` `ProductNameLabel` popisek webový ovládací prvek nyní zobrazuje název produktu s přiřazením jeho `Text` vlastnost výsledek z `<%# Eval("ProductName") %>`. Pokud chcete mít v případě potřeby zobrazí název a text [VYŘAZENO], aktualizujte deklarativní syntaxe tak, aby místo toho přiřadí `Text` vlastnost hodnotu z `DisplayProductNameAndDiscontinuedStatus` metody. Pokud tak učiníte, musíte předáváme produkt s názvem a ukončená hodnot pomocí `Eval("columnName")` syntaxe. `Eval` Vrátí hodnotu typu `Object`, ale `DisplayProductNameAndDiscontinuedStatus` metoda očekává, že vstupní parametry typu `String` a `Boolean`, proto jsme musíte přetypovat hodnot vrácených `Eval` metoda očekávaným vstupním parametrem typy, například takto:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample6.aspx)]

Pokud chcete zobrazit ceny, nám stačí nastavit `UnitPriceLabel` popisek s `Text` vlastnost na hodnotu vrácenou příkazem `DisplayPrice` metody, stejně jako jsme to udělali pro zobrazení názvu produktu s a [zrušit] text. Namísto předání `UnitPrice` jako skalární vstupní parametr, místo toho předáváme celý `ProductsRow` instance:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample7.aspx)]

Při volání pro formátování funkce na místě věnujte chvíli zobrazíte náš postup v prohlížeči. Vaše obrazovka by měla vypadat podobně jako na obrázku 5, s odpojené produkty, včetně textu [VYŘAZENO] a tyto produkty ocenění více než 20,00 $za s jejich cena nahrazena textem prosím volání pro cenové nabídky.


[![Nákladné produktů cena je nahrazena textem, vyvolejte funkci pro cenové nabídky](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image13.png)

**Obrázek 5**: nákladné produktů, cena je nahrazena textem, vyvolejte funkci pro cenové nabídky ([kliknutím ji zobrazíte obrázek v plné velikosti](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image15.png))


## <a name="summary"></a>Souhrn

Formátování obsah ovládacího prvku DataList nebo Repeater na základě dat, můžete to provést pomocí dvou technik. Jako první postup se používá k vytvoření obslužné rutiny události pro `ItemDataBound` událost, která aktivuje každý záznam ve zdroji dat je vázán na nové `DataListItem` nebo `RepeaterItem`. V `ItemDataBound` obslužná rutina události, se dají prozkoumat data aktuální položky s a pak můžete použít formátování na obsah šablony, nebo pro `DataListItem` s celou položku samotný.

Vlastní formátování můžete alternativně realizované prostřednictvím funkce formátování. Formátování funkce je metoda, která lze volat z prvku DataList nebo Repeater s šablony, které vrátí kód HTML a vygenerovat na příslušné místo. Často HTML vrácené funkcí formátování se určuje podle hodnoty svázaný s aktuální položkou. Tyto hodnoty mohou být předány do funkci formátování jako skalární hodnoty nebo předáním celý objekt svázaný s položkou (například `ProductsRow` instance).

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Yaakov Ellis, Randy Schmidt a Liz Shulok. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
> [další](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
