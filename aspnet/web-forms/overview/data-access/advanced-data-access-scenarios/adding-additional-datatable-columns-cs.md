---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: "Přidání další DataTable sloupců (C#) | Microsoft Docs"
author: rick-anderson
description: "Při použití Průvodce nastavením TableAdapter vytvoření typové datové sady, obsahuje odpovídající DataTable sloupců vrácených dotazem hlavní databáze. Ale zde..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 0b1fe8d2e376065aed8d94b1267910bd1f7e5bd0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-additional-datatable-columns-c"></a>Přidání další DataTable sloupců (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip) nebo [stáhnout PDF](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> Při použití Průvodce nastavením TableAdapter vytvoření typové datové sady, obsahuje odpovídající DataTable sloupců vrácených dotazem hlavní databáze. Ale v určitých případech, kdy DataTable musí zahrnovat další sloupce. V tomto kurzu jsme zjistěte, proč se doporučují uložené procedury, když budeme potřebovat další DataTable sloupce.


## <a name="introduction"></a>Úvod

Při přidávání TableAdapter typové datové sady, odpovídající schéma s DataTable je určen podle hlavní dotazu TableAdapter s. Například pokud hlavní dotaz vrátí datová pole *A*, *B*, a *C*, DataTable bude mít tři odpovídající sloupce s názvem *A*, *B*, a *C*. Kromě jeho hlavním dotazu TableAdapter může obsahovat další dotazy, které možná, vrátí podmnožinu dat na základě některé parametru. Například k `ProductsTableAdapter` s hlavní dotaz, který vrací informace o všech produktů, také obsahuje metody, třeba `GetProductsByCategoryID(categoryID)` a `GetProductByProductID(productID)`, který vrátí informace o konkrétní produktu založené na zadaný parametr.

Model s schéma s DataTable odráží hlavní dotazu TableAdapter s funguje dobře v případě, že všechny metody TableAdapter s vrátí stejné nebo méně datových polí než procesory zadané v hlavním dotazu. Pokud metoda TableAdapter musí vrátit další datová pole, pak jsme by měl rozšířit schéma DataTable s odpovídajícím způsobem. V [hlavní/podrobností s odrážkami seznamu v hlavní záznamů pomocí DataList podrobnosti](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) kurzu jsme přidali metodu pro `CategoriesTableAdapter` vrácená `CategoryID`, `CategoryName`, a `Description` datová pole definovaná v Hlavní dotaz plus `NumberOfProducts`, na pole další data, která hlášena počet produktů, které jsou spojené s každou kategorii. Jsme ručně přidat nový sloupec na `CategoriesDataTable` k zachycení `NumberOfProducts` data pole hodnotu z tato nová metoda.

Jak je popsáno v [nahrávání souborů](../working-with-binary-files/uploading-files-cs.md) kurz, skvělé musí dát pozor s TableAdapters, kde používat příkazy SQL ad-hoc a metod, jejichž datová pole neodpovídají přesněji hlavní dotazu. Pokud je znovu spustit Průvodce konfigurací TableAdapter, bude všech metod TableAdapter s aktualizovat tak, aby jejich seznam polí dat odpovídá hlavní dotazu. Žádné metody pomocí vlastní sloupce seznamů v důsledku toho se vrátit do seznamu sloupců hlavní dotazu s a nevrací očekávaná data. Tento problém nevzniká při použití uložených procedur.

V tomto kurzu se podíváme na postup rozšíření schématu s DataTable přidání dalších sloupců. Z důvodu brittleness z TableAdapter při použití příkazů SQL ad-hoc v tomto kurzu použijeme uložené procedury. Odkazovat [vytváření nové uložených procedur, pro typové datové sady s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) a [použití existující uložené procedury, pro typové datové sady s TableAdapters](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs) kurzy pro další informace o Konfigurace TableAdapter používat uložené procedury.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Krok 1: Přidání`PriceQuartile`sloupec, který se`ProductsDataTable`

V *vytváření nové uložených procedur, pro typové datové sady s TableAdapters* kurzu jsme vytvořili zadali datovou sadu s názvem `NorthwindWithSprocs`. Tato datová sada obsahuje teď dvě DataTables: `ProductsDataTable` a `EmployeesDataTable`. `ProductsTableAdapter` Má následující tři metody:

- `GetProducts`-main dotazu, která vrátí všechny záznamy z `Products` tabulky
- `GetProductsByCategoryID(categoryID)`-Vrátí všechny produkty se zadaným *categoryID*.
- `GetProductByProductID(productID)`-vrací konkrétní produkt se zadaným *productID*.

Hlavní dotaz a dva další metody všechny vrátit stejnou sadu datová pole, konkrétně všechny sloupce z `Products` tabulky. Neexistují žádné korelační poddotazy nebo `JOIN` s stahování dat v relaci z `Categories` nebo `Suppliers` tabulky. Proto `ProductsDataTable` má odpovídající sloupec pro každé pole v `Products` tabulky.

V tomto kurzu umožňují s přidejte metodu k `ProductsTableAdapter` s názvem `GetProductsWithPriceQuartile` která vrací všechny produkty. Kromě datová pole standardní produktu `GetProductsWithPriceQuartile` budou taky zahrnovat `PriceQuartile` datové pole, které označuje, pod které QUARTIL cena produktu s spadá. Například, budou mít tyto produkty, jejichž ceny jsou v nejnákladnější 25 % `PriceQuartile` hodnotu 1, při jejichž ceny spadají v posledních 25 % bude mít hodnotu 4. Jsme starat o vytváření uložené procedury, která vrátí tyto informace, ale nejprve musíme aktualizovat `ProductsDataTable` aby zahrnovaly sloupec pro uložení `PriceQuartile` výsledky při `GetProductsWithPriceQuartile` metoda se používá.

Otevřete `NorthwindWithSprocs` datovou sadu a klikněte pravým tlačítkem na `ProductsDataTable`. Zvolte možnost přidat v místní nabídce a potom vyberte sloupec.


[![Přidejte nový sloupec ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**Obrázek 1**: Přidat nový sloupec, který má `ProductsDataTable` ([Kliknutím zobrazit obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image3.png))


Tím se přidá nový sloupec s názvem Column1 typu DataTable `System.String`. Je potřeba aktualizovat tento sloupec s název PriceQuartile a její typ `System.Int32` vzhledem k tomu, že se použije k uložení číslo mezi 1 a 4. Vyberte sloupec nově přidané v `ProductsDataTable` a v okně vlastnosti nastavit `Name` vlastnost PriceQuartile a `DataType` vlastnost `System.Int32`.


[![Nastavit nový sloupec s název a vlastnosti datového typu](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**Obrázek 2**: nastavte nový sloupec s `Name` a `DataType` vlastnosti ([Kliknutím zobrazit obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image6.png))


Jak je vidět na obrázku 2, existují další vlastnosti, které lze nastavit, například zda hodnoty ve sloupci musí být jedinečné, pokud se sloupec je sloupec s automatickým krokem, zda databáze `NULL` hodnoty jsou povoleny a tak dále. Ponechte tyto hodnotami nastavenými na jejich výchozí hodnoty.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Krok 2: Vytvoření`GetProductsWithPriceQuartile`– metoda

Teď, když `ProductsDataTable` byl aktualizovaný, aby zahrnují `PriceQuartile` sloupce, jsme připraveni vytvořit `GetProductsWithPriceQuartile` metoda. Start pravým tlačítkem myši na TableAdapter a zvolením přidat dotazu v místní nabídce. Otevře Průvodce konfigurací dotazu TableAdapter, nám nejdřív vyzve, jestli chcete používat příkazy SQL ad-hoc nebo nové nebo existující uložené procedury. Vzhledem k tomu, že jsme nejsou zobrazeny t ještě mít uložené procedury, jež vrátí data o QUARTIL ceníku, umožní s povolit TableAdapter vytvořit tuto uloženou proceduru pro nás. Vyberte možnost vytvořit novou uložené procedury a klikněte na tlačítko Další.


[![Průvodce nastavením TableAdapter vytvoření uložené procedury pro nás, požádejte](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**Obrázek 3**: dá pokyn Průvodce nastavením TableAdapter vytvořte uložené procedury pro nás ([Kliknutím zobrazit obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image9.png))


Na další obrazovce znázorněno na obrázku 4 Průvodce zobrazí nám jaký typ dotazu pro přidání. Vzhledem k tomu `GetProductsWithPriceQuartile` metoda vrací všechny sloupce a záznamy ze `Products` tabulky, vyberte, která vrací řádky možnost a kliknutím na tlačítko Další.


[![Naše dotazu bude příkaz SELECT této vrátí více řádků](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**Obrázek 4**: Naše dotazu bude `SELECT` příkaz tohoto vrátí více řádků ([Kliknutím zobrazit obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image12.png))


Potom jsme se zobrazí výzva k `SELECT` dotazu. V průvodci zadejte následující dotaz:


[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

Výše uvedené dotaz používá SQL Server 2005 s novou [ `NTILE` funkce](https://msdn.microsoft.com/en-us/library/ms175126.aspx) k rozdělení výsledky do čtyř skupin, kde jsou určeny skupiny `UnitPrice` hodnoty seřazené v sestupném pořadí.

Bohužel Tvůrce dotazů nebude vědět, jak analyzovat `OVER` – klíčové slovo a zobrazí chybu při analýze výše uvedeném dotazu. Proto výše uvedeném dotazu přímo do textového pole zadejte v Průvodci bez použití Tvůrce dotazů.

> [!NOTE]
> Další informace o NTILE a SQL Server 2005 s najdete v dalších funkcí hodnocení [vrácení seřazeny výsledků s Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) a [řazení funkce oddílu](https://msdn.microsoft.com/en-us/library/ms189798.aspx) z [SQL Server 2005 Books Online](https://msdn.microsoft.com/en-us/library/ms189798.aspx).


Po zadání `SELECT` dotazu a kliknutím na tlačítko Další, tento průvodce zobrazí, abychom mohli poskytovat název uložené procedury vytvoří. Název nové uložené procedury `Products_SelectWithPriceQuartile` a klikněte na tlačítko Další.


[![Název uložené procedury Products_SelectWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**Obrázek 5**: název uložená procedura `Products_SelectWithPriceQuartile` ([Kliknutím zobrazit obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image15.png))


Nakonec jsme se výzva k název metod TableAdapter. Nechte obě výplně DataTable a vrátit zaškrtnutí políček DataTable a název metody `FillWithPriceQuartile` a `GetProductsWithPriceQuartile`.


[![Název TableAdapter s metody a klikněte na tlačítko Dokončit.](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**Obrázek 6**: název TableAdapter s metod a kliknutím na tlačítko Dokončit ([Kliknutím zobrazit obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image18.png))


Pomocí `SELECT` zadaný dotaz a uložené procedury a metod TableAdapter s názvem, klikněte na tlačítko Dokončit dokončete průvodce. V tomto okamžiku můžete obdržet varování nebo dvě z Průvodce oznámením, že `OVER` konstrukce SQL nebo příkaz není podporován. Tato upozornění můžete ignorovat.

Po dokončení průvodce, by měla obsahovat TableAdapter `FillWithPriceQuartile` a `GetProductsWithPriceQuartile` metody a databáze by měla obsahovat uložené procedury s názvem `Products_SelectWithPriceQuartile`. Ověřte, že TableAdapter skutečně obsahovat tato nová metoda a zda uložené procedury správně přidán do databáze chvíli trvat. Při kontrole databáze, pokud se nezobrazí uložené procedury zkuste kliknete pravým tlačítkem na složku, uložené procedury a výběr aktualizace.


![Ověřte, že přidala nová metoda TableAdapter](adding-additional-datatable-columns-cs/_static/image19.png)

**Obrázek 7**: Ověřte, že přidala nová metoda TableAdapter


[![Zajistěte, aby obsahoval databázi Products_SelectWithPriceQuartile uložené procedury](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**Obrázek 8**: Zajistěte, aby databáze obsahuje `Products_SelectWithPriceQuartile` uloženou proceduru ([Kliknutím zobrazit obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image22.png))


> [!NOTE]
> Jednou z výhod použití uložených procedur místo ad-hoc příkazů SQL je, že znovu spustíte Průvodce konfigurací TableAdapter nezmění sloupec seznamy uložené procedury. To ověřte tak, že pravým tlačítkem myši na TableAdapter, možnosti konfigurace v místní nabídce spusťte průvodce a pak kliknutím na tlačítko Dokončit dokončete ji. Dále přejděte do okna databáze a zobrazení `Products_SelectWithPriceQuartile` uložené procedury. Všimněte si, jeho seznamu sloupců nebyl změněn. Měli jsme byl pomocí příkazů SQL ad-hoc, znovu spustíte Průvodce konfigurací TableAdapter by se vrátíte tohoto seznamu sloupců dotazu s tak, aby odpovídaly seznamu sloupců dotazu hlavní a odpadne příkaz NTILE z dotazu použitého v `GetProductsWithPriceQuartile` metoda.


Když Data Access Layer s `GetProductsWithPriceQuartile` metoda je volána, provede TableAdapter `Products_SelectWithPriceQuartile` uložené procedury a přidá řádek do `ProductsDataTable` pro každý záznam vrácené. Datová pole vrácené uložené procedury jsou namapované na `ProductsDataTable` s sloupce. Vzhledem k tomu, že je `PriceQuartile` datové pole, kterou vrátil uloženou proceduru, jeho hodnota je přiřazena k `ProductsDataTable` s `PriceQuartile` sloupce.

Pro tyto metody TableAdapter, jejichž dotazy nevrátí `PriceQuartile` pole dat, `PriceQuartile` hodnotu zadanou pomocí, je hodnota sloupce s jeho `DefaultValue` vlastnost. Jak je vidět na obrázku 2, tato hodnota nastavena na `DBNull`, výchozí hodnota. Pokud si přejete jinou výchozí hodnotu, jednoduše nastavit `DefaultValue` vlastnost odpovídajícím způsobem. Dejte pozor, který `DefaultValue` hodnota je platná zadaný sloupec s `DataType` (tj, `System.Int32` pro `PriceQuartile` sloupce).

V tuto chvíli jsme jste udělali potřebné kroky pro přidání dalších sloupců do DataTable. Pokud chcete ověřit, že tento další sloupec funguje podle očekávání, umožní vytvořit stránku ASP.NET, který zobrazuje každou s název produktu, cenu a cena QUARTIL s. To ale nejprve musíme aktualizovat vrstvu obchodní logiky obsahovat metodu, která volá, aby se DAL s `GetProductsWithPriceQuartile` metoda. Jsme se aktualizovat BLL v dalším kroku v kroku 3 a pak vytvořte stránku ASP.NET v kroku 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Krok 3: Rozšířit vrstvu obchodní logiky

Než budeme používat nové `GetProductsWithPriceQuartile` metoda od prezentační vrstvy, musí nejprve přidáme odpovídající metodu k BLL. Otevřete `ProductsBLLWithSprocs` třídy souboru a přidejte následující kód:


[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

Jako další data načtení metody v `ProductsBLLWithSprocs`, `GetProductsWithPriceQuartile` metoda provede jednoduché volání DAL s odpovídající `GetProductsWithPriceQuartile` metodu a vrátí jeho výsledky.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Krok 4: Zobrazení informací QUARTIL cena na webovou stránku ASP.NET

Díky přidání BLL dokončete jsme re připravené k vytvoření stránky ASP.NET, která zobrazuje QUARTIL ceny pro každý produkt. Otevřete `AddingColumns.aspx` stránku `AdvancedDAL` složku a přetáhněte GridView z panelu nástrojů na návrháře nastavení jeho `ID` vlastnost, která má `Products`. Ze inteligentní značky s GridView navázat jej na nové ObjectDataSource s názvem `ProductsDataSource`. Konfigurace ObjectDataSource používat `ProductsBLLWithSprocs` třídu s `GetProductsWithPriceQuartile` metoda. Vzhledem k tomu, že to bude mřížka jen pro čtení, nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný).


[![Konfigurace ObjectDataSource použití třídy ProductsBLLWithSprocs](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**Obrázek 9**: Konfigurace ObjectDataSource pro použití `ProductsBLLWithSprocs` – třída ([Kliknutím zobrazit obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image25.png))


[![Načtení informací o produktu z metody GetProductsWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**Obrázek 10**: načíst informace o produktu z `GetProductsWithPriceQuartile` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image28.png))


Po dokončení průvodce Konfigurace zdroje dat, Visual Studio automaticky přidá BoundField nebo vlastnost CheckBoxField do GridView pro každé datové pole vrácené metodou. Jedno z těchto polí dat je `PriceQuartile`, což je sloupec jsme přidali do `ProductsDataTable` v kroku 1.

Upravte pole s GridView, odebrání všech ale na `ProductName`, `UnitPrice`, a `PriceQuartile` BoundFields. Konfigurace `UnitPrice` BoundField a jeho hodnotu jako měny naformátovat `UnitPrice` a `PriceQuartile` BoundFields a center zarovnána doprava, v uvedeném pořadí. Nakonec aktualizujte zbývající BoundFields `HeaderText` vlastností produktu, ceny a ceny QUARTIL v uvedeném pořadí. Také zaškrtněte políčko Povolit řazení z GridView s inteligentním.

Po tyto úpravy GridView a ObjectDataSource s deklarativní by měl vypadat následovně:


[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

Obrázek 11 zobrazí tato stránka při navštívené prostřednictvím prohlížeče. Všimněte si, že na začátku produkty jsou seřazené podle jejich cena v sestupném pořadí u každého produktu přiřazené příslušné `PriceQuartile` hodnotu. Samozřejmě dají řadit tato data podle jiných kritérií s cena sloupec kvartil stále odrážející hodnocení produktu s s ohledem na ceny (viz obrázek 12).


[![Produkty jsou seřazené podle jejich ceny](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**Obrázek 11**: produkty jsou seřazené podle jejich ceny ([Kliknutím zobrazit obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image31.png))


[![Produkty jsou seřazené podle jejich názvy](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**Obrázek 12**: produkty jsou seřazené podle jejich názvy ([Kliknutím zobrazit obrázek v plné velikosti](adding-additional-datatable-columns-cs/_static/image34.png))


> [!NOTE]
> Zadání několika řádků kódu jsme může posílení GridView tak, aby ho barevné produktu řádky, na základě jejich `PriceQuartile` hodnotu. Jsme může barvu, která tyto produkty v první kvartil světle zelená, programů v druhé QUARTIL žlutě a tak dále. I doporučujeme přidat tuto funkci chvíli trvat. Pokud potřebujete aktualizačnímu programu na formátování GridView, projděte si [vlastní formátování dat na základě po](../custom-formatting/custom-formatting-based-upon-data-cs.md) kurzu.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Alternativní způsob – vytváření jiný TableAdapter

Jak jsme viděli v tomto kurzu při přidání metody do TableAdapter, který vrací pole dat, která nebyla vypsány hlavní dotazem, jsme do DataTable můžete přidat odpovídající sloupce. Tento postup, ale funguje dobře, pouze v případě, že je malý počet metody v TableAdapter, které vracejí jiná datová pole a v případě příliš mnoho z hlavní dotazu se neliší tyto alternativní datová pole.

Místo přidávání sloupců do DataTable, můžete místo toho přidat jiné TableAdapter na datovou sadu, která obsahuje metody z první TableAdapter, které vracejí jiná datová pole. V tomto kurzu místo přidat `PriceQuartile` sloupec, který se `ProductsDataTable` (kde používá se pouze pomocí `GetProductsWithPriceQuartile` metoda), jsme může přidali další TableAdapter na datovou sadu s názvem `ProductsWithPriceQuartileTableAdapter` kterého `Products_SelectWithPriceQuartile` uložené postup jako jeho hlavní dotaz. Stránky ASP.NET, které jsou potřebné k získání informací o produktu s QUARTIL cena využije `ProductsWithPriceQuartileTableAdapter`, zatímco ty, které nebyla může nadále používat `ProductsTableAdapter`.

Přidáním nové TableAdapter zůstat DataTables untarnished a jejich sloupce přesněji zrcadlení datová pole vrácené své TableAdapter s metody. Další TableAdapters může způsobovat podíl opakovaných úkolů a funkce. Například, pokud tyto stránky ASP.NET, který zobrazí `PriceQuartile` sloupec také potřeby zajistit vložit, aktualizovat a odstraňovat podporu `ProductsWithPriceQuartileTableAdapter` třeba, aby měl jeho `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti správně nakonfigurovat. Když tyto vlastnosti by zrcadlení `ProductsTableAdapter` s, tato konfigurace zavádí další krok. Kromě toho jsou nyní dva způsoby, jak aktualizovat, odstranit nebo přidat produkt do databáze - prostřednictvím `ProductsTableAdapter` a `ProductsWithPriceQuartileTableAdapter` třídy.

Ke stažení pro tento kurz zahrnuje `ProductsWithPriceQuartileTableAdapter` třídy v `NorthwindWithSprocs` datovou sadu, která ukazuje tento alternativní přístup.

## <a name="summary"></a>Souhrn

Ve většině scénářů všechny metody v TableAdapter vrátí stejnou sadu datová pole, ale existují situace, když konkrétní metody nebo dvě možná muset vrátit další pole. Například v [hlavní/podrobností s odrážkami seznamu v hlavní záznamů pomocí DataList podrobnosti](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) kurzu jsme přidali metodu pro `CategoriesTableAdapter` že kromě datová pole hlavní dotazu s vrátil `NumberOfProducts` pole, která hlášené počet produktů, které jsou spojené s každou kategorii. V tomto kurzu jsme se podívali na přidání metody v `ProductsTableAdapter` vrácená `PriceQuartile` pole kromě hlavní dotazu s datová pole. K zaznamenání dat o další pole vrácené TableAdapter s metodami, že je potřeba přidat odpovídající sloupců do DataTable.

Pokud máte v plánu na ruční přidávání sloupců do DataTable, doporučuje se, že TableAdapter pomocí uložených procedur. Pokud TableAdapter používá příkazů SQL ad-hoc, kdykoli TableAdapter spuštění Průvodce konfigurací všechny metody vrátit se do pole dat vrácených dotazem hlavní seznamy pole data. Tento problém neprodlužuje k uloženým procedurám, proto se doporučují a byly použity v tomto kurzu.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Randy Schmidt, Jan Goor, Bernadette Leigh a Hilton Giesenow. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](updating-the-tableadapter-to-use-joins-cs.md)
[další](working-with-computed-columns-cs.md)
