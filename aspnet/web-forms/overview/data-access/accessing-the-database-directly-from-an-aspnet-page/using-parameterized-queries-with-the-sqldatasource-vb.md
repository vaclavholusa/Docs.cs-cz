---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
title: Použití parametrizovaných dotazů s ovládacím prvkem SqlDataSource (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu jsme naše pohled na ovládacím prvkem SqlDataSource pokračovat a zjistěte, jak definovat parametrizované dotazy. Parametry lze zadat obě decla...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: e322f34c-83b7-41ea-ab65-ab1e0bdcc609
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 1dffaf59c6519f288dc36519897e51efa22c6a26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756948"
---
<a name="using-parameterized-queries-with-the-sqldatasource-vb"></a>Použití parametrizovaných dotazů s ovládacím prvkem SqlDataSource (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_VB.exe) nebo [stahovat PDF](using-parameterized-queries-with-the-sqldatasource-vb/_static/datatutorial48vb1.pdf)

> V tomto kurzu jsme naše pohled na ovládacím prvkem SqlDataSource pokračovat a zjistěte, jak definovat parametrizované dotazy. Parametry se dá nastavit pomocí deklarace i prostřednictvím kódu programu a je mohly vyžádat z mnoha umístění jako řetězec dotazu, relace stavu, další ovládací prvky a další.


## <a name="introduction"></a>Úvod

V předchozím kurzu jsme viděli, jak pomocí ovládacím prvkem SqlDataSource data přímo z databáze. Pomocí Průvodce konfigurace zdroje dat, můžeme může zvolte databázi a pak jednu: Vybrat sloupce, které chcete vrátit z tabulky nebo zobrazení. Zadejte vlastní příkaz SQL; nebo použijte uloženou proceduru. Zda výběr sloupců v tabulce nebo zobrazení nebo zadat vlastní příkaz jazyka SQL, ve třídě SqlDataSource ovládací prvek s `SelectCommand` vlastnost je přiřazena výsledný SQL ad-hoc `SELECT` příkazu a je to `SELECT` příkaz, který je spuštěna při SqlDataSource s `Select()` vyvolání metody (buď prostřednictvím kódu programu, nebo automaticky z dat webový ovládací prvek).

SQL `SELECT` příkazů použité v předchozím kurzu s ukázky chybějící `WHERE` klauzule. V `SELECT` příkazu `WHERE` klauzuli lze použít k omezení vrácené výsledky. Například pokud chcete zobrazit názvy produktů ocenění více než 50,00 $, můžeme použít následující dotaz:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample1.sql)]

Obvykle hodnot použitých v `WHERE` klauzule se určuje na základě některých externího zdroje, jako je například hodnota řetězce dotazu, proměnné relace nebo uživatelského vstupu z webový ovládací prvek na stránce. V ideálním případě jsou tyto vstupy určené prostřednictvím *parametry*. S Microsoft SQL Server, parametry jsou rozlišeny pomocí `@parameterName`, například:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample2.sql)]

Ve třídě SqlDataSource podporuje parametrizované dotazy, obě `SELECT` příkazy a `INSERT`, `UPDATE`, a `DELETE` příkazy. Kromě toho hodnot parametrů může být automaticky získaných z různých zdrojů, řetězec dotazu, stav relace, ovládací prvky na stránce a tak dále nebo je možné přiřadit prostřednictvím kódu programu. V tomto kurzu se podíváme, jak definovat parametrizované dotazy a také, jak zadat parametr hodnoty deklarativně i prostřednictvím kódu programu.

> [!NOTE]
> V předchozím kurzu jsme porovnání prvek ObjectDataSource, který byl náš nástroj podle výběru přes nejprve 46 kurzy s ovládacím prvkem SqlDataSource zmínku jejich koncepční podobnosti. Tyto podobnosti rozšířit také na parametry. Prvek ObjectDataSource s parametry namapované na vstupní parametry pro metody v vrstvy obchodní logiky. S ovládacím prvkem SqlDataSource parametry jsou definované přímo v rámci dotazu SQL. Oba ovládací prvky mají kolekce parametrů pro jejich `Select()`, `Insert()`, `Update()`, a `Delete()` metod a jak může mít tyto hodnoty parametrů naplněny z předem definovaných zdroje (hodnoty řetězce dotazu, proměnné relace a tak dále ) nebo prostřednictvím kódu programu přiřazen.


## <a name="creating-a-parameterized-query"></a>Vytvoření parametrického dotazu

Průvodce konfigurace zdroje dat SqlDataSource ovládacího prvku s nabízí tři způsoby definování příkazu ke spuštění pro načtení záznamů databáze:

- Výběrem sloupců z existující tabulky nebo zobrazení
- Tak, že zadáte vlastní příkaz SQL nebo
- Výběrem uložené procedury

Při výběru sloupců z existující tabulky nebo zobrazení, parametry `WHERE` potřeba zadat klauzuli prostřednictvím přidat `WHERE` klauzule dialogovému oknu. Při vytváření vlastní příkaz jazyka SQL, ale můžete zadat parametry přímo do `WHERE` – klauzule (pomocí `@parameterName` k označení každého parametru). A [uloženou proceduru](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) se skládá z jednoho nebo více příkazů SQL, a tyto příkazy mohou být parametrizovány. Parametry použité v příkazech SQL, ale musí být předán v jako vstupní parametry uložené procedury.

Od vytvoření parametrického dotazu závisí na tom SqlDataSource s `SelectCommand` je zadaný, umožněte s podívejte se na všech třech přístupy. Chcete-li začít, otevřete `ParameterizedQueries.aspx` stránku `SqlDataSource` složky, přetáhněte z panelu nástrojů na Návrhář ovládacím prvkem SqlDataSource a nastavte jeho `ID` k `Products25BucksAndUnderDataSource`. Klikněte na odkaz Konfigurovat zdroj dat z ovládacího prvku s inteligentním. Vyberte databáze, kterou chcete použít (`NORTHWINDConnectionString`) a klikněte na tlačítko Další.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Krok 1: Přidání`WHERE`klauzule při výběru sloupců z tabulky nebo zobrazení

Při výběru dat k vrácení z databáze s ovládacím prvkem SqlDataSource, průvodce Konfigurovat zdroj dat umožňuje jednoduše vybrat sloupce, které chcete vrátit z existující tabulky nebo zobrazení (viz obrázek 1). To tedy automaticky vytvoří SQL `SELECT` příkazu, který je, co se odesílá do databáze při SqlDataSource s `Select()` vyvolání metody. Jako jsme to udělali v předchozím kurzu, vyberte tabulku produktů z rozevíracího seznamu a zkontrolujte, `ProductID`, `ProductName`, a `UnitPrice` sloupce.


[![Vybrat sloupce, které chcete vrátit z tabulky nebo zobrazení](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.png)

**Obrázek 1**: Vyberte sloupce, které chcete vrátit z tabulky nebo zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.png))


Zahrnout `WHERE` klauzuli v `SELECT` příkaz, klikněte na tlačítko `WHERE` tlačítko, kterým se zobrazí přidat `WHERE` klauzule dialogové okno (viz obrázek 2). Přidání parametru omezit rozsah výsledků vrácených `SELECT` dotazování, nejdřív vyberte sloupec pro filtrování dat podle. V dalším kroku vyberte operátor, který má použít pro filtrování (=, &lt;, &lt;=, &gt;, a tak dále). Nakonec vyberte zdroj s hodnotu parametru, jako ze stavu řetězce dotazu nebo v jiné relaci. Až nakonfigurujete parametr, klikněte na tlačítko Přidat jej v zahrnout `SELECT` dotazu.

V tomto příkladu vám umožňují s vrátit pouze těch výsledků kde `UnitPrice` hodnota je menší než nebo rovna 25,00 $. Proto vyberte `UnitPrice` z rozevíracího seznamu sloupců a &lt;= z rozevíracího seznamu operátor. Při použití hodnoty pevně zakódované parametru (např. $25,00) nebo pokud hodnota parametru je třeba zadat prostřednictvím kódu programu, vyberte žádný z rozevíracího seznamu zdrojů. Dále zadejte hodnotu parametru pevně zakódované v textovém poli hodnota 25,00 a dokončete proces kliknutím na tlačítko Přidat.


[![Omezit rozsah výsledků vrácených přidat WHERE dialogové okno – klauzule](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.png)

**Obrázek 2**: omezit výsledky vrácené přidat `WHERE` dialogové okno – klauzule ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.png))


Po přidání parametru, klikněte na tlačítko OK se vraťte do Průvodce konfigurace zdroje dat. `SELECT` Příkaz v dolní části průvodce by teď měl obsahovat `WHERE` klauzule s parametrem s názvem `@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample3.sql)]

> [!NOTE]
> Pokud zadáte více podmínek v `WHERE` klauzule z přidat `WHERE` klauzule dialogové okno, průvodce připojí, je pomocí `AND` operátor. Pokud je potřeba zahrnout `OR` v `WHERE` klauzuli (například `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`) pak budete muset sestavit `SELECT` prostřednictvím vlastní obrazovky příkaz SQL.


Dokončení konfigurace ovládacím prvkem SqlDataSource (klikněte na tlačítko Další, pak dokončete) a pak zkontrolujte SqlDataSource s deklarativní. Značky teď zahrnuje `<SelectParameters>` kolekce, která obsahuje zdroje pro parametry v `SelectCommand`.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample4.aspx)]

Při SqlDataSource s `Select()` je vyvolána metoda `UnitPrice` hodnota parametru (25,00) platí pro `@UnitPrice` parametr `SelectCommand` před odesláním do databáze. Net výsledkem je pouze produkty, menší nebo rovna $25,00 vrácená `Products` tabulky. GridView potvrdit, přidat na stránku, vázat na tento zdroj dat a pak zobrazte stránku prostřednictvím prohlížeče. Měli byste vidět pouze produkty uvedené, které jsou menší než nebo rovna $25,00, potvrdí obr. 3.


[![Jsou zobrazeny pouze ty produkty menší než nebo rovno 25,00 $](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.png)

**Obrázek 3**: jsou zobrazeny pouze ty produkty menší než nebo rovno 25,00 $ ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Krok 2: Přidání parametrů do vlastní SQL příkaz

Při přidávání vlastního příkazu SQL můžete zadat `WHERE` klauzule explicitně nebo zadejte hodnotu v buňce filtr Tvůrce dotazů. Abychom to umožní s zobrazit jenom tyto produkty v ovládacího prvku GridView, jejichž ceny jsou kratší než určitá mezní hodnotu. Začněte tím, že přidáte textové pole na `ParameterizedQueries.aspx` stránku a shromažďovat tato prahová hodnota od uživatele. Nastavit textové pole s `ID` vlastnost `MaxPrice`. Přidání ovládacího prvku tlačítko a nastavte jeho `Text` vlastnosti k zobrazení odpovídající produkty.

V dalším kroku přetáhněte na stránku GridView a od jeho inteligentních značek můžete vytvořit nové SqlDataSource s názvem `ProductsFilteredByPriceDataSource`. Z Průvodce konfigurace zdroje dat, přejděte k určení vlastní příkaz SQL nebo uloženou proceduru obrazovky (viz obrázek 4) a zadejte následující příkaz:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample5.sql)]

Po zadání dotazu (ručně nebo pomocí Tvůrce dotazů), klikněte na tlačítko Další.


[![Vrátit pouze produkty, menší než hodnota parametru](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.png)

**Obrázek 4**: vrátí pouze ty produkty menší než nebo rovno hodnota parametru ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.png))


Protože dotaz obsahuje parametry, na další obrazovce Průvodce nám vyzve k zadání zdrojové hodnoty parametrů. Vyberte ovládací prvek ze seznamu parametrů zdroj rozevíracího seznamu a `MaxPrice` (ovládací prvek TextBox s `ID` hodnota) z rozevíracího seznamu ControlID. Můžete také zadat volitelnou výchozí hodnotu pro použití v případě, kde uživatel nebyl zadali jakýkoli text do `MaxPrice` textového pole. Prozatím, nezadávejte výchozí hodnotu.


[![MaxPrice TextBox s vlastností Text slouží jako zdroj parametru](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.png)

**Obrázek 5**: `MaxPrice` textové pole s `Text` vlastnost se používá jako zdroj parametru ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.png))


Dokončete průvodce Konfigurace zdroje dat kliknutím na další a pak dokončit. Deklarativní ovládacího prvku GridView, textového pole, tlačítka a SqlDataSource takto:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample6.aspx)]

Všimněte si, že parametr ve třídě SqlDataSource s `<SelectParameters>` oddíl je `ControlParameter`, což zahrnuje další vlastnosti, jako je `ControlID` a `PropertyName`. Při SqlDataSource s `Select()` je vyvolána metoda `ControlParameter` vezme hodnoty ze zadané vlastnosti ovládacího prvku webové a přiřadí ji k odpovídajícím parametrem `SelectCommand`. V tomto příkladu `MaxPrice` s vlastnost Text slouží jako `@MaxPrice` hodnotu parametru.

Chcete-li zobrazit tuto stránku prostřednictvím prohlížeče chvíli trvat. Při první návštěvě stránky nebo pokaždé, když `MaxPrice` textového pole chybí hodnota žádné záznamy se zobrazují v prvku GridView.


[![Nejsou žádné záznamy, že se že zobrazí při the MaxPrice textové pole je prázdné](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.png)

**Obrázek 6**: žádné záznamy se zobrazí, když `MaxPrice` textové pole je prázdné ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.png))


Jsou zobrazeny žádné produkty. Důvodem je to proto, že ve výchozím nastavení, je převedena na prázdný řetězec pro hodnotu parametru do databáze `NULL` hodnotu. Od porovnání `[UnitPrice] <= NULL` vždycky vyhodnotí jako False, nevrátí žádné výsledky.

Zadejte hodnotu do textového pole, jako je 5.00 a klikněte na tlačítko Zobrazit odpovídající produkty. Na zpětné volání ve třídě SqlDataSource informuje o tom, že se že změnilo v prvku GridView, že jedna z jeho zdroje parametru. V důsledku toho prvku GridView znovu připojí k zobrazování těchto produktů, které je menší než nebo rovna $5.00 ve třídě SqlDataSource.


[![Produkty menší než nebo rovno $5.00 jsou zobrazeny.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.png)

**Obrázek 7**: se zobrazují produkty menší než nebo rovno 5.00 $ ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.png))


## <a name="initially-displaying-all-products"></a>Zpočátku zobrazuje všechny produkty

Namísto zobrazení žádné produkty při prvním načtení stránky, budeme chtít zobrazit *všechny* produktů. Jedním ze způsobů, chcete-li vypsat všechny produkty pokaždé, když `MaxPrice` textové pole je prázdné, je nastavit výchozí hodnotu parametru s některé neskutečně vysokou hodnotu, jako je 1000000, protože s nepravděpodobné, že Northwind Traders se někdy mají inventarizovat jejichž cena za jednotku překračuje $ 1 000 000. Tento přístup je však shortsighted a nemusí fungovat v jiných situacích.

V předchozích kurzech - [deklarované parametry](../basic-reporting/declarative-parameters-vb.md) a [filtrování záznamů Master/Detail s DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) jsme se potýkají s podobný problém. Existuje naším řešením bylo vložte tuto logiku vrstvy obchodní logiky. Konkrétně BLL prozkoumat příchozí hodnoty a pokud se jednalo `NULL` nebo některé vyhrazenou hodnotou, volání, byla směrována do vrstvy DAL metodu, která vrátí všechny záznamy. Pokud hodnota příchozí běžné hodnoty filtrování, došlo k volání metody DAL, který spouští příkaz SQL, který používá parametrizované `WHERE` klauzule se zadanou hodnotou.

Bohužel jsme se nebude používat architekturu při použití ve třídě SqlDataSource. Místo toho, musíme přizpůsobit příkaz jazyka SQL a monetizace zkopírovat všechny záznamy, pokud `@MaximumPrice` parametr `NULL` nebo některé rezervovanou hodnotu. Pro toto cvičení, vám umožňují s jste ho tak, že pokud `@MaximumPrice` rovná parametru `-1.0`, pak *všechny* záznamy mají být vráceny (`-1.0` funguje jako rezervovanou hodnotu, protože žádný produkt, který může mít negativní `UnitPrice`hodnotu). K tomu můžeme použít následující příkaz SQL:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample7.sql)]

To `WHERE` klauzule vrátí *všechny* zaznamená, pokud `@MaximumPrice` rovná parametru `-1.0`. Pokud hodnota parametru není `-1.0`, pouze produkty, jejichž `UnitPrice` je menší než nebo rovna hodnotě `@MaximumPrice` se vrátí hodnota parametru. Tím, že nastavíte na výchozí hodnotu `@MaximumPrice` parametr `-1.0`, při prvním načtení stránky (nebo vždy, když `MaxPrice` textové pole je prázdné), `@MaximumPrice` bude mít hodnotu `-1.0` a zobrazí všechny produkty.


[![Teď všechny produkty, které jsou zobrazeny při the MaxPrice textové pole je prázdné](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.png)

**Obrázek 8**: teď všechny produkty se zobrazí, když `MaxPrice` textové pole je prázdné ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image16.png))


Existuje několik upozornění si s tímto přístupem. Nejprve dobré si uvědomit, že je datový typ parametru s vyvozena na základě jeho použití s v dotazu SQL. Pokud změníte `WHERE` klauzule z `@MaximumPrice = -1.0` k `@MaximumPrice = -1`, modul runtime zpracovává parametr jako celé číslo. Pokud se pokusíte přiřadit `MaxPrice` textového pole na hodnotu decimal (např. 5.00), dojde k chybě protože 5.00 nelze převést na celé číslo. Chcete-li to napravit, buď Ujistěte se, že používáte `@MaximumPrice = -1.0` v `WHERE` klauzule nebo vyšší, nastavte `ControlParameter` objektu s `Type` vlastnost na desetinné číslo.

Za druhé, tak, že přidáte `OR @MaximumPrice = -1.0` k `WHERE` klauzule dotazu modul nemůže použít index na `UnitPrice` (za předpokladu, že jeden existuje), což by vedlo k prohledávání tabulky. Pokud jsou dostatečně velký počet záznamů v může ovlivnit výkon `Products` tabulky. Lepším řešením může být přesunout tuto logiku do uložené procedury kde `IF` by provést příkaz `SELECT` dotaz z `Products` tabulky bez `WHERE` klauzuli, pokud všechny záznamy se muset vrátit, nebo jeden jehož `WHERE` klauzule WHERE obsahuje pouze `UnitPrice` kritéria, tak, aby indexu můžete použít.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Krok 3: Vytvoření a použití parametrizované uložené procedury

Uložené procedury může obsahovat sadu vstupní parametry, které je pak možné použít v příkazů SQL definovaný v rámci uložené procedury. Při konfiguraci ve třídě SqlDataSource použití uložené procedury, která přijímá vstupní parametry, lze tyto hodnoty parametrů zadat pomocí stejné techniky stejně jako u příkazů SQL ad-hoc.

Pro ilustraci použití uložených procedur v ovládacím prvkem SqlDataSource umožňují s vytvořit novou úložnou proceduru v databázi Northwind s názvem `GetProductsByCategory`, který přijme parametr s názvem `@CategoryID` a vrátí všechny sloupce produkty jehož `CategoryID` odpovídá sloupci `@CategoryID`. Vytvoření uložené procedury, přejděte do Průzkumníka serveru a k podrobnostem `NORTHWND.MDF` databáze. (Pokud don t zobrazí v Průzkumníku serveru, otevřete ho tak, že přejdete do nabídky zobrazení a výběrem možnosti Průzkumníka serveru.)

Z `NORTHWND.MDF` databázi, klikněte pravým tlačítkem na složku uložené procedury, zvolte možnost Přidat novou uloženou proceduru a zadejte následující syntaxi:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample8.sql)]

Klikněte na tlačítko Uložit ikona (nebo Ctrl + S) Chcete-li uložit uloženou proceduru. Uložené procedury můžete otestovat tak, že kliknete pravým tlačítkem ve složce uložené procedury a zvolíte Execute. To vás vyzve k zadání parametrů uložené procedury s (`@CategoryID`, v tomto případě), po který výsledky se zobrazí v okně výstup.


[![GetProductsByCategory uložené procedury při spuštění s @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image17.png)

**Obrázek 9**: `GetProductsByCategory` uložená procedura při spuštění s `@CategoryID` 1 ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image18.png))


Umožní použít tuto uloženou proceduru k zobrazení všech produktů do kategorie Nápoje v GridView s. Přidání nového ovládacího prvku GridView na stránku a jeho vazbu na nové třídě SqlDataSource s názvem `BeverageProductsDataSource`. Pokračovat k určení vlastní příkaz SQL nebo uloženou proceduru obrazovky, vyberte přepínač uložené procedury a vybrat `GetProductsByCategory` uloženou proceduru z rozevíracího seznamu.


[![Vyberte GetProductsByCategory uloženou proceduru z rozevíracího seznamu](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image19.png)

**Obrázek 10**: vyberte `GetProductsByCategory` uloženou proceduru z rozevíracího seznamu ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image20.png))


Protože uložené procedury přijímá jako vstupní parametr (`@CategoryID`), klikněte na další výzvy nám jako zdroj pro tuto hodnotu s parametrem. Do této skupiny nápoje `CategoryID` 1, tak ponechte žádný parametr zdroj rozevíracího seznamu a zadejte do textového pole Výchozí hodnota 1.


[![Pomocí pevně zakódovaného hodnotu 1 vrátit produkty do kategorie Nápoje](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image21.png)

**Obrázek 11**: můžete vrátit produkty do kategorie Nápoje Hard-Coded hodnotu 1 ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image22.png))


Jak ukazuje následující deklarativní, při použití uložené procedury SqlDataSource s `SelectCommand` je nastavena na název uložené procedury a [ `SelectCommandType` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) je nastavena na `StoredProcedure`, která udává který `SelectCommand` je název uložené procedury, spíše než ad-hoc příkazu SQL.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample9.aspx)]

Otestování stránky v prohlížeči. Jsou zobrazeny pouze produkty, které patří do kategorie Nápoje, i když *všechny* produktu jsou zobrazena pole od `GetProductsByCategory` uloženou proceduru vrátí všechny sloupce `Products` tabulky. Můžeme samozřejmě může omezit nebo přizpůsobit pole zobrazené v prvku GridView z dialogového okna s upravit sloupce prvku GridView.


[![Jsou zobrazeny všechny nápoje](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image23.png)

**Obrázek 12**: jsou zobrazeny všechny nápoje ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Krok 4: Programová volání SqlDataSource s`Select()`– příkaz

V příkladech jsme ve zatím viděli v předchozím kurzu a v tomto kurzu jste SqlDataSource ovládací prvky svázané s přímo do GridView. Data ovládacího prvku s SqlDataSource, ale prostřednictvím kódu programu přístupný a uvedené v kódu. To může být zvláště užitečné při dotazování na data budete muset zkontrolovat, ale don t potřebovat zobrazit ho. Namísto nutnosti psát všechny často používaný kód ADO.NET pro připojení k databázi, zadejte příkaz a načtení výsledků, můžete nechat ve třídě SqlDataSource zpracovat tento monotónní kód.

Pro ilustraci práci s SqlDataSource s dat prostřednictvím kódu programu, představte si, že vašemu nadřízenému dosáhla jste se žádostí o vytvoření webové stránky, která zobrazuje název náhodně vybranou kategorii a její související produkty. To znamená, když uživatel navštíví tuto stránku, chceme náhodně zvolit některou kategorii ze `Categories` tabulky, zobrazovaný název kategorie a potom zobrazí seznam produktů, které patří do této kategorie spadají.

K tomu potřebujeme dva SqlDataSource ovládací prvky jeden a zkopírovat náhodné kategorie z `Categories` zobrazíte kategorie produktů s tabulkami. Vytvoříme ve třídě SqlDataSource, který načte záznam náhodné kategorie v tomto kroku; Krok 5 zjistí vytváření ve třídě SqlDataSource, který načte produkty s kategorie.

Začněte přidáním SqlDataSource k `ParameterizedQueries.aspx` a nastavte jeho `ID` k `RandomCategoryDataSource`. Nakonfigurujte ji tak, aby používala následujícího dotazu SQL:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample10.sql)]

`ORDER BY NEWID()` Vrátí záznamy seřadit v náhodném pořadí (viz [použití `NEWID()` náhodně řazení záznamů](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1` Vrátí první záznam ze sady výsledků. V kostce řečeno, tento dotaz vrátí `CategoryID` a `CategoryName` hodnot sloupců z jedné, náhodně vybrané kategorie.

Zobrazíte kategorie s `CategoryName` hodnota, přidání ovládacího prvku popisek na stránku a nastavit jeho `ID` vlastnost `CategoryNameLabel`a vymažte její `Text` vlastnost. Data můžete programově načítat ze ovládacím prvkem SqlDataSource, potřebujeme k vyvolání jeho `Select()` metoda. [ `Select()` Metoda](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) očekává, že jeden vstupní parametr typu [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), která určuje, jak by měl být messaged data před vrácením. Můžete uvést pokyny pro řazení a filtrování dat a používá data, která webové ovládací prvky při řazení nebo procházení dat z ovládacího prvku SqlDataSource po stránkách. V našem příkladu však budeme zadávat t potřeba data upravit před vrácením a proto bude předáno `DataSourceSelectArguments.Empty` objektu.

`Select()` Metoda vrátí objekt, který implementuje `IEnumerable`. Vrácený přesný typ závisí na hodnotě ovládacím prvkem SqlDataSource s [ `DataSourceMode` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Jak je popsáno v předchozím kurzu, může tato vlastnost nastavená na hodnotu buď `DataSet` nebo `DataReader`. Pokud nastavena na `DataSet`, `Select()` metoda vrátí hodnotu [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) objekt; Pokud `DataReader`, vrátí objekt, který implementuje [ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Protože `RandomCategoryDataSource` SqlDataSource má jeho `DataSourceMode` nastavenou na `DataSet` (výchozí), budeme pracovat s objektem DataView.

Následující kód ukazuje, jak načíst záznamy ze `RandomCategoryDataSource` SqlDataSource jako zobrazení dat a také způsob čtení `CategoryName` hodnota sloupce z prvního řádku DataView:


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample11.vb)]

`randomCategoryView(0)` Vrátí první `DataRowView` v DataView. `randomCategoryView(0)("CategoryName")` Vrátí hodnotu `CategoryName` sloupec v této první řádek. Všimněte si, že je DataView volného typu. Chcete-li odkazovat na konkrétní sloupce potřebujeme předat název sloupce jako řetězec (v tomto případě CategoryName). Obrázek 13 zobrazuje zpráva zobrazená v `CategoryNameLabel` při zobrazení stránky. Samozřejmě kategorie skutečný název zobrazený náhodně zvolí `RandomCategoryDataSource` SqlDataSource na každé návštěvě stránky (včetně postbacků).


[![S náhodně vybrané kategorie, název se zobrazí.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image25.png)

**Obrázek 13**: název se zobrazí s náhodně vybrané kategorii ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image26.png))


> [!NOTE]
> Pokud s ovládacím prvkem SqlDataSource `DataSourceMode` nastavili vlastnost `DataReader`, návratová hodnota z `Select()` metoda byste potřebovali přetypovat `IDataReader`. Ke čtení `CategoryName` hodnota sloupce z prvního řádku, jsme d použijte kód jako:


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample12.vb)]

S ovládacím prvkem SqlDataSource náhodně vyberete kategorii, můžeme znovu připraven k přidání prvku GridView, který obsahuje seznam kategorií s produkty.

> [!NOTE]
> Místo použití popisku webový ovládací prvek pro zobrazení s názvem kategorie, může do jsme přidali FormView nebo prvku DetailsView. na stránce vytvoříte jejich vazbu na ovládacím prvkem SqlDataSource. Použití popisku, ale povoleno nám a objevujte jak programově volat SqlDataSource s `Select()` příkazu a pracovat s jeho Výsledná data v kódu.


## <a name="step-5-assigning-parameter-values-programmatically"></a>Krok 5: Přiřazení hodnoty parametrů prostřednictvím kódu programu

Ve všech příkladech jsme ve zatím viděli v tomto kurzu použili hodnotu pevně zakódované parametru nebo jeden z jednoho z předem definovaných parametrů zdrojů (hodnotu querystring, ovládací prvek webové stránky a tak dále). Ale SqlDataSource ovládacího prvku s parametry můžete nastavit také prostřednictvím kódu programu. K dokončení našich aktuální příklad, potřebujeme SqlDataSource, který vrátí všechny produkty, které patří do zadané kategorie. Bude mít tento SqlDataSource `CategoryID` parametr, jehož hodnota se musí nastavit na základě `CategoryID` vrácenou hodnotu sloupce `RandomCategoryDataSource` SqlDataSource v `Page_Load` obslužné rutiny události.

Začněte přidáním GridView na stránku a jeho vazbu na nové třídě SqlDataSource s názvem `ProductsByCategoryDataSource`. Mnohem postupem uvedeným v kroku 3, nakonfigurujte ve třídě SqlDataSource tak, aby vyvolá `GetProductsByCategory` uložené procedury. Nechat sadu parametrů zdrojové rozevírací seznam na hodnotu None, ale nezadávejte výchozí hodnotu, jak prostřednictvím kódu programu nastavíme tuto výchozí hodnotu.


[![Nezadávejte parametr zdroje nebo výchozí hodnotu](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image27.png)

**Obrázek 14**: není zadán parametr zdroj nebo výchozí hodnotu ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image28.png))


Po dokončení Průvodce SqlDataSource výsledný deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample13.aspx)]

My je můžeme přiřadit `DefaultValue` z `CategoryID` programově v parametru `Page_Load` obslužné rutiny události:


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample14.vb)]

Uveďte stránka obsahuje GridView zobrazující produkty související s náhodně vybranou kategorii.


[![Nezadávejte parametr zdroje nebo výchozí hodnotu](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image29.png)

**Obrázek 15**: není zadán parametr zdroj nebo výchozí hodnotu ([kliknutím ji zobrazíte obrázek v plné velikosti](using-parameterized-queries-with-the-sqldatasource-vb/_static/image30.png))


## <a name="summary"></a>Souhrn

Ve třídě SqlDataSource vývojářům stránky k definování parametrizované dotazy, jehož hodnoty parametrů můžete pevně zakódované, získaných z předem definovaných parametrů zdroje nebo prostřednictvím kódu programu přiřazen. V tomto kurzu jsme viděli, jak vytvořit parametrický dotaz z Průvodce konfigurace zdroje dat pro ad-hoc dotazy SQL a uložených procedur. Také jsme se podívali na použití pevně zakódované parametr zdrojů, webový ovládací prvek jako zdroj parametru a programově určující hodnotu parametru.

Stejně jako ovládacím prvkem ObjectDataSource, ovládacím prvkem SqlDataSource také umožňuje upravovat podkladová data. V dalším kurzu podíváme na tom, jak definovat `INSERT`, `UPDATE`, a `DELETE` příkazy s ovládacím prvkem SqlDataSource. Jakmile se přidaly tyto příkazy, jsme můžete využít předdefinované vložení, úpravy a odstranění funkce přináší ovládací prvky GridView, DetailsView a FormView.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Scott Clyde, Randell Schmidt a Ken Pespisa. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](querying-data-with-the-sqldatasource-control-vb.md)
> [další](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
