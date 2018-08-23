---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
title: Nahrávání souborů (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Zjistěte, jak povolit uživatelům odesílat binární soubory (jako jsou například dokumenty aplikace Word nebo PDF) k vašemu webovému serveru, kde mohou být uloženy v systému souborů na server, buď...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: b381b1da-feb3-4776-bc1b-75db53eb90ab
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
msc.type: authoredcontent
ms.openlocfilehash: 8849f8f279dde883a71fb3ba1678a589f2e321eb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754562"
---
<a name="uploading-files-c"></a>Nahrávání souborů (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_CS.exe) nebo [stahovat PDF](uploading-files-cs/_static/datatutorial54cs1.pdf)

> Zjistěte, jak povolit uživatelům odesílat binární soubory (jako jsou například dokumenty aplikace Word nebo PDF) k vašemu webovému serveru, kde mohou být uloženy v systému souborů serveru nebo v databázi.


## <a name="introduction"></a>Úvod

Všechny kurzy jsme pracovali ve prozkoumat zatím výhradně s textová data. Mnoho aplikací však mít datové modely, které zaznamenávají textové a binární data. Online dating lokality může uživatelům umožní nahrát obrázek pro přidružení k svůj profil. Náboru webu může umožnit uživatelům odeslat jejich obnovení jako dokument aplikace Microsoft Word nebo PDF.

Práce s binárními daty přidá novou řadu jiných problémů. Jsme musíte rozhodnout, jak binární data jsou uložena v aplikaci. Rozhraní použité pro vkládání nových záznamů musí aktualizovat, aby uživatel mohl odeslat soubor ze svého počítače a pár kroků navíc musí být přijata má být zobrazeno nebo poskytují prostředky pro stahování záznam s přidružená binární data. V tomto kurzu a další tři podíváme, jak hurdle tyto výzvy. Na konci tyto kurzy budete mít vytváříme plně funkční aplikaci, která přidružuje obrázek a PDF si brožuru o jednotlivých kategorií. V tomto konkrétním kurzu vytvoříme podívejte se na různých postupů pro ukládání binárních dat a podívejte se, jak povolit uživatelům odesílat soubor z jejich počítače a jeho uložení v systému souborů webového serveru s.

> [!NOTE]
> Binární data, která je součástí aplikace s datový model se někdy označuje jako [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), zkratka pro Binary Large OBject. V těchto kurzech můžu zvolil možnost použití terminologie binární data, i když jako objekt BLOB je synonymní.


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Krok 1: Vytvoření práce s webovými stránkami binární Data

Než začneme k prozkoumání problémů, které jsou přidružené k přidání podpory pro binární data, umožní s nejdřív využít k vytvoření stránky technologie ASP.NET v našem projektu webu, který budeme potřebovat pro tento kurz a další tři. Začněte přidáním novou složku s názvem `BinaryData`. Dále přidejte následující stránky ASP.NET do této složky, nezapomeňte přiřadit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![Přidání stránky technologie ASP.NET pro binární Data související kurzy](uploading-files-cs/_static/image1.gif)

**Obrázek 1**: Přidání stránky technologie ASP.NET pro binární Data související kurzy


V jiných složkách, jako jsou `Default.aspx` v `BinaryData` složky zobrazí seznam kurzů v příslušném oddílu. Vzpomeňte si, že `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek tuto funkci poskytuje. Proto přidat tento uživatelský ovládací prvek `Default.aspx` přetažením v Průzkumníku řešení na stránku s návrhové zobrazení.


[![Přidat na stránku Default.aspx SectionLevelTutorialListing.ascx uživatelského ovládacího prvku](uploading-files-cs/_static/image2.gif)](uploading-files-cs/_static/image1.png)

**Obrázek 2**: Přidejte `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek `Default.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](uploading-files-cs/_static/image2.png))


A konečně, přidejte tyto stránky jako položky `Web.sitemap` souboru. Konkrétně, přidejte následující kód za Enhancing prvku GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-cs/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, věnujte chvíli zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro práci s kurzy binární Data.


![Mapa webu nyní obsahuje záznamy pro práci s kurzy binární Data](uploading-files-cs/_static/image3.gif)

**Obrázek 3**: mapy webu nyní obsahuje záznamy pro práci s kurzy binární Data


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Krok 2: Rozhodování, kam Store binárních dat

Binární data, která je přidružena s datovým modelem aplikace mohou být uloženy v jednom z následujících dvou míst: v systému souborů webového serveru s odkazem na soubor uložený v databázi. nebo přímo v rámci samotné databázi (viz obrázek 4). Každý přístup má svou vlastní sadu výhody a nevýhody a merits podrobnější informace.


[![Binární Data mohou být uloženy v systému souborů nebo přímo v databázi](uploading-files-cs/_static/image4.gif)](uploading-files-cs/_static/image3.png)

**Obrázek 4**: binární Data mohou být uloženy v systému souborů nebo přímo v databázi ([kliknutím ji zobrazíte obrázek v plné velikosti](uploading-files-cs/_static/image4.png))


Představte si, že jsme chtěli rozšířit databázi Northwind k přidružení obrázek každého produktu. Jednou z možností by být k ukládání těchto souborů obrázků v systému souborů webového serveru s a poznamenejte si cestu v `Products` tabulky. S tímto přístupem d přidáme `ImagePath` sloupec, který se `Products` tabulku typu `varchar(200)`, možná. Když se uživatel nahraje obrázek pro Chai, daný obrázek může být uložen v systému souborů webového serveru s v `~/Images/Tea.jpg`, kde `~` představuje fyzickou cestu s aplikací. To znamená pokud na webu je kořenovým adresářem v fyzickou cestu `C:\Websites\Northwind\`, `~/Images/Tea.jpg` ekvivalentní `C:\Websites\Northwind\Images\Tea.jpg`. Po nahrání souboru obrázku, d aktualizujeme Chai záznamu v `Products` tabulku tak, aby jeho `ImagePath` sloupec odkazuje cesta nová image. Mohli bychom použít `~/Images/Tea.jpg` nebo jen `Tea.jpg` Pokud jsme se rozhodli, že všechny bitové kopie produktu by měly být umístěny v aplikaci s `Images` složky.

Hlavní výhody ukládání binárních dat v systému souborů jsou:

- **Snadné implementaci** jako ukážeme za chvíli, ukládání a načítání binárních dat ukládají přímo v rámci databáze vyžaduje trochu více kódu, než při práci s daty prostřednictvím systému souborů. Kromě toho, aby uživatel k zobrazení nebo stažení binární data musí být předkládány s adresou URL k těmto datům. Pokud jsou data uložená v systému souborů webového serveru s, adresa URL je jednoduché. Pokud jsou data uložená v databázi, ale webové stránce musí být vytvořen, které se načítají a vrátit data z databáze.
- **Širší přístup k datům binární** možná bude nutné binární data dostupná pro ostatní služby nebo aplikace, které není možné si vyžádat data z databáze. Například může být také musí být dostupné uživatelům prostřednictvím imagí spojené s každou produktu [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), v takovém případě d chceme ukládání binárních dat v systému souborů.
- **Výkon** Pokud binární data uložená v systému souborů, vyžádání a zahlcení sítě mezi serverem databáze a webového serveru bude menší než pokud binární data jsou uložena přímo v databázi.

Hlavní nevýhodou ukládání binárních dat v systému souborů je, že odděluje data z databáze. Pokud se odstraní záznam z `Products` tabulky, přidružený soubor v systému souborů webového serveru s není automaticky odstraněn. Jsme musí zapsat další kód k odstranění souboru nebo systému souborů se zaplní nevyužité, osamocené soubory. Kromě toho při zálohování databáze, musí zajišťujeme, že aby zálohy přidružené binárních dat v systému souborů, i. Přesun databáze do jiné lokality nebo server představuje podobné problémy.

Alternativně binární data mohou být uloženy přímo v databázi Microsoft SQL Server 2005 vytvořením sloupce typu `varbinary`. Stejně jako s jinými datovými typy proměnné délky, můžete zadat maximální délka binárních dat, která se můžou uchovávat v tomto sloupci. Například pokud chcete rezervovat maximálně 5 000 bajtů použijte `varbinary(5000)`; `varbinary(MAX)` umožňuje maximální velikost úložiště, přibližně 2 GB.

Hlavní výhodou ukládání binárních dat přímo do databáze je určitou úzkou svázanost mezi binárních dat a záznam v databázi. To výrazně zjednodušuje administrativní úlohy, jako je zálohování nebo přesunutí databáze do jiné lokality nebo server. Odstranění záznamu automaticky odstraní také, odpovídající binární data. Existují také další drobným výhody ukládání binárních dat v databázi. Zobrazit [ukládání binární soubory přímo v databázi pomocí technologie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) najdete podrobnější informace.

> [!NOTE]
> V systému Microsoft SQL Server 2000 a předchozími verzemi `varbinary` datový typ má maximální limit 8 000 bajtů. K ukládání binárních dat až 2 GB [ `image` datový typ](https://msdn.microsoft.com/library/ms187993.aspx) použije místo toho je potřeba. Přidání `MAX` v systému SQL Server 2005, ale `image` datový typ je zastaralá. To s i nadále podporovány pro zpětnou kompatibilitu, ale společnost Microsoft ohlásila, že `image` datový typ bude v budoucí verzi systému SQL Server odebrána.


Pokud pracujete se starší datový model může se zobrazit `image` datového typu. Databáze Northwind s `Categories` tabulka má `Picture` sloupec, který slouží k ukládání binárních dat souboru obrázku pro kategorii. Protože databáze Northwind má jeho kořenových adresářů v aplikaci Microsoft Access a dřívějších verzích systému SQL Server, je tento sloupec typu `image`.

V tomto kurzu a další tři použijeme oba přístupy. `Categories` Tabulka již obsahuje `Picture` sloupec pro ukládání binární obsah image pro kategorii. Přidáme další sloupec `BrochurePath`pro uložení v systému souborů serveru s web, který slouží k poskytování kvalita tisku, dokonalý přehled o kategorii cestu k souboru PDF.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Krok 3: Přidání`BrochurePath`sloupec, který se`Categories`tabulky

Aktuálně tabulce kategorie obsahuje pouze čtyři sloupce: `CategoryID`, `CategoryName`, `Description`, a `Picture`. Kromě těchto polí potřebujeme přidat nový, který bude odkazovat do kategorie s brožura (pokud existuje). Chcete-li přidat tento sloupec, přejděte do Průzkumníka serveru podrobnostem do tabulek, klikněte pravým tlačítkem na `Categories` tabulce a zvolte Otevřít definici tabulky (viz obrázek 5). Pokud se nezobrazí v Průzkumníku serveru, otevřete ho tak, že vyberete možnost Průzkumníku serveru v nabídce Zobrazit nebo stiskněte kombinaci kláves Ctrl + Alt + S.

Přidat nový `varchar(200)` sloupec, který se `Categories` tabulku s názvem `BrochurePath` a umožňuje `NULL` s a klikněte na ikonu Uložit (nebo stiskněte kombinaci kláves Ctrl + S).


[![Přidání BrochurePath sloupce do tabulky kategorie](uploading-files-cs/_static/image5.gif)](uploading-files-cs/_static/image5.png)

**Obrázek 5**: Přidejte `BrochurePath` sloupec, který se `Categories` tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](uploading-files-cs/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Krok 4: Aktualizace architekturu pro použití`Picture`a`BrochurePath`sloupce

`CategoriesDataTable` V Data přístup Layer (DAL) aktuálně obsahuje čtyři `DataColumn` s definované: `CategoryID`, `CategoryName`, `Description`, a `NumberOfProducts`. Když jsme navrhovali původně tohoto objektu DataTable v [vytvoření vrstvy přístupu k datům](../introduction/creating-a-data-access-layer-cs.md) kurzu `CategoriesDataTable` platili jen první tři sloupce; `NumberOfProducts` byl přidán sloupec v [pomocí záznamů Master/Detail seznam s odrážkami Seznam hlavních záznamů s podrobnostmi v prvku DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) kurzu.

Jak je popsáno v *vytvoření vrstvy přístupu k datům*, datové tabulky v datové sadě zadán tvoří obchodní objekty. Objekty TableAdapter jsou zodpovědní za komunikaci s databází a naplňování obchodních objektů s výsledky dotazu. `CategoriesDataTable` Je vyplněn `CategoriesTableAdapter`, která má tři metody pro načítání dat:

- `GetCategories()` s hlavním dotazu objektu TableAdapter provede a vrátí `CategoryID`, `CategoryName`, a `Description` pole všech záznamů v `Categories` tabulky. Hlavní dotaz je, co je používán automaticky generovanou `Insert` a `Update` metody.
- `GetCategoryByCategoryID(categoryID)` Vrátí `CategoryID`, `CategoryName`, a `Description` pole kategorie, jehož `CategoryID` rovná *categoryID*.
- `GetCategoriesAndNumberOfProducts()` -Vrátí `CategoryID`, `CategoryName`, a `Description` pole pro všechny záznamy v `Categories` tabulky. Vrátit počet produktů, které jsou spojené s každou kategorii, zabírá poddotaz.

Všimněte si, že žádná z nich vrácena dotazy `Categories` tabulky s `Picture` nebo `BrochurePath` sloupce; ani nemá `CategoriesDataTable` poskytují `DataColumn` s těchto polí. Chcete-li pracovat s obrázkem a `BrochurePath` vlastnosti, musíme nejprve přidat je do `CategoriesDataTable` a následně neaktualizují `CategoriesTableAdapter` třídy pro vracení tyto sloupce.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Přidávání`Picture`a`BrochurePath``DataColumn` s

Začněte přidáním těchto dvou sloupců `CategoriesDataTable`. Klikněte pravým tlačítkem na `CategoriesDataTable` s záhlaví, v místní nabídce vyberte možnost Přidat a pak zvolte možnosti sloupce. Tím se vytvoří nový `DataColumn` v objektu DataTable s názvem `Column1`. Přejmenujte tento sloupec na `Picture`. V okně Vlastnosti nastavte `DataColumn` s `DataType` vlastnost `System.Byte[]` (nejedná se o možnost v rozevíracím seznamu, je potřeba zadat ho v).


[![Vytvoření obrázku s názvem DataColumn, jejichž datový typ je System.Byte](uploading-files-cs/_static/image6.gif)](uploading-files-cs/_static/image7.png)

**Obrázek 6**: vytvoření `DataColumn` pojmenované `Picture` jehož `DataType` je `System.Byte[]` ([kliknutím ji zobrazíte obrázek v plné velikosti](uploading-files-cs/_static/image8.png))


Přidejte další `DataColumn` do objektu DataTable, jeho pojmenování `BrochurePath` pomocí výchozího `DataType` hodnotu (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Vrací`Picture`a`BrochurePath`hodnoty z objektu TableAdapter

Pomocí těchto dvou `DataColumn` s přidán do `CategoriesDataTable`, můžeme znovu připravený k aktualizaci `CategoriesTableAdapter`. Společnost Microsoft může mít obě tyto hodnoty sloupců vrácený v hlavním dotazu objektu TableAdapter, ale to by vrácení binárních dat pokaždé, když `GetCategories()` vyvolání metody. Místo toho aktualizovat umožňují s hlavním dotazu objektu TableAdapter vrací do stavu `BrochurePath` a vytvořte metodu načtení dalších dat, která vrací určité kategorie s `Picture` sloupce.

Aktualizovat hlavní dotaz TableAdapter, klikněte pravým tlačítkem na `CategoriesTableAdapter` s záhlaví a zvolte možnost konfigurace v místní nabídce. Tím se vyvolá průvodce konfigurací adaptéru tabulky, které jsme viděli v několika posledních kurzy ve. Aktualizovat dotaz vrací do stavu `BrochurePath` a klikněte na tlačítko Dokončit.


[![Aktualizovat seznam sloupců v příkazu SELECT také vrátit BrochurePath](uploading-files-cs/_static/image7.gif)](uploading-files-cs/_static/image9.png)

**Obrázek 7**: aktualizace v seznamu sloupců `SELECT` příkaz rovněž vracejí `BrochurePath` ([kliknutím ji zobrazíte obrázek v plné velikosti](uploading-files-cs/_static/image10.png))


Při použití příkazů jazyka SQL ad hoc pro TableAdapter, aktualizuje se seznam sloupců v hlavním dotazu aktualizuje seznam sloupců pro všechny `SELECT` metody v TableAdapter dotazu. To znamená, že `GetCategoryByCategoryID(categoryID)` metoda aktualizovala se vraťte `BrochurePath` sloupec, který může být jsme chtěli. Ale je také aktualizovat v seznamu sloupců `GetCategoriesAndNumberOfProducts()` metoda odebrání poddotazu, který vrací počet produktů pro každou kategorii! Proto musíme aktualizovat tuto metodu s `SELECT` dotazu. Klikněte pravým tlačítkem na `GetCategoriesAndNumberOfProducts()` metoda, zvolením možnosti konfigurovat a vrátit se `SELECT` dotazu zpět na původní hodnotu:


[!code-sql[Main](uploading-files-cs/samples/sample2.sql)]

V dalším kroku vytvoření nové metody TableAdapter, který vrací určité kategorie s `Picture` hodnota ve sloupci. Klikněte pravým tlačítkem na `CategoriesTableAdapter` s záhlaví a výběrem možnosti Přidat dotaz spustíte Průvodce konfigurací dotazu TableAdapter. Prvním krokem tohoto průvodce výzva, že když chceme dotazy na data pomocí ad-hoc příkazu SQL, nový uložená procedura nebo některý z existujících. Vyberte možnost použít SQL příkazy a klikněte na tlačítko Další. Protože jsme se vrací řádek, zvolte SELECT, který vrátí řádky možnost v druhém kroku.


[![Vyberte možnost použít SQL příkazy možnost](uploading-files-cs/_static/image8.gif)](uploading-files-cs/_static/image11.png)

**Obrázek 8**: Vyberte použít SQL příkazy možnost ([kliknutím ji zobrazíte obrázek v plné velikosti](uploading-files-cs/_static/image12.png))


[![Vzhledem k tomu, že dotaz vrátí záznam z tabulky kategorie, zvolte Vybrat, které vrátí řádky](uploading-files-cs/_static/image9.gif)](uploading-files-cs/_static/image13.png)

**Obrázek 9**: vzhledem k tomu, že dotaz vrátí záznam z tabulky kategorie, zvolte možnost vybrat, na které vrátí řádky ([kliknutím ji zobrazíte obrázek v plné velikosti](uploading-files-cs/_static/image14.png))


V tomto kroku zadejte následující dotaz SQL a klikněte na tlačítko Další:


[!code-sql[Main](uploading-files-cs/samples/sample3.sql)]

Posledním krokem je vybrat název pro novou metodu. Použití `FillCategoryWithBinaryDataByCategoryID` a `GetCategoryWithBinaryDataByCategoryID` zaplní, datové tabulky a vrátit objekt DataTable vzory, v uvedeném pořadí. Kliknutím na Dokončit dokončíte průvodce.


[![Zvolte názvy pro metody s TableAdapter](uploading-files-cs/_static/image10.gif)](uploading-files-cs/_static/image15.png)

**Obrázek 10**: Zvolte názvy pro TableAdapter s metod ([kliknutím ji zobrazíte obrázek v plné velikosti](uploading-files-cs/_static/image16.png))


> [!NOTE]
> Po dokončení Průvodce konfigurací dotazu adaptér tabulka může se zobrazit dialogové okno oznamující, že nový text příkazu vrací data se schématem liší od schématu hlavního dotazu. V krátkém průvodci je konstatujme, že s hlavním dotazu objektu TableAdapter `GetCategories()` vrátí odlišné schéma než ten, který jsme právě vytvořili. Ale to je jak chceme, abyste tuto zprávu můžete ignorovat.


Také, mějte na paměti, pokud používáte SQL příkazy ad-hoc a pomocí průvodce můžete změnit TableAdapter s hlavním dotazu někdy později v čase, se bude měnit `GetCategoryWithBinaryDataByCategoryID` metody s `SELECT` zahrnout pouze tyto sloupce ze seznamu sloupců příkazu s Hlavní dotaz (to znamená, že se odeberou `Picture` sloupce z dotazu). Budete muset ručně aktualizovat seznam sloupců se vraťte `Picture` sloupce, fungují podobně jako `GetCategoriesAndNumberOfProducts()` metoda dříve v tomto kroku.

Po přidání obou `DataColumn` s `CategoriesDataTable` a `GetCategoryWithBinaryDataByCategoryID` metodu `CategoriesTableAdapter`, těchto tříd v Návrháři datové sady typu by měl vypadat jako na snímku obrazovky v obrázek 11.


![Návrhář DataSet obsahuje nové sloupce a – metoda](uploading-files-cs/_static/image11.gif)

**Obrázek 11**: Návrhář DataSet obsahuje nové sloupce a – metoda


## <a name="updating-the-business-logic-layer-bll"></a>Aktualizace vrstvy obchodní logiky (BLL)

Pomocí vrstvy DAL aktualizovat, už jen zbývá k posílení obchodní logiky vrstvy (BLL) obsahovat metodu pro novou `CategoriesTableAdapter` metody. Přidejte následující metodu do `CategoriesBLL` třídy:


[!code-csharp[Main](uploading-files-cs/samples/sample4.cs)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Krok 5: Nahrát soubor z klienta na webový server

Při shromažďování binárních dat, často tato data pochází koncovým uživatelem. Pokud chcete zaznamenat tyto informace, musí uživatel moct nahrát soubor z počítače na webový server. Odesílaná data pak musí být integrovaná s datovým modelem, který může to znamenat ukládání souboru do systému souborů s webového serveru a přidání cesty k souboru v databázi nebo zápis binární obsah přímo do databáze. V tomto kroku podíváme na to, jak chcete, aby uživatel nahrát na server soubory z počítače. V dalším kurzu jsme vám zapnout pozornost na integraci nahraný soubor s datovým modelem.

ASP.NET 2.0 s novou [FileUpload webový ovládací prvek](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) poskytuje mechanismus pro uživatelům odeslat soubor z počítače na webový server. Ovládací prvek FileUpload vykreslí jako `<input>` elementu jehož `type` atribut je nastaven na soubor, který prohlížeče zobrazí jako textové pole pomocí tlačítka Procházet. Kliknutím na tlačítko Procházet zobrazí dialogové okno, ze kterého může uživatel vybrat soubor. Formulář, když se pošle zpátky s obsahem vybraný soubor jsou poslat spolu s zpětné volání. Informace o nahraný soubor na straně serveru, jsou přístupné prostřednictvím FileUpload ovládacího prvku s vlastností.

Abychom si předvedli nahrávání souborů, otevřete `FileUpload.aspx` stránku `BinaryData` složku, přetáhněte FileUpload ovládacího prvku z panelu nástrojů do návrháře a nastavení ovládacího prvku s `ID` vlastnost `UploadTest`. Dále přidejte ovládací prvek tlačítko webového nastavení jeho `ID` a `Text` vlastností `UploadButton` a nahrajte soubor vybrané, v uvedeném pořadí. A konečně, umístěte ovládací prvek popisek webové pod tlačítko, vymažte jeho `Text` vlastnost a nastavte jeho `ID` vlastnost `UploadDetails`.


[![Přidání ovládacího prvku odesílání souborů při odpovědích na stránku ASP.NET](uploading-files-cs/_static/image12.gif)](uploading-files-cs/_static/image17.png)

**Obrázek 12**: Přidání ovládacího prvku odesílání souborů při odpovědích na stránce ASP.NET ([kliknutím ji zobrazíte obrázek v plné velikosti](uploading-files-cs/_static/image18.png))


Zobrazí obrázek 13 tuto stránku při prohlížení prostřednictvím prohlížeče. Všimněte si, že kliknete na tlačítko Procházet zobrazí výběr dialogového okna souboru, které uživateli umožňují vybrat soubor z počítače. Jakmile byl vybrán soubor, kliknutím na tlačítko Odeslat vybraný soubor vyvolá zpětné volání, která odešle binární obsah s vybraný soubor na webový server.


[![Uživatel může vybrat soubor k odeslání z jejich počítače k serveru](uploading-files-cs/_static/image13.gif)](uploading-files-cs/_static/image19.png)

**Obrázek 13**: uživatel může vybrat soubor k nahrání z jejich počítače k serveru ([kliknutím ji zobrazíte obrázek v plné velikosti](uploading-files-cs/_static/image20.png))


Zpětné volání nahraný soubor je ukládat do systému souborů nebo binární data je možné pracovat s přímo prostřednictvím Stream. V tomto příkladu umožní s vytvořit `~/Brochures` složce a uložit nahraný soubor. Začněte přidáním `Brochures` složky do lokality jako podsložku ke kořenovému adresáři. Dále vytvořte obslužnou rutinu události pro `UploadButton` s `Click` událostí a přidejte následující kód:


[!code-csharp[Main](uploading-files-cs/samples/sample5.cs)]

Ovládací prvek FileUpload poskytuje celou řadu vlastností pro práci s odesílaná data. Například [ `HasFile` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) označuje, zda soubor byl odeslán uživatelem, zatímco [ `FileBytes` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) poskytuje přístup k nahrané binárních dat jako pole bajtů. `Click` Obslužná rutina události začíná tím, že zajišťuje, že se soubor odeslal. Pokud souboru je nahraná, popisek se zobrazuje název uloženého souboru, jeho velikost v bajtech a jeho typ obsahu.

> [!NOTE]
> K zajištění, že uživatel nahraje soubor můžete zkontrolovat `HasFile` vlastnost a zobrazí upozornění, pokud ho s `false`, nebo můžete použít [ovládací prvek RequiredFieldValidator](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) místo.


FileUpload s `SaveAs(filePath)` uloží nahraný soubor do zadaného *filePath*. *filePath* musí být *fyzická cesta* (`C:\Websites\Brochures\SomeFile.pdf`) spíše než *virtuální* *cesta* (`/Brochures/SomeFile.pdf`). [ `Server.MapPath(virtPath)` Metoda](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) obsahuje virtuální cestu a vrátí odpovídající fyzická cesta. Virtuální cesta, která následuje `~/Brochures/fileName`, kde *fileName* je název uloženého souboru. Zobrazit [pomocí Server.MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) pro další informace o virtuálních a fyzických cest a používání `Server.MapPath`.

Po dokončení `Click` obslužná rutina události, využít k otestování stránky v prohlížeči. Klikněte na tlačítko Procházet a vyberte soubor z pevného disku a pak klikněte na tlačítko Nahrát soubor vybrali. Zpětné volání pošle obsah na vybraný soubor na webový server, který se pak zobrazí informace o souboru před uložením do `~/Brochures` složky. Po nahrání souboru, vraťte se do sady Visual Studio a klikněte na tlačítko Aktualizovat v Průzkumníku řešení. Soubor, který jste právě nahráli ve složce ~/Brochures byste měli vidět!


[![EvolutionValley.jpg soubor se odeslal do webového serveru](uploading-files-cs/_static/image14.gif)](uploading-files-cs/_static/image21.png)

**Obrázek 14**: soubor `EvolutionValley.jpg` byl odeslán na webový server ([kliknutím ji zobrazíte obrázek v plné velikosti](uploading-files-cs/_static/image22.png))


![EvolutionValley.jpg byl uložen do složky ~/Brochures](uploading-files-cs/_static/image15.gif)

**Obrázek 15**: `EvolutionValley.jpg` byla uložena do `~/Brochures` složky


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Odlišnosti s ukládáním nahraných souborech do systému souborů

Existuje několik odlišností, které je nutné vyřešit při ukládání nahrávání souborů do systému souborů webového serveru s. Nejdřív se tam s problém zabezpečení. Uložte soubor do systému souborů, kontext zabezpečení, pod kterou je prováděna stránky ASP.NET musí mít oprávnění k zápisu. Webový Server ASP.NET Development běží v kontextu aktuálního uživatelského účtu. Pokud používáte Microsoft s (Internetová informační služba) jako webový server, kontext zabezpečení závisí na verzi služby IIS a jeho konfigurace.

Dalším problémem ukládat soubory do systému souborů zásadní kolem pojmenování souborů. V současné době všechny nahrané soubory na naší stránce uloží `~/Brochures` adresářem pomocí služby se stejným názvem jako soubor s klientského počítače. Pokud uživatel A nahraje brožuru s názvem `Brochure.pdf`, soubor se uloží jako `~/Brochure/Brochure.pdf`. Ale co když nějakou dobu novější uživateli B nahraje si brožuru o jiný soubor, který se stane, chcete-li mít stejný název souboru (`Brochure.pdf`)? S kódem máme dnes, uživatel s soubor se přepíše nahrávání s uživateli B.

Existuje několik technik pro řešení konfliktů název souboru. Jednou z možností je zakázat nahrávání souboru, pokud existuje se stejným názvem už existuje. Díky tomuto přístupu, když se uživatel B se pokusí odeslat soubor s názvem `Brochure.pdf`, systém nebude uložit soubor a místo toho zobrazí zpráva uživateli B soubor přejmenujte a zkuste to znovu. Další možností je uložte soubor pomocí jedinečný název souboru, který může být [globálně jedinečný identifikátor (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) nebo hodnotu od odpovídajících záznamů s primární sloupce klíče databáze (za předpokladu, že je přidružené k odeslání konkrétního řádku v datovém modelu). V dalším kurzu prozkoumáme těchto možností podrobněji.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Výzvy spojené s velmi velkého objemu binárních dat

Tyto kurzy předpokládá, že je binární data zaznamenaná středně velká velikost. Pracujete s velmi velkým množstvím binární datové soubory, které jsou několika megabajtů nebo větší zavádí nové výzvy, které jsou nad rámec těchto kurzů. Například ve výchozím nastavení technologie ASP.NET, bude taková nahrávání více než 4 MB, i když je možné nakonfigurovat pomocí [ `<httpRuntime>` element](https://msdn.microsoft.com/library/e1f13641.aspx) v `Web.config`. Služba IIS ukládá příliš vlastní omezení velikosti nahrávání souborů. Zobrazit [IIS nahrát soubor velikost](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) Další informace. Čas potřebný k nahrávání velkých souborů navíc může překročit výchozí 110 sekund, po které ASP.NET bude čekat požadavek. Existují také výkon a paměť problémy, které vznikají při práci s velkými soubory.

Je FileUpload ovládacího prvku nepraktické pro nahrávání velkých souborů. Jako obsah souboru s jsou odeslání na server, musí koncový uživatel dokončení čekat bez jakéhokoli potvrzení, že jejich nahrávání probíhá. To není tolik problém, při práci s menší soubory, které se dají nahrát, během několika sekund, ale může být problém při práci s většími soubory, které může trvat k nahrání. Existuje řada různých výrobců souboru nahrávání ovládací prvky, které jsou vhodnější pro zpracování velkých nahrávání a mnohé z těchto dodavatelů poskytují indikátory průběhu a ActiveX nahrát správci, které představují mnohem zajímavější činnost koncového uživatele.

Pokud vaše aplikace potřebuje pro zpracování velkých souborů, budete muset pečlivě prozkoumat problémů a najděte vhodné řešení pro vaše konkrétní potřeby.

## <a name="summary"></a>Souhrn

Vytvoření aplikace, kterou je potřeba zaznamenat binárních dat představuje určité problémy. V tomto kurzu Prozkoumali jsme první dva: rozhodování, kam chcete uložit binární data a která uživatelům umožňuje nahrát binární obsah prostřednictvím webové stránky. V následujících třech kurzy uvidíme, jak přidružit záznam v databázi nahraných dat, jakož i způsob zobrazení binárních dat společně s jeho datová pole text.

Všechno nejlepší programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Použití vysoké hodnoty datových typů](https://msdn.microsoft.com/library/ms178158.aspx)
- [Rychlé starty fileUpload ovládacího prvku](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [Serverový ovládací prvek ASP.NET 2.0 FileUpload](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [Tmavě straně nahrávání souborů](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Teresy Murphy a Bernadette Leigh. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](displaying-binary-data-in-the-data-web-controls-cs.md)
