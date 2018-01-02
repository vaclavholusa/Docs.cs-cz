---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
title: "Vytváření uložené procedury a uživatelem definované funkce s spravovaného kódu (VB) | Microsoft Docs"
author: rick-anderson
description: "Microsoft SQL Server 2005 se integruje s Common Language Runtime rozhraní .NET umožňuje vývojářům vytvářet databázové objekty pomocí spravovaného kódu. V tomto kurzu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 8be9a51b-ea6b-46c7-bfa2-476d9b14c24c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
msc.type: authoredcontent
ms.openlocfilehash: efec52c4085c24b1d6227a86f7c435ca657e493c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-vb"></a>Vytváření uložené procedury a funkce definované uživatelem se spravovaným kódem (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_VB.zip) nebo [stáhnout PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/datatutorial75vb1.pdf)

> Microsoft SQL Server 2005 se integruje s Common Language Runtime rozhraní .NET umožňuje vývojářům vytvářet databázové objekty pomocí spravovaného kódu. Tento kurz ukazuje, jak vytvořit spravované uložené procedury a spravovat uživatelsky definované funkce s kódu Visual Basic a C#. Také zjistit, jak tyto edice sady Visual Studio umožňují ladění tyto objekty spravované databáze.


## <a name="introduction"></a>Úvod

Použít databází jako s Microsoft SQL Server 2005 [Transact-Structured dotazovací jazyk (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) pro vložení, úpravy a načítání dat. Většina systémů databáze patří konstrukce pro seskupení řady příkazů SQL, které lze provést jako jeden, opakovaně použitelný jednotku. Uložené procedury jsou jedním z příkladů. Další je *funkce definované uživatelem*(UDF), konstruktor, který jsme prozkoumá podrobněji v kroku 9.

Jádro aplikace SQL je určená pro práci s datových sad. `SELECT`, `UPDATE`, A `DELETE` příkazy ze své podstaty platí pro všechny záznamy v příslušné tabulce a jsou omezena pouze jejich `WHERE` klauzule. Ještě existuje mnoho funkcí jazyka, které jsou navrženy pro práci s jedním záznamem najednou a manipulace s daty skalární. [`CURSOR`s](http://www.sqlteam.com/item.asp?ItemID=553) povolit pro sadu záznamů být smyčce prostřednictvím jednoho najednou. Řetězec funkce pro zpracování jako `LEFT`, `CHARINDEX`, a `PATINDEX` pracovní skalární daty. SQL také zahrnuje řízení toku příkazy, jako je třeba `IF` a `WHILE`.

Před Microsoft SQL Server 2005 uložené procedury a funkce UDF může definovat pouze jako kolekci příkazů T-SQL. SQL Server 2005, byla však určená k poskytnutí integrace s [Common Language Runtime (CLR)](https://msdn.microsoft.com/en-us/netframework/aa497266.aspx), což je modul runtime používá všechna sestavení .NET. V důsledku toho uložené procedury a funkce UDF v databázi systému SQL Server 2005 lze vytvořit pomocí spravovaného kódu. To znamená můžete vytvořit uložené procedury nebo UDF jako metodu v třídě jazyka Visual Basic. To umožňuje tyto uložené procedury a funkce UDF, abyste mohli využívat funkce v rozhraní .NET Framework a z vlastní třídy.

V tomto kurzu vyzkoušíme vytvoření spravované uložené procedury a funkce definované uživatelem a postup jejich integraci do databáze Northwind. Umožňují s začít!

> [!NOTE]
> Spravované databázové objekty nabízí několik výhod oproti nástroji SQL. Jazyk zvučnost a znalosti a možnost pro opakované použití existujícího kódu a logiku jsou uvedené hlavní výhody. Ale spravované databázové objekty, které by mohly být méně efektivní, při práci se sadami dat, které nezahrnují mnohem procedurální logiku. Podrobnější informace o výhodách používání spravovaného kódu a T-SQL, podívejte se [výhody z pomocí spravovaného kódu k vytvoření databázové objekty](https://msdn.microsoft.com/en-us/library/k2e1fb36(VS.80).aspx).


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>Krok 1: Přesouváte databázi Northwind z`App_Data`

Všechny naše kurzy doposud použili souboru databáze Microsoft SQL Server 2005 Express Edition za s aplikací webové `App_Data` složky. Umístění databáze v `App_Data` zjednodušené distribuci a spuštění tyto kurzy jako všechny soubory nebyly nalezeny v rámci jednoho adresáře a potřeba žádné další kroky konfigurace k otestování tohoto kurzu.

V tomto kurzu ale umožňují s přesunout databázi Northwind mimo `App_Data` a zaregistrovat ji explicitně pomocí instance databáze SQL Server 2005 Express Edition. Když jsme můžete provést kroky v tomto kurzu s databází v `App_Data` složky, počet kroků jsou vytvářeny mnohem jednodušší explicitně zaregistrováním databázi k instanci databáze SQL Server 2005 Express Edition.

Ke stažení pro účely tohoto kurzu má dva databázové soubory - `NORTHWND.MDF` a `NORTHWND_log.LDF` – umístěný ve složce s názvem `DataFiles`. Pokud postupujete podle spolu s vlastní implementaci kurzů k, zavřete Visual Studio a přesuňte `NORTHWND.MDF` a `NORTHWND_log.LDF` souborů z webu s `App_Data` složky do složky mimo web. Po přesunutí soubory databáze do jiné složky, je potřeba zaregistrovat databázi Northwind instance databáze SQL Server 2005 Express Edition. To můžete provést z SQL Server Management Studio. Pokud máte jiný - Express Edition z SQL Server 2005 v počítači nainstalována pak pravděpodobně už máte nainstalovaný Management Studio. Pokud jste pouze SQL Server 2005 Express Edition ve vašem počítači pak za chvíli ke stažení a instalaci [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Spusťte SQL Server Management Studio. Jak ukazuje obrázek 1, Management Studio se spustí s dotazem, jakou serveru pro připojení. Zadejte localhost\SQLExpress pro název serveru, vyberte v rozevíracím seznamu ověřování ověřování systému Windows a klikněte na tlačítko Připojit.


![Připojte se k instanci příslušné databáze](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image1.png)

**Obrázek 1**: Připojte se k instanci příslušné databáze


Po připojení jste již okno Průzkumník objektů se zobrazí seznam informace o instanci databáze SQL Server 2005 Express Edition, včetně databází, informace o zabezpečení, možnosti správy a tak dále.

Je potřeba připojit databázi Northwind v `DataFiles` složky (nebo kdekoli jste přesunuli ji) na instanci databáze SQL Server 2005 Express Edition. Klikněte pravým tlačítkem na složku databází a vyberte možnost připojit v místní nabídce. Tím se otevře dialogové okno připojení databáze. Kliknutím na tlačítko Přidat, přejít k podrobnostem a odpovídající `NORTHWND.MDF` souboru a klikněte na tlačítko OK. Obrazovky v tomto okamžiku by měla vypadat podobně jako na obrázku 2.


[![Připojte se k instanci příslušné databáze](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image2.png)

**Obrázek 2**: Připojte se k příslušné instanci databáze ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image4.png))


> [!NOTE]
> Pokud se připojujete k instanci SQL Server 2005 Express Edition prostřednictvím Management Studio dialogové okno Připojit databáze neumožňuje můžete rozbalit adresáře profilu uživatele, jako jsou dokumenty. Proto nezapomeňte umístit `NORTHWND.MDF` a `NORTHWND_log.LDF` soubory v adresáři jiný uživatelský profil.


Klikněte na tlačítko OK připojte databázi. Průzkumník objektů by měl nyní seznam databázi právě připojené a se zavře dialogové okno připojení databáze. Pravděpodobné Northwind databáze má název, jako je `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Přejmenujte databázi Northwind pravým tlačítkem myši na databázi a zvolením přejmenovat.


![Přejmenujte databáze Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image5.png)

**Obrázek 3**: přejmenujte databázi Northwind


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Krok 2: Vytvoření nové řešení a SQL Server projektu v sadě Visual Studio

Chcete-li vytvořit spravované uložené procedury nebo funkce UDF v systému SQL Server 2005 jsme bude zapisovat uložené procedury a logiku UDF jako kód jazyka Visual Basic v třídě. Jakmile kód byl zapsán, budeme muset tato třída kompilována sestavení ( `.dll` soubor), registraci sestavení databázi systému SQL Server a pak vytvořte uložená procedura nebo objekt UDF v databázi, která odkazuje na metodu odpovídající v sestavení. Takto lze všechny provést ručně. Jsme vytvoření kódu v jakýkoli text editoru, kompilace z příkazového řádku pomocí Visual Basic – kompilátor (`vbc.exe`), zaregistrujte ho pomocí databáze [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/en-us/library/ms189524.aspx) příkaz nebo z Management Studio a přidejte uložené procedura nebo objekt UDF prostřednictvím podobným způsobem. Naštěstí Professional a systémy Team verze sady Visual Studio obsahují typ projekt SQL Server, který automatizuje tyto úlohy. V tomto kurzu jsme provede procesem vytváření spravované uložené procedury a UDF pomocí typu SQL Server projektu.

> [!NOTE]
> Pokud používáte Visual Web Developer nebo ve standardní edici sady Visual Studio, budete muset použít ruční metodu místo. Krok 13 obsahuje podrobné pokyny k provedení těchto kroků ručně. I doporučujeme číst kroky 2 až 12 než si přečtete krok 13 vzhledem k tomu, že k těmto krokům patří důležité pokyny konfigurace systému SQL Server, které se musí použít bez ohledu na to, jaká verze sady Visual Studio, kterou používáte.


Začněte otevřením sady Visual Studio. V nabídce Soubor vyberte nový projekt zobrazíte dialogové okno Nový projekt (viz obrázek 4). Přejít k podrobnostem a typu projektu databáze a pak z šablon uvedené na pravé straně, vyberte pro vytvoření nového projektu serveru SQL. Rozhodli název tohoto projektu `ManagedDatabaseConstructs` a umístit ho do řešení s názvem `Tutorial75`.


[![Vytvoření nového projektu serveru SQL](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image6.png)

**Obrázek 4**: Vytvořte nový projekt serveru SQL ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image8.png))


Klikněte na tlačítko OK v dialogovém okně Nový projekt pro vytvoření řešení a projekt SQL Server.

Projekt SQL Server je vázaný na konkrétní databázi. V důsledku toho po vytvoření nového projektu serveru SQL jsme okamžitě vyzváni k zadejte tyto informace. Obrázek 5 zobrazuje dialogové okno Nový odkaz databáze, která byla doplnit tak, aby odkazoval na databázi Northwind, jsme registrován v instanci databáze SQL Server 2005 Express Edition zpět v kroku 1.


![Přidružit serverový projekt SQL databáze Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image9.png)

**Obrázek 5**: přidružit serverový projekt SQL databáze Northwind


Pokud chcete ladit spravované uložené procedury a funkce UDF vytvoříme v rámci tohoto projektu, je potřeba povolit ladění podporu pro připojení k SQL nebo CLR. Vždy, když přidružení projekt SQL Server s novou databázi (jako jsme to udělali na obrázku 5), Visual Studio požádá nám Pokud nám chcete povolit ladění SQL nebo CLR na připojení (viz obrázek 6). Klikněte na tlačítko Ano.


![Povolit ladění SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image10.png)

**Obrázek 6**: Povolit ladění SQL/CLR


V tomto okamžiku nový projekt serveru SQL se přidal k řešení. Obsahuje složku s názvem `Test Scripts` se soubor s názvem `Test.sql`, který se používá pro ladění objekty spravované databáze vytvořené v projektu. Podíváme se na ladění v kroku 12.

Jsme nyní můžete přidat nové spravované uložené procedury a funkce UDF do projektu, ale před nám umožní s nejprve zahrnout naše stávající webovou aplikaci v řešení. V nabídce Soubor vyberte možnost Přidat a vyberte existující webovou stránku. Přejděte do složky, příslušný web a klikněte na tlačítko OK. Jak je vidět na obrázku 7, dojde k aktualizaci řešení obsahovat dva projekty: web a `ManagedDatabaseConstructs` projekt SQL Server.


![Průzkumníku řešení teď obsahuje dva projekty](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image11.png)

**Obrázek 7**: Průzkumníku řešení teď obsahuje dva projekty


`NORTHWNDConnectionString` Hodnotu `Web.config` aktuálně odkazuje `NORTHWND.MDF` souboru v `App_Data` složky. Vzhledem k tomu, že jsme odebrali tuto databázi z `App_Data` a explicitně zaregistrovaný ho instance databáze SQL Server 2005 Express Edition, musíme odpovídajícím způsobem aktualizovat `NORTHWNDConnectionString` hodnotu. Otevřete `Web.config` ve webu a změny `NORTHWNDConnectionString` hodnotu, aby četl připojovací řetězec: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Po této změně vaší `<connectionStrings>` kapitoly `Web.config` by měl vypadat podobně jako následující:


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample1.xml)]

> [!NOTE]
> Jak je popsáno v [předchozí kurzu](debugging-stored-procedures-vb.md), při ladění objekt SQL serveru z klientské aplikace, jako je například webu ASP.NET, je potřeba zakázat sdružování připojení. Připojovací řetězec, ve výše uvedeném zakáže sdružování připojení ( `Pooling=false` ). Pokud neplánujete na ladění spravované uložené procedury a funkce UDF z webu ASP.NET, povolte sdružování připojení.


## <a name="step-3-creating-a-managed-stored-procedure"></a>Krok 3: Vytvoření spravované uložené procedury

Chcete-li přidat spravované uložené procedury k databázi Northwind, musíme nejprve vytvořit uložené procedury jako metodu v projektu SQL Server. V Průzkumníku řešení klikněte pravým tlačítkem na `ManagedDatabaseConstructs` název projektu a vyberte, chcete-li přidat novou položku. Tato akce zobrazí dialogové okno Přidat novou položku, která jsou uvedeny typy spravovaných databázové objekty, které mohou být přidány do projektu. Jak ukazuje obrázek 8, to mimo jiné zahrnuje uložené procedury a funkce definované uživatelem.

Umožní s Začněte přidáním uložené procedury, která jednoduše vrátí všechny produkty, které byla zrušena. Název nového souboru uložené procedury `GetDiscontinuedProducts.vb`.


[![Přidat nové uložené procedury s názvem GetDiscontinuedProducts.vb](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image12.png)

**Obrázek 8**: Přidejte nové uložené procedury s názvem `GetDiscontinuedProducts.vb` ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image14.png))


Tím se vytvoří nový soubor třídy jazyka Visual Basic s následujícím obsahem:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample2.vb)]

Všimněte si, že uložené procedury je implementovaný jako `Shared` metoda v rámci `Partial` soubor třídy s názvem `StoredProcedures`. Kromě toho `GetDiscontinuedProducts` metoda opatřen s [ `SqlProcedure` atribut](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlprocedureattribute.aspx), což označuje metodu jako uložené procedury.

Následující kód vytvoří `SqlCommand` objekt a nastaví její `CommandText` k `SELECT` dotaz, který vrátí všechny sloupce z `Products` tabulky produktů, jehož `Discontinued` pole se rovná 1. Pak spustí příkaz a odesílá výsledky zpět do klientské aplikace. Přidejte tento kód, který `GetDiscontinuedProducts` metoda.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample3.vb)]

K dispozici všechny objekty spravované databáze [ `SqlContext` objekt](https://msdn.microsoft.com/en-us/library/ms131108.aspx) představující kontext volajícího. `SqlContext` Poskytuje přístup k [ `SqlPipe` objekt](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlpipe.aspx) přes jeho [ `Pipe` vlastnost](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). To `SqlPipe` objekt se používá k ferry informace mezi databázi systému SQL Server a je volající aplikace. Jak již název napovídá, [ `ExecuteAndSend` metoda](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) provede předané `SqlCommand` objekt a odešle výsledky zpět do klientské aplikace.

> [!NOTE]
> Objekty spravované databáze jsou nejvhodnější pro uložené procedury a funkce UDF, které používají procedurální logiku, nikoli na základě sady logiku. Procedurální logika zahrnuje práce sady dat na základě řádku na řádek nebo práci s daty skalární. `GetDiscontinuedProducts` Metoda jsme právě vytvořili, ale zahrnuje žádné procedurální logiku. Proto se by v ideálním případě implementovat jako T-SQL uložené procedury. Jsou implementované jako spravované uložené procedury k předvedení kroky potřebné pro vytvoření a nasazení spravované uložené procedury.


## <a name="step-4-deploying-the-managed-stored-procedure"></a>Krok 4: Nasazení spravovaný uložené procedury

S tímto kódem dokončení jsou připraveny k nasazení do databáze Northwind. Nasazení projektu serveru SQL kompilovaný kód do sestavení, zaregistruje sestavení s databází a vytvoří odpovídající objekty v databázi, je připojení k příslušné metody v sestavení. Sada úkoly prováděné možnost nasazení je přesněji vyjádřeny v kroku 13. Klikněte pravým tlačítkem na `ManagedDatabaseConstructs` projektu název v Průzkumníku řešení a zvolte možnost nasadit. Nasazení se však nezdaří s následující chybou: nesprávná syntaxe poblíž textu "Externí". Budete muset nastavit úroveň kompatibility aktuální databáze na vyšší hodnotu k povolení této funkce. V tématu nápovědy pro uloženou proceduru `sp_dbcmptlevel`.

Této chybě dochází při pokusu o registraci sestavení s databázi Northwind. Chcete-li zaregistrovat sestavení s databází systému SQL Server 2005, musí nastavit úroveň kompatibility databáze s a 90. Ve výchozím nastavení mají nové databáze systému SQL Server 2005 úroveň kompatibility 90. Databáze vytvořené pomocí systému Microsoft SQL Server 2000 však mít výchozí úroveň kompatibility 80. Vzhledem k tomu, že databáze Microsoft SQL Server 2000 byla původně databáze Northwind, její úroveň kompatibility je nastaveno na 80 a proto musí být zvýšena na 90 Register spravované databázové objekty.

Chcete-li aktualizovat úroveň kompatibility databáze s, otevřete okno Nový dotaz v aplikaci Management Studio a zadejte:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample4.sql)]

Klikněte na ikonu spouštět na panelu nástrojů ke spuštění dotazu výše.


[![Aktualizace s úrovní kompatibility databáze Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image15.png)

**Obrázek 9**: aktualizace s úrovní kompatibility databáze Northwind ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image17.png))


Po aktualizaci úroveň kompatibility, znovu nasaďte projekt SQL Server. Tentokrát nasazení provést bez chyby.

Vraťte SQL Server Management Studio, klikněte pravým tlačítkem na databázi Northwind v Průzkumníku objektů a zvolte aktualizace. Dále přejděte do složky, programovatelnosti a poté rozbalte složku sestavení. Jak ukazuje obrázek 10, databázi Northwind nyní zahrnuje sestavení vygenerované `ManagedDatabaseConstructs` projektu.


![Sestavení ManagedDatabaseConstructs je nyní zaregistrována databáze Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image18.png)

**Obrázek 10**: `ManagedDatabaseConstructs` sestavení je nyní zaregistrována databáze Northwind


Také rozbalte složku uložené procedury. Existuje uvidíte uložené procedury s názvem `GetDiscontinuedProducts`. Tuto uloženou proceduru byl vytvořen tak, že proces nasazení a body `GetDiscontinuedProducts` metoda v `ManagedDatabaseConstructs` sestavení. Když `GetDiscontinuedProducts` spuštění uložené procedury, pak provede `GetDiscontinuedProducts` metoda. Vzhledem k tomu, že toto je spravované uložené procedury nelze upravovat pomocí Management Studio (proto ikonu zámku vedle názvu uložené procedury).


![Uložená procedura GetDiscontinuedProducts je uvedena ve složce uložené procedury](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image19.png)

**Obrázek 11**: `GetDiscontinuedProducts` uloženou proceduru, je uvedena ve složce uložené procedury


Je stále jeden další mezní máme k překonání před říkáme spravované uložené procedury: databáze nastaven tak, aby se zabránilo vykonání spravovaného kódu. Ověřit, otevřete nové okno dotazu a provádění `GetDiscontinuedProducts` uložené procedury. Zobrazí se následující chybová zpráva: spuštění uživatelského kódu v rozhraní .NET Framework je zakázáno. Povolte možnost konfigurace clr povolena.

Zkontrolujte informace o konfiguraci databáze s Northwind, zadejte a spusťte příkaz `exec sp_configure` v okně dotazu. Ukazuje to, že modul clr povolit nastavení je nastaveno na hodnotu 0.


[![Modul clr povolit nastavení je aktuálně nastaven na hodnotu 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image20.png)

**Obrázek 12**: clr povoleno nastavení je aktuálně nastaven na hodnotu 0 ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image22.png))


Všimněte si, že každé nastavení konfigurace v obrázek 12 má čtyři hodnoty uvedené s ním: minimální a maximální hodnoty a konfigurace a spuštění hodnoty. Chcete-li aktualizovat konfigurační hodnota u clr povoleno nastavení, spusťte následující příkaz:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample5.sql)]

Pokud znovu spustíte `exec sp_configure` uvidíte, že výše uvedený příkaz aktualizován konfigurační hodnota clr povoleno nastavení s na 1, ale že spuštění hodnota je stále nastavené na hodnotu 0. Změna konfigurace se projeví, je potřeba provést [ `RECONFIGURE` příkaz](https://msdn.microsoft.com/en-us/library/ms176069.aspx), který bude nastaven na aktuální hodnota konfigurace spuštění hodnota. Jednoduše zadejte `RECONFIGURE` v okně dotazu a klikněte na ikonu spouštět na panelu nástrojů. Pokud spustíte `exec sp_configure` teď by měl zobrazit hodnotu 1 pro konfiguraci nastavení clr povoleno s a spouštět hodnoty.

Dokončení konfigurace clr povoleno jsme jsou připravené ke spuštění spravovaný `GetDiscontinuedProducts` uložené procedury. V okně dotazu zadejte a spusťte příkaz `exec` `GetDiscontinuedProducts`. Volání uložené procedury způsobí, že odpovídající spravovaným kódem v `GetDiscontinuedProducts` metodu provést. Tento kód problémy `SELECT` dotaz vrátí všechny produkty, které se zastaví a vrátí tato data do volající aplikace, který je v této instanci systému SQL Server Management Studio. Management Studio obdrží tyto výsledky a zobrazí je v okně výsledky.


[![Zastaví GetDiscontinuedProducts uložené procedury vrátí všechny produkty](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image23.png)

**Obrázek 13**: `GetDiscontinuedProducts` uložené procedury vrátí všechny zastaví produkty ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Krok 5: Vytvoření spravované uložené procedury, které přijímají vstupní parametry

Spousta dotazy a uložené procedury jsme vytvořili v rámci tyto kurzy použili *parametry*. Například v [vytváření nové uložených procedur, pro typové datové sady s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) kurzu jsme vytvořili uložené procedury s názvem `GetProductsByCategoryID` , přijato vstupní parametr s názvem `@CategoryID`. Uložená procedura vracen všechny produkty jejichž `CategoryID` pole odpovídá hodnotě zadaných `@CategoryID` parametr.

Pokud chcete vytvořit spravované uložené procedury, která přijímá vstupní parametry, jednoduše zadejte tyto parametry v definici metoda s. Pro znázornění je umožňují s přidat jiné spravované uložené procedury k `ManagedDatabaseConstructs` projektu s názvem `GetProductsWithPriceLessThan`. Tato spravované uložené procedury bude akceptovat vstupní parametr specifikací ceny a vrátí všechny produkty jejichž `UnitPrice` pole je menší než hodnota parametru s.

Chcete-li přidat nový uložené procedury do projektu, klikněte pravým tlačítkem na `ManagedDatabaseConstructs` název projektu a zvolte pro přidání nové uložené procedury. Název souboru `GetProductsWithPriceLessThan.vb`. Jak jsme viděli v kroku 3, tím se vytvoří nový soubor třídy jazyka Visual Basic pomocí metody s názvem `GetProductsWithPriceLessThan` umístit `Partial` třída `StoredProcedures`.

Aktualizace `GetProductsWithPriceLessThan` definici metody s tak, aby přijímala [ `SqlMoney` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlmoney.aspx) vstupní parametr s názvem `price` a napsat kód pro spuštění a vrátí výsledky dotazu:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample6.vb)]

`GetProductsWithPriceLessThan` Definici metody s a kód podobá definice a kódu `GetDiscontinuedProducts` metoda vytvořili v kroku 3. Pouze rozdíly si, že `GetProductsWithPriceLessThan` metoda akceptuje jako vstupní parametr (`price`), `SqlCommand` s dotaz obsahuje parametr (`@MaxPrice`), a parametr se přidá `SqlCommand` s `Parameters` kolekce je a přiřadí hodnota `price` proměnné.

Po přidání tohoto kódu, znovu nasaďte projekt SQL Server. V dalším kroku se vraťte na SQL Server Management Studio a aktualizovat složku uložené procedury. Měli byste vidět nový záznam, `GetProductsWithPriceLessThan`. V okně dotazu zadejte a spusťte příkaz `exec GetProductsWithPriceLessThan 25`, který bude seznam všech produktů méně než 25, jak je vidět na obrázku 14.


[![Produkty v rámci 25 zobrazují](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image26.png)

**Obrázek 14**: produkty v rámci 25 zobrazují ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Krok 6: Volání z Data Access Layer spravované uložené procedury

V tuto chvíli jsme přidali `GetDiscontinuedProducts` a `GetProductsWithPriceLessThan` spravované uložené procedury pro `ManagedDatabaseConstructs` projektu a jejich zaregistrovali databázi Northwind SQL serveru. Můžeme také vyvolat tyto spravované uložené procedury z SQL Server Management Studio (viz obrázky 13 a 14). V pořadí pro naše ASP.NET spravované aplikace pro používání těchto uložených procedur, ale potřebujeme je přidáte do přístup k datům a vrstvy obchodní logiky v architektuře. V tomto kroku přidáme dvě nové metody pro `ProductsTableAdapter` v `NorthwindWithSprocs` typové datové sady, která byla původně vytvořena v [vytváření nové uložených procedur, pro typové datové sady s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) kurzu. V kroku 7 přidáme BLL odpovídající metody.

Otevřete `NorthwindWithSprocs` typové datové sady v sadě Visual Studio a spustíte tak, že přidáte nové metody pro `ProductsTableAdapter` s názvem `GetDiscontinuedProducts`. Pokud chcete přidat nové metody TableAdapter, klikněte pravým tlačítkem myši na název TableAdapter s v návrháři a zvolte možnost Přidat dotazu v místní nabídce.

> [!NOTE]
> Vzhledem k tomu, že jsme přesunout databázi Northwind z `App_Data` složku pro instanci databáze SQL Server 2005 Express Edition, je nutné aktualizovat odpovídající připojovací řetězec v souboru Web.config, aby odrážela tuto změnu. V kroku 2 jsme probrali aktualizace `NORTHWNDConnectionString` hodnotu `Web.config`. Pokud jste zapomněli provést tuto aktualizaci, se zobrazí chybová zpráva se nezdařilo přidat dotazu. Nelze najít připojení `NORTHWNDConnectionString` pro objekt `Web.config` v dialogovém okně při pokusu o přidání nové metody do TableAdapter. Chcete-li tuto chybu vyřešit, klikněte na tlačítko OK a potom přejděte na `Web.config` a aktualizovat `NORTHWNDConnectionString` hodnoty, jak je popsáno v kroku 2. Pak zkuste znovu přidat metodu TableAdapter. Tentokrát ho by měla fungovat bez chyby.


Přidání nové metody spustí Průvodce konfigurací dotazu TableAdapter, který jsme použili mnohokrát v posledních kurzy. Prvním krokem požádá, abychom mohli určit, jak TableAdapter by měl přístup k databázi: prostřednictvím příkazu SQL ad-hoc nebo nové nebo existující uložené procedury. Vzhledem k tomu, že jsme už máte vytvořené a registrované `GetDiscontinuedProducts` spravované uložené procedury s databází, zvolte použití existující uložené procedury možnost a stiskněte tlačítko Další.


[![Zvolte použití existující uložené procedury možnost](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image29.png)

**Obrázek 15**: Zvolte použití existující uložené procedury možnost ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image31.png))


Na další obrazovce nám vyzve k zadání uložené procedury, které se vyvolat metodu. Vyberte `GetDiscontinuedProducts` spravované uložené procedury z rozevíracího seznamu a klepněte na možnost Další.


[![Vyberte GetDiscontinuedProducts spravovaný uložené procedury](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image32.png)

**Obrázek 16**: vyberte `GetDiscontinuedProducts` spravované uloženou proceduru ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image34.png))


Potom jsme vyzváni k určení, zda uložené procedury vrátí řádky, jedna hodnota nebo nic. Vzhledem k tomu `GetDiscontinuedProducts` vrací sadu řádků deaktivovaný produkt, zvolte první možnost (Tabular data) a klikněte na tlačítko Další.


[![Vyberte možnost tabulková Data](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image35.png)

**Obrázek 17**: Vyberte možnost tabulkové dat ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image37.png))


Na poslední obrazovce průvodce umožňuje zadat názvy výsledné metod a vzory přístupu k dat použít. Ponechejte zaškrtnuto zaškrtávací políčka a název metody `FillByDiscontinued` a `GetDiscontinuedProducts`. Kliknutím na tlačítko Dokončit ukončete průvodce.


[![Název metody FillByDiscontinued a GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image38.png)

**Obrázek 18**: název metody `FillByDiscontinued` a `GetDiscontinuedProducts` ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image40.png))


Opakujte tyto kroky k vytvoření metody s názvem `FillByPriceLessThan` a `GetProductsWithPriceLessThan` v `ProductsTableAdapter` pro `GetProductsWithPriceLessThan` spravované uložené procedury.

19 obrázek ukazuje snímek obrazovky návrháře DataSet po přidání metody, které se `ProductsTableAdapter` pro `GetDiscontinuedProducts` a `GetProductsWithPriceLessThan` spravované uložené procedury.


[![ProductsTableAdapter obsahuje nové metody přidat v tomto kroku](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image41.png)

**Obrázek 19**: `ProductsTableAdapter` zahrnuje nové metody přidat v tomto kroku ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Krok 7: Přidání metody odpovídající na vrstvu obchodní logiky

Teď, když aktualizovali jsme Data Access Layer zahrnout metod pro volání spravované uložené procedury přidat kroky 4 a 5, je potřeba přidat odpovídající metody do vrstvy obchodní logiky. Přidejte následující dvě metody, které `ProductsBLLWithSprocs` třídy:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample7.vb)]

Obě metody jednoduše volat metodu odpovídající DAL a vrátit `ProductsDataTable` instance. `DataObjectMethodAttribute` Značek výše každá metoda způsobí, že tyto metody, které mají být zahrnuty v rozevíracím seznamu vyberte kartě Průvodce zdrojem dat Confgure s ObjectDataSource.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Krok 8: Vyvolání spravovaný uložené procedury od prezentační vrstvy

S obchodní logiku a Data přístup vrstvy zahrnují podporu pro volání rozšíření, `GetDiscontinuedProducts` a `GetProductsWithPriceLessThan` spravované uložené procedury, můžete, zobrazuje se teď tyto uložené procedury výsledky prostřednictvím stránky ASP.NET.

Otevřete `ManagedFunctionsAndSprocs.aspx` stránku `AdvancedDAL` složku a ze sady nástrojů, přetáhněte GridView na návrháře. Nastavit GridView s `ID` vlastnost `DiscontinuedProducts` a z jeho inteligentních značek navázat jej na nové ObjectDataSource s názvem `DiscontinuedProductsDataSource`. Konfigurace ObjectDataSource načítat data z `ProductsBLLWithSprocs` třídu s `GetDiscontinuedProducts` metoda.


[![Konfigurace ObjectDataSource použití třídy ProductsBLLWithSprocs](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image44.png)

**Obrázek 20**: Konfigurace ObjectDataSource pro použití `ProductsBLLWithSprocs` – třída ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image46.png))


[![Zvolte metodu GetDiscontinuedProducts z rozevíracího seznamu vyberte kartě](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image47.png)

**Obrázek 21**: Zvolte `GetDiscontinuedProducts` z rozevíracího seznamu na kartě vyberte metodu ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image49.png))


Vzhledem k tomu, že mřížce se použije k právě zobrazí informace o produktu, nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstraňte karty na (žádný) a pak klikněte na tlačítko Dokončit.

Po dokončení průvodce, Visual Studio automaticky přidá BoundField nebo vlastnost CheckBoxField pro každé datové pole v `ProductsDataTable`. Za chvíli odebrat všechna tyto pole s výjimkou `ProductName` a `Discontinued`, na který bodu vaší GridView a ObjectDataSource s deklarativní by měl vypadat podobně jako následující:


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample8.aspx)]

Chcete-li zobrazit tuto stránku prostřednictvím prohlížeče chvíli trvat. Když je navštívené stránky, volání ObjectDataSource `ProductsBLLWithSprocs` třídu s `GetDiscontinuedProducts` metoda. Jak jsme viděli v kroku 7, tato metoda volá na DAL s `ProductsDataTable` třídu s `GetDiscontinuedProducts` metodu, která volá `GetDiscontinuedProducts` uložené procedury. Tato uložená procedura je spravované uložené procedury a spouští kód, který jsme vytvořili v kroku 3, vrácení – starší formáty produkty.

Výsledky vrácené spravované uložené procedury jsou sbaleny do `ProductsDataTable` Dal a poté vrácen BLL, který vrátí prezentační vrstvy, kde jsou vázány na GridView a zobrazí. Podle očekávání, mřížky uvádí tyto produkty, které byla zrušena.


[![Jsou uvedené produkty nepoužívá](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image50.png)

**Obrázek 22**: jsou uvedené produkty zastaveny ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image52.png))


Pro další postup přidejte na stránku textové pole a jiné GridView. Tato rutina GridView zobrazit produkty menší než velikost zadaná do textového pole voláním mít `ProductsBLLWithSprocs` třídu s `GetProductsWithPriceLessThan` metoda.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Krok 9: Vytváření a volání funkce UDF T-SQL

Uživatelem definované funkce nebo funkce UDF, jsou databázové objekty úzce mimic sémantiku funkcí v programovacích jazyků. Jako funkce v jazyce Visual Basic můžete zahrnout proměnný počet vstupních parametrů UDF a vrátit hodnotu konkrétního typu. Uživatelem definovanou FUNKCI může vrátit buď skalární data – řetězec, celé číslo a tak dále – nebo tabulková data. Umožní rychle zobrazit na oba typy UDF, počínaje UDF, který vrací skalární datový typ s.

Následující UDF vypočítá odhadované hodnoty zásob pro konkrétní produkt. Dělá to tak, přičemž tři vstupní parametry - `UnitPrice`, `UnitsInStock`, a `Discontinued` hodnoty pro konkrétní produkt - a vrátí hodnotu typu `money`. Vypočítá odhadované hodnoty inventář vynásobením `UnitPrice` pomocí `UnitsInStock`. Pro – starší formáty položky je oproti jiným poloviční tuto hodnotu.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample9.sql)]

Jakmile tato UDF byl přidán do databáze, se nachází prostřednictvím Management Studio rozšířením programovatelnosti složku, pak funkce a pak funkce skalární hodnotu. Je možné v `SELECT` dotazu takto:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample10.sql)]

I přidali `udf_ComputeInventoryValue` UDF k databázi Northwind; Obrázek 23 zobrazí výstup výše uvedeného `SELECT` dotazu při zobrazení v Management Studio. Všimněte si také, že UDF je uveden v části funkce skalární hodnota složky v Průzkumníku objektů.


[![Každý produkt s inventáře hodnoty je uveden.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image53.png)

**Obrázek 23**: každý produkt s inventáře hodnoty je uveden ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image55.png))


Funkce UDF můžete se taky vrátit tabulková data. Například můžeme vytvořit UDF, který vrací produkty, které patří do určité kategorie:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample11.sql)]

`udf_GetProductsByCategoryID` UDF přijme `@CategoryID` vstupní parametr a vrací výsledky zadaného `SELECT` dotazu. Po vytvoření této UDF může být odkazováno v `FROM` (nebo `JOIN`) klauzuli `SELECT` dotazu. V následujícím příkladu by vrátit `ProductID`, `ProductName`, a `CategoryID` hodnoty pro každou vlastnost nápojů.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample12.sql)]

I přidali `udf_GetProductsByCategoryID` UDF k databázi Northwind; Obrázek 24 zobrazí výstup výše uvedeného `SELECT` dotazu při zobrazení v Management Studio. Funkce UDF, které vracejí tabulková data naleznete ve složce Průzkumník objektů s hodnotou tabulky funkcí.


[![ProductID, ProductName a CategoryID jsou uvedené pro každou nápoj](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image56.png)

**Obrázek 24**: `ProductID`, `ProductName`, a `CategoryID` jsou uvedené pro každou nápoj ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image58.png))


> [!NOTE]
> Další informace o vytváření a používání funkce UDF, podívejte se na [Úvod do funkce definované uživatelem](http://www.sqlteam.com/item.asp?ItemID=1955). Také si přečtěte [výhody a funkce Drawbacks of User-Defined](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).


## <a name="step-10-creating-a-managed-udf"></a>Krok 10: Vytvoření spravované UDF

`udf_ComputeInventoryValue` a `udf_GetProductsByCategoryID` UDF vytvořit v uvedených příkladech jsou databázové objekty, T-SQL. Také podporuje spravované UDF, které mohou být přidány do systému SQL Server 2005 `ManagedDatabaseConstructs` projektu stejně jako spravovaný uložené procedury z kroky 3 a 5. Pro tento krok mohli s implementovat `udf_ComputeInventoryValue` UDF ve spravovaném kódu.

Chcete-li přidat spravovaný UDF na `ManagedDatabaseConstructs` projektu, klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a vyberte možnost Přidat novou položku. Vyberte šablonu, uživatelem definované z dialogového okna Přidat novou položku a pojmenujte nový soubor UDF `udf_ComputeInventoryValue_Managed.vb`.


[![Přidat nové spravované UDF ManagedDatabaseConstructs projektu](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image59.png)

**Obrázek 25**: Přidání nové spravované UDF na `ManagedDatabaseConstructs` projektu ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image61.png))


Uživatelem definované funkce šablona vytváří `Partial` třídu s názvem `UserDefinedFunctions` se metoda, jejíž název je stejný jako název souboru s – třída (`udf_ComputeInventoryValue_Managed`, u této instance). Tato metoda je dekorované pomocí [ `SqlFunction` atribut](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), který flags – metoda jako spravované UDF.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample13.vb)]

`udf_ComputeInventoryValue` Metoda aktuálně vrací hodnotu [ `SqlString` objekt](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlstring.aspx) a nepřijímá žádné vstupní parametry. Je potřeba aktualizovat definici metody tak, aby přijímá tři vstupní parametry - `UnitPrice`, `UnitsInStock`, a `Discontinued` - a vrátí `SqlMoney` objektu. Logika pro výpočet inventáře hodnota je stejná jako v T-SQL `udf_ComputeInventoryValue` UDF.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample14.vb)]

Všimněte si, že vstupní parametry metody s UDF jsou jejich odpovídající typy SQL: `SqlMoney` pro `UnitPrice` pole, [ `SqlInt16` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlint16.aspx) pro `UnitsInStock`, a [ `SqlBoolean` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlboolean.aspx) pro `Discontinued`. Tyto datové typy odráží typy definované v `Products` tabulky: `UnitPrice` sloupec je typu `money`, `UnitsInStock` sloupec typu `smallint`a `Discontinued` sloupec typu `bit`.

Spustí kód tak, že vytvoříte `SqlMoney` instanci s názvem `inventoryValue` přiřazené s hodnotou 0. `Products` Tabulky umožňuje pro databázi `NULL` hodnoty ve `UnitsInPrice` a `UnitsInStock` sloupce. Proto je potřeba první kontrola chcete zobrazit, pokud tyto hodnoty obsahovat `NULL` s, který provedeme prostřednictvím `SqlMoney` objekt s [ `IsNull` vlastnost](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlmoney.isnull.aspx). Pokud oba `UnitPrice` a `UnitsInStock` obsahovat jinou hodnotu než`NULL` hodnoty, potom jsme výpočetní `inventoryValue` být součin dvou. Poté, pokud `Discontinued` hodnotu true, jsme polovinu hodnota.

> [!NOTE]
> `SqlMoney` Objektu umožňuje pouze dva `SqlMoney` instance vynásobí společně. Neumožňuje `SqlMoney` instance, která má být násobí hodnotou literálu číslo s plovoucí desetinnou čárkou. Proto na polovinu `inventoryValue` jsme vynásobíme ji novou `SqlMoney` instanci, která má hodnota 0,5.


## <a name="step-11-deploying-the-managed-udf"></a>Krok 11: Nasazení spravovaného UDF

Nyní, že byla vytvořena spravované UDF, jsme připravení nasadit k databázi Northwind. Jak jsme viděli v kroku 4, spravované objekty v projektu serveru SQL se nasazují pravým tlačítkem myši na název projektu v Průzkumníku řešení a zvolením možnosti nasazení v místní nabídce.

Jakmile jste nasadili projekt, vraťte se na SQL Server Management Studio a aktualizovat složku funkce vracející skalární hodnota. Teď byste měli vidět dvě položky:

- `dbo.udf_ComputeInventoryValue`-T-SQL UDF vytvořené v kroku 9, a
- `dbo.udf ComputeInventoryValue_Managed`-spravované UDF vytvořené v kroku 10, který byl právě nasazen.

K testování této spravované UDF, spusťte následující dotaz ze studia správy:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample15.sql)]

Tento příkaz používá spravovaný `udf ComputeInventoryValue_Managed` UDF místo T-SQL `udf_ComputeInventoryValue` UDF, ale výstup je stejný. Odkazovat zpět do 23 obrázek zobrazíte snímek obrazovky výstupu s UDF.

## <a name="step-12-debugging-the-managed-database-objects"></a>Krok 12: Ladění spravovaného databázové objekty

V [ladění uložené procedury](debugging-stored-procedures-vb.md) kurzu jsme probrali tři možnosti pro ladění systému SQL Server pomocí sady Visual Studio: přímé ladění databáze, ladění aplikací a ladění z projektu SQL Server. Spravovat databáze objekty nemůžete ladit prostřednictvím přímé ladění databáze, ale můžete ladit z klientské aplikace a přímo z projektu SQL Server. V pořadí pro ladění pracovat ale databázi systému SQL Server 2005 musí umožňovat ladění SQL nebo CLR. Odvolat, při prvním vytvoření `ManagedDatabaseConstructs` projektu sady Visual Studio zobrazí výzva, nám jestli jsme chtěli povolit SQL nebo CLR ladění (viz obrázek 6 v kroku 2). Toto nastavení můžete změnit kliknutím pravým tlačítkem na databázi z okna Průzkumníka serveru.


![Zajistěte, aby povolovalo databázi SQL nebo CLR ladění](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image62.png)

**Obrázek 26**: Zajistěte, aby povolovalo databázi SQL nebo CLR ladění


Představte si, že jsme chtěli ladění `GetProductsWithPriceLessThan` spravované uložené procedury. Jsme byste začali nastavením zarážky v kódu `GetProductsWithPriceLessThan` metoda.


[![Nastavte zarážky v metodě GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image63.png)

**Obrázek 27**: Nastavte zarážky v `GetProductsWithPriceLessThan` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image65.png))


Umožní s první pohled na ladění spravovaného databázové objekty z projektu SQL Server. Vzhledem k tomu, že naše řešení obsahuje dva projekty – `ManagedDatabaseConstructs` projekt SQL Server spolu s náš web – Pokud chcete ladit z projektu serveru SQL, potřebujeme dáte pokyn, aby Visual Studio spustíte `ManagedDatabaseConstructs` projekt SQL Server, když jsme spustit ladění. Klikněte pravým tlačítkem myši `ManagedDatabaseConstructs` projektu v Průzkumníku řešení a vyberte sadu jako spouštěný projekt možnost v místní nabídce.

Když `ManagedDatabaseConstructs` projektu je spuštěno z ladicí program se provede příkazy SQL v `Test.sql` souboru, který se nachází v `Test Scripts` složky. Například pro test `GetProductsWithPriceLessThan` spravované uložené procedury, nahradit existující `Test.sql` souboru obsahu pomocí následujícího příkazu, který volá `GetProductsWithPriceLessThan` spravované uložené procedury předávání v `@CategoryID` hodnotu 14.95:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample16.sql)]

Jakmile jste již zadali skript výše do `Test.sql`, spustit ladění přechodem do nabídky ladění a zvolením spustit ladění nebo stále mačkat F5 nebo zeleným přehrání ikonu na panelu nástrojů. Toto sestavení projekty v řešení, nasadí spravované databázové objekty k databázi Northwind a potom spusťte `Test.sql` skriptu. V tomto okamžiku bude dosaženo zarážce a jsme lze jednotlivé kroky `GetProductsWithPriceLessThan` metoda, zkontrolujte hodnoty vstupní parametry a tak dále.


[![Zarážka v metodě GetProductsWithPriceLessThan byl vybrán.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image66.png)

**Obrázek 28**: zarážka v `GetProductsWithPriceLessThan` byl vybrán – metoda ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image68.png))


Aby objekt databáze SQL chcete ladit prostřednictvím klientskou aplikaci je nutné, aby databáze nakonfigurovat podporu ladění aplikace. Klikněte pravým tlačítkem na databázi v Průzkumníku serveru a ujistěte se, že je zaškrtnuté možnosti ladění aplikace. Kromě toho je potřeba nakonfigurovat aplikace ASP.NET pro integraci s ladicím programem SQL a zakázat sdružování připojení. Tyto kroky byly podrobněji v kroku 2 [ladění uložené procedury](debugging-stored-procedures-vb.md) kurzu.

Po konfiguraci aplikace ASP.NET a databázi, nastavení webu ASP.NET jako projekt po spuštění a spuštění ladění. Pokud navštívíte stránky, který volá jeden spravovaných objektů, které má zarážku, aplikace se zastaví a řízení bude nutné předat k ladicí program, kde můžete krokovat kód jak ukazuje obrázek 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Krok 13: Ručně kompilace a nasazení spravované objekty databáze.

SQL Server projekty usnadňují vytváření, kompilace a nasazení spravovaného databázové objekty. Bohužel SQL Server projekty jsou k dispozici pouze v edicích sady Visual Studio Professional a systémy týmu. Pokud používáte Visual Web Developer nebo Standard Edition ze sady Visual Studio a chcete použít spravované databázové objekty, musíte ručně vytvořit a nasadit je. To zahrnuje čtyři kroky:

1. Vytvořte soubor, který obsahuje zdrojový kód pro objekt spravované databáze
2. Kompilace objektu do sestavení,
3. Registrace sestavení s databází systému SQL Server 2005 a
4. Vytvoření objektu databáze v systému SQL Server, který odkazuje na odpovídající metodu v sestavení.

Pro ilustraci tyto úlohy, umožní vytvořit nový s spravované uložené procedury, jež vrátí těchto produktů jehož `UnitPrice` je větší než zadaná hodnota. Vytvořte nový soubor v počítači s názvem `GetProductsWithPriceGreaterThan.vb` a zadejte následující kód do souboru (můžete sady Visual Studio, Poznámkový blok nebo libovolného textového editoru k tomu):


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample17.vb)]

Tento kód je téměř shodná s `GetProductsWithPriceLessThan` vytvořili v kroku 5. Pouze rozdíly jsou názvy metod `WHERE` klauzule a název parametru, který je použitý v dotazu. Zpět v `GetProductsWithPriceLessThan` metody `WHERE` klauzule číst: `WHERE UnitPrice < @MaxPrice`. Zde v `GetProductsWithPriceGreaterThan`, používáme: `WHERE UnitPrice > @MinPrice` .

Nyní potřebujeme zkompilovat tuto třídu do sestavení. Z příkazového řádku přejděte do adresáře, kam jste uložili `GetProductsWithPriceGreaterThan.vb` souboru a použít kompilátor jazyka C# (`csc.exe`) ke kompilaci souboru třídy do sestavení:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample18.cmd)]

Pokud složka obsahující v `bc.exe` v není v systému s `PATH`, budete muset plně odkazovat na jeho cesty `%WINDOWS%\Microsoft.NET\Framework\version\`, například takto:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample19.cmd)]


[![Kompilace GetProductsWithPriceGreaterThan.vb do sestavení](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image69.png)

**Obrázek 29**: zkompilovat `GetProductsWithPriceGreaterThan.vb` do sestavení ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image71.png))


`/t` Příznak určuje, že má být soubor třídy jazyka Visual Basic zkompilován do knihovny DLL (nikoli spustitelného souboru). `/out` Příznak určuje název výsledné sestavení.

> [!NOTE]
> Místo kompilování `GetProductsWithPriceGreaterThan.vb` soubor třídy z příkazového řádku, můžete alternativně použít [Visual Basic Express Edition](https://msdn.microsoft.com/vstudio/express/vb/) nebo k vytvoření samostatného projektu knihovny tříd v aplikaci Visual Studio Standard Edition. S ren Jakub Lauritsen, poskytl takový Visual Basic Express Edition projekt s kódem pro `GetProductsWithPriceGreaterThan` uložené procedury a dva spravované uložené procedury a UDF vytvořili v kroku 3, 5 a 10. Projekt s ren S také zahrnuje T-SQL příkazy potřebné k přidání odpovídající databázové objekty.


Kód zkompilován do sestavení jsme připraveni k registraci sestavení v databázi systému SQL Server 2005. To lze provést pomocí T-SQL, pomocí příkazu `CREATE ASSEMBLY`, nebo prostřednictvím SQL Server Management Studio. Umožní s fokus pomocí nástroje Management Studio.

Z Management Studio rozbalte složku programovatelnosti databáze Northwind. Jeden z jeho podsložky je sestavení. Ručně přidat nové sestavení do databáze, klikněte pravým tlačítkem na složku sestavení a v místní nabídce zvolte nové sestavení. Tato zobrazí dialogové okno Nový sestavení pole (viz obrázek 30). Klikněte na tlačítko Procházet, vyberte `ManuallyCreatedDBObjects.dll` sestavení jsme právě kompilovat a potom klikněte na OK přidáte sestavení do databáze. Byste neměli vidět `ManuallyCreatedDBObjects.dll` sestavení v Průzkumníku objektů.


[![Přidat ManuallyCreatedDBObjects.dll sestavení do databáze](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image72.png)

**Obrázek 30**: Přidat `ManuallyCreatedDBObjects.dll` sestavení do databáze ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image74.png))


![ManuallyCreatedDBObjects.dll je uveden v Průzkumníku objektů](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image75.png)

**Obrázek 31**: `ManuallyCreatedDBObjects.dll` je uveden v Průzkumníku objektů


Když jsme přidali sestavení k databázi Northwind, musíme ještě přidružit uložená procedura s `GetProductsWithPriceGreaterThan` metoda v sestavení. K tomu, otevřete nové okno dotazu a spusťte následující skript:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample20.sql)]

Tím se vytvoří nový uloženou proceduru v databázi Northwind s názvem `GetProductsWithPriceGreaterThan` a přidruží ji spravované metoda `GetProductsWithPriceGreaterThan` (což je ve třídě `StoredProcedures`, se v sestavení `ManuallyCreatedDBObjects`).

Po provedení výše skriptu, aktualizujte složce uložené procedury v Průzkumníku objektů. Měli byste vidět nový záznam uložené procedury - `GetProductsWithPriceGreaterThan` -který má ikonu zámku vedle sebe. Pokud chcete vyzkoušet tuto uloženou proceduru, zadejte a v okně dotazu spusťte následující skript:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample21.sql)]

Jak ukazuje obrázek 32, výše uvedeném příkazu zobrazí informace o těchto produktů s `UnitPrice` větší než 24,95 $.


[![ManuallyCreatedDBObjects.dll je uveden v Průzkumníku objektů](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image76.png)

**Obrázek 32**: `ManuallyCreatedDBObjects.dll` je uveden v Průzkumníku objektů ([Kliknutím zobrazit obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image78.png))


## <a name="summary"></a>Souhrn

Microsoft SQL Server 2005 poskytuje integraci s CLR Common Language Runtime (), která umožňuje vytvořit pomocí spravovaného kódu objekty databáze. Dříve tyto objekty databáze může vytvářet pouze pomocí T-SQL, ale nyní jsme můžete vytvořit tyto objekty pomocí rozhraní .NET programovací jazyky jako jazyka Visual Basic. V tomto kurzu, které jsme vytvořili dva spravované uložené procedury a spravované uživatelem definované funkce.

Visual Studio s typu SQL Server projektu usnadňuje vytváření, kompilace a nasazování spravovaných databázové objekty. Kromě toho nabízí bohatou podporu ladění. Ale typy projektů serveru SQL jsou k dispozici pouze v edicích sady Visual Studio Professional a systémy týmu. Pro ty pomocí Visual Web Developer nebo Standard Edition ze sady Visual Studio, vytváření, kompilace a kroky nasazení je nutné provést ručně, jak jsme viděli v kroku 13.

Radostí programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Výhody a nevýhody uživatelsky definované funkce](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Vytváření objektů serveru SQL Server 2005 ve spravovaném kódu](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Vytváření aktivační události pomocí spravovaného kódu v systému SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Postupy: Vytvoření a spuštění CLR SQL serveru uložené procedury](https://msdn.microsoft.com/en-us/library/5czye81z(VS.80).aspx)
- [Postupy: Vytvoření a spuštění uživatelem definované funkce CLR SQL serveru](https://msdn.microsoft.com/en-us/library/w2kae45k(VS.80).aspx)
- [Postupy: Úprava `Test.sql` spuštění objektů SQL skriptu](https://msdn.microsoft.com/en-us/library/ms233682(VS.80).aspx)
- [Úvod uživatelem definované funkce](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Spravovaný kód a SQL Server 2005 (Video)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Referenční informace Transact-SQL](https://msdn.microsoft.com/en-us/library/aa299742(SQL.80).aspx)
- [Návod: Vytvoření uložené procedury ve spravovaném kódu](https://msdn.microsoft.com/en-us/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz se S ren Jakub Lauritsen. Kromě kontroly tohoto článku, S ren také vytvořit projekt Visual C# Express Edition zahrnuté v tomto článku s ke stažení ručně kompilaci spravovaného databázové objekty. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](debugging-stored-procedures-vb.md)
