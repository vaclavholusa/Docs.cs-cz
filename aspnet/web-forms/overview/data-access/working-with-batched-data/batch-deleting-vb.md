---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
title: Dávkové odstranění (VB) | Microsoft Docs
author: rick-anderson
description: Zjistěte, jak odstranit více záznamů databáze v rámci jedné operace. V uživatelské rozhraní vrstvě jsme stavějí rozšířené GridView vytvořili v předchozím kroku tut...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 4fb72f75-32ab-4bf7-a764-be20367be726
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 0d17ec6ec20be65b3ec9369f1c5a08d5970ec0dd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880112"
---
<a name="batch-deleting-vb"></a>Dávkové odstranění (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_VB.zip) nebo [stáhnout PDF](batch-deleting-vb/_static/datatutorial65vb1.pdf)

> Zjistěte, jak odstranit více záznamů databáze v rámci jedné operace. V uživatelské rozhraní vrstvě jsme stavějí rozšířené GridView vytvořené v dřívějších kurzu. V Data Access Layer jsme zabalit více operací odstranit v rámci transakce, aby všechna odstranění úspěšné nebo všechna odstranění jsou vrácena zpět.


## <a name="introduction"></a>Úvod

[Předchozí kurzu](batch-updating-vb.md) prozkoumali postup vytvoření dávky úpravy rozhraní pomocí plně upravitelné GridView. V situacích, kde uživatelé jsou často úpravy mnoho záznamů najednou, bude vyžadovat úpravy rozhraní batch mnohem méně postback a kontext klávesnice myši přepínačích, což povede ke zlepšení efektivity s koncovým uživatelem. Tento postup je podobně užitečné pro stránky, kde je běžné, že uživatelům odstranit velké množství záznamů najednou.

Každý, kdo má použít online e-mailového klienta je již obeznámeni s jedním z nejčastějších dávkové odstranění rozhraní: zaškrtávací políčko v jednotlivých řádcích v mřížce s odpovídající odstranit všechny zaškrtnutí položky tlačítko (viz obrázek 1). V tomto kurzu je místo krátké protože jsme jste už nainstalovali všechny pevné práce v předchozí kurzech při vytváření webové rozhraní a metoda odstranit řadu záznamy jako jeden atomické operace. V [přidáním sloupce GridView všech políček](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) jsme vytvořili GridView se sloupcem zaškrtávací políčka a v kurzu [zabalení změny databáze v rámci transakce](wrapping-database-modifications-within-a-transaction-vb.md) kurzu jsme vytvořili metoda v BLL, který byste použili transakce odstranit `List<T>` z `ProductID` hodnoty. V tomto kurzu jsme stavějí a sloučení naše předchozí možnosti pracovní list odstraňování příkladu vytvoříte.


[![Každý řádek obsahuje zaškrtávací políčko](batch-deleting-vb/_static/image1.gif)](batch-deleting-vb/_static/image1.png)

**Obrázek 1**: každý řádek obsahuje zaškrtávací políčko ([Kliknutím zobrazit obrázek v plné velikosti](batch-deleting-vb/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>Krok 1: Vytvoření dávkové odstranění rozhraní

Vzhledem k tomu, že jsme vytvořili již odstraňování rozhraní v dávce [přidáním sloupce GridView všech políček](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) kurzu jsme můžete jednoduše zkopírujte ho do `BatchDelete.aspx` místo vytvoření od začátku. Začněte otevřením `BatchDelete.aspx` stránku `BatchData` složky a `CheckBoxField.aspx` stránky v `EnhancedGridView` složky. Z `CheckBoxField.aspx` stránky, přejděte do zobrazení zdroje a zkopírujte kód mezi `<asp:Content>` značky, jak je znázorněno na obrázku 2.


[![Deklarativní kódu CheckBoxField.aspx zkopírujte do schránky](batch-deleting-vb/_static/image2.gif)](batch-deleting-vb/_static/image3.png)

**Obrázek 2**: kopírování deklarativní z `CheckBoxField.aspx` do schránky ([Kliknutím zobrazit obrázek v plné velikosti](batch-deleting-vb/_static/image4.png))


Dále přejděte do zobrazení zdroje v `BatchDelete.aspx` a vložit obsah schránky v rámci `<asp:Content>` značky. Také zkopírujte a vložte kód z v rámci třídy kódu v `CheckBoxField.aspx.vb` do v rámci třídy kódu v `BatchDelete.aspx.vb` ( `DeleteSelectedProducts` tlačítko s `Click` obslužné rutiny události, `ToggleCheckState` metody a `Click` obslužné rutiny událostí pro `CheckAll` a `UncheckAll` tlačítek). Po zkopírování přes tento obsah `BatchDelete.aspx` třída stránky s kódu by měla obsahovat následující kód:


[!code-vb[Main](batch-deleting-vb/samples/sample1.vb)]

Po zkopírování přes deklarativní a zdrojový kód, pozorně k testování `BatchDelete.aspx` zobrazením prostřednictvím prohlížeče. Měli byste vidět GridView výpis prvních deset produkty v GridView s každým řádkem výpis produktu s název, kategorie a ceníku spolu s zaškrtávací políčko. Měla by existovat tři tlačítka: Zkontrolujte všechny, zrušte zaškrtnutí všech a odstranit vybrané produkty. Klepnutím na tlačítko Zkontrolovat všechny vybere všechny zaškrtávací políčka, zatímco zrušte zaškrtnutí všech vymaže všechna zaškrtávací políčka. Kliknutím na tlačítko Odstranit vybrané produkty zobrazí zprávu, že jsou uvedené `ProductID` hodnoty vybraných produktů, ale ve skutečnosti neodstraní produkty.


[![Rozhraní z CheckBoxField.aspx byl přesunut do BatchDeleting.aspx](batch-deleting-vb/_static/image3.gif)](batch-deleting-vb/_static/image5.png)

**Obrázek 3**: rozhraní `CheckBoxField.aspx` byl přesunut do `BatchDeleting.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](batch-deleting-vb/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Krok 2: Odstranění zaškrtnuté produkty použití transakcí

Pomocí služby batch odstraňování rozhraní úspěšně zkopírovali do `BatchDeleting.aspx`, všechny, že zůstanou je aktualizovat kód tak, aby tlačítko Odstranit vybrané produkty odstraní zaškrtnuté produkty pomocí `DeleteProductsWithTransaction` metoda v `ProductsBLL` – třída. Tato metoda přidán do [zabalení změny databáze v rámci transakce](wrapping-database-modifications-within-a-transaction-vb.md) kurzu, přijímá jako vstup `List(Of T)` z `ProductID` hodnoty a odstraní každý odpovídající `ProductID` v rámci oboru transakce.

`DeleteSelectedProducts` Tlačítko s `Click` obslužné rutiny události v současné době používá následující `For Each` a iterovat každý řádek GridView:


[!code-vb[Main](batch-deleting-vb/samples/sample2.vb)]

Pro každý řádek `ProductSelector` ovládací prvek zaškrtávací políčko webu se odkazuje prostřednictvím kódu programu. Pokud je zaškrtnutí řádek s `ProductID` se načítají `DataKeys` kolekce a `DeleteResults` štítek s `Text` vlastnost aktualizovala a obsahovala zpráva označující, zda řádek byla vybrána pro odstranění.

Ve výše uvedeném kódu jako volání ve skutečnosti neodstraňuje žádné záznamy `ProductsBLL` třídu s `Delete` metoda je označeno jako komentář. Chtěli použít tuto logiku odstranit, kód by odstranit produkty, ale není v rámci atomické operace. To znamená pokud první několik odstranění v pořadí bylo úspěšné, ale novější verzi se nezdařila (pravděpodobně z důvodu narušení omezení pro cizí klíč), bude vyvolána výjimka ale tyto produkty již odstraněna by zůstala odstraněné.

Aby bylo možné zajistit nedělitelnost, musíme použijte `ProductsBLL` třídu s `DeleteProductsWithTransaction` metoda. Vzhledem k tomu, že tato metoda přijímá seznam `ProductID` hodnoty, musíme napřed zkompilovat tento seznam z mřížky a předejte ji jako parametr. Nejdříve vytvoříme instanci `List(Of T)` typu `Integer`. V rámci `For Each` smyčky, je potřeba přidat vybrané produkty `ProductID` hodnoty k tomuto `List(Of T)`. Po smyčky to `List(Of T)` musí být předán `ProductsBLL` třídu s `DeleteProductsWithTransaction` metoda. Aktualizace `DeleteSelectedProducts` tlačítko s `Click` obslužné rutiny události s následujícím kódem:


[!code-vb[Main](batch-deleting-vb/samples/sample3.vb)]

Aktualizovaný kód vytvoří `List(Of T)` typu `Integer` (`productIDsToDelete`) a ta je s `ProductID` hodnoty odstranit. Po `For Each` cykly, pokud je alespoň jeden produktů vyberete, `ProductsBLL` třídu s `DeleteProductsWithTransaction` metoda je volána a předán tohoto seznamu. `DeleteResults` Se také zobrazí popisek a data odrážejí na GridView (tak, aby nově odstraněné záznamy se už nebude zobrazovat jako řádky v mřížce).

Obrázek 4 ukazuje GridView po počet řádků, byly vybrány pro odstranění. Obrázek 5 ukazuje na obrazovce okamžitě po klepnutí na tlačítko Odstranit vybrané produkty. Všimněte si, že se na obrázku 5 `ProductID` hodnoty odstraněné záznamy zobrazené v popisku pod GridView a jsou již tyto řádky GridView.


[![Odstraní vybrané produkty](batch-deleting-vb/_static/image4.gif)](batch-deleting-vb/_static/image7.png)

**Obrázek 4**: vybrané produkty budou odstraněny ([Kliknutím zobrazit obrázek v plné velikosti](batch-deleting-vb/_static/image8.png))


[![Hodnoty odstranit ProductID produkty jsou uvedené pod the GridView](batch-deleting-vb/_static/image5.gif)](batch-deleting-vb/_static/image9.png)

**Obrázek 5**: produkty odstranit `ProductID` hodnoty jsou uvedené pod the GridView ([Kliknutím zobrazit obrázek v plné velikosti](batch-deleting-vb/_static/image10.png))


> [!NOTE]
> K testování `DeleteProductsWithTransaction` metoda s nedělitelnost ručně přidejte položku pro produkt v `Order Details` tabulce a pak se pokusíte odstranit produktu (spolu s ostatními). Narušení omezení pro cizí klíč obdržíte při pokusu o odstranění produktu s přidružené pořadí, ale Všimněte si, jak jsou ostatní vybrané produkty odstranění vrácena zpět.


## <a name="summary"></a>Souhrn

Vytvoření dávky odstraňování rozhraní zahrnuje přidání GridView se sloupcem zaškrtávací políčka a tlačítko webové řídit, při kliknutí na, odstraní všechny vybrané řádky jako jeden atomické operace. V tomto kurzu jsme vytvořené takového rozhraní piecing společně práci ve dvou předchozí kurzy, [přidáním sloupce GridView všech políček](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) a [zabalení změny databáze v rámci transakce](wrapping-database-modifications-within-a-transaction-vb.md). V prvním kurzu jsme vytvořili GridView se sloupcem zaškrtávací políčka a v jeho implementovali jsme metoda v BLL, když uplyne `List(Of T)` z `ProductID` hodnoty, odstranit, je vše v rámci transakce.

V dalším kurzu vytvoříme rozhraní pro provádění dávkové vložení.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Hilton Giesenow a Teresy Murphy. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](batch-updating-vb.md)
> [další](batch-inserting-vb.md)
