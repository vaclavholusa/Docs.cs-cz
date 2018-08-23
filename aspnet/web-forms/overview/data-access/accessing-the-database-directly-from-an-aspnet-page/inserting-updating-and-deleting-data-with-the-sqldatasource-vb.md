---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
title: Vkládání, aktualizaci a odstraňování dat ovládacím prvkem SqlDataSource (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V předchozích kurzech jsme zjistili, jak ovládací prvek ObjectDataSource povolené pro vkládání, aktualizaci a odstraňování dat. Ovládacím prvkem SqlDataSource podporuje t...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: 9673bef3-892c-45ba-a7d8-0da3d6f48ec5
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 3124d53bad0040938c6a1090971ceecdf8c92333
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756426"
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-vb"></a>Vkládání, aktualizaci a odstraňování dat ovládacím prvkem SqlDataSource (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_VB.exe) nebo [stahovat PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/datatutorial49vb1.pdf)

> V předchozích kurzech jsme zjistili, jak ovládací prvek ObjectDataSource povolené pro vkládání, aktualizaci a odstraňování dat. Ovládacím prvkem SqlDataSource podporuje stejné operace, ale tento přístup se liší a tento kurz ukazuje, jak nakonfigurovat ve třídě SqlDataSource vložit, aktualizovat a odstraňovat data.


## <a name="introduction"></a>Úvod

Jak je popsáno v [přehled o vložení, aktualizaci a odstraňování](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), ovládací prvek GridView poskytuje integrované aktualizace a odstranění funkce, zatímco obsahovat ovládací prvky prvku DetailsView a FormView vkládání podporu spolu s úpravy a odstranění funkce. Možnosti úprav těchto dat může být připojeno přímo do správy zdrojového kódu bez jediného řádku kódu by bylo potřeba zapsat data. [Přehled o vložení, aktualizaci a odstraňování](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) prozkoumat pomocí ObjectDataSource usnadňuje vkládání, aktualizaci a odstraňování s ovládacími prvky GridView DetailsView a FormView. Alternativně je možné ve třídě SqlDataSource místo ObjectDataSource.

Vzpomeňte si, že pro podporu vkládání, aktualizaci a odstraňování ovládacím prvkem ObjectDataSource jsme potřeby určují metody vrstvy objektu k vyvolání provést vložení, aktualizace nebo odstranění akce. S ovládacím prvkem SqlDataSource potřebujeme pro poskytování `INSERT`, `UPDATE`, a `DELETE` SQL příkazy (nebo úložné procedury) ke spuštění. Jak uvidíme v tomto kurzu, tyto příkazy lze vytvořit ručně nebo můžete být automaticky generované průvodcem knihovnou SqlDataSource s zdroj dat nakonfigurovat.

> [!NOTE]
> Protože jsme ve již probírali vložení, úpravy a odstranění možnosti prvku GridView, DetailsView a řídí FormView, tento kurz se zaměřuje na konfiguraci ovládacím prvkem SqlDataSource pro podporu těchto operací. Pokud se chcete zdokonalit v implementaci těchto funkcí v rámci ovládacího prvku GridView, DetailsView a FormView, vraťte se na kurzy úpravy, vložení a odstranění dat, počínaje [přehled o vložení, aktualizaci a odstraňování](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Krok 1: Určení`INSERT`,`UPDATE`, a`DELETE`příkazy

Jako jsme viděli v posledních dvou kurzy k načtení dat z SqlDataSource ovládací prvek, který potřebujeme ve nastavte dvě vlastnosti:

1. `ConnectionString`, která určuje, co bude odeslán dotaz, databáze a
2. `SelectCommand`, která určuje ad-hoc příkaz SQL nebo název uložené procedury k provedení pro vrácení výsledků.

Pro `SelectCommand` hodnoty parametrů, parametr hodnoty jsou specifikované prostřednictvím SqlDataSource s `SelectParameters` kolekci a může obsahovat pevně definovaných hodnot zdrojové hodnoty společných parametrů (pole řetězce dotazu, proměnné relace webové hodnoty ovládacího prvku, a tak dále), nebo je možné přiřadit prostřednictvím kódu programu. Když ve třídě SqlDataSource řídit s `Select()` metoda vyvolat prostřednictvím kódu programu nebo automaticky pomocí dat webový ovládací prvek navázání připojení k databázi, hodnoty parametrů jsou přiřazeny k dotazu a je příkaz shuttled vypnout na databáze. Výsledky jsou vráceny jako datovou sadu nebo DataReader, v závislosti na hodnotě ovládacího prvku s `DataSourceMode` vlastnost.

Spolu s výběr dat ovládacím prvkem SqlDataSource slouží k vložení, aktualizace a odstranění dat zadáním `INSERT`, `UPDATE`, a `DELETE` příkazy SQL v podstatě stejným způsobem. Jednoduše přiřadit `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti `INSERT`, `UPDATE`, a `DELETE` SQL příkazy ke spuštění. Pokud se příkazy mají parametry (jako největší vždy), zahrňte je do `InsertParameters`, `UpdateParameters`, a `DeleteParameters` kolekce.

Jednou `InsertCommand`, `UpdateCommand`, nebo `DeleteCommand` byla zadána hodnota, bude k dispozici možnost Povolit vložení, povolit úpravy nebo Povolit odstranění v odpovídajících dat webové ovládací prvek s inteligentních značek. Pro znázornění, trvat umožňují s příkladem `Querying.aspx` stránku jsme vytvořili v [dotazování na Data ovládacím prvkem SqlDataSource(VB)](querying-data-with-the-sqldatasource-control-vb.md) kurz a rozšířit tak, aby obsahovala odstranit možnosti.

Začněte otevřením `InsertUpdateDelete.aspx` a `Querying.aspx` ze stránky `SqlDataSource` složky. Z Návrháře na `Querying.aspx` stránce, vyberte v prvním příkladu SqlDataSource a ovládacího prvku GridView ( `ProductsDataSource` a `GridView1` ovládací prvky). Po výběru dvou ovládacích prvků, přejděte na nabídku Úpravy a zvolte Kopírovat (nebo jenom stiskněte kombinaci kláves Ctrl + C). Dále přejděte na Návrhář `InsertUpdateDelete.aspx` a vložte do ovládacích prvků. Po přesunutí dvou ovládacích prvků k `InsertUpdateDelete.aspx`, test si stránku v prohlížeči. Zobrazí se hodnoty `ProductID`, `ProductName`, a `UnitPrice` sloupce pro všechny záznamy v `Products` databázové tabulky.


[![Všechny produkty jsou uvedené, seřazené podle ID produktu](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.png)

**Obrázek 1**: všech produktů, které jsou uvedeny, seřazené podle `ProductID` ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Přidání SqlDataSource s`DeleteCommand`a`DeleteParameters`vlastnosti

V tuto chvíli máme SqlDataSource, který jednoduše vrací všechny záznamy z `Products` tabulky a prvku GridView, který vykreslí tato data. Naším cílem je rozšíření tohoto příkladu a umožní uživateli odstraňovat produkty prostřednictvím prvku GridView. K tomu potřebujeme zadat hodnoty pro ovládacím prvkem SqlDataSource s `DeleteCommand` a `DeleteParameters` vlastnosti a pak proveďte konfiguraci GridView pro podporu odstranění.

`DeleteCommand` a `DeleteParameters` vlastnosti lze zadat v několika způsoby:

- Využijte deklarativní syntaxi
- V okně Vlastnosti v Návrháři
- Zadejte vlastní příkaz SQL nebo uloženou proceduru obrazovky v Průvodci nakonfigurujte zdroj dat
- Přes tlačítko Upřesnit v zadat sloupce z tabulky obrazovka pro zobrazení v Průvodci nakonfigurujte zdroj dat, která bude ve skutečnosti automaticky generovat `DELETE` SQL příkazu a parametru kolekce používán `DeleteCommand` a `DeleteParameters` Vlastnosti

Prozkoumáme jak automaticky `DELETE` příkaz vytvořili v kroku 2. Prozatím umožní s použijte okno Vlastnosti v návrháři, i když Průvodce konfigurací zdroje dat nebo možnost deklarativní syntaxe by fungovat stejně dobře.

Z Návrháře v `InsertUpdateDelete.aspx`, klikněte na `ProductsDataSource` SqlDataSource a pak otevřete okno Vlastnosti (z nabídky Zobrazit zvolte okno Vlastnosti nebo jednoduše stiskněte tlačítko F4). Vyberte vlastnost DeleteQuery, kterým se zobrazí sada symbol tří teček.


![V okně Vlastnosti vyberte vlastnost DeleteQuery](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.gif)

**Obrázek 2**: v okně Vlastnosti vyberte vlastnost DeleteQuery


> [!NOTE]
> Kódu SqlDataSource nemá vlastnost DeleteQuery. Místo toho DeleteQuery je kombinací `DeleteCommand` a `DeleteParameters` vlastnosti a při zobrazení okna pomocí návrháře je pouze uvedené v okně Vlastnosti. Pokud chcete v okně Vlastnosti v zobrazení zdroje, najdete `DeleteCommand` vlastnost místo.


Klikněte na symbol tří teček v DeleteQuery vlastnost, která má vyvolat dialogové okno Editor příkazů a parametrů pole (viz obrázek 3). Z tohoto dialogového okna můžete zadat `DELETE` SQL příkaz a zadejte parametry. Zadejte následující dotaz do `DELETE` TextBox – příkaz (buď ručně nebo pomocí Tvůrce dotazů, pokud dáváte přednost):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample1.sql)]

Dále klikněte na tlačítko Aktualizovat parametry pro přidání `@ProductID` parametru do seznamu níže uvedené parametry.


[![V okně Vlastnosti vyberte vlastnost DeleteQuery](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.png)

**Obrázek 3**: v okně Vlastnosti vyberte vlastnost DeleteQuery ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.png))


Proveďte *není* zadejte hodnotu pro tento parametr (Ponechte její parametr zdroj v žádné). Po odstranění podpory přidáme do prvku GridView, prvku GridView bude tuto hodnotu Automaticky doplnit parametrů, pomocí hodnoty jeho `DataKeys` kolekce pro řádek došlo ke kliknutí na tlačítko jehož odstranit.

> [!NOTE]
> Název parametru použitý v `DELETE` dotazu *musí* být stejný jako název `DataKeyNames` hodnotu v prvku GridView, DetailsView nebo FormView. To znamená, že parametr v `DELETE` příkazu je záměrně s názvem `@ProductID` (místo, například `@ID`), protože je název pro sloupec primárního klíče v tabulce produkty (a tedy hodnota DataKeyNames v prvku GridView) `ProductID`.


Pokud název parametru a `DataKeyNames` hodnota kódu t shodu, prvku GridView. nelze přiřadit automaticky parametr hodnotu z `DataKeys` kolekce.

Jakmile zadáte informace týkající se odstranění do dialogového okna Editor příkazů a parametrů klikněte na tlačítko OK a přejděte do zobrazení zdroje pro zjištění výsledného deklarativní:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample2.aspx)]

Poznámka: přidání `DeleteCommand` vlastnost také `<DeleteParameters>` oddílu a objekt parametr s názvem `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Konfigurace ovládacího prvku GridView pro odstranění

S `DeleteCommand` přidána vlastnost ovládacího prvku GridView s inteligentním teď obsahuje možnost Povolit odstranění. Pokračujte a zaškrtněte toto políčko. Jak je popsáno v [přehled o vložení, aktualizaci a odstraňování](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), způsobí to, že prvku GridView. Chcete-li přidat CommandField s jeho `ShowDeleteButton` nastavenou na `True`. Obrázek 4 ukazuje, když je uživatel na stránce prostřednictvím prohlížeče je součástí tlačítko pro odstranění. Na této stránce si otestujte tak, že odstraníte některé produkty.


[![Každý řádek prvku GridView teď obsahuje tlačítko pro odstranění](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.png)

**Obrázek 4**: každý řádek prvku GridView teď obsahuje tlačítko Delete ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.png))


Po kliknutí na tlačítko pro odstranění, dojde k zpětné volání, přiřadí prvku GridView `ProductID` parametr hodnotu z `DataKeys` hodnota kolekce pro řádek, jehož tlačítko pro odstranění došlo ke kliknutí na a vyvolá SqlDataSource s `Delete()` metoda. Ovládacím prvkem SqlDataSource pak připojí k databázi a spustí `DELETE` příkazu. Prvku GridView. potom znovu připojí k ovládacím prvkem SqlDataSource návrat a zobrazení aktuální sadu produktů (který už obsahuje záznam odstranit jenom).

> [!NOTE]
> Od prvku GridView používá jeho `DataKeys` kolekce k naplnění parametrů SqlDataSource ho s důležité, který GridView s `DataKeyNames` vlastnost nastavit na sloupce, které tvoří primární klíč a že SqlDataSource s `SelectCommand` vrátí Tyto sloupce. Kromě toho je důležité, že parametr name ve třídě SqlDataSource s `DeleteCommand` je nastavena na `@ProductID`. Pokud `DataKeyNames` vlastnost není nastavená nebo není parametr pojmenovaný `@ProductsID`, kliknutím na tlačítko Odstranit budou vyvolávají zpětné odeslání, ale získaných oproti očekávaným t skutečně odstranit kterýkoli záznam.


Obrázek 5 graficky znázorňuje tuto interakci. Vraťte se do [zkoumání události spojené s vložení, aktualizace a odstranění](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) kurz podrobnější informace o sled událostí spojených s vložením, aktualizace a odstranění dat webový ovládací prvek.


![Kliknutím na tlačítko Odstranit v prvku GridView vyvolá metodu SqlDataSource s Delete()](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.gif)

**Obrázek 5**: Kliknutím na tlačítko Odstranit v prvku GridView vyvolá SqlDataSource s `Delete()` – metoda


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Krok 2: Automatické generování`INSERT`,`UPDATE`, a`DELETE`příkazy

Krok 1 zkontrolují `INSERT`, `UPDATE`, a `DELETE` příkazů jazyka SQL je možné zadat pomocí okna vlastnosti nebo ovládací prvek s deklarativní syntaxe. Tento přístup vyžaduje, že jsme ručně vypsat příkazy SQL ručně, což může být monotónní a náchylné k chybě. Naštěstí Průvodce konfigurace zdroje dat poskytuje možnost mít `INSERT`, `UPDATE`, a `DELETE` příkazy automaticky generovány při použití zadat sloupce z tabulky obrazovka pro zobrazení.

Umožní prozkoumat tuto možnost automatické generování s. Přidat do návrháře v DetailsView `InsertUpdateDelete.aspx` a nastavte jeho `ID` vlastnost `ManageProducts`. Potom z inteligentních značek s prvku DetailsView. Vyberte pro vytvoření nového zdroje dat a vytvoření SqlDataSource s názvem `ManageProductsDataSource`.


[![Vytvořit nový SqlDataSource s názvem ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.png)

**Obrázek 6**: Vytvořte nový SqlDataSource s názvem `ManageProductsDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.png))


V Průvodci nakonfigurujte zdroj dat rozhodnout použít `NORTHWINDConnectionString` připojovací řetězec a klikněte na tlačítko Další. Z konfigurace obrazovky příkaz Select, ponechte zadat sloupce z tabulky nebo zobrazení přepínač vybraný a vyberte si `Products` tabulku z rozevíracího seznamu. Vyberte `ProductID`, `ProductName`, `UnitPrice`, a `Discontinued` sloupce ze seznamu zaškrtávací políčko.


[![Pomocí tabulky produktů, vrátí ProductID, ProductName, UnitPrice a již nepoužívané sloupce](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.png)

**Obrázek 7**: použití `Products` tabulky, vraťte se `ProductID`, `ProductName`, `UnitPrice`, a `Discontinued` sloupce ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image10.png))


Automaticky generovat `INSERT`, `UPDATE`, a `DELETE` příkazy na základě vybrané tabulky a sloupce, klikněte na tlačítko Upřesnit a zkontrolujte generovat `INSERT`, `UPDATE`, a `DELETE` příkazy zaškrtávací políčko.


![Zkontrolujte příkazy Generovat INSERT, UPDATE a DELETE zaškrtávací políčko](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.gif)

**Obrázek 8**: Zkontrolujte, generování `INSERT`, `UPDATE`, a `DELETE` příkazy zaškrtávací políčko


Generovat `INSERT`, `UPDATE`, a `DELETE` příkazy zaškrtávací políčko bude pouze kontrolovatelné Pokud primární klíč má vybranou tabulku a sloupec primárního klíče (nebo sloupce) jsou zahrnuty v seznamu vrácené sloupce. Zaškrtávací políčko optimistického řízení souběžnosti používá stane volitelných jednou generovat `INSERT`, `UPDATE`, a `DELETE` po kontrole příkazy zaškrtávacího políčka se rozšířit `WHERE` ustanovení výsledný `UPDATE` a `DELETE` příkazy k poskytování optimistického řízení souběžnosti. Prozatím ponechejte zaškrtnuté políčko; Toto zaškrtávací políčko prozkoumáme optimistického řízení souběžnosti ovládacím prvkem SqlDataSource(VB) v dalším kurzu.

Po kontrole generovat `INSERT`, `UPDATE`, a `DELETE` příkazy zaškrtávací políčko, klikněte na tlačítko OK vraťte na obrazovku Konfigurace příkazu Select a pak klikněte na tlačítko Další a pak dokončete, dokončete průvodce pro zdroj dat nakonfigurovat. Po dokončení průvodce bude Visual Studio přidá BoundFields do ovládacího prvku DetailsView pro `ProductID`, `ProductName`, a `UnitPrice` sloupce a třídě CheckBoxField pro `Discontinued` sloupce. Z inteligentních značek prvek DetailsView s zaškrtněte možnost Povolit stránkování tak, aby uživatel na této stránce můžete krokovat produktů. Také smažte prvek DetailsView s `Width` a `Height` vlastnosti.

Všimněte si, že se inteligentní značky možnosti Povolit vložení, povolit úpravy a Povolit odstranění, které jsou k dispozici. Důvodem je, že ve třídě SqlDataSource obsahuje hodnoty pro jeho `InsertCommand`, `UpdateCommand`, a `DeleteCommand`, jak ukazuje následující deklarativní syntaxe:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample3.aspx)]

Všimněte si, jak ovládacím prvkem SqlDataSource zaznamenala hodnoty automaticky nastaví pro jeho `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti. Sadu sloupců, které odkazuje `InsertCommand` a `UpdateCommand` vlastnosti jsou podle toho, které v `SELECT` příkaz. To znamená místo generování *každý* produkty sloupec v `InsertCommand` a `UpdateCommand`, existují pouze sloupce, podle `SelectCommand` (méně `ProductID`, což je vynechána, protože ho s [ `IDENTITY` sloupec](http://www.sqlteam.com/item.asp?ItemID=102), jehož hodnota nemůže být změněna při úpravách a které se automaticky přiřazují při vkládání). Kromě toho pro každý parametr `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti jsou odpovídajícím parametrům in v `InsertParameters`, `UpdateParameters`, a `DeleteParameters` kolekce.

Úpravy funkce DetailsView s dat zapnout, zkontrolujte Povolit vložení, povolit úpravy a Povolit odstranění možnosti v její inteligentních značek. Tento postup přidá CommandField s jeho `ShowInsertButton`, `ShowEditButton`, a `ShowDeleteButton` nastaveny `True`.

Na stránce v prohlížeči a Všimněte si, úprava, odstranění a nová tlačítka, které jsou zahrnuté v ovládacím prvku DetailsView. Do režimu úprav, který zobrazuje každou vlastnost BoundField kliknutím na tlačítko pro úpravy se změní na ovládacím prvku DetailsView jehož `ReadOnly` je nastavena na `False` (výchozí) jako textové pole a třídě CheckBoxField jako zaškrtávací políčko.


[![Prvek DetailsView s výchozí editační rozhraní](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image11.png)

**Obrázek 9**: The DetailsView s výchozí rozhraní pro úpravy ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image12.png))


Podobně můžete odstranit aktuálně vybraný produkt nebo přidání nového produktu do systému. Protože `InsertCommand` příkaz funguje jenom s `ProductName`, `UnitPrice`, a `Discontinued` sloupce, další sloupce, které mají buď `NULL` nebo výchozí hodnoty přiřazené v databázi při vložení. Stejně jako ovládacím prvkem ObjectDataSource, pokud `InsertCommand` neobsahuje žádné databázové tabulky, sloupce, které nejsou t povolit `NULL` s a Nepřipojovat t mají výchozí hodnotu, dojde k chybě SQL při pokusu provést `INSERT` příkazu.

> [!NOTE]
> Prvek DetailsView s vkládání a úpravy rozhraní nedostatku jakýkoli druh ověření nebo vlastního nastavení. Přidání validačních ovládacích prvků nebo přizpůsobit rozhraní, musíte převést BoundFields vlastností TemplateField. Odkazovat [přidání validačních ovládacích prvků pro úpravy a vložení rozhraní](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) a [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) kurzy pro další informace.


Také vzít v úvahu, že pro aktualizaci a odstraňování, používá ovládacím prvku DetailsView. aktuální produkt s `DataKey` hodnotu, která je k dispozici pouze pokud `DataKeyNames` vlastnost je nakonfigurovaná. Pokud se zdá, že oprávnění úpravy nebo odstranění nemají žádný vliv, ujistěte se, že `DataKeyNames` je nastavena.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Omezení pro automatické generování SQL příkazy

Od generovat `INSERT`, `UPDATE`, a `DELETE` příkazy možnost je dostupná jenom při výběru sloupců z tabulky, pro složitější dotazy, které bude nutné napsat vlastní `INSERT`, `UPDATE`, a `DELETE` příkazy, jak jsme to udělali v kroku 1. Běžně, SQL `SELECT` příkazy používají `JOIN` s vrací do stavu zobrazení dat z jednoho nebo více vyhledávací tabulky pro účely (např. znovu připojuje zpět `Categories` tabulky s `CategoryName` pole při zobrazení informací o produktu). Ve stejnou dobu může být chceme aby uživatel mohl upravit, aktualizovat nebo vložit data do tabulky core (`Products`, v tomto případě).

Zatímco `INSERT`, `UPDATE`, a `DELETE` příkazy lze zadat ručně, vezměte v úvahu následující tip ušetří čas. Počáteční nastavení ve třídě SqlDataSource tak, aby načte zpět data pouze z `Products` tabulky. Použít průvodce Konfigurovat zdroj dat s zadat sloupce z tabulky nebo zobrazení obrazovky, takže můžete automaticky vygenerovat `INSERT`, `UPDATE`, a `DELETE` příkazy. Po dokončení Průvodce nakonfigurovat SelectQuery z okna Vlastnosti rozhodnete (nebo případně přejděte zpět do Průvodce konfigurací zdroje dat, ale použijte zadejte vlastní příkaz SQL nebo uloženou proceduru možnost). Pak aktualizujte `SELECT` příkazu zahrnout `JOIN` syntaxe. Tento postup nabízí výhody ušetří čas automaticky generované příkazů SQL a umožňuje více přizpůsobit `SELECT` příkazu.

Další omezení automatického generování `INSERT`, `UPDATE`, a `DELETE` příkazech je to, že sloupce v `INSERT` a `UPDATE` příkazy jsou založeny na sloupcích vrácené `SELECT` příkazu. Budeme muset aktualizovat nebo vložit více nebo méně pole, ale. Například v příkladu v kroku 2, možná Chceme mít `UnitPrice` Vlastnost BoundField být jen pro čtení. V takovém případě ji joinkind nesmí obsahovat více t `UpdateCommand`. Nebo může být vhodné nastavit hodnotu pole tabulky, které nejsou uvedené v prvku GridView. Například při přidání nového záznamu může chceme `QuantityPerUnit` nastavenou TODO.

Pokud tyto úpravy jsou požadovány, musíte je vytvořit ručně, buď v okně Vlastnosti, zadejte vlastní příkaz SQL nebo uloženou proceduru možnost v průvodci nebo pomocí deklarativní syntaxe.

> [!NOTE]
> Při přidávání parametrů, které nemají odpovídající pole v datech ovládací prvek webu, mějte na paměti, s jehož hodnoty těchto parametrů musí být přiřazeny hodnoty způsobem. Tyto hodnoty mohou být: pevně zakódované přímo `InsertCommand` nebo `UpdateCommand`; mohou pocházet z některé předdefinované zdroje (řetězce dotazu, stav relace, webové ovládací prvky na stránce a tak dále); nebo můžete přiřadit prostřednictvím kódu programu, jako jsme viděli v předchozím kurzu.


## <a name="summary"></a>Souhrn

V pořadí dat webové ovládací prvky využívat jejich vkládání předdefinovaných, úpravy a odstranění funkce musí ovládací prvek zdroje dat, které jsou vázány na nabízí tyto funkce. Pro ovládacím prvkem SqlDataSource, to znamená, že `INSERT`, `UPDATE`, a `DELETE` příkazy SQL, musíte být přiřazeni k `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti. Tyto vlastnosti a odpovídající parametry kolekce, můžete přidat ručně nebo prostřednictvím Průvodce konfigurace zdroje dat, vygenerovaný automaticky. V tomto kurzu jsme se zaměřili na obě tyto metody.

Jsme se zaměřili na pomocí optimistického řízení souběžnosti ovládacím prvkem ObjectDataSource v [implementace optimistického řízení souběžnosti](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) kurzu. Ovládacím prvkem SqlDataSource také poskytuje podporu optimistického řízení souběžnosti. Jak je uvedeno v kroku 2, při automatickém generování `INSERT`, `UPDATE`, a `DELETE` příkazy, nabízí průvodce použijte možnost optimistického řízení souběžnosti. Jak uvidíme v dalším kurzu, pomocí optimistického řízení souběžnosti ovládacím prvkem SqlDataSource upraví `WHERE` ustanovení `UPDATE` a `DELETE` příkazy k zajištění, že některé hodnoty ostatních sloupců t změněna od poslední data na stránce zobrazí.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](using-parameterized-queries-with-the-sqldatasource-vb.md)
> [další](implementing-optimistic-concurrency-with-the-sqldatasource-vb.md)
