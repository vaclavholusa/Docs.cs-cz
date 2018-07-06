---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
title: Účinné stránkování velkých objemů dat (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Výchozí možnost stránkování prvku prezentace dat nevhodné, při práci s velkými objemy dat, jako jeho základní retriev ovládací prvek zdroje dat...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 59c01998-9326-4ecb-9392-cb9615962140
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
msc.type: authoredcontent
ms.openlocfilehash: a6d023f299d3c36e0b9f0d00f2b531d73657135c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367878"
---
<a name="efficiently-paging-through-large-amounts-of-data-c"></a>Účinné stránkování velkých objemů dat (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_CS.exe) nebo [stahovat PDF](efficiently-paging-through-large-amounts-of-data-cs/_static/datatutorial25cs1.pdf)

> Výchozí možnost stránkování prvku prezentace dat není vhodná, při práci s velkými objemy dat, jako jeho základní ovládací prvek zdroje dat zjišťuje všechny záznamy, i když se zobrazí pouze podmnožinu dat. V takových případech jsme musíte zapnout na vlastní stránkování.


## <a name="introduction"></a>Úvod

Jak jsme probírali v předchozím kurzu, stránkování je implementovat v jednom ze dvou způsobů:

- **Výchozí stránkování** může být implementována pouze kontrola možnost Povolit stránkování v datech webový ovládací prvek s inteligentní značky, ale při prohlížení stránky dat ObjectDataSource načte *všechny* záznamů, dokonce i i když jsou na stránce se zobrazí pouze jejich podmnožinu
- **Vlastní stránkování** zlepšuje výkon výchozí stránkování načtením jen takové záznamy z databáze, která musí být zobrazen pro konkrétní stránce požadovaná tímto uživatelem; data však vlastní stránkování vyžaduje trochu další úsilí k implementaci než výchozí stránkování

Kvůli snadnému provádění jenom zaškrtněte zaškrtávací políčko a níž Hotovo! výchozí stránkování je atraktivní možnosti. Svůj přístup není k dispozici ve při načítání všechny záznamy, ale umožňuje nepravděpodobné číselné volbou při procházení dostatečně velké objemy dat nebo pro weby s mnoha souběžnými uživateli. V takových případech jsme musíte zapnout na vlastní stránkování, aby bylo možné poskytovat responzivní systému.

Výzvy vlastní stránkování je schopnost vytvořit dotaz, který vrátí přesnou sadu záznamů potřebné pro konkrétní stránce data. Naštěstí Microsoft SQL Server 2005 poskytuje new – klíčové slovo pro výsledky hodnocení umožňující nám napsat dotaz, který můžete efektivně načítat správné podmnožinu záznamů. V tomto kurzu uvidíme, jak používat tento nový SQL Server 2005 – klíčové slovo implementovat vlastní stránkování v prvku GridView. Přestože je stejná jako výchozí stránkování, krokování z jedné stránky na další pomocí uživatelského rozhraní pro vlastní stránkování vlastní stránkování může být několik řádů rychleji než výchozí stránkování.

> [!NOTE]
> Přesné výkonový zisk plynoucí vykazují vlastní stránkování, závisí na celkový počet záznamů stránkování prostřednictvím a zatížení umístěných na databázovém serveru. Na konci tohoto kurzu podíváme na některé hrubý metriky, které prezentovat výhod ve výkonu, které získali prostřednictvím vlastní stránkování.


## <a name="step-1-understanding-the-custom-paging-process"></a>Krok 1: Principy procesu vlastní stránkování

Při procházení dat po stránkách, přesné záznamy zobrazené na stránce závisí na stránce se požadovaná data a počet záznamů zobrazených na stránce. Představte si například, že jsme chtěli stránkovat 81 produkty 10 produkty na stránku zobrazení. Při prohlížení na první stránce, d chceme produkty 1 až 10. Při prohlížení na druhé stránce jsme d zajímat produkty 11 až 20 a tak dále.

Existují tři proměnné, které určují, jaké záznamů je potřeba načíst a jakým způsobem má být vykreslen rozhraní stránkování:

- **Start Index řádku** index první řádek na stránce dat zobrazíte; to může index dá vypočítat jako index stránky vynásobí záznamy zobrazených na stránce a přičtení jedné. Například při procházení záznamů 10 najednou, první stránka (jehož index stránky je 0), Start Index řádku je 0 \* 10 + 1 nebo 1; na druhé stránce (jehož index stránky je 1), Start Index řádku je 1 \* 10 + 1 , nebo 11.
- **Maximální počet řádků** maximální počet záznamů zobrazených na stránce. Tato proměnná se označuje jako maximální počet řádků, protože pro poslední existuje stránka může být méně záznamů vrácených než je velikost stránky. Například při procházení 81 produkty 10 záznamů na stránku, devátý a poslední stránka bude mít jenom jeden záznam. Žádné stránky, ale zobrazí více výsledků, než je hodnota maximální počet řádků.
- **Celkový počet záznamů** celkový počet záznamů stránkování prostřednictvím. Zatímco tato proměnná není t potřebných k určení záznamů pro danou stránku, vyžadují rozhraní stránkování. Například pokud existují 81 stránkování prostřednictvím produktů, stránkovací rozhraní věděl, může zobrazit devět čísla stránek ve stránkovacím rozhraní.

S výchozí stránkování, je vypočítán Index řádku spustit jako produktu index stránky a velikost stránky plus jedna, že maximální počet řádků je jednoduše velikost stránky. Od výchozího stránkování načte všechny záznamy z databáze při vykreslování libovolnou stránku dat, index pro každý řádek se označuje, díky čemuž Přesun na řádek Start Index řádku jednoduchý úkol. Navíc je snadno dostupný, protože celkový počet záznamů s jednoduše počet záznamů v objektu DataTable (nebo jakýkoli objekt se používá k ukládání výsledků databáze).

Zadané proměnné Start Index řádku a maximální počet řádků, vlastní implementaci stránkování musí vracet pouze přesné podmnožinu záznamů spuštění na řádek indexu spustit až maximální počet řádků počet záznamů, které potom. Vlastní stránkování poskytuje dva problémy:

- Jsme musí být schopen efektivně přidružit každý řádek v veškerá data stránkování prostřednictvím tak, že můžeme začít vrací záznamy na zadaný Index řádku Start index řádku
- Potřebujeme pro poskytování celkový počet záznamů stránkování prostřednictvím

V následujících dvou kroků prozkoumáme skriptu SQL nutné odpovědět na tyto dvě výzvy. Kromě skript SQL také potřebujeme k implementaci metody v knihoven BLL a DAL.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Krok 2: Vrátí celkový počet záznamů stránkování prostřednictvím

Před Zkoumáme, jak získat přesné podmnožinu záznamů pro na stránce se zobrazí, umožní s nejdřív se podívejte na tom, jak vrátit celkový počet záznamů stránkování prostřednictvím. Tyto informace je potřeba, abyste mohli správně nakonfigurovat uživatelského rozhraní stránkování. Celkový počet záznamů vrácených dotazem SQL konkrétní můžete získat pomocí [ `COUNT` agregační funkce](https://msdn.microsoft.com/library/ms175997.aspx). Například, chcete-li zjistit celkový počet záznamů v `Products` tabulku, můžeme použít následující dotaz:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample1.sql)]

Umožní s přidejte metodu k naší DAL, která vrací tyto informace. Zejména, vytvoříme DAL metodu nazvanou `TotalNumberOfProducts()` , který se spustí `SELECT` výše uvedeném příkazu.

Začněte otevřením `Northwind.xsd` soubor typované datové sady v `App_Code/DAL` složky. Pak klikněte pravým tlačítkem na `ProductsTableAdapter` v návrháři a zvolte Přidat dotaz. Jako jsme viděli v předchozích kurzech se ve to umožní, abychom mohli přidat novou metodu vrstvy Dal, která při vyvolání, spustí konkrétní příkaz SQL nebo uloženou proceduru. Stejně jako u našich metody TableAdapter v předchozích kurzech, použijte příkaz SQL ad-hoc optimalizované pro tento jeden.


![Použití příkazu SQL Ad-Hoc](efficiently-paging-through-large-amounts-of-data-cs/_static/image1.png)

**Obrázek 1**: použití příkazu SQL Ad-Hoc


Na další obrazovce můžeme určit, jaký typ dotazu vytvořte. Vzhledem k tomu, že tento dotaz vrátí jednu skalární hodnota celkový počet záznamů v `Products` zvolte tabulky `SELECT` vracející možnost hodnotu jednom.


![Konfigurovat dotaz, který bude použit příkaz SELECT, který vrací jedinou hodnotu](efficiently-paging-through-large-amounts-of-data-cs/_static/image2.png)

**Obrázek 2**: Konfigurace dotaz, který bude použit příkaz SELECT, který vrací jedinou hodnotu


Po označující typ použití dotazu, budeme dále musíte zadat dotaz.


![Použití vyberte COUNT(*) z dotazu produkty](efficiently-paging-through-large-amounts-of-data-cs/_static/image3.png)

**Obrázek 3**: Vyberte počet použití (\*) od produktů dotazu


Nakonec zadejte název metody. Stejně jako s výše uvedené, umožňují použít `TotalNumberOfProducts`.


![Pojmenujte TotalNumberOfProducts DAL – metoda](efficiently-paging-through-large-amounts-of-data-cs/_static/image4.png)

**Obrázek 4**: název TotalNumberOfProducts DAL – metoda


Po kliknutí na tlačítko Dokončit, průvodce přidá `TotalNumberOfProducts` metoda vrstvy Dal. Metody vracející skalární v DAL vrátí typy připouštějící hodnotu Null v případě, že výsledkem bude příkaz jazyka SQL je `NULL`. Naše `COUNT` dotaz, ale vždy vrátí non -`NULL` hodnota; bez ohledu na to, vrátí metoda DAL s možnou hodnotou Null celé číslo.

Kromě metodu DAL potřebujeme také metody v BLL. Otevřít `ProductsBLL` třídy soubor a přidejte `TotalNumberOfProducts` metodu, která jednoduše volá, aby s vrstvou DAL `TotalNumberOfProducts` metoda:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample2.cs)]

S vrstvou DAL `TotalNumberOfProducts` metoda vrátí celé číslo s možnou hodnotou Null; nicméně, jsme vytvořili `ProductsBLL` třída s `TotalNumberOfProducts` metodu tak, aby se standardní celé číslo. Proto musíme mít `ProductsBLL` třída s `TotalNumberOfProducts` metoda vrátit hodnotu část s možnou hodnotou Null celé číslo, které jsou vrácené s vrstvou DAL `TotalNumberOfProducts` metody. Volání `GetValueOrDefault()` vrátí hodnotu na celé číslo s možnou hodnotou Null, pokud existuje; pokud celočíselná hodnota s možnou hodnotou null je `null`, ale vrátí celé číslo výchozí hodnota je 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Krok 3: Vrací přesné podmnožinu záznamů

Naše dalším krokem je vytvoření metod v DAL a BLL, ve kterém přijmout Start Index řádku a maximální počet řádků proměnné jsme probírali výše a vrátí odpovídající záznamy. Než to, umožňují s první pohled na potřebné skript SQL. Před obrovskou výzvou – směřující nám je, že nám musí být schopen efektivně přiřazovat indexu na každý řádek v celé výsledky stránkování prostřednictvím tak, aby vrátíme jenom ty záznamy, spuštění na řádek indexu spustit (a až maximální počet záznamů počet záznamů).

Toto není výzvu, pokud již existuje sloupec v tabulce databáze, která slouží jako index řádku. Na první pohled může myslíme si, který `Products` tabulky s `ProductID` pole bude stačit, jako má první produkt `ProductID` 1 sekundu a 2, a tak dále. Odstraňuje se produkt však zůstanou mezera v pořadí, anulovány tento přístup.

Existují dvě obecné postupy slouží k efektivní přidružení index řádku dat na stránce, a tím umožnit přesné podmnožinu záznamů se má načíst:

- **Pomocí SQL Server 2005 s `ROW_NUMBER()` – klíčové slovo** nový SQL Server 2005 `ROW_NUMBER()` – klíčové slovo přidruží hodnoticí záznamech vrácené podle pořadí. Toto pořadí lze použít jako index řádku pro každý řádek.
- **Pomocí proměnné tabulky a `SET ROWCOUNT`**  systému SQL Server s [ `SET ROWCOUNT` příkaz](https://msdn.microsoft.com/library/ms188774.aspx) slouží k určení, kolik celkem záznamů dotaz by měl zpracovat před ukončením; [tabulky proměnných](http://www.sqlteam.com/item.asp?ItemID=9454) představují lokální proměnné jazyka T-SQL, které mohou obsahovat tabulková data akin k [dočasné tabulky](http://www.sqlteam.com/item.asp?ItemID=2029). Tento postup funguje stejně dobře s Microsoft SQL Server 2005 a SQL Server 2000 (vzhledem k tomu `ROW_NUMBER()` přístup funguje pouze s SQL Server 2005).  
  
  Zde spočívá v vytvoření proměnné tabulky, který má `IDENTITY` sloupce a sloupce pro primární klíče v tabulce, jejichž data se stránkování prostřednictvím. V dalším kroku je obsah tabulky, jejichž data se stránkování prostřednictvím zálohované do proměnné tabulky, a tím přidružíte sekvenční řádek indexu (prostřednictvím `IDENTITY` sloupce) pro každý záznam v tabulce. Jakmile připravil proměnnou tabulky `SELECT` příkaz na proměnnou tabulky spojit s podkladové tabulce, mohou být provedeny a vytáhnout konkrétní záznamy. `SET ROWCOUNT` Prohlášení se používá k inteligentně omezit počet záznamů, které je potřeba zálohované do proměnné tabulky.  
  
  Této efektivity přístup s je založen na číslo stránky, které jsou požadovány, jako `SET ROWCOUNT` hodnota přiřazena hodnota Start Index řádku a maximální počet řádků. Při procházení nízkými čísly stránek, jako je například první několik stránek tento přístup je velmi efektivní. Ale vykazuje výchozí stránkování jako výkon při načítání stránky poblíž konce.

V tomto kurzu se implementuje pomocí vlastní stránkování `ROW_NUMBER()` – klíčové slovo. Další informace o používání proměnnou tabulky a `SET ROWCOUNT` techniku, naleznete v tématu [další efektivní metodu pro stránkování prostřednictvím velkých sad výsledků dotazu A](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

`ROW_NUMBER()` – Klíčové slovo přidružený k pořadí jednotlivých záznamů vrácených průběhu konkrétní řazení pomocí následující syntaxe:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample3.sql)]

`ROW_NUMBER()` vrátí číselnou hodnotu, která určuje pořadí pro každý záznam s ohledem na uvedené pořadí. Například zobrazíte počet rozměrů pro jednotlivé produkty, seřazené od nejvíce nákladné nejméně, můžeme použít následující dotaz:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample4.sql)]

Obrázek 5 ukazuje tento dotaz s výsledky při spuštění v okně dotazu v sadě Visual Studio. Všimněte si, že produkty jsou řazeny podle cen, spolu s pořadí ceny pro každý řádek.


![Cena řád je součástí pro každý záznam vrátil](efficiently-paging-through-large-amounts-of-data-cs/_static/image5.png)

**Obrázek 5**: The pořadí cena je součástí pro každý záznam vrátil


> [!NOTE]
> `ROW_NUMBER()` pouze jedna z mnoha nových funkcí hodnocení je k dispozici v systému SQL Server 2005. Podrobnější diskuzi o `ROW_NUMBER()`, společně s další hodnocení funkce, přečtěte si [vrácení seřazených výsledků s Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).


Při hodnocení výsledky podle zadaného `ORDER BY` sloupec v `OVER` – klauzule (`UnitPrice`, v předchozím příkladu), SQL Server musí řazení výsledků. To je rychlá operace v případě clusterovaného indexu přes sloupců, které jsou právě výsledky seřazené podle, nebo pokud je pokrytí indexu, ale může být dražší jinak. Chcete-li zvýšit výkon pro dotazy na dostatečně velký, zvažte přidání neclusterovaný index pro sloupec, podle kterého se výsledky jsou řazeny podle. Zobrazit [hodnocení funkcí a výkonu v systému SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) pro podrobnější pohled na důležité informace o výkonu.

Hodnocení informace vrácené `ROW_NUMBER()` nelze použít přímo v `WHERE` klauzuli. Ale odvozené tabulky je možné vrátit `ROW_NUMBER()` výsledek, který pak může objevit v `WHERE` klauzuli. Například následující dotaz používá odvozené tabulky budou vráceny sloupce ProductName a UnitPrice spolu s `ROW_NUMBER()` výsledek a pak používá `WHERE` klauzule pouze vráceny produkty, jejichž cena řád je 11 až 20:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample5.sql)]

Tento koncept trochu další rozšíření, můžeme využít tento přístup pro načtení konkrétní stránky údaje požadované hodnoty Start Index řádku a maximální počet řádků:


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample6.html)]

> [!NOTE]
> Jak uvidíme dále v tomto kurzu *`StartRowIndex`* poskytnutých ObjectDataSource indexování od nuly, že `ROW_NUMBER()` hodnotu vrácenou příkazem SQL Server 2005 je indexováno od 1. Proto `WHERE` klauzule vrátí záznamy kde `PriceRank` striktně větší než *`StartRowIndex`* a menší než nebo rovna hodnotě *`StartRowIndex`*  +  *`MaximumRows`*.


Nyní, který jsme probírali jak ve `ROW_NUMBER()` lze použít k načtení konkrétní stránky dat, na základě hodnot Start Index řádku a maximální počet řádků, teď musíme implementovat tuto logiku metody v knihoven BLL a DAL.

Při vytváření tohoto dotazu že musí rozhodneme řazení podle kterého se bude řazena výsledky; umožní s seřazení produktů podle jejich názvu v abecedním pořadí. To znamená, že s vlastní implementaci stránkování v tomto kurzu jsme nebude možné vytvořit vlastní stránkovanou sestavu, než se také dají řadit. V dalším kurzu se ale podíváme jak tato funkce může být poskytnuta.

V předchozí části jsme vytvořili metodu DAL jako ad-hoc příkazu SQL. Bohužel analyzátor jazyka T-SQL v sadě Visual Studio používá t TableAdapter Průvodce kódu jako `OVER` syntaxe používané `ROW_NUMBER()` funkce. Tato metoda DAL jsme proto musíte vytvořit jako uloženou proceduru. V Průzkumníku serveru vyberte z nabídky Zobrazit (nebo přístupů Ctrl + Alt + S) a rozbalte `NORTHWND.MDF` uzlu. Pokud chcete přidat novou úložnou proceduru, klikněte pravým tlačítkem na uzel uložené procedury a zvolte Přidat novou uloženou proceduru (viz obrázek 6).


![Přidat novou úložnou proceduru pro stránkování prostřednictvím produktů](efficiently-paging-through-large-amounts-of-data-cs/_static/image6.png)

**Obrázek 6**: Přidat novou úložnou proceduru pro stránkování prostřednictvím produktů


Tuto uloženou proceduru by měla přijímat dvě celočíselné vstupní parametry - `@startRowIndex` a `@maximumRows` a použít `ROW_NUMBER()` funkce seřazené podle `ProductName` pole vrací pouze řádky větší než zadaný `@startRowIndex` a menší než nebo rovno `@startRowIndex`  +  `@maximumRow` s. Zadejte následující skript do nové uložené procedury a pak klikněte na ikonu Uložit do databáze přidat uložené procedury.


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample7.sql)]

Po vytvoření uložené procedury, věnujte chvíli otestování. Klikněte pravým tlačítkem na `GetProductsPaged` uloženou proceduru název v Průzkumníku serveru a zvolte možnost spouštět. Visual Studio vás pak vyzve k zadání vstupní parametry `@startRowIndex` a `@maximumRow` s (viz obrázek 7). Zkuste jiné hodnoty a podívejte se na výsledky.


![Zadejte hodnotu @startRowIndex a @maximumRows parametry](efficiently-paging-through-large-amounts-of-data-cs/_static/image7.png)

<strong>Obrázek 7</strong>: Zadejte hodnotu @startRowIndex a @maximumRows parametry


Po výběru těchto vstupní hodnoty parametrů, v okně výstupu se zobrazí výsledky. Obrázek 8 ukazuje výsledky při předávání v 10 pro obě `@startRowIndex` a `@maximumRows` parametry.


[![Jsou vráceny záznamy, že by se zobrazí v druhé Data stránky](efficiently-paging-through-large-amounts-of-data-cs/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image8.png)

**Obrázek 8**: The záznamy, že by se zobrazí v druhé stránce Data jsou vráceny ([kliknutím ji zobrazíte obrázek v plné velikosti](efficiently-paging-through-large-amounts-of-data-cs/_static/image10.png))


Tuto uloženou proceduru vytvořili, můžeme znovu připravený k vytvoření `ProductsTableAdapter` metody. Otevřít `Northwind.xsd` typová, klikněte pravým tlačítkem v `ProductsTableAdapter`a zvolte možnost přidat dotaz. Místo vytváření dotazů pomocí příkazu SQL ad-hoc, vytvořte ho pomocí stávající úložnou proceduru.


![Vytvořit metodu DAL pomocí stávající úložnou proceduru](efficiently-paging-through-large-amounts-of-data-cs/_static/image11.png)

**Obrázek 9**: vytvoření metody DAL pomocí stávající úložnou proceduru


Dále jsme vyzváni k výběru uloženou proceduru, která se má vyvolat. Vyberte si `GetProductsPaged` uloženou proceduru z rozevíracího seznamu.


![Zvolte GetProductsPaged uloženou proceduru z rozevíracího seznamu](efficiently-paging-through-large-amounts-of-data-cs/_static/image12.png)

**Obrázek 10**: Zvolte GetProductsPaged uloženou proceduru z rozevíracího seznamu


Na další obrazovce pak zeptá, jaký druh dat je vrácený uložená procedura: tabulková data, jedinou hodnotu nebo žádnou hodnotu. Vzhledem k tomu, `GetProductsPaged` uložené procedury může vrátit více záznamů, označuje, že vrátí tabulková data.


![Označuje, že bude procedura vracet tabulková Data](efficiently-paging-through-large-amounts-of-data-cs/_static/image13.png)

**Obrázek 11**: označuje, že bude procedura vracet tabulková Data


Nakonec uveďte názvy metod, které chcete vytvořili. Stejně jako u našich kurzů pro předchozí, pokračujte a vytvoření datové tabulky metod pomocí obou výplně a vrátit tabulku DataTable. Pojmenujte první metodu `FillPaged` a druhá `GetProductsPaged`.


![Název metody FillPaged a GetProductsPaged](efficiently-paging-through-large-amounts-of-data-cs/_static/image14.png)

**Obrázek 12**: název metody FillPaged a GetProductsPaged


Kromě toho vytvořit vrstvy DAL metody vrátit konkrétní stránce produktů, musíme také poskytuje tyto funkce v BLL. Stejná jako metoda DAL s BLL GetProductsPaged metoda dvou celé číslo vstupů pro určení Start Index řádku a maximální počet řádků, musíte přijmout a musí vracet jenom ty záznamy, které spadají do zadaného rozsahu. Vytvoření BLL metody ve třídě ProductsBLL pouze volání dolů do s vrstvou DAL GetProductsPaged metody, například takto:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample8.cs)]

Můžete použít libovolný název pro metody s BLL vstupní parametry, ale protože jsme se zobrazí po chvíli, rozhodnete použít `startRowIndex` a `maximumRows` nám ušetří speciální hodně práce při konfiguraci ObjectDataSource při použití této metody.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Krok 4: Konfigurace ObjectDataSource používat vlastní stránkování

U knihoven BLL a DAL metod pro konkrétní podmnožinu záznamů úplný přístup k řízení jsme znovu připravený k vytvoření GridView této stránky si její podkladové záznamy používá vlastní stránkování. Začněte otevřením `EfficientPaging.aspx` stránku `PagingAndSorting` složky, přidejte na stránku GridView a nakonfigurujte ho na použití nového ovládacího prvku ObjectDataSource. V našich kurzech posledních jsme měli často ObjectDataSource konfigurován pro použití `ProductsBLL` třída s `GetProducts` metody. Tentokrát ale chceme použít `GetProductsPaged` metoda místo toho od `GetProducts` vrátí metoda *všechny* produktů v databázi vzhledem k tomu `GetProductsPaged` vrátí jenom konkrétní podmnožinu záznamů.


![Konfigurace ObjectDataSource metody GetProductsPaged ProductsBLL třída s](efficiently-paging-through-large-amounts-of-data-cs/_static/image15.png)

**Obrázek 13**: Konfigurace ObjectDataSource metody GetProductsPaged ProductsBLL třída s


Od jsme opětovné vytváření GridView jen pro čtení věnujte chvíli nastavit metodu rozevíracího seznamu v INSERT, UPDATE a odstranit karty na (žádný).

V dalším kroku průvodce ObjectDataSource vyzve nám zdroje `GetProductsPaged` metody s `startRowIndex` a `maximumRows` vstupní hodnoty parametrů. Tyto vstupní parametry to jednoduše ve skutečnosti nastaví v prvku GridView automaticky ponechte zdrojová sada na hodnotu None a klikněte na tlačítko Dokončit.


![Ponechte vstupní parametr zdroje jako None](efficiently-paging-through-large-amounts-of-data-cs/_static/image16.png)

**Obrázek 14**: ponechte vstupní parametr zdroje jako žádné


Po dokončení Průvodce prvek ObjectDataSource, bude prvku GridView obsahovat vlastnost BoundField nebo třídě CheckBoxField pro každé pole data produktu. Teď můžete přizpůsobit vzhled ovládacího prvku GridView s podle svých potřeb. Můžu odebrat přihlášení k zobrazení pouze `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`, a `UnitPrice` BoundFields. Nakonfigurujte také GridView pro podporu stránkování zaškrtnutím políčka Povolit stránkování v jeho inteligentních značek. Po provedení těchto změn ovládacími prvky GridView a ObjectDataSource deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample9.aspx)]

Pokud na stránce prostřednictvím prohlížeče prvku GridView. je však není odkud nalezen.


![GridView se nezobrazuje](efficiently-paging-through-large-amounts-of-data-cs/_static/image17.png)

**Obrázek 15**: komponenta GridView se nezobrazuje


Protože ObjectDataSource právě používá 0 jako hodnoty pro oba prvku GridView. chybí `GetProductsPaged` `startRowIndex` a `maximumRows` vstupní parametry. Proto výsledný dotaz SQL vrací žádné záznamy a proto se nezobrazí prvku GridView.

Chcete-li to napravit, musíme nakonfigurovat ObjectDataSource používat vlastní stránkování. Můžete to provést v následujících krocích:

1. **Nastavit prvek ObjectDataSource s `EnablePaging` vlastnost `true`**  tak prvek ObjectDataSource, musíte předat informaci o `SelectMethod` dva další parametry: zadat Start Index řádku ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) a zadat maximální počet řádků ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Nastavit prvek ObjectDataSource s `StartRowIndexParameterName` a `MaximumRowsParameterName` vlastnosti odpovídajícím způsobem** `StartRowIndexParameterName` a `MaximumRowsParameterName` vlastnosti označit názvy vstupních parametrů předané do `SelectMethod` pro vlastní účely stránkování. Ve výchozím nastavení, jsou tyto názvy parametrů `startIndexRow` a `maximumRows`, což je důvod, proč při vytváření `GetProductsPaged` metoda v BLL, můžu použít tyto hodnoty pro vstupní parametry. Pokud jste se rozhodli použít různé názvy parametrů pro BLL s `GetProductsPaged` metody, jako `startIndex` a `maxRows`pro příklad, je třeba nastavit prvku ObjectDataSource s `StartRowIndexParameterName` a `MaximumRowsParameterName` vlastnosti odpovídajícím způsobem (jako je například hodnota startIndex pro `StartRowIndexParameterName` a maxRows. pro `MaximumRowsParameterName`).
3. **Nastavit prvek ObjectDataSource s [ `SelectCountMethod` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) název metody, která vrací celkový počet číslo sady záznamů se stránkovaného fondu přes (`TotalNumberOfProducts`)** Vzpomeňte si, že `ProductsBLL` třída s `TotalNumberOfProducts`metoda vrátí celkový počet záznamů stránkování pomocí vrstvy DAL metodu, která se spustí `SELECT COUNT(*) FROM Products` dotazu. Tyto informace vyžadované ObjectDataSource, aby bylo možné správně vykreslit rozhraní stránkování.
4. **Odeberte `startRowIndex` a `maximumRows` `<asp:Parameter>` prvky z prvku ObjectDataSource s deklarativní** při konfiguraci ObjectDataSource prostřednictvím průvodce, Visual Studio automaticky přidá dvě `<asp:Parameter>` elementy pro `GetProductsPaged` metody s vstupní parametry. Nastavením `EnablePaging` k `true`, tyto parametry se předají automaticky, pokud se zobrazí také v deklarativní syntaxe, ObjectDataSource se pokusí o předání *čtyři* parametrů `GetProductsPaged` – metoda a dvou parametrů `TotalNumberOfProducts` metody. Pokud zapomenete k jejich odebrání `<asp:Parameter>` elementy, pokud na stránce prostřednictvím prohlížeče, zobrazí se zpráva chyby jako: *prvek ObjectDataSource "ObjectDataSource1" nenalezl neobecnou metodu 'TotalNumberOfProducts", který má Parametry: startRowIndex, maximumRows*.

Po provedení těchto změn, deklarativní syntaxe prvku ObjectDataSource s by měl vypadat nějak takto:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample10.aspx)]

Všimněte si, že `EnablePaging` a `SelectCountMethod` byly nastaveny a `<asp:Parameter>` prvky byly odebrány. Obrázek 16 ukazuje snímek obrazovky okna vlastnosti po provedení těchto změn.


![Pokud chcete použít vlastní stránkování, konfigurace ovládacího prvku ObjectDataSource](efficiently-paging-through-large-amounts-of-data-cs/_static/image18.png)

**Obrázek 16**: Pokud chcete použít vlastní stránkování, konfigurace ovládacího prvku ObjectDataSource


Po provedení těchto změn, navštivte tuto stránku prostřednictvím prohlížeče. Měli byste vidět 10 produkty uvedené, seřazené podle abecedy. Chcete-li si jednu stránku dat najednou chvíli trvat. Neplatí žádné rozdíly ve visual z pohledu koncového uživatele s mezi stránkování výchozí a vlastní stránkování, vlastní stránkování efektivněji stránky velkých objemů dat, jak načítat pouze ty záznamy, které je třeba má být zobrazen pro danou stránku.


[![Data, Objednáno podle produktu s názvem, je stránkování pomocí vlastní stránkování](efficiently-paging-through-large-amounts-of-data-cs/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image19.png)

**Obrázek 17**: Data, Objednáno podle produktu s názvem, je stránkování pomocí vlastní stránkování ([kliknutím ji zobrazíte obrázek v plné velikosti](efficiently-paging-through-large-amounts-of-data-cs/_static/image21.png))


> [!NOTE]
> Na stránce s vlastní stránkování, počítat hodnotu vrácenou příkazem ObjectDataSource s `SelectCountMethod` je uložena v prvku GridView s zobrazení stavu. Další proměnné ovládacího prvku GridView `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` kolekce a tak dále se ukládají v *stav ovládacích prvků*, což je trvalý bez ohledu na hodnotu GridView s `EnableViewState` Vlastnost. Protože `PageCount` se ukládají hodnoty postbacků pomocí zobrazení stavu, při použití rozhraní stránkování, který zahrnuje odkaz můžete přejít na poslední stránku, je nutné povolit stav zobrazení ovládacího prvku GridView s. (Pokud rozhraní stránkování neobsahuje přímý odkaz na poslední stránce, pak můžete kdykoli deaktivovat stavu zobrazení.)


Kliknutím na odkaz na poslední stránku vyvolá zpětné volání a dává pokyn prvku GridView. Chcete-li aktualizovat jeho `PageIndex` vlastnost. Pokud dojde ke kliknutí na odkaz na poslední stránku, přiřadí prvku GridView jeho `PageIndex` vlastnost na jednu hodnotu menší než jeho `PageCount` vlastnost. Zobrazení stavu Zakázáno `PageCount` hodnota se ztratí mezi ostatními postbacků a `PageIndex` přiřazen maximální celočíselnou hodnotu. V dalším kroku prvku GridView, pokusí se zjistit počáteční index řádku tak, `PageSize` a `PageCount` vlastnosti. Výsledkem je `OverflowException` od produktu je větší než velikost maximální povolené celé číslo.

## <a name="implement-custom-paging-and-sorting"></a>Implementujte vlastní stránkování a řazení

Naše aktuální vlastní implementaci stránkování vyžaduje objednávky, podle kterého je stránkování dat prostřednictvím staticky při vytváření `GetProductsPaged` uložené procedury. Můžete však pravděpodobně jste si poznamenali, že prvek GridView s inteligentním obsahuje zaškrtávací políčko Povolit řazení kromě možnost Povolit stránkování. Bohužel přidání podpory pro řazení do prvku GridView s naší aktuální vlastní implementaci stránkování bude pouze se záznamy řadily na aktuálně zobrazené stránce data. Například pokud nakonfigurujete prvku GridView a také podporuje stránkování, při prohlížení na první stránku dat, seřaďte podle názvu produktu v sestupném pořadí, bude převraťte pořadí produktů na stránce 1. Jak ukazuje obrázek 18, například Carnarvon tygři zobrazuje jako první produktu při řazení ve vzestupném abecedním pořadí, které ignoruje 71 jiné produkty, které po Carnarvon tygři abecedně; jen takové záznamy na první stránce jsou považovány za v řazení.


[![Má řazení proběhnout pouze Data zobrazí na aktuální stránce.](efficiently-paging-through-large-amounts-of-data-cs/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image22.png)

**Obrázek 18**: pouze Data zobrazí na aktuální stránce má řazení proběhnout ([kliknutím ji zobrazíte obrázek v plné velikosti](efficiently-paging-through-large-amounts-of-data-cs/_static/image24.png))


Řazení platí pouze pro aktuální stránku dat, protože řazení dochází po načetla data z knihoven BLL s `GetProductsPaged` metoda a tato metoda vrátí pouze záznamy pro konkrétní stránku. K implementaci neřadí správně, musíme předat výraz řazení pro `GetProductsPaged` metoda tak, aby data lze seřadit správně před vrácením konkrétní stránky data. Uvidíme, jak to provést v následujícím kurzem.

## <a name="implementing-custom-paging-and-deleting"></a>Implementace vlastní stránkování a odstranění

Pokud jste povolení odstraňování funkce ovládacího prvku GridView, jejichž data se stránkovaného fondu pomocí technik vlastní stránkování, zjistíte, které při odstraňování poslední záznam na poslední stránce, prvku GridView zmizí místo odpovídajícím způsobem dekrementace GridView s `PageIndex`. Chcete-li tuto chybu reprodukovat, povolte odstranění pro tento kurz, který právě jsme právě vytvořili. Přejdete na poslední stránku (stránka 9), kde byste měli od jsme procházení 81 produkty, 10 produkty najednou vidět jednoho produktu. Odstraňte tento produkt.

Při odstraňování poslední produktu prvku GridView *by měl* automaticky přejděte na stránku osmého a tato funkce je vystavených s výchozí stránkování. S vlastní stránkování, ale po odstranění posledního produktu na poslední stránce prvku GridView jednoduše dané zařízení zmizí z obrazovky úplně se vynechá. Přesný důvod *proč* tomu, že je bit nad rámec tohoto kurzu, najdete v článku [odstranění posledního záznamu na poslední stránce z prvku GridView s vlastní stránkování](http://scottonwriting.net/sowblog/posts/7326.aspx) nízké úrovně podrobnosti jde o zdroje Tento problém. Stručně řečeno ji s z důvodu následující sled kroků, které provádí prvku GridView při kliknutí na tlačítko Odstranit:

1. Odstranit záznam
2. Získat příslušné záznamy k zobrazení pro danou `PageIndex` a `PageSize`
3. Zaškrtněte, pokud chcete zajistit, aby `PageIndex` nepřekračuje počet stránek dat ve zdroji dat; pokud ji, automaticky sníží GridView s `PageIndex` vlastnost
4. Svázat s příslušnou stránku dat prvku GridView pomocí záznamů, kterou jste získali v kroku 2

Problém vyplývá ze skutečnosti, že v kroku 2 `PageIndex` použít, pokud kliknete na záznamy k zobrazení je stále `PageIndex` poslední stránky, jehož jediným záznam právě byl odstraněn. Proto se v kroku 2 *žádné* vzhledem k tomu, že poslední stránka dat již obsahuje nějaké záznamy, vrátí se záznamy. Pak v kroku 3 prvku GridView realizuje, jeho `PageIndex` vlastnost je větší než celkový počet stránek ve zdroji dat (od jsme ve odstranit poslední záznam na poslední stránce) a sníží proto jeho `PageIndex` vlastnost. V kroku 4 pokusí samotné připojení k datům získali v kroku 2; prvku GridView. ale v kroku 2 nebyly vráceny žádné záznamy, tedy výsledkem prázdná prvku GridView. Pomocí výchozího stránkování, tento problém není t surface protože v kroku 2 *všechny* ze zdroje dat se načítají záznamy.

Chcete-li vyřešit tím, který máme k dispozici dvě možnosti. První je vytvořit obslužnou rutinu události pro prvek GridView s `RowDeleted` obslužná rutina události, která určuje, kolik záznamů se zobrazí na stránce, který byl právě odstraněn. Pokud došlo jenom jeden záznam, pak záznam odstranit jenom nutné, aby byla poslední z nich a My potřebujeme se sníží GridView s `PageIndex`. Samozřejmě, chceme jenom aktualizovat `PageIndex` Pokud byla operace odstranění ve skutečnosti úspěšně, která se dají určit pomocí zajistit, aby `e.Exception` je vlastnost `null`.

Tento přístup funguje, protože aktualizuje `PageIndex` po kroku 1, ale před kroku 2. V kroku 2, proto se vrátí odpovídající sadu záznamů. K tomu použijte kód podobný tomuto:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample11.cs)]

Alternativní řešení je vytvořit obslužnou rutinu události pro prvek ObjectDataSource s `RowDeleted` událostí a nastavit `AffectedRows` vlastnost na hodnotu 1. Po odstranění záznamu v kroku 1 (ale před opětovné načtení dat v kroku 2), aktualizuje prvku GridView jeho `PageIndex` vlastnost Pokud operace vliv na jeden nebo více řádků. Ale `AffectedRows` vlastnost není nastavená ObjectDataSource a proto je tento krok vynechat. Jedním ze způsobů, aby tento krok provést je ručně nastavit `AffectedRows` vlastnosti, pokud se operace odstranění úspěšně dokončí. Můžete to provést pomocí kódu, jako je následující:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample12.cs)]

Kód pro oba tyto obslužné rutiny události najdete v třídě modelu code-behind `EfficientPaging.aspx` příklad.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Porovnání výkonu výchozí a vlastní stránkování

Protože vlastní stránkování pouze načte potřebné záznamy, že vrátí výchozí stránkování *všechny* záznamů pro jednotlivé stránky zobrazení, ho s vymazat, je efektivnější než výchozí stránkování vlastní stránkování. Ale stejně jak mnohem efektivnější je vlastní stránkování? Jaký druh zvýšení výkonu můžete zobrazit přechodem z výchozího stránkování na vlastní stránkování?

Bohužel neexistuje s bez velikosti, která tady všechny odpovědi. Zvýšení výkonu závisí na několika faktorech, většina viditelného dvě je počet záznamů stránkování prostřednictvím a zatížení umístit do databáze serveru a komunikačních kanálů mezi webovým serverem a serverem databáze. Pro malé tabulky s pár desítek záznamy může být nepatrné rozdíl ve výkonu. Pro velké tabulky s tisíci na stovky tisíc řádků, ale výkon rozdíl je čárkou nad vpravo.

Článek min, [vlastní stránkování v technologii ASP.NET 2.0 pomocí serveru SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), obsahuje některé testy výkonnosti mám spuštěných vykazovat rozdíly ve výkonu mezi tyto dva postupy stránkování při procházení tabulky databáze s 50 000 záznamů. V těchto testech můžu prověřit, čas k provedení dotazu na úrovni serveru SQL Server (pomocí [SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) a na stránku ASP.NET pomocí [funkce trasování s ASP.NET](https://msdn.microsoft.com/library/y13fw6we.aspx). Mějte na paměti, že tyto testy byly spuštěny na můj vývoj pole s jedním aktivním uživatelem a proto jsou unscientific a ne napodobují vzorů zatížení typické webu. Bez ohledu na to výsledky ukazují relativní rozdíly v čase spuštění pro výchozí a vlastní stránkování při práci s dostatečně velké objemy dat.


|  | **Střední Doba trvání (s)** | **Operace čtení** |
| --- | --- | --- |
| **Výchozí stránkování SQL Profiler** | 1.411 | 383 |
| **Vlastní stránkování SQL Profiler** | 0.002 | 29 |
| **Výchozí trasování procesu ASP.NET stránkování** | 2.379 | *NENÍ K DISPOZICI* |
| **Vlastní stránkování ASP.NET trasování** | 0.029 | *NENÍ K DISPOZICI* |


Jak je vidět, načtení konkrétní stránky dat vyžaduje 354 méně čtení v průměru a za zlomek času dokončení. Na stránce technologie ASP.NET, vlastní stránka byla schopna vykreslit v blízkosti 1/100<sup>th</sup> dobu, jakou trvalo při použití výchozího stránkování. Zobrazit [Moje článku](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) Další informace o těchto výsledků spolu s kódem a databázi, která si můžete stáhnout pro reprodukci tyto testy ve vlastním prostředí.

## <a name="summary"></a>Souhrn

Výchozí stránkování je velmi snadno implementovat pouze kontrola políčko Povolit stránkování v data webového ovládacího prvku s inteligentní značky, ale takové zjednodušení spočívá za cenu výkonu. Pomocí výchozího stránkování, když si uživatel vyžádá libovolnou stránku dat *všechny* záznamy se vrátí, i když se může zobrazit jenom zlomek je. K boji proti takové nároky na výkon, nabízí ObjectDataSource alternativní stránkování možnost vlastní stránkování.

Při vlastní stránkování dále to vylepšuje výchozí stránkování problémy s výkonem s načtením jen takové záznamy, které je třeba zobrazit, ho s více potřebný k implementaci vlastní stránkování. Dotaz musí být nejprve napsané, že, který správně (a efektivně) má přístup k určitou podmnožinu záznamů požadované. To lze provést různými způsoby; Ten jsme se zaměřili na v tomto kurzu, je použít SQL Server 2005 s novou `ROW_NUMBER()` funkce pro řazení výsledků a pak vrátit pouze ty výsledky jehož pořadí spadají do zadaného rozsahu. Kromě toho potřebujeme přidat prostředky, chcete-li zjistit celkový počet záznamů stránkování prostřednictvím. Po vytvoření tyto vrstvy DAL a BLL metody, musíme také nakonfigurovat ObjectDataSource tak, aby můžete určit, kolik celkem záznamů jsou stránkování prostřednictvím a správně můžete předat hodnoty Start Index řádku a maximální počet řádků BLL.

Při provádění vlastní stránkování vyžaduje několik kroků a není téměř stejně jednoduché jako výchozího stránkování, vlastní stránkování je nezbytné v případě procházení dostatečně velké objemy dat po stránkách. Jak prozkoumat výsledky ukázala, že, vlastní stránkování můžete zbavit sekund z doba vykreslování stránek ASP.NET a jedné nebo více řádů zesvětlit zatížení na serveru databáze.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](paging-and-sorting-report-data-cs.md)
> [další](sorting-custom-paged-data-cs.md)
