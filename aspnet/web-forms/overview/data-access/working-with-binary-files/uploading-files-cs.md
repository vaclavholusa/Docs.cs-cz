---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
title: Nahrávání souborů (C#) | Microsoft Docs
author: rick-anderson
description: Zjistěte, jak povolit uživatelům odesílat binární soubory (například dokumenty aplikace Word nebo PDF) na svůj web, kde může být uložena v systému souborů na server, buď...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: b381b1da-feb3-4776-bc1b-75db53eb90ab
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
msc.type: authoredcontent
ms.openlocfilehash: 3c758e94311817d01b17d27083733f805caf600f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
---
<a name="uploading-files-c"></a>Nahrávání souborů (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_CS.exe) nebo [stáhnout PDF](uploading-files-cs/_static/datatutorial54cs1.pdf)

> Zjistěte, jak umožnit uživatelům odesílat binární soubory (například dokumenty aplikace Word nebo PDF) na svůj web, kde může být uložena v systému souborů serveru nebo databáze.


## <a name="introduction"></a>Úvod

Všechny z kurzů jsme zkontrolován, pokud jste již dříve pracovali výhradně pomocí textová data. Mnoho aplikací mít však datové modely, které zaznamenat textové a binární data. Stránku online dating může povolit uživatelům odesílat obrázku chcete přidružit svůj profil. Náborová webu může uživatelům nahrát jejich obnovení jako dokument aplikace Microsoft Word nebo PDF.

Práce s binární data přidá novou sadu problémy. Jsme musíte se rozhodnout, jak je binární data uložená v aplikaci. Rozhraní používá pro vkládání nových záznamů má aktualizovat, aby uživateli umožní nahrát soubor z jejich počítače a kroků navíc musí být přijata k zobrazení nebo poskytují způsob pro stahování záznam s přidružené binární data. V tomto kurzu a další tři jsme budete prozkoumejte postup hurdle tyto problémy. Na konci tyto kurzy budete Vyvinuli jsme plně funkční aplikaci, která přidruží obrázků a PDF – příručka každou kategorii. V tomto kurzu konkrétní jsme budete podívejte se na různé techniky pro ukládání binárních dat a zjistit, jak povolit uživatelům odesílat soubor z jejich počítače a byla uložena v systému souborů webového serveru s.

> [!NOTE]
> Binární data, která je součástí datový model aplikace s se někdy označuje jako [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), zkratka pro binární rozsáhlý objekt. V těchto kurzech I jste se rozhodli použít terminologie binární data, i když termín objektu BLOB je totožná.


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Krok 1: Vytvoření pracovní s webovými stránkami binární Data

Než začneme prozkoumat problémy související s přidání podpory pro binární data, umožní s nejprve vytváření stránek ASP.NET v našem webu projekt, který budeme potřebovat pro tento kurz a další tři chvíli trvat. Nejprve přidejte novou složku s názvem `BinaryData`. Dál přidejte následující stránky ASP.NET do této složky, a zkontrolujte, zda přidružit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![Přidání stránky ASP.NET pro binární Data související s kurzy](uploading-files-cs/_static/image1.gif)

**Obrázek 1**: Přidání stránky ASP.NET pro binární Data související s kurzy


V jiných složkách, jako `Default.aspx` v `BinaryData` složky zobrazí seznam kurzů k v jeho části. Odvolat, který `SectionLevelTutorialListing.ascx` tuto funkci zajišťuje uživatelský ovládací prvek. Proto přidat tento uživatelský ovládací prvek pro `Default.aspx` přetažením z Průzkumníka řešení na stránku s zobrazení návrhu.


[![Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx](uploading-files-cs/_static/image2.gif)](uploading-files-cs/_static/image1.png)

**Obrázek 2**: Přidat `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku na `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](uploading-files-cs/_static/image2.png))


Nakonec přidejte tyto stránek jako položky na `Web.sitemap` souboru. Konkrétně, přidejte následující kód po Enhancing GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-cs/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, pozorně Zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně nyní obsahuje položky pro práci s kurzy binární Data.


![Mapy webu nyní obsahuje položky pro práci s kurzy binární Data](uploading-files-cs/_static/image3.gif)

**Obrázek 3**: mapy webu nyní obsahuje položky pro práci s kurzy binární Data


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Krok 2: Rozhodování, kam se mají ukládat binární Data

Binární data, která souvisí s datovým modelem aplikace s mohou být uloženy v jednom ze dvou míst: v systému souborů webového serveru s s odkazem na soubor uložené v databázi. nebo přímo v rámci samotná databáze (viz obrázek 4). Každý přístup má svou vlastní sadu výhody a nevýhody a merits podrobnější informace.


[![Binární Data se uloží v systému souborů nebo přímo v databázi](uploading-files-cs/_static/image4.gif)](uploading-files-cs/_static/image3.png)

**Obrázek 4**: binární Data se uloží v systému souborů nebo přímo v databázi ([Kliknutím zobrazit obrázek v plné velikosti](uploading-files-cs/_static/image4.png))


Představte si, že jsme chtěli rozšířit databázi Northwind přidružit obrázku u každého produktu. Jednou z možností by být k ukládání těchto souborů bitové kopie v systému souborů webového serveru s a poznamenejte si cestu v `Products` tabulky. S tímto přístupem d přidáme `ImagePath` sloupec, který se `Products` tabulku typu `varchar(200)`, možná. Když uživatel nahrál obrázku pro Chai, že obrázek je uložený v systému souborů webového serveru s v `~/Images/Tea.jpg`, kde `~` představuje fyzickou cestu s aplikací. To znamená pokud webová stránka se zobrazuje na fyzickou cestu `C:\Websites\Northwind\`, `~/Images/Tea.jpg` by se ekvivalentní `C:\Websites\Northwind\Images\Tea.jpg`. Po nahrání souboru bitové kopie, jsme d Chai záznam v aktualizujte `Products` tabulky tak, aby jeho `ImagePath` sloupec odkazuje cesta novou bitovou kopii. Můžeme použít `~/Images/Tea.jpg` nebo právě `Tea.jpg` Pokud jsme se rozhodli, že všechny obrázky produktů by měly být umístěny v aplikaci s `Images` složky.

Jsou uvedené hlavní výhody ukládání binární data v systému souborů:

- **Snadná implementace** jako jsme krátce zobrazí, ukládání a načítání binární data uložená v databázi přímo zahrnuje trochu další kód než při práci s daty v systému. Kromě toho aby se uživatel k zobrazení nebo stažení. binární data se mají být uvedeny adresou URL tato data. Pokud data umístěná v systému souborů webového serveru s, adresa URL je jednoduchá. Pokud jsou data uložena v databázi, ale webové stránky musí vytvořit, které se načítají a vrátit data z databáze.
- **Pro binární data širší přístup** binární data může musí být přístupné pro ostatní služby nebo aplikace, které nelze načítat data z databáze. Například může také musí být k dispozici uživatelům přes obrázky spojené s každou produktu [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), v takovém případě jsme d Chcete uložit binární data v systému souborů.
- **Výkon** Pokud binární data jsou uložena v systému souborů, vyžádání a zahlcení sítě mezi serverem databáze a webového serveru bude menší než pokud binární data jsou uložena přímo v databázi.

Hlavní nevýhodou ukládání binárních dat v systému souborů je, že ji odděluje data z databáze. Pokud dojde k odstranění záznamu z `Products` tabulky, související soubor v systému souborů webového serveru s není automaticky odstraněn. Jsme musí zápisu kódu navíc odstranit soubor nebo systému souborů se zaplní nepoužívané, osamocené soubory. Kromě toho při zálohování databáze, musí zajišťujeme, že aby zálohování přidružená binární data v systému souborů, také. Přesun databáze do jiné lokality nebo server představuje podobné problémy.

Alternativně můžete uložená binární data přímo v databázi Microsoft SQL Server 2005 vytvořením sloupec typu `varbinary`. Jako pomocí jiných datových typů proměnné délky, můžete určit maximální délka binárních dat, která mohou být uložena v tomto sloupci. Například můžete vyhradit maximálně 5 000 bajtů použít `varbinary(5000)`; `varbinary(MAX)` umožňuje maximální velikost úložiště, o 2 GB.

Hlavní výhodou ukládání binární data přímo v databázi je úzkou párování mezi binární data a záznamů databáze. To výrazně zjednodušuje databáze úlohy správy, jako je zálohování nebo přesunutí databáze do jiné lokality nebo server. Odstranění záznamu automaticky odstraní také odpovídající binární data. Existují také další jemně výhod ukládání binární data v databázi. V tématu [ukládání binární soubory přímo v databázi pomocí technologie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) pro podrobné informace.

> [!NOTE]
> V systému Microsoft SQL Server 2000 a dřívějších verzích `varbinary` datový typ měli maximální limit 8 000 bajtů. K uložení až 2 GB binárních dat [ `image` datový typ](https://msdn.microsoft.com/library/ms187993.aspx) se musí použít místo. Po přidání `MAX` v systému SQL Server 2005, ale `image` datový typ je zastaralá. Ho s stále podporována z důvodů zpětné kompatibility, ale Microsoft oznámila, který `image` datový typ budou odebrány v budoucí verzi systému SQL Server.


Pokud pracujete s starší datového modelu může zobrazit `image` datového typu. Databáze Northwind s `Categories` tabulka má `Picture` sloupec, který slouží k uložení binární data souboru s bitovou kopií pro kategorii. Vzhledem k tomu, že databáze Northwind má jeho kořeny v aplikaci Microsoft Access a dřívějších verzích systému SQL Server, tento sloupec je typu `image`.

Pro tento kurz a další tři použijeme obou přístupů. `Categories` Tabulka již obsahuje `Picture` sloupec pro ukládání binární obsah bitové kopie pro kategorii. Sloupec, přidáme `BrochurePath`, k uložení cestu k souboru PDF v systému souborů webového serveru s, který slouží k poskytování kvalita tisku a profesionální přehled kategorie.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Krok 3: Přidání`BrochurePath`sloupec, který se`Categories`tabulky

Aktuálně kategorie tabulka obsahuje pouze čtyři sloupce: `CategoryID`, `CategoryName`, `Description`, a `Picture`. Kromě těchto polí je potřeba přidat novou, který bude odkazovat na kategorie s – příručka (pokud existuje). Chcete-li přidat tento sloupec, přejděte do Průzkumníka serveru, přejděte do tabulky, klikněte pravým tlačítkem na `Categories` tabulky a zvolte otevřete definice tabulky (viz obrázek 5). Pokud nevidíte Průzkumníka serveru, zprovoznit výběrem možnosti v Průzkumníku serveru v nabídce zobrazení nebo stiskněte kombinaci kláves Ctrl + Alt + S.

Přidejte nový `varchar(200)` sloupec, který se `Categories` tabulky, který je pojmenován `BrochurePath` a umožňuje `NULL` s a klikněte na ikonu Uložit (nebo stiskněte kombinaci kláves Ctrl + S).


[![Přidat sloupec BrochurePath do tabulky kategorií](uploading-files-cs/_static/image5.gif)](uploading-files-cs/_static/image5.png)

**Obrázek 5**: přidání `BrochurePath` sloupec, který se `Categories` tabulky ([Kliknutím zobrazit obrázek v plné velikosti](uploading-files-cs/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Krok 4: Aktualizace architekturu pro použití`Picture`a`BrochurePath`sloupce

`CategoriesDataTable` v dat přístup Layer (DAL) v současnosti má čtyři `DataColumn` s definované: `CategoryID`, `CategoryName`, `Description`, a `NumberOfProducts`. Když jsme původně navržené tento DataTable v [vytváření Data Access Layer](../introduction/creating-a-data-access-layer-cs.md) kurzu `CategoriesDataTable` pouze měl první tři sloupce; `NumberOfProducts` sloupec se přidal ve [a podrobností pomocí s odrážkami Seznam záznamů hlavní s DataList podrobnosti](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) kurzu.

Jak je popsáno v *vytváření Data Access Layer*, DataTables v datové sadě zadali tvoří obchodních objektů. TableAdapters jsou zodpovědní za komunikaci s databází a naplnění objekty obchodní s výsledky dotazu. `CategoriesDataTable` Se nacházejí `CategoriesTableAdapter`, který má tři metody načtení dat:

- `GetCategories()` spustí hlavní dotazu TableAdapter s a vrátí `CategoryID`, `CategoryName`, a `Description` pole všechny záznamy v `Categories` tabulky. Hlavní dotaz je co je používán automaticky generovaný `Insert` a `Update` metody.
- `GetCategoryByCategoryID(categoryID)` Vrátí `CategoryID`, `CategoryName`, a `Description` pole kategorie, jejichž `CategoryID` rovná *categoryID*.
- `GetCategoriesAndNumberOfProducts()` -Vrátí `CategoryID`, `CategoryName`, a `Description` pole pro všechny záznamy v `Categories` tabulky. Také pomocí poddotazu vrátí počet produktů, které jsou spojené s každou kategorii.

Všimněte si, že žádná z nich vrácena dotazy `Categories` tabulky s `Picture` nebo `BrochurePath` sloupce; ani nemá `CategoriesDataTable` poskytují `DataColumn` s pro tato pole. Chcete-li pracovat na obrázku a `BrochurePath` vlastnosti, musíme nejprve přidat je do `CategoriesDataTable` a aktualizujte `CategoriesTableAdapter` třídy vracení tyto sloupce.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Přidávání`Picture`a`BrochurePath``DataColumn` s

Začněte přidáním těchto dvou sloupců `CategoriesDataTable`. Klikněte pravým tlačítkem na `CategoriesDataTable` s hlavičky v místní nabídce vyberte Přidat a poté zvolte možnost sloupec. Tím se vytvoří nový `DataColumn` DataTable s názvem `Column1`. Přejmenujte tento sloupec `Picture`. V okně vlastnosti nastavit `DataColumn` s `DataType` vlastnost `System.Byte[]` (Toto není možnost v rozevíracím seznamu, je potřeba zadat v).


[![Vytvoření obrázku s názvem DataColumn, jejichž datový typ je System.Byte]](uploading-files-cs/_static/image6.gif)](uploading-files-cs/_static/image7.png)

**Obrázek 6**: vytvoření `DataColumn` pojmenované `Picture` jejichž `DataType` je `System.Byte[]` ([Kliknutím zobrazit obrázek v plné velikosti](uploading-files-cs/_static/image8.png))


Přidejte další `DataColumn` do DataTable, pojmenování ho `BrochurePath` pomocí výchozího `DataType` hodnotu (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Návrat`Picture`a`BrochurePath`hodnoty z TableAdapter

S tyto dva `DataColumn` s přidán do `CategoriesDataTable`, jsme re připravené k aktualizaci `CategoriesTableAdapter`. Jsme může mít obě tyto hodnoty ve sloupcích, vrátí se v hlavní dotazu TableAdapter, ale to by navrácení binární data pokaždé, když `GetCategories()` byla volána metoda. Místo toho umožňují s aktualizovat hlavní dotazu TableAdapter tak, aby `BrochurePath` a vytvořte metodu načtení další data, která vrátí určité kategorie s `Picture` sloupce.

Pokud chcete aktualizovat hlavní dotazu TableAdapter, klikněte pravým tlačítkem na `CategoriesTableAdapter` záhlaví s a v místní nabídce vyberte možnost konfigurovat. Otevře Průvodce konfigurací adaptéru tabulky, které jsme jste viděli v několika posledních kurzy. Aktualizovat dotaz tak, aby `BrochurePath` a klikněte na tlačítko Dokončit.


[![Aktualizace seznamu sloupců v příkazu SELECT také vrátit BrochurePath](uploading-files-cs/_static/image7.gif)](uploading-files-cs/_static/image9.png)

**Obrázek 7**: aktualizace v seznamu sloupců `SELECT` příkaz také vrátit `BrochurePath` ([Kliknutím zobrazit obrázek v plné velikosti](uploading-files-cs/_static/image10.png))


Při použití příkazů SQL ad-hoc TableAdapter, aktualizaci seznamu sloupců v hlavním dotazu pro všechny aktualizace seznamu sloupců `SELECT` dotaz metody v TableAdapter. To znamená `GetCategoryByCategoryID(categoryID)` metoda se aktualizovalo a vrátit `BrochurePath` sloupec, který může být jsme určený. Však je aktualizované taky v seznamu sloupců `GetCategoriesAndNumberOfProducts()` metoda, odebrání poddotazu, která vrátí počet produktů, pro každou kategorii! Proto je potřeba aktualizovat tato metoda s `SELECT` dotazu. Klikněte pravým tlačítkem na `GetCategoriesAndNumberOfProducts()` metoda, zvolte konfigurovat a obnovit `SELECT` dotazu zpět na původní hodnotu:


[!code-sql[Main](uploading-files-cs/samples/sample2.sql)]

Dál vytvořte novou TableAdapter metodu, která vrátí určité kategorie s `Picture` hodnota sloupce. Klikněte pravým tlačítkem na `CategoriesTableAdapter` s záhlaví a zvolte možnost přidat dotaz spustíte Průvodce konfigurací dotazu TableAdapter. Prvním krokem v tomto průvodci nám požádá, že pokud nám chcete dotaz na data pomocí příkazu SQL ad-hoc, nový uložená procedura nebo některého ze stávajících. Vyberte příkazy SQL použít a klikněte na tlačítko Další. Vzhledem k tomu, že jsme se vracejí řádek, vyberte SELECT, který vrací řádky možnost z druhém kroku.


[![Vyberte příkazy SQL použijte možnost](uploading-files-cs/_static/image8.gif)](uploading-files-cs/_static/image11.png)

**Obrázek 8**: Vyberte příkazy SQL použijte možnost ([Kliknutím zobrazit obrázek v plné velikosti](uploading-files-cs/_static/image12.png))


[![Vzhledem k tomu, že dotaz vrátí záznam z tabulky kategorie, zvolte vyberte, která vrací řádky](uploading-files-cs/_static/image9.gif)](uploading-files-cs/_static/image13.png)

**Obrázek 9**: vzhledem k tomu, že dotaz vrátí záznam z tabulky kategorie, zvolte výběru, která vrací řádky ([Kliknutím zobrazit obrázek v plné velikosti](uploading-files-cs/_static/image14.png))


V tomto kroku zadejte následující dotaz SQL a klikněte na tlačítko Další:


[!code-sql[Main](uploading-files-cs/samples/sample3.sql)]

Posledním krokem je vybrat název pro novou metodu. Použití `FillCategoryWithBinaryDataByCategoryID` a `GetCategoryWithBinaryDataByCategoryID` výplně DataTable a vraťte se DataTable vzory, v uvedeném pořadí. Kliknutím na tlačítko Dokončit ukončete průvodce.


[![Zvolte názvy metody s TableAdapter](uploading-files-cs/_static/image10.gif)](uploading-files-cs/_static/image15.png)

**Obrázek 10**: Zvolte názvy pro TableAdapter s metody ([Kliknutím zobrazit obrázek v plné velikosti](uploading-files-cs/_static/image16.png))


> [!NOTE]
> Po dokončení Průvodce konfigurací tabulky adaptér dotaz může zobrazit dialogové okno oznamující, že nový text příkazu vrací data se schématem liší od schématu hlavní dotazu. Průvodce je poznamenat stručně řečeno, který hlavní dotazu TableAdapter s `GetCategories()` vrátí schéma jiný než ten, který jsme právě vytvořili. Ale toto je co chceme, takže můžete tuto zprávu ignorovat.


Navíc mějte na paměti, že pokud používáte ad-hoc příkazů SQL a pomocí průvodce můžete změnit hlavní dotazu TableAdapter s novější někde v čase, se upraví `GetCategoryWithBinaryDataByCategoryID` metoda s `SELECT` zahrnout pouze tyto sloupce ze seznamu sloupců příkaz s hlavní dotazu (to znamená, budou odebrány `Picture` sloupce z dotazu). Budete muset ručně aktualizovat sloupce seznamu vrátíte zpět `Picture` sloupců, podobně jako co jsme se s `GetCategoriesAndNumberOfProducts()` metoda dříve v tomto kroku.

Po přidání dvou `DataColumn` s k `CategoriesDataTable` a `GetCategoryWithBinaryDataByCategoryID` metodu `CategoriesTableAdapter`, tyto třídy v Návrháři DataSet zadali by měl vypadat jako na snímku obrazovky v obrázek 11.


![Návrháři DataSet zahrnuje nové sloupce a – metoda](uploading-files-cs/_static/image11.gif)

**Obrázek 11**: návrháře DataSet zahrnuje nové sloupce a – metoda


## <a name="updating-the-business-logic-layer-bll"></a>Aktualizace vrstvu obchodní logiky (BLL)

S vrstvy DAL aktualizovat zbývá k posílení obchodní logiky vrstvy (BLL) o metodu pro nové `CategoriesTableAdapter` metoda. Přidejte následující metodu do `CategoriesBLL` třídy:


[!code-csharp[Main](uploading-files-cs/samples/sample4.cs)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Krok 5: Nahrání souboru z klienta na webový server

Při shromažďování binární data, často tato data poskytuje koncovým uživatelem. Pokud chcete zaznamenat tyto informace, uživatel musí být možné nahrát soubor z jejich počítače do webového serveru. Odeslaná data je pak potřeba integrovat datový model, který může to znamenat ukládání souboru do systému souborů s webového serveru a přidání cesty k souboru v databázi nebo zápis binární obsah přímo do databáze. V tomto kroku budete podíváme na to, jak chcete, aby uživatel k nahrání souborů z jejich počítače k serveru. V dalším kurzu jsme budete zapněte naše pozornost integraci nahrávaný soubor do datového modelu.

ASP.NET 2.0 s novou [ovládací prvek webu odesílání souborů při odpovědích](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) poskytuje mechanismus pro uživatelům odeslat soubor z počítače na webový server. Ovládací prvek odesílání souborů při odpovědích vykreslí jako `<input>` element jehož `type` je atribut nastaven na soubor, který prohlížeče zobrazí jako textové pole s tlačítko Procházet. Klepnutím na tlačítko Procházet vyvolá dialogové okno, ze kterého může uživatel vybrat soubor. Při odeslání formuláře zpět obsah s vybraný soubor se odesílají společně s zpětné volání. Na straně serveru je přístupný prostřednictvím vlastnosti odesílání souborů při odpovědích ovládacích prvků s informace o nahrávaný soubor.

K předvedení odesílání souborů, otevřete `FileUpload.aspx` stránku `BinaryData` složku, přetáhněte ovládací prvek odesílání souborů při odpovědích z panelu nástrojů na návrháře a nastavení ovládacího prvku s `ID` vlastnost `UploadTest`. V dalším kroku přidání ovládacího prvku tlačítko webové nastavení jeho `ID` a `Text` vlastnosti, které chcete `UploadButton` a nahrajte soubor vybrané v uvedeném pořadí. Nakonec, umístěte ovládací prvek popisek webové pod tlačítko, vymažte jeho `Text` vlastnost a sadu jeho `ID` vlastnost `UploadDetails`.


[![Přidání ovládacího prvku odesílání souborů při odpovědích na stránku ASP.NET](uploading-files-cs/_static/image12.gif)](uploading-files-cs/_static/image17.png)

**Obrázek 12**: Přidání ovládacího prvku odesílání souborů při odpovědích na stránku ASP.NET ([Kliknutím zobrazit obrázek v plné velikosti](uploading-files-cs/_static/image18.png))


Obrázek 13 zobrazí tato stránka při zobrazení prostřednictvím prohlížeče. Všimněte si, že klepnutím na tlačítko Procházet otevře výběr dialogové okno souboru, umožňuje uživateli vybrat soubor z jejich počítače. Jakmile se soubor nebyl vybraný, klepnutím na tlačítko Odeslat vybraný soubor způsobí, že zpětné volání, která odešle binární obsah s vybraného souboru na webový server.


[![Uživatele můžete vybrat soubor k odeslání z jejich počítače k serveru](uploading-files-cs/_static/image13.gif)](uploading-files-cs/_static/image19.png)

**Obrázek 13**: kterého uživatel může vybrat soubor k odeslání z jejich počítače na serveru ([Kliknutím zobrazit obrázek v plné velikosti](uploading-files-cs/_static/image20.png))


Zpětné volání nahraný soubor lze uložit do systému souborů nebo jeho binární data možné pracovat s přímo prostřednictvím datového proudu. V tomto příkladu umožní s vytvořit `~/Brochures` složky a nahraný soubor uložte. Začněte přidáním `Brochures` složku do lokality jako podsložku kořenový adresář. Dále vytvořte obslužnou rutinu události pro `UploadButton` s `Click` událostí a přidejte následující kód:


[!code-csharp[Main](uploading-files-cs/samples/sample5.cs)]

Ovládací prvek odesílání souborů při odpovědích poskytuje celou řadu vlastností pro práci s odeslaná data. Pro instanci [ `HasFile` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) označuje, zda byl soubor odeslaný uživatelem, zatímco [ `FileBytes` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) poskytuje přístup k nahrané binární data jako pole bajtů. `Click` Obslužné rutiny události spustí tím zajistí, že soubor byl odeslán. Pokud soubor se nahrají popisek zobrazuje název nahrávaný soubor, jeho velikost v bajtech a typ obsahu.

> [!NOTE]
> K zajištění, že uživatel uloží soubor můžete zkontrolovat `HasFile` vlastnost a zobrazí upozornění, pokud ji s `false`, nebo můžete použít [ovládací prvek RequiredFieldValidator](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) místo.


Odesílání souborů při odpovědích s `SaveAs(filePath)` nahrávaný soubor uloží do zadaného *filePath*. *filePath* musí být *fyzická cesta* (`C:\Websites\Brochures\SomeFile.pdf`) ne *virtuální* *cesta* (`/Brochures/SomeFile.pdf`). [ `Server.MapPath(virtPath)` Metoda](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) obsahuje virtuální cestu a vrátí jeho odpovídající fyzická cesta. Virtuální cesta, která následuje `~/Brochures/fileName`, kde *fileName* je název nahrávaný soubor. V tématu [pomocí Server.MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) pro další informace o virtuálních a fyzických cest a používání `Server.MapPath`.

Po dokončení `Click` obslužné rutiny události, za chvíli k otestování stránku v prohlížeči. Klikněte na tlačítko Procházet a vyberte soubor z pevného disku a potom klikněte na tlačítko Nahrát soubor vybrané. Zpětné volání odešle obsah vybraného souboru webového serveru, který se potom zobrazí informace o souboru před jeho uložením `~/Brochures` složky. Po nahrání souboru, vraťte k sadě Visual Studio a klikněte na tlačítko Aktualizovat v Průzkumníku řešení. Měli byste vidět soubor, který jste právě nahráli ve složce ~/Brochures!


[![EvolutionValley.jpg soubor byl odeslán na webový server](uploading-files-cs/_static/image14.gif)](uploading-files-cs/_static/image21.png)

**Obrázek 14**: soubor `EvolutionValley.jpg` byl odeslán na webový server ([Kliknutím zobrazit obrázek v plné velikosti](uploading-files-cs/_static/image22.png))


![EvolutionValley.jpg byla uložena do složky ~/Brochures](uploading-files-cs/_static/image15.gif)

**Obrázek 15**: `EvolutionValley.jpg` byla uložena do `~/Brochures` složky


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Odlišnosti s ukládání odeslané soubory do systému souborů

Existuje několik odlišnosti, které je potřeba řešit při ukládání nahrávání souborů do systému souborů webového serveru s. První, zde s problém zabezpečení. Uložte soubor do systému souborů, kontext zabezpečení, pod kterou je prováděna stránku ASP.NET musí mít oprávnění k zápisu. Webový vývojový Server ASP.NET běží v kontextu aktuálního uživatelského účtu. Pokud používáte Microsoft s Internetové informační služby (IIS) jako webový server, kontext zabezpečení závisí na verzi služby IIS a její konfiguraci.

Dalším problémem ukládat soubory do systému souborů se pohybuje kolem pojmenování souborů. V současné době naší stránce s uloží všechny soubory ukládány `~/Brochures` adresáře pomocí stejný název jako soubor s klientského počítače. Pokud uživatel A odešle – příručka s názvem `Brochure.pdf`, soubor se uloží jako `~/Brochure/Brochure.pdf`. Ale co když zopakovat později B uživatel odešle – příručka jiný soubor, který se stane mít stejný název souboru (`Brochure.pdf`)? Kódem máme teď uživatele s souboru, budou přepsány nahrávání s uživatel B.

Existuje několik postupů pro řešení konfliktů názvů souborů. Jednou z možností je zakázat nahrání souboru, pokud existuje nějaká se stejným názvem již existuje. S tímto přístupem, když se uživatel B se pokusí odeslat soubor s názvem `Brochure.pdf`, systém nebude uložit soubor a místo toho zobrazí zpráva informující uživatele B soubor přejmenujte a zkuste to znovu. Jiná možnost je k uložení souboru pomocí jedinečný název souboru, který může být [globálně jedinečný identifikátor (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) nebo hodnotu z příslušné databáze záznamů s primární klíče sloupce (za předpokladu, že nahrávání je přidružen konkrétního řádku v datovém modelu). V dalším kurzu jsme budete tyto možnosti podrobněji prozkoumat.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Problémy spojené s velmi velkých objemů binární Data

Tyto kurzy předpokládat, že je binární data zaznamenaná mírné velikost. Pracujete s velmi velkým množstvím binární datové soubory, které jsou několik MB nebo větší zavádí nové výzvy, které jsou nad rámec těchto kurzů. Například ve výchozím nastavení ASP.NET odmítnou nahrávání více než 4 MB, i když to můžete nakonfigurovat přes [ `<httpRuntime>` element](https://msdn.microsoft.com/library/e1f13641.aspx) v `Web.config`. Služba IIS ukládá příliš vlastní omezení velikosti nahrávání souborů. V tématu [velikost souboru nahrát IIS](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) Další informace. Čas potřebný k nahrání velkých souborů kromě toho můžete překročit výchozí 110 sekundách, po kterou čekat ASP.NET pro žádost. Existují také paměti a výkon problémy, které nastat při práci s velkými soubory.

Odesílání souborů při odpovědích řízení je nepraktické pro nahrávání velkých souborů. Jako obsah souboru s jsou odeslání na server, čekat, bez jakékoli potvrzení, že je jejich nahrávání pokročíte patiently koncového uživatele. Toto není mnoho problém, při plánování práce s menší soubory, které lze odeslat během pár sekund, ale může být problém při plánování práce s větší soubory, které může trvat nahrát. Existuje mnoho různých třetích stran souboru nahrávání ovládací prvky, které jsou vhodnější pro zpracování velkých nahrávání a mnohé z těchto dodavatelů poskytují indikátory průběhu a ActiveX nahrát správci, která představují mnohem zajímavější činnost koncového uživatele.

Pokud aplikace potřebuje pro zpracování velkých souborů, budete muset pečlivě prozkoumat problémů a najděte vhodné řešení pro konkrétních potřeb.

## <a name="summary"></a>Souhrn

Vytváření aplikace, která je potřeba zaznamenat binární data představuje určité problémy. V tomto kurzu jsme prozkoumali první dvě: rozhodování, kam se mají ukládat binární data a povolení pro nahrání binární obsah prostřednictvím na webové stránce. Prostřednictvím následujících třech kurzy ukážeme, jak přidružit odesílaná data záznam v databázi a také jak zobrazit binární data souběžně s jeho text datová pole.

Radostí programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Použití velké hodnoty datových typů](https://msdn.microsoft.com/library/ms178158.aspx)
- [Odesílání souborů při odpovědích ovládací prvek – elementy QuickStart](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [Ovládací prvek ASP.NET 2.0 odesílání souborů při odpovědích serveru](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [Tmavý straně nahrávání souborů](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Teresy Murphy a Bernadette Leigh. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](displaying-binary-data-in-the-data-web-controls-cs.md)
