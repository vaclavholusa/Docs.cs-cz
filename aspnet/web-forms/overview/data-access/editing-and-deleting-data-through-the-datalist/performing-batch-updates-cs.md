---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
title: Provádění dávkové aktualizace (C#) | Microsoft Docs
author: rick-anderson
description: Naučte se vytvářet plně nelze upravit upravit DataList, kde jsou všechny jeho položky v režimu a jejichž hodnoty můžete uložit kliknutím tlačítko Aktualizovat vše na...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 57743ca7-5695-4e07-aed1-44b297f245a9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
msc.type: authoredcontent
ms.openlocfilehash: af19104edb1849272773193befe1f5b2c7347683
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
---
<a name="performing-batch-updates-c"></a>Provádění dávkové aktualizace (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_CS.exe) nebo [stáhnout PDF](performing-batch-updates-cs/_static/datatutorial37cs1.pdf)

> Naučte se vytvářet plně nelze upravit upravit DataList, kde jsou všechny jeho položky v režimu a jejichž hodnoty můžete uložit kliknutím tlačítko "Vše aktualizace" na stránce.


## <a name="introduction"></a>Úvod

V [předchozí kurzu](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) jsme se zaměřili na tom, jak vytvořit DataList na úrovni položek. Jako standardní upravitelné GridView každou položku v prvku DataList zahrnuté tlačítko upravíte, při kliknutí na, by zkontrolujte položky upravovat. Když to úrovni položek úpravy funguje dobře pro data, která je jenom příležitostně aktualizovat, určitých scénářích případů použití vyžadovat, aby uživatel k úpravě mnoho záznamů. Pokud je uživatel nemůže upravovat desítek záznamů a je nucen se klikněte na tlačítko Upravit, změnit jejich a kliknutím na tlačítko Aktualizovat u každé z nich, může omezovat množství kliknutím na jeho produktivitu. V takových situacích je lepší volbou poskytnout plně upravitelné DataList, kde jeden *všechny* jeho položek jsou v režimu úprav a jejichž hodnoty můžete upravit kliknutím tlačítko Aktualizovat vše na stránce (viz obrázek 1).


[![Každá položka v plně upravitelné DataList je možné upravit.](performing-batch-updates-cs/_static/image2.png)](performing-batch-updates-cs/_static/image1.png)

**Obrázek 1**: je možné upravit každou položku ve plně upravitelné DataList ([Kliknutím zobrazit obrázek v plné velikosti](performing-batch-updates-cs/_static/image3.png))


V tomto kurzu podíváme, jak povolit uživatelům aktualizovat informace o adrese dodavatelé pomocí plně upravitelné DataList.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Krok 1: Vytvoření upravitelné uživatelské rozhraní ve vlastnosti ItemTemplate s

V předchozím kurzu, kde vytváření upravitelné DataList standardní, na úrovni položek, jsme použili jsme dvě šablony:

- `ItemTemplate` obsahuje jen pro čtení uživatelské rozhraní (popisek webové ovládací prvky zobrazení každý s názvem produktu a ceny).
- `EditItemTemplate` obsahuje úpravy režimu uživatelské rozhraní (dva textové pole webové kontroly).

DataList s `EditItemIndex` vlastnost stanoví, co `DataListItem` (pokud existuje) je vykreslen pomocí `EditItemTemplate`. Konkrétně `DataListItem` jejichž `ItemIndex` hodnota odpovídá DataList s `EditItemIndex` vlastnost je vykreslen pomocí `EditItemTemplate`. Tento model funguje dobře, pokud lze upravit jen jednu položku na dobu, ale od sebe spadá při vytváření plně upravitelné DataList.

Pro plně upravitelné DataList, chceme *všechny* z `DataListItem` s k vykreslení pomocí rozhraní upravovat. Nejjednodušší způsob, jak se má definovat upravitelné rozhraní v `ItemTemplate`. Úpravy informace o adrese dodavatelů, upravovat rozhraní obsahuje název dodavatele jako text a pak textová pole pro adresu, Město a země hodnoty.

Začněte otevřením `BatchUpdate.aspx` stránky, přidání ovládacího prvku DataList a nastavit jeho `ID` vlastnost `Suppliers`. Z inteligentních značek DataList s opt přidání nové ovládacího prvku ObjectDataSource s názvem `SuppliersDataSource`.


[![Vytvořit nový ObjectDataSource s názvem SuppliersDataSource](performing-batch-updates-cs/_static/image5.png)](performing-batch-updates-cs/_static/image4.png)

**Obrázek 2**: vytvoření nové ObjectDataSource s názvem `SuppliersDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](performing-batch-updates-cs/_static/image6.png))


Konfigurace ObjectDataSource k načtení dat pomocí `SuppliersBLL` třídu s `GetSuppliers()` – metoda (viz obrázek 3). Stejně jako u předchozí kurz, nikoli aktualizaci informací dodavatele prostřednictvím ObjectDataSource, jsme budete pracovat přímo s vrstvu obchodní logiky. Proto nastavit rozevíracím seznamu (None) na kartě aktualizace (viz obrázek 4).


[![Načíst informace o dodavateli metodou GetSuppliers()](performing-batch-updates-cs/_static/image8.png)](performing-batch-updates-cs/_static/image7.png)

**Obrázek 3**: načtení dodavatele informací pomocí `GetSuppliers()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](performing-batch-updates-cs/_static/image9.png))


[![Nastavit rozevíracím seznamu (None) na kartě aktualizace](performing-batch-updates-cs/_static/image11.png)](performing-batch-updates-cs/_static/image10.png)

**Obrázek 4**: nastavit rozevíracím seznamu (None) na kartě aktualizace ([Kliknutím zobrazit obrázek v plné velikosti](performing-batch-updates-cs/_static/image12.png))


Po dokončení průvodce, Visual Studio automaticky vygeneruje DataList s `ItemTemplate` pro každé datové pole vrácené zdroji dat v ovládacím prvku popisek webové zobrazení. Potřebujeme upravit tuto šablonu tak, aby místo toho poskytuje rozhraní pro úpravy. `ItemTemplate` Lze přizpůsobit pomocí Designer pomocí možnosti Upravit šablony ze DataList s inteligentním nebo přímo pomocí deklarativní syntaxe.

Za chvíli vytvořit úpravy rozhraní, které zobrazuje název dodavatele s jako text, ale zahrnuje textová pole pro adresu s jiného dodavatele, Město a země hodnoty. Po provedení těchto změn, deklarativní syntaxi stránky s by měl vypadat takto:


[!code-aspx[Main](performing-batch-updates-cs/samples/sample1.aspx)]

> [!NOTE]
> Jako u předchozí kurzu DataList v tomto kurzu musí mít svůj stav zobrazení, které jsou povolené.


V `ItemTemplate` I m pomocí dva nové třídy CSS, `SupplierPropertyLabel` a `SupplierPropertyValue`, které jsou přidané do `Styles.css` třídy a nakonfigurovaná pro použití stejné nastavení jako styl `ProductPropertyLabel` a `ProductPropertyValue` tříd CSS.


[!code-css[Main](performing-batch-updates-cs/samples/sample2.css)]

Po provedení těchto změn, najdete na této stránce prostřednictvím prohlížeče. Jak je vidět na obrázku 5, každá položka DataList zobrazí název dodavatele jako text a pomocí textových polí zobrazí adresa, Město a zemi.


[![Každý poskytovatel v prvku DataList není upravit](performing-batch-updates-cs/_static/image14.png)](performing-batch-updates-cs/_static/image13.png)

**Obrázek 5**: každý dodavatele v prvku DataList je upravit ([Kliknutím zobrazit obrázek v plné velikosti](performing-batch-updates-cs/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>Krok 2: Přidání aktualizaci všech tlačítko

Při každé dodavatele z obrázku 5, má svou adresu, města a zobrazí do textového pole. pole země, aktuálně není aktualizace tlačítko k dispozici. Místo na tlačítko Aktualizovat na položku, s plně upravitelné DataLists je obvykle jedno tlačítko Aktualizovat vše na stránce, která po kliknutí na aktualizace *všechny* záznamy v prvku DataList. V tomto kurzu mohli s přidat dvě Aktualizovat vše tlačítka – jeden v horní části stránky a jeden v dolní části (i když kliknete na tlačítko buď bude mít stejný účinek).

Spuštění přidáním ovládacího prvku tlačítko webové nad DataList a sadu jeho `ID` vlastnost `UpdateAll1`. V dalším kroku přidejte druhý ovládací prvek tlačítko webu pod DataList, nastavení jeho `ID` k `UpdateAll2`. Nastavte `Text` vlastnosti pro dvě tlačítka Aktualizovat vše. Nakonec vytváření obslužných rutin událostí pro obě tlačítka `Click` události. Místo duplikování logiku aktualizace v každé z obslužné rutiny událostí, umožňují s Refaktorovat tuto logiku třetí metodu, `UpdateAllSupplierAddresses`, s obslužné rutiny událostí jednoduše volání této metody třetí.


[!code-csharp[Main](performing-batch-updates-cs/samples/sample3.cs)]

Obrázek 6 zobrazuje stránku po přidání tlačítka Aktualizovat vše.


[![Dvě tlačítka všechny aktualizace byly přidány na stránku](performing-batch-updates-cs/_static/image17.png)](performing-batch-updates-cs/_static/image16.png)

**Obrázek 6**: dvě tlačítka všechny aktualizace byly přidány na stránku ([Kliknutím zobrazit obrázek v plné velikosti](performing-batch-updates-cs/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Krok 3: Aktualizace všechny informace o adrese dodavatelů

S všechny položky s DataList zobrazení úpravy rozhraní a přidání tlačítka Aktualizovat všechny všechno, co zůstane zapisuje kód k provedení aktualizace dávky. Konkrétně budeme muset projít DataList s položky a volání `SuppliersBLL` třídu s `UpdateSupplierAddress` metoda pro každé z nich.

Kolekce `DataListItem` instance tento způsob vytvoření prvku DataList je přístupná prostřednictvím DataList s [ `Items` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). S odkazem na `DataListItem`, jsme můžete získat odpovídající `SupplierID` z `DataKeys` kolekce a programově TextBox webové ovládací prvky v rámci odkaz `ItemTemplate` jak ukazuje následující kód:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample4.cs)]

Po klepnutí na Aktualizovat vše tlačítka `UpdateAllSupplierAddresses` metoda iteruje v rámci každé `DataListItem` v `Suppliers` DataList a volání `SuppliersBLL` třídu s `UpdateSupplierAddress` metody předávání v odpovídající hodnoty. Hodnota je hodnota není zadaná pro adresu, město nebo země předává `Nothing` k `UpdateSupplierAddress` (místo prázdný řetězec), což vede databázi `NULL` pro základní pole záznamu s.

> [!NOTE]
> Jako vylepšení můžete přidat stav ovládací prvek popisek webu na stránku, která po provedení aktualizace batch poskytuje některé potvrzovací zpráva.


## <a name="updating-only-those-addresses-that-have-been-modified"></a>Aktualizace pouze adresy, které byly upraveny

Algoritmus aktualizace batch použít pro tento kurz volání `UpdateSupplierAddress` metodu pro *každých* dodavatele v DataList, bez ohledu na to, jestli se změnil informace o jejich adres. Při takové blind aktualizací t nejsou obvykle problémy výkonem, mohou vést nadbytečné záznamy li níž auditování změn do databázové tabulky. Například, pokud používáte k zaznamenání všechny aktivační události `UPDATE` s k `Suppliers` tabulka, která se auditování tabulka pokaždé, když uživatel klikne na tlačítko Aktualizovat vše nový záznam auditu budou vytvořeny pro každého dodavatele v systému, bez ohledu na to, jestli uživatel provedeny žádné změny.

Třídy ADO.NET DataTable a DataAdapter jsou navrženy pro podporu dávková aktualizace kde pouze změny, odstraněných a nové záznamy výsledkem komunikace žádné databáze. Každý řádek v DataTable má [ `RowState` vlastnost](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) určující, zda řádek byl přidán do DataTable, z ho změnit, odstranit nebo zůstává beze změny. Pokud původně naplnění DataTable všechny řádky jsou označeny beze změny. Změna hodnoty sloupce s řádků označí řádek, jako je upravit.

V `SuppliersBLL` jsme aktualizovat informace o adrese zadané dodavatele s první čtení v záznamu o jediného poskytovatele do třídy `SuppliersDataTable` a poté nastavte `Address`, `City`, a `Country` hodnoty ve sloupcích pomocí následujícího kódu:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample5.cs)]

Tento kód naively přiřadí předané adresu, Město a země hodnoty `SuppliersRow` v `SuppliersDataTable` bez ohledu na to, zda hodnoty změnily. Tyto úpravy způsobit, že `SuppliersRow` s `RowState` vlastnost, která má být označena jako upravená. Při Data Access Layer s `Update` metoda je volána, se zobrazí, který `SupplierRow` byl upraven a proto odešle `UPDATE` příkazu k databázi.

Představte si, ale, že jsme přidali kód pro tuto metodu za účelem pouze přiřadit předané adresu, Město a hodnoty země, pokud se liší od `SuppliersRow` s existující hodnoty. V případě, kdy stejný jako stávající data jsou adresy, města a země, nebudou provedeny žádné změny a `SupplierRow` s `RowState` nebude změněno vlevo označen jako. Net výsledkem je, že když DAL s `Update` metoda je volána, bez volání databáze bude možné provést, protože `SuppliersRow` nebyl změněn.

Chcete-li tato změna uplatní, nahraďte příkazy, které slepě přiřazení adres předané, města a země hodnoty následujícím kódem:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample6.cs)]

S tímto přidali kód, DAL s `Update` metoda odešle `UPDATE` příkaz pro databázi pouze záznamy, jejichž hodnoty související s adresu změnily.

Alternativně jsme může sledovat jestli existují případné rozdíly mezi pole předané adresy a databázových dat a, pokud jsou none, jednoduše vynechat volání DAL s `Update` metoda. Tento postup funguje dobře v případě, že níž pomocí databáze přímá metoda, protože předán t DB přímá metoda neběží `SuppliersRow` instance, jehož `RowState` lze zkontrolovat, určete, zda je skutečně potřeba volání databáze.

> [!NOTE]
> Pokaždé, když `UpdateSupplierAddress` metoda je volána, k načtení informací o aktualizovaný záznam Přišla žádost o do databáze. Poté pokud existují změny v datech, jiné do databáze Přišla žádost o aktualizaci řádku tabulky. Tento pracovní postup může být optimalizovaná tak, že vytvoříte `UpdateSupplierAddress` přetížení metody, která přijímá `EmployeesDataTable` instanci, která má *všechny* změn z `BatchUpdate.aspx` stránky. Poté může nastavit jeden volání do databáze s cílem získat všechny záznamy z `Suppliers` tabulky. Dva resultsets pak ve výčtu a nelze ji aktualizovat pouze záznamy, kde mají změny došlo k chybě.


## <a name="summary"></a>Souhrn

V tomto kurzu jsme viděli, jak vytvořit plně upravitelné DataList, umožňuje uživatelům rychle změnit informace o adrese pro více dodavatelů. Spuštění definováním úpravy rozhraní ovládacího prvku webového textového pole pro adresu s jiného dodavatele, města a země hodnoty v DataList s `ItemTemplate`. V dalším kroku jsme přidali Aktualizovat vše tlačítka nahoru a dolů DataList. Poté, co uživatel má jeho změny a kliknutí na jednu z tlačítka Aktualizovat vše `DataListItem` s jsou uvedené a volání `SuppliersBLL` třídu s `UpdateSupplierAddress` metoda provádí.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Zack Petr a Ken Pespisa. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)
> [další](handling-bll-and-dal-level-exceptions-cs.md)
