---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: "Přidat sloupec GridView všech políček (VB) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu vypadá na tom, jak přidat sloupec zaškrtávacích políček do ovládacího prvku GridView poskytnout intuitivní způsob této výběru více řádků G. uživatele..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 326201f9fe9ba5f482308dc8bfd7d2decb9fbd8f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-gridview-column-of-checkboxes-vb"></a>Přidat sloupec GridView všech políček (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe) nebo [stáhnout PDF](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> V tomto kurzu vypadá na tom, jak přidat sloupec zaškrtávacích políček do ovládacího prvku GridView poskytnout intuitivní způsob této výběru více řádků GridView uživatele.


## <a name="introduction"></a>Úvod

V předchozím kurzu jsme se zaměřili jak přidat sloupec přepínačů do GridView pro účely výběrem konkrétního záznamu. Sloupec přepínačů je vhodné uživatelské rozhraní, když uživatel je omezený výběr maximálně jednu položku z mřížky. V některých případech ale může chceme uživateli umožní vybrat libovolný počet položek mřížky. Webové e-mailové klienty, například obvykle zobrazí seznam zpráv se sloupcem zaškrtávací políčka. Uživatele můžete vybrat libovolný počet zpráv a potom provést některé akce, například přesun e-maily do jiné složky nebo jejich odstranění.

V tomto kurzu jsme se zobrazí, jak přidat sloupec zaškrtávací políčka a o tom, jaké zaškrtávací políčka zaškrtnuty na zpětné volání. Konkrétně jsme budete vytvářet příklad, který úzce napodobuje uživatelské rozhraní klienta e-mailové zprávy. Náš příklad bude obsahovat stránkové GridView výpis produkty ve `Products` databázové tabulky s zaškrtávací políčko v každém řádku (viz obrázek 1). Tlačítko Odstranit vybrané produkty, při kliknutí na, dojde k odstranění těchto produktů.


[![Každý řádek produkt obsahuje zaškrtávací políčko](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**Obrázek 1**: zaškrtávací políčko obsahuje každý řádek produktu ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Krok 1: Přidání stránkové GridView, které jsou uvedeny informace o produktu

Před jsme starat o přidání sloupce zaškrtávací políčka, umožní s první zaměřit se na seznam produktů v GridView, který podporuje stránkování. Začněte otevřením `CheckBoxField.aspx` stránku `EnhancedGridView` složku a přetáhněte GridView z panelu nástrojů na návrháře nastavení jeho `ID` k `Products`. V dalším kroku vyberte pro vazbu GridView nové ObjectDataSource s názvem `ProductsDataSource`. Konfigurace ObjectDataSource používat `ProductsBLL` třídy, volání `GetProducts()` metoda vrátit data. Vzhledem k tomu, že tato rutina GridView budou jen pro čtení, nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný).


[![Vytvořit nový ObjectDataSource s názvem ProductsDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**Obrázek 2**: vytvoření nové ObjectDataSource s názvem `ProductsDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))


[![Konfigurace ObjectDataSource k načtení dat pomocí metody GetProducts()](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**Obrázek 3**: Konfigurace ObjectDataSource k načtení dat pomocí `GetProducts()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))


[![Nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný)](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**Obrázek 4**: rozevíracího seznamu jsou uvedeny ve UPDATE, INSERT a odstranit karty na hodnotu (žádná) ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))


Po dokončení průvodce Konfigurace zdroje dat, Visual Studio automaticky vytvoří BoundColumns a CheckBoxColumn pro týkající se produktu datová pole. Postupem uvedeným v tomto kurzu předchozí, odeberte všechny ale na `ProductName`, `CategoryName`, a `UnitPrice` BoundFields a změňte `HeaderText` vlastností produktu, kategorie a ceny. Konfigurace `UnitPrice` BoundField tak, aby její hodnota je formátován jako měny. Také nakonfigurujte GridView pro podporu stránkování zaškrtnutím políčka Povolit stránkování ze inteligentní značky.

Umožní s také přidat uživatelské rozhraní pro odstranění vybrané produkty. Přidání ovládacího prvku tlačítko webové pod GridView, nastavení jeho `ID` k `DeleteSelectedProducts` a jeho `Text` vlastnost odstranit vybrané produkty. Místo skutečného odstranění produkty z databáze, v tomto příkladu budete právě, zobrazuje se zpráva s oznámením, produkty, které by byly odstraněny. K tomuto požadavku vyhovělo, přidání ovládacího prvku popisek webové pod tlačítko. Nastavit jeho ID na `DeleteResults`, zrušte na jeho `Text` vlastnost a nastavte její `Visible` a `EnableViewState` vlastnosti, které chcete `False`.

Po provedení těchto změn, by měl GridView, ObjectDataSource, tlačítka a štítek s deklarativní podobný následujícímu:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

Za chvíli zobrazíte stránku v prohlížeči (viz obrázek 5). Měli byste vidět v tomto okamžiku název, kategorie a cena prvních deset produktů.


[![Název, kategorie a cena první deset produkty jsou uvedeny.](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**Obrázek 5**: Name, kategorie a cena první deset produkty jsou uvedeny ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Krok 2: Přidání sloupce zaškrtávací políčka

Vzhledem k tomu, že technologie ASP.NET 2.0 obsahuje třídy CheckBoxField, jeden může myslíte, že ho může použít k přidání sloupce zaškrtávací políčka do GridView. Bohužel, která není tak, jako třída CheckBoxField je navržen pro práci s polem Boolean – datový. Chcete-li použít vlastnost CheckBoxField jsme to znamená, musíte zadat základní datové pole, jehož hodnota je ohled tehdy, chcete-li zjistit, zda vykreslené zaškrtávací políčko je zaškrtnuto. Nemůžeme použít vlastnost CheckBoxField právě zahrnout sloupec není zaškrtnuto zaškrtávací políčka.

Místo toho jsme musíte přidat TemplateField a přidat ovládací prvek zaškrtávací políčko webového k jeho `ItemTemplate`. Pokračovat a přidat TemplateField k `Products` GridView a nastavit jej jako první pole (úplně zprava doleva). Z s GridView inteligentních značek, klikněte na odkaz Upravit šablony a poté přetáhněte ovládací prvek zaškrtávací políčko webového z panelu nástrojů do `ItemTemplate`. Nastavit toto zaškrtávací políčko s `ID` vlastnost `ProductSelector`.


[![Přidání ovládacího prvku webového zaškrtávací políčko s názvem ProductSelector k TemplateField s ItemTemplate](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**Obrázek 6**: Přidání zaškrtávací políčko webové ovládací prvek s názvem `ProductSelector` k TemplateField s `ItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))


Každý řádek s TemplateField a zaškrtávací políčko webové ovládací prvek přidán, teď obsahuje zaškrtávací políčko. Obrázek 7 znázorňuje tuto stránku při zobrazení prostřednictvím prohlížeče, po přidání TemplateField a zaškrtávací políčko.


[![Každý řádek produktu nyní zahrnuje zaškrtávací políčko](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**Obrázek 7**: každý řádek produktu nyní zahrnuje zaškrtávací políčko ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Krok 3: Určení, jaké zaškrtávací políčka zaškrtnuty na zpětné volání

V tuto chvíli nemáme sloupec zaškrtávací políčka, ale žádný způsob, jak zjistit, jaké zaškrtávací políčka zaškrtnuty na zpětné volání. Při kliknutí na tlačítko Odstranit vybrané produkty, ale je potřeba vědět, jaké zaškrtávací políčka zaškrtnuty Chcete-li odstranit tyto produkty.

Rutina GridView s [ `Rows` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rows.aspx) poskytuje přístup k řádky dat v GridView. Jsme iterace tyto řádky prostřednictvím kódu programu přistupovat ovládacího prvku CheckBox a poté se podívejte jeho `Checked` vlastnosti k určení, zda bylo zaškrtnuto políčko.

Vytvoření obslužné rutiny událostí `DeleteSelectedProducts` ovládací prvek tlačítko webu s `Click` událostí a přidejte následující kód:


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

`Rows` Vlastnost vrátí kolekci `GridViewRow` instance tento způsob vytvoření řádky dat s GridView. `For Each` Smyčky tady zobrazí tuto kolekci. Pro každou `GridViewRow` objektu řádek s zaškrtávací políčko je programově přistupovat pomocí `row.FindControl("controlID")`. Pokud je zaškrtnuté políčko, odpovídající řádek s `ProductID` hodnota je načten z `DataKeys` kolekce. V tomto cvičení zobrazujeme jednoduše informativní zprávy v `DeleteResults` označení, i když v funkční aplikaci jsme d místo toho proveďte volání `ProductsBLL` třídu s `DeleteProduct(productID)` metoda.

Přidáním této obslužné rutiny události se teď klepnutím na tlačítko Odstranit vybrané produkty zobrazí `ProductID` s vybrané produkty.


[![Při kliknutí na tlačítko vybrané produkty odstranit jsou uvedeny produkty ProductIDs vybrané](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**Obrázek 8**: při the odstranit vybrané produktů po kliknutí na tlačítko produkty vybrané `ProductID` s jsou uvedeny ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Krok 4: Přidání zkontroluje všechny a zrušte zaškrtnutí všech tlačítek

Pokud uživatel chce odstranit všechny produkty na aktuální stránce, se musíte zkontrolovat, každý z deset políček. Pomůžeme vám urychlit tento proces přidáním zkontrolujte všechny tlačítko, která po kliknutí na vybere všechny políček v mřížce. Zrušte zaškrtnutí všech tlačítko by stejně užitečné.

Přidání dvě tlačítka na stránce je umístit nad GridView. Nastavit první z nich s `ID` k `CheckAll` a jeho `Text` vlastnost zkontrolujte všechny; nastavte druhý jeden s `ID` k `UncheckAll` a jeho `Text` vlastnost zrušte zaškrtnutí všech.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

V dalším kroku vytvoření metody ve třídě kódu s názvem `ToggleCheckState(checkState)` , po vyvolání vytvoří výčet `Products` GridView s `Rows` kolekce a nastaví každý zaškrtávacího políčka s `Checked` vlastnost na hodnotu předaný v *checkState*  parametr.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

Dále vytvořte `Click` obslužné rutiny události pro `CheckAll` a `UncheckAll` tlačítka. V `CheckAll` obslužné rutiny události s, jednoduše volání `ToggleCheckState(True)`; v `UncheckAll`, volání `ToggleCheckState(False)`.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

S tímto kódem klepnutím na tlačítko Zkontrolovat všechny způsobí zpětné volání a kontroluje všechny políček v GridView. Kliknutím na tlačítko Zrušit zaškrtnutí všech, zruší výběr nakresleného všechna zaškrtávací políčka. Obrázek 9 ukazuje obrazovky po byl vrácen zkontrolujte všechny tlačítko.


[![Kliknutím na kontrolu, kterou všechny tlačítko vybere všechny zaškrtávací políčka](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**Obrázek 9**: Kliknutím na tlačítko Zkontrolovat všechny tlačítko vybere všechny políček ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))


> [!NOTE]
> Při zobrazení sloupce zaškrtávací políčka, jeden ze způsobů pro výběr nebo unselecting všechna zaškrtávací políčka je prostřednictvím zaškrtávací políčko v řádku záhlaví. Kromě toho aktuální zkontroluje všechny nebo zrušte zaškrtnutí všech implementace vyžaduje zpětné volání. Zaškrtávací políčka může být zaškrtnuté nebo nezaškrtnuté, ale zcela prostřednictvím skript na straně klienta, což zajišťuje snappier činnost koncového uživatele. Prozkoumat pomocí zaškrtávacího políčka řádek záhlaví pro zkontrolujte všechny a zrušte zaškrtnutí všech podrobně spolu se zabývat pomocí technik na straně klienta, podívejte se na [kontrola všech políček v GridView pomocí klientský skript a zkontrolujte všechny políčko](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Souhrn

Přidání sloupce zaškrtávací políčka v případech, kde je nutné, aby mohli uživatelé vybrat libovolný počet řádků z GridView než budete pokračovat, je jednou z možností. Jak jsme viděli v tomto kurzu, včetně sloupec zaškrtávací políčka v GridView má za následek přidání TemplateField s ovládacím prvkem webové zaškrtávací políčko. Pomocí ovládacího prvku webového (oproti vložení kódu přímo do šablony, jako jsme to udělali v předchozím kurzu) technologie ASP.NET je automaticky pamatuje co zaškrtávací políčka byly a nebyly zaškrtnutí napříč zpětné volání. Zaškrtávací políčka v kódu k určení, zda daný zaškrtávací políčko je zaškrtnuto, nebo na změnu stavu zaškrtnuté jsme můžete získat přístup také prostřednictvím kódu programu.

Tento kurz a poslední prohlédli GridView přidat sloupec pro výběr řádků. V našem kurzu další podíváme, jak se bit práce, můžete přidáme vkládání možnosti k GridView.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Předchozí](adding-a-gridview-column-of-radio-buttons-vb.md)
[další](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
