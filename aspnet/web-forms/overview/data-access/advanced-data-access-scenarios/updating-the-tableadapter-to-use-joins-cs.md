---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
title: Aktualizace komponenty TableAdapter kvůli použití spojení (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Při práci s databází je společné pro žádost o data, která je rozdělena mezi více tabulek. Načíst data ze dvou různých tabulek můžete použít buď...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 675531a7-cb54-4dd6-89ac-2636e4c285a5
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
msc.type: authoredcontent
ms.openlocfilehash: c59ab13eb5e5e548055c6c8c9343a8d5c4cabe7d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841118"
---
<a name="updating-the-tableadapter-to-use-joins-c"></a>Aktualizace komponenty TableAdapter kvůli použití spojení (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_CS.zip) nebo [stahovat PDF](updating-the-tableadapter-to-use-joins-cs/_static/datatutorial69cs1.pdf)

> Při práci s databází je společné pro žádost o data, která je rozdělena mezi více tabulek. Načíst data ze dvou různých tabulek můžeme použít korelační poddotaz nebo operace spojení. V tomto kurzu jsme před pohledu na tom, jak vytvořit, který obsahuje spojení v jeho hlavním dotazem objektu TableAdapter porovnání korelační poddotazy a syntaxi příkazu JOIN.


## <a name="introduction"></a>Úvod

U relačních databází data, která nás zajímá při práci se často rozděleny mezi více tabulek. Například při zobrazení informací o produktu pravděpodobně chceme seznamu jednotlivých produktů s odpovídající kategorii a dodavatele s názvy. `Products` Tabulka má `CategoryID` a `SupplierID` hodnoty, ale skutečné názvy kategorií a dodavatele jsou `Categories` a `Suppliers` tabulek, v uvedeném pořadí.

K načtení informací z jiného, související tabulky, můžeme použít *korelační poddotazy* nebo `JOIN` *s*. Je vnořený poddotaz korelační `SELECT` dotaz, který odkazuje na sloupce ve vnějším dotazu. Například v [vytvoření vrstvy přístupu k datům](../introduction/creating-a-data-access-layer-cs.md) kurzu jsme použili dva korelační poddotazy v `ProductsTableAdapter` s hlavní dotaz, který vrací kategorie a dodavatele názvy pro každý produkt. A `JOIN` je konstrukce SQL, která sloučí související řádky ze dvou různých tabulek. Jsme použili `JOIN` v [dotazování na Data ovládacím prvkem SqlDataSource(VB)](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs.md) kurzu zobrazíte informace o kategoriích vedle každého produktu.

Z důvodu budeme mít zdržel pomocí `JOIN` s objekty TableAdapter se kvůli omezením v Průvodci vytvořením objektu TableAdapter s automaticky vygenerovat odpovídající `INSERT`, `UPDATE`, a `DELETE` příkazy. Přesněji řečeno pokud s hlavním dotazu objektu TableAdapter obsahuje některý `JOIN` s, TableAdapter nelze automaticky vytvořit příkazy ad-hoc SQL nebo uložených procedur pro jeho `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti.

V tomto kurzu stručně porovnáme a kontrast korelační poddotazy a `JOIN` s prozkoumáte vytvoření objektu typu TableAdapter, který zahrnuje `JOIN` s v jeho hlavním dotazem.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Porovnávání a kontrastní korelační poddotazy a`JOIN` s

Vzpomeňte si, že `ProductsTableAdapter` vytvořili v prvním kurzu v `Northwind` datová sada používá korelační poddotazy vrací do stavu každého s odpovídající kategorii a dodavatele názvu produktu. `ProductsTableAdapter` s hlavním dotazu je uveden níže.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample1.sql)]

Dva korelační poddotazy - `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` a `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` – jsou `SELECT` dotazů, které vrátí jednu hodnotu jednotlivých produktů jako další sloupce ve vnějším `SELECT` seznam sloupců příkazu s.

Můžete také `JOIN` lze použít k vrácení každý název dodavatele a kategorie produktu s. Následující dotaz vrátí stejný výstup jako ten výše, ale používá `JOIN` s namísto poddotazů:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample2.sql)]

A `JOIN` slučuje záznamy z jedné tabulky se záznamy z jiné tabulky podle kritérií. Ve výše uvedeném dotazu, například `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` SQL serveru pokyn ke sloučení každý záznam produktu v kategorii záznam, jehož `CategoryID` hodnota odpovídá produkt s `CategoryID` hodnotu. Výsledek sloučení umožňuje pracovat s odpovídající pole kategorií pro každý produkt (jako například `CategoryName`).

> [!NOTE]
> `JOIN` s se běžně používají při dotazování na data z relační databáze. Pokud jste ještě `JOIN` syntaxe nebo potřeba trochu zdokonalit v práci jejich využití, d doporučuji [kurzu připojit k SQL](http://www.w3schools.com/sql/sql_join.asp) na [W3 školy](http://www.w3schools.com/). Je také vhodné čtení jsou [ `JOIN` Základy](https://msdn.microsoft.com/library/ms191517.aspx) a [poddotaz Základy](https://msdn.microsoft.com/library/ms189575.aspx) oddíly [dokumentaci knihy Online SQL](https://msdn.microsoft.com/library/ms130214.aspx).


Protože `JOIN` s a korelační poddotazy obě slouží k načtení souvisejících dat z jiné tabulky, celá řada vývojářů zůstanou hrabání jejich hlavy a zajímá vás, jaký přístup používat. Všechny SQL gurus I ve mluvili Pokud chcete mít říká, že přibližně stejnou věc ji kódu t opravdu důležité performance-wise systému SQL Server bude vytvářet plány zhruba stejné provádění. Svá doporučení, potom je použijte techniku, která se nejvíce vyhovuje vám a vašemu týmu. To věci poznamenat, že po vodorovně tuto žádost tyto odborníky okamžitě express jejich předvolbu `JOIN` s nad korelační poddotazy.

Při sestavování vrstvy přístupu k datům pomocí zadané datové sady, nástroje fungují lépe při použití poddotazy. Zejména s průvodci vytvořením objektu TableAdapter nebudou automaticky generovat odpovídající `INSERT`, `UPDATE`, a `DELETE` příkazy, pokud hlavním dotazem obsahuje některý `JOIN` s, ale bude automaticky vygenerovat tyto příkazy, pokud je korelační poddotazů se používají.

Prozkoumat tento nedostatek, vytvořit dočasný typované datové sady v `~/App_Code/DAL` složky. V průběhu Průvodce konfigurací TableAdapter zvolit použití ad-hoc SQL a zadejte následující `SELECT` dotazu (viz obrázek 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample3.sql)]


[![Zadejte hlavní dotaz, který obsahuje spojení](updating-the-tableadapter-to-use-joins-cs/_static/image2.png)](updating-the-tableadapter-to-use-joins-cs/_static/image1.png)

**Obrázek 1**: Zadejte hlavní dotaz, který obsahuje `JOIN` s ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-cs/_static/image3.png))


Ve výchozím nastavení, se automaticky vytvoří TableAdapter `INSERT`, `UPDATE`, a `DELETE` příkazy na základě hlavního dotazu. Pokud kliknete na tlačítko Upřesnit uvidíte, že je tato funkce povolena. Bez ohledu na toto nastavení TableAdapter nebude možné vytvořit `INSERT`, `UPDATE`, a `DELETE` příkazy protože obsahuje hlavní dotaz `JOIN`.


![Zadejte hlavní dotaz, který obsahuje spojení](updating-the-tableadapter-to-use-joins-cs/_static/image4.png)

**Obrázek 2**: Zadejte hlavní dotaz, který obsahuje `JOIN` s


Kliknutím na Dokončit dokončíte průvodce. V tomto okamžiku bude obsahovat jeden TableAdapter s datové tabulky se sloupci pro každé pole vrácené v vaše datová sada s návrháře `SELECT` seznamu sloupců dotazu s. Jedná se o `CategoryName` a `SupplierName`, jak je vidět na obrázku 3.


![Objekt DataTable obsahuje sloupec pro každé pole vrácené v seznamu sloupců](updating-the-tableadapter-to-use-joins-cs/_static/image5.png)

**Obrázek 3**: objekt DataTable obsahuje sloupec pro každé pole vrácené v seznamu sloupců


Objekt DataTable obsahuje odpovídající sloupce, TableAdapter nemá hodnoty pro jeho `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti. Pokud to pokud chcete potvrdit, klikněte na TableAdapter v návrháři a potom přejděte do okna Vlastnosti. Existuje, který se zobrazí `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti jsou nastaveny na (žádný).


[![Událost InsertCommand, událost UpdateCommand a událost DeleteCommand vlastnosti nastavené na (žádný)](updating-the-tableadapter-to-use-joins-cs/_static/image7.png)](updating-the-tableadapter-to-use-joins-cs/_static/image6.png)

**Obrázek 4**: `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti jsou nastaveny na (žádný) ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-cs/_static/image8.png))


Tento nedostatek obejít, můžete ručně poskytujeme SQL příkazy a parametry `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti pomocí okna Vlastnosti. Alternativně může Začneme tím, že nakonfigurujete hlavního dotazu TableAdapter s k *není* zahrnutí všech `JOIN` s. To vám umožní `INSERT`, `UPDATE`, a `DELETE` příkazů bude automaticky generované pro nás. Po dokončení průvodce, může pak ručně aktualizujeme TableAdapter s `SelectCommand` v okně Vlastnosti tak, aby zahrnoval `JOIN` syntaxe.

Přestože tento přístup funguje, je velmi křehká při pomocí ad-hoc dotazy SQL, protože pokaždé, když s hlavním dotazu objektu TableAdapter je znovu nakonfigurovaný pomocí Průvodce automaticky generovanou `INSERT`, `UPDATE`, a `DELETE` příkazy jsou znovu vytvořena. To znamená, že veškerá přizpůsobení jsme udělali později by dojít ke ztrátě, pokud jsme klikli pravým tlačítkem myši na TableAdapter, zvolili konfigurace v místní nabídce a dokončení průvodce znovu.

Brittleness TableAdapter s automaticky generovanou `INSERT`, `UPDATE`, a `DELETE` příkazy je naštěstí omezené nebo ad-hoc příkazů jazyka SQL. Pokud vaše TableAdapter používá uložené procedury, můžete přizpůsobit `SelectCommand`, `InsertCommand`, `UpdateCommand`, nebo `DeleteCommand` uložené procedury a znovu spusťte Průvodce nastavením TableAdapter nemusíte se obávat, že se budou uložené procedury upravit.

Příštích několika krocích vytvoříme TableAdapter, standardně používá hlavní dotaz, který se vynechá všechny `JOIN` s odpovídající vložení, aktualizace a odstranění uložených procedur se vygeneruje automaticky. Potom aktualizujeme `SelectCommand` tak, která používá `JOIN` další sloupce, který vrací ze souvisejících tabulek. Nakonec vytvoříme odpovídající třída vrstvy obchodní logiky a demonstruje použití objektu TableAdapter na webové stránce ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Krok 1: Vytvoření pomocí jednodušší hlavním dotazu objektu TableAdapter

Pro účely tohoto kurzu přidáme TableAdapter a DataTable silného typu pro `Employees` v tabulku `NorthwindWithSprocs` datové sady. `Employees` Obsahuje tabulku `ReportsTo` pole, které zadané `EmployeeID` nadřízeného zaměstnance s. Například zaměstnance Anne Veselého má `ReportTo` hodnotu 5, která je `EmployeeID` z Steven Novák. V důsledku toho Anne sestavy Steven manažerovi. Spolu s reporting jednotliví zaměstnanci s `ReportsTo` hodnotu, jsme také možné načíst název příslušného manažera. Můžete to udělat pomocí `JOIN`. Ale pomocí `JOIN` při prvotním vytvoření objektu TableAdapter vylučuje Průvodce automatického generování odpovídající vložení, aktualizace a odstranění funkce. Proto začneme vytvořením objektu TableAdapter, jejichž hlavní dotaz neobsahuje žádný `JOIN` s. Pak v kroku 2, aktualizujeme hlavního dotazu uložené procedury načíst název správce s prostřednictvím `JOIN`.

Začněte otevřením `NorthwindWithSprocs` datovou sadu v `~/App_Code/DAL` složky. Klikněte pravým tlačítkem na návrháři, v místní nabídce vyberte možnost Přidat a vyberte položku nabídky TableAdapter. Tím spustíte Průvodce konfigurací TableAdapter. Jak znázorňuje obrázek 5 má průvodce vytvořit nové uložené procedury a klikněte na tlačítko Další. U aktualizačního programu na vytvoření nových uložených procedur v Průvodci vytvořením objektu TableAdapter s, najdete [vytváří se nové uložené procedury, pro zadané datové sady s objekty TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) kurzu.


[![Vyberte vytvořit nové uložené procedury možnost](updating-the-tableadapter-to-use-joins-cs/_static/image10.png)](updating-the-tableadapter-to-use-joins-cs/_static/image9.png)

**Obrázek 5**: Vyberte vytvořit nové uložené procedury možnost ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-cs/_static/image11.png))


Pomocí následujících `SELECT` příkazu pro TableAdapter s hlavním dotazu:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample4.sql)]

Vzhledem k tomu, že tento dotaz neobsahuje žádné `JOIN` s, TableAdapter průvodce automaticky vytvoří uložené procedury s odpovídající `INSERT`, `UPDATE`, a `DELETE` příkazy, stejně jako uloženou proceduru pro provádění hlavní dotaz.

Následující krok umožňuje pojmenovat s uložené procedury TableAdapter. Názvy `Employees_Select`, `Employees_Insert`, `Employees_Update`, a `Employees_Delete`, jak je znázorněno na obrázku 6.


[![Název TableAdapter s uložené postupy](updating-the-tableadapter-to-use-joins-cs/_static/image13.png)](updating-the-tableadapter-to-use-joins-cs/_static/image12.png)

**Obrázek 6**: název třídy TableAdapter s uložené procedury ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-cs/_static/image14.png))


V posledním kroku vyzve nám název metody s TableAdapter. Použití `Fill` a `GetEmployees` jako názvy metod. Také je potřeba nechat vytvořit metody k odeslání aktualizací přímo do databáze (GenerateDBDirectMethods) zaškrtnutí.


[![Název TableAdapter s metody Fill a GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image16.png)](updating-the-tableadapter-to-use-joins-cs/_static/image15.png)

**Obrázek 7**: název třídy TableAdapter s metod `Fill` a `GetEmployees` ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-cs/_static/image17.png))


Po dokončení průvodce, věnujte chvíli prozkoumat uložené procedury v databázi. Měli byste vidět čtyři nové: `Employees_Select`, `Employees_Insert`, `Employees_Update`, a `Employees_Delete`. Dále zkontrolujte `EmployeesDataTable` a `EmployeesTableAdapter` právě vytvořili. Objekt DataTable obsahuje sloupec pro každé pole vráceného hlavním dotazem. Kliknutím na TableAdapter a přejděte do okna Vlastnosti. Uvidíte, `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti jsou správně nakonfigurované volat odpovídající uložené procedury.


[![TableAdapter zahrnuje příkaz Insert, Update a odstranit funkce](updating-the-tableadapter-to-use-joins-cs/_static/image19.png)](updating-the-tableadapter-to-use-joins-cs/_static/image18.png)

**Obrázek 8**: The zahrnuje TableAdapter Insert, Update a Delete možnosti ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-cs/_static/image20.png))


S vložit, aktualizovat a odstraňovat uložené procedury automaticky vytvoří a `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti správně konfigurované, jsme připraveni k přizpůsobení `SelectCommand` s uloženou proceduru vrátit další informace o Správci každý zaměstnanec s. Konkrétně, musíme aktualizovat `Employees_Select` uložené procedury pro použití `JOIN` a vraťte se správce s `FirstName` a `LastName` hodnoty. Po aktualizaci uložené procedury, budeme muset update DataTable tak, že obsahují tyto dodatečné sloupce. Jsme budete řešit tyto dvě úlohy v krocích 2 a 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Krok 2: Přizpůsobení uložené procedury, která patří`JOIN`

Začněte tím, že přejdete do Průzkumníka serveru, procházení k podrobnostem složky uložené procedury databáze s Northwind a otevřít `Employees_Select` uložené procedury. Pokud nevidíte tuto uloženou proceduru, klikněte pravým tlačítkem na složku uložené procedury a vyberte příkaz Aktualizovat. Aktualizovat uložené procedury tak, aby používala `LEFT JOIN` vrátit správce s prvním a příjmení:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample5.sql)]

Po aktualizaci `SELECT` prohlášení, uložte změny tak, že přejdete do nabídky soubor a zvolíte Uložit `Employees_Select`. Alternativně můžete kliknutím na ikonu Uložit na panelu nástrojů nebo stiskněte kombinaci kláves Ctrl + S. Po uložení změn, klikněte pravým tlačítkem na `Employees_Select` uloženou proceduru v Průzkumníku serveru a zvolte spustit. Spustí uloženou proceduru a zobrazit jeho výsledky v okně výstupu (viz obrázek 9).


[![V okně výstupu se zobrazují výsledky uložené procedury](updating-the-tableadapter-to-use-joins-cs/_static/image22.png)](updating-the-tableadapter-to-use-joins-cs/_static/image21.png)

**Obrázek 9**: The uložené procedury výsledky se zobrazí v okně výstupu ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-cs/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>Krok 3: Aktualizace sloupců s DataTable

V tomto okamžiku `Employees_Select` uložené procedury vrátí `ManagerFirstName` a `ManagerLastName` hodnoty, ale `EmployeesDataTable` chybí tyto sloupce. Tyto chybějící sloupce lze přidat do DataTable v jednom ze dvou způsobů:

- **Ručně** – klikněte pravým tlačítkem na objekt DataTable v návrháři datových sad a z nabídky přidat, vyberte sloupec. Můžete pak název sloupce a odpovídajícím způsobem nastavit jeho vlastnosti.
- **Automaticky** – Průvodce konfigurací TableAdapter se aktualizovat sloupce s DataTable tak, aby odrážely pole vrácené `SelectCommand` uložené procedury. Při použití příkazů jazyka SQL ad-hoc, Průvodce odstraní také `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti od `SelectCommand` nyní obsahuje `JOIN`. Ale při použití uložených procedur, tyto vlastnosti příkazu nijak nezmění.

Prozkoumali jsme ruční přidávání sloupců do tabulky DataTable v předchozích kurzech, včetně [Master/Detail, použití odrážkovém seznamu v hlavních záznamů s podrobnostmi v prvku DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) a [nahrávání souborů](../working-with-binary-files/uploading-files-cs.md), a My Podívejte se na tento proces znovu podrobněji v následujícím kurzem. Pro účely tohoto kurzu ale umožní použít automatické přístup přes Průvodce konfigurací TableAdapter s.

Začněte tím, že pravým tlačítkem myši na `EmployeesTableAdapter` a výběrem možnosti konfigurace v místní nabídce. Tím se vyvolá průvodce konfigurací TableAdapter, který obsahuje seznam uložených procedur používaných pro výběr, vkládání, aktualizaci a odstraňování spolu s jejich návratové hodnoty a parametry (pokud existuje). Obrázek 10 ukazuje tohoto průvodce. Tady vidíme, `Employees_Select` uložené procedury nyní vrátí `ManagerFirstName` a `ManagerLastName` pole.


[![Průvodce zobrazí seznam aktualizované sloupců Employees_Select uložené procedury](updating-the-tableadapter-to-use-joins-cs/_static/image25.png)](updating-the-tableadapter-to-use-joins-cs/_static/image24.png)

**Obrázek 10**: zobrazí průvodce aktualizuje seznam sloupců `Employees_Select` uloženou proceduru ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-cs/_static/image26.png))


Dokončete průvodce kliknutím na tlačítko Dokončit. Po návratu návrháři datových sad `EmployeesDataTable` zahrnuje další dva sloupce: `ManagerFirstName` a `ManagerLastName`.


[![EmployeesDataTable obsahuje dva nové sloupce](updating-the-tableadapter-to-use-joins-cs/_static/image28.png)](updating-the-tableadapter-to-use-joins-cs/_static/image27.png)

**Obrázek 11**: `EmployeesDataTable` obsahuje dva nové sloupce ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-cs/_static/image29.png))


Pro ilustraci, že aktualizovaná `Employees_Select` uložené procedury je v platnosti a že vložení, aktualizace a odstranění funkce objektu TableAdapter jsou funkční, umožňují vytvořit webovou stránku, která umožňuje uživatelům zobrazovat a odstraňovat zaměstnanci s. Než vytvoříme taková ještě stránka, však musíte nejprve vytvořit novou třídu do vrstvy obchodní logiky pro práci se zaměstnanci `NorthwindWithSprocs` datové sady. V kroku 4, vytvoříme `EmployeesBLLWithSprocs` třídy. V kroku 5 použijeme tuto třídu ze stránky ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Krok 4: Implementace vrstvy obchodní logiky

Vytvořte nový soubor třídy v `~/App_Code/BLL` složku s názvem `EmployeesBLLWithSprocs.cs`. Tato třída napodobuje sémantika existující `EmployeesBLL` třídy, tato nová jeden poskytuje méně metody a používá `NorthwindWithSprocs` datovou sadu (místo `Northwind` datové sady). Přidejte následující kód, který `EmployeesBLLWithSprocs` třídy.


[!code-csharp[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample6.cs)]

`EmployeesBLLWithSprocs` Třída s `Adapter` vlastnost vrací instanci `NorthwindWithSprocs` datovou sadu s `EmployeesTableAdapter`. Používá se ve třídě s `GetEmployees` a `DeleteEmployee` metody. `GetEmployees` Volání metod `EmployeesTableAdapter` s odpovídající `GetEmployees` metodu, která vyvolá `Employees_Select` uložené procedury a naplní jeho výsledky v `EmployeeDataTable`. `DeleteEmployee` Podobně volá metodu `EmployeesTableAdapter` s `Delete` metodu, která vyvolá `Employees_Delete` uložené procedury.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Krok 5: Práce s daty v prezentační vrstvě

S `EmployeesBLLWithSprocs` třídy dokončení, můžeme znovu připravené pro práci s daty o zaměstnancích prostřednictvím stránky ASP.NET. Otevřít `JOINs.aspx` stránku `AdvancedDAL` složky a GridView přetáhněte z panelu nástrojů do Návrháře nastavení jeho `ID` vlastnost `Employees`. V dalším kroku z inteligentních značek GridView s svázat mřížky nového ovládacího prvku ObjectDataSource s názvem `EmployeesDataSource`.

Konfigurace ObjectDataSource používat `EmployeesBLLWithSprocs` třídy a z karty vybrat a odstranit, ujistěte se, že `GetEmployees` a `DeleteEmployee` metody jsou vybrány z rozevíracích seznamů. Kliknutím na tlačítko Dokončit dokončete konfiguraci prvku ObjectDataSource s.


[![Konfigurace ObjectDataSource pomocí třídy EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-cs/_static/image31.png)](updating-the-tableadapter-to-use-joins-cs/_static/image30.png)

**Obrázek 12**: Konfigurace ObjectDataSource k použití `EmployeesBLLWithSprocs` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-cs/_static/image32.png))


[![Mít použití prvku ObjectDataSource GetEmployees a DeleteEmployee metody](updating-the-tableadapter-to-use-joins-cs/_static/image34.png)](updating-the-tableadapter-to-use-joins-cs/_static/image33.png)

**Obrázek 13**: mají použití prvku ObjectDataSource `GetEmployees` a `DeleteEmployee` metody ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-cs/_static/image35.png))


Visual Studio přidá vlastnost BoundField do prvku GridView. pro každou `EmployeesDataTable` s sloupce. Odeberte všechny tyto BoundFields s výjimkou `Title`, `LastName`, `FirstName`, `ManagerFirstName`, a `ManagerLastName` a přejmenovat `HeaderText` vlastnosti pro poslední čtyři BoundFields příjmení, křestního jména, správce s křestní jméno, a Správce s příjmení, v uvedeném pořadí.

Povolit uživatelům odstranit zaměstnanci z této stránky, budeme muset udělat dvě věci. Nejprve dáte pokyn, aby prvku GridView a poskytují možnosti odstranění zaškrtnutím možnosti Povolit odstranění z jeho inteligentních značek. Za druhé, změňte ObjectDataSource s `OldValuesParameterFormatString` vlastnost z hodnoty nastavena průvodcem ObjectDataSource (`original_{0}`) na výchozí hodnotu (`{0}`). Po provedení těchto změn vašeho ovládacího prvku GridView a prvku ObjectDataSource s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample7.aspx)]

Otestování stránky návštěvou prostřednictvím prohlížeče. Jak ukazuje obrázek 14 stránce začlení jednotliví zaměstnanci a jeho správce s názvem (za předpokladu, že budou mít jeden).


[![SPOJENÍ v Employees_Select uložené procedury vrátí název s správce](updating-the-tableadapter-to-use-joins-cs/_static/image37.png)](updating-the-tableadapter-to-use-joins-cs/_static/image36.png)

**Obrázek 14**: `JOIN` v `Employees_Select` vrátí správce s názvem uložené procedury ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-cs/_static/image38.png))


Kliknutím na tlačítko Odstranit spustí odstranit pracovní postup, který culminates za běhu `Employees_Delete` uložené procedury. Ale neúspěšné pokusy o `DELETE` příkaz v uložené proceduře se nezdaří z důvodu narušení omezení pro cizí klíč (viz obrázek 15). Konkrétně každý zaměstnanec má jeden nebo více záznamů `Orders` tabulky, což způsobí odstranění k selhání.


[![Odstraňuje se zaměstnanec, který má odpovídající výsledky objednávky v porušení omezení cizího klíče](updating-the-tableadapter-to-use-joins-cs/_static/image40.png)](updating-the-tableadapter-to-use-joins-cs/_static/image39.png)

**Obrázek 15**: odstranění zaměstnanec, který má odpovídající výsledky objednávky v porušení omezení cizího klíče ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-cs/_static/image41.png))


Aby zaměstnance, který bude odstraněn vám může:

- Aktualizovat omezení cizího klíče kaskádové odstranění
- Ručně odstraňte záznamy ze `Orders` tabulka pro zaměstnance, kterou chcete odstranit, nebo
- Aktualizace `Employees_Delete` uložené procedury nejprve odstranit souvisejících záznamů z `Orders` tabulky před odstraněním `Employees` záznamu. Jsme probírali v tato technika [použití existující uložené procedury, pro zadané datové sady s objekty TableAdapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) kurzu.

Můžu ponechte toto cvičení pro čtečku.

## <a name="summary"></a>Souhrn

Při práci s relačními databázemi, je běžné dotazy na svá data z několika, o přijetí změn související tabulky. Korelační poddotazy a `JOIN` s zadejte dva různé postupy pro přístup k datům ze souvisejících tabulek v dotazu. V předchozích kurzech nejčastěji jsme použít korelační poddotazů, protože TableAdapter nelze automaticky generovat `INSERT`, `UPDATE`, a `DELETE` příkazy pro dotazy zahrnující `JOIN` s. Zatímco tyto hodnoty lze zadat ručně, při použití příkazů jazyka SQL ad-hoc všechna vlastní nastavení se přepíšou po dokončení Průvodce konfigurací TableAdapter.

Naštěstí objekty TableAdapter vytvořeny pomocí uložených procedur není mají stejné brittleness jako ty ad-hoc SQL příkazy using. Proto je vhodná k vytvoření objektu typu TableAdapter používá jehož hlavní dotaz `JOIN` při použití uložených procedur. V tomto kurzu jsme viděli, jak vytvořit takové třídy TableAdapter. Začali jsme s využitím `JOIN`– méně `SELECT` dotazovat na hlavním dotazu objektu TableAdapter s tak, aby odpovídající insert, update a delete uložené procedury bude automaticky vytvořený. TableAdapter s počáteční konfiguraci kompletní, můžeme rozšířit `SelectCommand` uložené procedury pro použití `JOIN` a znovu spustili Průvodce konfigurací TableAdapter aktualizovat `EmployeesDataTable` sloupce s.

Opětovné spuštění Průvodce konfigurací TableAdapter automaticky aktualizuje `EmployeesDataTable` sloupce tak, aby odrážely datová pole vrácené `Employees_Select` uložené procedury. Alternativně může přidali jsme tyto sloupce ručně do objektu DataTable. Se podíváme na ruční přidávání sloupců do objektu DataTable v dalším kurzu.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Hilton Geisenow, David Suru a Teresy Murphy. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [další](adding-additional-datatable-columns-cs.md)
