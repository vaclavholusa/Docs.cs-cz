---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
title: "Práce s počítaných sloupcích (C#) | Microsoft Docs"
author: rick-anderson
description: "Při vytváření tabulky databáze, Microsoft SQL Server umožňuje definovat počítaný sloupec, jehož hodnota je vypočítána z výrazu to obvykle referen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 57459065-ed7c-4dfe-ac9c-54c093abc261
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 41206f76f9d9ca68971a53d79e84d82349e92333
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="working-with-computed-columns-c"></a>Práce s počítaných sloupcích (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_CS.zip) nebo [stáhnout PDF](working-with-computed-columns-cs/_static/datatutorial71cs1.pdf)

> Při vytváření tabulky databáze, Microsoft SQL Server umožňuje definovat počítaný sloupec, jehož hodnota je vypočítána z výraz, který obvykle odkazuje na jiné hodnoty ve stejném záznamu databáze. Tyto hodnoty jsou v databázi, která vyžaduje zvláštní aspekty při práci s TableAdapters jen pro čtení. V tomto kurzu jsme zjistěte, jak ke splnění výzvy související počítané sloupce.


## <a name="introduction"></a>Úvod

Microsoft SQL Server umožňuje  *[vypočítaného sloupce](https://msdn.microsoft.com/library/ms191250.aspx)*, které jsou sloupce, jejichž hodnoty se vypočítají z výraz, který obvykle odkazuje na hodnoty z ostatních sloupců ve stejné tabulce. Jako příklad čas sledování datového modelu může být tabulka s názvem `ServiceLog` se sloupci včetně `ServicePerformed`, `EmployeeID`, `Rate`, a `Duration`, mimo jiné. Při velikosti kvůli pro službu položky (rychlost násobí hodnotou doba trvání) může být výpočet prostřednictvím webové stránky nebo jiné programové rozhraní, může být užitečný, aby zahrnovaly sloupec v `ServiceLog` tabulku s názvem `AmountDue` který ohlásil to informace. V tomto sloupci by bylo možné vytvořit jako normální sloupec, ale třeba, aby kdykoli aktualizovat `Rate` nebo `Duration` hodnot sloupce změněn. Lepší přístup by bylo aby `AmountDue` sloupec počítaný sloupec pomocí výrazu `Rate * Duration`. Díky tomu by způsobilo systému SQL Server se automaticky vypočítá `AmountDue` hodnota sloupce vždy, když byl odkazovaný v dotazu.

Vzhledem k tomu, že hodnota počítaný sloupec s je určen podle výrazu, tyto sloupce jsou jen pro čtení, a proto nemůže mít hodnoty přiřazen, je v `INSERT` nebo `UPDATE` příkazy. Ale při počítaného sloupce jsou součástí hlavní dotazu pro TableAdapter používající příkazů SQL ad-hoc, budou se automaticky přidají do automaticky vygenerované `INSERT` a `UPDATE` příkazy. V důsledku toho TableAdapter s `INSERT` a `UPDATE` dotazy a `InsertCommand` a `UpdateCommand` vlastnosti musí být aktualizovány k odebrat odkazy na všechny počítané sloupce.

Problémy pomocí vypočítaného sloupce s TableAdapter používající ad-hoc příkazů SQL je, že TableAdapter s `INSERT` a `UPDATE` dotazy jsou automaticky znovu vygenerovat kdykoli dokončení Průvodce nastavením TableAdapter konfigurace. Proto počítaných sloupcích ručně odstraněn z `INSERT` a `UPDATE` dotazy se znovu zobrazí, pokud je znovu spusťte průvodce. I když TableAdapters, které používají uložené procedury nejsou zobrazeny t trpí tento brittleness, mají své vlastní Adaptivní, které jsme vyřeší v kroku 3.

V tomto kurzu přidáme počítaný sloupec pro `Suppliers` tabulky v databázi Northwind a pak vytvořte odpovídající TableAdapter pro práci s Tato tabulka a její počítaný sloupec. Máme naše TableAdapter pomocí uložených procedur místo příkazů SQL ad-hoc, tak, aby naše úpravy nejsou t ztráty při použití Průvodce nastavením TableAdapter konfigurací.

Umožňují s začít!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Krok 1: Přidání vypočítaného sloupce na`Suppliers`tabulky

Databáze Northwind nemá všechny počítané sloupce, takže budeme muset přidat jeden sebe. Pro tento kurz umožní přidat počítaný sloupec s `Suppliers` tabulka s názvem `FullContactName` , vrátí jméno kontaktu s, název a fungují pro společnost v následujícím formátu: `ContactName` (`ContactTitle`, `CompanyName`). Tento počítaný sloupec může používat v sestavách, při zobrazení informace o dodavateli.

Začněte otevřením `Suppliers` definice tabulky kliknutím pravým tlačítkem na `Suppliers` tabulky v Průzkumníku serveru a v místní nabídce zvolte otevřete definici tabulky. Sloupce v tabulce a jejich vlastnosti, jako datový typ, bude se zobrazovat jestli umožňují `NULL` s a tak dále. Chcete-li přidat počítaný sloupec, spusťte zadáním názvu sloupce do definice tabulky. Potom zadejte do textového pole (vzorec) v části specifikace vypočítaného sloupce v okně Vlastnosti sloupce jeho výrazu (viz obrázek 1). Název počítaný sloupec `FullContactName` a použijte následující výraz:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample1.sql)]

Všimněte si, že může být zřetězen řetězce v systému SQL pomocí `+` operátor. `CASE` Příkaz lze použít jako podmíněného v tradiční programovací jazyk. Ve výše uvedené výrazu `CASE` příkaz lze přečíst jako: Pokud `ContactTitle` není `NULL` ve výstupu `ContactTitle` zřetězen s čárkou, jinak hodnota emitování nic. Další informace o užitečnost `CASE` prohlášení, najdete v části [Power SQL `CASE` příkazy](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Místo použití `CASE` prohlášení, může případně použili jsme `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx)Vrátí *checkExpression* Pokud jinou hodnotu než NULL, jinak vrátí *zastaralá*. While – buď `ISNULL` nebo `CASE` bude fungovat v tomto případě jsou komplikovanější scénáře kde flexibilitu `CASE` příkaz nelze porovnat podle `ISNULL`.


Po přidání Tento počítaný sloupec obrazovky by měla vypadat podobně jako na následujícím snímku na obrázku 1.


[![Přidejte počítaný sloupec s názvem FullContactName k tabulce dodavatelů](working-with-computed-columns-cs/_static/image2.png)](working-with-computed-columns-cs/_static/image1.png)

**Obrázek 1**: Přidat počítaný sloupec s názvem `FullContactName` k `Suppliers` tabulky ([Kliknutím zobrazit obrázek v plné velikosti](working-with-computed-columns-cs/_static/image3.png))


Po pojmenování počítaný sloupec a zadáním jeho výrazu, uložit změny do tabulky a kliknutím na ikonu Uložit na panelu nástrojů, zasažení Ctrl + S nebo přechodem do nabídky soubor a zvolením Uložit `Suppliers`.

Ukládá se tabulka by měl aktualizovat Průzkumníka serveru, včetně sloupci právě přidané v `Suppliers` seznam sloupců tabulky s. Kromě toho automaticky upraví výraz do textového pole (vzorce) na ekvivalentní výraz, který odstraní nepotřebné prázdné znaky, obklopuje názvy sloupců do závorek (`[]`) a zahrnuje závorkách více explicitně zobrazíte pořadí operací:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample2.sql)]

Další informace o počítané sloupce v systému Microsoft SQL Server, najdete v části [technické dokumentace](https://msdn.microsoft.com/library/ms191250.aspx). Také si přečtěte [postup: Zadejte vypočítaného sloupce](https://msdn.microsoft.com/library/ms188300.aspx) pro podrobný postup vytváření počítaných sloupcích.

> [!NOTE]
> Ve výchozím nastavení počítané sloupce nejsou fyzicky uložené v tabulce, ale místo toho jsou přepočítána pokaždé, když se odkazuje v dotazu. Zaškrtnutím políčka se jako trvalý, ale můžete určit, aby SQL Server fyzicky ukládat počítaný sloupec v tabulce. To uděláte indexu má být vytvořena na počítaný sloupec, což může zlepšit výkon dotazů, které používají hodnotě počítaný sloupec v jejich `WHERE` klauzule. V tématu [vytváření indexů v vypočítaného sloupce](https://msdn.microsoft.com/library/ms189292.aspx) Další informace.


## <a name="step-2-viewing-the-computed-column-s-values"></a>Krok 2: Zobrazení hodnoty s počítaný sloupec

Než začneme pracovní na Data Access Layer, umožňují s trvat několik minut, chcete-li zobrazit `FullContactName` hodnoty. Z Průzkumníka serveru, klikněte pravým tlačítkem na `Suppliers` název tabulky a v místní nabídce vyberte nový dotaz. Otevře se dialogové okno s dotazem, jejichž zadání vyzývá nám můžete zvolit jaké tabulky, které chcete zahrnout do dotazu. Přidat `Suppliers` tabulky a klikněte na tlačítko Zavřít. Dále zkontrolujte `CompanyName`, `ContactName`, `ContactTitle`, a `FullContactName` sloupce z tabulky dodavatelů. Nakonec klikněte na ikonu červený vykřičník na panelu nástrojů k provedení dotazu a zobrazit výsledky.

Jak je vidět na obrázku 2, výsledky budou zahrnovat `FullContactName`, které seznamy `CompanyName`, `ContactName`, a `ContactTitle` sloupců pomocí formátu ldquo;`ContactName` (`ContactTitle`, `CompanyName`) .


[![FullContactName používá formát kontakt (funkce, NázevSpolečnosti)](working-with-computed-columns-cs/_static/image5.png)](working-with-computed-columns-cs/_static/image4.png)

**Obrázek 2**: `FullContactName` používá formát `ContactName` (`ContactTitle`, `CompanyName`) ([Kliknutím zobrazit obrázek v plné velikosti](working-with-computed-columns-cs/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Krok 3: Přidání`SuppliersTableAdapter`na Data Access Layer

Chcete-li pracovat s informace o dodavateli v naší aplikaci musíme nejprve vytvořit TableAdapter a DataTable v našem DAL. V ideálním případě by to by provést pomocí stejných kroků přehledné na dřívější kurzy. Práce s počítané sloupce však zavádí několik vráskami, které si zasloužila diskusní.

Pokud používáte TableAdapter používající příkazů SQL ad-hoc, můžete jednoduše zahrnout počítaný sloupec hlavní dotazu TableAdapter s prostřednictvím Průvodce nastavením TableAdapter konfigurací. To však bude automaticky generovat `INSERT` a `UPDATE` příkazy, které zahrnují počítaný sloupec. Pokud se pokusíte provést jednu z těchto metod `SqlException` se zprávou sloupec *ColumnName* nelze upravit, protože je počítaný sloupec nebo je výsledkem operátoru UNION bude vyvolána. Když `INSERT` a `UPDATE` příkazu můžete ručně upraveny prostřednictvím TableAdapter s `InsertCommand` a `UpdateCommand` vlastnosti, tato přizpůsobení budou ztraceny vždy, když je znovu spustit Průvodce konfigurací TableAdapter.

Z důvodu brittleness z TableAdapters, který používat příkazy SQL ad-hoc se doporučuje jsme pomocí uložených procedur při práci s počítané sloupce. Pokud používáte existující uložené procedury, TableAdapter popsané v jednoduše nakonfigurovat [použití existující uložené procedury, pro typové datové sady s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) kurzu. Pokud máte Průvodce nastavením TableAdapter vytvořit uložené procedury pro vás, ale je potřeba původně vynechejte všechny počítané sloupce z hlavní dotazu. Pokud zahrnete počítaný sloupec v hlavním dotazu, Průvodce konfigurací TableAdapter bude informovat o tom, po dokončení, že ho nelze vytvořit odpovídající uložené procedury. Stručně řečeno, musíme nejprve nakonfigurovat TableAdapter použití počítaný sloupec bez hlavní dotazu a potom ručně aktualizovat odpovídající uložené procedury a TableAdapter s `SelectCommand` zahrnout počítaný sloupec. Tento přístup je podobná používaný v [aktualizace pro použití TableAdapter](updating-the-tableadapter-to-use-joins-cs.md)`JOIN`*s* kurzu.

V tomto kurzu mohli s přidat nové TableAdapter a automaticky vytvořit uložené procedury pro nás. V důsledku toho budeme muset nejdřív vynechejte `FullContactName` počítaný sloupec z hlavní dotazu.

Začněte otevřením `NorthwindWithSprocs` datovou sadu v `~/App_Code/DAL` složky. Klikněte pravým tlačítkem myši v návrháři a v místní nabídce vyberte Přidat nové TableAdapter. Tím se spustí Průvodce nastavením TableAdapter konfigurací. Zadání dotazu na data z databáze (`NORTHWNDConnectionString` z `Web.config`) a klikněte na tlačítko Další. Vzhledem k tomu, že jsme ještě nevytvořili žádné uložené procedury pro dotazy nebo úprava `Suppliers` tabulky, vyberte možnost vytvořit novou uložené procedury možnost tak, aby průvodce vytvořit je pro nás a klikněte na tlačítko Další.


[![Zvolte nové uložené procedury vytvořit možnost](working-with-computed-columns-cs/_static/image8.png)](working-with-computed-columns-cs/_static/image7.png)

**Obrázek 3**: Zvolte vytvořit nové uložené procedury možnost ([Kliknutím zobrazit obrázek v plné velikosti](working-with-computed-columns-cs/_static/image9.png))


Další krok nám vyzve k hlavním dotazu. Zadejte následující dotaz, který vrátí `SupplierID`, `CompanyName`, `ContactName`, a `ContactTitle` sloupců pro každý dodavatele. Všimněte si, že tento dotaz záměrně vynechá počítaný sloupec (`FullContactName`); aktualizujeme odpovídající uložené procedury, která zahrnovat tento sloupec v kroku 4.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample3.sql)]

Po zadání hlavního dotazu a klepnutím na tlačítko Další, umožňuje průvodci název čtyři uložené procedury, které se budou generovat. Název tyto uložené procedury `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, a `Suppliers_Delete`, jak ukazuje obrázek 4.


[![Přizpůsobení názvy automaticky generované uložené procedury](working-with-computed-columns-cs/_static/image11.png)](working-with-computed-columns-cs/_static/image10.png)

**Obrázek 4**: přizpůsobení názvy Auto-Generated uložené procedury ([Kliknutím zobrazit obrázek v plné velikosti](working-with-computed-columns-cs/_static/image12.png))


V dalším kroku průvodce umožňuje nám název metody s TableAdapter a určit vzorce používá k přístupu a aktualizace dat. Ponechejte všechna tři políčka zaškrtnuto, ale přejmenovat `GetData` metodu `GetSuppliers`. Kliknutím na tlačítko Dokončit ukončete průvodce.


[![Přejmenujte metody GetData GetSuppliers](working-with-computed-columns-cs/_static/image14.png)](working-with-computed-columns-cs/_static/image13.png)

**Obrázek 5**: Přejmenovat `GetData` metodu `GetSuppliers` ([Kliknutím zobrazit obrázek v plné velikosti](working-with-computed-columns-cs/_static/image15.png))


Po kliknutí na tlačítko Dokončit, bude průvodce vytvořit čtyři uložené procedury a přidejte TableAdapter a odpovídající DataTable do typové datové sady.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Krok 4: Včetně počítaný sloupec s hlavního dotazu TableAdapter

Teď je potřeba aktualizovat TableAdapter a DataTable vytvořili v kroku 3 zahrnout `FullContactName` počítaný sloupec. To zahrnuje dva kroky:

1. Aktualizace `Suppliers_Select` uložené procedury se vrátíte `FullContactName` počítaný sloupec a
2. Aktualizace DataTable zahrnout odpovídající `FullContactName` sloupce.

Spustit tak, že přejdete na Průzkumníka serveru a procházení k podrobnostem do složky, uložené procedury. Otevřete `Suppliers_Select` uložené procedury a aktualizace `SELECT` dotaz, který patří `FullContactName` počítaný sloupec:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample4.sql)]

Uložit změny do uložené procedury, kliknutím na ikonu Uložit na panelu nástrojů, pak stisknete Ctrl + S nebo výběrem uložení `Suppliers_Select` možnost v nabídce Soubor.

Pak se vraťte do návrháře DataSet, klikněte pravým tlačítkem na `SuppliersTableAdapter`a v místní nabídce zvolte Konfigurovat. Všimněte si, že `Suppliers_Select` teď obsahuje sloupec `FullContactName` sloupec v jeho kolekce dat sloupců.


[![Spusťte Průvodce TableAdapter s konfigurací a aktualizovat sloupce s DataTable](working-with-computed-columns-cs/_static/image17.png)](working-with-computed-columns-cs/_static/image16.png)

**Obrázek 6**: Spusťte TableAdapter s Průvodce konfigurací aktualizace s DataTable sloupců ([Kliknutím zobrazit obrázek v plné velikosti](working-with-computed-columns-cs/_static/image18.png))


Kliknutím na tlačítko Dokončit ukončete průvodce. Odpovídající sloupec, který se bude automaticky přidáno `SuppliersDataTable`. Průvodce nastavením TableAdapter je zjistit, který `FullContactName` sloupec je počítaný sloupec a proto jen pro čtení. V důsledku toho nastaví sloupec s `ReadOnly` vlastnost `true`. Chcete-li to ověřit, vyberte sloupec z `SuppliersDataTable` a potom přejděte do okna vlastností (viz obrázek 7). Všimněte si, že `FullContactName` sloupec s `DataType` a `MaxLength` vlastnosti jsou nastaveny také odpovídajícím způsobem.


[![FullContactName sloupec označený jako jen pro čtení](working-with-computed-columns-cs/_static/image20.png)](working-with-computed-columns-cs/_static/image19.png)

**Obrázek 7**: `FullContactName` sloupec označený jako jen pro čtení ([Kliknutím zobrazit obrázek v plné velikosti](working-with-computed-columns-cs/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Krok 5: Přidání`GetSupplierBySupplierID`metodu TableAdapter

Pro účely tohoto kurzu vytvoříme stránku ASP.NET, které se zobrazí dodavatelů v aktualizovatelné mřížky. V po kurzy jste aktualizovali jsme jeden záznam ze vrstvu obchodní logiky načtením, že konkrétní záznam z DAL jako DataTable silného typu, aktualizace vlastností a pak odešle aktualizované DataTable zpět do vrstvy DAL k šíření změny databáze. Chcete-li provést tento první krok - načítání záznam aktualizováno z hodnoty DAL - musíme nejprve přidat `GetSupplierBySupplierID(supplierID)` metoda vrstvy Dal.

Klikněte pravým tlačítkem na `SuppliersTableAdapter` v návrhu datové sady a zvolte možnost Přidat dotazu v místní nabídce. Jako jsme to udělali v kroku 3, aby průvodce generovat nové uložené procedury pro nás tak, že vyberete možnost vytvořit novou uložené procedury (odkazovat zpět na obrázku 3 pro snímek tohoto kroku průvodce). Vzhledem k tomu, že tato metoda vrátí záznam s více sloupců, znamenat, že má použít dotaz SQL, který je vybrat, která vrací řádky a klikněte na tlačítko Další.


[![Zvolte Vybrat, která vrací řádky možnost](working-with-computed-columns-cs/_static/image23.png)](working-with-computed-columns-cs/_static/image22.png)

**Obrázek 8**: Zvolte Vybrat, která vrací řádky možnost ([Kliknutím zobrazit obrázek v plné velikosti](working-with-computed-columns-cs/_static/image24.png))


Další krok vyzve nám pro dotaz, použít tuto metodu. Zadejte následující text, který vrátí stejné datová pole jako hlavní dotaz, ale konkrétního dodavatele.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample5.sql)]

Na další obrazovce zobrazí nám název uložené procedury, která bude automaticky generovaný. Název tuto uloženou proceduru `Suppliers_SelectBySupplierID` a klikněte na tlačítko Další.


[![Název uložené procedury Suppliers_SelectBySupplierID](working-with-computed-columns-cs/_static/image26.png)](working-with-computed-columns-cs/_static/image25.png)

**Obrázek 9**: název uložená procedura `Suppliers_SelectBySupplierID` ([Kliknutím zobrazit obrázek v plné velikosti](working-with-computed-columns-cs/_static/image27.png))


Nakonec průvodce výzvy nám pro data přístup vzory a metoda názvů pro TableAdapter. Nechte zaškrtnutí obou políček zaškrtnuto, ale přejmenovat `FillBy` a `GetDataBy` metody `FillBySupplierID` a `GetSupplierBySupplierID`, v uvedeném pořadí.


[![Název FillBySupplierID metod TableAdapter a GetSupplierBySupplierID](working-with-computed-columns-cs/_static/image29.png)](working-with-computed-columns-cs/_static/image28.png)

**Obrázek 10**: název metod TableAdapter `FillBySupplierID` a `GetSupplierBySupplierID` ([Kliknutím zobrazit obrázek v plné velikosti](working-with-computed-columns-cs/_static/image30.png))


Kliknutím na tlačítko Dokončit ukončete průvodce.

## <a name="step-6-creating-the-business-logic-layer"></a>Krok 6: Vytvoření vrstvu obchodní logiky

Nemůžeme vytvořit stránku ASP.NET, který používá počítaný sloupec vytvořili v kroku 1, nejprve musíme přidat odpovídající metody v BLL. Naše stránku ASP.NET, které vytvoříte v kroku 7, vám umožní uživatelům zobrazit a upravit dodavatelů. Proto potřebujeme naše BLL zadat minimálně, metodu k získání všech dodavatelů a druhý k aktualizaci konkrétní dodavatele.

Vytvořte soubor novou třídu s názvem `SuppliersBLLWithSprocs` v `~/App_Code/BLL` složky a přidejte následující kód:


[!code-csharp[Main](working-with-computed-columns-cs/samples/sample6.cs)]

Jako ostatní třídy BLL `SuppliersBLLWithSprocs` má `protected` `Adapter` vlastnost, která vrací instanci třídy `SuppliersTableAdapter` třída spolu se dvěma `public` metody: `GetSuppliers` a `UpdateSupplier`. `GetSuppliers` Volá metodu a vrátí `SuppliersDataTable` vrácené odpovídající `GetSupplier` metoda v datové vrstvě přístup. `UpdateSupplier` Metoda načte informace o konkrétní dodavatele aktualizované prostřednictvím volání DAL s `GetSupplierBySupplierID(supplierID)` metoda. Potom aktualizuje `CategoryName`, `ContactName`, a `ContactTitle` vlastnosti a potvrdí tyto změny do databáze voláním Data Access Layer s `Update` metody předávání v upravenou `SuppliersRow` objektu.

> [!NOTE]
> S výjimkou `SupplierID` a `CompanyName`, povolit všechny sloupce v tabulce Dodavatelé `NULL` hodnoty. Proto pokud předané `contactName` nebo `contactTitle` parametry jsou `null` je potřeba nastavit odpovídající `ContactName` a `ContactTitle` vlastnosti, které chcete `NULL` databázi pomocí hodnotu `SetContactNameNull` a `SetContactTitleNull`metody, v uvedeném pořadí.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Krok 7: Práce s počítaný sloupec od prezentační vrstvy

S počítaný sloupec přidán do `Suppliers` tabulky a DAL a BLL příslušným způsobem aktualizuje, jsme připraveni vytvořit stránku ASP.NET, který funguje s `FullContactName` počítaný sloupec. Začněte otevřením `ComputedColumns.aspx` stránku `AdvancedDAL` složku a přetáhněte GridView z panelu nástrojů na návrháře. Nastavit GridView s `ID` vlastnost `Suppliers` a z jeho inteligentních značek navázat jej na nové ObjectDataSource s názvem `SuppliersDataSource`. Konfigurace ObjectDataSource používat `SuppliersBLLWithSprocs` třídy jsme přidali v kroku 6 zpět a klikněte na tlačítko Další.


[![Konfigurace ObjectDataSource použití třídy SuppliersBLLWithSprocs](working-with-computed-columns-cs/_static/image32.png)](working-with-computed-columns-cs/_static/image31.png)

**Obrázek 11**: Konfigurace ObjectDataSource pro použití `SuppliersBLLWithSprocs` – třída ([Kliknutím zobrazit obrázek v plné velikosti](working-with-computed-columns-cs/_static/image33.png))


Existují pouze dvě metody, které jsou definované v `SuppliersBLLWithSprocs` – třída: `GetSuppliers` a `UpdateSupplier`. Ujistěte se, že tyto dvě metody jsou určené v SELECT a aktualizovat karty, v uvedeném pořadí a klikněte na tlačítko Dokončit k dokončení konfigurace ObjectDataSource.

Po dokončení Průvodce konfigurací zdroje dat přidá Visual Studio BoundField pro každé pole dat, která je vrácena. Odeberte `SupplierID` BoundField a změňte `HeaderText` vlastnosti `CompanyName`, `ContactName`, `ContactTitle`, a `FullContactName` BoundFields společnosti, obraťte se na název, název a úplný název kontaktu, v uvedeném pořadí. Ze inteligentní značky zaškrtněte políčko Povolit úpravy zapnutí GridView s integrované úpravy funkce.

Kromě přidání BoundFields do GridView, dokončení Průvodce zdrojem dat také způsobí, že chcete nastavit ObjectDataSource s Visual Studio `OldValuesParameterFormatString` vlastnost, která má původní\_{0}. Vraťte se toto nastavení zpět na výchozí hodnotu, {0}.

Po provedení tyto úpravy GridView a ObjectDataSource, jejich deklarativní značek by měl vypadat takto:


[!code-aspx[Main](working-with-computed-columns-cs/samples/sample7.aspx)]

Potom navštivte tuto stránku prostřednictvím prohlížeče. Jak ukazuje obrázek 12, každý dodavatele je uveden v tabulce, která zahrnuje `FullContactName` sloupec, jehož hodnota je jednoduše zřetězení tři sloupce formátovaný jako `ContactName` (`ContactTitle`, `CompanyName`).


[![Každý poskytovatel je uveden v mřížce](working-with-computed-columns-cs/_static/image35.png)](working-with-computed-columns-cs/_static/image34.png)

**Obrázek 12**: každý dodavatele je uveden v mřížce ([Kliknutím zobrazit obrázek v plné velikosti](working-with-computed-columns-cs/_static/image36.png))


Kliknutím na tlačítko Upravit pro konkrétní dodavatele způsobí postback a má tento řádek vykreslen v jeho úpravy rozhraní (viz obrázek 13). První tři sloupce se vykreslí v jejich výchozí úpravy rozhraní – řízení textové pole, jehož `Text` je nastavena na hodnotu pole data. `FullContactName` Sloupec, ale zůstává jako text. Když BoundFields byly přidány do GridView po dokončení Průvodce konfigurací zdroje dat `FullContactName` BoundField s `ReadOnly` byla nastavena na `true` protože odpovídající `FullContactName` sloupec v `SuppliersDataTable` má jeho `ReadOnly` vlastnost nastavena na hodnotu `true`. Jak jsme uvedli v kroku 4 `FullContactName` s `ReadOnly` byla nastavena na `true` protože TableAdapter zjistila, že sloupec je počítaný sloupec.


[![Sloupec FullContactName nejsou upravitelné](working-with-computed-columns-cs/_static/image38.png)](working-with-computed-columns-cs/_static/image37.png)

**Obrázek 13**: `FullContactName` sloupec nejsou upravitelné ([Kliknutím zobrazit obrázek v plné velikosti](working-with-computed-columns-cs/_static/image39.png))


Pokračujte a aktualizujte hodnotu jednoho nebo více sloupců, upravovat a kliknutím na tlačítko Aktualizovat. Poznámka: Jak `FullContactName` s hodnota se automaticky aktualizuje, aby odrážely změny.

> [!NOTE]
> GridView aktuálně používá BoundFields upravitelné pole, což vede k rozhraní nastaven jako výchozí. Vzhledem k tomu `CompanyName` pole je povinné, by měly být převedeny TemplateField, která zahrnuje RequiredFieldValidator. Nechat to jako cvičení pro zúčastněným čtečku. Obrátit [přidání ověřovací ovládací prvky pro úpravy a vkládání rozhraní](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) kurz podrobné pokyny k převodu BoundField na TemplateField a přidání ovládacích prvků ověření.


## <a name="summary"></a>Souhrn

Při definování schématu pro tabulku, Microsoft SQL Server umožňuje zahrnutí počítané sloupce. Jedná se o sloupce, jejichž hodnoty se vypočítají z výraz, který obvykle odkazuje na hodnoty z ostatních sloupců ve stejném záznamu. Od hodnoty pro počítané sloupce jsou založené na výrazu, jsou jen pro čtení a nelze jí přiřadit hodnotu v `INSERT` nebo `UPDATE` příkaz. Vzniká problémy při použití počítaný sloupec v hlavním dotazu TableAdapter, která se pokusí automaticky generovat odpovídající `INSERT`, `UPDATE`, a `DELETE` příkazy.

V tomto kurzu jsme probrali techniky pro obcházení výzvy související počítané sloupce. Konkrétně jsme použili uložené procedury v našem TableAdapter k překonání brittleness vyplývajících z TableAdapters, který používat příkazy SQL ad hoc. Při nutnosti vytvořit nový průvodce nastavením TableAdapter uložené procedury, je důležité, že obsahuje hlavní dotaz původně vynechejte všechny počítané sloupce, protože jejich přítomnosti brání generován postupy úpravy uložená data. Po dokončení původně konfigurace TableAdapter jeho `SelectCommand` uložené procedury lze retooled zahrnout všechny počítané sloupce.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Hilton Geisenow a Teresy Murphy. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](adding-additional-datatable-columns-cs.md)
[další](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
