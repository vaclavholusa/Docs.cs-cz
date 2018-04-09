---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
title: Vytvoření vrstvy obchodní logiky (C#) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu ukážeme, jak centralizovat obchodní pravidla do obchodní logiku vrstvy (BLL), který slouží jako prostředník pro výměnu dat mezi t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 85554606-47cb-4e4f-9848-eed9da579056
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 6e73e9e68e4abb0d382baa7da925c167809e417a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-business-logic-layer-c"></a>Vytvoření vrstvy obchodní logiky (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_2_CS.exe) nebo [stáhnout PDF](creating-a-business-logic-layer-cs/_static/datatutorial02cs1.pdf)

> V tomto kurzu vidíte jak centralizovat obchodní pravidla do obchodní logiku vrstvy (BLL), který slouží jako prostředník pro výměnu dat mezi prezentační vrstvou a DAL.


## <a name="introduction"></a>Úvod

Data přístup Layer (DAL) vytvořené v [první kurz](creating-a-data-access-layer-cs.md) této aplikace odděluje data přístup logiku z logiky prezentace. Ale při DAL řádně odděluje podrobnosti o přístupu dat od prezentační vrstvy, se nevynucuje žádné obchodní pravidla, které se mohou vztahovat. Například pro naši aplikaci může chceme zakáže `CategoryID` nebo `SupplierID` pole `Products` tabulky má být změněn, kdy `Discontinued` je nastaveno na 1, nebo jsme chtít vynutit pravidla služebních zákazu situace, kdy Zaměstnanec spravuje uživatele, který byl přijat po jejich. Další z typických možností je možná pouze uživatelé autorizace v konkrétní roli můžete odstranit produkty nebo můžete změnit `UnitPrice` hodnotu.

V tomto kurzu vidíte jak centralizovat do obchodní logiku vrstvy (BLL), který slouží jako prostředník pro výměnu dat mezi prezentační vrstvou a DAL tyto obchodní pravidla. V reálné aplikaci by měla být implementována BLL jako samostatné projektu knihovny tříd. ale pro tyto kurzy jsme budete implementovat BLL jako řadu třídy v našem `App_Code` složky, aby bylo možné zjednodušit strukturu projektu. Obrázek 1 ukazuje architektury vztahy mezi prezentační vrstvy, BLL a DAL.


![BLL odděluje prezentační vrstvy od Data Access Layer a ukládá obchodní pravidla](creating-a-business-logic-layer-cs/_static/image1.png)

**Obrázek 1**: BLL odděluje prezentační vrstvy od Data Access Layer a ukládá obchodní pravidla


## <a name="step-1-creating-the-bll-classes"></a>Krok 1: Vytvoření BLL třídy

Naše BLL bude skládat ze čtyř třídy, jeden pro každou TableAdapter v DAL; Každá z těchto tříd BLL bude mít metody pro načítání, vkládání, aktualizaci a odstraňování z příslušných TableAdapter v DAL, použití příslušné obchodní pravidla.

Chcete-li více této aplikace oddělte třídy související s vrstvou DAL a BLL, vytvoříme dvě podsložky v `App_Code` složky, `DAL` a `BLL`. Jednoduše klikněte pravým tlačítkem na `App_Code` složky v Průzkumníku řešení a vyberte novou složku. Po vytvoření tyto dvě složky, přesuňte typové datové sady vytvořené v první kurz do `DAL` podsložky.

Dále vytvořte čtyři soubory třídy BLL v `BLL` podsložky. K tomu, klikněte pravým tlačítkem na `BLL` podsložku, zvolte možnost Přidat novou položku a vyberte šablonu třída. Název třídy čtyři `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, a `EmployeesBLL`.


![Přidat čtyři nové třídy do složky App_Code](creating-a-business-logic-layer-cs/_static/image2.png)

**Obrázek 2**: přidat čtyři nové třídy `App_Code` složky


Dále umožňuje přidání metody do jednotlivých tříd jednoduše zabalit metody definované pro TableAdapters z první kurz. Teď budou tyto metody stačí zavolat přímo do vrstvy DAL; se vrátíme později přidat veškeré potřebné obchodní logiky.

> [!NOTE]
> Pokud používáte Visual Studio Standard Edition nebo vyšší (to znamená, že *není* pomocí Visual Web Developer), Volitelně můžete navrhnout vaše třídy vizuálně pomocí [návrhář tříd](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Odkazovat [třídu návrháře Blog](https://blogs.msdn.com/classdesigner/default.aspx) Další informace o této nové funkce v sadě Visual Studio.


Pro `ProductsBLL` třída je potřeba přidat celkem sedm metody:

- `GetProducts()` Vrátí všechny produkty
- `GetProductByProductID(productID)` Vrátí součin s ID zadaného produktu
- `GetProductsByCategoryID(categoryID)` Vrátí všechny produkty z Zadaná kategorie
- `GetProductsBySupplier(supplierID)` Vrátí všechny produkty z zadaný dodavatele
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` Vloží nového produktu do databáze pomocí hodnot předané; Vrátí `ProductID` hodnotu nově vloženou záznamu
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` Aktualizuje existující produkt v databázi pomocí hodnoty předané; Vrátí `true` Pokud přesně jeden řádek aktualizován, `false` jinak
- `DeleteProduct(productID)` Odstraní zadaný produkt z databáze

ProductsBLL.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample1.cs)]

Metody, které jednoduše vrátit data `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, a `GetProductBySuppliersID` jsou přímočará jednoduše volají do vrstvy DAL. Když v některých případech může být obchodní pravidla, které potřebují k implementaci na této úrovni (například na základě aktuálně přihlášeného uživatele nebo roli, do které uživatel patří autorizační pravidla), jednoduše Ponecháme tyto metody jako-je. Pro tyto metody pak BLL slouží pouze jako proxy server, pomocí kterého se přistupuje prezentační vrstvy podkladová data z Data Access Layer.

`AddProduct` a `UpdateProduct` metody jak využít jako parametry hodnoty pro různá pole produktu a přidání nového produktu nebo aktualizaci existující, v uvedeném pořadí. Od řadu `Product` sloupce tabulky může přijmout `NULL` hodnoty (`CategoryID`, `SupplierID`, a `UnitPrice`, a další), ty vstupní parametry pro `AddProduct` a `UpdateProduct` které jsou namapovány na tyto sloupce použijte [typy s možnou hodnotou Null](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Typy s možnou hodnotou Null jsou nové pro rozhraní .NET 2.0 a zadejte techniku, pro která udává, zda hodnota typ měli, místo toho se `null`. V jazyce C# můžete příznak typ hodnoty jako typ s možnou hodnotou Null přidáním `?` po typ (například `int? x;`). Odkazovat [typy s možnou hodnotou Null](https://msdn.microsoft.com/library/1t3y8s4s.aspx) tématu [Průvodce programováním v C#](https://msdn.microsoft.com/library/67ef8sbd%28VS.80%29.aspx) Další informace.

Všechny tři metody vrací logickou hodnotu udávající, zda byl řádek vložit, aktualizovat nebo odstranit, protože operaci nemusí mít za následek ovlivněných řádků. Vývojář stránky volá-li například `DeleteProduct` předávání v `ProductID` pro neexistující produkt, `DELETE` prohlášení vydané do databáze bude mít žádný vliv a proto `DeleteProduct` metoda vrátí `false`.

Všimněte si, že při přidání nového produktu nebo aktualizace stávající jsme trvat produktu nové nebo upravené hodnoty v polích jako seznam skalárních hodnot a přijetí `ProductsRow` instance. Tento přístup je vybraná, protože `ProductsRow` třída odvozená z technologie ADO.NET `DataRow` třídy, která nemá výchozí konstruktor bez parametrů. Chcete-li vytvořit nový `ProductsRow` instanci, musíte nejdřív vytvoříme `ProductsDataTable` instance a pak vyvolejte jeho `NewProductRow()` – metoda (který My v `AddProduct`). Tohoto nedostatku rears jeho head, když jsme zapněte, pokud k vložení a aktualizace pomocí ObjectDataSource produkty. Stručně řečeno ObjectDataSource se pokusí vytvořit instanci vstupní parametry. Pokud metoda BLL očekává `ProductsRow` instanci, ObjectDataSource se pokusí vytvořit, ale nezdaří z důvodu nedostatku výchozí konstruktor bez parametrů. Další informace o tento problém, naleznete v následujících dvou příspěvky ve fórech ASP.NET: [aktualizace ObjectDataSources s datovými sadami Strongly-Typed](https://forums.asp.net/1098630/ShowPost.aspx), a [problém s ObjectDataSource a datovou sadu Strongly-Typed](https://forums.asp.net/1048212/ShowPost.aspx).

Dále v obou `AddProduct` a `UpdateProduct`, vytvoří kód `ProductsRow` instance a naplní s právě předané hodnoty. Při přiřazování hodnot objektů DataColumns DataRow situace může nastat různé kontroly ověření na úrovni pole. Proto uvedení předané hodnoty ručně zpět do DataRow pomáhá zajistit platnost dat, je předaná metodě BLL. Bohužel silně typované třídy DataRow vygenerované Visual Studio nepoužívejte typy s možnou hodnotou Null. Místo toho k označení, že by měla odpovídat konkrétní DataColumn v DataRow `NULL` databáze hodnotu musí používáme `SetColumnNameNull()` metoda.

V `UpdateProduct` nám nejdřív načíst v rámci produktu aktualizovat pomocí `GetProductByProductID(productID)`. Když toto se může zdát jako nepotřebné cesty k databázi, bude prokázat smysl v budoucnu návodů, které prozkoumat optimistickou metodu souběžného tuto Navíc cestu. Optimistickou metodu souběžného je technika zajistit, že dva uživatelé, kteří pracují současně na stejná data náhodnému přepsání své změny. Metodou celý záznam také usnadňuje vytvoření metody aktualizace v BLL, který změnit, jenom podmnožinu sloupců DataRow. Když jsme prozkoumat `SuppliersBLL` třída jsme tyto příklady najdete.

Nakonec, Všimněte si, že `ProductsBLL` třída má [DataObject atribut](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) na něho použít ( `[System.ComponentModel.DataObject]` syntaxe bezprostředně před Class – příkaz v horní části souboru) a metody mají [ Atributy DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). `DataObject` Atribut určí třída jako vhodný pro vytvoření vazby na objekt [ovládacího prvku ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), zatímco `DataObjectMethodAttribute` označuje cílem této metody. Jak jsme budete vidět v budoucích kurzy, technologii ASP.NET 2.0 ObjectDataSource usnadňuje deklarativně přístup k datům z třídy. Chcete-li filtrovat seznam možných třídy k vytvoření vazby ObjectDataSource průvodce, ve výchozím nastavení pouze tyto třídy, které jsou označeny jako `DataObjects` se zobrazují v rozevíracím seznamu v průvodci. `ProductsBLL` Třída bude fungovat stejně dobře bez těchto atributů, ale jejich přidáním usnadňuje práci s v Průvodci ObjectDataSource.

## <a name="adding-the-other-classes"></a>Přidávání dalších tříd

Pomocí `ProductsBLL` třída dokončení, ještě musíme přidat třídy pro práci s kategorií, Dodavatelé a zaměstnanci. Za chvíli vytvořit následující třídy a metody pomocí koncepty z výše uvedeném příkladu:

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

Jedna metoda vhodné poznamenat, je `SuppliersBLL` třídy `UpdateSupplierAddress` metoda. Tato metoda poskytuje rozhraní pro aktualizaci jenom informace o adrese dodavatele. Interně tato metoda čtení `SupplierDataRow` objekt pro zadaný `supplierID` (pomocí `GetSupplierBySupplierID`), nastaví její vlastnosti související s adres a pak zavolá do `SupplierDataTable`na `Update` metoda. `UpdateSupplierAddress` Metoda následovně:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample2.cs)]

Naleznete v tomto článku stahování pro moje dokončení implementace třídy BLL.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Krok 2: Přístup prostřednictvím třídy BLL typové datové sady

Z prvního kurzu jsme viděli příklady pracovat přímo s datovou sadou zadali prostřednictvím kódu programu, ale s přidáním systému naše BLL třídy, prezentační vrstvou by měla fungovat proti BLL místo toho. V `AllProducts.aspx` příklad z první kurz `ProductsTableAdapter` byl použit k vytvoření vazby seznam produktů GridView, jak je znázorněno v následujícím kódu:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample3.cs)]

Pokud chcete používat nové BLL třídy, všechny které je potřeba změnit je jednoduše nahradit první řádek kódu `ProductsTableAdapter` objektu s `ProductBLL` objektu:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample4.cs)]

Třídy BLL můžete přistupovat pomocí ObjectDataSource také deklarativně (jak je můžete typové datové sady). Jsme budete mít hovoříte o ObjectDataSource podrobněji v následujících kurzech.


[![Zobrazí se seznam produktů v GridView](creating-a-business-logic-layer-cs/_static/image4.png)](creating-a-business-logic-layer-cs/_static/image3.png)

**Obrázek 3**: produkty v seznamu se zobrazí v GridView ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-business-logic-layer-cs/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Krok 3: Přidání ověření na úrovni pole do třídy DataRow

Ověření na úrovni pole jsou kontroly, které se vztahují na hodnoty vlastností objektů obchodní při vložení nebo aktualizaci. Některá pravidla ověření na úrovni pole pro produkty patří:

- `ProductName` Pole musí být 40 znaků nebo méně
- `QuantityPerUnit` Pole musí být 20 znaků nebo méně
- `ProductID`, `ProductName`, A `Discontinued` jsou povinná pole, ale všechny ostatní pole jsou volitelná
- `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, A `ReorderLevel` pole musí být větší než nebo rovna hodnotě nula.

Tato pravidla můžete a by měla být vyjádřena na úrovni databáze. Limitu znak `ProductName` a `QuantityPerUnit` pole jsou zachyceny datové typy těchto sloupců `Products` tabulky (`nvarchar(40)` a `nvarchar(20)`, v uvedeném pořadí). Zda jsou povinné a nepovinné pole jsou vyjádřeny ve sloupci tabulky databáze umožňuje `NULL` s. Čtyři [omezení check](https://msdn.microsoft.com/library/ms188258.aspx) existují, ujistěte se, že pouze hodnoty větší než nebo rovna hodnotě nula provádět ji do `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, nebo `ReorderLevel` sloupce.

Kromě vynucování tato pravidla na databázi by také měly být vynucovat na úrovni datové sady. Ve skutečnosti se pro každý DataTable sadu objektů DataColumns zachytit již délka pole a zda je hodnota požadované nebo volitelné. Pokud chcete zobrazit stávající ověření na úrovni pole automaticky poskytnuty, přejděte na návrháře DataSet, vyberte pole z jednoho z DataTables a potom přejděte do okna vlastností. Jak ukazuje obrázek 4, `QuantityPerUnit` DataColumn v `ProductsDataTable` má maximální délku 20 znaků a povolit `NULL` hodnoty. Pokud jsme pokus o nastavení `ProductsDataRow`na `QuantityPerUnit` vlastnost na hodnotu řetězce, který je delší než 20 znaků `ArgumentException` bude vyvolána.


[![Sloupci DataColumn poskytuje základní ověření na úrovni pole](creating-a-business-logic-layer-cs/_static/image7.png)](creating-a-business-logic-layer-cs/_static/image6.png)

**Obrázek 4**: DataColumn poskytuje základní ověření na úrovni pole ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-business-logic-layer-cs/_static/image8.png))


Bohužel jsme nelze zadat rozsah kontroly, jako `UnitPrice` hodnota musí být větší než nebo rovna hodnotě nula prostřednictvím okna Vlastnosti. Chcete-li provést tento typ ověření na úrovni pole, je potřeba vytvořit obslužnou rutinu události pro element DataTable na [columnchanging –](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) událostí. Jak je uvedeno v [předchozí kurzu](creating-a-data-access-layer-cs.md), datové sady, DataTables a DataRow objekty vytvořené typové datové sady je možné rozšířit pomocí částečné třídy. Touto technikou, můžeme vytvořit `ColumnChanging` obslužné rutiny události pro `ProductsDataTable` třídy. Začněte vytvořením třídy v `App_Code` složku s názvem `ProductsDataTable.ColumnChanging.cs`.


[![Přidejte novou třídu do složky App_Code](creating-a-business-logic-layer-cs/_static/image10.png)](creating-a-business-logic-layer-cs/_static/image9.png)

**Obrázek 5**: Přidání nové třídy do `App_Code` složky ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-business-logic-layer-cs/_static/image11.png))


Dále vytvořte obslužnou rutinu události pro `ColumnChanging` událost, která zajistí, že `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, a `ReorderLevel` hodnoty ve sloupcích (není-li `NULL`) jsou větší než nebo rovna hodnotě nula. Pokud tyto sloupce je mimo rozsah, throw `ArgumentException`.

ProductsDataTable.ColumnChanging.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample5.cs)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Krok 4: Vlastní obchodní pravidla přidávat do BLL třídy

Kromě ověření na úrovni pole můžou být vysoké úrovně vlastní obchodní pravidla, které se týkají různých entit nebo koncepty není vyjádřit kombinací na úrovni jeden sloupec, jako například:

- Pokud už není podporovaný produktu, jeho `UnitPrice` nelze aktualizovat
- Zaměstnance zemi, kde bydlíte musí být stejná jako jejich manager země, kde bydlíte
- Produkt nelze zastaví, pokud se jedná pouze produktu, dodavatel

Třídy BLL by měl obsahovat kontroly k zajištění dodržování obchodních pravidel aplikace. Tyto kontroly lze přidat přímo do metod, na které se vztahují.

Představte si, že naše obchodní pravidla určují, že produkt nelze označit – starší formáty Pokud byl produkt pouze od jiného dodavatele daného. To znamená pokud produkt *X* byla pouze produktu jsme zakoupenému od dodavatele *Y*, jsme nelze označit *X* jako zastaví; Pokud však dodavatele *Y*nám součástí tři produkty, *A*, *B*, a *C*, jsme může označte všechny a všechny tyto jako zastaveny. Vždy nejsou zarovnány liché obchodní pravidlo, ale obchodní pravidla a běžné smysl!

K vynucení toto obchodní pravidlo v `UpdateProducts` jsme by měl začínat kontrolou, pokud metoda `Discontinued` byla nastavena na `true` a pokud ano, by říkáme `GetProductsBySupplierID` k určení, kolik produkty jsme zakoupenému od dodavatele tohoto produktu. Pokud jenom jeden produkt zakoupen z tohoto dodavatele, jsme throw `ApplicationException`.


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample6.cs)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Zpracování chyb při ověřování v prezentační vrstvě

Při volání BLL z prezentační vrstvou jsme rozhodnout, zda chcete pokus o zpracování všech výjimek, které může být vyvolána, nebo nechte je bublinový až ASP.NET (který vyvolá `HttpApplication`na `Error` událostí). Ke zpracování výjimky při práci s BLL prostřednictvím kódu programu, můžeme použít [try... catch](https://msdn.microsoft.com/library/0yd65esw.aspx) bloku, jak ukazuje následující příklad:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample7.cs)]

Ukážeme v budoucnosti kurzy, zpracování výjimek, které vyvolat z BLL při použití dat webové ovládacího prvku pro vkládání, aktualizaci, nebo odstranění dat mohou být ošetřeny přímo do obslužné rutiny události a nutnosti zabalení kód v `try...catch` bloky.

## <a name="summary"></a>Souhrn

Dobře navrženou aplikace je vytvořený do různých vrstev, z nichž každý zapouzdří konkrétní roli. Z prvního kurzu této série článku jsme vytvořili Data Access Layer pomocí typové datové sady; v tomto kurzu jsme vytvořili vrstvu obchodní logiky jako řadu třídy v naší aplikaci `App_Code` složky, která volají do našich DAL. BLL implementuje úroveň pole a obchodní logiku pro naši aplikaci. Kromě vytvoření samostatné BLL, jako jsme to udělali v tomto kurzu je další možností rozšířit TableAdapters metody prostřednictvím částečné třídy. Však touto technikou neumožňuje nám přepsat existující metody ani nemá ho oddělit naše DAL a naše BLL řádně přístupů, které jsme si prováděné v tomto článku.

DAL a BLL dokončení je připraven ke spuštění na našem prezentační vrstvy. V [další kurz](master-pages-and-site-navigation-cs.md) jsme budete trvat stručný detour od data přístup témat a definovat konzistentní stránky rozložení pro používání v rámci kurzů k.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu bylo Liz Shulok, společnosti Dennis Patterson, Carlos Santos a Hilton Giesenow. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](creating-a-data-access-layer-cs.md)
> [další](master-pages-and-site-navigation-cs.md)
