---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
title: Konfigurace nastavení připojení a příkaz úrovně Data Access Layer (VB) | Microsoft Docs
author: rick-anderson
description: TableAdapters v prvku DataSet zadali automaticky postará o připojení k databázi, vydávání příkazů a naplnění DataTable s výsledky...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: d57dfa2b-d627-45cb-b5b1-abbf3159d770
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
msc.type: authoredcontent
ms.openlocfilehash: b73c6113e84e290025e5835781fa2f85587289b1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877174"
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-vb"></a>Konfigurace nastavení připojení a příkaz úrovně Data Access Layer (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_VB.zip) nebo [stáhnout PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/datatutorial72vb1.pdf)

> TableAdapters v prvku DataSet zadali automaticky postará o připojení k databázi, vydávání příkazů a naplnění DataTable s výsledky. Ale pokud chceme, abyste dbali na tyto podrobnosti sebe a v tomto kurzu jsme zjistěte, jak pro přístup k nastavení připojení a příkaz úrovni databáze v TableAdapter existují situace.


## <a name="introduction"></a>Úvod

V průběhu kurzu jsme použili, implementovat vrstvou a obchodní objekty naše vrstveného architektury typové datové sady. Jak je popsáno v [první kurz](../introduction/creating-a-data-access-layer-vb.md), s typové datové sady DataTables sloužit jako úložiště dat, zatímco TableAdapters fungovat jako obálky ke komunikaci s databází načíst a upravit základní data. TableAdapters zapouzdření složité při práci s databází a ušetří nám s napsat kód pro připojení k databázi, příkaz nebo naplnit výsledky do DataTable.

Existují dobu, ale pokud je potřeba burrow do hloubce TableAdapter a napsat kód, který pracuje přímo s objekty ADO.NET. V [zabalení změny databáze v rámci transakce](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) kurzu, například jsme přidali metody k TableAdapter pro začátek, potvrzení a vracení změn transakce ADO.NET. Tyto metody používat interní, ručně vytvořit `SqlTransaction` objekt, který byl přiřazen TableAdapter s `SqlCommand` objekty.

V tomto kurzu vyzkoušíme jak získat přístup k nastavení připojení a příkaz úrovni databáze v TableAdapter. Konkrétně přidáme funkce `ProductsTableAdapter` , umožňuje přístup k podkladové připojovací řetězec a příkaz Nastavení časového limitu.

## <a name="working-with-data-using-adonet"></a>Práce s daty pomocí ADO.NET

Rozhraní Microsoft .NET Framework obsahuje nadbytku třídy vytvořena speciálně pro práci s daty. Tyto třídy, v rámci nalezen [ `System.Data` obor názvů](https://msdn.microsoft.com/library/system.data.aspx), se označují jako *ADO.NET* třídy. Některé třídy pod pojmem ADO.NET, jsou svázané s konkrétní *zprostředkovatele dat*. Zprostředkovatele dat si můžete představit jako komunikační kanál, který umožňuje informací mezi třídy ADO.NET a příslušné úložiště data. Existuje zobecněný poskytovatelů, jako je OleDb a rozhraní ODBC, jakož i poskytovatelé, které jsou speciálně určené pro konkrétní databázi systému. Například i když je možné se připojit k databázi systému Microsoft SQL Server pomocí zprostředkovatele služeb OleDb, poskytovatel Sqlclienta je mnohem efektivnější jako byl navržen a optimalizované speciálně pro SQL Server.

Při prostřednictvím kódu programu přístupu k datům, se obvykle používá vzoru následující:

1. Připojení k databázi.
2. Příkaz.
3. Pro `SELECT` dotazy, pracovat s výsledné záznamy.

Existují samostatné třídy ADO.NET pro provádění jednotlivých kroků. Pro připojení k databázi pomocí poskytovatel Sqlclienta, například použít [ `SqlConnection` třída](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). K problému `INSERT`, `UPDATE`, `DELETE`, nebo `SELECT` příkaz k databázi, použijte [ `SqlCommand` – třída](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

S výjimkou [zabalení změny databáze v rámci transakce](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) kurzu jsme nebyly museli vytvářet žádná označována nízké úrovně ADO.NET kódu, protože TableAdapters automaticky generovaný kód obsahuje funkce, které jsou potřebné k připojení k databázi, vydávání příkazů, načtení dat a naplnění dat do DataTables. Může však nastat situace, když je potřeba upravit tyto nízké úrovně nastavení. Prostřednictvím několika dalších krocích vyzkoušíme postup klepněte na do objektů ADO.NET interně používané TableAdapters.

## <a name="step-1-examining-with-the-connection-property"></a>Krok 1: Prozkoumání s vlastností připojení

Každá třída TableAdapter má `Connection` vlastnost, která určuje informace o připojení databáze. Tento typ dat vlastnosti s a `ConnectionString` výběru hodnot v Průvodci konfigurací TableAdapter Určuje hodnotu. Odvolat, že pokud nejprve přidáme TableAdapter na datovou sadu zadali tohoto průvodce nám vyzve k zadání databáze zdroje (viz obrázek 1). Rozevíracím seznamu v prvním kroku zahrnuje tyto databáze zadané v konfiguračním souboru, jakož i jiné databáze v Průzkumníku serveru s datová připojení. Pokud databáze, kterou chcete použít v rozevíracím seznamu neexistuje, připojení k nové databázi lze klepnutím na tlačítko nové připojení a poskytování informací o potřebných připojení.


[![Prvním krokem Průvodce nastavením TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image1.png)

**Obrázek 1**: prvním krokem Průvodce nastavením TableAdapter ([Kliknutím zobrazit obrázek v plné velikosti](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image3.png))


Chcete-li prověřit kód pro TableAdapter s chvíli trvat, umožňují s `Connection` vlastnost. Jak jsme uvedli v [vytváření Data Access Layer](../introduction/creating-a-data-access-layer-vb.md) kurzu automaticky vygenerovaný kód TableAdapter jsme můžete zobrazit tak, že přejdete do okna zobrazení tříd, přecházení na příslušné třídy a pak dvakrát klikněte na název člena.

Přejděte do okna zobrazení tříd přechodem do nabídky zobrazení a výběr zobrazení tříd (nebo zadáním Ctrl + Shift + C). Z horní polovině okna zobrazení tříd, přejdete dolů k `NorthwindTableAdapters` obor názvů a vyberte `ProductsTableAdapter` třídy. Bude se zobrazovat `ProductsTableAdapter` členy s v dolní polovinu zobrazení tříd, jak je znázorněno na obrázku 2. Dvakrát klikněte `Connection` vlastnost zobrazíte jeho kód.


![Dvakrát klikněte na vlastnost připojení v zobrazení tříd zobrazíte jeho automaticky vygenerovaný kód](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image4.png)

**Obrázek 2**: dvakrát klikněte na vlastnost připojení v zobrazení tříd zobrazíte jeho automaticky vygenerovaný kód


TableAdapter s `Connection` vlastnost a další související připojení kód způsobem:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample1.vb)]

Při vytvoření instance třídy TableAdapter, členské proměnné `_connection` rovná `Nothing`. Když `Connection` získat přístup k vlastnosti nejdřív zkontroluje, pokud `_connection` po vytvoření instance členské proměnné. Pokud ne, `InitConnection` je volána metoda, která vytvoří instanci `_connection` a nastaví její `ConnectionString` vlastnost k hodnotě připojovacího řetězce, který byl zadán z první krok průvodce s TableAdapter konfigurace.

`Connection` Vlastnost lze přiřadit také `SqlConnection` objektu. Díky tomu přidruží nový `SqlConnection` objekt s jednotlivými TableAdapter s `SqlCommand` objekty.

## <a name="step-2-exposing-connection-level-settings"></a>Krok 2: Vystavení nastavení úroveň připojení

Informace o připojení by měla zůstat zapouzdřené v rámci TableAdapter a nemusejí být k dispozici na jiných vrstev v architektuře aplikace. Však může existovat scénáře při informace TableAdapter s úroveň připojení musí být přístupné nebo přizpůsobitelné dotazu, uživatele nebo stránku ASP.NET.

Umožní s rozšířit `ProductsTableAdapter` v `Northwind` datovou sadu, která patří `ConnectionString` vlastnost, která lze vrstvu obchodní logiky číst nebo změnit připojovací řetězec používaný TableAdapter.

> [!NOTE]
> A *připojovací řetězec* je řetězec, který určuje informace o připojení databáze, jako je například zprostředkovatele, který má používat, umístění databáze, pověření pro ověření a další nastavení vztahující se k databázi. Seznam vzorů řetězec připojení používá celou řadu úložiště dat a poskytovatelů najdete v tématu [ConnectionStrings.com](http://www.connectionstrings.com/).


Jak je popsáno v [vytváření Data Access Layer](../introduction/creating-a-data-access-layer-vb.md) kurzu typové datové sady s automaticky vygenerované třídy je možné rozšířit pomocí částečné třídy. Nejprve vytvořte novou podsložku v projektu s názvem `ConnectionAndCommandSettings` pod `~/App_Code/DAL` složky.


![Přidat podsložku s názvem ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image5.png)

**Obrázek 3**: Přidat podsložku s názvem `ConnectionAndCommandSettings`


Přidat nový soubor třídy s názvem `ProductsTableAdapter.ConnectionAndCommandSettings.vb` a zadejte následující kód:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample2.vb)]

Přidá tuto třídu `Public` vlastnost s názvem `ConnectionString` k `ProductsTableAdapter` třídu, která umožňuje libovolné vrstvě načtení nebo aktualizace připojovací řetězec pro s TableAdapter základní připojení.

Tuto třídu vytvořit (a uložíte), otevřete `ProductsBLL` třídy. Přejděte na jednu z existujících metod, zadejte v `Adapter` a pak klikněte na tlačítko období klíč zapínají IntelliSense. Měli byste vidět nové `ConnectionString` vlastnost k dispozici v technologii IntelliSense, což znamená, že můžou prostřednictvím kódu programu číst nebo upravit tuto hodnotu z BLL.

## <a name="exposing-the-entire-connection-object"></a>Vystavení objekt celý připojení

Tuto třídu zpřístupní právě jednu vlastnost podkladového objektu připojení: `ConnectionString`. Pokud chcete zpřístupnit objekt celý připojení nad rámec TableAdapter, případně můžete změnit `Connection` vlastnosti s úroveň ochrany. Automaticky generovaný kód jsme se zaměřili v kroku 1 ukázalo, že TableAdapter s `Connection` vlastnost označena jako `Friend`, což znamená, že ho můžete přistupovat pouze třídy ve stejném sestavení. To se dá změnit, ale pomocí TableAdapter s `ConnectionModifier` vlastnost.

Otevřete `Northwind` datovou sadu kliknutím na tlačítko `ProductsTableAdatper` v návrháři a přejděte do okna vlastností. Zde se zobrazí `ConnectionModifier` nastavit na výchozí hodnotu, `Assembly`. Chcete-li `Connection` vlastnost k dispozici mimo sestavení s typové datové sady, změny `ConnectionModifier` vlastnost `Public`.


[![Je možné nakonfigurovat úroveň usnadnění připojení vlastnost s přes vlastnost ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image6.png)

**Obrázek 4**: `Connection` vlastnosti s usnadnění úroveň lze nakonfigurovat pomocí `ConnectionModifier` vlastnost ([Kliknutím zobrazit obrázek v plné velikosti](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image8.png))


Uložte datovou sadu a pak se vraťte do `ProductsBLL` třídy. Jako dříve, přejděte na jednu z existujících metod, zadejte v `Adapter` a pak klikněte na tlačítko období klíč zapínají IntelliSense. V seznamu by měla obsahovat `Connection` vlastnost, což znamená, že můžete nyní prostřednictvím kódu programu čtení nebo přiřazení nastavení úroveň připojení z BLL.

## <a name="step-3-examining-the-command-related-properties"></a>Krok 3: Prozkoumání vlastností souvisejících s příkaz

TableAdapter se skládá z hlavní dotaz, který ve výchozím nastavení, je automaticky generovaný `INSERT`, `UPDATE`, a `DELETE` příkazy. Tento hlavní dotaz s `INSERT`, `UPDATE`, a `DELETE` příkazy jsou implementované v kódu TableAdapter s jako objekt adaptér dat ADO.NET pomocí `Adapter` vlastnost. Jako s jeho `Connection` vlastnost, `Adapter` vlastnosti s datový typ je dáno poskytovatel dat použít. Vzhledem k tomu, že tyto kurzy použít poskytovatel Sqlclienta `Adapter` vlastnost je typu [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

TableAdapter s `Adapter` vlastnost má tři vlastnosti typu `SqlCommand` využívající na problém `INSERT`, `UPDATE`, a `DELETE` příkazy:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

A `SqlCommand` objekt je odpovědná za zasílání konkrétní dotaz do databáze a obsahuje vlastnosti, například: [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), která obsahuje příkaz ad-hoc SQL nebo uloženou proceduru provést; a [ `Parameters` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), což je kolekce `SqlParameter` objekty. Jak jsme viděli zpět v [vytváření Data Access Layer](../introduction/creating-a-data-access-layer-vb.md) kurzu těchto příkazů objekty lze přizpůsobit prostřednictvím okna Vlastnosti.

Kromě jeho hlavní dotazů TableAdapter může obsahovat proměnné několik metod, která při vyvolání, odesílání zadaný příkaz k databázi. Objekt příkazu hlavní dotazu s a objekty příkaz pro všechny další metody jsou uloženy v TableAdapter s `CommandCollection` vlastnost.

Umožňují s pozorně podívejte se na kód vygenerovaný `ProductsTableAdapter` v `Northwind` datovou sadu pro tyto dvě vlastnosti a jejich podpůrné členské proměnné a pomocné metody:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample3.vb)]

Kód pro `Adapter` a `CommandCollection` u vlastnosti úzce napodobuje `Connection` vlastnost. Existují členské proměnné, které obsahují objekty používané vlastnosti. Vlastnosti `Get` přístupové objekty spustit kontrolou, jestli je odpovídající členské proměnné `Nothing`. Pokud ano, inicializační metoda je volána, který vytvoří instanci členské proměnné a přiřadí základní vlastnosti týkající se příkaz.

## <a name="step-4-exposing-command-level-settings"></a>Krok 4: Vystavení nastavení příkaz úrovně

V ideálním případě by měla zůstat informace na úrovni příkaz zapouzdřené v rámci Data Access Layer. Tyto informace potřeba v jiných vrstev architektury, ale ho mohou být zpřístupněny prostřednictvím konkrétní třídu, stejně jako s nastavením úroveň připojení.

Vzhledem k tomu, že TableAdapter má jenom jeden `Connection` vlastnost kód pro vystavení nastavení úroveň připojení je přímočará. Si trochu složitější při úpravě nastavení úrovně příkaz, protože TableAdapter může mít více objektů příkaz – `InsertCommand`, `UpdateCommand`, a `DeleteCommand`, společně s proměnlivým počtem příkaz objekty v `CommandCollection` Vlastnost. Při aktualizaci nastavení příkaz úrovně se tato nastavení bude nutné rozšíří do všech objektů příkaz.

Představte si například, že byly v TableAdapter, kterou trvala mimořádná dlouhou dobu spuštění některých dotazů. Při použití provést jednu z těchto dotazů TableAdapter, budeme chtít zvýšit objekt příkazu s [ `CommandTimeout` vlastnost](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Tato vlastnost určuje počet sekund pro čekání na spuštění příkazu a výchozí nastavení je 30.

Povolit `CommandTimeout` vlastnost, která má být upraven BLL, přidejte následující `Public` metodu `ProductsDataTable` pomocí souboru třídu vytvořili v kroku 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.vb`):


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample4.vb)]

Tato metoda může vyvolat z BLL nebo prezentační vrstvy nastavit časový limit příkazu pro všechny příkazy problémy TableAdapter instanci.

> [!NOTE]
> `Adapter` a `CommandCollection` vlastnosti jsou označeny jako `Private`, což znamená, lze pouze k nim z kódu v rámci TableAdapter. Na rozdíl od `Connection` vlastnost, tyto modifikátory přístupu se nedají konfigurovat. Proto, pokud je nutné vystavit úrovni příkaz Vlastnosti na jiných vrstev v architektuře, musíte použít metodu třídu výše popsané zajistit `Public` metody nebo vlastnosti, která čtení nebo zápisu `Private` příkazů objektů.


## <a name="summary"></a>Souhrn

TableAdapters v prvku DataSet zadali sloužit k zapouzdření podrobnosti o přístupu dat a složitost. Pomocí TableAdapters, jsme nemusíte si dělat starosti o psaní kódu technologie ADO.NET pro připojení k databázi, příkaz ke nebo naplnit výsledky do DataTable. Všechny zpracuje se automaticky pro nás.

Ale může nastat situace, kdy je potřeba přizpůsobit nízké úrovně podrobností ADO.NET, jako je například změna připojovací řetězec nebo výchozí hodnoty časového limitu připojení nebo příkaz. TableAdapter má automaticky generovaný `Connection`, `Adapter`, a `CommandCollection` vlastnosti, ale ty jsou buď `Friend` nebo `Private`, ve výchozím nastavení. Tyto interní informace mohou být zpřístupněny tím, že rozšíří TableAdapter pomocí částečné třídy mají být zahrnuty `Public` metody nebo vlastnosti. Alternativně TableAdapter s `Connection` – modifikátor přístupu vlastnost lze nastavit pomocí TableAdapter s `ConnectionModifier` vlastnost.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Burnadette Leigh, S ren Jakub Lauritsen Teresy Murphy a Hilton Geisenow. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](working-with-computed-columns-vb.md)
> [další](protecting-connection-strings-and-other-configuration-information-vb.md)
