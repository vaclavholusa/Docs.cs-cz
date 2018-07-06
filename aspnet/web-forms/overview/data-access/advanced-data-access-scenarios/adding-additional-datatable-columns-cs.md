---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: Přidávání sloupců do další tabulky DataTable (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Při použití třídy TableAdapter průvodce k vytvoření datové sady typu, odpovídající objekt DataTable obsahuje sloupce vrácené dotazem hlavní databáze. Ale existovat...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 4b99b1c01056b8e06e925eca65371a90d2831326
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813262"
---
<a name="adding-additional-datatable-columns-c"></a>Přidávání sloupců do další tabulky DataTable (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip) nebo [stahovat PDF](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> Při použití třídy TableAdapter průvodce k vytvoření datové sady typu, odpovídající objekt DataTable obsahuje sloupce vrácené dotazem hlavní databáze. Existují situace, kdy objekt DataTable musí obsahovat další sloupce. V tomto kurzu jsme dozvíte, proč se doporučuje uložené procedury, až budeme potřebovat další sloupců do tabulky DataTable.


## <a name="introduction"></a>Úvod

Při přidávání objektu typu TableAdapter k datové sadě zadán, odpovídající schématu s DataTable je určena s hlavním dotazu objektu TableAdapter. Například, pokud hlavním dotazem datová pole *A*, *B*, a *C*, objekt DataTable bude mít tři odpovídající sloupce s názvem *A*, *B*, a *C*. Kromě jeho hlavním dotazu objektu TableAdapter může obsahovat další dotazy, které vracejí možná podmnožinu dat na základě některých parametrů. Například, navíc k `ProductsTableAdapter` s hlavním dotazu, který vrací informace o všech produktů, také obsahuje metody, jako je `GetProductsByCategoryID(categoryID)` a `GetProductByProductID(productID)`, který vrací informace určitý produkt na základě zadaného parametru.

Model s schématu s DataTable odrážejí s hlavním dotazu objektu TableAdapter funguje dobře v případě, že všechny metody třídy TableAdapter s vrátí stejnou nebo méně datových polí než procesory zadané v hlavním dotazu. Pokud TableAdapter metoda musí vracet další datová pole, pak jsme byste rozšířit schéma objektu DataTable s odpovídajícím způsobem. V [Master/Detail, použití odrážkovém seznamu v hlavních záznamů s podrobnostmi v prvku DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) kurzu jsme přidali metodu `CategoriesTableAdapter` vrácená `CategoryID`, `CategoryName`, a `Description` datová pole, které jsou definovány v Hlavní dotaz plus `NumberOfProducts`, další datové pole, která hlásí počet produktů, které jsou spojené s každou kategorii. Přidali jsme ručně nový sloupec, `CategoriesDataTable` k zachycení `NumberOfProducts` datové pole hodnotu z této nové metody.

Jak je popsáno v [nahrávání souborů](../working-with-binary-files/uploading-files-cs.md) výukový program, skvělé třeba dávat pozor pomocí instancí TableAdapter, kde použít SQL příkazy ad-hoc a metod, jejichž datová pole nejsou přesně shodné hlavního dotazu. Pokud je znovu spustit Průvodce konfigurací TableAdapter, bude se aktualizovat všechny metody třídy TableAdapter s tak, aby jejich seznam polí dat odpovídá hlavního dotazu. Všechny metody se seznamy vlastní sloupec v důsledku toho se vrátit do seznamu sloupců hlavního dotazu s a nevrací očekávaná data. Tento problém nevzniká při použití uložených procedur.

V tomto kurzu se podíváme na tom, jak rozšířit schéma objektu DataTable s zahrnout další sloupce. Z důvodu brittleness TableAdapter při použití příkazů jazyka SQL ad-hoc v tomto kurzu budeme používat uložené procedury. Odkazovat [vytváří se nové uložené procedury, pro zadané datové sady s objekty TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) a [použití existující uložené procedury, pro zadané datové sady s objekty TableAdapter](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs) kurzy pro další informace o konfigurace objektu typu TableAdapter na pouze pomocí uložených procedur.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Krok 1: Přidání`PriceQuartile`sloupec, který se`ProductsDataTable`

V *vytváří se nové uložené procedury, pro zadané datové sady s objekty TableAdapter* kurzu jsme vytvořili datovou sadu typu s názvem `NorthwindWithSprocs`. Tato datová sada v současné době obsahuje dva DataTables: `ProductsDataTable` a `EmployeesDataTable`. `ProductsTableAdapter` Má následující tři metody:

- `GetProducts` -Hlavní dotaz, který vrátí všechny záznamy z `Products` tabulky
- `GetProductsByCategoryID(categoryID)` -Vrátí všechny produkty se zadaným *categoryID*.
- `GetProductByProductID(productID)` -Vrátí konkrétní produkt se zadaným *productID*.

Vrátí stejnou sadu datových polí, a to všech sloupců z hlavního dotazu a další dvě metody všechny `Products` tabulky. Neexistují žádné korelační poddotazy nebo `JOIN` s načítání souvisejících dat ze `Categories` nebo `Suppliers` tabulky. Proto `ProductsDataTable` nemá odpovídající sloupec pro každé pole v `Products` tabulky.

Pro účely tohoto kurzu vám umožňují s přidejte metodu k `ProductsTableAdapter` s názvem `GetProductsWithPriceQuartile` , který vrátí všechny produkty. Kromě datových polí standardní produkty `GetProductsWithPriceQuartile` bude také obsahovat `PriceQuartile` datové pole, která určuje, za které QUARTIL cena produktu s spadá. Například, budou mít tyto produkty, jejichž ceny jsou uvedeny v nejdražší 25 % `PriceQuartile` hodnotu 1, zatímco ty, jejichž ceny spadat do posledních 25 % bude mít hodnotu 4. Než jsme starat o vytvoření uložené procedury tyto informace mají vracet, ale nejprve musíte aktualizovat `ProductsDataTable` zahrnout sloupec obsahoval `PriceQuartile` výsledky při `GetProductsWithPriceQuartile` metoda se používá.

Otevřít `NorthwindWithSprocs` datovou sadu a klikněte pravým tlačítkem na `ProductsDataTable`. V místní nabídce zvolte možnost Přidat a pak vyberte sloupec.


[![Přidat nový sloupec ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**Obrázek 1**: přidejte nový sloupec, `ProductsDataTable` ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image3.png))


Nový sloupec se přidá do DataTable s názvem Sloupec1 typu `System.String`. Potřebujeme aktualizovat tento sloupec s názvem PriceQuartile a jeho typ ke `System.Int32` vzhledem k tomu, že se použije k uložení číslo mezi 1 a 4. Vyberte sloupec nově přidané `ProductsDataTable` a v okně Vlastnosti nastavte `Name` vlastnost PriceQuartile a `DataType` vlastnost `System.Int32`.


[![Nastavte nový sloupec s název a datový typ vlastnosti](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**Obrázek 2**: nastavte nový sloupec s `Name` a `DataType` vlastnosti ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image6.png))


Jak je vidět na obrázku 2, jsou další vlastnosti, které můžete nastavit, jako jsou hodnoty ve sloupci určuje, zda musí být jedinečné, pokud sloupec je sloupec s automatickým krokem, zda databáze `NULL` hodnoty jsou povoleny a tak dále. Ponechte tyto hodnoty výchozí nastavení.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Krok 2: Vytvoření`GetProductsWithPriceQuartile`– metoda

Teď, když `ProductsDataTable` byl aktualizován na zahrnují `PriceQuartile` sloupce, budeme připravení vytvořit `GetProductsWithPriceQuartile` metoda. Začněte tím, že pravým tlačítkem myši na TableAdapter a zvolením přidat dotaz v místní nabídce. Tím se vyvolá průvodce konfigurací dotazu TableAdapter, který nejprve nám vyzve k tom, zda chceme použít SQL příkazy ad-hoc nebo nové nebo existující uložené procedury. Protože jsme zadávat t ještě nemáte uložené procedury, která vrací data QUARTIL cena, umožní s povolit TableAdapter vytvořte tuto uloženou proceduru pro nás. Výběr možnosti vytvořit nové uložené procedury a klikněte na tlačítko Další.


[![Dáte pokyn, aby TableAdapter průvodce k vytvoření uložené procedury pro nás](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**Obrázek 3**: dáte pokyn, aby průvodce TableAdapter vytvořte uložené procedury pro USA ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image9.png))


Na následující obrazovce je znázorněno na obrázku 4, zobrazí Průvodce požadavek nám jaký typ dotazu přidat. Protože `GetProductsWithPriceQuartile` metoda vrátí všechny sloupce a záznamy ze `Products` tabulku, vyberte, které vrací řádky možnost a klikněte na tlačítko Další.


[![Dotaz bude příkaz SELECT tohoto vrátí více řádků](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**Obrázek 4**: náš dotaz bude `SELECT` příkaz této vrátí více řádků ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image12.png))


Dále jsme se výzva k zadání `SELECT` dotazu. V průvodci zadejte následující dotaz:


[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

Výše uvedeném dotazu používá SQL Server 2005 s novou [ `NTILE` funkce](https://msdn.microsoft.com/library/ms175126.aspx) výsledky rozdělit do čtyř skupin, ve kterém jsou určeny skupiny `UnitPrice` hodnoty seřazeny v sestupném pořadí.

Bohužel Tvůrce dotazů není známo, jak analyzovat `OVER` – klíčové slovo a bude se zobrazovat chyby při analýze výše uvedeném dotazu. Proto výše uvedeném dotazu přímo do textového pole zadejte v Průvodci bez použití editoru dotazů.

> [!NOTE]
> Další informace o ntile povolen a SQL Server 2005 s najdete v dalších funkcí hodnocení [vrácení seřazených výsledků s Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) a [hodnocení funkce v tématu](https://msdn.microsoft.com/library/ms189798.aspx) z [SQL Server 2005 Books Online](https://msdn.microsoft.com/library/ms189798.aspx).


Po zadání `SELECT` dotazů a kliknutí na tlačítko Další, Průvodce výzva k zadání názvu pro uloženou proceduru se vytvoří. Pojmenujte novou úložnou proceduru `Products_SelectWithPriceQuartile` a klikněte na tlačítko Další.


[![Název uložené procedury Products_SelectWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**Obrázek 5**: název uložená procedura `Products_SelectWithPriceQuartile` ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image15.png))


Nakonec jsme se výzva k pojmenování metody TableAdapter. Nechte obou výplně datové tabulky a vrátí objekt DataTable zaškrtnutých políček a název metody `FillWithPriceQuartile` a `GetProductsWithPriceQuartile`.


[![Název TableAdapter s metod a klikněte na tlačítko Dokončit](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**Obrázek 6**: název třídy TableAdapter s metod a kliknutím na tlačítko Dokončit ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image18.png))


S `SELECT` zadaný dotaz a uložených procedur a metody třídy TableAdapter s názvem, klikněte na tlačítko Dokončit dokončete průvodce. V tomto okamžiku se může zobrazit upozornění nebo dvě z Průvodce chci říct, že `OVER` konstrukci SQL nebo příkaz nejsou podporovány. Tato upozornění můžete ignorovat.

Po dokončení průvodce, by měl obsahovat objektu TableAdapter `FillWithPriceQuartile` a `GetProductsWithPriceQuartile` metody a databáze by měl obsahovat uložené procedury s názvem `Products_SelectWithPriceQuartile`. Za chvíli ověřte, že TableAdapter skutečně obsahují tato nová metoda a že uloženou proceduru správně přidán do databáze. Při kontrole databáze, pokud se nezobrazí uloženou proceduru zkuste pravým tlačítkem myši na složku uložené procedury a vyberte aktualizovat.


![Ověřte, že byla přidána nová metoda do TableAdapter](adding-additional-datatable-columns-cs/_static/image19.png)

**Obrázek 7**: Ověřte, že byla přidána nová metoda do TableAdapter


[![Ujistěte se, že databáze obsahuje Products_SelectWithPriceQuartile uložené procedury](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**Obrázek 8**: Ujistěte se, že databáze obsahuje `Products_SelectWithPriceQuartile` uloženou proceduru ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image22.png))


> [!NOTE]
> Jednou z výhod použití uložených procedur místo SQL příkazy ad-hoc je, že znovu spustíte Průvodce konfigurací TableAdapter nezmění sloupec seznamy uložené procedury. To ověřte tak, že pravým tlačítkem myši na TableAdapter, vyberete možnost konfigurace v místní nabídce spusťte průvodce a pak kliknutím na tlačítko Dokončit k jejímu dokončení. Dále přejděte do databáze a zobrazení `Products_SelectWithPriceQuartile` uložené procedury. Všimněte si, že byl změněn jeho seznamu sloupců. Měli jsme používali SQL příkazy ad-hoc, znovu spustíte Průvodce konfigurací TableAdapter by se vrátíte tento dotaz s seznam sloupců tak, aby odpovídaly hlavního dotazu seznamu sloupců, odpadne ntile povolen příkaz z dotazu použitého `GetProductsWithPriceQuartile` metody.


Když vrstvy přístupu k datům s `GetProductsWithPriceQuartile` vyvolání metody, TableAdapter provede `Products_SelectWithPriceQuartile` uložené procedury a přidá řádek do `ProductsDataTable` pro každý záznam vrátil. Datová pole vrácené uložené procedury jsou namapovány na `ProductsDataTable` s sloupce. Vzhledem k tomu, že je `PriceQuartile` data pole vrácené z uložené procedury, jeho hodnota přiřazena `ProductsDataTable` s `PriceQuartile` sloupce.

Pro tyto metody TableAdapter, jejichž dotazy nevrátí `PriceQuartile` datové pole, `PriceQuartile` sloupec s hodnotou je hodnotu zadanou pomocí jeho `DefaultValue` vlastnost. Jak je vidět na obrázku 2, tato hodnota nastavená na `DBNull`, výchozí hodnota. Pokud si přejete jinou výchozí hodnotu, stačí nastavit `DefaultValue` vlastnost odpovídajícím způsobem. Dejte pozor, který `DefaultValue` je hodnota platná zadané sloupce s `DataType` (například `System.Int32` pro `PriceQuartile` sloupec).

V tuto chvíli jsme jste provedli kroky pro přidání dalších sloupců do DataTable. Pokud chcete ověřit, že tento další sloupec funguje podle očekávání, umožní vytvořit stránku ASP.NET, která zobrazí každý produkt s názvem, ceny a cena QUARTIL s. Než to, ale nejprve musíme aktualizovat vrstvy obchodní logiky obsahovat metodu, která volá na s vrstvou DAL `GetProductsWithPriceQuartile` metody. Budeme aktualizovat BLL v dalším kroku v kroku 3 a pak vytvořte stránku ASP.NET v kroku 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Krok 3: Rozšíření vrstvy obchodní logiky

Předtím, než jsme pomocí nové `GetProductsWithPriceQuartile` metoda od prezentační vrstvy jsme měli nejprve přidat odpovídající metody do BLL. Otevřít `ProductsBLLWithSprocs` třídy soubor a přidejte následující kód:


[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

Data načtení metod v, jako jsou `ProductsBLLWithSprocs`, `GetProductsWithPriceQuartile` metoda provede jednoduché volání DAL s odpovídající `GetProductsWithPriceQuartile` metodu a vrátí jeho výsledky.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Krok 4: Zobrazení informací o QUARTIL cena na webové stránce ASP.NET

Přidání knihoven BLL dokončení jsme znovu připravený k vytvoření stránky technologie ASP.NET, který zobrazuje QUARTIL ceny pro každý produkt. Otevřít `AddingColumns.aspx` stránku `AdvancedDAL` složky a GridView přetáhněte z panelu nástrojů do Návrháře nastavení jeho `ID` vlastnost `Products`. Z inteligentních značek GridView s vázat na nového prvku ObjectDataSource s názvem `ProductsDataSource`. Konfigurace ObjectDataSource používat `ProductsBLLWithSprocs` třída s `GetProductsWithPriceQuartile` metody. Vzhledem k tomu, že to bude jen pro čtení mřížky, nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný).


[![Konfigurace ObjectDataSource pomocí třídy ProductsBLLWithSprocs](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**Obrázek 9**: Konfigurace ObjectDataSource k použití `ProductsBLLWithSprocs` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image25.png))


[![Získávání informací o produktech z GetProductsWithPriceQuartile – metoda](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**Obrázek 10**: načíst informace o produktech z `GetProductsWithPriceQuartile` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image28.png))


Po dokončení Průvodce nakonfigurovat zdroj dat, sada Visual Studio automaticky přidá vlastnost BoundField nebo třídě CheckBoxField do prvku GridView. pro každý vrácený metodou datová pole. Jedním z těchto polí je `PriceQuartile`, což je sloupec přidali jsme `ProductsDataTable` v kroku 1.

Upravit pole ovládacího prvku GridView s odebráním všech ale na `ProductName`, `UnitPrice`, a `PriceQuartile` BoundFields. Konfigurace `UnitPrice` Vlastnost BoundField a jeho hodnotu jako měnu naformátovat `UnitPrice` a `PriceQuartile` BoundFields a System center zarovnaný doprava, v uvedeném pořadí. Nakonec aktualizujte zbývající BoundFields `HeaderText` vlastnosti na produkt, ceny a cena QUARTIL, v uvedeném pořadí. Také zaškrtněte políčko Povolit řazení z inteligentních značek s ovládacího prvku GridView.

Po změnách ovládacími prvky GridView a prvku ObjectDataSource s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

Obrázku 11 můžete vidět tuto stránku, když uživatel prostřednictvím prohlížeče. Všimněte si, že na začátku produkty jsou řazeny podle jejich cena v sestupném pořadí u každého produktu přiřazené odpovídající `PriceQuartile` hodnotu. Samozřejmě tato data můžou být řada seřazena podle jiných kritérií se cena sloupec kvartil stále odráží hodnocení produktu s s ohledem na cenu (viz obrázek 12).


[![Produkty jsou řazeny podle jejich cenu](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**Obrázek 11**: The produkty jsou řazeny podle jejich cenu ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image31.png))


[![Produkty jsou řazeny podle svých názvů](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**Obrázek 12**: The produkty jsou řazeny podle svých názvů ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image34.png))


> [!NOTE]
> Po zadání několika řádků kódu jsme mohli rozšířit prvku GridView tak, aby vybarvenými řádky produkt na základě jejich `PriceQuartile` hodnotu. Společnost Microsoft může barva tyto produkty v první kvartil světle zelená, ve druhém QUARTIL žlutě a tak dále. Neváhejte se za chvíli přidat tuto funkci. Pokud potřebujete občerstvit o formátování GridView, obraťte se [vlastní formátování založené na Data](../custom-formatting/custom-formatting-based-upon-data-cs.md) kurzu.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Alternativním přístupem – vytváření jiné třídy TableAdapter

Jak jsme viděli v tomto kurzu se při přidání metody do třídy TableAdapter, který vrací datových polí než ty, které států hlavního dotazu, můžeme přidat odpovídající sloupce do objektu DataTable. Takový přístup, ale funguje dobře jenom v případě, že existují malý počet metod v TableAdapter, které vracejí různých datových polí, a pokud tato alternativní datové pole se neliší o příliš mnoho z hlavního dotazu.

Místo přidávání sloupců do objektu DataTable, můžete místo toho přidat jiné třídy TableAdapter do datové sady, který obsahuje metodu z první třídy TableAdapter, které vracejí různých datových polí. Pro účely tohoto kurzu množstvím než přidávat `PriceQuartile` sloupec, který se `ProductsDataTable` (kde používá se pouze podle `GetProductsWithPriceQuartile` metoda), může do jsme přidali další TableAdapter datovou sadu s názvem `ProductsWithPriceQuartileTableAdapter` , který používá `Products_SelectWithPriceQuartile` uložená postup jako jeho hlavním dotazem. Stránky ASP.NET, které jsou potřeba k získání informací o produktu s QUARTIL cena byste použili `ProductsWithPriceQuartileTableAdapter`, zatímco ty, které ne může pokračovat v používání `ProductsTableAdapter`.

Přidáním nového TableAdapter DataTables zůstanou untarnished a jejich sloupců přesně zrcadlení datová pole vrácené z metody s jejich TableAdapter. Další objekty TableAdapter lze však zavést opakujících se úloh a funkcí. Například, pokud tyto stránky ASP.NET, která se zobrazí `PriceQuartile` sloupec také potřeba poskytnout vložení, aktualizace a odstranění podpory, `ProductsWithPriceQuartileTableAdapter` byste potřebovali mít jeho `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti správně konfigurovat. Zatímco bude zrcadlit tyto vlastnosti `ProductsTableAdapter` s touto konfigurací zavádí další krok. Kromě toho nyní existují dva způsoby, jak aktualizovat, odstranit nebo přidat produkt do databáze - prostřednictvím `ProductsTableAdapter` a `ProductsWithPriceQuartileTableAdapter` třídy.

Zahrnuje stažení pro účely tohoto kurzu `ProductsWithPriceQuartileTableAdapter` třídy v `NorthwindWithSprocs` datovou sadu, která tento alternativní postup znázorňuje.

## <a name="summary"></a>Souhrn

Ve většině scénářů si všechny metody v objektu typu TableAdapter vrátí stejnou sadu datových polí, ale existují situace, kdy konkrétní metody nebo dvou může být zapotřebí vracet další pole. Například v [Master/Detail, použití odrážkovém seznamu v hlavních záznamů s podrobnostmi v prvku DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) kurzu jsme přidali metodu `CategoriesTableAdapter` , kromě datových polí hlavního dotazu s vrátil `NumberOfProducts` pole, které hlásí počet produktů, které jsou spojené s každou kategorii. V tomto kurzu jsme se podívali na přidání metody v `ProductsTableAdapter` vrácená `PriceQuartile` pole kromě hlavního dotazu s datová pole. K zaznamenání dat o další pole vrácené pomocí metod TableAdapter s že potřebujeme přidat odpovídající sloupce do objektu DataTable.

Pokud plánujete ruční přidávání sloupců do DataTable, doporučuje se, že TableAdapter pomocí uložených procedur. Pokud TableAdapter používá příkazy SQL ad-hoc, kdykoli TableAdapter spuštění Průvodce konfigurací všechny metody seznamů polí data obnovit datová pole vráceného hlavním dotazem. Tento problém nerozšiřuje třídu na uložené procedury, což je důvod, proč se doporučují a byly použity v tomto kurzu.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Randym Schmidt, Jacky Goor, Bernadette Leigh a Hilton Giesenow. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](updating-the-tableadapter-to-use-joins-cs.md)
> [další](working-with-computed-columns-cs.md)
