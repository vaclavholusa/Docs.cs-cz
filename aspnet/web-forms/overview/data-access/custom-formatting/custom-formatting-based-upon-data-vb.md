---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: Vlastní formátování na základě údajů o (VB) | Microsoft Docs
author: rick-anderson
description: Úprava formát GridView, DetailsView nebo FormView podle na něj navázaná data lze provést několika způsoby. V tomto kurzu jsme budete l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: a5c7f99b863697cc49a5bc9831dae861f51e129d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876940"
---
<a name="custom-formatting-based-upon-data-vb"></a>Vlastní formátování na základě údajů o (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe) nebo [stáhnout PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> Úprava formát GridView, DetailsView nebo FormView podle na něj navázaná data lze provést několika způsoby. V tomto kurzu budeme zabývat jak provést data vázaná formátování prostřednictvím Vycentrovat a RowDataBound obslužné rutiny událostí.


## <a name="introduction"></a>Úvod

Vzhled ovládacích prvků GridView DetailsView a FormView lze přizpůsobit prostřednictvím velkého počtu vlastnosti související se styly. Vlastnosti, například `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, a `Height`, mimo jiné určuje celkový vzhled vykreslené ovládacího prvku. Vlastnosti včetně `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, a ostatní povolit tyto stejné nastavení stylů má být použita pro konkrétní části. Tato nastavení styl podobně lze použít na úrovni pole.

V mnoha případech ale požadavky formátování závisí na hodnotu zobrazená data. K upozornění na nedostatek stock produktů, například sestavy obsahující informace o produktu může nastavit barvu pozadí na žlutou pro tyto produkty, jehož `UnitsInStock` a `UnitsOnOrder` pole se rovná 0. Abyste měli na očích nákladnější produktů, jsme chtít zobrazit ceny z těchto produktů výpočet nákladů více než 75.00 $ tučným písmem.

Úprava formát GridView, DetailsView nebo FormView podle na něj navázaná data lze provést několika způsoby. V tomto kurzu budeme zabývat jak provést data vázaná formátování prostřednictvím `DataBound` a `RowDataBound` obslužné rutiny událostí. V dalším kurzu jsme budete prozkoumejte alternativní způsob.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Použití ovládacího prvku DetailsView`DataBound`obslužné rutiny události

Data je vázána k DetailsView, buď z prvku zdroje dat, nebo prostřednictvím prostřednictvím kódu programu přiřazování dat k ovládacímu prvku `DataSource` vlastnost a volání jeho `DataBind()` dojít v případě metody následující pořadí kroků:

1. Data ovládací prvek webu `DataBinding` je aktivována událost.
2. Data je vázána k datům ovládací prvek webu.
3. Data ovládací prvek webu `DataBound` je aktivována událost.

Vlastní logiky můžete vložit okamžitě po provedení kroků 1 a 3 prostřednictvím obslužné rutiny události. Vytvořením obslužné rutiny události pro `DataBound` událostí můžeme prostřednictvím kódu programu určit data, která byla vázána na ovládací prvek webu dat a formátování podle potřeby upravit. Pro ilustraci to umožňuje vytvořit DetailsView, který zobrazí seznam obecných informací o produktu, ale zobrazí `UnitPrice` hodnotu ***tučné, kurzíva písma*** pokud překročí $75.00.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Krok 1: Zobrazení informací o produktu v DetailsView

Otevřete `CustomColors.aspx` stránku `CustomFormatting` složky, přetáhněte ovládací prvek DetailsView z panelu nástrojů na návrháře, nastavte jeho `ID` hodnotu vlastnosti na `ExpensiveProductsPriceInBoldItalic`a navázat jej nové prvku ObjectDataSource, která volá `ProductsBLL` třídy `GetProducts()` metoda. Podrobné pokyny k provedení to byly vynechány zde jako stručný výtah, protože jsme se zaměřili je podrobně v předchozí kurzy.

Jakmile je vázaný ObjectDataSource do ovládacího prvku DetailsView, upravte seznam, pole pomocí za chvíli. Jste se rozhodli odebrat `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, a `Discontinued` BoundFields a přejmenovat a zbývající BoundFields naformátována. I taky bylo vymazáno `Width` a `Height` nastavení. Vzhledem k tomu, že DetailsView zobrazí pouze jeden záznam, je potřeba povolit stránkování, chcete-li povolit koncový uživatel Chcete-li zobrazit všechny produkty. Učinit zaškrtnutím políčka Povolit stránkování v ovládacím prvku DetailsView inteligentních značek.


[![Obrázek 1: Zaškrtnutím políčka Povolit stránkování v ovládacím prvku DetailsView inteligentní značky](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**Obrázek 1**: obrázek 1: Zaškrtněte políčko Povolit stránkování v ovládacím prvku DetailsView inteligentní značky ([Kliknutím zobrazit obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image3.png))


Po provedení těchto změn bude DetailsView značek:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

Za chvíli k otestování této stránce v prohlížeči.


[![Ovládací prvek DetailsView zobrazí jeden produktu současně](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**Obrázek 2**: DetailsView ovládací prvek zobrazí jeden produkt najednou ([Kliknutím zobrazit obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Krok 2: Určení prostřednictvím kódu programu hodnotu Data v obslužné rutiny události datové vazby

Aby bylo možné zobrazit cenu v tučné, kurzíva písma pro tyto produkty jejichž `UnitPrice` hodnota přesahuje $75.00, musíme nejprve být schopní prostřednictvím kódu programu určit `UnitPrice` hodnotu. Pro DetailsView, můžete to provést v `DataBound` obslužné rutiny události. Chcete-li vytvořit událost obslužné rutiny DetailsView v Návrháři klepněte na Přejít do okna vlastností. Stisknutím klávesy F4 tím, že ji, pokud není viditelná, nebo přejděte do nabídky zobrazení a vyberte možnost nabídky Vlastnosti – okno. V okně vlastností klikněte na na ikonu bolt seznam prvku DetailsView události. V dalším kroku buď dvakrát klikněte na `DataBound` události nebo zadejte název obslužné rutiny události, kterou chcete vytvořit.


![Vytvoření obslužné rutiny události pro událost datové vazby](custom-formatting-based-upon-data-vb/_static/image7.png)

**Obrázek 3**: vytvoření obslužné rutiny událostí `DataBound` událostí


> [!NOTE]
> Můžete také vytvořit obslužnou rutinu události z části kódu stránky ASP.NET. Existuje zjistíte dva seznamy rozevírací seznam v horní části stránky. Vyberte objekt z levé rozevíracího seznamu a události, kterou chcete vytvořit obslužnou rutinu pro z pravé rozevíracího seznamu a Visual Studio automaticky vytvoří obslužné rutiny příslušné události.


Díky tomu bude automaticky vytvoření obslužné rutiny události a můžete přejít k části kódu kde byl přidán. V tomto okamžiku se zobrazí:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

Data vázaná na DetailsView je přístupná prostřednictvím `DataItem` vlastnost. Odvolat, že jsme se vazba naše ovládacích prvků do DataTable silného typu, který se skládá z kolekce instancí DataRow silného typu. Pokud DataTable vázán na DetailsView, první DataRow DataTable je přiřazen do ovládacího prvku DetailsView `DataItem` vlastnost. Konkrétně `DataItem` vlastnost je přiřazena `DataRowView` objektu. Můžeme použít `DataRowView`na `Row` vlastnost k získání přístupu k podkladového objektu DataRow, která je ve skutečnosti a `ProductsRow` instance. Jakmile jsme to `ProductsRow` instance bychom mohli naše rozhodnutí jednoduše zkontrolováním hodnoty vlastností objektu.

Následující kód ukazuje, jak určit, zda `UnitPrice` vázána na ovládací prvek DetailsView hodnota je větší než $75.00:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> Vzhledem k tomu `UnitPrice` může mít `NULL` hodnota v databázi, nejprve zkontrolujte a ujistěte se, že jsme nejsou zabývajících se `NULL` hodnotu před přístupem k `ProductsRow`na `UnitPrice` vlastnost. Tato kontrola je důležité, protože pokud jsme pokusí o přístup k `UnitPrice` vlastnost, pokud má `NULL` hodnotu `ProductsRow` objekt vyvolá výjimku [StrongTypingException výjimka](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Krok 3: Formátování hodnotu UnitPrice v ovládacím prvku DetailsView

V tomto okamžiku můžeme určit zda `UnitPrice` hodnota vázána na DetailsView má hodnotu, která překračuje $75.00, ale jsme ještě chcete zjistit, jak programově upravit DetailsView je formátování odpovídajícím způsobem. Pokud chcete upravit, formátování v ovládacím prvku DetailsView celý řádek, programový přístup pomocí řádek `DetailsViewID.Rows(index)`; pokud ho chcete změnit konkrétní buňku, přístup k použití `DetailsViewID.Rows(index).Cells(index)`. Jakmile odkaz na řádek nebo buňky jsme potom můžete upravit její vzhled nastavením své vlastnosti související se styly.

Přístup k řádek prostřednictvím kódu programu vyžaduje, že znáte index řádku, který začíná na 0. `UnitPrice` Řádek je pátý řádek v ovládacím prvku DetailsView předá indexu 4 a jejich zpřístupnění programově přístupný pomocí `ExpensiveProductsPriceInBoldItalic.Rows(4)`. V tuto chvíli jsme může mít celý řádek obsah se zobrazuje ve tučné, kurzíva písma a to pomocí následujícího kódu:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

Však bude *i* popisek (cena) a hodnota tučné písmo a kurzíva. Pokud Chceme mít pouze hodnotu tučné písmo a kurzíva musíme použít formátování do druhé buňky v řádku, což lze provést pomocí následujících:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

Vzhledem k tomu, že naše kurzy doposud použili předlohy se styly udržovat čistou oddělení mezi vykreslované značky a informace týkající se styl, místo nastavení vlastností konkrétní styl, jako v příkladu nahoře budeme místo toho použijte třídu CSS. Otevřete `Styles.css` šablony stylů a přidejte novou třídu CSS s názvem `ExpensivePriceEmphasis` s následující definice:


[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

Potom v `DataBound` obslužné rutiny události, nastavte na buňku `CssClass` vlastnost `ExpensivePriceEmphasis`. Následující kód ukazuje `DataBound` obslužné rutiny událostí v celé jeho šíři:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

Při zobrazení Chai, které stojí méně než 75.00 $, ceny se zobrazí v normální písmo (viz obrázek 4). Při zobrazení Niku Kobe Mishi, který má cena $97.00, cenu je však zobrazen tučné, kurzíva písmeny (viz obrázek 5).


[![Ceny nižší než $75.00 se zobrazují v normální písmo](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**Obrázek 4**: ceny nižší než $75.00 se zobrazují v normální písmo ([Kliknutím zobrazit obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image10.png))


[![Ceny nákladné produkty, které se zobrazují v tučné, kurzíva písma](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**Obrázek 5**: ceny nákladné produkty, které se zobrazují v tučné, kurzíva písma ([Kliknutím zobrazit obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>Použití ovládacího prvku FormView`DataBound`obslužné rutiny události

Kroky pro určení základní data vázaná na FormView jsou stejné jako pro vytvoření DetailsView `DataBound` obslužné rutiny události, přetypovat `DataItem` vlastnost na příslušný typ objektu vázán k řízení a určit, jak chcete pokračovat. FormView a DetailsView liší, ale v jak aktualizovat vzhled uživatelské rozhraní.

FormView neobsahuje žádné BoundFields a proto nemá `Rows` kolekce. Místo toho FormView se skládá ze šablony, které může obsahovat kombinaci statických HTML, ovládací prvky webového a syntaxe datové vazby. Úprava styl FormView obvykle zahrnuje úpravě stylu jednoho nebo více webových ovládacích prvků v rámci třídy FormView šablony.

Pro znázornění je umožňuje použití FormView na seznam produktů, jako v předchozím příkladu, ale tentokrát umožňuje zobrazit pouze název produktu a jednotky v stock s jednotky v stock zobrazí red písmeny, pokud je menší než nebo rovno 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Krok 4: Zobrazení informací o produktu v FormView

Přidat FormView k `CustomColors.aspx` stránky pod DetailsView a sadu jeho `ID` vlastnost `LowStockedProductsInRed`. Vytvoření vazby FormView do ovládacího prvku ObjectDataSource vytvořili v předchozím kroku. Tím se vytvoří `ItemTemplate`, `EditItemTemplate`, a `InsertItemTemplate` pro FormView. Odeberte `EditItemTemplate` a `InsertItemTemplate` a zjednodušit `ItemTemplate` zahrnout jenom na `ProductName` a `UnitsInStock` hodnoty, každý vlastní ovládací prvky popisek příslušně pojmenovaných. Stejně jako u DetailsView z předchozího příkladu, také zaškrtněte políčko Povolit stránkování ve třídě FormView inteligentních značek.

Po tyto úpravy vaší FormView značek by měl vypadat takto:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

Všimněte si, že `ItemTemplate` obsahuje:

- **Statické HTML** text "produktu:" a "jednotek na skladě:" spolu s `<br />` a `<b>` elementy.
- **Ovládací prvky webových** dvou ovládacích prvků popisek `ProductNameLabel` a `UnitsInStockLabel`.
- **Datové vazby syntaxe** `<%# Bind("ProductName") %>` a `<%# Bind("UnitsInStock") %>` syntaxe, který přiřazuje hodnoty z těchto polí k ovládacím prvkům Label `Text` vlastnosti.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Krok 5: Určení prostřednictvím kódu programu hodnotu Data v obslužné rutiny události datové vazby

Pomocí značky FormView dokončení, dalším krokem je prostřednictvím kódu programu určete, zda `UnitsInStock` hodnota je menší než nebo rovna 10. To lze provést přesně stejně jako s FormView, stejně jako tomu bylo s DetailsView. Začněte vytvořením obslužné rutiny události pro třídy FormView `DataBound` událostí.


![Vytvoření obslužné rutiny události datové vazby](custom-formatting-based-upon-data-vb/_static/image14.png)

**Obrázek 6**: vytvoření `DataBound` obslužné rutiny události


Události obslužná rutina přetypovat FormView `DataItem` vlastnost, která má `ProductsRow` instance a určit, zda `UnitsInPrice` hodnota je tak, že je potřeba ho zobrazila v red písmo.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Krok 6: Formátování ovládacího prvku UnitsInStockLabel štítek FormView na ItemTemplate

Posledním krokem je k formátování zobrazené `UnitsInStock` hodnotu red písma, pokud hodnota je 10 nebo méně. K tomu je potřeba programový přístup `UnitsInStockLabel` řídit ve `ItemTemplate` a nastavit jeho vlastnosti stylu tak, aby jeho text se zobrazí červeně. Chcete-li získat přístup k ovládací prvek webu v šabloně, použijte `FindControl("controlID")` metoda takto:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

Pro náš příklad chceme přístup štítek řídit, jehož `ID` hodnota je `UnitsInStockLabel`, takže by používáme:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

Jakmile jsme programový odkaz na ovládací prvek webu, jsme podle potřeby změnit jeho vlastnosti související se styly. Jako předchozí příklad, vytvořili jste třídu CSS v `Styles.css` s názvem `LowUnitsInStockEmphasis`. Chcete-li použít tento styl pro ovládací prvek popisek webu, nastavte jeho `CssClass` vlastnost odpovídajícím způsobem.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> Syntaxe pro formátování šablonu prostřednictvím kódu programu přístup k ovládacímu prvku pomocí webové `FindControl("controlID")` a pak nastavení jeho vlastnosti související se styly lze také při použití [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) v DetailsView nebo GridView ovládací prvky. V našem kurzu další podíváme TemplateFields.


Následující obrázky 7 vidíte FormView při prohlížení produktu jejichž `UnitsInStock` hodnota je větší než 10, zatímco v produktu na obrázku 8 je jeho hodnota menší než 10.


[![Pro produkty s dostatečně velké jednotky v Stock ne vlastní formátování](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**Obrázek 7**: se použije pro produkty s dostatečně velké jednotky v Stock, ne vlastní formátování ([Kliknutím zobrazit obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image17.png))


[![Jednotky v Stock číslo se zobrazí červeně pro ty produkty s hodnoty 10 nebo méně](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**Obrázek 8**: jednotky v Stock číslo se zobrazí červeně pro ty produkty s hodnoty 10 nebo méně ([Kliknutím zobrazit obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Formátování s prvku GridView`RowDataBound`událostí

Dříve jsme se zaměřili na pořadí kroků DetailsView a FormView ovládací prvky průběhu prostřednictvím během datové vazby. Podíváme se přes tyto kroky ještě jednou jako aktualizačního programu.

1. Data ovládací prvek webu `DataBinding` je aktivována událost.
2. Data je vázána k datům ovládací prvek webu.
3. Data ovládací prvek webu `DataBound` je aktivována událost.

Tyto tři jednoduchých kroků postačí pro DetailsView a FormView, protože se zobrazí pouze jeden záznam. Pro GridView, který zobrazuje *všechny* záznamy vázána k němu (nikoli jen na prvním), krok 2 je trochu složitější.

V kroku 2 GridView zobrazí zdroj dat a pro každý záznam vytvoří `GridViewRow` instance a vytvoří vazbu na aktuální záznam. Pro každou `GridViewRow` přidá do GridView, dvě události jsou vyvolány:

- **`RowCreated`** Aktivuje se po `GridViewRow` byla vytvořena
- **`RowDataBound`** Aktivuje se po záznam na aktuální záznam byla svázána se `GridViewRow`.

Pro GridView pak datová vazba je přesněji popsané v následujícím pořadí kroků:

1. Rutina GridView na `DataBinding` je aktivována událost.
2. Data je vázána GridView.   
  
   Pro každý záznam ve zdroji dat. 

    1. Vytvoření `GridViewRow` objektu
    2. Ještě efektivněji `RowCreated` událostí
    3. Záznam, který chcete vytvořit vazbu `GridViewRow`
    4. Ještě efektivněji `RowDataBound` událostí
    5. Přidat `GridViewRow` k `Rows` kolekce
3. Rutina GridView na `DataBound` je aktivována událost.

Přizpůsobit formát prvku GridView jednotlivé záznamy, pak je potřeba vytvořit obslužnou rutinu události pro `RowDataBound` událostí. Pro znázornění je přidejme GridView k `CustomColors.aspx` stránky, který obsahuje název, kategorie a ceny pro každý produkt zvýraznění produkty, jejichž cena je menší než 10,00 USD barvou pozadí žlutý.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Krok 7: Zobrazení informací o produktu v GridView

Přidejte GridView pod FormView z předchozího příkladu a nastavte její `ID` vlastnost `HighlightCheapProducts`. Vzhledem k tomu, že jsme už máte ObjectDataSource, která vrací všechny produkty, které na stránce, vazbu k který GridView. Nakonec upravte prvku GridView BoundFields zahrnout jenom tyto produkty názvy, kategorie a ceny. Po tyto úpravy prvku GridView značek by měl vypadat podobně jako:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

Obrázek 9 ukazuje naše probíhá na tento bod při zobrazení prostřednictvím prohlížeče.


[![GridView uvádí název, kategorie a ceny pro každý produkt](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**Obrázek 9**: GridView uvádí název, kategorie a ceny pro každý produkt ([Kliknutím zobrazit obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Krok 8: Programové stanovení hodnotu Data v obslužné rutiny události RowDataBound

Při `ProductsDataTable` je vázán k GridView jeho `ProductsRow` instance jsou výčtové a pro každou `ProductsRow` `GridViewRow` je vytvořena. `GridViewRow`Na `DataItem` vlastnost je přiřazena k konkrétní `ProductRow`, po jejímž uplynutí rutina GridView `RowDataBound` se vyvolá obslužnou rutinu události. Chcete-li zjistit `UnitPrice` hodnotu pro každý produkt vázána na GridView a pak je potřeba vytvořit obslužnou rutinu události pro prvku GridView `RowDataBound` událostí. V této obslužné rutiny události jsme můžete prohlédnout `UnitPrice` hodnotu pro aktuální `GridViewRow` a provedení formátování rozhodnutí na příslušném řádku.

Tuto obslužnou rutinu události lze vytvořit pomocí stejného postupu jako s FormView a DetailsView.


![Vytvoření obslužné rutiny události pro událost RowDataBound GridView.](custom-formatting-based-upon-data-vb/_static/image24.png)

**Obrázek 10**: vytvoření obslužné rutiny události pro prvku GridView `RowDataBound` událostí


Vytváření událostí rutinu tímto způsobem způsobí, že následující kód, který se automaticky přidá do části kódu stránky ASP.NET:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

Když `RowDataBound` aktivuje událost obslužné rutiny události se předá jako její druhý parametr objekt typu `GridViewRowEventArgs`, který má vlastnost s názvem `Row`. Tato vlastnost vrátí odkaz na `GridViewRow` , který byl právě data vázaná. Pro přístup k `ProductsRow` instance vázána `GridViewRow` používáme `DataItem` vlastnost takto:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

Při práci s `RowDataBound` obslužné rutiny události je důležité mít na paměti, která GridView se skládá z různých typů řádků a tato událost je aktivována například pro *všechny* řádek typy. A `GridViewRow`na typ se dá určit pomocí jeho `RowType` vlastnost a může mít jednu z možných hodnot:

- `DataRow` řádek, který je vázaný na záznam z GridView. `DataSource`
- `EmptyDataRow` řádek zobrazí, pokud rutina GridView `DataSource` je prázdná
- `Footer` řádek zápatí; Pokud zobrazené GridView `ShowFooter` je nastavena na `True`
- `Header` řádek záhlaví; Zobrazí, pokud rutina GridView ShowHeader je nastavena na `True` (výchozí)
- `Pager` rutina GridView na které implementují, stránkování, řádek, který zobrazuje rozhraní stránkování
- `Separator` nepoužívá se pro GridView, ale používá `RowType` vlastnosti DataList a opakovače ovládací prvky, dvěma datovými ovládací prvky webového probereme v budoucnosti kurzy

Vzhledem k tomu, `EmptyDataRow`, `Header`, `Footer`, a `Pager` řádků nejsou přidružené `DataSource` záznamů, bude vždy mají hodnotu `Nothing` pro jejich `DataItem` vlastnost. Z tohoto důvodu před pokusem o práci s aktuální `GridViewRow`na `DataItem` vlastnost nám Nejdřív musíte zajistit, aby jsme pracujete s `DataRow`. To lze provést pomocí kontroly `GridViewRow`na `RowType` vlastnost takto:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Krok 9: Zvýraznění žlutý při the UnitPrice hodnota řádku je menší než 10,00 USD

Posledním krokem je prostřednictvím kódu programu zvýrazněte celý `GridViewRow` Pokud `UnitPrice` hodnotu pro tento řádek je menší než 10,00 USD. Syntaxe pro přístup k řádků nebo buněk GridView je stejná jako u DetailsView `GridViewID.Rows(index)` pro přístup k celý řádek `GridViewID.Rows(index).Cells(index)` pro přístup k určité buňky. Ale když `RowDataBound` obslužné rutiny události je v rozhraní vazby dat `GridViewRow` ještě musí být přidán do prvku GridView `Rows` kolekce. Proto nemůže přistupovat k aktuální `GridViewRow` instanci z `RowDataBound` obslužné rutiny události pomocí kolekce řádků.

Místo `GridViewID.Rows(index)`, jsme může odkazovat na aktuální `GridViewRow` instance v `RowDataBound` pomocí obslužných rutin událostí `e.Row`. To znamená, v pořadí, abyste měli na očích aktuální `GridViewRow` instanci z `RowDataBound` by používáme obslužné rutiny události:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

Místo sady `GridViewRow`na `BackColor` vlastnost přímo, můžeme přilepit pomocí tříd CSS. Vytvořili jste třídu CSS s názvem `AffordablePriceEmphasis` , nastaví barvu pozadí žlutě. Dokončené `RowDataBound` následuje obslužné rutiny události:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]


[![Většina cenově dostupné produkty jsou zvýrazněná žlutý](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**Obrázek 11**: produkty cenově dostupné ve většině se zvýrazněnou žlutý ([Kliknutím zobrazit obrázek v plné velikosti](custom-formatting-based-upon-data-vb/_static/image27.png))


## <a name="summary"></a>Souhrn

V tomto kurzu jsme viděli způsob formátování GridView, DetailsView a FormView podle data vázaná na ovládací prvek. K tomu jsme vytvořili obslužné rutiny události pro `DataBound` nebo `RowDataBound` událostí, kde byla v základních datech zkontrolován společně s změny formátování, v případě potřeby. Přístup k datům svázané se DetailsView nebo FormView používáme `DataItem` vlastnost `DataBound` obslužné rutiny události; pro GridView, každý `GridViewRow` instance `DataItem` vlastnost obsahuje data vázaná na příslušném řádku, která je k dispozici v `RowDataBound` obslužné rutiny události.

Syntaxe prostřednictvím kódu programu úpravě formátování ovládacího prvku webového dat závisí na ovládací prvek webu a zobrazení data, která mají být ve formátu. Pro DetailsView a GridView ovládací prvky, řádků a buněk byla přístupná pomocí indexu řadových číslovek. Pro FormView, který používá šablony, `FindControl("controlID")` metoda se běžně používá pro hledání ovládacího prvku webového z v rámci šablony.

V dalším kurzu Podíváme se rutina GridView a DetailsView použití šablon. Kromě toho jsme zobrazí jiné technika pro přizpůsobení formátování na základě základní dat.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Pro tento kurz byly E.R. vést kontrolorů Gilmore, společnosti Dennis Patterson a Dana Jagers. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [další](using-templatefields-in-the-gridview-control-vb.md)
