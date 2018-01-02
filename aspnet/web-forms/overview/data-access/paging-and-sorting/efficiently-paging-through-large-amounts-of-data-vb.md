---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
title: "Efektivní stránkování prostřednictvím velké objemy dat (VB) | Microsoft Docs"
author: rick-anderson
description: "Při práci s velkými objemy dat, jako jeho základní retriev řízení zdroje dat není vhodná výchozí možnost stránkování ovládacího prvku prezentace dat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 3e20e64a-8808-4b49-88d6-014e2629d56f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 00a5358361fa3f37d13ea74d61c437088b388ece
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="efficiently-paging-through-large-amounts-of-data-vb"></a>Efektivní stránkování prostřednictvím velké objemy dat (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_VB.exe) nebo [stáhnout PDF](efficiently-paging-through-large-amounts-of-data-vb/_static/datatutorial25vb1.pdf)

> Výchozí možnost stránkování ovládacího prvku prezentace dat není vhodná, při práci s velkými objemy dat, jako ovládací prvek jeho základní zdroj dat načte všechny záznamy, i když se zobrazí pouze podmnožinu dat. V takových případech jsme musíte zapnout pro vlastní stránkování.


## <a name="introduction"></a>Úvod

Jak již bylo zmíněno v předchozím kurzu, můžou se implementovat stránkování v jedním ze dvou způsobů:

- **Výchozí stránkování** může být implementováno jednoduše kontrolou možnost Povolit stránkování v datech ovládací prvek webu s inteligentní značky; však vždy, když je zobrazení stránky dat, ObjectDataSource načte *všechny* záznamy, dokonce i Přestože jsou zobrazena pouze podmnožina z nich na stránce
- **Vlastní stránkování** zlepšuje výkon výchozí stránkování načtením pouze záznamy z databáze, který se má zobrazit na konkrétní stránku dat, které požadoval uživatel, potřebujete však vlastní stránkování vyžaduje trochu další úsilí k implementaci než výchozí stránkování

Z důvodu usnadnění implementace jenom kontroly zaškrtávací políčko a níž Hotovo. výchozí stránkování je lákavou možnost. Svůj přístup na sunout při načítání všech záznamů, ale je nepravděpodobné číselné volbou při procházení dostatečně velké objemy dat nebo pro servery s více souběžných uživatelů. V takových případech musí jsme zapněte na vlastní stránkování Chcete-li poskytovat přizpůsobivý systému.

Na výzvu vlastní stránkování se moci napsat dotaz, který vrátí sadu záznamů, které jsou potřeba pro konkrétní stránku dat přesné. Naštěstí Microsoft SQL Server 2005 poskytuje new – klíčové slovo pro výsledky hodnocení, umožňující nám vytvořit dotaz, který můžete efektivně načíst správné podmnožinu záznamů. V tomto kurzu jsme zobrazí, jak implementovat vlastní stránkování v ovládacím prvku GridView pomocí této new – klíčové slovo systému SQL Server 2005. I když je stejná jako pro výchozí stránkování, krokování z jedné stránky na další pomocí uživatelského rozhraní pro vlastní stránkování vlastní stránkování může být několik pořadí podle velikosti rychleji než výchozí stránkování.

> [!NOTE]
> Přesný výkonnější vykazují vlastní stránkování závisí na celkový počet záznamů stránkování prostřednictvím a zatížení je umístěn na serveru databáze. Na konci tohoto kurzu podíváme některé hrubý metriky, které prezentují výhod výkonu získaných pomocí vlastní stránkování.


## <a name="step-1-understanding-the-custom-paging-process"></a>Krok 1: Pochopení vlastní proces stránkování

Při procházení dat, přesné záznamy zobrazené na stránce závisí na stránce dat požadovanou a počet záznamů zobrazených na stránce. Představte si například, že jsme chtěli 81 produkty, procházet po stránkách zobrazení 10 produktů na stránce. Při zobrazení na první stránku, d chceme produkty 1 až 10; Při zobrazení na druhé stránce jsme d zajímat produkty 11 až 20 a tak dále.

Existují tři proměnné, které určují, co je třeba záznamy načíst a jakým způsobem má být vykreslen rozhraní stránkování:

- **Počáteční řádek indexu** index první řádek na stránce data k zobrazení; to index může být vypočítána vynásobí záznamy zobrazených na stránce index stránky a přidávání jednoho. Například při procházení záznamy 10 současně, na první stránce (jehož index stránky je 0), řádek indexu spustit je 0 \* 10 + 1 nebo 1; pro druhé stránce (jehož index stránky je 1), řádek indexu spustit je 1 \* 10 + 1 , nebo 11.
- **Maximální počet řádků** maximální počet záznamů zobrazených na stránce. Tato proměnná se označuje jako maximální počet řádků, protože poslední existuje stránka může být méně záznamů vrácených než je velikost stránky. Například když stránkování prostřednictvím 81 produkty 10 záznamů na stránku, deváté a poslední stránka bude mít jenom jeden záznam. Neobsahuje žádné stránky, ale zobrazí další záznamy než hodnota maximální počet řádků.
- **Celkový počet záznamů** celkový počet záznamů stránkování prostřednictvím. Při této proměnné neběží t se potřeba zjistit, co záznamů pro danou stránku, určují rozhraní stránkování. Například pokud jsou 81 produkty stránkování prostřednictvím, rozhraní stránkování zná zobrazíte devět čísla stránek v stránkování uživatelského rozhraní.

S výchozí stránkování, je výpočet řádek indexu spustit jako produktu index stránky a velikost stránky a jeden, zatímco maximální počet řádků je jednoduše velikost stránky. Vzhledem k tomu, že výchozí stránkování načte všechny záznamy z databáze při vykreslování jakoukoli stránku dat, pro každý řádek indexu se označuje, a díky přesunutí spustit Index řádku řádek jednoduchý úkol. Kromě toho je snadno dostupné, protože celkový počet záznamů s celkový počet záznamů v DataTable (nebo jakýkoliv objekt se používá k ukládání výsledků databáze).

Zadané proměnné spustit Index řádku a maximální počet řádků, vlastní implementaci stránkování musí vrátit pouze přesné podmnožinu spuštění Index řádku spustit a až do maximální počet řádků počet záznamů, po který záznamů. Vlastní stránkování poskytuje dva problémy:

- Jsme musí mít efektivní přidružit každý řádek v celého datového stránkování prostřednictvím tak, aby můžeme začít vrácení záznamy na zadaný Index řádku spustit index řádku
- Je potřeba zadat celkový počet záznamů stránkování prostřednictvím

V následujících dvou krocích podíváme skript SQL se musel reagovat na tyto dva problémy. Kromě skript SQL budete také musíme implementovat metody DAL a BLL.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Krok 2: Vrátí celkový počet záznamů stránkování prostřednictvím

Před jsme zkontrolujte postup načtení přesné podmnožinu záznamů pro stránku se zobrazuje, umožní s první pohled na tom, jak vrátit celkový počet záznamů stránkování prostřednictvím. Tyto informace je třeba správně nakonfigurovat uživatelské rozhraní stránkování. Celkový počet záznamů vrácených konkrétní dotazu SQL můžete získat pomocí [ `COUNT` agregační funkce](https://msdn.microsoft.com/en-US/library/ms175997.aspx). Například můžete určit celkový počet záznamů v `Products` tabulky, můžeme použít následující dotaz:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample1.sql)]

Umožní s přidání metody do našich DAL, který vrací tyto informace. Konkrétně vytvoříme DAL metodu s názvem `TotalNumberOfProducts()` , která se spouští `SELECT` příkaz uvedené výše.

Začněte otevřením `Northwind.xsd` soubor typové datové sady `App_Code/DAL` složky. Pak klikněte pravým tlačítkem na `ProductsTableAdapter` v návrháři a zvolte dotazu přidat. Jako jsme jste viděli v předchozí kurzech to vám umožní na přidání nové metody vrstvy Dal, při vyvolání, provede konkrétní příkaz jazyka SQL nebo uloženou proceduru. Stejně jako u našich TableAdapter metody v předchozí kurzy, použijte příkaz SQL ad-hoc rozhodnout pro tento jeden.


![Použití příkazu SQL Ad-Hoc](efficiently-paging-through-large-amounts-of-data-vb/_static/image1.png)

**Obrázek 1**: použití příkazu SQL Ad-Hoc


Na další obrazovce můžeme určit, jaký typ dotaz k vytvoření. Vzhledem k tomu, že tento dotaz vrátí jeden, skalární hodnotu celkový počet záznamů v `Products` vyberte tabulku `SELECT` která vrací hodnotu možnost jednom.


![Konfigurace dotaz, který bude použit příkaz SELECT, který vrací jedinou hodnotu](efficiently-paging-through-large-amounts-of-data-vb/_static/image2.png)

**Obrázek 2**: Konfigurace dotaz, který bude použit příkaz SELECT, který vrací jedinou hodnotu


Po označující typ dotazu používat, musí další určíme dotazu.


![Použití vyberte COUNT(*) z produktů dotazu](efficiently-paging-through-large-amounts-of-data-vb/_static/image3.png)

**Obrázek 3**: použít vyberte počet (\*) FROM produkty dotazu


Nakonec zadejte název metody. Jako s zmíněnými, umožňují použít `TotalNumberOfProducts`.


![Název TotalNumberOfProducts DAL – metoda](efficiently-paging-through-large-amounts-of-data-vb/_static/image4.png)

**Obrázek 4**: název TotalNumberOfProducts DAL – metoda


Po kliknutí na tlačítko Dokončit, průvodce přidá `TotalNumberOfProducts` metoda vrstvy Dal. Skalární vracejících metody v DAL vrátit typy podporující hodnoty Null v případě, že je výsledek v dotazu SQL `NULL`. Naše `COUNT` dotaz, ale vždy vrátí jinou hodnotu než`NULL` hodnoty; bez ohledu na to, vrátí metoda DAL s možnou hodnotou Null celé číslo.

Kromě metodu DAL potřebujeme také metoda v BLL. Otevřete `ProductsBLL` třídy souboru a přidejte `TotalNumberOfProducts` metoda, která jednoduše volá, aby se DAL s `TotalNumberOfProducts` metoda:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample2.vb)]

DAL s `TotalNumberOfProducts` metoda vrátí celé číslo s možnou hodnotou Null, nicméně jsme vytvořili, `ProductsBLL` třídu s `TotalNumberOfProducts` metody, které se vrátí standardní celé číslo. Proto je potřeba mít `ProductsBLL` třídu s `TotalNumberOfProducts` metoda vrátí hodnotu část s možnou hodnotou Null celé číslo vrácené DAL s `TotalNumberOfProducts` metoda. Volání `GetValueOrDefault()` vrací hodnotu na celé číslo s možnou hodnotou Null, pokud existuje; Pokud je na celé číslo s možnou hodnotou Null `null`, ale vrátí výchozí hodnota celé číslo, 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Krok 3: Vrácení přesné dílčí sady záznamů

Naše dalším krokem je vytvoření metod v DAL a BLL, který přijímat Index řádku spustit a maximální počet řádků proměnné už jsme probírali výše a vrátí příslušné záznamy. Než to, umožňují s první pohled na potřebné skript SQL. Výzvy, kterým čelí nám je, že jsme musí být schopen efektivně přiřadit indexu na každý řádek ve výsledcích celý stránkování prostřednictvím tak, aby vrátíme pouze tyto záznamy spouštění na řádek indexu spustit (a až zaznamenává maximální počet záznamů).

Toto není výzvu, pokud již existuje sloupec v tabulce databáze, která slouží jako index řádku. Na první pohled může myslíme si, která `Products` tabulky s `ProductID` by stačit pole, jako má první produktu `ProductID` 1, druhý a 2, a tak dále. Odstranění produktu však zanechává mezera v sekvenci, anulovány tento přístup.

Existují dvě obecné postupy používané k efektivní přidružit index řádku s daty na stránku prostřednictvím, zpřístupňují přesné dílčí sadu záznamů, které mají být načteny:

- **Pomocí systému SQL Server 2005 s `ROW_NUMBER()` – klíčové slovo** nový SQL Server 2005, `ROW_NUMBER()` – klíčové slovo přidruží hodnoticí každý vrácený záznam podle pořadí. Toto pořadí slouží jako index řádku pro každý řádek.
- **Pomocí proměnné tabulky a `SET ROWCOUNT`**  systému SQL Server s [ `SET ROWCOUNT` příkaz](https://msdn.microsoft.com/en-us/library/ms188774.aspx) umožňuje určit počet celkový počet záznamů dotaz by měl zpracovat před ukončením; [tabulky proměnné](http://www.sqlteam.com/item.asp?ItemID=9454) jsou místní proměnné T-SQL, které mohou být uloženy tabulková data akin k [dočasných tabulek](http://www.sqlteam.com/item.asp?ItemID=2029). Tento postup funguje stejně dobře s Microsoft SQL Server 2005 a SQL Server 2000 (vzhledem k tomu `ROW_NUMBER()` přístup pracuje pouze s SQL Server 2005).  
  
 Rada tady je vytvoření proměnné tabulky, která má `IDENTITY` sloupce a sloupce pro primární klíče v tabulce, jejichž data se prostřednictvím stránkování. V dalším kroku obsah tabulky, jejichž data je stránkování prostřednictvím vypsána do proměnné tabulky, a tím přidružení sekvenční řádek indexu (prostřednictvím `IDENTITY` sloupec) pro každý záznam v tabulce. Po naplnění proměnnou tabulky `SELECT` příkaz na proměnnou tabulky spojena s podkladové tabulce, mohou být provedeny vysunout konkrétní záznamy. `SET ROWCOUNT` Se použije příkaz inteligentně omezit počet záznamů, které je třeba uložit do proměnné tabulky.  
  
 Této efektivity přístup s je založena na číslo stránky, které jsou požadovány, jako `SET ROWCOUNT` hodnota je přiřazena na hodnotu Index řádku spustit a maximální počet řádků. Při procházení nízká číslované stránky, jako je například první několik stránek dat. Tento přístup je velmi efektivní. Ale vykazuje výkonu stránkování jako výchozí při načítání stránky poblíž konce.

V tomto kurzu implementuje pomocí vlastní stránkování `ROW_NUMBER()` – klíčové slovo. Další informace o používání proměnnou tabulky a `SET ROWCOUNT` techniku, najdete v části [A Další efektivní metodu pro stránkování prostřednictvím velkých sad výsledků dotazu](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

`ROW_NUMBER()` – Klíčové slovo přidružené hodnoticí jednotlivých záznamů vrácených přes konkrétní řazení pomocí následující syntaxe:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample3.sql)]

`ROW_NUMBER()`vrátí číselnou hodnotu, která určuje pořadí pro každý záznam s ohledem na uvedené řazení. Například zobrazíte pořadí pro jednotlivé produkty, seřazené z nejvíc nákladné nejmenší, můžeme použít následující dotaz:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample4.sql)]

Obrázek 5 ukazuje tento dotaz s výsledky při spuštění pomocí okna dotazu v sadě Visual Studio. Všimněte si, že tyto produkty jsou seřazené podle ceníku spolu s pořadí ceny pro každý řádek.


![Cena pořadí je zahrnuté pro každý záznam se vrátí](efficiently-paging-through-large-amounts-of-data-vb/_static/image5.png)

**Obrázek 5**: je zahrnuta pořadí ceny pro každý záznam se vrátí


> [!NOTE]
> `ROW_NUMBER()`je jen jednou z mnoha nových funkcí hodnocení k dispozici v systému SQL Server 2005. Podrobnější diskuzi o `ROW_NUMBER()`, spolu s dalšími funkcemi hodnocení, přečtěte si [vrácení seřazeny výsledků s Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).


Při řazení výsledků k zadanému `ORDER BY` sloupec v `OVER` – klauzule (`UnitPrice`, v předchozím příkladu), SQL Server musí řazení výsledků. To je rychlý operace v případě clusterovaného indexu přes sloupcům, které jsou právě výsledky seřazené podle, nebo pokud je zahrnut indexu, ale může být dražší jinak. Zlepšení výkonu pro dotazy na dostatečně velký, zvažte přidání neclusterovaný index pro sloupec, podle kterého jsou výsledky seřazené podle. V tématu [řazení funkcí a výkonu v systému SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) pro podrobnější pohled na důležité informace o výkonu.

Informace o hodnocení, vrácené `ROW_NUMBER()` nelze použít přímo v `WHERE` klauzule. Však lze vrátit odvozené tabulky `ROW_NUMBER()` výsledek, který lze potom zobrazí v `WHERE` klauzule. Například následující dotaz používá odvozenou tabulku vrátit sloupce ProductName a UnitPrice spolu s `ROW_NUMBER()` výsledek a pak používá `WHERE` klauzule pouze vráceny produkty, jejichž pořadí cena je 11 až 20:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample5.sql)]

Rozšíření tento koncept trochu další, můžeme využívat tuto metodu pro načtení konkrétní stránky požadované hodnoty Start Index řádku a maximální počet řádků dat:


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample6.html)]

> [!NOTE]
> Jak jsme se zobrazí později v tomto kurzu  *`StartRowIndex`*  poskytl ObjectDataSource je indexovaný začínající na nule, zatímco `ROW_NUMBER()` hodnoty vrácené systému SQL Server 2005 je indexovaný začínajícím hodnotou 1. Proto `WHERE` klauzule vrací tyto záznamy kde `PriceRank` striktně větší než  *`StartRowIndex`*  a menší než nebo rovno  *`StartRowIndex`*   +  *`MaximumRows`*.


Nyní který jsme sunout popsané jak `ROW_NUMBER()` lze použít k načtení konkrétní stránky data hodnoty Start Index řádku a maximální počet řádků, teď musíme implementovat tuto logiku jako metody v DAL a BLL.

Při vytváření tohoto dotazu, že jsme musíte se rozhodnout řazení podle kterého výsledky se určí; umožní s produkty seřadit podle názvu jejich v abecedním pořadí. To znamená, že s vlastní implementaci stránkování v tomto kurzu jsme nebude možné vytvořit vlastní sestavu stránkové, než můžete také řadit. V dalším kurzu ale uvidíme jak takové funkce lze zadat.

V předchozí části jsme vytvořili metodu DAL jako příkaz SQL ad hoc. Bohužel analyzátor T-SQL v sadě Visual Studio používá t nemá průvodce nastavením TableAdapter jako `OVER` syntaxe používané `ROW_NUMBER()` funkce. Tato metoda DAL jsme proto musíte vytvořit jako uložené procedury. Vyberte Průzkumníka serveru v nabídce zobrazení (nebo v počtu Ctrl + Alt + S) a rozbalte `NORTHWND.MDF` uzlu. Pokud chcete přidat nové uložené procedury, klikněte pravým tlačítkem na uzel uložené procedury a zvolte možnost Přidat nové uloženou proceduru (viz obrázek 6).


![Přidat nové uložené procedury pro stránkování prostřednictvím produkty](efficiently-paging-through-large-amounts-of-data-vb/_static/image6.png)

**Obrázek 6**: Přidání nové uložené procedury pro stránkování prostřednictvím produkty


Tuto uloženou proceduru musí přijmout dva vstupní parametry celé číslo - `@startRowIndex` a `@maximumRows` a použít `ROW_NUMBER()` funkce seřazené podle `ProductName` pole, které vrací pouze řádky větší než zadaná `@startRowIndex` a menší než nebo rovno `@startRowIndex`  +  `@maximumRow` s. Zadejte následující skript do nové uložené procedury a pak klikněte na ikonu Uložit Přidat uloženou proceduru k databázi.


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample7.sql)]

Po vytvoření uložené procedury, pozorně otestování. Klikněte pravým tlačítkem na `GetProductsPaged` uložené procedury název v Průzkumníku serveru a vyberte možnost spustit. Visual Studio se potom zobrazí výzvu pro vstupní parametry `@startRowIndex` a `@maximumRow` s (viz obrázek 7). Zkuste různé hodnoty a podívejte se na výsledky.


![Zadejte hodnotu @startRowIndex a @maximumRows parametry](efficiently-paging-through-large-amounts-of-data-vb/_static/image7.png)

**Obrázek 7**: Zadejte hodnotu @startRowIndex a @maximumRows parametry


Po výběru těchto vstupních hodnot parametrů, ve výstupním okně se zobrazí výsledky. Obrázek 8 ukazuje výsledky při předávání v 10 pro oba `@startRowIndex` a `@maximumRows` parametry.


[![Jsou vráceny záznamy, by se zobrazí v druhé Data stránky](efficiently-paging-through-large-amounts-of-data-vb/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image8.png)

**Obrázek 8**: záznamů, by se zobrazí v druhé stránce Data jsou vrácena ([Kliknutím zobrazit obrázek v plné velikosti](efficiently-paging-through-large-amounts-of-data-vb/_static/image10.png))


Pomocí této uložené procedury vytvořili, jsme re připravené k vytvoření `ProductsTableAdapter` metoda. Otevřete `Northwind.xsd` zadané v datové sadě klikněte pravým tlačítkem v `ProductsTableAdapter`a zvolte možnost Přidat dotazu. Místo vytvoření dotazu pomocí příkazu SQL ad-hoc, vytvořte ho pomocí existující uložené procedury.


![Create – metoda DAL použití existující uložené procedury](efficiently-paging-through-large-amounts-of-data-vb/_static/image11.png)

**Obrázek 9**: vytvoření metody DAL použití existující uložené procedury


V dalším kroku jsme vyzváni k výběru uložená procedura vyvolat. Vyberte `GetProductsPaged` uložené procedury z rozevíracího seznamu.


![Vyberte GetProductsPaged uložené procedury z rozevíracího seznamu](efficiently-paging-through-large-amounts-of-data-vb/_static/image12.png)

**Obrázek 10**: Zvolte GetProductsPaged uložené procedury z rozevíracího seznamu


Na další obrazovce pak zeptá, jaký typ dat je vrácený uložené procedury: tabulková data, jedna hodnota nebo žádná hodnota. Vzhledem k tomu `GetProductsPaged` uložená procedura může vrátit více záznamů, znamenat, že vrací tabulková data.


![Označuje, že uložené procedury vrátí tabulková Data](efficiently-paging-through-large-amounts-of-data-vb/_static/image13.png)

**Obrázek 11**: znamenat, že uložené procedury vrátí tabulková Data


Nakonec znamenat názvy metod, které chcete jste vytvořili. Stejně jako u naše kurzy předchozí pokračujte a vytvořte metod pomocí obou výplně DataTable a vrátit DataTable. Název první metodu `FillPaged` a druhý `GetProductsPaged`.


![Název metody FillPaged a GetProductsPaged](efficiently-paging-through-large-amounts-of-data-vb/_static/image14.png)

**Obrázek 12**: název metody FillPaged a GetProductsPaged


Kromě toho vytvořit DAL metody vrátit konkrétní stránky produktů, také potřebujeme nabízí takové funkce v BLL. Jako metodu DAL BLL s GetProductsPaged metoda dva vstupy celé číslo pro zadání spustit Index řádku a maximální počet řádků, musíte přijmout a musí vracet pouze tyto záznamy, které spadají do zadaného rozsahu. Vytvořte BLL metody ve třídě ProductsBLL jenom volání dolů do vrstvy DAL s GetProductsPaged metoda, například takto:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample8.vb)]

Můžete použít libovolný název pro vstupní parametry BLL metoda s ale, jak jsme se zobrazí za chvíli rozhodnete použít `startRowIndex` a `maximumRows` uloží nám z další bit práce při konfiguraci ObjectDataSource při použití této metody.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Krok 4: Konfigurace ObjectDataSource používat vlastní stránkování

Metody BLL a DAL pro přístup k určité podmnožiny záznamů dokončení jsme re připravené k vytvoření GridView kontrolu této stránky prostřednictvím jeho základní záznamů pomocí vlastní stránkování. Začněte otevřením `EfficientPaging.aspx` stránku `PagingAndSorting` složku, rutina GridView přidat na stránku a nakonfigurujte ho na použití nového ovládacího prvku ObjectDataSource. V našem posledních kurzech jsme často měli ObjectDataSource nakonfigurovaná pro použití `ProductsBLL` třídu s `GetProducts` metoda. Tentokrát ale chceme používat `GetProductsPaged` metoda místo toho od `GetProducts` metoda vrátí *všechny* produktů v databázi zatímco `GetProductsPaged` vrátí pouze konkrétní podmnožinu záznamů.


![Konfigurace ObjectDataSource lze pomocí této metody GetProductsPaged ProductsBLL třídu s](efficiently-paging-through-large-amounts-of-data-vb/_static/image15.png)

**Obrázek 13**: Konfigurace ObjectDataSource lze pomocí této metody GetProductsPaged ProductsBLL třídu s


Od jsme re vytváření jen pro čtení GridView za chvíli nastavit metoda rozevíracím seznamu v příkaz INSERT, UPDATE a odstraňte karty na (žádný).

V dalším kroku ObjectDataSource průvodce k zadání nám zdroje `GetProductsPaged` metoda s `startRowIndex` a `maximumRows` vstupních hodnot parametrů. Tyto vstupní parametry, jednoduše se ve skutečnosti nastavit GridView automaticky nechte zdrojovou sadu na hodnotu None a klikněte na tlačítko Dokončit.


![Nechte zdroje vstupní parametr jako None](efficiently-paging-through-large-amounts-of-data-vb/_static/image16.png)

**Obrázek 14**: ponechte zdroje vstupní parametr jako None


Po dokončení Průvodce ObjectDataSource, bude GridView obsahovat BoundField nebo vlastnost CheckBoxField pro každé pole data produktu. Nebojte se, že přizpůsobení vzhledu s GridView podle svých potřeb. I sunout vyjádřit výslovný souhlas pro zobrazení jenom `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`, a `UnitPrice` BoundFields. Navíc nakonfigurujte GridView pro podporu stránkování zaškrtnutím políčka Povolit stránkování v jeho inteligentních značek. Po provedení těchto změn GridView a ObjectDataSource deklarativní by měl vypadat podobně jako následující:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample9.aspx)]

Pokud navštívíte stránku prostřednictvím prohlížeče, ale GridView je žádná, kdy chcete vyhledat.


![GridView se nezobrazí](efficiently-paging-through-large-amounts-of-data-vb/_static/image17.png)

**Obrázek 15**: The GridView se nezobrazí


Protože ObjectDataSource aktuálně používá 0 jako hodnoty pro obě chybí GridView `GetProductsPaged` `startRowIndex` a `maximumRows` vstupní parametry. Proto výsledný dotaz SQL vrací žádné záznamy a proto GridView nezobrazuje.

Chcete-li to opravit, je potřeba nakonfigurovat ObjectDataSource používat vlastní stránkování. Můžete to provést v následujících krocích:

1. **Nastavit ObjectDataSource s `EnablePaging` vlastnost `true`**  tím je oznámeno ObjectDataSource, který je nutné předat `SelectMethod` dva další parametry: jeden k určení Index řádku spustit ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) a jeden, který zadejte maximální počet řádků ([`MaximumRowsParameterName`](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Nastavit ObjectDataSource s `StartRowIndexParameterName` a `MaximumRowsParameterName` vlastnosti odpovídajícím způsobem** `StartRowIndexParameterName` a `MaximumRowsParameterName` vlastnosti označují názvy vstupní parametry předané do `SelectMethod` pro vlastní účely stránkování . Ve výchozím nastavení, jsou tyto názvy parametrů `startIndexRow` a `maximumRows`, což je důvod, proč, při vytváření `GetProductsPaged` metoda v BLL, lze použít tyto hodnoty pro vstupní parametry. Pokud jste se rozhodli použít jiný parametr názvy BLL s `GetProductsPaged` metoda jako `startIndex` a `maxRows`pro příklad by bylo potřeba nastavit ObjectDataSource s `StartRowIndexParameterName` a `MaximumRowsParameterName` vlastnosti odpovídajícím způsobem (například počáteční index pro `StartRowIndexParameterName` a maxRows pro `MaximumRowsParameterName`).
3. **Nastavit ObjectDataSource s [ `SelectCountMethod` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) na název metody, která vrátí celkový počet z záznamy se stránkovaného prostřednictvím (`TotalNumberOfProducts`)** odvolat, který `ProductsBLL` třídu s `TotalNumberOfProducts`metoda vrátí celkový počet záznamů stránkování prostřednictvím DAL metodu, která provede `SELECT COUNT(*) FROM Products` dotazu. Tyto informace je třeba pomocí ObjectDataSource správně vykreslení rozhraní stránkování.
4. **Odeberte `startRowIndex` a `maximumRows` `<asp:Parameter>` elementy z ObjectDataSource s deklarativní** při konfiguraci ObjectDataSource prostřednictvím průvodce, Visual Studio automaticky přidá dva `<asp:Parameter>` prvky `GetProductsPaged` metoda s vstupní parametry. Nastavením `EnablePaging` k `true`, tyto parametry se předají automaticky, pokud se zobrazí také v deklarativní syntaxi, bude ObjectDataSource pokus předat *čtyři* parametry, které `GetProductsPaged` – metoda a dva parametry pro `TotalNumberOfProducts` metoda. Pokud zapomenete pro jejich odebrání `<asp:Parameter>` prvky, pokud na stránce prostřednictvím prohlížeče získáte chybovou zprávu jako: *ObjectDataSource 'ObjectDataSource1' nelze najít neobecnou metodu 'TotalNumberOfProducts', který má Parametry: startRowIndex maximumRows*.

Po provedení těchto změn, deklarativní syntaxi s ObjectDataSource by měl vypadat následovně:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample10.aspx)]

Všimněte si, že `EnablePaging` a `SelectCountMethod` byly nastaveny a `<asp:Parameter>` byl odebrán. 16 obrázek ukazuje snímek obrazovky okna vlastnosti po provedly tyto změny.


![Pokud chcete použít vlastní stránkování, nakonfigurujte ovládacího prvku ObjectDataSource](efficiently-paging-through-large-amounts-of-data-vb/_static/image18.png)

**Obrázek 16**: vlastní stránkování, nakonfigurovat ovládacího prvku ObjectDataSource


Po provedení těchto změn, najdete na této stránce prostřednictvím prohlížeče. Měli byste vidět uvedeny, 10 produkty v abecedním pořadí. Za chvíli krok prostřednictvím jednu stránku dat najednou. Když není žádný visual rozdíl z hlediska koncového uživatele s mezi výchozí stránkování a vlastní stránkování, vlastní stránkování efektivněji stránky prostřednictvím velké objemy dat jako načítat pouze tyto záznamy, které musí být zobrazen pro danou stránku.


[![Data, Objednáno produktu s názvem, je stránkovaného fondu pomocí vlastní stránkování](efficiently-paging-through-large-amounts-of-data-vb/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image19.png)

**Obrázek 17**: dat, Objednáno produktu s názvem, je stránkovaného fondu pomocí vlastní stránkování ([Kliknutím zobrazit obrázek v plné velikosti](efficiently-paging-through-large-amounts-of-data-vb/_static/image21.png))


> [!NOTE]
> S vlastní stránkování, počet stránce hodnoty vrácené ObjectDataSource s `SelectCountMethod` je uložený ve stavu zobrazení s GridView. Ostatní GridView proměnné `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` kolekce a tak dále jsou uloženy v *řízení stavu*, který je jako trvalý, bez ohledu na hodnotu GridView s `EnableViewState` Vlastnost. Vzhledem k tomu `PageCount` hodnota je trvalé napříč postback pomocí zobrazení stavu, při použití rozhraní stránkování, která obsahuje odkaz na můžete přejít na poslední stránce, je nutné, aby byl povolen stav zobrazení s GridView. (Pokud vaše rozhraní stránkování neobsahuje přímý odkaz na poslední stránce, pak může zakázat stav zobrazení.)


Kliknutím na odkaz na poslední stránku způsobí, že zpětné volání a dává pokyn GridView aktualizovat jeho `PageIndex` vlastnost. Po kliknutí na odkaz na poslední stránku GridView přiřadí jeho `PageIndex` vlastnost na hodnotu jedna menší než jeho `PageCount` vlastnost. Zobrazení stavu Zakázáno `PageCount` hodnotu dojde ke ztrátě napříč postback a `PageIndex` přiřazen maximální celočíselná hodnota. V dalším kroku se pokusí určit počáteční index řádku vynásobením GridView `PageSize` a `PageCount` vlastnosti. To vede `OverflowException` vzhledem k tomu, že produkt je větší než velikost maximální povolené celé číslo.

## <a name="implement-custom-paging-and-sorting"></a>Řazení a stránkování vlastní implementace

Naše aktuální vlastní implementaci stránkování vyžaduje pořadí, ve kterém je stránkovaného data prostřednictvím staticky při vytváření `GetProductsPaged` uložené procedury. Však můžete může mít poznamenat, že rutina GridView s inteligentním obsahuje políčko Povolit řazení kromě možnost Povolit stránkování. Bohužel se přidání podpory pro řazení do GridView s naše aktuální vlastní implementaci stránkování pouze seřadíte záznamy na stránce aktuálně zobrazených dat. Například pokud nakonfigurujete GridView také podporují stránkování a potom při prohlížení na první stránku dat, řadit podle názvu produktu v sestupném pořadí, bude zpětná pořadí produktů na stránce 1. Jak ukazuje obrázek 18, například Carnarvon tygři zobrazuje jako první produktu řazení ve vzestupném abecedním pořadí, který ignoruje 71 ostatních produktů po Carnarvon tygři abecedně; na první stránce pouze tyto záznamy jsou považovány za v toto řazení.


[![Má řazení proběhnout na Data zobrazují pouze na aktuální stránce.](efficiently-paging-through-large-amounts-of-data-vb/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image22.png)

**Obrázek 18**: na Data zobrazují pouze na aktuální stránce má řazení proběhnout ([Kliknutím zobrazit obrázek v plné velikosti](efficiently-paging-through-large-amounts-of-data-vb/_static/image24.png))


Toto řazení se vztahuje pouze na aktuální stránku dat vzhledem k tomu, že toto řazení dochází po načtení dat z BLL s `GetProductsPaged` metoda a tato metoda vrátí pouze záznamy pro konkrétní stránky. K implementaci řazení správně, je potřeba předat výraz řazení, který se `GetProductsPaged` metoda tak, aby data můžete před vrácením na konkrétní stránku dat seřazeny správně. Jak to provést v našem kurzu další uvidíme.

## <a name="implementing-custom-paging-and-deleting"></a>Implementace vlastních stránkování a odstranění

Pokud jste povolení odstraňování funkcí v GridView, jejichž data se stránkovaného fondu pomocí technik vlastní stránkování, zjistíte, které při odstraňování poslední záznam na poslední stránce, GridView zmizí místo správně dekrementace GridView s `PageIndex`. Chcete-li reprodukovat této chyby, povolte odstraňování pro tento kurz, který právě jsme právě vytvořili. Přejděte na poslední stránku (stránka 9), kde měli vidět jednoho produktu, protože jsme se stránkování prostřednictvím 81 produkty, 10 produkty najednou. Odstranění tohoto produktu.

Při odstraňování poslední produktu GridView *by měl* automaticky přejít na stránku osmého a takové funkce je vystavených s výchozí stránkování. S vlastní stránkování, ale po odstranění poslední produktu na poslední stránce GridView jednoduše zmizí z obrazovky úplně. Přesné příčině *proč* tomu, že je trochu nad rámec tohoto kurzu, zjistit [odstranění poslední záznam na poslední stránce z GridView s vlastní stránkování](http://scottonwriting.net/sowblog/posts/7326.aspx) nízké úrovně podrobnosti, zdroj Tento problém. V souhrnu ho s z důvodu následující posloupnosti kroků, které provádí GridView při kliknutí na tlačítko Odstranit:

1. Odstranit záznam
2. Získání příslušných záznamů má být zobrazen pro zadaný `PageIndex` a`PageSize`
3. Zkontrolujte, ujistěte se, že `PageIndex` není delší než počet stránek dat ve zdroji dat; pokud ji, automaticky snížení GridView s `PageIndex` vlastnost
4. Rutina GridView pomocí záznamy získané v kroku 2 vytvořit vazbu na příslušnou stránku dat

Problém vyplývá z faktu, že v kroku 2 `PageIndex` používá při metodou záznamy, které chcete zobrazit stále `PageIndex` poslední stránky, jehož jediným záznam právě byla odstraněna. Proto v kroku 2 *žádné* záznamy jsou vráceny vzhledem k tomu, že poslední stránka dat už neobsahuje žádné záznamy. Potom v kroku 3 GridView rozpoznává, jeho `PageIndex` vlastnost je větší než celkový počet stránek ve zdroji dat (od jsme sunout odstranit poslední záznam na poslední stránku) a proto snižuje jeho `PageIndex` vlastnost. V kroku 4 GridView pokusí o samotné vazbu data načtená v kroku 2; ale v kroku 2 se nevrátily žádné záznamy, proto výsledkem prázdná GridView. S výchozí stránkování, tento problém nemá t surface, protože v kroku 2 *všechny* ze zdroje dat se načítají záznamy.

Chcete-li máme dvě možnosti. První je vytvoření obslužné rutiny události pro GridView s `RowDeleted` obslužné rutiny události, která určuje počet záznamů, které byly zobrazeny na stránce, který byl právě odstraněn. Pokud došlo pouze jeden záznam, pak záznam právě odstranit poslední byla a potřebujeme se sníží GridView s `PageIndex`. Samozřejmě pouze chceme aktualizovat `PageIndex` Pokud operace odstranění byl ve skutečnosti úspěšný, který lze určit podle zajistit, aby `e.Exception` vlastnost je `null`.

Tento přístup funguje, protože aktualizuje `PageIndex` po kroku 1, ale před kroku 2. V kroku 2, proto se vrátí příslušné sady záznamů. K tomu použijte kód takto:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample11.vb)]

Alternativní řešení, je vytvoření obslužné rutiny události pro ObjectDataSource s `RowDeleted` událostí a nastavit `AffectedRows` vlastnost na hodnotu 1. Po odstranění záznamu v kroku 1 (ale před znovu načítání dat v kroku 2), aktualizace GridView jeho `PageIndex` vlastnost Pokud operaci měla vliv na jeden nebo více řádků. Ale `AffectedRows` vlastnost není nastaven ObjectDataSource a proto je tento krok vynechán. Je možné mít tento krok provést ručně nastavit `AffectedRows` vlastnost po úspěšném dokončení operace odstranění. Můžete to provést pomocí kódu takto:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample12.vb)]

Kód pro obě tyto obslužné rutiny události lze nalézt v kódu třídu `EfficientPaging.aspx` příklad.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Porovnání výkonu výchozích a vlastních stránkování

Vzhledem k tomu, že vlastní stránkování pouze načte potřebné záznamů, zatímco výchozí stránkování vrátí *všechny* záznamy pro jednotlivé stránky zobrazení, se s vymazat, že je efektivnější než výchozí stránkování vlastní stránkování. Ale jenom kolik efektivnější je vlastní stránkování? Jaký způsob zvýšení výkonu může zobrazit přesun z výchozího stránkování na vlastní stránkování?

Bohužel se zde s žádné velikosti, která vyhoví všechny zodpovědět sem. Zvýšení výkonu závisí na počtu faktorů, nejvíce viditelného dva se počet záznamů stránkování prostřednictvím a zatížení umístit do databáze serveru a komunikační kanály mezi webovým serverem a serverem databáze. Pro malé tabulky s několika záznamy desítek může být nepatrné rozdíl výkonem. Pro rozsáhlé tabulky s tisíci na stovky tisíc řádků, ale výkonu rozdíl je čárkou nad vpravo.

Článek min, [vlastní stránkování v technologii ASP.NET 2.0 pomocí serveru SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), obsahuje některé testy výkonu byl spuštěn vykazovat rozdíly ve výkonu mezi tyto dvě techniky stránkování při procházení tabulky databáze s 50 000 záznamů. Tyto testy I ověřuje čas k provedení dotazu na úrovni serveru SQL (pomocí [SQL Profiler](https://msdn.microsoft.com/en-us/library/ms173757.aspx)) a na stránku ASP.NET pomocí [ASP.NET s trasování funkce](https://msdn.microsoft.com/en-US/library/y13fw6we.aspx). Mějte na paměti, že tyto testy měla spustit na můj vývoj pole s jedním uživatelem aktivní a proto jsou unscientific a není napodobovat vzorů zatížení typické webu. Bez ohledu na to výsledky znázorňují relativní rozdíly v čase spuštění pro výchozí a vlastní stránkování při práci s dostatečně velké objemy dat.


|  | **Střední Doba trvání (sekundy)** | **Čtení** |
| --- | --- | --- |
| **Výchozí stránkování SQL profileru** | 1.411 | 383 |
| **Vlastní stránkování SQL profileru** | 0.002 | 29 |
| **Výchozí stránkování ASP.NET trasování** | 2.379 | *NENÍ K DISPOZICI* |
| **Vlastní trasování ASP.NET stránkování** | 0.029 | *NENÍ K DISPOZICI* |


Jak vidíte, načítání konkrétní stránku dat vyžaduje 354 méně čtení v průměru a dokončit za zlomek času. Na stránce ASP.NET, vlastní stránky bylo možné vykreslit v blíží 1/100<sup>tý</sup> času trvalo při použití výchozí stránkování. V tématu [Moje článku](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) Další informace o těchto výsledků spolu s kódem a databáze můžete stáhnout na reprodukujte tyto testy ve svém vlastním prostředí.

## <a name="summary"></a>Souhrn

Výchozí stránkování je lze velmi snadno implementovat jenom zaškrtnutí políčka Povolit stránkování v dat webové ovládací prvek s inteligentní značky, ale tyto jednoduchost dodává za cenu výkonu. S výchozí stránkování, když si uživatel vyžádá jakoukoli stránku dat *všechny* záznamů se vrátí, i když se může zobrazit pouze jen nepatrnou část z nich. Boje proti tento nároky na výkon, nabízí ObjectDataSource alternativní stránkování vlastní možnost stránkování.

Při vlastní stránkování vylepšuje výchozí stránkování problémy s výkonem s načtením pouze záznamy, které musí být zobrazeny, ho s složitější implementovat vlastní stránkování. Nejdřív musí být napsané dotazu, který správně (a efektivně) přistupuje určitou podmnožinu požadované záznamy. To lze provést v mnoha různými způsoby; jeden jsme se zaměřili na v tomto kurzu se má používat systém SQL Server 2005 s novou `ROW_NUMBER()` funkce pro řazení výsledků a pak vrátit jenom na ty výsledků jehož hodnocení spadá do zadaného rozsahu adres. Kromě toho je potřeba přidat prostředky můžete určit celkový počet záznamů stránkování prostřednictvím. Po vytvoření těchto metod DAL a BLL, musíme také nakonfigurovat ObjectDataSource, aby mohla určit, kolik celkový počet záznamů jsou stránkování prostřednictvím a správně můžete předat hodnoty Start Index řádku a maximální počet řádků BLL.

Při implementaci vlastní stránkování vyžaduje několik kroků a není skoro stejně jednoduché jako výchozí stránkování, vlastní stránkování je nezbytné v případě stránkování prostřednictvím dostatečně velké objemy dat. Výsledky zkontrolován ukázal, vlastní stránkování můžete přenesen sekund z čas vykreslení stránky ASP.NET a zatížení na serveru databáze můžete zjednodušit pomocí jedné nebo více pořadí podle velikosti.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Předchozí](paging-and-sorting-report-data-vb.md)
[další](sorting-custom-paged-data-vb.md)
