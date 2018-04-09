---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
title: Řazení vlastní stránkovaného dat (VB) | Microsoft Docs
author: rick-anderson
description: V předchozích kurzu jsme zjistili, jak implementovat vlastní stránkování při presentating data na webové stránce. V tomto kurzu jsme zjistit, jak rozšířit předchozím...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 4823a186-caaf-4116-a318-c7ff4d955ddc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
msc.type: authoredcontent
ms.openlocfilehash: e144b434bd759c4253065e365b1337a3eca82fa7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="sorting-custom-paged-data-vb"></a>Řazení vlastní stránkovaného dat (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_VB.exe) nebo [stáhnout PDF](sorting-custom-paged-data-vb/_static/datatutorial26vb1.pdf)

> V předchozích kurzu jsme zjistili, jak implementovat vlastní stránkování při presentating data na webové stránce. V tomto kurzu jsme naleznete v části předchozí příklad rozšířit na zahrnují podporu pro řazení vlastní stránkování.


## <a name="introduction"></a>Úvod

Ve srovnání s výchozí stránkování, vlastní stránkování lze vylepšit výkon stránkování prostřednictvím data několik pořadí podle velikosti, provedení vlastní stránkování fakticky Volba implementace stránkování při procházení velkých objemů dat. Implementace vlastních stránkování se tak zapojí víc než implementaci stránkování výchozí, ale, zejména v případě, že přidání řazení s různými. V tomto kurzu budete rozšiřujeme příklad od předchozí verze na zahrnují podporu pro řazení *a* vlastní stránkování.

> [!NOTE]
> Vzhledem k tomu, že v tomto kurzu staví na předchozí jeden před začátku pozorně zkopírujte deklarativní syntaxi v rámci `<asp:Content>` element z předchozí webovou stránku s kurzu (`EfficientPaging.aspx`) a vložte jej mezi `<asp:Content>` element v `SortParameter.aspx` stránky. Odkazuje zpět na krok 1 [přidání ověřovací ovládací prvky pro úpravy a vkládání rozhraní](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) kurz podrobnější diskuzi na replikace funkce jednu stránku ASP.NET do jiného.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>Krok 1: Přezkoumání vlastní technika stránkování

Pro vlastní stránkování fungovalo správně, musíme implementovat některé technik, které můžete efektivně získat pouze konkrétní podmnožinu záznamů zadané parametry Start Index řádku a maximální počet řádků. Existuje několik technik, které lze použít k dosažení tohoto cíle. V předchozím kurzu jsme se podívali na způsoby použití nového systému Microsoft SQL Server 2005 s `ROW_NUMBER()` řazení funkce. Stručně řečeno `ROW_NUMBER()` řazení funkce přiřadí každý řádek vrácený dotaz, který je seřazeny podle pořadí řazení zadané číslo řádku. Odpovídající podmnožinu záznamů je pak získat vrácením určitého oddílu číslem výsledků. Následující dotaz znázorňuje, jak použít tento postup, který vrátí těchto produktů číslované 11 až 20 při řazení výsledků v abecedním pořadí podle `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample1.sql)]

Tento postup funguje dobře pro stránkování, pomocí konkrétní řazení (`ProductName` řadí abecedně, v takovém případě), ale dotazu je potřeba upravit tak, aby zobrazit výsledky seřazené podle výraz různé řazení. V ideálním případě by mohla být přepsána výše uvedeném dotazu použít parametr v `OVER` klauzule, například takto:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample2.sql)]

Bohužel parametry `ORDER BY` klauzule nejsou povoleny. Místo toho musíte vytvořit uložené procedury, která přijímá `@sortExpression` vstupní parametr, ale používá jeden z následujících řešení:

- Zápis dotazů pevně pro jednotlivé výrazy řazení, které se dají použít; poté použijte `IF/ELSE` příkazů T-SQL k určení dotazu, který chcete spustit.
- Použití `CASE` příkaz zajistit dynamické `ORDER BY` výrazy na základě `@sortExpressio` n vstupní parametr; viz používá oddílu dynamicky řazení výsledků dotazu v [Power SQL `CASE` příkazy](http://www.4guysfromrolla.com/webtech/102704-1.shtml) Další informace.
- Vytvořit odpovídající dotazu jako řetězec v uložené proceduře a potom pomocí [ `sp_executesql` systémové uložené procedury](https://msdn.microsoft.com/library/ms188001.aspx) provést dynamické dotaz.

Každý z těchto řešení má některé nevýhody. První možnost není jako udržovatelný jako další dvě jako vyžaduje, že vytvoříte dotaz pro každý výraz možné řazení. Proto pokud později se rozhodnete přidat nový, řazení pole do GridView také musíte přejít zpět a aktualizovat uložené procedury. Druhý přístup má některé odlišnosti, které zavést otázky výkonu při řazení podle sloupce jiné než řetězec databáze a také vykazuje stejný problémy udržovatelnosti jako první. A třetí volby, která používá dynamický SQL, představuje riziko pro útok prostřednictvím injektáže SQL, pokud útočník je možné spustit uloženou proceduru předávání v vstupní parametr hodnoty podle vlastního uvážení.

Žádný z těchto postupů je ideální, je pravděpodobné, že třetí možnost je nejvhodnější ze tří. S jeho použití dynamické SQL nabízí úroveň flexibilitu, které další dvě nepodporují. Kromě toho útok prostřednictvím injektáže SQL může zneužít pouze v případě útočník je možné spustit uloženou proceduru předávání vstupní parametry podle svého výběru. Vzhledem k tomu, že DAL používá parametrizované dotazy, ADO.NET bude chránit tyto parametry, které se odesílají do databáze prostřednictvím architektury, znamená SQL zranitelnost útoku pouze existuje pokud útočník můžete přímo spustit uloženou proceduru.

Pokud chcete tuto funkci implementovat, vytvořit nové uloženou proceduru v databázi Northwind s názvem `GetProductsPagedAndSorted`. Tuto uloženou proceduru musí přijmout tři vstupní parametry: `@sortExpression`, vstupní parametr typu `nvarchar(100`) určující, jak by měly být seřazeny výsledky a je vložit přímo po `ORDER BY` textu v `OVER` klauzule; a `@startRowIndex` a `@maximumRows`, stejné vstupní parametry dva celé číslo z `GetProductsPaged` uložené procedury zkontrolován v předchozím kurzu. Vytvořte `GetProductsPagedAndSorted` uložené procedury pomocí následující skript:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample3.sql)]

Uložená procedura se spustí kontrolu, hodnotu `@sortExpression` byl zadán parametr. Pokud není nalezena, výsledky jsou seřazeny podle `ProductID`. Dále je vytvořený dynamické dotazu SQL. Všimněte si, že zde dynamické dotazu SQL se mírně liší od našich předchozí dotazy, používá se k načtení všechny řádky z tabulky produktů. V předchozích příkladech můžeme získat každý přidružené kategorie produktů s s názvy s a dodavatele pomocí poddotazu. Toto rozhodnutí došlo základě [vytváření Data Access Layer](../introduction/creating-a-data-access-layer-vb.md) kurzu a bylo provedeno místo pomocí `JOIN` s vzhledem k tomu, že TableAdapter nelze automaticky vytvořit přidružené vložit, aktualizovat a odstraňovat metody pro například dotazy. `GetProductsPagedAndSorted` Uložené procedury, ale musí používat `JOIN` s pro výsledky podle názvů kategorie nebo dodavatele.

Tento dotaz dynamické je vytvářet zřetězením části statické dotazu a `@sortExpression`, `@startRowIndex`, a `@maximumRows` parametry. Vzhledem k tomu `@startRowIndex` a `@maximumRows` jsou parametry celé číslo, je nutné převést do nvarchars Chcete-li být zřetězen správně. Po této dynamické dotazu SQL byla vytvořená, je proveden prostřednictvím `sp_executesql`.

Za chvíli otestovat tuto uloženou proceduru s různými hodnotami `@sortExpression`, `@startRowIndex`, a `@maximumRows` parametry. Z Průzkumníka serveru klikněte pravým tlačítkem na název uložené procedury a zvolte spustit. Tím se otevře dialogové okno Spustit uloženou proceduru, do kterého můžete zadat vstupní parametry (viz obrázek 1). Chcete-li seřadit výsledky podle název kategorie, použijte CategoryName pro `@sortExpression` hodnota parametru; seřadit podle dodavatele s název společnosti, použijte NázevSpolečnosti. Po zadání hodnot parametrů, klikněte na tlačítko OK. Výsledky jsou zobrazeny v okně výstupu. Obrázek 2 ukazuje výsledky při vrácení produkty seřazeny 11 až 20 při řazení podle `UnitPrice` v sestupném pořadí.


![Vyzkoušejte různé hodnoty pro uloženou proceduru s tři vstupní parametry](sorting-custom-paged-data-vb/_static/image1.png)

**Obrázek 1**: Zkuste různé hodnoty pro uloženou proceduru s tři vstupní parametry


[![Uložená procedura s výsledky se zobrazí v okně výstupu](sorting-custom-paged-data-vb/_static/image3.png)](sorting-custom-paged-data-vb/_static/image2.png)

**Obrázek 2**: uloženou proceduru s výsledky se zobrazí v okně výstupu ([Kliknutím zobrazit obrázek v plné velikosti](sorting-custom-paged-data-vb/_static/image4.png))


> [!NOTE]
> Při řazení výsledků k zadanému `ORDER BY` sloupec v `OVER` klauzule, SQL Server musí řazení výsledků. To je rychlý operace v případě clusterovaného indexu přes sloupcům, které jsou právě výsledky seřazené podle nebo pokud je zahrnut indexu, ale může být dražší jinak. Chcete-li zlepšit výkon dotazů dostatečně velký, zvažte přidání neclusterovaný index pro sloupec, podle kterého jsou výsledky seřazené podle. Odkazovat na [řazení funkcí a výkonu v systému SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) další podrobnosti.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Krok 2: Rozšířit přístup k datům a vrstvy obchodní logiky

Pomocí `GetProductsPagedAndSorted` vytvořit uložené procedury, naše dalším krokem je použití prostředků ke spuštění této uložené procedury prostřednictvím Naše architektura aplikace. To má za následek přidání odpovídající metody DAL i BLL. Umožní s začněte tím, že přidání metody do vrstvy DAL. Otevřete `Northwind.xsd` zadané v datové sadě klikněte pravým tlačítkem na `ProductsTableAdapter`a zvolte možnost Přidat dotazu v místní nabídce. Jako jsme to udělali v předchozím kurzu, chceme konfigurovat tato nová metoda DAL pro použití existující uložené procedury - `GetProductsPagedAndSorted`, v tomto případě. Začít indikující, že chcete pomocí nové metody TableAdapter pro použití existující uložené procedury.


![Pokud se rozhodnete použít existující uložené procedury](sorting-custom-paged-data-vb/_static/image5.png)

**Obrázek 3**: rozhodnete použít existující uložené procedury


Uložená procedura použít, vyberte `GetProductsPagedAndSorted` uložené procedury z rozevíracího seznamu na další obrazovce.


![Použít GetProductsPagedAndSorted uložené procedury](sorting-custom-paged-data-vb/_static/image6.png)

**Obrázek 4**: použití GetProductsPagedAndSorted uložené procedury


Tuto uloženou proceduru vrací sadu záznamů, jako své výsledky tedy na další obrazovce znamenat, že vrací tabulková data.


![Označuje, že uložené procedury vrátí tabulková Data](sorting-custom-paged-data-vb/_static/image7.png)

**Obrázek 5**: znamenat, že uložené procedury vrátí tabulková Data


Nakonec vytvořte DAL metody, které používají oba výplně DataTable a vraťte DataTable vzory, pojmenování metody `FillPagedAndSorted` a `GetProductsPagedAndSorted`, v uvedeném pořadí.


![Zvolte názvy metod](sorting-custom-paged-data-vb/_static/image8.png)

**Obrázek 6**: Zvolte názvy metod


Nyní který jsme sunout rozšířené vrstvy DAL jsme re připravené obrátit se BLL. Otevřete `ProductsBLL` třídy souboru a přidejte nové metody, `GetProductsPagedAndSorted`. Tato metoda je potřeba přijmout tři vstupní parametry `sortExpression`, `startRowIndex`, a `maximumRows` a měla by volat jednoduše do DAL s `GetProductsPagedAndSorted` metoda, například takto:


[!code-vb[Main](sorting-custom-paged-data-vb/samples/sample4.vb)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Krok 3: Konfigurace v parametru SortExpression ObjectDataSource průchodu

S rozšířen DAL a BLL zahrnují metody, které využívají `GetProductsPagedAndSorted` uložené procedury, všechny možnosti, které zůstává je konfigurace ObjectDataSource v `SortParameter.aspx` stránky lze pomocí této nové metody BLL a předávat `SortExpression` na základě parametr sloupec, který uživatel požádal o výsledky seřaďte podle.

Začněte tím, že změna ObjectDataSource s `SelectMethod` z `GetProductsPaged` k `GetProductsPagedAndSorted`. To lze provést pomocí Průvodce konfigurace zdroje dat, v okně Vlastnosti nebo přímo pomocí deklarativní syntaxe. Dále je potřeba zadat hodnotu pro ObjectDataSource s [ `SortParameterName` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Pokud je tato vlastnost nastavena, ObjectDataSource pokusí předat GridView s `SortExpression` vlastnost, která má `SelectMethod`. Konkrétně ObjectDataSource hledá vstupní parametr, jehož název je rovna hodnotě `SortParameterName` vlastnost. Od BLL s `GetProductsPagedAndSorted` metoda má řazení výraz vstupní parametr s názvem `sortExpression`, nastavit ObjectDataSource s `SortExpression` vlastnost sortExpression.

Po provedení těchto dvou změn, deklarativní syntaxi s ObjectDataSource by měl vypadat takto:


[!code-aspx[Main](sorting-custom-paged-data-vb/samples/sample5.aspx)]

> [!NOTE]
> Protože předchozí kurzu, ujistěte se, že ObjectDataSource nemá *není* zahrnují sortExpression, startRowIndex nebo maximumRows vstupní parametry v kolekci jeho prvků vlastnosti SelectParameters obsahovat.


Pokud chcete povolit řazení v GridView, stačí zaškrtnout políčko Povolit řazení v GridView s inteligentní značky, která nastavuje hodnoty GridView s `AllowSorting` vlastnost `true` a způsobit tak text záhlaví pro každý sloupec bude vykresleno jako LinkButton. Když koncový uživatel klikne na jedna z hlaviček LinkButtons, vyplývá zpětné volání a transpire následující kroky:

1. Rutina GridView aktualizace jeho [ `SortExpression` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) na hodnotu `SortExpression` pole, jehož propojení záhlaví označeného
2. ObjectDataSource vyvolá BLL s `GetProductsPagedAndSorted` metody předávání v GridView s `SortExpression` vlastnost jako hodnota pro metodu s `sortExpression` vstupní parametr (spolu s odpovídající `startRowIndex` a `maximumRows` vstupních parametrů)
3. Vyvolá s vrstvou DAL BLL `GetProductsPagedAndSorted` – metoda
4. DAL provede `GetProductsPagedAndSorted` uložené procedury, předávání v `@sortExpression` parametr (spolu s `@startRowIndex` a `@maximumRows` vstupních parametrů)
5. Uložená procedura vrátí odpovídající podmnožinu dat do BLL, který vrátí ji do ObjectDataSource; Tato data pak vázána na GridView, vykresluje do kódu HTML a posílá koncového uživatele

Obrázek 7 znázorňuje první stránky s výsledky při řazení podle `UnitPrice` ve vzestupném pořadí.


[![Výsledky jsou seřazeny podle pole UnitPrice](sorting-custom-paged-data-vb/_static/image10.png)](sorting-custom-paged-data-vb/_static/image9.png)

**Obrázek 7**: výsledky jsou seřazené podle pole UnitPrice ([Kliknutím zobrazit obrázek v plné velikosti](sorting-custom-paged-data-vb/_static/image11.png))


Při aktuální implementace můžete správně výsledky seřaďte podle názvu produktu, název kategorie, množství jednotce a jednotkové ceny, pokus o řazení výsledků Dodavatel název výsledky v výjimku modulu runtime (viz obrázek 8).


![Probíhá pokus o výsledky seřaďte podle dodavatele výsledky v následující výjimku modulu Runtime](sorting-custom-paged-data-vb/_static/image12.png)

**Obrázek 8**: Probíhá pokus o výsledky seřaďte podle dodavatele výsledky v následující výjimku modulu Runtime


K této výjimce, protože `SortExpression` souboru GridView s `SupplierName` BoundField je nastaven na `SupplierName`. Ale název dodavatele s `Suppliers` tabulky je ve skutečnosti volána `CompanyName` intenzivně jsme alias tento název sloupce jako `SupplierName`. Ale `OVER` klauzule používané `ROW_NUMBER()` funkce nelze používat s aliasem a musí používat skutečný název sloupce. Proto změnit `SupplierName` BoundField s `SortExpression` z dodavatel pro NázevSpolečnosti (viz obrázek 9). Jak ukazuje obrázek 10, po této změně výsledků lze seřadit podle dodavatele.


![Změňte SortExpression s dodavatel BoundField NázevSpolečnosti](sorting-custom-paged-data-vb/_static/image13.png)

**Obrázek 9**: Změňte SortExpression s dodavatel BoundField NázevSpolečnosti


[![Výsledky lze nyní seřadit podle dodavatele](sorting-custom-paged-data-vb/_static/image15.png)](sorting-custom-paged-data-vb/_static/image14.png)

**Obrázek 10**: výsledků lze nyní seřadit podle dodavatele ([Kliknutím zobrazit obrázek v plné velikosti](sorting-custom-paged-data-vb/_static/image16.png))


## <a name="summary"></a>Souhrn

Vlastní implementace stránkování, které jsme se zaměřili v předchozím kurzu vyžaduje, aby byl specifikován pořadí, ve kterém byly výsledky který se má seřadit, v době návrhu. Stručně řečeno vynutila si, že vlastní implementaci stránkování, jsme implementováno může není, ve stejnou dobu, poskytnutí možnosti řazení. V tomto kurzu jsme overcame toto omezení rozšířením uložené procedury od prvního zahrnout `@sortExpression` vstupní parametr, podle kterého může seřadit výsledky.

Po vytvoření to uložený postup a vytváření nových metod v DAL a BLL, se nám implementovat GridView, který nabízí i řazení a vlastní stránkování ObjectDataSource předávat GridView s aktuální konfigurací `SortExpression` vlastnost, která má BLL `SelectMethod`.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Carlos Santos. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](efficiently-paging-through-large-amounts-of-data-vb.md)
> [další](creating-a-customized-sorting-user-interface-vb.md)
