---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
title: Přidání sloupce GridView zaškrtávacích políček (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Tento kurz zkoumá postup přidání sloupce zaškrtávacích políček do ovládacího prvku GridView uživatel poskytnout intuitivní způsob výběru více řádků G....
ms.author: riande
ms.date: 03/06/2007
ms.assetid: f63a9443-2db0-4f80-8246-840d3e86c2a3
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 1921ceeb33197299f3cedb0eef082af0fd8fa960
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756838"
---
<a name="adding-a-gridview-column-of-checkboxes-c"></a>Přidání sloupce GridView zaškrtávacích políček (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_CS.exe) nebo [stahovat PDF](adding-a-gridview-column-of-checkboxes-cs/_static/datatutorial52cs1.pdf)

> V tomto kurzu snaží přidání sloupce zaškrtávacích políček do ovládacího prvku GridView uživatel poskytnout intuitivní způsob výběru více řádků z prvku GridView.


## <a name="introduction"></a>Úvod

V předchozím kurzu jsme se zaměřili na tom, jak přidání sloupce přepínačů do ovládacího prvku GridView pro účely výběru konkrétního záznamu. Sloupce přepínačů je vhodné uživatelské rozhraní, když uživatel je omezen na maximálně jednu položku výběru z mřížky. V některých případech však může chceme, aby uživatel mohl vybrat libovolný počet položek z mřížky. Webové e-mailové klienty, například obvykle zobrazí seznam zpráv s sloupce zaškrtávacích políček. Uživatele můžete vybrat libovolný počet zpráv a pak provést některé akce, například přesun e-mailů do jiné složky nebo jejich odstranění.

V tomto kurzu jsme se zobrazí přidání sloupce zaškrtávacích políček a jak určit, jaké zaškrtávací políčka byly vráceny na zpětné volání. Zejména vytvoříme příklad, který úzce napodobuje uživatelské rozhraní klienta webového e-mailu. Náš příklad bude obsahovat stránkovaného GridView výpis produktů `Products` databázové tabulky pomocí checkbox v každém řádku (viz obrázek 1). Tlačítko Odstranit vybrané produkty, po kliknutí na dojde k odstranění těchto vybraných produktů.


[![Každý řádek produkt obsahuje zaškrtávací políčko](adding-a-gridview-column-of-checkboxes-cs/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image1.png)

**Obrázek 1**: každý produkt řádek obsahuje zaškrtávací políčko ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-cs/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Krok 1: Přidání stránkovaného prvku GridView, který obsahuje informace o produktu

Předtím, než jsme starat o přidání sloupce zaškrtávacích políček, umožní s první zaměřit se na seznam produktů v prvku GridView, který podporuje stránkování. Začněte otevřením `CheckBoxField.aspx` stránku `EnhancedGridView` složky a GridView přetáhněte z panelu nástrojů do Návrháře nastavení jeho `ID` k `Products`. Dále zvolte k vytvoření nového prvku ObjectDataSource s názvem vazby prvku GridView `ProductsDataSource`. Konfigurace ObjectDataSource používat `ProductsBLL` třídy, volání `GetProducts()` metoda vrátit data. Od tohoto ovládacího prvku GridView budou jen pro čtení, nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný).


[![Vytvoření nového prvku ObjectDataSource s názvem ProductsDataSource](adding-a-gridview-column-of-checkboxes-cs/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image3.png)

**Obrázek 2**: vytvoření nového prvku ObjectDataSource s názvem `ProductsDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-cs/_static/image4.png))


[![Konfigurace ObjectDataSource k načtení dat pomocí GetProducts() – metoda](adding-a-gridview-column-of-checkboxes-cs/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image5.png)

**Obrázek 3**: Konfigurace ObjectDataSource k načtení dat pomocí `GetProducts()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-cs/_static/image6.png))


[![Nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný)](adding-a-gridview-column-of-checkboxes-cs/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image7.png)

**Obrázek 4**: Nastavte rozevírací seznam obsahuje v UPDATE, INSERT a odstranit záložky (žádný) ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-cs/_static/image8.png))


Po dokončení Průvodce nakonfigurovat zdroj dat, sada Visual Studio automaticky vytvoří BoundColumns a CheckBoxColumn týkající se produktu datových polí. Postupem uvedeným v předchozím kurzu, odeberte všechny kromě na `ProductName`, `CategoryName`, a `UnitPrice` BoundFields a změnit `HeaderText` vlastnosti na produkt, kategorie a ceny. Konfigurace `UnitPrice` Vlastnost BoundField tak, aby její hodnota je formátován jako měnu. Také nakonfigurujte GridView pro podporu stránkování zaškrtnutím políčka Povolit stránkování z inteligentních značek.

Umožňuje také přidat uživatelské rozhraní pro odstranění vybrané produkty s. Přidat ovládací prvek tlačítko webového pod prvku GridView, nastavením jeho `ID` k `DeleteSelectedProducts` a jeho `Text` vlastnost odstranit vybrané produkty. Místo skutečného odstranění produktů z databáze, v tomto příkladu jsme budete jenom zobrazovat zpráva, produkty, které by byly odstraněny. Tuto skutečnost zohlednit, přidejte ovládací prvek webového popisek pod tlačítko. Nastavit jeho ID na `DeleteResults`, zrušte zaškrtnutí políčka si jeho `Text` vlastnost a nastavte jeho `Visible` a `EnableViewState` vlastností `false`.

Po provedení těchto změn, by měl ovládacího prvku GridView, prvek ObjectDataSource, tlačítko a popisek s deklarativní podobný následujícímu:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample1.aspx)]

Za chvíli zobrazíte stránku v prohlížeči (viz obrázek 5). V tomto okamžiku byste měli vidět, název, kategorie a cena za prvních deseti produkty.


[![Název, kategorie a cena prvních deset produktů jsou uvedeny.](adding-a-gridview-column-of-checkboxes-cs/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image9.png)

**Obrázek 5**: název, kategorie a cena prvních deset produktů jsou uvedeny ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-cs/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Krok 2: Přidání sloupce zaškrtávacích políček

Protože ASP.NET 2.0 obsahuje třídě CheckBoxField, jeden myslíte, že ho může použít k přidání sloupce zaškrtávacích políček ovládacího prvku GridView. Bohužel to není případ, protože třídě CheckBoxField je navržen pro práci s polem Boolean – datový. Chcete-li použít třídě CheckBoxField jsme tedy musíte zadat podkladové datové pole, jehož hodnota je konzultaci k určení, zda je vykreslené políčko zaškrtnuté. Třídě CheckBoxField jsme nelze použít k obsahuje pouze sloupec není zaškrtnuto zaškrtávací políčka.

Místo toho jsme musí přidat TemplateField a přidejte ovládací prvek zaškrtávací políčko Web pro jeho `ItemTemplate`. Pokračovat a přidat k TemplateField `Products` GridView a nastavte ji na první pole (úplně vlevo). Z ovládacího prvku GridView s inteligentní značky, klikněte na odkaz Upravit šablony a potom přetáhněte ovládací prvek zaškrtávací políčko Web z panelu nástrojů do `ItemTemplate`. Nastavit toto zaškrtávací políčko s `ID` vlastnost `ProductSelector`.


[![Přidejte ovládací prvek zaškrtávací políčko Web s názvem ProductSelector k TemplateField s ItemTemplate](adding-a-gridview-column-of-checkboxes-cs/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image11.png)

**Obrázek 6**: přidejte název ovládacího prvku zaškrtávací políčko Web `ProductSelector` do TemplateField s `ItemTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-cs/_static/image12.png))


Každý řádek s TemplateField a zaškrtávací políčko webové ovládací prvek přidat, teď obsahuje zaškrtávací políčko. Obrázek 7 znázorňuje tuto stránku při zobrazení v prohlížeči po přidání TemplateField a zaškrtávací políčko.


[![Každý řádek produktu nyní obsahuje zaškrtávací políčko](adding-a-gridview-column-of-checkboxes-cs/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image13.png)

**Obrázek 7**: každý řádek produktu nyní obsahuje zaškrtávací políčko ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-cs/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Krok 3: Určení, jaké zaškrtávací políčka byly vráceny na zpětné volání

V tuto chvíli máme sloupec zaškrtávací políčka, ale žádný způsob, jak určit, jaké zaškrtávací políčka byly vráceny na zpětné volání. Při kliknutí na tlačítko Odstranit vybrané produkty, ale potřebujeme vědět, jaké zaškrtávací políčka byly vráceny, pokud chcete odstranit tyto produkty.

GridView s [ `Rows` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) poskytuje přístup k datovým řádkům při prvku GridView. Společnost Microsoft může iterovat v rámci těchto řádků, programový přístup k ovládacím prvku zaškrtávací políčko a potom prostudovat si jeho `Checked` a určí, zda byla vybrána na zaškrtávací políčko.

Vytvořte obslužnou rutinu události pro `DeleteSelectedProducts` tlačítko webový ovládací prvek s `Click` událostí a přidejte následující kód:


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample2.cs)]

`Rows` Vlastnost vrátí kolekci `GridViewRow` instance tuto strukturu řádky dat s ovládacího prvku GridView. `foreach` Smyčky tady zobrazí tuto kolekci. Pro každou `GridViewRow` objektu, řádku s zaškrtávací políčko je programově přistupovat pomocí `row.FindControl("controlID")`. Pokud je zaškrtnuté políčko, odpovídající řádek s `ProductID` hodnota je načten z `DataKeys` kolekce. V tomto cvičení, můžeme jednoduše zobrazit informativní zprávy v `DeleteResults` popisek, i když v aplikaci funkční jsme d místo toho volají `ProductsBLL` třída s `DeleteProduct(productID)` metody.

Přidání této obslužné rutiny události kliknutí na tlačítko Odstranit vybrané produkty teď zobrazí `ProductID` s vybraných produktů.


[![Po kliknutí na tlačítko vybrané produkty odstranit jsou uvedeny produkty ProductIDs vybrané](adding-a-gridview-column-of-checkboxes-cs/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image15.png)

**Obrázek 8**: když the odstranit vybrané produkty po kliknutí na tlačítko vybrané produkty `ProductID` s patří ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-cs/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Krok 4: Přidáním zaškrtnout vše a zrušte zaškrtnutí políčka všechna tlačítka

Pokud chce uživatel odstranit všechny produkty na aktuální stránce, že musíte zkontrolovat, všech deset zaškrtávací políčka. Můžeme vám pomoct urychlit tento proces přidáním a zkontrolovat všechny tlačítko, které, po kliknutí na vybere všechny zaškrtávací políčka v mřížce. Zrušte zaškrtnutí všech tlačítko by stejně vhodné.

Přidejte dva ovládací prvky tlačítka webovou stránku, je umístit nad prvku GridView. Nastavte první z nich s `ID` k `CheckAll` a jeho `Text` vlastnost zkontrolovat všechny; nastavte druhá s `ID` k `UncheckAll` a jeho `Text` vlastnost zrušit zaškrtnutí všech.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample3.aspx)]

Dále vytvořte metodu v třídě modelu code-behind s názvem `ToggleCheckState(checkState)` , při vyvolání, zobrazí `Products` GridView s `Rows` kolekce a nastaví každý zaškrtávacího políčka s `Checked` k hodnotě předané v *– checkState*  parametru.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample4.cs)]

Dále vytvořte `Click` obslužné rutiny událostí pro `CheckAll` a `UncheckAll` tlačítka. V `CheckAll` obslužné rutiny události s, jednoduše volání `ToggleCheckState(true)`; v `UncheckAll`, volání `ToggleCheckState(false)`.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample5.cs)]

S tímto kódem kliknutím na tlačítko Zkontrolovat všechny vyvolá zpětné volání a kontroluje všechny zaškrtávacích políček v prvku GridView. Kliknutím na zrušit zaškrtnutí všech, zruší výběr nakresleného všech zaškrtávacích políček. Obrázek 9 ukazuje obrazovky po byly vráceny na tlačítko Zkontrolovat všechny.


[![Kliknutím na kontrolu, kterou všechny tlačítko vybere všech zaškrtávacích políček](adding-a-gridview-column-of-checkboxes-cs/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image17.png)

**Obrázek 9**: Kliknutím zkontrolovat všechny tlačítko vybere všech zaškrtávacích políček ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-cs/_static/image18.png))


> [!NOTE]
> Při zobrazení sloupce zaškrtávacích políček, jedním z přístupů pro výběr nebo unselecting všechna zaškrtávací políčka je zaškrtávací políčko v řádku záhlaví. Kromě toho aktuální zaškrtnout vše / zrušit zaškrtnutí všech implementace vyžaduje zpětného odeslání. Zaškrtávací políčka může být zaškrtnuté nebo nezaškrtnuté, ale výhradně pomocí skriptu na straně klienta, získat tak zacílíte činnost koncového uživatele. Prozkoumejte používání zaškrtávací políčko v řádku záhlaví pro zaškrtnout vše a zrušte zaškrtnutí všech podrobně spolu se zabývat pomocí technik na straně klienta, přečtěte si [kontrolu všech zaškrtávacích políček do ovládacího prvku GridView pomocí skriptu na straně klienta a zaškrtněte všechna políčka](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Souhrn

V případech, kdy potřebujete umožněte uživatelům si vybrat libovolný počet řádků z prvku GridView než budete pokračovat je přidání sloupce zaškrtávacích políček jednu možnost. Jak jsme viděli v tomto kurzu, včetně sloupce zaškrtávacích políček v prvku GridView. má za následek přidání TemplateField s ovládacím prvkem webového zaškrtávací políčko. Pomocí webového ovládacího prvku (oproti injektáž kódu přímo do šablony, jako jsme to udělali v předchozím kurzu) technologie ASP.NET automaticky si pamatuje co zaškrtávací políčka byly a nebyly vráceny napříč zpětného odeslání. Můžeme také programový přístup k zaškrtávací políčka v kódu k určení, zda je daný políčko zaškrtnuté, nebo chcete změnit zaškrtnutém stavu.

V tomto kurzu, posledním blokem podívali se na přidání sloupec pro výběr řádků k prvku GridView. V následujícím kurzem prozkoumáme, jak se trochu práce, můžeme přidat vkládání funkcí do prvku GridView.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](adding-a-gridview-column-of-radio-buttons-cs.md)
> [další](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
