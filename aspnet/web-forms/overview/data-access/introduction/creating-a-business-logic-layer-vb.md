---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
title: Vytvoření vrstvy obchodní logiky (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu uvidíme, jak centralizovat do obchodní logiku vrstvy (BLL), která slouží jako zprostředkovatel pro výměnu dat mezi t obchodní pravidla...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 142e5181-29ce-4bb9-907b-2a0becf7928b
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: eaa91cc46f0d043983c252c1d0adfc413f3b582d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832351"
---
<a name="creating-a-business-logic-layer-vb"></a>Vytvoření vrstvy obchodní logiky (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_2_VB.exe) nebo [stahovat PDF](creating-a-business-logic-layer-vb/_static/datatutorial02vb1.pdf)

> V tomto kurzu uvidíme, jak centralizovat obchodních pravidel do obchodní logiku vrstvy (BLL), která slouží jako zprostředkovatel pro výměnu dat mezi prezentační vrstvou a vrstvy DAL.


## <a name="introduction"></a>Úvod

Data přístup Layer (DAL) vytvořené v [první kurz](creating-a-data-access-layer-vb.md) jasně odděluje data přístup logiky od logiky prezentace. Ale při DAL čistě odděluje podrobnosti o přístupu dat od prezentační vrstvy, nevynucuje žádná obchodní pravidla, která jim umožňují uplatnit. Například pro naši aplikaci budeme chtít zakázat `CategoryID` nebo `SupplierID` pole `Products` tabulka, která má být změněna při `Discontinued` je nastaveno na 1 nebo budeme chtít vynutit pravidla služební zákazu situace, kdy Zaměstnanec spravuje uživatele, který byl přijat po nich. Další z typických možností je možná jenom uživatelé autorizace v konkrétní roli můžete odstranit produktů nebo můžete změnit `UnitPrice` hodnotu.

V tomto kurzu uvidíme, jak centralizovat těchto obchodních pravidel do obchodní logiku vrstvy (BLL), která slouží jako zprostředkovatel pro výměnu dat mezi prezentační vrstvou a vrstvy DAL. V reálné aplikaci BLL by měla být implementována jako samostatný projekt knihovny tříd; ale tyto kurzy jsme budete implementovat BLL jako řadu objektů třídy v našich `App_Code` složku pro zjednodušení strukturu projektu. Obrázek 1 ukazuje architekturu vztahy mezi prezentační vrstvou, knihoven BLL a DAL.


![BLL odděluje prezentační vrstvy z vrstvy přístupu k datům a ukládá obchodní pravidla](creating-a-business-logic-layer-vb/_static/image1.png)

**Obrázek 1**: BLL odděluje prezentační vrstvy z vrstvy přístupu k datům a ukládá obchodní pravidla


Místo vytvoření samostatné třídy k implementaci naše [obchodní logiky](http://en.wikipedia.org/wiki/Business_logic), jsme také může umístit tuto logiku přímo v datové sadě zadán pomocí částečných tříd. Příklad vytvoření a rozšiřování typované datové sady vraťte se k první kurz.

## <a name="step-1-creating-the-bll-classes"></a>Krok 1: Vytvoření BLL tříd

Našich knihoven BLL se skládá ze čtyř tříd, jeden pro každý TableAdapter v DAL; Každá z těchto tříd BLL bude mít metody pro načítání, vkládání, aktualizaci a odstraňování z příslušných TableAdapter v použití pravidel obchodními vrstvy DAL.

Chcete-li více čistě oddělte třídy související s vrstvou DAL a BLL, vytvoříme dvě podsložky v `App_Code` složce `DAL` a `BLL`. Jednoduše klikněte pravým tlačítkem na `App_Code` složku v Průzkumníku řešení a zvolte možnost nové složky. Po vytvoření tyto dvě složky, přesuňte typované datové sady vytvořené v první kurz služby do `DAL` podsložky.

Dále vytvořte čtyři soubory tříd BLL v `BLL` podsložky. Chcete-li to provést, klikněte pravým tlačítkem na `BLL` podsložku, zvolte možnost Přidat novou položku a zvolte šablonu třídy. Název třídy čtyři `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, a `EmployeesBLL`.


![Čtyři nové třídy přidat složku App_Code](creating-a-business-logic-layer-vb/_static/image2.png)

**Obrázek 2**: přidat čtyři nové třídy `App_Code` složky


V dalším kroku přidejme metody do všech tříd jednoduše zabalit metody definované pro objekty TableAdapter z prvního kurzu. Teď tyto metody se právě volají přímo do vrstvy DAL; se vrátíme později a přidejte veškeré potřebné obchodní logiky.

> [!NOTE]
> Pokud používáte Visual Studio Standard Edition nebo vyšší (to znamená, že *není* pomocí Visual Web Developer), Volitelně můžete navrhnout vizuálně pomocí tříd [návrhář tříd](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Odkazovat [blogu návrháře tříd](https://blogs.msdn.com/classdesigner/default.aspx) Další informace o této nové funkce v sadě Visual Studio.


Pro `ProductsBLL` třídy, je potřeba přidat celkem sedmi metod:

- `GetProducts()` Vrátí všechny produkty
- `GetProductByProductID(productID)` Vrátí hodnotu pomocí zadaného kódu product ID produktu
- `GetProductsByCategoryID(categoryID)` Vrátí všechny produkty v určené kategorii
- `GetProductsBySupplier(supplierID)` Vrátí všechny produkty od zadané dodavatele
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` Vložení nového produktu do databáze pomocí hodnot předaný; Vrátí `ProductID` hodnotu nově vloženou záznamu
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` Aktualizuje existující produktu v databázi pomocí hodnoty předané; Vrátí `True` Pokud přesně jeden řádek byl aktualizován, `False` jinak
- `DeleteProduct(productID)` Odstraní zadaný produkt z databáze

ProductsBLL.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample1.vb)]

Metody, které jednoduše vrátí data `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, a `GetProductBySuppliersID` jsou poměrně jednoduché, jak jednoduše volají do vrstvy DAL. I když v některých scénářích může být obchodní pravidla, které je potřeba implementovat na této úrovni (jako jsou pravidla autorizace na základě aktuálně přihlášeného uživatele nebo role, do které patří uživateli), jednoduše Ponecháme tyto metody jako-je. Pro tyto metody pak BLL slouží pouze jako proxy server, jehož prostřednictvím přistupuje prezentační vrstvy k podkladová data z vrstvy přístupu k datům.

`AddProduct` a `UpdateProduct` metody i jako parametry hodnoty pro pole různých produktů a přidat nový produkt aktualizace trvat i existující, v uvedeném pořadí. Od mnoha `Product` sloupců tabulky může přijmout `NULL` hodnoty (`CategoryID`, `SupplierID`, a `UnitPrice`, další), tyto vstupní parametry pro `AddProduct` a `UpdateProduct` , která je namapována na takové použití sloupce [typy připouštějící hodnotu Null](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Typy s možnou hodnotou Null jsou nové pro rozhraní .NET 2.0 a poskytuje techniku pro označující, zda typ hodnoty by měly místo toho být `Nothing`. Odkazovat [Paul Vick](http://www.panopticoncentral.net/)na blogu [The pravdivých informací o typy připouštějící hodnotu Null a VB](http://www.panopticoncentral.net/archive/2004/06/04/1180.aspx) a technická dokumentace pro [Nullable](https://msdn.microsoft.com/library/b3h38hb0%28VS.80%29.aspx) strukturu pro další informace.

Všechny tři metody vrátit hodnotu typu Boolean označující, zda byl řádek vložit, aktualizovat nebo odstranit, protože operace nemusí mít za následek ovlivněných řádků. Například, pokud zavolá vývojář stránky `DeleteProduct` předávání v `ProductID` pro neexistující produkt, `DELETE` prohlášení vydané do databáze bude mít žádný vliv a proto `DeleteProduct` vrátí metoda `False`.

Všimněte si, že při přidání nového produktu nebo aktualizuje existující pořídíme produktu nové nebo upravené hodnoty v polích jako seznam skaláry, na rozdíl od přijetí `ProductsRow` instance. Tento přístup byla vybrána, protože `ProductsRow` třída odvozena z ADO.NET `DataRow` třídu, která nemá výchozí konstruktor bez parametrů. Chcete-li vytvořit nový `ProductsRow` instanci, musíte nejdřív vytvoříme `ProductsDataTable` instance a pak vyvolejte jeho `NewProductRow()` – metoda (které děláme `AddProduct`). Tento nedostatek rears jeho head, když jsme na vkládání a aktualizacích produktů pomocí ObjectDataSource. Stručně řečeno ObjectDataSource se pokusí vytvořit instanci vstupní parametry. Pokud se očekává, že metoda knihoven BLL `ProductsRow` instance, ObjectDataSource se pokusí vytvořit, ale nezdaří z důvodu nedostatku výchozí konstruktor bez parametrů. Další informace o tomto problému najdete v následujících dvou příspěvky ve fórech ASP.NET: [ObjectDataSources aktualizace s datovými sadami Strongly-Typed](https://forums.asp.net/1098630/ShowPost.aspx), a [problém s ObjectDataSource a datová sada Strongly-Typed](https://forums.asp.net/1048212/ShowPost.aspx).

Dále v obou `AddProduct` a `UpdateProduct`, vytvoří kód `ProductsRow` instance a naplní ji hodnotami právě předaný. Při přiřazování hodnot objektů DataColumns objektu DataRow situace může nastat různé kontroly ověření na úrovni pole. Proto sestavení předaných hodnot ručně zpět do DataRow pomáhá zajistit platnost dat předávaných metodě BLL. Objekt DataRow třídy silného typu vygenerované sadou Visual Studio bohužel nepoužívejte typy připouštějící hodnotu Null. Místo toho k označení, že by měl odpovídat konkrétní objekt DataColumn v DataRow `NULL` databáze hodnotě musíme použít `SetColumnNameNull()` metody.

V `UpdateProduct` načteme nejprve v produktu k aktualizaci pomocí `GetProductByProductID(productID)`. Když to může zdát, jako je zbytečné cesty k databázi, bude prokázat smysl v budoucích kurzech, které se zabývají optimistického řízení souběžnosti tento další latence. Optimistická souběžnost je technika, k zajištění, že dva uživatelé, kteří pracují současně na stejná data náhodnému přepsání nepřípustným změny. Zachytávat celý záznam také usnadňuje vytváření metod aktualizace BLL, změnit pouze podmnožinu sloupců objekt DataRow. Když se podíváme `SuppliersBLL` třídy uvidíme takových příkladů.

A konečně, Všimněte si, že `ProductsBLL` třída nemá [DataObject atribut](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) použit ( `[System.ComponentModel.DataObject]` syntaxe přímo před příkaz třídy v horní části souboru) a metody mají [ Atributy DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). `DataObject` Atribut určí třídy jako vhodný pro vazbu k objektu [ovládacího prvku ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), že `DataObjectMethodAttribute` označuj účel metody. Jak uvidíme v budoucích kurzech, technologii ASP.NET 2.0 ObjectDataSource usnadňuje deklarativní přístup k datům ze třídy. Chcete-li filtrovat seznam možných tříd pro vazbu v Průvodci ObjectDataSource, ve výchozím nastavení pouze tyto třídy, které jsou označeny jako `DataObjects` jsou uvedeny v rozevíracím seznamu průvodci. `ProductsBLL` Třídy bude fungovat stejně, bez těchto atributů, ale jejich přidávání usnadňuje práci v prvku ObjectDataSource průvodce.

## <a name="adding-the-other-classes"></a>Přidávání tříd

S `ProductsBLL` třídy kompletní, stále je potřeba přidat tříd pro práci s kategorií, Dodavatelé a zaměstnanci. Za chvíli vytvořit následující třídy a metody využitím konceptů, jako v předchozím příkladu:

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

Je jedna metoda za zmínku `SuppliersBLL` třídy `UpdateSupplierAddress` metody. Tato metoda poskytuje rozhraní pro aktualizaci jenom informace o adrese dodavatele. Interně tato metoda načte v `SupplierDataRow` objekt pro zadaný `supplierID` (pomocí `GetSupplierBySupplierID`), nastaví vlastností souvisejících s adresou a pak zavolá do `SupplierDataTable`společnosti `Update` metoda. `UpdateSupplierAddress` Metoda takto:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample2.vb)]

Odkazovat na soubor ke stažení v tomto článku pro moje úplnou implementaci třídy BLL.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Krok 2: Přístup k zadané datové sady pomocí tříd knihoven BLL

V tomto prvním kurzu jsme viděli příklady pracovat přímo s datovou sadou zadali prostřednictvím kódu programu, ale přidání z našich tříd BLL prezentační vrstvy by měl fungovat BLL místo toho. V `AllProducts.aspx` příklad z první kurz `ProductsTableAdapter` byla použita k vytvoření vazby seznam produktů prvku GridView, jak je znázorněno v následujícím kódu:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample3.vb)]

Použít nové BLL třídy, všechny, které je potřeba změnit je, stačí nahradit prvním řádku kódu `ProductsTableAdapter` objektu `ProductBLL` objektu:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample4.vb)]

Třídy BLL lze přistupovat pomocí ObjectDataSource také deklarativně (jak můžete datové sady typu). Budeme mluvit o ObjectDataSource podrobněji v následujících kurzech.


[![Zobrazí se seznam produktů v GridView](creating-a-business-logic-layer-vb/_static/image4.png)](creating-a-business-logic-layer-vb/_static/image3.png)

**Obrázek 3**: seznam produktů se zobrazí v GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-business-logic-layer-vb/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Krok 3: Přidání ověření na úrovni pole do třídy objektu DataRow

Ověření na úrovni pole jsou kontroly, které se vztahují na hodnoty vlastností objektů firmy při vkládání nebo aktualizaci. Některá pole pravidla pro produkty patří:

- `ProductName` Pole musí být 40 znaků nebo méně
- `QuantityPerUnit` Pole musí být 20 znaků nebo méně
- `ProductID`, `ProductName`, A `Discontinued` pole jsou povinná, ale všechny ostatní pole jsou volitelná
- `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, A `ReorderLevel` pole musí být větší než nebo rovna hodnotě nula.

Tato pravidla můžete a by měl být vyjádřen na úrovni databáze. Znak limitu `ProductName` a `QuantityPerUnit` pole jsou zachyceny na základě těchto sloupců v datové typy `Products` tabulky (`nvarchar(40)` a `nvarchar(20)`v uvedeném pořadí). Určuje, zda jsou povinné a nepovinné pole jsou vyjádřeny ve sloupci tabulky databáze umožňuje `NULL` s. Čtyři [kontrola omezení](https://msdn.microsoft.com/library/ms188258.aspx) existuje, ujistěte se, že pouze hodnoty větší než nebo rovna hodnotě nula. můžete usnadnit do `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, nebo `ReorderLevel` sloupce.

Kromě vynucuje tato pravidla na databázi by také měly být vynucují na úrovni datové sady. Délka pole a určuje, zda je hodnota požadované nebo volitelné jsou ve skutečnosti už zachycena pro každý objekt DataTable sadu objektů DataColumns. Zobrazíte existující ověření na úrovni pole automaticky k dispozici, přejděte na návrháři datových sad, vyberte pole z jednoho DataTables a přejděte do okna Vlastnosti. Jak ukazuje obrázek 4 `QuantityPerUnit` objekt DataColumn v `ProductsDataTable` má maximální délku 20 znaků a neumožňuje procházet spojené `NULL` hodnoty. Pokud jsme pokus o nastavení `ProductsDataRow`společnosti `QuantityPerUnit` vlastnost na hodnotu řetězce, který je delší než 20 znaků `ArgumentException` bude vyvolána výjimka.


[![Objekt DataColumn poskytuje základní ověření na úrovni pole](creating-a-business-logic-layer-vb/_static/image7.png)](creating-a-business-logic-layer-vb/_static/image6.png)

**Obrázek 4**: The DataColumn poskytuje základní ověření na úrovni pole ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-business-logic-layer-vb/_static/image8.png))


Bohužel jsme nelze zadat rozsah kontroly, jako `UnitPrice` hodnota musí být větší než nebo rovna hodnotě nula, v okně Vlastnosti. Aby bylo možné poskytovat tento typ pole potřebujeme vytvořit obslužnou rutinu události pro DataTable [columnchanging –](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) událostí. Jak je uvedeno v [předchozím kurzu](creating-a-data-access-layer-vb.md), datové sady, datové tabulky a datového řádku objektů vytvořených typované datové sady je možné rozšířit pomocí částečných tříd. Tímto způsobem můžeme vytvářet `ColumnChanging` obslužné rutiny události pro `ProductsDataTable` třídy. Začněte tím, že vytvoření třídy v `App_Code` složku s názvem `ProductsDataTable.ColumnChanging.vb`.


[![Přidejte novou třídu ke složce App_Code](creating-a-business-logic-layer-vb/_static/image10.png)](creating-a-business-logic-layer-vb/_static/image9.png)

**Obrázek 5**: přidejte novou třídu do `App_Code` složky ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-business-logic-layer-vb/_static/image11.png))


Dále vytvořte obslužnou rutinu události pro `ColumnChanging` událost, která zajistí, že `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, a `ReorderLevel` hodnoty ve sloupcích (není-li `NULL`) jsou větší než nebo rovna hodnotě nula. Pokud tyto sloupce je mimo rozsah, výjimku `ArgumentException`.

ProductsDataTable.ColumnChanging.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample5.vb)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Krok 4: Přidání vlastních obchodních pravidel BLL tříd

Kromě ověření na úrovni pole může být vysoké úrovně vlastní obchodní pravidla, které zahrnují různé entity nebo koncepty anyAttribute není na úrovni jednoho sloupce, jako například:

- Pokud se vyřazuje, produktu, jeho `UnitPrice` se nedá aktualizovat
- Zaměstnance zemi, kde bydlíte musí být stejný jako jejich správce zemi, kde bydlíte
- Produkt nejde ukončit, pokud je jenom produkt poskytnutých dodavatelem

Třídy BLL by měl obsahovat kontroly pro zajištění dodržování obchodních pravidel vaší aplikace. Tyto kontroly lze přidat přímo do metod, na které se vztahují.

Představte si, že naše obchodní pravidla určují, že produkt nelze označit ukončená Pokud byla pouze produkt z daného dodavatele. To znamená pokud produkt *X* byl jenom produkt jsme zakoupili od dodavatele *Y*, jsme nelze označit *X* jako ukončena; Pokud je však dodavatele *Y*nám součástí tři produkty *A*, *B*, a *C*, jsme mohli označit všechny a všechny z nich bude ukončena. Vždy nejsou zarovnány liché obchodní pravidlo, ale obchodní pravidla a zdravý!

K vynucení toto obchodní pravidlo v `UpdateProducts` začít kontrolou, jestli metoda `Discontinued` byl nastaven na `True` a proto jsme by volání `GetProductsBySupplierID` určit počet produktů jsme zakoupili od dodavatele tohoto produktu. Pokud pouze jeden produkt zakoupen od tohoto dodavatele, jsme throw `ApplicationException`.


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample6.vb)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Reakce na chyby ověření do prezentační vrstvy

Při volání BLL od prezentační vrstvy můžeme rozhodnout, zda pokus o zpracovávat všechny výjimky, které může být vyvolána, nebo jim bublinový až technologie ASP.NET (což vyvolá `HttpApplication`společnosti `Error` událostí). Ke zpracování výjimky při práci s BLL prostřednictvím kódu programu, můžeme použít [zkuste... Zachytit](https://msdn.microsoft.com/library/fk6t46tz%28VS.80%29.aspx) bloku, jako v následujícím příkladu:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample7.vb)]

Jak uvidíme v budoucích kurzech, zpracovávat výjimky, které se BLL při použití dat ovládací prvek webu pro vkládání, aktualizace nebo odstranění dat mohou být zpracovány přímo v obslužné rutině události na rozdíl od nutnosti pro zalomení kódu v `Try...Catch` bloky.

## <a name="summary"></a>Souhrn

Aplikace s dobře navrženou obsahuje do různých vrstev, z nichž každý zapouzdřuje určité role. Z prvního kurzu této série článku jsme vytvořili vrstvy přístupu k datům pomocí zadané datové sady; v tomto kurzu jsme vytvořili vrstvy obchodní logiky jako řadu objektů třídy v naší aplikaci `App_Code` složky, které volají naši vrstvy DAL. BLL implementuje pole úrovni nebo obchodní logiku pro naši aplikaci. Kromě vytvoření samostatných BLL, jako jsme to udělali v tomto kurzu, Další možností je rozšíření metod objekty TableAdapter pomocí částečných tříd. Však tímto způsobem neumožňuje přepsat stávající metody ani nemá ho oddělíte naše DAL a našich knihoven BLL čistě přístup, který jsme měli v tomto článku.

DAL a BLL kompletní jsme připraveni na na našich prezentační vrstvy. V [další kurz](master-pages-and-site-navigation-vb.md) vytvoříme trvat stručný náhradního procesu z témat datového přístupu a definovat rozložení konzistentní stránky pro používání v rámci kurzů.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Liz Shulok, Dennis Patterson, Carlos Santos a Hilton Giesenow. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](creating-a-data-access-layer-vb.md)
> [další](master-pages-and-site-navigation-vb.md)
