---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: Provádění dávkových aktualizací (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Zjistěte, jak vytvořit plně neupravovatelné kde jsou všechny položky v prvku DataList režim úprav a jehož hodnoty mohou být uloženy po kliknutí na tlačítko Aktualizovat vše na...
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: 6d6575e5f13441c38c5d7c74c8b5136a5206ffa9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803892"
---
<a name="performing-batch-updates-vb"></a>Provádění dávkových aktualizací (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe) nebo [stahovat PDF](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> Zjistěte, jak vytvořit plně neupravovatelné kde jsou všechny položky v prvku DataList režim úprav a jehož hodnoty můžete uložit kliknutím na "Aktualizovat vše" tlačítko na stránce.


## <a name="introduction"></a>Úvod

V [předchozím kurzu](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md) jsme se zaměřili na tom, jak vytvořit ovládacích prvků DataList na úrovni položek. Jako zahrnuté standardní upravitelné prvku GridView, každou položku v ovládacím prvku DataList tlačítko úpravy, které, při kliknutí na, s žádným položky upravovat. Zatímco tato položka úrovně úpravy funguje dobře pro data, která je jenom čas od času aktualizován, vyžaduje určité scénáře použití uživateli upravovat mnoho záznamů. Pokud uživatel je potřeba upravit desítky záznamy a bude muset kliknout na upravit, proveďte své změny a klikněte na tlačítko Aktualizovat pro každou z nich, může omezovat velikost kliknutím na její produktivitu. V takových situacích je lepší volbou poskytnout plně upravitelné DataList jednoho, kde *všechny* jeho položek jsou v režimu úprav a jehož hodnoty můžete upravit kliknutím na tlačítko Aktualizovat vše na stránce (viz obrázek 1).


[![Každá položka v plně upravit DataList je možné upravit.](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**Obrázek 1**: je možné upravit každou položku v plně upravit DataList ([kliknutím ji zobrazíte obrázek v plné velikosti](performing-batch-updates-vb/_static/image3.png))


V tomto kurzu prozkoumáme jak povolit uživatelům aktualizovat informace o adrese dodavatelé pomocí plně upravitelné DataList.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Krok 1: Vytvoření upravovat uživatelské rozhraní ve vlastnosti ItemTemplate s

V předchozím kurzu, kde jsme vytváření upravitelné DataList standard, na úrovni položek, použili jsme dvě šablony:

- `ItemTemplate` obsahovala jen pro čtení uživatelského rozhraní (popisek webové ovládací prvky pro zobrazení jednotlivých produktů s názvem a cena).
- `EditItemTemplate` obsažené uživatelského rozhraní režimu úprav (dva textové pole webové ovládací prvky).

DataList s `EditItemIndex` vlastnost určuje, co `DataListItem` (pokud existuje) je vykreslen pomocí `EditItemTemplate`. Konkrétně se `DataListItem` jehož `ItemIndex` hodnota odpovídá prvku DataList s `EditItemIndex` vlastnost je vykreslen pomocí `EditItemTemplate`. Tento model funguje dobře, pokud lze upravovat pouze jedna položka v čase, ale spadající do určité od sebe při vytváření plně upravitelné DataList.

Pro plně upravitelné DataList chceme *všechny* z `DataListItem` s vykreslit pomocí rozhraní upravovat. K definování rozhraní upravovat v je nejjednodušší způsob, jak to provést `ItemTemplate`. Upravitelné rozhraní pro úpravu informace o adrese dodavatelů, obsahuje název dodavatele jako text a potom textová pole pro adresu, Město a zemi hodnoty.

Začněte otevřením `BatchUpdate.aspx` stránce, přidejte ovládací prvek DataList a nastavte jeho `ID` vlastnost `Suppliers`. Z inteligentních značek v prvku DataList s optimalizované pro přidání nového ovládacího prvku ObjectDataSource s názvem `SuppliersDataSource`.


[![Vytvoření nového prvku ObjectDataSource s názvem SuppliersDataSource](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**Obrázek 2**: vytvoření nového prvku ObjectDataSource s názvem `SuppliersDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](performing-batch-updates-vb/_static/image6.png))


Konfigurace ObjectDataSource k načtení dat pomocí `SuppliersBLL` třída s `GetSuppliers()` – metoda (viz obrázek 3). Stejně jako v předchozím kurzu, nikoli aktualizují se informace o dodavateli prostřednictvím ObjectDataSource, budete spolupracujeme přímo se vrstvy obchodní logiky. Proto nastavte rozevírací seznam na (žádný) na kartě aktualizace (viz obrázek 4).


[![Načíst informace o dodavateli pomocí GetSuppliers() – metoda](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**Obrázek 3**: načtení dodavatele informací pomocí `GetSuppliers()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](performing-batch-updates-vb/_static/image9.png))


[![Nastavte rozevírací seznam na (žádný) na kartě aktualizace](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**Obrázek 4**: Nastavte rozevírací seznam na (žádný) na kartě aktualizace ([kliknutím ji zobrazíte obrázek v plné velikosti](performing-batch-updates-vb/_static/image12.png))


Po dokončení průvodce se sada Visual Studio automaticky generuje DataList s `ItemTemplate` pro každé datové pole vrácené zdroje dat v ovládacím prvku popisek webového zobrazení. Potřebujeme upravit tuto šablonu tak, aby místo toho poskytuje rozhraní pro úpravy. `ItemTemplate` Lze přizpůsobit pomocí návrháře pomocí možnosti Upravit šablony v prvku DataList s inteligentním nebo přímo prostřednictvím deklarativní syntaxe.

Využijte k vytvoření úpravy rozhraní, která zobrazuje název s dodavatelem jako text, ale zahrnuje textová pole pro dodavatele adresa, Město a zemi hodnoty. Po provedení těchto změn, vaše stránka s deklarativní syntaxe by měl vypadat nějak takto:


[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> Jako v předchozím kurzu DataList v tomto kurzu musíte mít svůj stav zobrazení povolený.


V `ItemTemplate` můžu m pomocí dvou nových tříd šablon stylů CSS `SupplierPropertyLabel` a `SupplierPropertyValue`, které byly přidány do `Styles.css` třídy a nakonfigurovat tak, aby používat stejné nastavení jako styl `ProductPropertyLabel` a `ProductPropertyValue` tříd CSS.


[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

Po provedení těchto změn, navštivte tuto stránku prostřednictvím prohlížeče. Jak je vidět na obrázku 5, každé položky v prvku DataList zobrazí název dodavatele jako text a používá textových polí k zobrazení adresu, Město a zemi.


[![Každý poskytovatel v ovládacím prvku DataList není upravit](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**Obrázek 5**: každý dodavatele v ovládacím prvku DataList je upravit ([kliknutím ji zobrazíte obrázek v plné velikosti](performing-batch-updates-vb/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>Krok 2: Přidání aktualizační tlačítko vše

Zatímco každého dodavatele na obrázku 5 má jeho adresu, Město a zemi polí zobrazených v testovém poli, aktuálně není žádné tlačítko Aktualizovat. Místo na tlačítko Aktualizovat jednu položku s plně upravitelné DataLists je obvykle na jediné tlačítko Aktualizovat vše na stránce, která aktualizuje po klepnutí na *všechny* záznamy v ovládacím prvku DataList. Pro účely tohoto kurzu nechte s přidejte dvě aktualizace všechna tlačítka - jeden v horní části stránky a druhý v dolní části (i když se klepnutím na tlačítko buď tlačítko bude mít stejný účinek).

Začněte tím, že přidání ovládacího prvku tlačítko webové nad ovládacích prvků DataList a nastavte jeho `ID` vlastnost `UpdateAll1`. V dalším kroku přidejte druhý ovládací prvek tlačítko webového pod ovládacím prvku DataList nastavení jeho `ID` k `UpdateAll2`. Nastavte `Text` vlastnosti pro dvě tlačítka Aktualizovat vše. A konečně, vytváření obslužných rutin událostí pro obě tlačítka `Click` události. Namísto duplikování logika aktualizace v každé z obslužné rutiny událostí, umožňují s Refaktorovat tuto logiku pro třetí metoda `UpdateAllSupplierAddresses`, s jednoduše vyvolání této třetí metody obslužné rutiny událostí.


[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

Obrázek 6 zobrazuje stránku po přidání tlačítka Aktualizovat vše.


[![Dvě tlačítka všechny aktualizace byly přidány na stránku](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**Obrázek 6**: dvě tlačítka všechny aktualizace byly přidány na stránku ([kliknutím ji zobrazíte obrázek v plné velikosti](performing-batch-updates-vb/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Krok 3: Aktualizuje se všechny informace o adrese dodavatelů

Se všemi položkami s DataList zobrazení úprav rozhraní a uveďte aktualizovat všechna tlačítka všechno, zůstane píše kód a proveďte aktualizaci služby batch. Konkrétně jsme muset projít DataList s položky a volání `SuppliersBLL` třída s `UpdateSupplierAddress` metodu pro každý z nich.

Kolekce `DataListItem` instance tuto strukturu prvku DataList je přístupná prostřednictvím DataList s [ `Items` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). S odkazem na `DataListItem`, jsme stáhněte si odpovídající `SupplierID` z `DataKeys` kolekce a programově odkaz webové TextBox – ovládací prvky v rámci `ItemTemplate` jak ukazuje následující kód:


[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

Po kliknutí na jednu z tlačítka Aktualizovat vše `UpdateAllSupplierAddresses` metoda iteraci v každém `DataListItem` v `Suppliers` ovládacích prvků DataList a volání `SuppliersBLL` třída s `UpdateSupplierAddress` metodu odpovídající hodnoty. Zadat hodnotu pro adresu, město nebo země předává je hodnota `Nothing` k `UpdateSupplierAddress` (místo prázdný řetězec), povede k databázi `NULL` pro základní záznam s pole.

> [!NOTE]
> Jako rozšíření můžete přidat stav popisek webový ovládací prvek na stránce, která poskytuje některé potvrzovací zpráva po provedení aktualizace služby batch.


## <a name="updating-only-those-addresses-that-have-been-modified"></a>Aktualizuje se jenom adresy, které byly změněny

Algoritmus aktualizace služby batch použít pro tento kurz volání `UpdateSupplierAddress` metodu *každý* dodavatele v ovládacím prvku DataList, bez ohledu na to, zda se informace o adrese změnil. Při takové blind aktualizuje t nejsou obvykle problému s výkonem, se může vést k nadbytečný záznamy Pokud re auditování změny do tabulky databáze. Například, pokud používáte aktivační události pro záznam všech `UPDATE` s `Suppliers` tabulky do auditování tabulky pokaždé, když uživatel klikne na tlačítko Aktualizovat vše pro každého dodavatele se vytvoří nový záznam auditu v systému, bez ohledu na to, jestli uživatel způsobil změny.

Objekt DataTable ADO.NET a DataAdapter třídy jsou navrženy pro podporu dávkové aktualizace kde výsledky pouze změny, odstraněných a nové záznamy v komunikaci se žádné databáze. Má každý řádek v DataTable [ `RowState` vlastnost](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) , která označuje, zda řádek byl přidán do objektu DataTable z něj, upravit, odstranit nebo zůstane beze změny. Pokud původně naplnění DataTable všechny řádky jsou označeny beze změny. Změna hodnoty sloupce s řádků označí řádku, jako upravit.

V `SuppliersBLL` aktualizujeme informace o adrese zadaný dodavatele s načtením první záznam jednoho dodavatele do třídy `SuppliersDataTable` a pak nastavte `Address`, `City`, a `Country` hodnoty ve sloupcích pomocí následujícího kódu:


[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

Tento kód naively přiřadí předaným adresu, Město a zemi hodnoty `SuppliersRow` v `SuppliersDataTable` bez ohledu na to, zda byly změněny hodnoty. Tyto úpravy způsobit, že `SuppliersRow` s `RowState` vlastnost má být označena jako upravená. Při vrstvy přístupu k datům s `Update` metoda je volána, zjistí, že `SupplierRow` byla změněna a proto pošle `UPDATE` příkaz do databáze.

Představte si však, že jsme přidali kód pro tuto metodu za účelem pouze přiřadit předaným adresu, Město a zemi hodnoty, pokud se liší od `SuppliersRow` s existující hodnoty. V případě, kde stejný jako stávající data jsou adresu, Město a zemi, nebudou provedeny žádné změny a `SupplierRow` s `RowState` nebude změněno vlevo označené jako. Net výsledkem je, že když s vrstvou DAL `Update` metoda je volána, bez volání databáze bude proveden, protože `SuppliersRow` nebyl změněn.

Chcete-li tato změna uplatní, nahraďte příkazy, které slepě přiřadit předaným adresu, Město a zemi hodnoty s následujícím kódem:


[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

To přidá kód, s vrstvou DAL `Update` metoda odešle `UPDATE` příkaz k databázi pro jen takové záznamy, jejichž adresa související hodnoty se změnily.

Také jsme může sledovat, jestli existují rozdíly mezi pole předaným adresy a dat z databáze a, pokud nejsou žádné, jednoduše obejít volání s vrstvou DAL `Update` metody. Tento přístup dobře fungují v případě níž pomocí databáze přímá metoda, protože t DB přímé metody není předaný `SuppliersRow` instance, jejíž `RowState` můžete být zkontrolována k určení, jestli je skutečně potřeba volání databáze.

> [!NOTE]
> Pokaždé, když `UpdateSupplierAddress` vyvolání metody, je provedeno volání databáze k načtení informací o aktualizovaný záznam. Potom, pokud existují změny v datech, další k databázi je provedeno volání aktualizovat řádek tabulky. Tento pracovní postup může optimalizovat tak, že vytvoříte `UpdateSupplierAddress` přetížení metody, která přijímá `EmployeesDataTable` instanci, která má *všechny* změn z `BatchUpdate.aspx` stránky. Poté může nastavit jedno volání databáze a mějte všechny záznamy z `Suppliers` tabulky. Dvě sady výsledků může potom vytvořit výčet a nelze ji aktualizovat jen takové záznamy, kde mají změny došlo k chybě.


## <a name="summary"></a>Souhrn

V tomto kurzu jsme viděli, jak vytvořit plně upravitelné DataList, která uživatelům umožňuje rychle změnit informace o adrese pro více dodavatelů. Začali jsme definováním rozhraní úpravy textového pole webový ovládací prvek pro dodavatele adresa, Město a zemi hodnoty v ovládacím prvku DataList s `ItemTemplate`. Dále jsme přidali aktualizace všechna tlačítka nahoru a dolů prvku DataList. Poté, co uživatel má své změny a kliknutí na jednu z tlačítka Aktualizovat vše `DataListItem` výčtu s a volání `SuppliersBLL` třída s `UpdateSupplierAddress` metoda provádí.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Zack Jones a Ken Pespisa. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [další](handling-bll-and-dal-level-exceptions-vb.md)
