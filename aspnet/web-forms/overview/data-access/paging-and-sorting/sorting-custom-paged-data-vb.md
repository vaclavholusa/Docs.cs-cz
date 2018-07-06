---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
title: Řazení s vlastním stránkováním dat (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V předchozím kurzu jsme zjistili, jak implementovat vlastní stránkování presentating data na webové stránce. V tomto kurzu jsme zjistit, jak rozšířit předchozí...
ms.author: aspnetcontent
ms.date: 08/15/2006
ms.assetid: 4823a186-caaf-4116-a318-c7ff4d955ddc
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 7a660f697676e20d8987af150b10fc6694ce7c57
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805311"
---
<a name="sorting-custom-paged-data-vb"></a>Řazení s vlastním stránkováním dat (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_VB.exe) nebo [stahovat PDF](sorting-custom-paged-data-vb/_static/datatutorial26vb1.pdf)

> V předchozím kurzu jsme zjistili, jak implementovat vlastní stránkování presentating data na webové stránce. V tomto kurzu jsme zjistit, jak rozšířit předchozí příklad zahrnující podporu pro řazení vlastní stránkování.


## <a name="introduction"></a>Úvod

Ve srovnání s výchozí stránkování, vlastní stránkování může zlepšit výkon stránkování prostřednictvím dat o několik řádů, provádění vlastní stránkování de facto stane Volba implementace stránkovacího při procházení velkých objemů dat po stránkách. Implementace vlastní stránkování se tak zapojí víc než implementovat výchozí stránkování, zejména v případě, že přidání řazení kombinaci. V tomto kurzu budete rozšíříme příklad od předchozí zahrnující podporu pro řazení *a* vlastní stránkování.

> [!NOTE]
> Protože v tomto kurzu navazuje na předchozí jeden před začátek využít ke zkopírování deklarativní syntaxe v rámci `<asp:Content>` element z předchozího kurzu s webové stránky (`EfficientPaging.aspx`) a vložte jej mezi `<asp:Content>` prvek `SortParameter.aspx` stránky. Vraťte se ke kroku 1 z [přidání validačních ovládacích prvků pro úpravy a vložení rozhraní](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) kurz podrobnější informace o replikaci do jiné funkce jednu stránku ASP.NET.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>Krok 1: Přezkoumání vlastní stránkování technika

Pro vlastní stránkování fungovalo správně, musíme implementovat některé techniky, které můžete efektivně vzít konkrétní podmnožinu záznamů zadané parametry Start Index řádku a maximální počet řádků. Existuje několik postupů, které slouží k dosažení tohoto cíle. V předchozím kurzu jsme se podívali na způsoby použití nového serveru Microsoft SQL Server 2005 s `ROW_NUMBER()` hodnocení funkce. Stručně řečeno `ROW_NUMBER()` hodnocení funkce přiřadí každý řádek vrácený dotaz, který je seřazené podle pořadí řazení zadané číslo řádku. Na příslušnou podmnožinu záznamů se potom získá tak, že vrací určitou část číslované výsledky. Následující dotaz ukazuje, jak používat tuto techniku ke vráceny produkty číslované 11 až 20 při hodnocení výsledky seřazené podle abecedy podle `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample1.sql)]

Tento postup funguje dobře pro stránkování pomocí konkrétní řazení (`ProductName` seřazená podle abecedy, v tomto případě), ale je potřeba upravit tak, aby zobrazit výsledky seřazené podle různých řadicí výraz dotazu. V ideálním případě by mohla být přepsána výše uvedeném dotazu určený parametrem `OVER` klauzuli, například takto:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample2.sql)]

Bohužel parametrizované `ORDER BY` klauzule nejsou povoleny. Místo toho musíte vytvořit uloženou proceduru, která přijímá `@sortExpression` vstupní parametr, ale používá jedno z následujících náhradních postupů:

- Psaní dotazů pevně zakódované pro jednotlivé výrazy řazení, které mohou být použity; potom použijte `IF/ELSE` příkazy jazyka T-SQL k určení dotazu, který chcete spustit.
- Použití `CASE` příkazu zadejte dynamické `ORDER BY` na základě výrazů `@sortExpressio` n vstupní parametr; viz slouží k části dynamicky řazení výsledků dotazu v [Power SQL `CASE` příkazy](http://www.4guysfromrolla.com/webtech/102704-1.shtml) Další informace.
- Vytvořit odpovídající dotazu jako řetězec v uložené proceduře a pak použijte [ `sp_executesql` systémové uložené procedury](https://msdn.microsoft.com/library/ms188001.aspx) ke spuštění dynamický dotaz.

Každá z těchto alternativních řešení obsahuje určité nevýhody. První možnost není jako udržovatelného jako další dvě jako vyžaduje, že vytvoříte dotaz pro každý možné řadicí výraz. Proto pokud později se rozhodnete přidat nové, seřaditelných pole do prvku GridView. je také potřeba vrátit zpět a aktualizovat uložené procedury. Druhý přístup má několik odlišností, které představují otázky výkonu při řazení podle sloupce jiné než řetězec databáze a také vykazuje stejné problémy udržovatelnosti jako první. A třetí volby, který používá dynamic SQL, představuje riziko útoků prostřednictvím injektáže SQL, pokud má útočník k spustit uloženou proceduru předání jako vstupní parametr hodnoty podle vlastního uvážení.

Když žádný z těchto postupů je ideálním řešením, myslím, že třetí možnost je nejlepší tři. S jejím použitím dynamic SQL nabízí úroveň flexibilitu, které další dvě tomu tak není. Kromě toho útok prostřednictvím injektáže SQL může zneužít pouze v případě útočník je možné spustit uloženou proceduru předání vstupních parametrů podle svého výběru. Protože DAL používá parametrizované dotazy, ADO.NET bude chránit tyto parametry, které se odesílají do databáze prostřednictvím architektury, což znamená, že zranitelnost útoku prostřednictvím injektáže SQL existuje pouze v případě, že útočník může přímo spustit uloženou proceduru.

Pro implementaci této funkce, vytvořte novou úložnou proceduru v databázi Northwind s názvem `GetProductsPagedAndSorted`. Tuto uloženou proceduru by měla přijímat tři vstupní parametry: `@sortExpression`, vstupní parametr typu `nvarchar(100`), která určuje, jak mají být řazeny výsledky a vloží přímo po `ORDER BY` text `OVER` klauzule; a `@startRowIndex` a `@maximumRows`, stejné dvě celočíselné vstupní parametry z `GetProductsPaged` uloženou proceduru vyšetřovány v předchozím kurzu. Vytvořte `GetProductsPagedAndSorted` uložené procedury pomocí následujícího skriptu:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample3.sql)]

Spustí uloženou proceduru tak zajistit, aby hodnotu `@sortExpression` nebyl zadán parametr. Pokud není nalezena, výsledky jsou seřazené podle `ProductID`. V dalším kroku je vytvořená dynamický dotaz SQL. Všimněte si, že zde dynamický dotaz SQL se mírně liší od naše předchozí dotazy, které se používá k načtení všech řádků z tabulky produktů. V předchozích příkladech můžeme získat jednotlivých kategorií produktů s přidružené s a dodavateli s názvy pomocí poddotazu. Toto rozhodnutí bylo zpátky [vytvoření vrstvy přístupu k datům](../introduction/creating-a-data-access-layer-vb.md) kurzu a bylo provedeno namísto použití `JOIN` s protože TableAdapter nelze automaticky vytvořit přidružené vložení, aktualizace a odstranění metody pro tento dotazy. `GetProductsPagedAndSorted` Uloženou proceduru, ale musí používat `JOIN` s výsledky provést řazení podle názvů kategorií nebo na dodavatele.

Tento dynamický dotaz se sestavil zřetězením části statické dotazu a `@sortExpression`, `@startRowIndex`, a `@maximumRows` parametry. Protože `@startRowIndex` a `@maximumRows` jsou celočíselné parametry, je potřeba je převést na nvarchars aby bylo možné správně zřetězení. Jakmile byl vytvořen tento dynamický dotaz SQL, je proveden prostřednictvím `sp_executesql`.

Za chvíli otestovat tuto uloženou proceduru s různými hodnotami parametru `@sortExpression`, `@startRowIndex`, a `@maximumRows` parametry. Z Průzkumníka serveru klikněte pravým tlačítkem na název uložené procedury a zvolte spustit. Tím se otevře dialogové okno Spustit uloženou proceduru, do kterého můžete zadat vstupní parametry (viz obrázek 1). Chcete-li seřadit výsledky podle názvu kategorie, použijte CategoryName pro `@sortExpression` hodnota parametru; chcete-li seřadit podle názvu společnosti dodavatele s, použijte CompanyName. Po zadání hodnot parametrů, klikněte na tlačítko OK. Výsledky se zobrazí v okně výstup. Obrázek 2 ukazuje výsledky při vrácení produktů seřazených 11 až 20 při řazení podle `UnitPrice` v sestupném pořadí.


![Vyzkoušejte si různé hodnoty pro vstupní parametry uložené procedury s tři](sorting-custom-paged-data-vb/_static/image1.png)

**Obrázek 1**: Vyzkoušejte jiné hodnoty pro vstupní parametry uložené procedury s tři


[![Uložená procedura s výsledky jsou zobrazeny v okně výstupu](sorting-custom-paged-data-vb/_static/image3.png)](sorting-custom-paged-data-vb/_static/image2.png)

**Obrázek 2**: The uložená procedura s výsledky jsou zobrazeny v okně výstupu ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-custom-paged-data-vb/_static/image4.png))


> [!NOTE]
> Při hodnocení výsledky podle zadaného `ORDER BY` sloupec `OVER` klauzule, SQL Server musí řazení výsledků. To je rychlá operace v případě clusterovaného indexu přes sloupců, které jsou právě výsledky seřazené podle nebo pokud je pokrytí indexu, ale může být dražší jinak. Chcete-li zvýšit výkon pro dotazy na dostatečně velký, zvažte přidání neclusterovaný index pro sloupec, podle kterého se výsledky jsou řazeny podle. Odkazovat na [hodnocení funkcí a výkonu v systému SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) další podrobnosti.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Krok 2: Rozšíření přístup k datům a vrstvy obchodní logiky

S `GetProductsPagedAndSorted` úložná procedura vytvořila naším dalším krokem je poskytují způsob provedení tuto úložnou proceduru prostřednictvím naší aplikace architektury. To má za následek přidání vrstvy DAL a BLL odpovídající metodu. Umožní s začněte tím, že přidání metody do vrstvy DAL. Otevřít `Northwind.xsd` typová, klikněte pravým tlačítkem na `ProductsTableAdapter`a v místní nabídce zvolte možnost přidat dotaz. Jako jsme to udělali v předchozím kurzu, chceme konfigurovat tato nová metoda DAL použít stávající úložnou proceduru - `GetProductsPagedAndSorted`, v tomto případě. Začněte tak, že má nové metody třídy TableAdapter používat stávající úložnou proceduru.


![Zvolit stávající úložnou proceduru](sorting-custom-paged-data-vb/_static/image5.png)

**Obrázek 3**: zvolit stávající úložnou proceduru


Uložené procedury k použití, vyberte `GetProductsPagedAndSorted` uloženou proceduru z rozevíracího seznamu na další obrazovce.


![Použít GetProductsPagedAndSorted uložené procedury](sorting-custom-paged-data-vb/_static/image6.png)

**Obrázek 4**: použijte GetProductsPagedAndSorted uložené procedury


Tuto uloženou proceduru vrátí sadu záznamů jako jeho výsledky tedy na další obrazovce znamenat, že vrátí tabulková data.


![Označuje, že bude procedura vracet tabulková Data](sorting-custom-paged-data-vb/_static/image7.png)

**Obrázek 5**: označuje, že bude procedura vracet tabulková Data


Nakonec vytvořte DAL metody, které použít obě výplně DataTable a vrátit objekt DataTable vzory, názvy metod `FillPagedAndSorted` a `GetProductsPagedAndSorted`v uvedeném pořadí.


![Zvolte názvy metod](sorting-custom-paged-data-vb/_static/image8.png)

**Obrázek 6**: Zvolte názvy metod


Nyní, který jsme ve rozšířené DAL, můžeme znovu připravený k zapnutí knihoven BLL. Otevřít `ProductsBLL` třídy soubor a přidejte novou metodu `GetProductsPagedAndSorted`. Tato metoda je potřeba přijmout tři vstupní parametry `sortExpression`, `startRowIndex`, a `maximumRows` a měla by volat jednoduše do vrstvy DAL s `GetProductsPagedAndSorted` metody takto:


[!code-vb[Main](sorting-custom-paged-data-vb/samples/sample4.vb)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Krok 3: Konfigurace v parametru SortExpression ObjectDataSource průchodu

S vylepšila DAL a BLL zahrnují metody, které využívají `GetProductsPagedAndSorted` uložené procedury, všechny zbývající okamžiku, kdy je ke konfiguraci ObjectDataSource v `SortParameter.aspx` stránku nové metody knihoven BLL a a zajistěte tak předání `SortExpression` na základě parametru sloupec, který uživatel požaduje seřadit výsledky podle.

Začněte změnou ObjectDataSource s `SelectMethod` z `GetProductsPaged` k `GetProductsPagedAndSorted`. To lze provést pomocí Průvodce konfigurace zdroje dat, v okně Vlastnosti nebo přímo pomocí deklarativní syntaxe. V dalším kroku potřeba zadat hodnotu pro prvek ObjectDataSource s [ `SortParameterName` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Pokud je tato vlastnost nastavena, a zajistěte tak předání GridView s pokusí ObjectDataSource `SortExpression` vlastnost `SelectMethod`. Zejména ObjectDataSource hledá vstupní parametr, jehož název je rovna hodnotě tohoto `SortParameterName` vlastnost. Od verze BLL s `GetProductsPagedAndSorted` metoda má vstupní parametr řazení výraz s názvem `sortExpression`, nastavte ObjectDataSource s `SortExpression` vlastnost sortExpression.

Po provedení těchto dvou změn, deklarativní syntaxe prvku ObjectDataSource s by měl vypadat nějak takto:


[!code-aspx[Main](sorting-custom-paged-data-vb/samples/sample5.aspx)]

> [!NOTE]
> Jako v předchozím kurzu, ujistěte se, že nemá prvku ObjectDataSource *není* zahrnout sortExpression, startRowIndex nebo maximumRows vstupní parametry svou kolekci prvků vlastnosti SelectParameters obsahovat.


Pokud chcete povolit řazení v prvku GridView, stačí zaškrtnout políčko Povolit řazení v prvku GridView s inteligentní značky, které nastaví prvek GridView s `AllowSorting` vlastnost `true` a způsobí text záhlaví pro každý sloupec mohl být vykreslen jako odkazem (LinkButton). Když koncový uživatel klikne na jednu z hlaviček LinkButtons, vyplývá zpětné volání a probíhají následující kroky:

1. Aktualizace prvku GridView jeho [ `SortExpression` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) na hodnotu `SortExpression` pole, jehož záhlaví propojení došlo ke kliknutí na
2. Prvku ObjectDataSource vyvolá BLL s `GetProductsPagedAndSorted` metodu GridView s `SortExpression` vlastnost jako hodnotu pro metodu s `sortExpression` vstupní parametr (společně s odpovídající `startRowIndex` a `maximumRows` vstupních parametrů)
3. Vyvolá s vrstvou DAL BLL `GetProductsPagedAndSorted` – metoda
4. Spustí DAL `GetProductsPagedAndSorted` předáním hodnoty uložené procedury, v `@sortExpression` parametr (spolu s `@startRowIndex` a `@maximumRows` vstupních parametrů)
5. Uložená procedura vrátí příslušnou podmnožinu dat do knihoven BLL, který vrátí ji do prvku ObjectDataSource; Tato data je pak vázán na prvku GridView, vykreslí do kódu HTML a odesílat koncového uživatele

Obrázek 7 znázorňuje první stránka výsledků při řazení podle `UnitPrice` ve vzestupném pořadí.


[![Výsledky jsou seřazené podle pole UnitPrice](sorting-custom-paged-data-vb/_static/image10.png)](sorting-custom-paged-data-vb/_static/image9.png)

**Obrázek 7**: The výsledky jsou seřazené podle pole UnitPrice ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-custom-paged-data-vb/_static/image11.png))


Když aktuální implementace může správně seřadit výsledky podle názvu produktu, název kategorie, množství jednotky a cena za jednotku, pokus o řazení výsledků dodavatelem název výsledky v výjimku při běhu (viz obrázek 8).


![Pokus o výsledky seřaďte podle dodavatele výsledkem následující výjimka za běhu](sorting-custom-paged-data-vb/_static/image12.png)

**Obrázek 8**: Pokus o výsledky seřaďte podle dodavatele výsledkem následující výjimka za běhu


Touto výjimkou způsobeno `SortExpression` z ovládacího prvku GridView s `SupplierName` nastavena vlastnost BoundField `SupplierName`. Ale název s od dodavatele v `Suppliers` tabulka ve skutečnosti se nazývá `CompanyName` jsme s aliasem tento název sloupce jako `SupplierName`. Ale `OVER` klauzule používané `ROW_NUMBER()` funkce nelze použít alias a musí používat skutečný název sloupce. Proto se změnit `SupplierName` Vlastnost BoundField s `SortExpression` z dodavatel na CompanyName (viz obrázek 9). Jak ukazuje obrázek 10 po této změně mohou být řazeny výsledky dodavatelem.


![Změnit vlastnosti BoundField Dodavatel s SortExpression CompanyName](sorting-custom-paged-data-vb/_static/image13.png)

**Obrázek 9**: Změna SortExpression s Vlastnost BoundField dodavatel CompanyName


[![Můžete teď být řazeny výsledky podle dodavatele](sorting-custom-paged-data-vb/_static/image15.png)](sorting-custom-paged-data-vb/_static/image14.png)

**Obrázek 10**: The výsledky můžete teď být řazeny podle dodavatele ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-custom-paged-data-vb/_static/image16.png))


## <a name="summary"></a>Souhrn

Vlastní implementaci stránkování, které jsme se zaměřili na v předchozím kurzu vyžaduje, aby byl specifikován pořadí, ve kterém byly výsledky který se má seřadit v době návrhu. Stručně řečeno to znamená, že vlastní implementaci stránkování, která jsme implementovali může Ne, ve stejnou dobu, zadejte možnosti řazení. V tomto kurzu jsme toto omezení overcame rozšířením uloženou proceduru z první zahrnout `@sortExpression` vstupní parametr, který by mohl být řazeny výsledky.

Po vytvoření to uloženou proceduru včetně vytváření nových metod v knihoven BLL a DAL, jsme byli schopni implementovat prvku GridView, která nabízí i řazení a vlastní stránkování prvku ObjectDataSource a zajistěte tak předání GridView s aktuální konfigurací `SortExpression` vlastnost BLL `SelectMethod`.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Santos Carlose. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](efficiently-paging-through-large-amounts-of-data-vb.md)
> [další](creating-a-customized-sorting-user-interface-vb.md)
