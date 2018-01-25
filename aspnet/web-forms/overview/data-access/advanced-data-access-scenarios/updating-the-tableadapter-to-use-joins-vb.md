---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
title: "Aktualizace pro použití TableAdapter spojí (VB) | Microsoft Docs"
author: rick-anderson
description: "Při práci s databází je běžné pro data požadavku, která je rozdělena mezi více tabulek. Načíst data ze dvou různých tabulek můžeme použít buď..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: e624a3e0-061b-4efc-8b0e-5877f9ff6714
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
msc.type: authoredcontent
ms.openlocfilehash: 2e0698269c0a29c234f03dc56f7b63e7bc83d032
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="updating-the-tableadapter-to-use-joins-vb"></a>Aktualizace pro použití TableAdapter spojí (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_VB.zip) nebo [stáhnout PDF](updating-the-tableadapter-to-use-joins-vb/_static/datatutorial69vb1.pdf)

> Při práci s databází je běžné pro data požadavku, která je rozdělena mezi více tabulek. Načíst data ze dvou různých tabulek jsme můžete použít korelační poddotazu nebo operace spojení. V tomto kurzu jsme porovnejte korelační poddotazy a syntaxe spojení před prohlížení vytváření TableAdapter, který zahrnuje spojení v jeho hlavní dotazu.


## <a name="introduction"></a>Úvod

Pomocí relačních databází je často data, která jsme zájem o práci s rozdělena mezi více tabulek. Například při zobrazení informací o produktu pravděpodobně chceme seznamu každý odpovídající kategorie produktů s a dodavatele s názvy. `Products` Tabulka má `CategoryID` a `SupplierID` hodnoty, ale skutečné názvy kategorií a dodavatele jsou v `Categories` a `Suppliers` tabulek, v uvedeném pořadí.

Pro načtení informací z jiného, související tabulky jsme můžete buď používat *korelační poddotazy* nebo `JOIN` *s*. Korelační poddotaz je vnořený `SELECT` dotaz, který odkazuje na sloupce ve vnějším dotazu. Například v [vytváření Data Access Layer](../introduction/creating-a-data-access-layer-vb.md) jsme použili dva korelační poddotazy v kurzu `ProductsTableAdapter` s hlavní dotaz vrátí název kategorie a dodavatele pro každý produkt. A `JOIN` je konstrukce SQL, která sloučí související řádky ze dvou různých tabulek. Použili jsme `JOIN` v [dotazování dat pomocí ovládacího prvku SqlDataSource](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb.md) kurzu zobrazíte informace o kategoriích vedle každého produktu.

Z důvodu jsme mít zdržel pomocí `JOIN` s s TableAdapters je z důvodu omezení v Průvodci TableAdapter s pro automatické generování odpovídající `INSERT`, `UPDATE`, a `DELETE` příkazy. Přesněji řečeno pokud hlavní dotazu TableAdapter s obsahuje některý `JOIN` s, TableAdapter nelze automaticky vytvořit příkazy SQL ad-hoc nebo uložené procedury pro jeho `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti.

V tomto kurzu stručně porovnáme a kontrast korelační poddotazy a `JOIN` s před zkoumat vytváření TableAdapter, která zahrnuje `JOIN` s v jeho hlavní dotazu.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Porovnávání a kontrastní korelační poddotazy a`JOIN` s

Odvolat, který `ProductsTableAdapter` vytvořené v z prvního kurzu `Northwind` datová sada používá korelační poddotazy tak, aby každý s odpovídající kategorii a dodavatele název produktu. `ProductsTableAdapter` s hlavním dotazu jsou uvedeny níže.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample1.sql)]

Dva korelační poddotazy - `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` a `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` -jsou `SELECT` dotazů, které vrátí jednu hodnotu na produkt jako další sloupec ve vnější `SELECT` příkaz s seznamu sloupců.

Alternativně `JOIN` lze použít k vrácení každý název dodavatele a kategorie produktu s. Následující dotaz vrátí stejné výstup jako výše, ale používá `JOIN` s místo poddotazy:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample2.sql)]

A `JOIN` slučuje záznamy z jedné tabulky se záznamy z jiné tabulky, na základě některé kritérií. Ve výše uvedeném dotazu, například `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` SQL serveru pokyn ke sloučení každý záznam produktu v kategorii záznam, jehož `CategoryID` hodnota odpovídá produktu s `CategoryID` hodnotu. Sloučené výsledek umožňuje pracovat s odpovídající pole kategorií každého produktu (například `CategoryName`).

> [!NOTE]
> `JOIN`s běžně se používají při dotazování na data z relační databáze. Pokud jste ještě `JOIN` syntaxe nebo potřeba zdokonalit v práci s trochu jeho využití, doporučujeme I d [kurzu připojení k SQL](http://www.w3schools.com/sql/sql_join.asp) v [škol W3](http://www.w3schools.com/). Také vhodné čtení jsou [ `JOIN` Základy](https://msdn.microsoft.com/library/ms191517.aspx) a [poddotazu Základy](https://msdn.microsoft.com/library/ms189575.aspx) části [dokumentaci knihy Online SQL](https://msdn.microsoft.com/library/ms130214.aspx).


Vzhledem k tomu `JOIN` s a korelační poddotazy obě slouží k načtení dat v relaci z jiné tabulky, celá řada vývojářů nezbývají hrabání jejich oznámení a vás zajímá, jaký přístup k použití. Všechny SQL gurus I sunout mluvili do mají uvedená zhruba samé, to nemá t skutečně vás performance-wise spojitá plány zhruba identické spuštění systému SQL Server. Jejich poradenství, pak je pomocí technik, které vám a vašemu týmu jsou nejvíce vyhovuje. Ho věci poznamenat, že po vodorovně tuto žádost tyto odborníky okamžitě express jejich předvolbu `JOIN` s přes korelační poddotazy.

Při sestavování Data Access Layer pomocí typové datové sady, nástroje fungují lépe při použití poddotazy. Konkrétně Průvodce nastavením TableAdapter s nebude automaticky generovat odpovídající `INSERT`, `UPDATE`, a `DELETE` příkazy Pokud hlavní dotaz obsahuje některý `JOIN` s, ale bude automaticky generovat tyto příkazy při korelační poddotazy se používají.

Chcete-li prozkoumat tohoto nedostatku, vytvořit dočasný typové datové sady v `~/App_Code/DAL` složky. Během Průvodce konfigurací TableAdapter rozhodnout používat příkazy SQL ad-hoc a zadejte následující `SELECT` dotazu (viz obrázek 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample3.sql)]


[![Zadejte hlavní dotaz, který obsahuje spojení](updating-the-tableadapter-to-use-joins-vb/_static/image2.png)](updating-the-tableadapter-to-use-joins-vb/_static/image1.png)

**Obrázek 1**: Zadejte hlavní dotaz, který obsahuje `JOIN` s ([Kliknutím zobrazit obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-vb/_static/image3.png))


Ve výchozím nastavení, se automaticky vytvoří TableAdapter `INSERT`, `UPDATE`, a `DELETE` příkazy založená na hlavní dotazu. Pokud kliknete na tlačítko Upřesnit uvidíte, že je tato funkce povolená. Bez ohledu na toto nastavení TableAdapter nebude možné vytvořit `INSERT`, `UPDATE`, a `DELETE` příkazy protože hlavním dotazu obsahuje `JOIN`.


![Zadejte hlavní dotaz, který obsahuje spojení](updating-the-tableadapter-to-use-joins-vb/_static/image4.png)

**Obrázek 2**: Zadejte hlavní dotaz, který obsahuje `JOIN` s


Kliknutím na tlačítko Dokončit ukončete průvodce. V tomto okamžiku vaše datová sada s Designer bude obsahovat jeden TableAdapter s DataTable s sloupce pro každé pole, vrátí se v `SELECT` seznamu sloupců dotazu s. To zahrnuje `CategoryName` a `SupplierName`, jak je vidět na obrázku 3.


![DataTable obsahuje sloupec pro každé pole, vrátí se v seznamu sloupců](updating-the-tableadapter-to-use-joins-vb/_static/image5.png)

**Obrázek 3**: DataTable obsahuje sloupec pro každé pole, vrátí se v seznamu sloupců


Zatímco DataTable má odpovídající sloupce, TableAdapter chybí hodnoty pro jeho `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti. Chcete-li to ověřit, klikněte na TableAdapter v návrháři a potom přejděte do okna vlastností. Existuje, uvidíte `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti jsou nastaveny na (žádný).


[![Událost InsertCommand, vlastnost UpdateCommand a DeleteCommand vlastnosti jsou nastaveny na (žádný)](updating-the-tableadapter-to-use-joins-vb/_static/image7.png)](updating-the-tableadapter-to-use-joins-vb/_static/image6.png)

**Obrázek 4**: `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti jsou nastaveny na hodnotu (None) ([Kliknutím zobrazit obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-vb/_static/image8.png))


Obejít tohoto nedostatku, můžete ručně poskytujeme příkazů SQL a parametry `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti prostřednictvím okna Vlastnosti. Alternativně může začneme nakonfigurováním hlavní dotazu TableAdapter s k *není* obsahovat žádné `JOIN` s. To vám umožní `INSERT`, `UPDATE`, a `DELETE` příkazy, čímž automaticky vygeneruje pro nás. Po dokončení průvodce, jsme může potom ručně aktualizovat TableAdapter s `SelectCommand` z okna vlastností, které zahrnuje `JOIN` syntaxe.

I když se tento přístup funguje, je velmi křehká po překonfigurovat pomocí Průvodce automaticky vygenerované pomocí dotazů ad-hoc SQL, protože kdykoli hlavní dotazu TableAdapter s `INSERT`, `UPDATE`, a `DELETE` příkazy se znovu vytvoří. To znamená, že všechny úpravy jsme provedli později by dojít ke ztrátě, pokud jsme klepli pravým tlačítkem myši na TableAdapter, zvolili konfigurace v místní nabídce a dokončit průvodce znovu.

Brittleness souboru s nastavením TableAdapter automaticky generovaný `INSERT`, `UPDATE`, a `DELETE` příkazy je naštěstí omezené na ad hoc příkazů SQL. Pokud vaše TableAdapter používá uložené procedury, můžete upravit `SelectCommand`, `InsertCommand`, `UpdateCommand`, nebo `DeleteCommand` uložené procedury a znovu spusťte Průvodce nastavením TableAdapter konfigurace bez nutnosti se obávat, že se budou uložené procedury upravit.

Přes další několik kroků vytvoříme TableAdapter, standardně používá hlavní dotaz, který vynechá žádné `JOIN` s tak, aby k odpovídající položce vložit, aktualizovat a odstranit uložené procedury bude automaticky generovaný. Pak bude aktualizován `SelectCommand` , která používá `JOIN` , vrátí další sloupce z tabulky v relaci. Nakonec jsme vytvoříte třídu odpovídající vrstvu obchodní logiky a demonstruje použití TableAdapter na webovou stránku ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Krok 1: Vytváření TableAdapter použití zjednodušené hlavní dotazu

V tomto kurzu přidáme TableAdapter a silného typu DataTable pro `Employees` tabulky v `NorthwindWithSprocs` datovou sadu. `Employees` Tabulka obsahuje `ReportsTo` pole, která zadána `EmployeeID` zaměstnanec s správce. Například zaměstnanců má Dana příjmení `ReportTo` hodnotu 5, která je `EmployeeID` z Steven Buchanan. V důsledku toho Dana sestav Steven manažerovi. Společně s reporting každý zaměstnanec s `ReportsTo` hodnotu, jsme také chtít získat název svého správce. Můžete to provést pomocí `JOIN`. Ale pomocí `JOIN` při vytváření původně TableAdapter vylučuje Průvodce automatického generování odpovídající vložení, aktualizace a odstranění možnosti. Proto bude začneme vytvořením TableAdapter jejichž hlavní dotazu neobsahuje žádné `JOIN` s. Potom v kroku 2, budeme aktualizovat hlavní dotazu uložený postup získat název manager s prostřednictvím `JOIN`.

Začněte otevřením `NorthwindWithSprocs` datovou sadu v `~/App_Code/DAL` složky. Klikněte pravým tlačítkem na návrháře, vyberte možnost přidat v místní nabídce a vyberte položku nabídky TableAdapter. Tím se spustí Průvodce nastavením TableAdapter konfigurací. Jak znázorňuje obrázek 5, aby průvodce vytvořit nové uložené procedury a klikněte na tlačítko Další. Pro aktualizační program na vytváření nových uložené procedury z Průvodce nastavením TableAdapter s, najdete [vytváření nové uložených procedur, pro typové datové sady s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) kurzu.


[![Vyberte nové uložené procedury vytvořit možnost](updating-the-tableadapter-to-use-joins-vb/_static/image10.png)](updating-the-tableadapter-to-use-joins-vb/_static/image9.png)

**Obrázek 5**: Vyberte vytvořením nové uložené procedury možnost ([Kliknutím zobrazit obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-vb/_static/image11.png))


Použijte následující `SELECT` příkaz dotazu TableAdapter s hlavní:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample4.sql)]

Vzhledem k tomu, že tento dotaz neobsahuje žádné `JOIN` s, TableAdapter průvodce automaticky vytvoří uložené procedury s odpovídajícími `INSERT`, `UPDATE`, a `DELETE` příkazy, stejně jako uložené procedury pro provádění hlavní dotaz.

Následující krok umožňuje název TableAdapter s uložené procedury. Použít názvy `Employees_Select`, `Employees_Insert`, `Employees_Update`, a `Employees_Delete`, jak je znázorněno na obrázku 6.


[![Název TableAdapter s uložené procedury](updating-the-tableadapter-to-use-joins-vb/_static/image13.png)](updating-the-tableadapter-to-use-joins-vb/_static/image12.png)

**Obrázek 6**: název uložené procedury s TableAdapter ([Kliknutím zobrazit obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-vb/_static/image14.png))


V posledním kroku vyzve nám název metody TableAdapter s. Použití `Fill` a `GetEmployees` jako názvy metoda. Nezapomeňte také ponechat vytvořit metody k odeslání aktualizací přímo do databáze (generatedbdirectmethods –) políčko zaškrtnuto.


[![Název výplně TableAdapter s metody a GetEmployees](updating-the-tableadapter-to-use-joins-vb/_static/image16.png)](updating-the-tableadapter-to-use-joins-vb/_static/image15.png)

**Obrázek 7**: název TableAdapter s metody `Fill` a `GetEmployees` ([Kliknutím zobrazit obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-vb/_static/image17.png))


Po dokončení průvodce, pozorně zkontrolujte uložené procedury v databázi. Měli byste vidět čtyři nové: `Employees_Select`, `Employees_Insert`, `Employees_Update`, a `Employees_Delete`. Dále zkontrolujte `EmployeesDataTable` a `EmployeesTableAdapter` právě vytvořili. DataTable obsahuje sloupec pro každé pole vrácené dotazem hlavní. Klikněte na TableAdapter a potom přejděte do okna vlastností. Existuje, uvidíte `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti jsou správně nakonfigurovány k volání odpovídající uložené procedury.


[![TableAdapter obsahuje příkaz Insert, Update a odstranit možnosti](updating-the-tableadapter-to-use-joins-vb/_static/image19.png)](updating-the-tableadapter-to-use-joins-vb/_static/image18.png)

**Obrázek 8**: TableAdapter zahrnuje vložení, aktualizace a odstranění možnosti ([Kliknutím zobrazit obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-vb/_static/image20.png))


S vložit, aktualizovat a odstraňovat uložené procedury automaticky vytvoří a `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti správně nakonfigurovaný, jsme připraveni k přizpůsobení `SelectCommand` s uložené procedury vrátit další informace o jednotlivých zaměstnanců s správci. Konkrétně je potřeba aktualizovat `Employees_Select` uložené procedury sloužící `JOIN` a vrátíte se správce s `FirstName` a `LastName` hodnoty. Po aktualizaci uložené procedury, budeme muset aktualizovat DataTable tak, že obsahují tyto další sloupce. Jsme budete řešení tyto dvě úlohy v kroky 2 a 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Krok 2: Přizpůsobení uložené procedury, která patří`JOIN`

Start, přejděte do Průzkumníka serveru, procházení k podrobnostem do složky Northwind databáze s uložené procedury a otevírání `Employees_Select` uložené procedury. Pokud nevidíte tuto uloženou proceduru, klikněte pravým tlačítkem na složku, uložené procedury a zvolte aktualizace. Aktualizace uložené procedury tak, aby používala `LEFT JOIN` nejprve vrátit správce s a příjmení:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample5.sql)]

Po aktualizaci `SELECT` příkaz Uložit změny přechodem do nabídky soubor a zvolením Uložit `Employees_Select`. Případně můžete kliknutím na ikonu Uložit na panelu nástrojů nebo stiskněte kombinaci kláves Ctrl + S. Po uložení změn, klikněte pravým tlačítkem na `Employees_Select` uložené procedury v Průzkumníku serveru a zvolte spustit. Tato akce spuštění uložené procedury a zobrazit své výsledky v okně výstupu (viz obrázek 9).


[![Výsledky uložené procedury jsou zobrazeny v okně výstupu](updating-the-tableadapter-to-use-joins-vb/_static/image22.png)](updating-the-tableadapter-to-use-joins-vb/_static/image21.png)

**Obrázek 9**: uložené procedury výsledky se zobrazí v okně výstupu ([Kliknutím zobrazit obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-vb/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>Krok 3: Aktualizace sloupců s DataTable

V tomto okamžiku `Employees_Select` uložené procedury vrátí `ManagerFirstName` a `ManagerLastName` hodnoty, ale `EmployeesDataTable` chybí tyto sloupce. Tyto chybějící sloupce lze přidat do DataTable v jednom ze dvou způsobů:

- **Ručně** – klikněte pravým tlačítkem na DataTable v Návrháři DataSet a z nabídky přidat, zvolte sloupce. Můžete pak zadejte název sloupce a odpovídajícím způsobem nastavit jeho vlastnosti.
- **Automaticky** – Průvodce konfigurací TableAdapter bude aktualizovat sloupce s DataTable tak, aby odrážela pole vrácené `SelectCommand` uložené procedury. Při použití příkazů SQL ad-hoc, budou odebrány také Průvodce `InsertCommand`, `UpdateCommand`, a `DeleteCommand` vlastnosti od `SelectCommand` nyní obsahuje `JOIN`. Ale při použití uložených procedur, tyto vlastnosti příkaz zůstanou beze změn.

Ruční přidání sloupce DataTable jsme jste prozkoumali v předchozí kurzy, včetně [hlavní/podrobností s odrážkami seznamu v hlavní záznamů pomocí DataList podrobnosti](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) a [nahrávání souborů](../working-with-binary-files/uploading-files-vb.md), a provedeme Podívejte se na tento proces znovu podrobněji v našem kurzu Další. V tomto kurzu ale umožní používat automatické přístup prostřednictvím Průvodce konfigurací TableAdapter s.

Začněte tím, že pravým tlačítkem myši na `EmployeesTableAdapter` a výběrem konfigurace v místní nabídce. Otevře Průvodce konfigurací TableAdapter, kde jsou uvedeny uložené procedury používané pro výběr, vkládání, aktualizaci a odstraňování spolu s jejich návratové hodnoty a parametry (pokud existuje). Obrázek 10 ukazuje tohoto průvodce. Zde jsme uvidíte, že `Employees_Select` uložené procedury nyní vrátí `ManagerFirstName` a `ManagerLastName` pole.


[![Průvodce zobrazí v seznamu aktualizované sloupce Employees_Select uložené procedury](updating-the-tableadapter-to-use-joins-vb/_static/image25.png)](updating-the-tableadapter-to-use-joins-vb/_static/image24.png)

**Obrázek 10**: Průvodce zobrazí seznam aktualizovat sloupec pro `Employees_Select` uloženou proceduru ([Kliknutím zobrazit obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-vb/_static/image26.png))


Dokončete průvodce kliknutím na tlačítko Dokončit. Při vrácení k návrháře DataSet `EmployeesDataTable` obsahuje dva další sloupce: `ManagerFirstName` a `ManagerLastName`.


[![EmployeesDataTable obsahuje dva nové sloupce](updating-the-tableadapter-to-use-joins-vb/_static/image28.png)](updating-the-tableadapter-to-use-joins-vb/_static/image27.png)

**Obrázek 11**: `EmployeesDataTable` obsahuje dva nové sloupce ([Kliknutím zobrazit obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-vb/_static/image29.png))


Pro ilustraci, aktualizovaná `Employees_Select` uložené procedury je v platnosti, vložit, aktualizovat a odstraňovat možnosti TableAdapter je funkční, a umožní s vytvořit webovou stránku, která umožňuje uživatelům zobrazit a odstranění zaměstnanci. Vytvoříme tyto stránek, ale musíme nejprve vytvořte novou třídu v vrstvu obchodní logiky pro práci se zaměstnanci `NorthwindWithSprocs` datovou sadu. V kroku 4, vytvoříme `EmployeesBLLWithSprocs` třídy. V kroku 5 budeme používat tato třída ze stránky ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Krok 4: Implementace vrstvu obchodní logiky

Vytvořte nový soubor – třída v `~/App_Code/BLL` složku s názvem `EmployeesBLLWithSprocs.vb`. Tato třída napodobuje sémantika existující `EmployeesBLL` třídy, tento nový jeden poskytuje méně metody a používá `NorthwindWithSprocs` datovou sadu (místo `Northwind` datové sady). Přidejte následující kód, který `EmployeesBLLWithSprocs` třídy.


[!code-vb[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample6.vb)]

`EmployeesBLLWithSprocs` Třídu s `Adapter` vlastnost vrací instanci třídy `NorthwindWithSprocs` datovou sadu s `EmployeesTableAdapter`. Toto je používáno třídu s `GetEmployees` a `DeleteEmployee` metody. `GetEmployees` Volání metod `EmployeesTableAdapter` s odpovídající `GetEmployees` metodu, která volá `Employees_Select` uložené procedury a naplní své výsledky v `EmployeeDataTable`. `DeleteEmployee` Metoda podobně volá `EmployeesTableAdapter` s `Delete` metodu, která volá `Employees_Delete` uložené procedury.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Krok 5: Práce s daty v prezentační vrstvě

Pomocí `EmployeesBLLWithSprocs` třída dokončení jsme re připravené k práci s daty zaměstnanec prostřednictvím stránky ASP.NET. Otevřete `JOINs.aspx` stránku `AdvancedDAL` složku a přetáhněte GridView z panelu nástrojů na návrháře nastavení jeho `ID` vlastnost, která má `Employees`. V dalším kroku ze inteligentní značky GridView s vazby mřížce do ovládacího prvku ObjectDataSource nové s názvem `EmployeesDataSource`.

Konfigurace ObjectDataSource používat `EmployeesBLLWithSprocs` třídy a z karty vyberte a odstranit, ujistěte se, že `GetEmployees` a `DeleteEmployee` metody jsou vybraný z rozevíracího seznamu. Klikněte na tlačítko Dokončit k dokončení konfigurace s ObjectDataSource.


[![Konfigurace ObjectDataSource použití třídy EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-vb/_static/image31.png)](updating-the-tableadapter-to-use-joins-vb/_static/image30.png)

**Obrázek 12**: Konfigurace ObjectDataSource pro použití `EmployeesBLLWithSprocs` – třída ([Kliknutím zobrazit obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-vb/_static/image32.png))


[![Mají použití ObjectDataSource GetEmployees a DeleteEmployee metody](updating-the-tableadapter-to-use-joins-vb/_static/image34.png)](updating-the-tableadapter-to-use-joins-vb/_static/image33.png)

**Obrázek 13**: mít použití ObjectDataSource `GetEmployees` a `DeleteEmployee` metody ([Kliknutím zobrazit obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-vb/_static/image35.png))


Visual Studio se přidá BoundField do GridView pro každou z `EmployeesDataTable` s sloupce. Odeberte všechny tyto BoundFields s výjimkou `Title`, `LastName`, `FirstName`, `ManagerFirstName`, a `ManagerLastName` a přejmenujte `HeaderText` vlastnosti pro poslední čtyři BoundFields příjmení, křestní jméno, křestní jméno správce s, a Správce s příjmení, v uvedeném pořadí.

Povolit uživatelům odstranit zaměstnanci z této stránky je potřeba udělat dvě věci. Požádejte nejprve GridView zajistit odstraňování možnosti kontrolou možnost Povolit odstranění z jeho inteligentních značek. Druhý, změnit ObjectDataSource s `OldValuesParameterFormatString` vlastnost z hodnoty nastavit průvodcem ObjectDataSource (`original_{0}`) na výchozí hodnotu (`{0}`). Po provedení těchto změn, vaše GridView a ObjectDataSource s deklarativní by měl vypadat takto:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample7.aspx)]

Otestování stránce návštěvou prostřednictvím prohlížeče. Jak ukazuje obrázek 14, stránky se zobrazí seznam každý zaměstnanec a své manager s názvem (za předpokladu, že mají jednu).


[![SPOJENÍ v Employees_Select uložené procedury vrátí název s Manager](updating-the-tableadapter-to-use-joins-vb/_static/image37.png)](updating-the-tableadapter-to-use-joins-vb/_static/image36.png)

**Obrázek 14**: `JOIN` v `Employees_Select` uložený postup vrátí Manager s názvem ([Kliknutím zobrazit obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-vb/_static/image38.png))


Kliknutím na tlačítko Odstranit spuštění pracovního postupu, odstraňování, které culminates za běhu `Employees_Delete` uložené procedury. Ale pokus o `DELETE` příkaz v uložené proceduře nezdaří z důvodu narušení omezení cizího klíče (viz obrázek 15). Konkrétně každý zaměstnanec má jeden nebo více záznamů `Orders` tabulky, způsobí odstranění k selhání.


[![Odstraňování zaměstnanec, který má odpovídající objednávky výsledky v narušení omezení pro cizí klíč](updating-the-tableadapter-to-use-joins-vb/_static/image40.png)](updating-the-tableadapter-to-use-joins-vb/_static/image39.png)

**Obrázek 15**: odstranění zaměstnanec, který má odpovídající objednávky výsledky v narušení omezení pro cizí klíč ([Kliknutím zobrazit obrázek v plné velikosti](updating-the-tableadapter-to-use-joins-vb/_static/image41.png))


Povolit zaměstnance, který bude odstraněn, vám může:

- Aktualizace omezení cizího klíče kaskádové odstranění,
- Ručně odstraňte záznamy ze `Orders` tabulky pro zaměstnance, kterou chcete odstranit, nebo
- Aktualizace `Employees_Delete` uložené procedury nejprve odstranit související záznamy z `Orders` tabulky před odstraněním `Employees` záznamu. Jsme probírali v tento postup [použití existující uložené procedury, pro typové datové sady s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) kurzu.

Nechat to jako cvičení pro čtečku.

## <a name="summary"></a>Souhrn

Při práci s relačních databází, je běžné dotazy načítat data z více, související tabulky. Korelační poddotazy a `JOIN` s zadejte dvě různé metody pro přístup k datům ze souvisejících tabulek v dotazu. V předchozích kurzy jsme provedli nejčastěji použít korelační poddotazů, protože TableAdapter nelze automaticky generovat `INSERT`, `UPDATE`, a `DELETE` příkazy pro dotazy zahrnující `JOIN` s. Při tyto hodnoty lze zadat ručně, při použití příkazů SQL ad-hoc po dokončení Průvodce konfigurací TableAdapter se přepíší všechna vlastní nastavení.

Naštěstí TableAdapters vytvořené pomocí uložené procedury se nebude vyskytovat ze stejné brittleness jako vytvořené pomocí příkazů SQL ad hoc. Proto je vhodná k vytváření TableAdapter jejichž hlavní dotaz používá `JOIN` při použití uložených procedur. V tomto kurzu jsme viděli, jak vytvořit takové TableAdapter. Spuštění pomocí `JOIN`-menší `SELECT` dotazu TableAdapter s hlavním dotazu tak, aby odpovídající insert, update a delete uložené procedury bude vytvořen automaticky. TableAdapter s počáteční dokončení konfigurace, jsme rozšířen `SelectCommand` uložené procedury sloužící `JOIN` a znovu se spustil Průvodce konfigurací TableAdapter aktualizovat `EmployeesDataTable` sloupce s.

Opětovné spuštění Průvodce konfigurací TableAdapter automaticky aktualizuje `EmployeesDataTable` sloupce tak, aby odrážela datová pole vrácené `Employees_Select` uložené procedury. Alternativně jsme může přidali tyto sloupce ručně do DataTable. Se podíváme na ruční přidávání sloupců do DataTable v dalším kurzu.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Hilton Geisenow David Suru a Teresy Murphy. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
[další](adding-additional-datatable-columns-vb.md)
