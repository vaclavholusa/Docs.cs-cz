---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
title: Konfigurace nastavení připojení a příkaz úrovně vrstvy přístupu k datům (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Objekty TableAdapter v datové sadě zadán automaticky postará o připojení k databázi, vydávání příkazů a naplnění DataTable s výsledky...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: cd330dd9-6254-4305-9351-dd727384c83b
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
msc.type: authoredcontent
ms.openlocfilehash: 142c8e93422ac03d2f2205b6635f88b982b4c9e2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756922"
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-c"></a>Konfigurace nastavení připojení a příkaz úrovně vrstvy přístupu k datům (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_CS.zip) nebo [stahovat PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/datatutorial72cs1.pdf)

> Objekty TableAdapter v datové sadě zadán automaticky postará o připojení k databázi, vydávání příkazů a naplnění DataTable s výsledky. Existují situace, ale když chceme, aby se postaral o tyto podrobnosti si a v tomto kurzu jsme zjistěte, jak získat přístup k nastavení databáze úroveň připojení a příkazu v TableAdapter.


## <a name="introduction"></a>Úvod

V celé řadě kurzů jsme použili typované datové sady k implementaci vrstvy přístupu k datům a naší vrstvené architektury pro obchodní objekty. Jak je popsáno v [první kurz](../introduction/creating-a-data-access-layer-cs.md), s typová DataTables sloužit jako úložiště dat, zatímco objekty TableAdapter fungovat jako obálky ke komunikaci s databází a načítají a upravují podkladová data. Objekty TableAdapter zapouzdření složitost při práci s databází a ušetří nás by bylo nutné napsat kód pro připojení k databázi, příkaz nebo naplnit výsledky do objektu DataTable.

Existují však situace, kdy musíme burrow do hlubin TableAdapter a napsat kód, který pracuje přímo s objekty ADO.NET. V [zabalení úprav databáze do transakce](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) kurz, například jsme přidali metody do třídy TableAdapter pro začátek, potvrzení a vrácení zpět transakcí ADO.NET. Tyto metody používat interní, ručně vytvořili `SqlTransaction` objekt, který bylo přiřazeno TableAdapter s `SqlCommand` objekty.

V tomto kurzu prozkoumáme jak získat přístup k nastavení databáze úroveň připojení a příkazu v TableAdapter. Konkrétně můžeme přidat funkce, které `ProductsTableAdapter` , která umožňuje přístup k podkladové připojovací řetězec a nastavení časového limitu příkazu.

## <a name="working-with-data-using-adonet"></a>Práce s daty pomocí ADO.NET

Rozhraní Microsoft .NET Framework obsahuje množství tříd určený konkrétně pro práci s daty. Tyto třídy v rámci nebyl nalezen [ `System.Data` obor názvů](https://msdn.microsoft.com/library/system.data.aspx), jsou označovány jako *ADO.NET* třídy. Některé třídy v rámci zastřešující ADO.NET jsou vázané na konkrétní *poskytovatele dat*. Zprostředkovatel dat můžete představit jako komunikační kanál, který umožňuje informací mezi třídy rozhraní ADO.NET a základnímu úložišti dat. Existují zobecněný poskytovatelé, třeba OleDb a rozhraní ODBC, jakož i zprostředkovatelů, které jsou navrženy speciálně pro konkrétní databázi systému. Například i když je možné se připojit k databázi serveru Microsoft SQL Server pomocí zprostředkovatele služeb OleDb, poskytovatel Sqlclienta je mnohem efektivnější byl navržený a optimalizovaný pro SQL Server.

Když programově přístup k datům, je obvykle používají následující vzor:

- Připojení k databázi.
- Příkaz.
- Pro `SELECT` dotazy, pracují s výsledné záznamy.

Existují samostatné třídy rozhraní ADO.NET pro provádění jednotlivých kroků. Pro připojení k databázi pomocí poskytovatel Sqlclienta, například použít [ `SqlConnection` třídy](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). K problému `INSERT`, `UPDATE`, `DELETE`, nebo `SELECT` k databázi, použijte příkaz [ `SqlCommand` třídy](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

S výjimkou [zabalení úprav databáze do transakce](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) kurzu jsme nebyly museli vytvářet žádná nízké úrovně ADO.NET kód sami, protože obsahuje automaticky generovaný kód objekty TableAdapter funkce potřebné pro připojení k databázi, vydávat příkazy, načtení dat a naplnění dat do datové tabulky. Může však nastat situace, kdy budeme potřebovat přizpůsobit nastavení nízké úrovně. Přes několik dalších kroků prozkoumáme jak pronikněte do ADO.NET objekty interně jej využívá objekty TableAdapter.

## <a name="step-1-examining-with-the-connection-property"></a>Krok 1: Zkoumání pomocí vlastnosti připojení

Každá třída TableAdapter má `Connection` vlastnost, která určuje informace o připojení databáze. Tento typ dat vlastnosti s a `ConnectionString` hodnota se určují podle výběru v Průvodci konfigurací TableAdapter. Připomínáme, že když jsme nejprve přidat TableAdapter k datové sadě zadán tento průvodce výzva pro databázi zdroje (viz obrázek 1). Rozevíracím seznamu v prvním kroku zahrnuje tyto databáze zadané v konfiguračním souboru, jakož i jiných databází v Průzkumníku serveru s datová připojení. Pokud databáze, kterou chceme použít neexistuje v rozevíracím seznamu, nové připojení k databázi je možné zadat tak kliknutím na tlačítko nové připojení a poskytuje informace o připojení potřebné.


[![Prvním krokem Průvodce nastavením TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image1.png)

**Obrázek 1**: The první krok průvodce nastavením TableAdapter ([kliknutím ji zobrazíte obrázek v plné velikosti](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image3.png))


Umožňují s věnujte chvíli kontrole kódu pro TableAdapter s `Connection` vlastnost. Jak je uvedeno v [vytvoření vrstvy přístupu k datům](../introduction/creating-a-data-access-layer-cs.md) kurzu jsme zobrazit automaticky vygenerovaný kód TableAdapter tak, že přejdete do okna zobrazení tříd, podrobnostem příslušné třídy a následným dvojitým kliknutím název členu.

Přejděte do okna zobrazení třídy tak, že přejdete do nabídky zobrazení a zvolíte zobrazení tříd (nebo zadáním kombinace kláves Ctrl + Shift + C). V horní polovině okno Zobrazení tříd, přejít k podrobnostem `NorthwindTableAdapters` obor názvů a vyberte `ProductsTableAdapter` třídy. Bude se zobrazovat `ProductsTableAdapter` s členy v dolní polovině zobrazení tříd, jak je znázorněno na obrázku 2. Dvakrát klikněte `Connection` vlastnost zobrazíte jeho kód.


![Dvakrát klikněte na vlastnost připojení v zobrazení tříd a zobrazte jeho automaticky generovaný kód](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image4.png)

**Obrázek 2**: dvakrát klikněte na vlastnost připojení v zobrazení tříd a zobrazte jeho automaticky generovaný kód


TableAdapter s `Connection` vlastností a další související připojení kódu takto:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample1.cs)]

Když je vytvořena instance třídy TableAdapter, členské proměnné `_connection` rovná `null`. Když `Connection` získat přístup k vlastnosti jej nejprve zkontroluje, jestli `_connection` po vytvoření instance členské proměnné. Pokud ne, `InitConnection` je vyvolána metoda, která vytvoří instanci `_connection` a nastaví její `ConnectionString` vlastnost na hodnotu připojovacího řetězce zadané z prvního kroku průvodce s konfigurace TableAdapter.

`Connection` Vlastnosti lze přiřadit také `SqlConnection` objektu. Tím přidruží nový `SqlConnection` objektu s jednotlivými TableAdapter s `SqlCommand` objekty.

## <a name="step-2-exposing-connection-level-settings"></a>Krok 2: Vystavení nastavení na úrovni připojení

Informace o připojení by měla zůstat zapouzdřené v objektu TableAdapter a nemusejí být k dispozici na ostatních vrstvách v architektuře aplikace. Však může existovat scénáře informace TableAdapter s úroveň připojení musí být přístupné nebo upravit dotaz, uživatele nebo stránky ASP.NET.

Umožní s rozšířit `ProductsTableAdapter` v `Northwind` datovou sadu, která patří `ConnectionString` vlastnost, která je možné pomocí vrstvy obchodní logiky ke čtení nebo změnit připojovací řetězec používaný projektem TableAdapter.

> [!NOTE]
> A *připojovací řetězec* je řetězec, který určuje informace o připojení databáze, jako je například zprostředkovatele má být použit, umístění databáze, přihlašovací údaje pro ověření a další nastavení vztahující se k databázi. Seznam vzorů řetězec připojení používaný řadou úložišť dat a poskytovatelů najdete v tématu [ConnectionStrings.com](http://www.connectionstrings.com/).


Jak je popsáno v [vytvoření vrstvy přístupu k datům](../introduction/creating-a-data-access-layer-cs.md) kurzu typované datové sady s automaticky generované třídy je možné rozšířit pomocí částečných tříd. Nejprve vytvořte novou podsložku v projektu s názvem `ConnectionAndCommandSettings` pod `~/App_Code/DAL` složky.


![Přidat podsložku s názvem ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image5.png)

**Obrázek 3**: Přidání podsložku s názvem `ConnectionAndCommandSettings`


Přidejte nový soubor třídy s názvem `ProductsTableAdapter.ConnectionAndCommandSettings.cs` a zadejte následující kód:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample2.cs)]

Tato částečná třída přidává `public` vlastnost s názvem `ConnectionString` k `ProductsTableAdapter` třídu, která umožňuje libovolné vrstvě číst a aktualizovat připojovací řetězec pro TableAdapter s základní připojení.

Tato částečná třída vytvořen (a uložili), otevřete `ProductsBLL` třídy. Přejít na jednu z existujících metod a zadejte `Adapter` a potom stiskněte klávesu období zobrazíte technologie IntelliSense. Měli byste vidět nové `ConnectionString` vlastnost k dispozici v technologii IntelliSense, což znamená, že můžete programově čtení nebo upravit této hodnoty BLL.

## <a name="exposing-the-entire-connection-object"></a>Vystavení objektu celé připojení

Tato částečná třída zveřejňuje pouze jedné vlastnosti takové základní objekt připojení: `ConnectionString`. Pokud chcete zpřístupnit objekt celé připojení nad rámec TableAdapter, případně můžete změnit `Connection` vlastnost s úroveň ochrany. Automaticky generovaný kód jsme se zaměřili na v kroku 1 ukázalo, že TableAdapter s `Connection` je vlastnost označená jako `internal`, což znamená, že to je přístupný pouze z tříd ve stejném sestavení. To lze změnit, ale prostřednictvím TableAdapter s `ConnectionModifier` vlastnost.

Otevřít `Northwind` datovou sadu, klikněte na `ProductsTableAdatper` v návrháři a přejděte do okna Vlastnosti. Uvidíte `ConnectionModifier` nastavit na výchozí hodnotu, `Assembly`. Chcete-li `Connection` mimo sestavení s typované datové sady, změny k dispozici `ConnectionModifier` vlastnost `Public`.


[![Úroveň připojení s vlastnosti usnadnění přístupu lze nakonfigurovat přes vlastnost ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image6.png)

**Obrázek 4**: `Connection` vlastnost s usnadnění úroveň lze nakonfigurovat prostřednictvím `ConnectionModifier` vlastnosti ([kliknutím ji zobrazíte obrázek v plné velikosti](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image8.png))


Uložte datovou sadu a pak se vraťte k `ProductsBLL` třídy. Než, přejděte na jednu z existujících metod a zadejte `Adapter` a potom stiskněte klávesu období zobrazíte technologie IntelliSense. Tento seznam by měl obsahovat `Connection` vlastnost, což znamená, že teď můžete prostřednictvím kódu programu číst nebo z BLL přiřadit nastavení úroveň připojení.

## <a name="step-3-examining-the-command-related-properties"></a>Krok 3: Prozkoumání vlastností související příkaz

Objektu TableAdapter se skládá z hlavní dotaz, který ve výchozím nastavení, má automaticky generované `INSERT`, `UPDATE`, a `DELETE` příkazy. Tento hlavní dotaz s `INSERT`, `UPDATE`, a `DELETE` příkazy jsou implementovány v kódu s TableAdapter jako objekt adaptéru dat ADO.NET pomocí `Adapter` vlastnost. Jak je jeho `Connection` vlastnost, `Adapter` vlastnost s datový typ je určen pomocí poskytovatele dat. Protože těchto kurzů použijte poskytovatel Sqlclienta `Adapter` vlastnost je typu [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

TableAdapter s `Adapter` vlastnost má tři vlastnosti typu `SqlCommand` , který se používá k problému `INSERT`, `UPDATE`, a `DELETE` příkazy:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

A `SqlCommand` objektu je odpovědná za zasílání speciální dotaz do databáze a má vlastnosti, například: [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), která obsahuje příkaz ad-hoc SQL nebo uloženou proceduru pro spuštění; a [ `Parameters` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), což je kolekce `SqlParameter` objekty. Jak jsme viděli zpátky [vytvoření vrstvy přístupu k datům](../introduction/creating-a-data-access-layer-cs.md) kurzu tyto příkaz objekty je možné přizpůsobit pomocí okna Vlastnosti.

Kromě jeho hlavním dotazu objektu TableAdapter může zahrnovat proměnný počet metody, která při vyvolání, odeslání zadaný příkaz k databázi. Objekt příkazu s hlavním dotazem a objekty příkazů pro všechny další metody jsou uloženy v objektu TableAdapter s `CommandCollection` vlastnost.

Umožňují s za chvíli si prohlédnout kód vygenerovaný `ProductsTableAdapter` v `Northwind` datovou sadu pro tyto dvě vlastnosti a jejich podpůrné členské proměnné a pomocné metody:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample3.cs)]

Kód `Adapter` a `CommandCollection` u vlastnosti úzce napodobuje `Connection` vlastnost. Existují členské proměnné, které obsahují objekty používané vlastnosti. Vlastnosti `get` přistupující objekty začněte tím, že kontroluje se, pokud je odpovídající členskou proměnnou `null`. Pokud ano, inicializační metoda je volána, který vytvoří instanci členské proměnné a přiřadí základní vlastnosti související se příkaz.

## <a name="step-4-exposing-command-level-settings"></a>Krok 4: Vystavení nastavení na úrovni příkazu

V ideálním případě by měla zůstat informace na úrovni příkazu zapouzdřené v rámci vrstvy přístupu k datům. V jiných vrstvách architekturu potřeba tyto informace, ale ho můžou zveřejnit prostřednictvím částečné třídy, stejně jako s nastavením úroveň připojení.

Protože TableAdapter má jenom jeden `Connection` vlastnost, kód pro vystavení nastavení na úrovni připojení je poměrně jednoduché. Máme o něco složitější při úpravě nastavení na úrovni příkazu, protože TableAdapter může mít více objektů příkazu - `InsertCommand`, `UpdateCommand`, a `DeleteCommand`, spolu s proměnný počet objektů příkazu v `CommandCollection` Vlastnost. Při aktualizaci nastavení na úrovni příkazu, tato nastavení bude nutné se rozšíří na všechny objekty příkazu.

Představte si například, že došlo ke některých dotazů v TableAdapter, který přijal mimořádné dlouhý čas ke spuštění. Při použití objektu TableAdapter spustí jednu z těchto dotazů, budeme chtít zvýšit objekt příkazu s [ `CommandTimeout` vlastnost](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Tato vlastnost určuje počet sekund čekání příkazu ke spuštění a výchozí hodnota je 30.

Povolit `CommandTimeout` vlastnost možné upravit tak, že BLL, přidejte následující `public` metodu `ProductsDataTable` pomocí souboru částečné třídy vytvořili v kroku 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.cs`):


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample4.cs)]

Tato metoda může vyvolat z knihoven BLL nebo prezentační vrstva nastavit časový limit příkazu pro všechny problémy s příkazy instancí TableAdapter.

> [!NOTE]
> `Adapter` a `CommandCollection` vlastnosti jsou označeny jako `private`, což znamená, že je přístupný pouze z kódu v rámci objektu TableAdapter. Na rozdíl od `Connection` vlastnost, tyto modifikátory přístupu se nedají konfigurovat. Proto pokud je nutné vystavit vlastnosti na úrovni příkazu na ostatních vrstvách v architektuře, která je nutné použít částečné třídy přístup bylo uvedeno výše, k poskytování `public` metoda nebo vlastnost, která čte nebo zapisuje do `private` příkaz objekty.


## <a name="summary"></a>Souhrn

Objekty TableAdapter v datové sadě zadán slouží k zapouzdření podrobnosti o přístupu dat a složitosti. Pomocí objektů TableAdapter, jsme nemusíte starat o zápis kódu ADO.NET pro připojení k databázi, příkaz nebo naplnit výsledky do objektu DataTable. Všechny zpracuje se automaticky pro nás.

Ale může nastat situace, kdy budeme potřebovat přizpůsobit nízké úrovně podrobností o ADO.NET, jako je například Změna připojovacího řetězce nebo výchozí hodnoty časového limitu připojení nebo příkaz. TableAdapter obsahuje automaticky generovaný `Connection`, `Adapter`, a `CommandCollection` vlastnosti, ale ty jsou buď `internal` nebo `private`, ve výchozím nastavení. Tyto interní informace mohou být vystaveny tím, že rozšíří TableAdapter pomocí částečných tříd mají být zahrnuty `public` metody nebo vlastnosti. Můžete také s TableAdapter `Connection` vlastnost modifikátor přístupu je možné nakonfigurovat pomocí TableAdapter s `ConnectionModifier` vlastnost.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Burnadette Leigh, S ren Jakub Lauritsen Teresy Murphy a Hilton Geisenow. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](working-with-computed-columns-cs.md)
> [další](protecting-connection-strings-and-other-configuration-information-cs.md)
