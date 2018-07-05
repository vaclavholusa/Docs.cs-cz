---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
title: Práce s vypočtenými sloupci (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Při vytváření tabulky databáze Microsoft SQL Server umožňuje definovat počítaný sloupec, jehož hodnota se počítá z výrazu, který obvykle referen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 57459065-ed7c-4dfe-ac9c-54c093abc261
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 96a6411fe72e044b8f35091192ecaaac0d376349
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385713"
---
<a name="working-with-computed-columns-c"></a>Práce s vypočtenými sloupci (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_CS.zip) nebo [stahovat PDF](working-with-computed-columns-cs/_static/datatutorial71cs1.pdf)

> Při vytváření tabulky databáze Microsoft SQL serveru můžete zadat počítaný sloupec, jehož hodnota se počítá od výraz, který obvykle odkazuje na jiné hodnoty do stejného databázového záznamu. Tyto hodnoty jsou jen pro čtení na databázi, která vyžaduje zvláštní aspekty při práci s objekty TableAdapter. V tomto kurzu naučíte čelit výzvám kompenzaci vypočítané sloupce.


## <a name="introduction"></a>Úvod

Microsoft SQL Server umožňuje  *[vypočítané sloupce](https://msdn.microsoft.com/library/ms191250.aspx)*, které jsou sloupce, jejichž hodnoty se počítají na základě výraz, který obvykle odkazuje na hodnoty z jiných sloupců stejné tabulky. Jako příklad může mít čas datového modelu pro sledování tabulku s názvem `ServiceLog` se sloupci včetně `ServicePerformed`, `EmployeeID`, `Rate`, a `Duration`, mimo jiné. Zatímco dlužná částka za služby položky (rychlost vynásobené DURATION) může být počítá prostřednictvím webové stránky nebo jiných programových rozhraní, může být po ruce, aby zahrnovaly sloupec v `ServiceLog` tabulku s názvem `AmountDue` který nahlásit to informace. Tento sloupec nemohl být vytvořen jako normální sloupec, ale třeba kdykoli aktualizovat `Rate` nebo `Duration` změnit hodnoty sloupce. Lepším řešením může být Ujistěte se, `AmountDue` sloupec počítaný sloupec pomocí výrazu `Rate * Duration`. To by způsobilo systému SQL Server se automaticky vypočítá `AmountDue` hodnota ve sloupci pokaždé, když na ni bylo odkazováno v dotazu.

Protože počítaný sloupec s hodnotou je určeno výrazem, tyto sloupce jsou jen pro čtení, a proto nelze hodnoty jim přiřadili v `INSERT` nebo `UPDATE` příkazy. Ale když počítané sloupce jsou součástí hlavním dotazu objektu TableAdapter, který používá příkazy SQL ad-hoc, jsou automaticky součástí automaticky generovanou `INSERT` a `UPDATE` příkazy. V důsledku toho s TableAdapter `INSERT` a `UPDATE` dotazy a `InsertCommand` a `UpdateCommand` vlastnosti se musí aktualizovat na odebrat odkazy na žádné vypočítané sloupce.

Problémy může vyvolávat použití vypočítané sloupce pomocí prvku TableAdapter, který používá příkazy SQL ad-hoc, který je TableAdapter s `INSERT` a `UPDATE` dotazy jsou automaticky obnovovány kdykoli dokončení Průvodce konfigurací TableAdapter. Proto počítané sloupce se ručně odebrat z `INSERT` a `UPDATE` dotazy se znovu zobrazí, pokud je znovu spusťte průvodce. Ačkoli objekty TableAdapter, které používají uložené procedury nejsou t trpí tento brittleness, mají své vlastní Adaptivní režim, které nám budou řešit v kroku 3.

V tomto kurzu přidáme počítaný sloupec, `Suppliers` tabulky v databázi Northwind a pak vytvořte odpovídající TableAdapter pro práci s touto tabulkou a jeho počítaný sloupec. Máme naši TableAdapter pomocí uložených procedur místo SQL příkazy ad-hoc tak, aby naše t přizpůsobení nejsou ztracena při použití Průvodce konfigurací TableAdapter.

Začínáme s let!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Krok 1: Přidání vypočítaného sloupce do`Suppliers`tabulky

Databáze Northwind nemá žádné vypočítané sloupce, takže bude potřeba přidat jeden sami. Pro tento kurz nechat přidat počítaný sloupec s `Suppliers` tabulce nazvané `FullContactName` , který vrátí jméno kontaktu s, název a fungují pro společnost v následujícím formátu: `ContactName` (`ContactTitle`, `CompanyName`). Tento počítaný sloupec může použít v sestavách, při zobrazení informací o dodavatelů.

Začněte otevřením `Suppliers` definice tabulky kliknutím pravým tlačítkem na `Suppliers` v Průzkumníku serveru tabulku a zvolíte Otevřít definici tabulky v místní nabídce. Zobrazí se sloupce v tabulce a jejich vlastností, jako je datový typ, zda povolit `NULL` s a tak dále. Chcete-li přidat počítaný sloupec, začněte zadáním názvu sloupce v definici tabulky. Dále zadejte jeho výraz do textového pole (vzorec) části specifikace vypočítaného sloupce v okně Vlastnosti sloupce (viz obrázek 1). Název počítaného sloupce `FullContactName` a použijte následující výraz:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample1.sql)]

Všimněte si, že by řetězců mohou být spojeny v SQL pomocí `+` operátor. `CASE` Příkaz lze použít jako podmíněné v některém tradičním programovacím jazyce. Ve výše uvedeném výrazu `CASE` příkaz lze přečíst jako: Pokud `ContactTitle` není `NULL` ve výstupu `ContactTitle` hodnota zřetězená s čárkou, jinak generovat žádnou akci. Další informace o užitečnost `CASE` příkaz, naleznete v tématu [Power SQL `CASE` příkazy](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Namísto použití `CASE` prohlášení, můžou také použili jsme `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) Vrátí *checkExpression* Pokud to je jiná než NULL, jinak vrátí *zastaralá*. While buď `ISNULL` nebo `CASE` bude fungovat v tomto případě existují komplikovanější scénáře kde flexibilitu `CASE` příkazu nesmí odpovídat podvýrazu `ISNULL`.


Po přidání Tento počítaný sloupec vaše obrazovka by měla vypadat obrazovky na obrázku 1.


[![Přidejte počítaný sloupec s názvem FullContactName k tabulce dodavatelů](working-with-computed-columns-cs/_static/image2.png)](working-with-computed-columns-cs/_static/image1.png)

**Obrázek 1**: přidejte počítaný sloupec s názvem `FullContactName` k `Suppliers` tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](working-with-computed-columns-cs/_static/image3.png))


Po pojmenování počítaný sloupec a jeho výrazu, uložte změny do tabulky a kliknutím na ikonu Uložit na panelu nástrojů, stisknutím Ctrl + S nebo tak, že přejdete do nabídky soubor a zvolíte Uložit `Suppliers`.

Ukládá se tabulka by měl aktualizovat Průzkumníka serveru, včetně právě přidán sloupec v `Suppliers` seznam sloupců tabulky s. Kromě toho výraz do textové pole (vzorce) automaticky přizpůsobí ekvivalentní výraz, který odstraní nepotřebné prázdné znaky, zobrazí se kolem názvy sloupců s hranaté závorky (`[]`) a obsahuje závorky, chcete-li zobrazit více explicitně pořadí operací:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample2.sql)]

Další informace o vypočítané sloupce v systému Microsoft SQL Server, najdete [technické dokumentace](https://msdn.microsoft.com/library/ms191250.aspx). Také si přečtěte [postupy: určení vypočítaného sloupce](https://msdn.microsoft.com/library/ms188300.aspx) podrobný názorný postup vytvoření počítaného sloupce.

> [!NOTE]
> Ve výchozím nastavení vypočítané sloupce nejsou fyzicky uložené v tabulce, ale místo toho bude vždy přepočítána, které jsou odkazovány v dotazu. Zaškrtnutím políčka se jako trvalý, ale můžete dát pokyn, SQL Server k ukládání fyzicky počítaný sloupec v tabulce. Díky tomu má být vytvořena na počítaný sloupec, což může zlepšit výkon dotazů, které používají hodnotu počítaný sloupec v indexu jejich `WHERE` klauzule. Zobrazit [vytváření indexů pro vypočítané sloupce](https://msdn.microsoft.com/library/ms189292.aspx) Další informace.


## <a name="step-2-viewing-the-computed-column-s-values"></a>Krok 2: Zobrazení hodnot s počítaný sloupec

Než začneme práce na vrstvy přístupu k datům, umožňují s trvat několik minut zobrazíte `FullContactName` hodnoty. Z Průzkumníka serveru, klikněte pravým tlačítkem na `Suppliers` název tabulky a v místní nabídce zvolte nový dotaz. Tím se otevře okno s dotazem, který vyzve se k nám se rozhodnout, jaké tabulky v dotazu. Přidat `Suppliers` tabulky a klikněte na tlačítko Zavřít. Dalším krokem je kontrola `CompanyName`, `ContactName`, `ContactTitle`, a `FullContactName` sloupce z tabulky. Nakonec klikněte na ikonu červeného vykřičníku na panelu nástrojů k provedení dotazu a zobrazit výsledky.

Jak je vidět na obrázku 2, budou výsledky obsahovat `FullContactName`, se seznamem `CompanyName`, `ContactName`, a `ContactTitle` sloupců pomocí formátu možnosti;`ContactName` (`ContactTitle`, `CompanyName`) .


[![FullContactName používá formát ContactName (funkce, CompanyName)](working-with-computed-columns-cs/_static/image5.png)](working-with-computed-columns-cs/_static/image4.png)

**Obrázek 2**: `FullContactName` používá formát `ContactName` (`ContactTitle`, `CompanyName`) ([kliknutím ji zobrazíte obrázek v plné velikosti](working-with-computed-columns-cs/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Krok 3: Přidání`SuppliersTableAdapter`do vrstvy přístupu k datům

Chcete-li pracovat s informacemi o dodavatele v naší aplikaci musíme nejprve vytvořit TableAdapter a DataTable v našich vrstvy DAL. V ideálním případě by to jejich provedení pomocí stejný jednoduchý postup prozkoumat v předchozích kurzech. Práce s vypočtenými sloupci však zavádí několik vráskami, které přicházejí diskuse.

Pokud používáte TableAdapter, který používá příkazy SQL ad-hoc, můžete jednoduše zahrnout počítaný sloupec v hlavním dotazu objektu TableAdapter s prostřednictvím Průvodce konfigurací TableAdapter. To však automaticky vygeneruje `INSERT` a `UPDATE` příkazy, které obsahují počítaný sloupec. Při pokusu provést jednu z těchto metod `SqlException` zprávou sloupci *Názevsloupce* nejde upravit, protože je počítaný sloupec nebo je výsledkem operátoru UNION, bude vyvolána. Zatímco `INSERT` a `UPDATE` příkaz je možné ručně upravit pomocí TableAdapter s `InsertCommand` a `UpdateCommand` vlastnosti, tato přizpůsobení budou ztraceny pokaždé, když se Průvodce nastavením TableAdapter je spouštět znovu.

Z důvodu brittleness objekty TableAdapter, použít SQL příkazy ad-hoc se doporučuje, můžeme pomocí uložených procedur při práci s vypočítanými sloupci. Pokud používáte existující uložené procedury, jednoduše nakonfigurujte TableAdapter jak je popsáno v [použití existující uložené procedury, pro zadané datové sady s objekty TableAdapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) kurzu. Pokud máte TableAdapter průvodce vytvořit úložné procedury pro vás, ale je potřeba zpočátku vynechat žádné vypočítané sloupce z hlavního dotazu. Pokud zahrnete počítaný sloupec v hlavním dotazu, Průvodce konfigurací TableAdapter bude informovat, po dokončení, nelze vytvořit odpovídající uložené procedury. Stručně řečeno, musíme nejprve nakonfigurujte TableAdapter pomocí počítaného sloupce bez hlavního dotazu a potom ručně aktualizovat odpovídající uložené procedury a TableAdapter s `SelectCommand` zahrnout počítaný sloupec. Tento přístup je podobný tomu použitému v [aktualizace komponenty TableAdapter kvůli použití](updating-the-tableadapter-to-use-joins-cs.md)`JOIN`*s* kurzu.

Pro účely tohoto kurzu nechte s přidáním nového TableAdapter a automaticky vytvořit úložné procedury pro nás. V důsledku toho budeme muset nejdřív vynechat, nechte `FullContactName` počítaného sloupce z hlavního dotazu.

Začněte otevřením `NorthwindWithSprocs` datovou sadu v `~/App_Code/DAL` složky. Klikněte pravým tlačítkem v návrháři a v kontextové nabídce zvolte možnost pro přidání nového TableAdapter. Tím spustíte Průvodce konfigurací TableAdapter. Zadejte dotaz na data z databáze (`NORTHWNDConnectionString` z `Web.config`) a klikněte na tlačítko Další. Protože ještě nevytvořili žádné uložené procedury pro dotazování nebo úpravě `Suppliers` tabulku, vyberte možnost vytvořit nové uložené procedury možnost tak, aby tento průvodce je vytvořit pro nás a klikněte na tlačítko Další.


[![Zvolte možnost vytvořit nové uložené procedury možnost](working-with-computed-columns-cs/_static/image8.png)](working-with-computed-columns-cs/_static/image7.png)

**Obrázek 3**: Zvolte vytvořit nové uložené procedury možnost ([kliknutím ji zobrazíte obrázek v plné velikosti](working-with-computed-columns-cs/_static/image9.png))


Následný krok nám vyzve k zadání hlavního dotazu. Zadejte následující dotaz, který vrátí `SupplierID`, `CompanyName`, `ContactName`, a `ContactTitle` sloupce pro každého dodavatele. Všimněte si, že tento dotaz záměrně vynechá počítaný sloupec (`FullContactName`); aktualizujeme odpovídající uložené procedury, která patří tento sloupec v kroku 4.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample3.sql)]

Po zadání hlavního dotazu a kliknutím na tlačítko Další, ho přes Průvodce zřídit nám čtyři uložené procedury, které budou generovat název. Název těchto uložených procedurách `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, a `Suppliers_Delete`, jak ukazuje obrázek 4.


[![Přizpůsobení názvů automaticky generované uložené procedury](working-with-computed-columns-cs/_static/image11.png)](working-with-computed-columns-cs/_static/image10.png)

**Obrázek 4**: Přizpůsobení názvů Auto-Generated uložené procedury ([kliknutím ji zobrazíte obrázek v plné velikosti](working-with-computed-columns-cs/_static/image12.png))


V dalším kroku průvodce umožňuje nám zadat tyto vzory se dají použít k přístupu a aktualizace dat a název metody třídy TableAdapter s. Ponechat všechny tři zaškrtnutých políček, ale přejmenovat `GetData` metodu `GetSuppliers`. Kliknutím na Dokončit dokončíte průvodce.


[![Přejmenovat na GetSuppliers GetData – metoda](working-with-computed-columns-cs/_static/image14.png)](working-with-computed-columns-cs/_static/image13.png)

**Obrázek 5**: Přejmenovat `GetData` metodu `GetSuppliers` ([kliknutím ji zobrazíte obrázek v plné velikosti](working-with-computed-columns-cs/_static/image15.png))


Po kliknutí na tlačítko Dokončit, bude průvodce vytvořit čtyři uložené procedury a přidat do datové sady typu TableAdapter a odpovídající objekt DataTable.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Krok 4: Včetně počítaný sloupec v hlavním dotazu objektu TableAdapter s

Nyní potřebujeme k aktualizaci objektu TableAdapter a DataTable vytvořený v kroku 3, které chcete zahrnout `FullContactName` počítaný sloupec. To zahrnuje dva kroky:

1. Aktualizuje `Suppliers_Select` uloženou proceduru se vraťte `FullContactName` počítanému sloupci, a
2. Aktualizuje se objekt DataTable zahrnout odpovídající `FullContactName` sloupce.

Začněte tím, že přejdete do Průzkumníka serveru a procházení k podrobnostem složky uložené procedury. Otevřít `Suppliers_Select` uloženou proceduru včetně aktualizace `SELECT` dotaz pro přidání `FullContactName` počítaný sloupec:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample4.sql)]

Uložte změny do uložené procedury, kliknutím na ikonu Uložit na panelu nástrojů, stisknutím Ctrl + S nebo kliknutím Uložit `Suppliers_Select` možnost z nabídky soubor.

Pak se vraťte do návrháře datových sad, klikněte pravým tlačítkem na `SuppliersTableAdapter`a zvolte možnost konfigurace v místní nabídce. Všimněte si, že `Suppliers_Select` teď obsahuje sloupec `FullContactName` sloupce v kolekci jeho datových sloupců.


[![Spusťte Průvodce konfigurací s TableAdapter aktualizovat sloupce s DataTable](working-with-computed-columns-cs/_static/image17.png)](working-with-computed-columns-cs/_static/image16.png)

**Obrázek 6**: Spusťte TableAdapter s Průvodce konfigurací a Update DataTable s sloupce ([kliknutím ji zobrazíte obrázek v plné velikosti](working-with-computed-columns-cs/_static/image18.png))


Kliknutím na Dokončit dokončíte průvodce. Tím se automaticky přidá odpovídající sloupec, který `SuppliersDataTable`. Průvodci vytvořením objektu TableAdapter je dostatečně inteligentní, chcete-li zjistit, který `FullContactName` sloupec je počítaný sloupec, takže je jen pro čtení. V důsledku toho, nastaví hlavičku sloupce s `ReadOnly` vlastnost `true`. Chcete-li to ověřit, vyberte sloupec, ze `SuppliersDataTable` a přejděte do okna vlastností (viz obrázek 7). Všimněte si, že `FullContactName` sloupec s `DataType` a `MaxLength` vlastnosti jsou nastaveny také odpovídajícím způsobem.


[![FullContactName sloupec je označen jen pro čtení](working-with-computed-columns-cs/_static/image20.png)](working-with-computed-columns-cs/_static/image19.png)

**Obrázek 7**: `FullContactName` sloupec je označen jen pro čtení ([kliknutím ji zobrazíte obrázek v plné velikosti](working-with-computed-columns-cs/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Krok 5: Přidání`GetSupplierBySupplierID`metoda do TableAdapter

Pro účely tohoto kurzu vytvoříme stránky ASP.NET, která zobrazuje dodavatelů aktualizovatelné mřížky. V posledních kurzy aktualizovali jsme jeden záznam z vrstvy obchodní logiky načtením, že konkrétní záznam z vrstvy DAL jako DataTable silného typu, aktualizuje jeho vlastnosti a potom odešlete aktualizovaný DataTable zpět do vrstvy DAL šíření změn databáze. K provedení tento první krok - načítání záznamu aktualizováno z hodnoty DAL - musíme nejprve přidat `GetSupplierBySupplierID(supplierID)` metoda vrstvy Dal.

Klikněte pravým tlačítkem na `SuppliersTableAdapter` v návrhu datové sady a v místní nabídce zvolte možnost přidat dotaz. Jako jsme to udělali v kroku 3, umožněte průvodci vytvořit novou úložnou proceduru pro nás tak, že vyberete možnost vytvořit novou úložnou proceduru (vrátit zpět k obrázek 3 pro snímek obrazovky tohoto průvodce kroku). Protože tato metoda vrátí záznam s více sloupců, označuje, že chceme použít dotaz SQL, který je s výběrem, který vrátí řádky a klikněte na tlačítko Další.


[![Zvolte, který vrátí řádky možnost](working-with-computed-columns-cs/_static/image23.png)](working-with-computed-columns-cs/_static/image22.png)

**Obrázek 8**: Zvolte, který vrátí řádky možnost ([kliknutím ji zobrazíte obrázek v plné velikosti](working-with-computed-columns-cs/_static/image24.png))


Následný krok vyzve k nám na dotaz, který chcete použít pro tuto metodu. Zadejte následující příkaz, který vrací stejné datová pole jako hlavní dotaz, ale pro konkrétní dodavatele.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample5.sql)]

Na další obrazovce dotazem, abychom uložené procedury, které se bude automaticky generovaný název. Pojmenujte tuto uloženou proceduru `Suppliers_SelectBySupplierID` a klikněte na tlačítko Další.


[![Název uložené procedury Suppliers_SelectBySupplierID](working-with-computed-columns-cs/_static/image26.png)](working-with-computed-columns-cs/_static/image25.png)

**Obrázek 9**: název uložená procedura `Suppliers_SelectBySupplierID` ([kliknutím ji zobrazíte obrázek v plné velikosti](working-with-computed-columns-cs/_static/image27.png))


A konečně Průvodce pokynů nám dat přístup vzory a názvy metod pro TableAdapter. Ponechte obou zaškrtnutých políček, ale přejmenovat `FillBy` a `GetDataBy` metody `FillBySupplierID` a `GetSupplierBySupplierID`v uvedeném pořadí.


[![Název metody FillBySupplierID TableAdapter a GetSupplierBySupplierID](working-with-computed-columns-cs/_static/image29.png)](working-with-computed-columns-cs/_static/image28.png)

**Obrázek 10**: název metody třídy TableAdapter `FillBySupplierID` a `GetSupplierBySupplierID` ([kliknutím ji zobrazíte obrázek v plné velikosti](working-with-computed-columns-cs/_static/image30.png))


Kliknutím na Dokončit dokončíte průvodce.

## <a name="step-6-creating-the-business-logic-layer"></a>Krok 6: Vytvoření vrstvy obchodní logiky

Než vytvoříme stránky ASP.NET, která používá počítaný sloupec vytvořili v kroku 1, musíme nejprve přidat odpovídající metody v BLL. Naši stránku ASP.NET, který vytvoříme v kroku 7, vám umožní uživatelům zobrazit a upravit dodavatelů. Proto potřebujeme našich knihoven BLL minimálně poskytnout metodu k získání všech dodavatelů a druhý k aktualizaci konkrétního dodavatele.

Vytvořte nový soubor třídy `SuppliersBLLWithSprocs` v `~/App_Code/BLL` složky a přidejte následující kód:


[!code-csharp[Main](working-with-computed-columns-cs/samples/sample6.cs)]

Jako jiné třídy BLL `SuppliersBLLWithSprocs` má `protected` `Adapter` vlastnost, která vrací instanci `SuppliersTableAdapter` třídy spolu se dvěma `public` metody: `GetSuppliers` a `UpdateSupplier`. `GetSuppliers` Volá metodu a vrátí `SuppliersDataTable` vrácené odpovídající `GetSupplier` metoda ve vrstvy přístupu k datům. `UpdateSupplier` Metoda načte informace o konkrétní dodavatele aktualizuje prostřednictvím volání s vrstvou DAL `GetSupplierBySupplierID(supplierID)` metody. Pak aktualizuje `CategoryName`, `ContactName`, a `ContactTitle` vlastnosti a potvrdí tyto změny do databáze voláním vrstvy přístupu k datům s `Update` předejte v upraveném `SuppliersRow` objektu.

> [!NOTE]
> S výjimkou `SupplierID` a `CompanyName`, povolit všechny sloupce v tabulce Dodavatelé `NULL` hodnoty. Proto pokud předaný `contactName` nebo `contactTitle` parametry jsou `null` musíme nastavit odpovídající `ContactName` a `ContactTitle` vlastnosti, které chcete `NULL` databáze pomocí hodnoty `SetContactNameNull` a `SetContactTitleNull`metody, v uvedeném pořadí.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Krok 7: Práce s vypočítaným sloupcem od prezentační vrstvy

Počítaný sloupec přidán do `Suppliers` tabulky a vrstvy DAL a BLL příslušným způsobem aktualizuje, jsme připraveni k sestavení, která funguje s stránky ASP.NET `FullContactName` počítaný sloupec. Začněte otevřením `ComputedColumns.aspx` stránku `AdvancedDAL` složky a GridView přetáhněte z panelu nástrojů do návrháře. Nastavit prvek GridView s `ID` vlastnost `Suppliers` a z inteligentních značek, jeho vazbu na nového prvku ObjectDataSource s názvem `SuppliersDataSource`. Konfigurace ObjectDataSource používat `SuppliersBLLWithSprocs` třídy jsme přidali zpět v kroku 6 a klikněte na tlačítko Další.


[![Konfigurace ObjectDataSource pomocí třídy SuppliersBLLWithSprocs](working-with-computed-columns-cs/_static/image32.png)](working-with-computed-columns-cs/_static/image31.png)

**Obrázek 11**: Konfigurace ObjectDataSource k použití `SuppliersBLLWithSprocs` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](working-with-computed-columns-cs/_static/image33.png))


Existují pouze dvě metody definované v `SuppliersBLLWithSprocs` třídy: `GetSuppliers` a `UpdateSupplier`. Ujistěte se, že tyto dvě metody jsou uvedeny v seznamu vyberte a aktualizovat karty, v uvedeném pořadí a klikněte na tlačítko Dokončit dokončete konfiguraci ObjectDataSource.

Po dokončení Průvodce konfigurací zdroje dat sada Visual Studio přidá vlastnost BoundField pro každé pole data vrácená. Odeberte `SupplierID` Vlastnost BoundField a změnit `HeaderText` vlastnosti `CompanyName`, `ContactName`, `ContactTitle`, a `FullContactName` BoundFields společnosti, kontakt, název a příjmení kontaktu v uvedeném pořadí. Z inteligentních značek zaškrtněte políčko Povolit úpravy zapnout GridView s integrované funkce pro úpravy.

Kromě přidání BoundFields do prvku GridView, dokončení Průvodce zdrojem dat navíc způsobí, že Visual Studio pro nastavení prvku ObjectDataSource s `OldValuesParameterFormatString` vlastnost s původní\_{0}. Vrátit zpět, toto nastavení zpět na výchozí hodnotu, {0} .

Po provedení těchto úprav ovládacího prvku GridView a prvek ObjectDataSource, jejich deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](working-with-computed-columns-cs/samples/sample7.aspx)]

V dalším kroku navštivte tuto stránku prostřednictvím prohlížeče. Jak ukazuje obrázek 12 každého dodavatele je uveden v tabulce, která zahrnuje `FullContactName` sloupec, jehož hodnota je jednoduše zřetězení tři sloupce naformátovaná jako `ContactName` (`ContactTitle`, `CompanyName`).


[![Každý poskytovatel je uveden v mřížce](working-with-computed-columns-cs/_static/image35.png)](working-with-computed-columns-cs/_static/image34.png)

**Obrázek 12**: každý poskytovatel je uveden v mřížce ([kliknutím ji zobrazíte obrázek v plné velikosti](working-with-computed-columns-cs/_static/image36.png))


Kliknutím na tlačítko Upravit pro konkrétní dodavatele vyvolá zpětné volání a dokončí vykreslování tento řádek v jeho úpravy rozhraní (viz obrázek 13). První tři sloupce se vykreslí v jejich výchozí úpravy rozhraní – ovládací prvek textové pole, jehož `Text` je nastavena na hodnotu pole data. `FullContactName` Sloupec, ale zůstává jako text. Když BoundFields byly přidány do prvku GridView, po dokončení Průvodce konfigurací zdroje dat `FullContactName` Vlastnost BoundField s `ReadOnly` nastavenou na `true` protože odpovídající `FullContactName` sloupec v `SuppliersDataTable` má jeho `ReadOnly` nastavenou na `true`. Jak je uvedeno v kroku 4 `FullContactName` s `ReadOnly` nastavenou na `true` protože TableAdapter zjistila, že sloupec je počítaný sloupec.


[![Je FullContactName sloupce nejde upravit](working-with-computed-columns-cs/_static/image38.png)](working-with-computed-columns-cs/_static/image37.png)

**Obrázek 13**: `FullContactName` sloupec nejde upravit ([kliknutím ji zobrazíte obrázek v plné velikosti](working-with-computed-columns-cs/_static/image39.png))


Pokračujte a aktualizovat hodnotu jednoho nebo více sloupců, upravovat a kliknutím na tlačítko Aktualizovat. Poznámka: Jak `FullContactName` s hodnota se automaticky aktualizuje tak, aby odrážely změny.

> [!NOTE]
> Prvku GridView. v současnosti využívá BoundFields pro upravitelná pole, což vede k výchozí úpravy rozhraní. Vzhledem k tomu, `CompanyName` pole je povinné, by měly být převedeny TemplateField zahrnuje RequiredFieldValidator. Můžu ponechte toto cvičení pro dotčené čtečku. Poraďte [přidání validačních ovládacích prvků pro úpravy a vložení rozhraní](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) kurz podrobné pokyny o převod na pole TemplateField Vlastnost BoundField a přidání validačních ovládacích prvků.


## <a name="summary"></a>Souhrn

Při definování schématu pro tabulku, Microsoft SQL Server umožňuje zařazení vypočítané sloupce. Jedná se o sloupce, jejichž hodnoty se počítají na základě výraz, který obvykle odkazuje na hodnoty od ostatních sloupců ve stejném záznamu. Od hodnoty pro počítané sloupce jsou založené na výrazu, jsou jen pro čtení a nelze jí přiřadit hodnotu v `INSERT` nebo `UPDATE` příkazu. Zavádí se problémy při použití počítaný sloupec v hlavním dotazu objektu TableAdapter, který se pokusí automaticky generovat odpovídající `INSERT`, `UPDATE`, a `DELETE` příkazy.

V tomto kurzu jsme probírali techniky pro obcházení výzvy související vypočítané sloupce. Konkrétně jsme použili uložené procedury v našich TableAdapter překonat brittleness přináší v prvcích TableAdapters, použít SQL příkazy ad-hoc. Když s průvodci vytvořením objektu TableAdapter vytvořit nové uložené procedury, je důležité, vidíme, že hlavní dotaz zpočátku vynechat žádné vypočítané sloupce, protože jejich přítomnost zabraňuje změny uložené procedury data generovaná. Po dokončení počáteční konfigurace TableAdapter jeho `SelectCommand` uložené procedury lze retooled zahrnout žádné vypočítané sloupce.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Hilton Geisenow a Teresy Murphy. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](adding-additional-datatable-columns-cs.md)
> [další](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
