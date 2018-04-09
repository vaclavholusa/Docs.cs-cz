---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: Vložení, aktualizace a odstranění dat pomocí SqlDataSource (C#) | Microsoft Docs
author: rick-anderson
description: V předchozích kurzy jsme zjistili, jak ovládacího prvku ObjectDataSource povolené pro vložení, aktualizaci a odstraňování dat. Ovládací prvek SqlDataSource podporuje t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 25dab0292aefa183a1abc2615a7ba8e7a512346d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>Vložení, aktualizace a odstranění dat pomocí SqlDataSource (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe) nebo [stáhnout PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> V předchozích kurzy jsme zjistili, jak ovládacího prvku ObjectDataSource povolené pro vložení, aktualizaci a odstraňování dat. Ovládací prvek SqlDataSource podporuje stejné operace, ale přístupu se liší a tento kurz ukazuje, jak nakonfigurovat SqlDataSource vložit, aktualizovat a odstranit data.


## <a name="introduction"></a>Úvod

Jak je popsáno v [přehled o vložení, aktualizace a odstranění](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)prvek GridView poskytuje integrované aktualizace a odstraňování funkce, zatímco ovládací prvky DetailsView a FormView zahrnují vkládání podporují spolu s úpravy a odstraňování funkce. Tyto možnosti úpravy dat lze připojit přímo do ovládacího prvku zdroje dat bez řádek kódu, která potřebuje k zapsání. [Přehled o vložení, aktualizace a odstranění](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) zkontrolován pomocí ObjectDataSource usnadňuje vložení, aktualizace a odstranění s ovládacími prvky GridView DetailsView a FormView. Případně SqlDataSource jde použít místo ObjectDataSource.

Odvolat, který pro podporu vkládání, aktualizaci a odstraňování s ObjectDataSource jsme potřebné k určení metody vrstvy objektu k vyvolání provést vložení, aktualizaci nebo odstranění akce. S SqlDataSource, potřebujeme zajistit `INSERT`, `UPDATE`, a `DELETE` SQL příkazy (nebo uložené procedury) k provedení. Jako ukážeme v tomto kurzu, tyto příkazy lze vytvořit ručně nebo můžete být automaticky generované průvodcem konfigurace zdroje dat s SqlDataSource.

> [!NOTE]
> Protože jsme jste již popsané vkládání, úpravy a odstraňování možnosti rutina GridView DetailsView a řídí FormView, v tomto kurzu se soustředí na konfiguraci ovládacího prvku SqlDataSource pro podporu těchto operací. Pokud potřebujete zdokonalit v implementaci těchto funkcí v rámci GridView DetailsView a FormView, vraťte se na kurzy úpravy, vkládání a odstraňování dat práci, počínaje [přehled o vložení, aktualizace a odstranění](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Krok 1: Určení`INSERT`,`UPDATE`, a`DELETE`příkazy

Jako jsme jste viděli v posledních dvou kurzy, k načtení dat z ovládacího prvku SqlDataSource, kterou je potřeba nastavit dvě vlastnosti:

1. `ConnectionString`, která určuje, co bude odeslán dotaz, databáze a
2. `SelectCommand`, která určuje název ad-hoc příkazu SQL nebo uloženou proceduru provést vrátí výsledky.

Pro `SelectCommand` hodnoty s parametry, parametr hodnoty jsou specifikované prostřednictvím SqlDataSource s `SelectParameters` kolekce a může obsahovat pevně definovaných hodnot zdroj hodnoty společných parametrů (pole řetězce dotazu, proměnné relace webové hodnoty řízení, a tak dále), nebo je možné přiřadit prostřednictvím kódu programu. Když SqlDataSource řízení s `Select()` metoda je volána prostřednictvím kódu programu nebo automaticky z dat ovládací prvek webu navázat připojení k databázi, hodnoty parametrů jsou přiřazeny k dotazu a příkaz je shuttled vypnout na databáze. Výsledky jsou pak vrácen jako datové sady nebo DataReader, v závislosti na hodnotě ovládacího prvku s `DataSourceMode` vlastnost.

Společně s výběr dat, ovládacího prvku SqlDataSource slouží k vložení, aktualizace a odstranění dat zadáním `INSERT`, `UPDATE`, a `DELETE` příkazů SQL mnohem stejným způsobem. Jednoduše přiřadit `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti `INSERT`, `UPDATE`, a `DELETE` příkazů SQL k provedení. Pokud se příkazy mít parametry (jak je nejvíce vždy se), zahrňte je do `InsertParameters`, `UpdateParameters`, a `DeleteParameters` kolekce.

Jednou `InsertCommand`, `UpdateCommand`, nebo `DeleteCommand` byla zadána hodnota, bude k dispozici možnost Povolit vložení, povolit úpravy nebo odstranění povolit v odpovídajících dat webové ovládací prvek s inteligentních značek. Pro znázornění je umožňují s trvat příklad z `Querying.aspx` stránky jsme vytvořili v [dotazování dat pomocí ovládacího prvku SqlDataSource](querying-data-with-the-sqldatasource-control-cs.md) kurzu a rozšířit tak, aby obsahovala odstranit možnosti.

Začněte otevřením `InsertUpdateDelete.aspx` a `Querying.aspx` stránky z `SqlDataSource` složky. Z Návrháře na `Querying.aspx` vyberte SqlDataSource a GridView z v prvním příkladu ( `ProductsDataSource` a `GridView1` ovládací prvky). Po výběru dvou ovládacích prvků, přejděte do nabídky Upravit a zvolte Kopírovat (nebo jenom stiskněte kombinaci kláves Ctrl + C). Potom přejděte na Návrhář `InsertUpdateDelete.aspx` a vkládání v ovládacích prvcích. Po dvou ovládacích prvků byl přesunut do `InsertUpdateDelete.aspx`, otestování stránku v prohlížeči. Měli byste vidět hodnoty `ProductID`, `ProductName`, a `UnitPrice` sloupce pro všechny záznamy v `Products` databázové tabulky.


[![Všechny produkty nejsou uvedeny, seřazené podle ID produktu](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**Obrázek 1**: všechny produkty nejsou uvedeny, seřazené podle `ProductID` ([Kliknutím zobrazit obrázek v plné velikosti](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Přidávání SqlDataSource s`DeleteCommand`a`DeleteParameters`vlastnosti

V tomto okamžiku máme SqlDataSource, která vrací všechny záznamy z jednoduše `Products` tabulka a rutina GridView, který vykreslí tato data. Naším cílem je v tomto příkladu umožňující uživateli odstranit produkty prostřednictvím GridView. K tomu je potřeba zadat hodnoty pro ovládací prvek SqlDataSource s `DeleteCommand` a `DeleteParameters` vlastnosti a pak nakonfigurujte GridView pro podporu odstranění.

`DeleteCommand` a `DeleteParameters` vlastnosti mohou být zadány v mnoha různými způsoby:

- Využijte deklarativní syntaxi
- V okně Vlastnosti v Návrháři
- Z zadejte vlastní příkaz SQL nebo uloženou proceduru obrazovky v průvodci Konfigurace zdroje dat
- Prostřednictvím tlačítko Upřesnit v zadat sloupce z tabulky obrazovky zobrazit v průvodci Konfigurace zdroje dat, která ve skutečnosti automaticky vygeneruje `DELETE` SQL příkazu a parametru kolekce používán `DeleteCommand` a `DeleteParameters` Vlastnosti

Podíváme, jak vám má automaticky `DELETE` příkaz vytvořili v kroku 2. Prozatím se umožní s použijte okno Vlastnosti v návrháři, i když Průvodce konfigurací zdroje dat nebo deklarativní syntaxe možnosti by stejně dobře fungovat.

Z Návrháře v `InsertUpdateDelete.aspx`, klikněte na `ProductsDataSource` SqlDataSource a pak otevřete okno vlastností (z nabídky Zobrazit zvolte Vlastnosti – okno nebo jednoduše stiskněte tlačítko F4). Vyberte vlastnost DeleteQuery, která se otevře sadu výpustky.


![Vyberte vlastnost DeleteQuery v okně Vlastnosti](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**Obrázek 2**: Vyberte vlastnost DeleteQuery v okně Vlastnosti


> [!NOTE]
> Nemá SqlDataSource nemá vlastnost DeleteQuery. Místo toho DeleteQuery je kombinací `DeleteCommand` a `DeleteParameters` vlastnosti a při zobrazení okna pomocí návrháře je pouze uvedené v okně Vlastnosti. Pokud hledáte v okně vlastností v zobrazení zdroje, najdete `DeleteCommand` vlastnost místo.


Kliknutím na symbol tří teček v DeleteQuery vlastnost, která má vyvolat Editor kolekce parametrů dialogové okno pole (viz obrázek 3). Z tohoto dialogového okna můžete zadat `DELETE` příkaz jazyka SQL a zadejte tyto parametry. Zadejte následující dotaz do `DELETE` příkaz textbox (buď ručně nebo pomocí Tvůrce dotazů, pokud dáváte přednost):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

Potom klikněte na tlačítko Aktualizovat parametry pro přidání `@ProductID` parametru do seznamu níže uvedených parametrů.


[![Vyberte vlastnost DeleteQuery v okně Vlastnosti](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**Obrázek 3**: Vyberte vlastnost DeleteQuery v okně Vlastnosti ([Kliknutím zobrazit obrázek v plné velikosti](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))


Proveďte *není* zadejte hodnotu tohoto parametru (ponechejte její parametr zdroje: žádná). Jakmile přidáme odstraňování podpory k GridView, GridView bude tuto hodnotu Automaticky doplnit parametr, pomocí hodnoty jeho `DataKeys` kolekce pro řádek označeného jehož tlačítko Odstranit.

> [!NOTE]
> Název parametru, který je používán `DELETE` dotazu *musí* být stejný jako název `DataKeyNames` hodnotu v GridView, DetailsView nebo FormView. To znamená, parametr do `DELETE` záměrně s názvem příkaz `@ProductID` (místo můžete, `@ID`), protože název sloupec primárního klíče v tabulce produkty (a proto je hodnota DataKeyNames v GridView) je `ProductID`.


Pokud název parametru a `DataKeyNames` hodnota nemá neshodují, GridView nelze přiřadit automaticky parametr s hodnotou od `DataKeys` kolekce.

Po zadání informace týkající se odstranění do dialogové okno Editor kolekce parametrů klikněte na tlačítko OK a přejděte do zobrazení zdroje a zkontrolujte výsledné deklarativní:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

Poznámka: přidání `DeleteCommand` vlastnost společně s `<DeleteParameters>` části a objekt parametr s názvem `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Konfigurace GridView pro odstranění

Pomocí `DeleteCommand` vlastnost přidána, rutina GridView s inteligentním nyní obsahuje možnost Povolit odstraňování. Pokračujte a zaškrtněte toto políčko. Jak je popsáno v [přehled o vložení, aktualizace a odstranění](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), to způsobí, že rutina GridView přidat CommandField s jeho `ShowDeleteButton` vlastnost nastavena na hodnotu `true`. Obrázek 4 ukazuje, když je prostřednictvím prohlížeče navštívené stránky je zahrnuté tlačítko Odstranit. Tato stránka se otestujte odstraněním některé produkty.


[![Každý řádek GridView teď obsahuje tlačítko pro odstranění](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**Obrázek 4**: každý řádek GridView teď obsahuje tlačítko Delete ([Kliknutím zobrazit obrázek v plné velikosti](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))


Po kliknutí na tlačítko Odstranit dochází k zpětné volání, přiřadí GridView `ProductID` parametr hodnotu z `DataKeys` hodnota kolekce pro řádek, jehož tlačítko Odstranit označeného a vyvolá SqlDataSource s `Delete()` metoda. Ovládací prvek SqlDataSource pak připojí k databázi a provede `DELETE` příkaz. GridView pak znovu připojí k SqlDataSource, získávání zpět a zobrazení aktuální sadu produktů (která již obsahuje záznam právě odstraněné).

> [!NOTE]
> Vzhledem k tomu, že používá GridView jeho `DataKeys` kolekce k naplnění parametry SqlDataSource ho s důležitých, GridView s `DataKeyNames` vlastnost nastavit na sloupce, který tvoří primární klíč a že SqlDataSource s `SelectCommand` vrátí Tyto sloupce. Kromě toho je důležité, aby parametr názvu SqlDataSource s s `DeleteCommand` je nastaven na `@ProductID`. Pokud `DataKeyNames` není nastavena vlastnost nebo parametr není s názvem `@ProductsID`, kliknutím na tlačítko Odstranit způsobí, že zpětné volání, ale získaných t ve skutečnosti odstranit všechny záznam.


Obrázek 5 graficky znázorňuje tato interakce. Odkazovat zpět [zkoumání události spojené s vložení, aktualizace a odstranění](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) kurz podrobnější diskuzi v řetězu událostí, které jsou přidružené k vkládání, aktualizace a odstranění z dat ovládací prvek webu.


![Kliknutím na tlačítko Odstranit v GridView vyvolá metodu Delete() s SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**Obrázek 5**: Kliknutím na tlačítko Odstranit v GridView vyvolá SqlDataSource s `Delete()` – metoda


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Krok 2: Automatické generování`INSERT`,`UPDATE`, a`DELETE`příkazy

Jako kroku 1 zkontrolují `INSERT`, `UPDATE`, a `DELETE` příkazy SQL lze zadat prostřednictvím okna vlastnosti nebo deklarativní syntaxe ovládacího prvku s. Tento přístup však vyžaduje, že jsme ručně zapsat příkazy SQL ručně, může být monotónní a k chybám. Naštěstí Průvodce konfigurace zdroje dat poskytuje možnost mít `INSERT`, `UPDATE`, a `DELETE` příkazy, které automaticky generuje při použití zadejte sloupců z tabulky obrazovky zobrazení.

Umožní s prozkoumat tuto možnost automatického generování. Přidání DetailsView do návrháře v `InsertUpdateDelete.aspx` a nastavit jeho `ID` vlastnost `ManageProducts`. V dalším kroku ze inteligentní značky DetailsView s rozhodnete vytvořit nový zdroj dat a vytvořit SqlDataSource, s názvem `ManageProductsDataSource`.


[![Vytvořit nový SqlDataSource s názvem ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**Obrázek 6**: vytvoření nové SqlDataSource s názvem `ManageProductsDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))


Z Průvodce konfigurace zdroje dat opt používat `NORTHWINDConnectionString` připojovací řetězec a klikněte na tlačítko Další. Z konfigurace obrazovky vyberte příkaz nechte zadat sloupce z tabulky nebo zobrazení přepínač vybrané a vyberte `Products` tabulku z rozevíracího seznamu. Vyberte `ProductID`, `ProductName`, `UnitPrice`, a `Discontinued` sloupce ze seznamu zaškrtávací políčko.


[![Pomocí tabulky produktů, vrátí ProductID, ProductName, UnitPrice a – starší formáty sloupců](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**Obrázek 7**: pomocí `Products` tabulky, vraťte se `ProductID`, `ProductName`, `UnitPrice`, a `Discontinued` sloupce ([Kliknutím zobrazit obrázek v plné velikosti](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))


K automatickému generování `INSERT`, `UPDATE`, a `DELETE` příkazy na základě vybrané tabulky a sloupce, klikněte na tlačítko Upřesnit a zkontrolujte generování `INSERT`, `UPDATE`, a `DELETE` políčko příkazy.


![Zkontrolujte příkazy Generovat INSERT, UPDATE a DELETE zaškrtávací políčko](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**Obrázek 8**: Zkontrolujte generování `INSERT`, `UPDATE`, a `DELETE` příkazy zaškrtávací políčko


Generování `INSERT`, `UPDATE`, a `DELETE` příkazy políčko bude pouze kontrolovatelné Pokud vybraná tabulka obsahuje primární klíč a sloupec primárního klíče (nebo sloupce) jsou zahrnuty v seznamu vrácených sloupců. Zaškrtávací políčko optimistickou metodu souběžného použití stane volitelný jednou generování `INSERT`, `UPDATE`, a `DELETE` bylo zaškrtnuto políčko příkazy, bude posílení `WHERE` klauzule v výsledná `UPDATE` a `DELETE` příkazy k poskytování řízení optimistickou metodu souběžného. Teď nechte nezaškrtnuté; tohoto zaškrtávacího políčka Podíváme optimistickou metodu souběžného pomocí ovládacího prvku SqlDataSource v dalším kurzu.

Po zkontrolování generování `INSERT`, `UPDATE`, a `DELETE` příkazy zaškrtávací políčko, kliknete na OK se vraťte na obrazovku Konfigurace příkazu Select a pak klikněte na tlačítko Další a pak dokončete průvodce Konfigurace zdroje dat. Po dokončení průvodce, přidá aplikace Visual Studio BoundFields DetailsView pro `ProductID`, `ProductName`, a `UnitPrice` sloupce a vlastnost CheckBoxField pro `Discontinued` sloupce. Ze DetailsView s inteligentní značky zaškrtnutí políčka Povolit stránkování tak, aby uživatele, kteří navštěvují tuto stránku krokovat produkty. Také vyčistit DetailsView s `Width` a `Height` vlastnosti.

Všimněte si, že má inteligentní značky možnosti Povolit vložení, povolit úpravy a Povolit odstranění, které jsou k dispozici. Důvodem je, že SqlDataSource obsahuje hodnoty pro jeho `InsertCommand`, `UpdateCommand`, a `DeleteCommand`, jak ukazuje následující deklarativní syntaxe:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

Všimněte si, jak má ovládací prvek SqlDataSource měla hodnoty, které jsou automaticky nastaví pro jeho `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti. V sadě sloupců v odkazuje `InsertCommand` a `UpdateCommand` vlastnosti jsou založené na těch, které v `SELECT` příkaz. To znamená místo *každých* produkty sloupec v `InsertCommand` a `UpdateCommand`, jsou pouze tyto sloupce zadané v `SelectCommand` (méně `ProductID`, což je vynechána, protože ho s [ `IDENTITY` sloupec](http://www.sqlteam.com/item.asp?ItemID=102), jehož hodnotu nelze změnit, když upravit, a který je automaticky přiřazen při vkládání). Kromě toho pro každý parametr `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti existují odpovídající parametry v `InsertParameters`, `UpdateParameters`, a `DeleteParameters` kolekce.

Pokud chcete zapnout funkce úpravy DetailsView s dat, zkontrolujte možnosti Povolit vložení, povolit úpravy a Povolit odstraňování v jeho inteligentních značek. Tento postup přidá CommandField s jeho `ShowInsertButton`, `ShowEditButton`, a `ShowDeleteButton` vlastnosti nastavit na `true`.

Navštívit stránku v prohlížeči a poznamenejte si úpravy, odstranění a nová tlačítka, které jsou součástí DetailsView. Kliknutím na tlačítko Upravit změní DetailsView do režimu úprav, která zobrazuje každou BoundField jejichž `ReadOnly` je nastavena na `false` (výchozí) jako textové pole a vlastnost CheckBoxField jako zaškrtávací políčko.


[![DetailsView s výchozí úpravy rozhraní](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**Obrázek 9**: DetailsView s výchozí rozhraní pro úpravy ([Kliknutím zobrazit obrázek v plné velikosti](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))


Podobně můžete odstranit aktuálně vybraný produkt nebo přidání nového produktu v systému. Vzhledem k tomu `InsertCommand` příkaz lze použít pouze se `ProductName`, `UnitPrice`, a `Discontinued` sloupce, ostatních sloupců mít buď `NULL` nebo jejich výchozí hodnotu přiřadila databázi po vložení. Stejně jako s ObjectDataSource, pokud `InsertCommand` chybí všechny databázové tabulky, sloupce, které nejsou zobrazeny t povolit `NULL` s a nemusíte t mít výchozí hodnotu, dojde k chybě SQL při pokusu o spuštění `INSERT` příkaz.

> [!NOTE]
> S DetailsView vkládání a úpravy rozhraní nemají žádné řazení přizpůsobení nebo ověření. K přidávání ovládacích prvků ověření nebo přizpůsobit rozhraní, musíte převést BoundFields TemplateFields. Odkazovat [přidání ověřovací ovládací prvky pro úpravy a vkládání rozhraní](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) a [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) kurzy pro další informace.


Navíc mějte na paměti, že pro aktualizaci a odstraňování, DetailsView používá aktuální produkt s `DataKey` hodnoty, který je přítomen pouze, pokud `DataKeyNames` nastavena vlastnost. Pokud úpravy nebo odstranění mít žádný vliv, ujistěte se, že `DataKeyNames` je nastavena.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Omezení automatického generování SQL příkazy

Od generovat `INSERT`, `UPDATE`, a `DELETE` příkazy možnost je dostupná, jenom když výběr sloupce z tabulky, pro složitější dotazy, budete muset napsat vlastní `INSERT`, `UPDATE`, a `DELETE` příkazy, jako jsme provedli v kroku 1. Běžně, SQL `SELECT` použít příkazy `JOIN` s tím, vrací data z jednoho nebo více vyhledávací tabulky pro zobrazení účely (např. znovu připojuje zpět `Categories` tabulky s `CategoryName` pole při zobrazení informací o produktu). Ve stejnou dobu, jsme chtít umožnit uživateli upravit, aktualizaci nebo vložení dat do základní tabulky (`Products`, v takovém případě).

Když `INSERT`, `UPDATE`, a `DELETE` příkazy lze zadat ručně, zvažte následující ušetřit čas špičky. Nejdřív nastavit SqlDataSource tak, aby zpět čte data pouze z `Products` tabulky. Použít Průvodce konfigurace zdroje dat s zadejte sloupce z tabulky nebo zobrazení obrazovky, takže můžete automaticky generovat `INSERT`, `UPDATE`, a `DELETE` příkazy. Po dokončení průvodce, nakonfigurujte SelectQuery v okně Vlastnosti rozhodnete (nebo, případně přejděte zpět na Průvodce konfigurací zdroje dat, ale použití zadejte vlastní příkaz SQL nebo uloženou proceduru možnost). Aktualizujte `SELECT` k začlenění `JOIN` syntaxe. Tento postup nabízí výhody ušetřit čas automaticky generované příkazů SQL a umožňuje více přizpůsobené `SELECT` příkaz.

Další omezení automaticky generování `INSERT`, `UPDATE`, a `DELETE` příkazy je, že ve sloupcích v `INSERT` a `UPDATE` příkazy jsou založené na sloupce vrácený `SELECT` příkaz. Budeme muset aktualizovat nebo vložit více nebo méně pole, ale. Například v příkladu z kroku 2, může být Chceme mít `UnitPrice` BoundField být jen pro čtení. V takovém případě ji by neměl se nezobrazí v `UpdateCommand`. Nebo může chceme nastavte hodnotu pole tabulky, který se nenachází v GridView. Například při přidávání nového záznamu může chceme `QuantityPerUnit` hodnota nastavena na úkolů.

Pokud tyto úpravy, je třeba, aby je ručně, buď prostřednictvím okna vlastnosti, zadejte vlastní příkaz SQL nebo uloženou proceduru možnost v průvodci nebo pomocí deklarativní syntaxe.

> [!NOTE]
> Při přidávání parametrů, které nemají odpovídající pole v datech webové ovládací prvek, mějte na paměti, který bude potřeba tyto hodnoty parametrů možné přiřadit hodnoty nějakým způsobem. Tyto hodnoty mohou být: pevně přímo v `InsertCommand` nebo `UpdateCommand`; mohou pocházet z z předem definovaných zdroje (řetězci dotazu, stav relace, webové ovládací prvky na stránce a tak dále); nebo lze přiřadit prostřednictvím kódu programu, jako jsme viděli v předchozím kurzu.


## <a name="summary"></a>Souhrn

V pořadí pro data, že webové ovládací prvky využívat jejich předdefinované vkládání, úpravy a odstraňování funkce musí ovládací prvek zdroje dat, které jsou vázány na nabízí takové funkce. Pro SqlDataSource, to znamená, že `INSERT`, `UPDATE`, a `DELETE` příkazů SQL musí být přiřazená k `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti. Tyto vlastnosti a odpovídající parametry kolekce, můžete přidat ručně nebo pomocí Průvodce konfigurace zdroje dat, generuje automaticky. V tomto kurzu jsme se zaměřili obě metody.

Jsme se zaměřili na optimistickou metodu souběžného pomocí ObjectDataSource v [implementace optimistickou metodu souběžného](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) kurzu. Ovládací prvek SqlDataSource taky poskytuje podporu optimistickou metodu souběžného. Jak jsme uvedli v kroku 2, při automatickém vytváření `INSERT`, `UPDATE`, a `DELETE` příkazy, Průvodce nabízí možnost optimistickou metodu souběžného použití. Jako ukážeme v dalším kurzu, pomocí SqlDataSource optimistickou metodu souběžného upraví `WHERE` klauzule v `UPDATE` a `DELETE` příkazy zajistit, že hodnoty ostatních sloupců nebyla některá t změnil od posledního data zobrazené na stránce.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [další](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
