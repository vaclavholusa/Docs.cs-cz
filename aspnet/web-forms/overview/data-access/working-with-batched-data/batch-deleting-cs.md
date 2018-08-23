---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
title: Dávkové odstraňování (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Zjistěte, jak odstranit více záznamů databáze v rámci jedné operace. Ve vrstvě uživatelského rozhraní můžeme stavět rozšířené GridView vytvořili v předchozím kroku tut...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: ac6916d0-a5ab-4218-9760-7ba9e72d258c
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: c5b4d3c21fad9000ae50ecb35a5d94d176a135ee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755209"
---
<a name="batch-deleting-c"></a>Dávkové odstraňování (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_CS.zip) nebo [stahovat PDF](batch-deleting-cs/_static/datatutorial65cs1.pdf)

> Zjistěte, jak odstranit více záznamů databáze v rámci jedné operace. Ve vrstvě uživatelského rozhraní jsme stavět rozšířené GridView vytvořili v předchozí kurzu. V datové vrstvě přístupu jsme zabalit více operací odstranit v rámci transakce, aby se zajistilo, že všechna odstranění úspěšné nebo všechna odstranění se vrátí zpět.


## <a name="introduction"></a>Úvod

[Předchozím kurzu](batch-updating-cs.md) prozkoumali vytvoření dávky úpravy rozhraní pomocí plně upravitelné ovládacího prvku GridView. V situacích, kdy uživatelé běžně upravujete mnoha záznamů najednou, bude vyžadovat úpravy rozhraní batch mnohem méně zpětného odeslání a kontext myš a klávesnici přepínače, a tím zlepšují efektivitu s koncovým uživatelem. Tato technika je užitečná podobně pro stránky, kde je běžné, že uživatelům odstranit mnoha záznamů najednou.

Každý, kdo použil online e-mailového klienta je již obeznámeni s jedním z nejběžnějších dávkové odstranění rozhraní: zaškrtávací políčko v každém řádku v tabulce s odpovídající odstranit všechny zaškrtnutí položky a tlačítka (viz obrázek 1). Tento kurz je spíše krátký protože jsme ve neučinili všechny těžkou práci v předchozích kurzech při vytváření webové rozhraní a metoda odstranit řadu záznamy jako jednu atomickou operaci. V [přidání sloupce zaškrtávacích políček do ovládacího prvku GridView](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) kurzu jsme vytvořili GridView sloupce zaškrtávacích políček a v [zabalení úprav databáze do transakce](wrapping-database-modifications-within-a-transaction-cs.md) kurzu jsme vytvořili metody v BLL, byste použili transakce odstranit `List<T>` z `ProductID` hodnoty. V tomto kurzu budeme stavět a sloučit naše předchozí prostředí, abyste vytvořili pracovní dávkové odstranění příklad.


[![Každý řádek obsahuje zaškrtávací políčko](batch-deleting-cs/_static/image1.gif)](batch-deleting-cs/_static/image1.png)

**Obrázek 1**: každý řádek obsahuje zaškrtávací políčko ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-deleting-cs/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>Krok 1: Vytvoření dávkové odstranění rozhraní

Protože jsme už vytvořili dávkové odstranění rozhraní [přidání sloupce zaškrtávacích políček do ovládacího prvku GridView](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) kurzu jsme můžete jednoduše zkopírovat ho do `BatchDelete.aspx` místo vytváření od začátku. Začněte otevřením `BatchDelete.aspx` stránku `BatchData` složky a `CheckBoxField.aspx` stránku `EnhancedGridView` složky. Z `CheckBoxField.aspx` stránce, přejděte do zobrazení zdroje a zkopírujte kód mezi `<asp:Content>` značek, jak je znázorněno na obrázku 2.


[![Deklarativní CheckBoxField.aspx zkopírujte do schránky](batch-deleting-cs/_static/image2.gif)](batch-deleting-cs/_static/image3.png)

**Obrázek 2**: Zkopírujte deklarativním označení `CheckBoxField.aspx` do schránky ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-deleting-cs/_static/image4.png))


Dále přejděte do zobrazení zdroje v `BatchDelete.aspx` a vložte obsah schránky. v rámci `<asp:Content>` značky. Také zkopírujte a vložte kód z v rámci třídy modelu code-behind ve `CheckBoxField.aspx.cs` k v rámci třídy modelu code-behind ve `BatchDelete.aspx.cs` ( `DeleteSelectedProducts` tlačítko s `Click` obslužné rutiny události `ToggleCheckState` metody a `Click` obslužné rutiny událostí pro `CheckAll` a `UncheckAll` tlačítka). Po zkopírování přes tento obsah `BatchDelete.aspx` třídy stránky s kódem na pozadí by měl obsahovat následující kód:


[!code-csharp[Main](batch-deleting-cs/samples/sample1.cs)]

Po zkopírování deklarativní a zdrojový kód, věnujte chvíli testování `BatchDelete.aspx` zobrazením prostřednictvím prohlížeče. Měli byste vidět seznam prvních deseti produktů v GridView s každým řádkem výpis produkt s názvem, kategorie a cena spolu s zaškrtávací políčko GridView. Měla by existovat tři tlačítka: Zkontrolujte všechny, zrušte zaškrtnutí všech a odstranit vybrané produkty. Kliknutím na tlačítko Zkontrolovat všechny vybere všech zaškrtávacích políček, zatímco zrušit zaškrtnutí všech vymaže všechna zaškrtávací políčka. Kliknutím na Odstranit vybrané produkty zobrazí zprávu, která obsahuje seznam `ProductID` hodnoty vybrané produkty, ale nedojde k odstranění ve skutečnosti produktů.


[![Rozhraní z CheckBoxField.aspx byl přesunut do BatchDeleting.aspx](batch-deleting-cs/_static/image3.gif)](batch-deleting-cs/_static/image5.png)

**Obrázek 3**: rozhraní z `CheckBoxField.aspx` byl přesunut do `BatchDeleting.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-deleting-cs/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Krok 2: Odstranění Checked produkty pomocí transakcí

Pomocí služby batch, odstraňuje se úspěšně zkopírovat do rozhraní `BatchDeleting.aspx`, all, zůstane je aktualizovat kód tak, aby na tlačítko Odstranit vybrané produkty odstraní checked produkty pomocí `DeleteProductsWithTransaction` metodu `ProductsBLL` třídy. Tato metoda přidá [zabalení úprav databáze do transakce](wrapping-database-modifications-within-a-transaction-cs.md) kurz, přijímá jako vstup `List<T>` z `ProductID` hodnoty a odstraní každý odpovídající `ProductID` v rámci oboru transakce.

`DeleteSelectedProducts` Tlačítko s `Click` obslužná rutina události aktuálně používá následující `foreach` smyčky k iteraci v rámci každého řádku prvku GridView:


[!code-csharp[Main](batch-deleting-cs/samples/sample2.cs)]

Pro každý řádek `ProductSelector` zaškrtávací políčko webový ovládací prvek se odkazuje prostřednictvím kódu programu. Pokud je zaškrtnuto, řádku s `ProductID` je načten z `DataKeys` kolekce a `DeleteResults` popisek s `Text` vlastnost je aktualizováno, aby zahrnovalo zpráva oznamující, že řádek byl vybrán k odstranění.

Výše uvedený kód jako volání skutečně neodstraní všechny záznamy `ProductsBLL` třída s `Delete` metoda je označené jako komentář. Tuto logiku odstranit chtěli použít, by kód odstranit produkty, ale ne v rámci atomická operace. To znamená pokud první několik odstraní v pořadí bylo úspěšné, ale novější verzi se nezdařilo (možná z důvodu narušení omezení pro cizí klíč), bude vyvolána výjimka, ale tyto produkty již odstraněn by zůstala odstraněné.

Aby bylo možné zajistit atomicitu, musíme místo toho použít `ProductsBLL` třída s `DeleteProductsWithTransaction` metody. Protože tato metoda přijímá seznam `ProductID` hodnoty, musíme nejprve ji zkompilujte tento seznam z mřížky a předat ji jako parametr. Nejprve vytvoříme instanci `List<T>` typu `int`. V rámci `foreach` smyčky, je potřeba přidat vybrané produkty `ProductID` hodnoty tohoto `List<T>`. Po smyčce to `List<T>` musí být předán `ProductsBLL` třída s `DeleteProductsWithTransaction` metody. Aktualizace `DeleteSelectedProducts` tlačítko s `Click` obslužné rutiny události s následujícím kódem:


[!code-csharp[Main](batch-deleting-cs/samples/sample3.cs)]

Aktualizovaný kód vytvoří `List<T>` typu `int` (`productIDsToDelete`) a naplní ho `ProductID` hodnoty odstranit. Po `foreach` smyčky, pokud je vybrána, aspoň jeden produkt `ProductsBLL` třída s `DeleteProductsWithTransaction` metoda je volána a tento seznam. `DeleteResults` Popisek se zobrazí také a znovu připojeno data do prvku GridView (tak, aby nově odstraněné záznamy se už nebude zobrazovat jako řádky mřížky).

Obrázek 4 ukazuje prvku GridView, po několik řádků vybraných pro odstranění. Obrázek 5 ukazuje na obrazovce, ihned po kliknutí na tlačítko Odstranit vybrané produkty. Všimněte si, že na obrázku 5 `ProductID` hodnoty odstraněné záznamy zobrazené v popisku pod prvku GridView a řádky už nejsou v prvku GridView.


[![Odstraní vybrané produkty](batch-deleting-cs/_static/image4.gif)](batch-deleting-cs/_static/image7.png)

**Obrázek 4**: The vybrané produkty se odstraní ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-deleting-cs/_static/image8.png))


[![Hodnoty odstranit ProductID produkty jsou uvedené pod prvku GridView](batch-deleting-cs/_static/image5.gif)](batch-deleting-cs/_static/image9.png)

**Obrázek 5**: The produkty odstranit `ProductID` hodnoty jsou uvedené pod prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-deleting-cs/_static/image10.png))


> [!NOTE]
> K testování `DeleteProductsWithTransaction` atomicitu metody s ručně přidat záznam pro produkt `Order Details` tabulku a pak se pokusíte odstranit tento produkt (spolu s ostatními). Narušení omezení pro cizí klíč se zobrazí při pokusu o odstranění produktu s související objednávky, ale Všimněte si, jak ostatní odstranění vybrané produkty jsou vráceny zpět.


## <a name="summary"></a>Souhrn

Vytvoření dávkové odstranění rozhraní zahrnuje přidání prvek GridView s sloupce zaškrtávacích políček a tlačítko webové ovládací prvek, který, při kliknutí na, odstraní všechny vybrané řádky jako jednu atomickou operaci. V tomto kurzu jsme vytvořili toto rozhraní podle piecing společně pracovat v předchozích kurzech [přidání sloupce zaškrtávacích políček do ovládacího prvku GridView](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) a [zabalení úprav databáze do transakce](wrapping-database-modifications-within-a-transaction-cs.md). V prvním kurzu jsme vytvořili GridView s sloupce zaškrtávacích políček a v ten jsme implementovali metodu BLL, při předání `List<T>` z `ProductID` hodnoty, je odstranit vše v rámci oboru transakce.

V dalším kurzu vytvoříme rozhraní pro provádění dávkové vložení.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Hilton Giesenow a Teresy Murphy. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](batch-updating-cs.md)
> [další](batch-inserting-cs.md)
