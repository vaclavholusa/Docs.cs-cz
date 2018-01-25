---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
title: "Parametrizované dotazy pomocí SqlDataSource (VB) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu budeme pokračovat naše pohled na ovládací prvek SqlDataSource a zjistěte, jak definovat parametrizované dotazy. Parametry mohou být zadány oba decla..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: e322f34c-83b7-41ea-ab65-ab1e0bdcc609
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: b1cda18620a970c45b05039dd380c393e3854889
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="using-parameterized-queries-with-the-sqldatasource-vb"></a>Parametrizované dotazy pomocí SqlDataSource (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_VB.exe) nebo [stáhnout PDF](using-parameterized-queries-with-the-sqldatasource-vb/_static/datatutorial48vb1.pdf)

> V tomto kurzu budeme pokračovat naše pohled na ovládací prvek SqlDataSource a zjistěte, jak definovat parametrizované dotazy. Parametry lze zadat deklarativně i prostřednictvím kódu programu a mohou být vyžádány z mnoha různých umístění, jako je například řetězec dotazu, relace stavu, další ovládací prvky a další.


## <a name="introduction"></a>Úvod

V předchozích kurzu jsme viděli, jak pomocí ovládacího prvku SqlDataSource načíst data přímo z databáze. Pomocí Průvodce konfigurace zdroje dat, můžeme může zvolit databáze a pak buď: Vyberte sloupce, které chcete vrátit z tabulek nebo zobrazení; Zadejte vlastní příkaz SQL; nebo použijte uloženou proceduru. Jestli výběr sloupců v tabulce nebo zobrazení nebo zadáním vlastního příkazu SQL, SqlDataSource řízení s `SelectCommand` vlastnost je přiřazen výsledné SQL ad-hoc `SELECT` příkaz a je to `SELECT` příkaz, který je spuštěna při SqlDataSource s `Select()` metoda je volána (buď prostřednictvím kódu programu, nebo automaticky z dat ovládací prvek webu).

SQL `SELECT` příkazy používají v předchozích kurzu s ukázky chybějící `WHERE` klauzule. V `SELECT` příkaz, `WHERE` klauzuli lze použít k omezení výsledky vrácené. Například pokud chcete zobrazit názvy produktů pro výpočet nákladů více než 50,00 $, můžeme použít následující dotaz:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample1.sql)]

Obvykle hodnoty používané v `WHERE` klauzule jsou určení některé externí zdroj, například hodnotu querystring, proměnné relace nebo uživatelský vstup z ovládacího prvku na webové stránce. V ideálním případě tyto vstupy zadávají prostřednictvím *parametry*. S Microsoft SQL Server, jsou rozlišeny pomocí parametrů `@parameterName`jako v:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample2.sql)]

Ve třídě SqlDataSource podporuje parametrických dotazů, pro `SELECT` příkazy a `INSERT`, `UPDATE`, a `DELETE` příkazy. Kromě toho hodnoty parametrů můžete být automaticky načtený z různých zdrojů řetězec dotazu, stav relace, ovládací prvky na stránce a tak dále nebo lze přiřadit prostřednictvím kódu programu. V tomto kurzu jsme zobrazí, jak definovat také parametrizované dotazy, jak zadat parametr hodnoty deklarativně i prostřednictvím kódu programu.

> [!NOTE]
> V předchozích kurzu jsme porovná ObjectDataSource, který byl naše nástroje přes první 46 kurzy s SqlDataSource, poznamenat jejich koncepční podobnosti. Tyto podobnosti také rozšířit parametry. Parametry s ObjectDataSource namapované na vstupní parametry pro metody v vrstvu obchodní logiky. S SqlDataSource jsou parametry definované přímo v dotazu SQL. Mají obě ovládací prvky kolekce parametrů pro jejich `Select()`, `Insert()`, `Update()`, a `Delete()` metody a jak může mít tyto hodnoty parametrů naplněny z předem definovaných zdroje (hodnoty řetězce dotazu, proměnné relace a tak dále ) nebo přiřazené prostřednictvím kódu programu.


## <a name="creating-a-parameterized-query"></a>Vytvoření parametrického dotazu

Průvodce konfigurace zdroje dat SqlDataSource ovládacího prvku s nabízí tři cesty pro definování provedení příkazu pro načtení záznamů databáze:

- Výběrem sloupce z existující tabulky nebo zobrazení
- Zadáním vlastního příkazu jazyka SQL, nebo
- Výběrem uložené procedury

Při výběru sloupců z existující tabulky nebo zobrazení, parametry `WHERE` klauzule musí být zadán prostřednictvím přidat `WHERE` dialogové okno klauzule. Při vytváření vlastní příkazu jazyka SQL, ale můžete zadat parametry přímo do `WHERE` – klauzule (pomocí `@parameterName` k označení jednotlivých parametrů). A [uložené procedury](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) se skládá z jedné nebo více příkazů SQL a tyto příkazy lze nastavit parametry. Parametry použité v příkazy SQL, ale musí být předán v jako vstupní parametry uložené procedury.

Vzhledem k tomu, že vytvoření parametrického dotazu závisí na tom SqlDataSource s `SelectCommand` je zadaný, umožňují s podívejte se na všech tří následujících metod. Chcete-li začít, otevřete `ParameterizedQueries.aspx` stránky v `SqlDataSource` složku, přetáhněte ovládací prvek SqlDataSource z panelu nástrojů na návrháře a nastavit jeho `ID` k `Products25BucksAndUnderDataSource`. Potom klikněte na odkaz Konfigurace zdroje dat z ovládacího prvku s inteligentním. Vyberte databázi, kterou chcete použít (`NORTHWINDConnectionString`) a klikněte na tlačítko Další.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Krok 1: Přidání`WHERE`klauzule při výběru sloupců z tabulky nebo zobrazení

Když vyberete data k návratu z databáze pomocí ovládacího prvku SqlDataSource, Průvodce konfigurace zdroje dat umožňuje jednoduše vyberte sloupce, které chcete vrátit z existující tabulky nebo zobrazení (viz obrázek 1). Takovém případě budou automaticky vytvoří SQL `SELECT` příkaz, který je co je odeslal do databáze při SqlDataSource s `Select()` metoda je volána. Jako jsme to udělali v předchozím kurzu, vyberte z rozevíracího seznamu tabulky produktů a zkontrolujte `ProductID`, `ProductName`, a `UnitPrice` sloupce.


[![Vyberte sloupce, které chcete vrátit z tabulek nebo zobrazení](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.png)

**Obrázek 1**: Vyberte sloupce k návratu z tabulek nebo zobrazení ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.png))


Zahrnout `WHERE` klauzuli v `SELECT` příkaz, klikněte na tlačítko `WHERE` tlačítko, které se vyvolá přidat `WHERE` klauzule dialogu (viz obrázek 2). Přidání parametru k omezení výsledků vrácených `SELECT` se nejprve vybrat sloupec pro filtrování dat pomocí dotazu. V dalším kroku vyberte operátor, který chcete použít pro filtrování (=, &lt;, &lt;=, &gt;a tak dále). Nakonec vyberte zdroj s hodnotu parametru, jako třeba ze stavu řetězci dotazu nebo relace. Po dokončení konfigurace parametru, klikněte na tlačítko Přidat se zahrnuje v `SELECT` dotazu.

V tomto příkladu umožňují s vrátit pouze výsledků kde `UnitPrice` hodnota je menší než nebo rovna 25,00 $. Proto vyberte `UnitPrice` z rozevíracího seznamu sloupců a &lt;= z rozevíracího seznamu operátor. Při použití hodnoty parametru pevně (např. $25,00) nebo pokud hodnota parametru je třeba zadat prostřednictvím kódu programu, vyberte jeden z rozevíracího seznamu zdrojů. Potom zadejte hodnotu parametru pevně do textového pole hodnota 25,00 a dokončete proces kliknutím na tlačítko Přidat.


[![Omezit výsledky vrácené přidat WHERE klauzule dialogové okno](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.png)

**Obrázek 2**: omezení vráceny výsledky z přidat `WHERE` dialogové okno klauzuli ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.png))


Po přidání parametru, klikněte na tlačítko OK se vrátíte do konfigurace zdroje dat průvodce. `SELECT` Příkaz v dolní části průvodce by měl nyní zahrnují `WHERE` klauzuli with parametr s názvem `@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample3.sql)]

> [!NOTE]
> Pokud zadáte více podmínek v `WHERE` klauzule z přidat `WHERE` klauzule dialogové okno, průvodce připojí, je `AND` operátor. Pokud je nutné zahrnout `OR` v `WHERE` klauzule (například `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`) budete muset vytvořit `SELECT` příkaz pomocí vlastní obrazovky příkaz SQL.


Dokončete konfiguraci SqlDataSource (klikněte na tlačítko Další, pak Dokončit) a zkontrolujte SqlDataSource s deklarativní. Nyní zahrnuje kód `<SelectParameters>` kolekce, která obsahuje zdroje pro parametry v `SelectCommand`.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample4.aspx)]

Když SqlDataSource s `Select()` metoda je volána, `UnitPrice` hodnota parametru (25,00) se použije pro `@UnitPrice` parametr ve `SelectCommand` před odesláním do databáze. Net výsledkem je pouze tyto produkty, které jsou menší než nebo rovna $25,00 vrácených `Products` tabulky. Potvrďte, přidat GridView na stránku, vazbu k tomuto zdroji dat a zobrazte stránku prostřednictvím prohlížeče. Měli byste vidět jenom tyto produkty uvedené, které jsou menší než nebo rovna $25,00, potvrdí obrázku 3.


[![Se zobrazí pouze ty produkty menší než nebo rovno $25,00](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.png)

**Obrázek 3**: se zobrazí pouze ty produkty menší než nebo rovno 25,00 Kč ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Krok 2: Přidání parametrů příkazu vlastní SQL

Při přidání vlastního příkazu SQL můžete zadat `WHERE` klauzule explicitně nebo zadejte hodnotu v buňce Tvůrce dotazů filtru. Ukazuje to umožní s zobrazit právě tyto produkty v GridView, jejichž ceny je nižší než zadané prahové hodnoty. Začněte přidáním textové pole k `ParameterizedQueries.aspx` stránku a shromažďovat tato prahová hodnota od uživatele. Nastavit textové pole s `ID` vlastnost `MaxPrice`. Přidání ovládacího prvku tlačítko a nastavit jeho `Text` vlastnost k produktům odpovídající zobrazení.

Přetáhněte GridView na stránku a z jeho inteligentních značek můžete vytvořit nové SqlDataSource s názvem `ProductsFilteredByPriceDataSource`. Z Průvodce konfigurace zdroje dat, přejděte k určení vlastního příkazu SQL nebo uloženou proceduru obrazovky (viz obrázek 4) a zadejte následující dotaz:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample5.sql)]

Po zadání dotazu (ručně nebo pomocí Tvůrce dotazů), klikněte na tlačítko Další.


[![Vrátí jenom tyto produkty menší než hodnota parametru](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.png)

**Obrázek 4**: vrátí jenom ty produkty menší než nebo rovno hodnotu parametru ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.png))


Vzhledem k tomu, že dotaz obsahuje parametry, na další obrazovce v Průvodci nám vyzve ke zdroji hodnot parametrů. Vyberte ovládací prvek z rozevíracího seznamu parametr zdroje a `MaxPrice` (ovládacího prvku textového pole s `ID` hodnotu) z rozevíracího seznamu ControlID. Můžete také zadat volitelnou výchozí hodnotu používat v případě, kdy uživatel nebyl zadali jakýkoli text do `MaxPrice` textové pole. Prozatím, nezadávejte výchozí hodnotu.


[![MaxPrice textové pole s Text – vlastnost se používá jako zdroj parametru](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.png)

**Obrázek 5**: `MaxPrice` textové pole s `Text` vlastnost se používá jako zdroj parametru ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.png))


Dokončete průvodce Konfigurace zdroje dat kliknutím na další a pak dokončit. Deklarativní GridView, textové pole, tlačítka a SqlDataSource následovně:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample6.aspx)]

Všimněte si, že parametr v rámci SqlDataSource s `<SelectParameters>` oddíl `ControlParameter`, což zahrnuje další vlastnosti, například `ControlID` a `PropertyName`. Pokud SqlDataSource s `Select()` metoda je volána, `ControlParameter` získá hodnotu z zadanou vlastnost ovládací prvek webové a přiřadí ji k odpovídající parametr v `SelectCommand`. V tomto příkladu `MaxPrice` s Text – vlastnost se používá jako `@MaxPrice` hodnotu parametru.

Trvat několik minut, chcete-li zobrazit tuto stránku prostřednictvím prohlížeče. Při první návštěvě stránky, nebo vždy, když `MaxPrice` textové pole chybí hodnota v GridView jsou zobrazeny žádné záznamy.


[![Žádné záznamy se zobrazí při the MaxPrice textového je prázdná](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.png)

**Obrázek 6**: žádné záznamy se zobrazí, když `MaxPrice` textového je prázdná ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.png))


Důvodem jsou zobrazeny žádné produktů je proto, že ve výchozím nastavení je prázdný řetězec pro hodnotu parametru převést do databáze `NULL` hodnotu. Protože při porovnání `[UnitPrice] <= NULL` vždy vyhodnotí jako hodnotu False, jsou vráceny žádné výsledky.

Zadejte hodnotu do textového pole, jako je 5.00 a klikněte na tlačítko produkty odpovídající zobrazení. Na zpětné volání SqlDataSource informuje, že došlo ke změně GridView než její parametr zdroje. V důsledku toho GridView znovu připojí k SqlDataSource, zobrazování těchto produktů, které je menší než nebo rovna na 5.00 $.


[![Jsou zobrazeny produkty menší než nebo rovno $5.00](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.png)

**Obrázek 7**: produkty menší než nebo rovno $5.00 zobrazují ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.png))


## <a name="initially-displaying-all-products"></a>Původně zobrazení všech produktů

Místo zobrazení žádné produkty při prvním načtení stránky, může chceme zobrazit *všechny* produkty. Jeden ze způsobů, jak zobrazit seznam všech produktů vždy, když `MaxPrice` textové pole je prázdné je nastavit výchozí hodnotu parametru s některé insanely vysokou hodnotu, jako je 1000000, protože s nepravděpodobné, že Northwind Traders se někdy mít inventáře jejichž jednotkové ceny překročí $ 1 000 000. Tento postup je však shortsighted a nemusí fungovat v jiných situacích.

V předchozích kurzech - [deklarativní parametry](../basic-reporting/declarative-parameters-vb.md) a [a podrobností filtrování s rozevírací seznam](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) jsme se potýkají s podobný problém. Naše řešení existuje byl uvést tuto logiku do vrstvu obchodní logiky. Konkrétně BLL zkontrolován hodnota příchozí a pokud se jednalo o `NULL` nebo některé vyhrazenou hodnotou, volání se směruje na DAL metodu, která vrátí všechny záznamy. Pokud hodnota příchozí normální filtrování hodnotu, došlo k volání do metody DAL, příkaz SQL, který používá parametrizované `WHERE` klauzuli with zadaná hodnota.

Bohužel jsme nepoužívat architekturu, při použití SqlDataSource. Místo toho je potřeba přizpůsobit příkaz jazyka SQL se inteligentně získat všechny záznamy, pokud `@MaximumPrice` parametr `NULL` nebo vyhrazenou hodnotou. Pro toto cvičení umožňují s mít ji, že pokud `@MaximumPrice` parametr rovná `-1.0`, pak *všechny* záznamy mají být vráceny (`-1.0` funguje jako vyhrazené hodnota, protože žádný produkt může mít negativní `UnitPrice`hodnotu). K tomu můžeme použít následující příkaz SQL:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample7.sql)]

To `WHERE` klauzule vrací *všechny* zaznamená, pokud `@MaximumPrice` rovná parametru `-1.0`. Pokud hodnota parametru není `-1.0`, pouze produkty, jejichž `UnitPrice` je menší než nebo rovno `@MaximumPrice` hodnota parametru jsou vráceny. Nastavením na výchozí hodnotu `@MaximumPrice` parametru `-1.0`, při prvním načtení stránky (nebo vždy, když `MaxPrice` textové pole je prázdné), `@MaximumPrice` bude mít hodnotu `-1.0` a zobrazí se všechny produkty.


[![Teď všechny produkty, které se zobrazí při the MaxPrice textového je prázdná](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.png)

**Obrázek 8**: nyní všechny produkty, které se zobrazí, když `MaxPrice` textového je prázdná ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image16.png))


Existuje několik upozornění pro mějte na paměti, tento přístup. První, uvědomte si, že je typ dat parametru s vyvozena na základě jeho použití s v dotazu SQL. Pokud změníte `WHERE` klauzule z `@MaximumPrice = -1.0` k `@MaximumPrice = -1`, modul runtime zpracovává parametr jako celé číslo. Pokud pak pokus o přiřazení `MaxPrice` textové pole na desetinnou hodnotu (např. 5.00), dojde k chybě protože 5.00 nemůže převést na celé číslo. Chcete-li to opravit, buď Ujistěte se, že používáte `@MaximumPrice = -1.0` v `WHERE` klauzuli nebo lepší ještě nastavit `ControlParameter` objekt s `Type` vlastnosti Decimal.

Za druhé, a to přidáním `OR @MaximumPrice = -1.0` k `WHERE` klauzuli modul dotazu nelze použít indexu na `UnitPrice` (za předpokladu, že jeden existuje), což by vedlo k prohledávání tabulky. Pokud jsou dostatečně velký počet záznamů v může ovlivnit výkon `Products` tabulky. Lepší přístup by bylo přesunout tuto logiku do uložené procedury kde `IF` příkaz by buď provést `SELECT` dotaz ze `Products` tabulky bez `WHERE` klauzuli, pokud je třeba vrátit všechny záznamy nebo jeden jejichž `WHERE` klauzule WHERE obsahuje jenom `UnitPrice` kritéria, tak, aby bylo možné použít indexu.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Krok 3: Vytvoření a použití parametrizovaného uložené procedury

Uložené procedury můžete zahrnout sadu vstupní parametry, které lze poté použít příkazy SQL definovaný v rámci uložené procedury. Při konfiguraci SqlDataSource používat uložené procedury, která přijímá vstupní parametry, tyto hodnoty parametrů lze pomocí stejné techniky stejně jako u příkazů SQL ad hoc.

Pro ilustraci použití uložených procedur v SqlDataSource, umožňují s vytvořit nové uloženou proceduru v databázi Northwind s názvem `GetProductsByCategory`, který přijme parametr s názvem `@CategoryID` a vrátí všechny sloupce produkty jejichž `CategoryID` sloupec odpovídá `@CategoryID`. Chcete-li vytvořit uložené procedury, přejděte do Průzkumníka serveru a k podrobnostem `NORTHWND.MDF` databáze. (Pokud tento t Průzkumníka serveru, otevřete ho přechodem do nabídky Zobrazit a výběrem možnosti Průzkumníka serveru.)

Z `NORTHWND.MDF` databáze, klikněte pravým tlačítkem na složku, uložené procedury, zvolte Přidat novou uloženou proceduru a zadejte následující syntaxi:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample8.sql)]

Klikněte na uložit ikonu (Ctrl + S) pro uložení uložené procedury. Uložená procedura můžete otestovat tak, že kliknete pravým tlačítkem ve složce uložené procedury a výběr Execute. Toto zobrazí výzvu pro uloženou proceduru s parametry (`@CategoryID`, u této instance), po který výsledky se zobrazí v okně výstupu.


[![GetProductsByCategory uložené procedury při spuštění s @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image17.png)

**Obrázek 9**: `GetProductsByCategory` uloženou proceduru při spuštění s `@CategoryID` 1 ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image18.png))


Umožní použít tuto uloženou proceduru zobrazíte všechny produkty v kategorii Nápoje v GridView s. Přidat nové GridView na stránku a navázat jej nové SqlDataSource s názvem `BeverageProductsDataSource`. Pokračovat k určení vlastního příkazu SQL nebo uloženou proceduru obrazovky, vyberte přepínač uložené procedury a vyberte `GetProductsByCategory` uložené procedury z rozevíracího seznamu.


[![Vyberte GetProductsByCategory uložené procedury z rozevíracího seznamu](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image19.png)

**Obrázek 10**: vyberte `GetProductsByCategory` uloženou proceduru v rozevíracím seznamu ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image20.png))


Vzhledem k tomu, že přijímá vstupní parametr uložené procedury (`@CategoryID`), kliknutím na tlačítko Další výzvy nám určit zdroj pro tuto hodnotu s parametr. Do této skupiny nápoje `CategoryID` je 1, tak v žádné ponechte parametr zdrojového rozevíracího seznamu a zadejte do textového pole Výchozí hodnota 1.


[![Použít pevně hodnotu 1, který vrátí produktů v kategorii Nápoje](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image21.png)

**Obrázek 11**: použít Hard-Coded hodnotu 1, který vrátí produktů v kategorii nápoje ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image22.png))


Jak ukazuje následující deklarativní značky, pokud pomocí uložené procedury SqlDataSource s `SelectCommand` je nastavena na název uložené procedury a [ `SelectCommandType` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) je nastaven na `StoredProcedure`, která udává která `SelectCommand` je název uložené procedury místo příkazu SQL ad hoc.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample9.aspx)]

Otestování stránku v prohlížeči. Jsou zobrazeny pouze produkty, které patří do kategorie Nápoje, i když *všechny* produktu se zobrazí pole od `GetProductsByCategory` uložené procedury vrátí všechny sloupce z `Products` tabulky. Nemůžeme samozřejmě může omezit nebo přizpůsobit pole, zobrazí v GridView z dialogového okna GridView s upravit sloupce.


[![Zobrazí se všechny vlastnost nápojů](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image23.png)

**Obrázek 12**: jsou zobrazeny všechny vlastnost nápojů ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Krok 4: Prostřednictvím kódu programu vyvolání SqlDataSource s`Select()`– příkaz

V příkladech jsme jste viděli v předchozí kurzu a v tomto kurzu, pokud mají vázané ovládací prvky SqlDataSource přímo na GridView. Data ovládacího prvku s SqlDataSource, ale prostřednictvím kódu programu přístupná a mohou uvedené v kódu. To může být zvláště užitečné při dotazu na data potřebujete zkontrolovat, ale tento t potřeba ji zobrazit. Místo pro zápis všechny standardní kódu technologie ADO.NET pro připojení k databázi, zadejte příkaz a načtěte výsledky, můžete je nechat SqlDataSource zpracovávat tento monotónní kód.

Pro ilustraci práci s SqlDataSource s dat prostřednictvím kódu programu, představte, že nadřízeného dosáhla můžete s požadavek na vytvoření webové stránky, která zobrazí název náhodně vybrané kategorie a její přidružené produkty. To znamená, když uživatel navštíví tuto stránku, chceme náhodně zvolit kategorii z `Categories` tabulky, zobrazovaný název kategorie a potom zobrazí seznam produktů, které patří do této kategorie spadají.

K tomu potřebujeme dvou SqlDataSource ovládacích prvků jeden se získat náhodné kategorie z `Categories` tabulky a druhou pro získání kategorii s produkty. Budete využijeme SqlDataSource, který načte záznam náhodných kategorie v tomto kroku; Krok 5 zjistí věnujte SqlDataSource, který načte kategorie s produkty.

Začněte přidáním SqlDataSource k `ParameterizedQueries.aspx` a nastavit jeho `ID` k `RandomCategoryDataSource`. Nakonfigurujte ji tak, aby používala následujícího dotazu SQL:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample10.sql)]

`ORDER BY NEWID()`Vrátí náhodné řazení záznamů (v tématu [pomocí `NEWID()` náhodně řazení záznamů](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1`Vrátí první záznam ze sady výsledků. V kostce řečeno, tento dotaz vrací `CategoryID` a `CategoryName` hodnoty sloupců z jedné, náhodně vybrané kategorie.

K zobrazení kategorie s `CategoryName` hodnotu, přidání ovládacího prvku popisek na stránku, nastavte jeho `ID` vlastnost `CategoryNameLabel`a vymažte jeho `Text` vlastnost. Prostřednictvím kódu programu načíst data z ovládacího prvku SqlDataSource, musíme vyvolání jeho `Select()` metoda. [ `Select()` Metoda](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) očekává, že jeden vstupní parametr typu [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), která určuje, jak by měla být data messaged před vrácením. To může obsahovat pokyny, řazení a filtrování dat a data, která webové ovládací prvky při řazení nebo procházení dat z ovládacího prvku SqlDataSource používá. Pro náš příklad, ale jsme nejsou zobrazeny t potřeba data před vrácením upravit a proto bude předávat `DataSourceSelectArguments.Empty` objektu.

`Select()` Metoda vrátí objekt, který implementuje `IEnumerable`. Vrátí typ přesné závisí na hodnotě ovládacího prvku SqlDataSource s [ `DataSourceMode` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Jak je popsáno v předchozí kurzu, tuto vlastnost lze nastavit na hodnotu buď `DataSet` nebo `DataReader`. Pokud nastavena na `DataSet`, `Select()` metoda vrátí [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) objekt; je-li nastavena na `DataReader`, vrátí objekt, který implementuje [ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Vzhledem k tomu `RandomCategoryDataSource` SqlDataSource má jeho `DataSourceMode` vlastnost nastavena na hodnotu `DataSet` (výchozí), budeme pracovat s objektem DataView.

Následující kód ukazuje, jak načíst záznamy ze `RandomCategoryDataSource` SqlDataSource jako v zobrazení DataView a také jak číst `CategoryName` hodnota sloupce z prvního řádku DataView:


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample11.vb)]

`randomCategoryView(0)`Vrátí první `DataRowView` v zobrazení DataView. `randomCategoryView(0)("CategoryName")`Vrátí hodnotu `CategoryName` sloupec v této první řádek. Všimněte si, zda je zobrazení DataView volného typu. Chcete-li hodnota konkrétní sloupce musíme předat název sloupce jako řetězec (v tomto případě CategoryName). Obrázek 13 zobrazuje zpráva zobrazená v `CategoryNameLabel` při zobrazení stránky. Samozřejmě kategorie skutečný název zobrazený náhodně vybrány ve `RandomCategoryDataSource` SqlDataSource na každou, navštivte stránku (včetně postback).


[![S náhodně vybrané kategorie, název se zobrazí](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image25.png)

**Obrázek 13**: název se zobrazí s The náhodně vybrané kategorie ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image26.png))


> [!NOTE]
> Pokud SqlDataSource řízení s `DataSourceMode` kdyby byla nastavena na `DataReader`, návratovou hodnotou z `Select()` metoda by musel přetypovat `IDataReader`. Ke čtení `CategoryName` hodnota sloupce z prvního řádku jsme d použít kód jako:


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample12.vb)]

Pomocí SqlDataSource náhodně vyberete kategorii, jsme re připraveno k přidání GridView, které jsou uvedeny produkty kategorie s.

> [!NOTE]
> Místo pomocí ovládacího prvku popisek webové zobrazení s názvem kategorie, jsme může přidali FormView nebo DetailsView na stránku, vytvoření vazby SqlDataSource. Pomocí štítek, ale povoleno nám zjistit, jak má být vyvolán prostřednictvím kódu programu SqlDataSource s `Select()` prohlášení a pracovat s jeho Výsledná data v kódu.


## <a name="step-5-assigning-parameter-values-programmatically"></a>Krok 5: Přiřazení hodnoty parametrů prostřednictvím kódu programu

Všechny příklady jsme viděli, pokud v tomto kurzu jste použili hodnotu pevně parametru nebo jeden provedeny z jednoho zdroje předem definovaných parametrů (hodnotu querystring, ovládací prvek webu na stránce a tak dále). Ale SqlDataSource ovládací prvek s parametry můžete nastavit také prostřednictvím kódu programu. K dokončení našem příkladu aktuální, potřebujeme SqlDataSource, která vrací všechny produkty, které patří do zadané kategorii. Bude mít tento SqlDataSource `CategoryID` parametr, jehož hodnota musí být nastavena na základě `CategoryID` sloupec hodnoty vrácené `RandomCategoryDataSource` SqlDataSource v `Page_Load` obslužné rutiny události.

Začněte přidáním GridView na stránku a navázat jej nové SqlDataSource s názvem `ProductsByCategoryDataSource`. Mnohem postupem uvedeným v kroku 3, nakonfigurujte SqlDataSource tak, aby vyvolá `GetProductsByCategory` uložené procedury. Zadaná sada parametrů zdroj rozevíracího seznamu nechat na hodnotu None, ale nezadávejte výchozí hodnotu, jako jsme nastavit výchozí hodnotu prostřednictvím kódu programu.


[![Nezadávejte parametr zdroje nebo výchozí hodnotu](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image27.png)

**Obrázek 14**: Nezadávejte parametr zdroje nebo výchozí hodnotu ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image28.png))


Po dokončení Průvodce SqlDataSource výsledné deklarativní by měl vypadat podobně jako následující:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample13.aspx)]

Jsme můžete přiřadit `DefaultValue` z `CategoryID` prostřednictvím kódu programu v parametru `Page_Load` obslužné rutiny události:


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample14.vb)]

Pomocí tohoto přidání stránky obsahuje GridView, který ukazuje produkty spojené s náhodně vybrané kategorie.


[![Nezadávejte parametr zdroje nebo výchozí hodnotu](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image29.png)

**Obrázek 15**: Nezadávejte parametr zdroje nebo výchozí hodnotu ([Kliknutím zobrazit obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image30.png))


## <a name="summary"></a>Souhrn

Ve třídě SqlDataSource vývojářům stránky umožňuje definovat parametrických dotazů, jejichž hodnoty parametrů můžete pevně, načtený z předem definovaných parametr zdroje nebo přiřazené prostřednictvím kódu programu. V tomto kurzu jsme viděli, jak vytvořit parametrizovaného dotazu z Průvodce konfigurace zdroje dat pro ad-hoc dotazy SQL a uložených procedur. Také jsme se podívali na pomocí pevně parametr zdrojů, jako parametr Zdroj ovládacího prvku webové a prostřednictvím kódu programu zadat hodnotu parametru.

Jako s ObjectDataSource, SqlDataSource také poskytuje funkce, které upravit jeho zdrojová data. V dalším kurzu podíváme jak definovat `INSERT`, `UPDATE`, a `DELETE` příkazy s SqlDataSource. Po přidání těchto příkazů, jsme můžete využít předdefinované vkládání, úpravy a odstraňování vyplývajících k ovládacím prvkům GridView DetailsView a FormView funkce.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Scott Clyde, Randell Schmidt a Ken Pespisa. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](querying-data-with-the-sqldatasource-control-vb.md)
[další](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
