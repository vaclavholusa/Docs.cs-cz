---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
title: Vytváření uložených procedur a uživatelem definované funkce se spravovaným kódem (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Microsoft SQL Server 2005 se integruje s .NET Common Language Runtime umožňující vývojářům vytváření databázových objektů pomocí spravovaného kódu. V tomto kurzu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 8be9a51b-ea6b-46c7-bfa2-476d9b14c24c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
msc.type: authoredcontent
ms.openlocfilehash: a2a4042303fe507af449e83e36f67f4624f579cc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371840"
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-vb"></a>Vytvoření uložených procedur a uživatelem definovaných funkcí spravovaným kódem (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_VB.zip) nebo [stahovat PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/datatutorial75vb1.pdf)

> Microsoft SQL Server 2005 se integruje s .NET Common Language Runtime umožňující vývojářům vytváření databázových objektů pomocí spravovaného kódu. Tento kurz ukazuje, jak vytvořit spravované uložené procedury a spravované uživatelem definované funkce s kódem jazyka Visual Basic nebo C#. Vidíme také, jak tyto edice sady Visual Studio umožňuje ladit tyto objekty spravované databáze.


## <a name="introduction"></a>Úvod

Použití databází, jako jsou s Microsoft SQL Server 2005 [Transact-Structured dotazovacího jazyka (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) pro vkládání, úpravy a načítají se data. Většina databázových systémů zahrnují konstrukce pro seskupení řadu příkazů SQL, které lze provést jako jeden, opakovaně použitelné jednotky. Uložené procedury jsou jedním z příkladů. Další je *uživatelsky definovaných funkcí*(UDF), konstrukce, která prozkoumáme podrobněji v kroku 9.

Ve své podstatě SQL je navržená pro práci se sadami dat. `SELECT`, `UPDATE`, A `DELETE` příkazy ze své podstaty platí pro všechny záznamy v tabulce odpovídající a jsou omezené jenom jejich `WHERE` klauzule. Ještě existuje mnoho funkcí jazyka, které jsou navržené pro práci s jedním záznamem najednou a manipulace s daty skalární. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) povolit pro sadu záznamů použít smyčka prostřednictvím postupně po jednom. Řetězec funkce pro zpracování jako `LEFT`, `CHARINDEX`, a `PATINDEX` práce s daty skalární. SQL také zahrnuje příkazech toku řízení jako `IF` a `WHILE`.

Před Microsoft SQL Server 2005 uložených procedur a uživatelem definovanými funkcemi může definovat pouze jako kolekci příkazů T-SQL. SQL Server 2005, ale bylo navrženo pro zajištění integrace s [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), což je modul runtime používá všechna sestavení rozhraní .NET. V důsledku toho uložených procedur a uživatelem definovanými funkcemi v databázi serveru SQL Server 2005 můžete vytvořit pomocí spravovaného kódu. To znamená můžete vytvořit uloženou proceduru nebo UDF jako metoda v třídě jazyka Visual Basic. Díky tomu těchto uložených procedurách a uživatelem definovanými funkcemi využívat základní funkce v rozhraní .NET Framework a ze své vlastní třídy.

V tomto kurzu se Zaměřujeme vytvoření spravované uložených procedur a uživatelsky definovaných funkcí a jak integrovat do našich databázi Northwind. Začínáme s let!

> [!NOTE]
> Spravované databázové objekty nabízejí několik výhod oproti jejich protějšky SQL. Jazyk kombinujícím funkce a znalosti a možnosti opakovaně používat existující kód a logiku jsou hlavní výhody. Ale spravované databázové objekty můžou mít méně efektivní při práci se sadami dat, které nezahrnují mnohem procesní logiky. Podrobnější informace o výhodách používání spravovaného kódu a T-SQL, najdete [výhody z používání spravovaného kódu k vytvoření databázových objektů](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>Krok 1: Přesunutí databáze Northwind z celkového počtu`App_Data`

Doposud všech našich kurzů pro soubor databáze Microsoft SQL Server 2005 Express Edition v s webové aplikace použili `App_Data` složky. Umístění databáze v `App_Data` zjednodušit distribuci a spuštění těchto kurzů a všechny soubory nebyly nalezeny v rámci jednoho adresáře vyžadováno žádné další kroky konfigurace k otestování tohoto kurzu.

Pro účely tohoto kurzu však umožňují s přesunout databázi Northwind z `App_Data` , zaregistrujte je explicitně v instanci databáze SQL Server 2005 Express Edition. Když jsme proveďte kroky v tomto kurzu s databází v `App_Data` složky, počet kroky probíhají mnohem jednodušší explicitně registrací databáze instanci databáze SQL Server 2005 Express Edition.

Stáhnout pro účely tohoto kurzu má dva databázové soubory – `NORTHWND.MDF` a `NORTHWND_log.LDF` – umístěný ve složce s názvem `DataFiles`. Pokud postupujete se vlastní implementaci v kurzech, zavřete sadu Visual Studio a přesunout `NORTHWND.MDF` a `NORTHWND_log.LDF` soubory z webu s `App_Data` složky mimo web. Jakmile se soubory databáze byly přesunuty do jiné složky, že budeme muset zaregistrovat databázi Northwind k instanci databáze SQL Server 2005 Express Edition. To můžete udělat v SQL Server Management Studio. Pokud máte bez – Express Edition z SQL Server 2005 nainstalovaná v počítači pravděpodobně již máte nainstalované sady Management Studio. Pokud pouze SQL Server 2005 Express Edition máte v počítači, věnujte chvíli ke stažení a instalaci [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Spusťte SQL Server Management Studio. Jak ukazuje obrázek 1, začne s dotazem, který server pro připojení k Management Studio. Zadejte název serveru localhost\SQLExpress, vyberte v rozevíracím seznamu ověřování ověřování Windows a klikněte na připojit.


![Připojte se k instanci příslušné databáze](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image1.png)

**Obrázek 1**: Připojte se k instanci příslušné databáze


Po připojení můžete uložit okno Průzkumníka objektů se zobrazí seznam informací o instanci databáze SQL Server 2005 Express Edition, včetně databází, informace o zabezpečení, možnosti správy a tak dále.

Budeme muset připojit databázi Northwind v `DataFiles` složky (nebo všude, kde jste přesunuli ji) na instanci databáze SQL Server 2005 Express Edition. Klikněte pravým tlačítkem na složku databází a zvolte možnost připojit v místní nabídce. Tím se otevře dialogové okno Připojit databáze. Klikněte na tlačítko Přidat, přejdete příslušnou `NORTHWND.MDF` souboru a klikněte na tlačítko OK. V tomto okamžiku vaše obrazovka by měla vypadat podobně jako na obrázku 2.


[![Připojte se k instanci příslušné databáze](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image2.png)

**Obrázek 2**: připojení k příslušné instanci databáze ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image4.png))


> [!NOTE]
> Při připojování k instanci SQL serveru 2005 Express Edition Management Studio dialogové okno Připojit databáze neumožňuje k podrobnostem adresáře profilu uživatele, jako například dokumenty. Proto se ujistěte se, že chcete umístit `NORTHWND.MDF` a `NORTHWND_log.LDF` soubory v adresáři profilu bez uživatele.


Klikněte na tlačítko OK připojit databázi. Zavře dialogové okno Připojit databáze a Průzkumník objektů zveřejnit teď právě připojené databázi. Je pravděpodobné Northwind databáze s názvem jako má `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Přejmenování databáze Northwind tak, že kliknete pravým tlačítkem na databázi a zvolíte přejmenovat.


![Přejmenování databáze Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image5.png)

**Obrázek 3**: přejmenování databáze Northwind


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Krok 2: Vytvoření nového řešení a projekt serveru SQL v sadě Visual Studio

Vytvoření spravované uložených procedur a uživatelem definovanými funkcemi v systému SQL Server 2005 napíšeme uložené procedury a UDF logic jako kód jazyka Visual Basic v třídě. Jakmile kód byl zapsán, budeme muset tato třída zkompilovat do sestavení ( `.dll` souboru), zaregistrovat sestavení s databází SQL serveru a pak vytvořte uloženou proceduru nebo objekt systému souborů UDF v databázi, která odkazuje na odpovídající metodu v sestavení. Tyto kroky můžete všechny provést ručně. Můžeme vytvářet kód v jakýkoli text editor, zkompilovat jej z příkazového řádku pomocí kompilátoru jazyka Visual Basic (`vbc.exe`), zaregistrujte ho pomocí databáze [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/library/ms189524.aspx) příkazu nebo ze sady Management Studio a přidejte uloženou procedura nebo objekt UDF podobné prostředky. Naštěstí verze Professional a systémy týmu sady Visual Studio zahrnují typ projektu SQL Server, který automatizuje tyto úlohy. V tomto kurzu jsme provede procesem vytváření spravované uložené procedury a UDF pomocí typu projektu serveru SQL.

> [!NOTE]
> Pokud používáte aplikaci Visual Web Developer nebo standardní edici sady Visual Studio, budete muset místo toho použít ruční metodu. Krok 13 obsahuje podrobné pokyny k provedení těchto kroků ručně. Neváhejte se číst kroky 2 až 12 před čtením kroku 13, protože tyto kroky zahrnují důležité systému SQL Server pokyny ke konfiguraci, které musí být použity bez ohledu na to, jakou verzi sady Visual Studio, kterou používáte.


Začněte tím, že otevření sady Visual Studio. V nabídce Soubor vyberte nový projekt pro zobrazení dialogového okna Nový projekt (viz obrázek 4). Podrobnostem a typu projektu databáze a pak ze šablon na pravé straně, zvolte Vytvořit nový projekt serveru SQL. Rozhodli pojmenujte tento projekt `ManagedDatabaseConstructs` a je umístěná v rámci řešení s názvem `Tutorial75`.


[![Vytvořte nový projekt serveru SQL](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image6.png)

**Obrázek 4**: Vytvořte nový projekt serveru SQL ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image8.png))


Klikněte na tlačítko OK v dialogovém okně Nový projekt pro vytvoření řešení a projekt SQL Server.

Projekt serveru SQL se váže na konkrétní databáze. V důsledku toho po vytvoření nového projektu serveru SQL jsme okamžitě výzva k zadání těchto informací. Obrázek 5 ukazuje dialogových oken nový odkaz na databázi, která se samo doplní, aby odkazoval na databázi Northwind, který jsme zaregistrovali v instanci databáze SQL Server 2005 Express Edition zpět v kroku 1.


![SQL Server projekt přidružit k databázi Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image9.png)

**Obrázek 5**: SQL Server projekt přidružit k databázi Northwind


Pokud chcete ladit spravované uložených procedur a uživatelem definovanými funkcemi, které vytvoříte v rámci tohoto projektu, musíme povolit SQL/CLR ladění podporu pro připojení. Pokaždé, když se přidružení projekt SQL Server s novou databázi (jako jsme to udělali v obrázek 5), Visual Studio se vás zeptá nám Pokud chcete povolit SQL/CLR ladění pro připojení (viz obrázek 6). Klikněte na tlačítko Ano.


![Povolit ladění SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image10.png)

**Obrázek 6**: Povolit SQL/CLR ladění


V tomto okamžiku nový projekt serveru SQL se přidala do řešení. Obsahuje složku s názvem `Test Scripts` pomocí souboru s názvem `Test.sql`, který se používá pro ladění spravované databázové objekty vytvořené v projektu. Podíváme se na ladění v kroku 12.

Můžete teď přidáme nové spravované uložených procedur a uživatelem definovanými funkcemi pro tento projekt, ale před umožňujeme s naší stávající webovou aplikaci nejprve vložení do řešení. V nabídce Soubor vyberte možnost Přidat a zvolte existující web. Přejděte do složky pro příslušný web a klikněte na tlačítko OK. Jak je vidět na obrázku 7, dojde k aktualizaci řešení, aby obsahovat dva projekty: na webu a `ManagedDatabaseConstructs` projekt SQL Server.


![V Průzkumníku řešení teď obsahuje dva projekty](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image11.png)

**Obrázek 7**: V Průzkumníku řešení teď obsahuje dva projekty


`NORTHWNDConnectionString` Hodnota v `Web.config` aktuálně odkazuje `NORTHWND.MDF` soubor `App_Data` složky. Protože jsme odebrali z této databáze `App_Data` a explicitně registrován v instanci databáze SQL Server 2005 Express Edition, musíme aktualizovat odpovídajícím způsobem `NORTHWNDConnectionString` hodnotu. Otevřít `Web.config` souboru v web a změňte `NORTHWNDConnectionString` hodnotu tak, aby načte připojovací řetězec: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Po této změně vašeho `<connectionStrings>` tématu `Web.config` by měl vypadat nějak takto:


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample1.xml)]

> [!NOTE]
> Jak je popsáno v [předchozím kurzu](debugging-stored-procedures-vb.md), při ladění objektu SQL Server z klientské aplikace, jako jsou webové stránky ASP.NET, musíme zakázat sdružování připojení. Připojovací řetězec výše uvedené zakáže sdružování připojení ( `Pooling=false` ). Pokud nemáte v plánu na ladění spravovaného uložených procedur a uživatelem definovanými funkcemi na webu technologie ASP.NET, povolte sdružování připojení.


## <a name="step-3-creating-a-managed-stored-procedure"></a>Krok 3: Vytvoření spravované uložené procedury

Chcete-li přidat spravované uložené procedury k databázi Northwind, musíme nejprve vytvořit uloženou proceduru jako metoda v projektu SQL Server. V Průzkumníku řešení klikněte pravým tlačítkem na `ManagedDatabaseConstructs` název projektu a zvolte Přidat novou položku. Tím se zobrazí dialogové okno Přidat novou položku, která obsahuje seznam typů spravované databázové objekty, které mohou být přidány do projektu. Jak ukazuje obrázek 8, to zahrnuje uložených procedur a uživatelsky definovaných funkcí, mimo jiné.

Umožní s Začněte přidáním uloženou proceduru, která jednoduše vrací všechny produkty, které byly zrušeny. Pojmenujte nový soubor uloženou proceduru `GetDiscontinuedProducts.vb`.


[![Přidat novou úložnou proceduru s názvem GetDiscontinuedProducts.vb](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image12.png)

**Obrázek 8**: Přidat nové uložené procedury s názvem `GetDiscontinuedProducts.vb` ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image14.png))


Tím se vytvoří nový soubor třídy jazyka Visual Basic s následujícím obsahem:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample2.vb)]

Všimněte si, že uložené procedury je implementovaný jako `Shared` metody v rámci `Partial` soubor třídy s názvem `StoredProcedures`. Kromě toho `GetDiscontinuedProducts` je upravená pomocí [ `SqlProcedure` atribut](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlprocedureattribute.aspx), který označuje metodu jako uloženou proceduru.

Následující kód vytvoří `SqlCommand` objekt a nastaví její `CommandText` k `SELECT` dotaz, který vrátí všechny sloupce z `Products` tabulku produktů, jehož `Discontinued` pole se rovná 1. Potom provede příkaz a odesílá výsledky zpět do klientské aplikace. Přidejte tento kód `GetDiscontinuedProducts` metody.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample3.vb)]

Všechny spravované databázové objekty mají přístup k [ `SqlContext` objekt](https://msdn.microsoft.com/library/ms131108.aspx) představující kontext volajícího. `SqlContext` Poskytuje přístup k [ `SqlPipe` objekt](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) prostřednictvím jeho [ `Pipe` vlastnost](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). To `SqlPipe` objektu se používá k ferry informace mezi databází systému SQL Server a volající aplikace. Jak již název napovídá, [ `ExecuteAndSend` metoda](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) provede předaným `SqlCommand` objektu a odesílá výsledky zpět do klientské aplikace.

> [!NOTE]
> Spravované databázové objekty jsou nejvhodnější pro uložených procedur a uživatelem definovanými funkcemi, které používají postupu logic namísto logiky založené na sadě. Procesní logiky zahrnuje práce se sadami dat na základě řádek po řádku nebo práce s daty skalární. `GetDiscontinuedProducts` Metoda jsme právě vytvořili, ale zahrnuje žádné Procedurální logika. Proto je by v ideálním případě možné implementovat jako T-SQL, které jsou uložené procedury. Je implementován jako spravované uložené procedury k předvedení kroky potřebné k vytvoření a nasazení spravované uložené procedury.


## <a name="step-4-deploying-the-managed-stored-procedure"></a>Krok 4: Nasazení spravované uložené procedury

S tímto kódem dokončení jsou připraveny k nasazení k databázi Northwind. Nasazení SQL serveru projektu kód se zkompiluje do sestavení, zaregistruje sestavení s databází a vytvoří odpovídající objekty v databázi a propojí je vhodné metody v sestavení. Konkrétní sada úlohy prováděné pomocí možnosti nasazení je přesněji států v kroku 13. Klikněte pravým tlačítkem na `ManagedDatabaseConstructs` projektu název v Průzkumníku řešení a zvolte možnost nasazení. Nasazení se však nezdaří s následující chybou: nesprávná syntaxe poblíž textu "Externí". Budete muset nastavit úroveň kompatibility aktuální databáze na vyšší hodnotu tuto funkci povolil. Zobrazit nápovědu pro uloženou proceduru `sp_dbcmptlevel`.

Této chybě dochází při pokusu o registraci sestavení pomocí databáze Northwind. Registrace sestavení s databází serveru SQL Server 2005, musí být úroveň kompatibility databáze s nastavena na hodnotu 90. Ve výchozím nastavení nové databáze systému SQL Server 2005 mají úroveň kompatibility 90. Databáze vytvořené ve službě Microsoft SQL Server 2000, ale mají výchozí úroveň kompatibility 80. Protože databázi Northwind byl zpočátku databáze Microsoft SQL Server 2000, její úroveň kompatibility je aktuálně nastavený na 80 a proto musí být zvýšena na 90-li se zaregistrovat spravované databázové objekty.

Pokud chcete aktualizovat úroveň kompatibility databáze s, otevřete okno Nový dotaz v aplikaci Management Studio a zadejte:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample4.sql)]

Klikněte na ikonu spustit v panelu nástrojů můžete spustit dotaz výše.


[![Aktualizovat úroveň kompatibility databáze Northwind s](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image15.png)

**Obrázek 9**: aktualizace s úrovní kompatibility databáze Northwind ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image17.png))


Po aktualizaci úroveň kompatibility, znovu nasaďte projekt SQL Server. Tentokrát nasazení měli provést bez chyb.

Vraťte se do systému SQL Server Management Studio, klikněte pravým tlačítkem na databázi Northwind v Průzkumníku objektů a vyberte příkaz Aktualizovat. V dalším kroku k podrobnostem programovatelnosti složky a potom rozbalte složku sestavení. Jak ukazuje obrázek 10 databázi Northwind teď obsahuje sestavení vygenerované `ManagedDatabaseConstructs` projektu.


![ManagedDatabaseConstructs sestavení je teď zaregistrované databázi Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image18.png)

**Obrázek 10**: `ManagedDatabaseConstructs` sestavení je teď zaregistrované databázi Northwind


Také rozbalte složku uložené procedury. Uvidíte uloženou proceduru s názvem `GetDiscontinuedProducts`. Tuto uloženou proceduru byl vytvořen tak, že proces nasazení a odkazuje na `GetDiscontinuedProducts` metodu `ManagedDatabaseConstructs` sestavení. Když `GetDiscontinuedProducts` spuštění uložené procedury, pak provede `GetDiscontinuedProducts` – metoda. Protože se jedná spravované uložené procedury nelze upravit Management Studio (proto na ikonu zámku vedle názvu uložené procedury).


![Uložená procedura GetDiscontinuedProducts je uveden ve složce uložené procedury](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image19.png)

**Obrázek 11**: `GetDiscontinuedProducts` uložená procedura je uveden ve složce uložené procedury


Je stále jeden další mezní musíme překonat před říkáme spravované uložené procedury: databáze je nakonfigurovaná zabránit spuštění spravovaného kódu. To můžete ověřit, že otevřete nové okno dotazu a provádění `GetDiscontinuedProducts` uložené procedury. Zobrazí se následující chybová zpráva: spuštění uživatelského kódu v rozhraní .NET Framework je zakázáno. Povolte možnost konfigurace clr povolena.

Zkontrolujte informace o konfiguraci databáze s Northwind, zadejte a spusťte příkaz `exec sp_configure` v okně dotazu. Ukazuje to, že modul clr povolené nastavení je aktuálně nastaven na hodnotu 0.


[![Clr povolené nastavení je aktuálně nastavený na hodnotu 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image20.png)

**Obrázek 12**: clr povolené nastavení je aktuálně nastavený na hodnotu 0 ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image22.png))


Všimněte si, že každé nastavení konfigurace v obrázek 12 má čtyři hodnoty uvedené s jeho: minimální a maximální hodnoty a konfiguraci a spuštění hodnoty. Aktualizovat konfigurační hodnota pro nastavení modulu clr povolena, spusťte následující příkaz:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample5.sql)]

Pokud znovu spustíte `exec sp_configure` uvidíte, že výše uvedený příkaz Aktualizovat konfigurační hodnota clr povolené nastavení s 1, ale, že spuštění hodnota je stále nastavena na hodnotu 0. Tato změna konfigurace projeví, potřebujeme spustit [ `RECONFIGURE` příkaz](https://msdn.microsoft.com/library/ms176069.aspx), která nastaví hodnotu spuštění aktuální hodnotu konfigurace. Jednoduše zadejte `RECONFIGURE` v okně dotazu a klikněte na ikonu spustit na panelu nástrojů. Pokud spustíte `exec sp_configure` teď by měl zobrazit hodnotu 1 pro clr povolené nastavení s konfiguračním a hodnoty.

S konfigurací clr povoleno dokončení, můžeme je připraveno ke spuštění spravovanou `GetDiscontinuedProducts` uložené procedury. V okně dotazu zadejte a spusťte příkaz `exec` `GetDiscontinuedProducts`. Volání uložené procedury způsobí, že odpovídající spravovaný kód v `GetDiscontinuedProducts` metodu provést. Tento kód problémy `SELECT` dotaz, který vrátí všechny produkty, které jsou ukončena a vrátí tato data do volající aplikace, která je v této instanci systému SQL Server Management Studio. Management Studio obdrží tyto výsledky a zobrazí ho v okně výsledky.


[![Ukončené GetDiscontinuedProducts uloženou proceduru vrátí všechny produkty](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image23.png)

**Obrázek 13**: `GetDiscontinuedProducts` uložené procedury vrátí všechny ukončena produkty ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Krok 5: Vytvoření spravované uložené procedury, které přijímají vstupní parametry

Mnoho dotazů a uložených procedur, které jsme vytvořili v rámci těchto kurzů jste použili *parametry*. Například v [vytváří se nové uložené procedury, pro zadané datové sady s objekty TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) kurzu jsme vytvořili uloženou proceduru s názvem `GetProductsByCategoryID` , přijato vstupní parametr s názvem `@CategoryID`. Uložená procedura vráceny všechny produkty jehož `CategoryID` pole odpovídající hodnotu zadané `@CategoryID` parametru.

Vytvoření spravované uložené procedury, která přijímá vstupní parametry, zadejte jednoduše tyto parametry v definici metody s. Pro znázornění, umožňují s přidat jiné spravované uložené procedury k `ManagedDatabaseConstructs` projekt s názvem `GetProductsWithPriceLessThan`. Přijme vstupní parametr určující cena této spravované uložené procedury a vrátí všechny produkty jehož `UnitPrice` pole je menší než hodnota parametru s.

Vytvoří novou úložnou proceduru přidejte do projektu, klikněte pravým tlačítkem na `ManagedDatabaseConstructs` název projektu a zvolte Přidat novou úložnou proceduru. Název souboru `GetProductsWithPriceLessThan.vb`. Jak jsme viděli v kroku 3, vytvoří se nový soubor třídy jazyka Visual Basic s metodu s názvem `GetProductsWithPriceLessThan` umístit `Partial` třídy `StoredProcedures`.

Aktualizace `GetProductsWithPriceLessThan` definici metody s tak, aby přijímala [ `SqlMoney` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) vstupní parametr s názvem `price` a napsat kód pro spuštění a vrácení výsledků dotazu:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample6.vb)]

`GetProductsWithPriceLessThan` Definici metody s a kód připomíná definice a pravidla `GetDiscontinuedProducts` metoda vytvořený v kroku 3. Pouze rozdíly `GetProductsWithPriceLessThan` metoda přijímá jako vstupní parametr (`price`), `SqlCommand` s dotaz obsahuje parametr (`@MaxPrice`), a parametru se přidá do `SqlCommand` s `Parameters` je kolekce a přiřazené hodnoty `price` proměnné.

Po přidání tohoto kódu, znovu nasaďte projekt SQL Server. V dalším kroku vraťte se na SQL Server Management Studio a aktualizujte složku uložené procedury. Měli byste vidět nový záznam, `GetProductsWithPriceLessThan`. Z okna dotazu zadejte a spusťte příkaz `exec GetProductsWithPriceLessThan 25`, který bude seznam všech produktů, méně než 25 USD, jak ukazuje obrázek 14.


[![Se zobrazují produkty v rámci 25 USD](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image26.png)

**Obrázek 14**: se zobrazují produkty v rámci 25 USD ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Krok 6: Volání spravované uložené procedury z vrstvy přístupu k datům

V tuto chvíli jsme přidali `GetDiscontinuedProducts` a `GetProductsWithPriceLessThan` spravované úložné procedury pro `ManagedDatabaseConstructs` projektu a mít je zaregistrované v databázi Northwind SQL serveru. Můžeme také vyvolat tyto spravované uložené procedury SQL Server Management Studio (viz obrázky 13 a 14). Naše technologie ASP.NET, aby spravované aplikace pro použití těchto uložených procedur, ale potřebujeme přidat přístup k datům a vrstvy obchodní logiky v architektuře. V tomto kroku přidáme dvě nové metody, které `ProductsTableAdapter` v `NorthwindWithSprocs` zadali datovou sadu, která byla původně vytvořena v [vytváří se nové uložené procedury, pro zadané datové sady s objekty TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) kurzu. V kroku 7 přidáme BLL odpovídající metody.

Otevřít `NorthwindWithSprocs` typované datové sady v sadě Visual Studio a začněte tak, že přidáte novou metodu pro `ProductsTableAdapter` s názvem `GetDiscontinuedProducts`. Chcete-li přidat novou metodu k objektu typu TableAdapter, klikněte pravým tlačítkem na název s TableAdapter v návrháři a v místní nabídce zvolte možnost přidat dotaz.

> [!NOTE]
> Protože jsme přesunuli z databáze Northwind `App_Data` složku pro instanci databáze SQL Server 2005 Express Edition, je nutné aktualizovat odpovídající připojovací řetězec v souboru Web.config, aby obsahovala tuto změnu. V kroku 2 jsme zmínili, aktualizuje `NORTHWNDConnectionString` hodnota v `Web.config`. Pokud jste zapomněli provést tuto aktualizaci, se zobrazí chybová zpráva nezdařilo se přidání dotazu. Nepodařilo se nalézt připojení `NORTHWNDConnectionString` pro objekt `Web.config` v dialogovém okně při pokusu o přidání nové metody pro TableAdapter. Chcete-li vyřešit tuto chybu, klikněte na tlačítko OK a potom přejděte ke `Web.config` a aktualizovat `NORTHWNDConnectionString` hodnoty, jak je popsáno v kroku 2. Potom zkuste znovu přidat metodu k objektu TableAdapter. Tentokrát ho mělo fungovat bez chyb.


Přidání nové metody, spustí se Průvodce konfigurací dotazu TableAdapter, který jsme použili v mnoha případech v posledních kurzy. Prvním krokem dotazem, abychom mohli určit, jak by měl TableAdapter přistupovat k databázi: pomocí příkazu SQL ad-hoc nebo přes novou nebo existující uložené procedury. Protože jsme již vytvořené a registrované `GetDiscontinuedProducts` spravované uložené procedury s databází, zvolte možnost použít existující uložené procedury možnost kliknu na další.


[![Zvolte možnost použít existující uložené procedury možnost](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image29.png)

**Obrázek 15**: Zvolte možnost použít existující uložené procedury možnost ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image31.png))


Na další obrazovce zobrazí výzvu nám pro uložené procedury, které metoda vyvolá. Zvolte `GetDiscontinuedProducts` spravované uloženou proceduru z rozevíracího seznamu kliknu na další.


[![Vyberte GetDiscontinuedProducts spravovaný uložené procedury](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image32.png)

**Obrázek 16**: vyberte `GetDiscontinuedProducts` spravované uložené procedury ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image34.png))


Potom jsme vyzváni k určení, jestli bude procedura vracet řádky, jedinou hodnotu nebo hodnotu nothing. Protože `GetDiscontinuedProducts` vrací sadu řádků ukončená produktu, vyberte možnost první (tabulková data) a klikněte na tlačítko Další.


[![Vyberte možnost tabulková Data](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image35.png)

**Obrázek 17**: možnost tabulkových dat ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image37.png))


Na poslední obrazovce průvodce umožňuje zadat názvy výsledný metod a vzory přístupu k datům používá. Ponechejte zaškrtnuto zaškrtávací políčka a název metody `FillByDiscontinued` a `GetDiscontinuedProducts`. Kliknutím na Dokončit dokončíte průvodce.


[![Název metody FillByDiscontinued a GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image38.png)

**Obrázek 18**: název metody `FillByDiscontinued` a `GetDiscontinuedProducts` ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image40.png))


Opakujte tyto kroky k vytvoření metody s názvem `FillByPriceLessThan` a `GetProductsWithPriceLessThan` v `ProductsTableAdapter` pro `GetProductsWithPriceLessThan` spravované uložené procedury.

Obrázek 19 ukazuje snímek obrazovky s návrháři datových sad, po přidání metody, které se `ProductsTableAdapter` pro `GetDiscontinuedProducts` a `GetProductsWithPriceLessThan` spravované uložené procedury.


[![ProductsTableAdapter zahrnuje nové metody přidat v tomto kroku](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image41.png)

**Obrázek 19**: `ProductsTableAdapter` zahrnuje nové metody přidat v tomto kroku ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Krok 7: Přidání vrstvy obchodní logiky odpovídající metody

Teď, když jsme aktualizovali vrstvy přístupu k datům zahrnout metody volání spravované uložené procedury kroky 4 a 5, potřebujeme přidat odpovídající metody do vrstvy obchodní logiky. Přidejte následující dvě metody, které `ProductsBLLWithSprocs` třídy:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample7.vb)]

Obě metody jednoduše zavolejte metodu odpovídající vrstvy DAL a vrátit `ProductsDataTable` instance. `DataObjectMethodAttribute` Značky nad jednotlivými metodami způsobí, že tyto metody mají být zahrnuty v rozevíracím seznamu vyberte kartě ObjectDataSource s zdroj dat Confgure průvodce.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Krok 8: Vyvolání spravovanou uložených procedur komponentami TableAdapter od prezentační vrstvy

Pomocí obchodní logiky a vrstvy přístupu k datům rozšíření zahrnují podporu pro volání `GetDiscontinuedProducts` a `GetProductsWithPriceLessThan` spravované uložené procedury, můžete teď zobrazíme tyto uložené procedury výsledky prostřednictvím stránky ASP.NET.

Otevřít `ManagedFunctionsAndSprocs.aspx` stránku `AdvancedDAL` složky a z panelu nástrojů přetáhněte GridView do návrháře. Nastavit prvek GridView s `ID` vlastnost `DiscontinuedProducts` a z inteligentních značek, jeho vazbu na nového prvku ObjectDataSource s názvem `DiscontinuedProductsDataSource`. Konfigurace ObjectDataSource přebírat jeho data ze `ProductsBLLWithSprocs` třída s `GetDiscontinuedProducts` metody.


[![Konfigurace ObjectDataSource pomocí třídy ProductsBLLWithSprocs](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image44.png)

**Obrázek 20**: Konfigurace ObjectDataSource k použití `ProductsBLLWithSprocs` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image46.png))


[![Vyberte z rozevíracího seznamu na kartě vyberte GetDiscontinuedProducts – metoda](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image47.png)

**Obrázek 21**: Zvolte `GetDiscontinuedProducts` z rozevíracího seznamu na kartě vyberte metodu ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image49.png))


Protože tato mřížka použijeme k tomu jenom zobrazit informace o produktu, nastavte rozevírací seznamy v UPDATE, INSERT a odstraňovat karty na (žádný) a pak klikněte na tlačítko Dokončit.

Po dokončení průvodce bude Visual Studio automaticky přidá vlastnost BoundField nebo třídě CheckBoxField pro každé datové pole v `ProductsDataTable`. Za chvíli odebrat všechna tato pole s výjimkou `ProductName` a `Discontinued`, kdy ukazatel vašeho ovládacího prvku GridView a prvku ObjectDataSource s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample8.aspx)]

Za chvíli zobrazení této stránky prostřednictvím prohlížeče. Když navštívené stránky, volání ObjectDataSource `ProductsBLLWithSprocs` třída s `GetDiscontinuedProducts` metody. Jak jsme viděli v kroku 7, tato metoda volá do s vrstvou DAL `ProductsDataTable` třída s `GetDiscontinuedProducts` metodu, která vyvolá `GetDiscontinuedProducts` uložené procedury. Tato uložená procedura je spravované uložené procedury a spouští kód, který jsme vytvořili v kroku 3, vrací odpojené produkty.

Výsledky vrácené spravované uložené procedury jsou sbaleny do `ProductsDataTable` vrstvou DAL a vrátili, aby BLL, který pak vrátí tyto adresy do prezentační vrstvy, kde jsou vázána na prvku GridView a zobrazeny. Podle očekávání, mřížky uvádí seznam těchto produktů, které byly zrušeny.


[![Jsou uvedeny produkty ukončena](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image50.png)

**Obrázek 22**: The ukončena produktů se nachází ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image52.png))


Pro další postup přidejte na stránku pro textové pole a jiného ovládacího prvku GridView. Mít tohoto ovládacího prvku GridView zobrazit produkty menší než velikost zadat do textového pole voláním `ProductsBLLWithSprocs` třída s `GetProductsWithPriceLessThan` metody.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Krok 9: Vytvoření a volání uživatelem definovanými funkcemi jazyka T-SQL

Uživatelem definované funkce a uživatelem definovanými funkcemi, jsou databázové objekty úzce mimic sémantiky funkce v programovacích jazycích. Jako funkce v jazyce Visual Basic můžete zahrnout proměnný počet vstupních parametrů funkcí UDF a vrátit hodnotu konkrétního typu. Uživatelem definovanou FUNKCI může vrátit buď skalárních datových – řetězec, celé číslo a tak dále - nebo tabulková data. Umožní rychle zkontrolovat na oba typy uživatelem definovanými funkcemi, počínaje uživatelem definovanou FUNKCI vracející skalární datový typ s.

Následující UDF vypočítá odhadovanou hodnotu inventáře pro konkrétní produkt. Dělá to tak ve vstupním tři parametry – `UnitPrice`, `UnitsInStock`, a `Discontinued` hodnoty pro konkrétní produkt – a vrátí hodnotu typu `money`. Předpokládanou hodnotu inventář, který vypočítává tak, `UnitPrice` podle `UnitsInStock`. Ukončená položek je zkrátili na polovinu tuto hodnotu.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample9.sql)]

Po tomto UDF byl přidán do databáze, nachází se Management Studio rozbalením programovatelnosti složku, pak funkce a pak skalární hodnota funkce. Je možné v `SELECT` dotazu se takto:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample10.sql)]

Po přidání `udf_ComputeInventoryValue` UDF na databázi Northwind. Obrázek 23 ukazuje výstup z výše uvedených `SELECT` dotaz při prohlížení Management Studio. Všimněte si také, že je UDF uvedeny ve složce skalární hodnota funkce v Průzkumníku objektů.


[![Uvedené jednotlivé produkty s hodnotami inventáře](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image53.png)

**Obrázek 23**: uvedené hodnoty zásob s každý produkt ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image55.png))


UDF může také vrátit tabulková data. Například můžeme vytvořit UDF, která vrací produkty, které patří do určité kategorie:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample11.sql)]

`udf_GetProductsByCategoryID` Přijímá UDF `@CategoryID` vstupní parametr a vrací výsledky zadaného `SELECT` dotazu. Po vytvoření tohoto UDF může být odkazováno v `FROM` (nebo `JOIN`) klauzuli `SELECT` dotazu. Následující příklad vrátí `ProductID`, `ProductName`, a `CategoryID` hodnoty pro každou vlastnost nápojů.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample12.sql)]

Po přidání `udf_GetProductsByCategoryID` UDF na databázi Northwind. Obrázek 24 ukazuje výstup z výše uvedených `SELECT` dotaz při prohlížení Management Studio. Uživatelem definovanými funkcemi, které vrátí tabulková data nachází ve složce Průzkumník objektů s hodnotou tabulky funkcí.


[![ProductID, ProductName a ID kategorie jsou uvedeny pro každý nápoje](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image56.png)

**Obrázek 24**: `ProductID`, `ProductName`, a `CategoryID` jsou uvedeny pro každý nápoje ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image58.png))


> [!NOTE]
> Další informace o vytváření a používání funkcí UDF, projděte si [Úvod do uživatelsky definovaných funkcí](http://www.sqlteam.com/item.asp?ItemID=1955). Také si přečtěte [výhody a funkce Drawbacks of User-Defined](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).


## <a name="step-10-creating-a-managed-udf"></a>Krok 10: Vytvoření spravované UDF

`udf_ComputeInventoryValue` a `udf_GetProductsByCategoryID` UDF vytvořený v předchozích příkladech jsou databázové objekty jazyka T-SQL. Také podporuje spravované uživatelem definovanými funkcemi, které mohou být přidány do systému SQL Server 2005 `ManagedDatabaseConstructs` projektu stejně jako spravovanou uložených procedur z kroky 3 a 5. V tomto kroku umožní s implementovat `udf_ComputeInventoryValue` UDF ve spravovaném kódu.

Přidání spravovaných UDF na `ManagedDatabaseConstructs` projektu, klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a zvolte Přidat novou položku. Vyberte šablonu uživatelsky definovaná z dialogového okna Přidat novou položku a pojmenujte nový soubor UDF `udf_ComputeInventoryValue_Managed.vb`.


[![Přidat nové spravované UDF ManagedDatabaseConstructs projektu](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image59.png)

**Obrázek 25**: přidání nových spravovaných UDF na `ManagedDatabaseConstructs` projektu ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image61.png))


Šablona uživatelsky definovaná funkce vytvoří `Partial` třídu s názvem `UserDefinedFunctions` s metodou, jejíž název je stejný jako soubor s názvem třídy (`udf_ComputeInventoryValue_Managed`, v tomto případě). Tato metoda je upravený pomocí [ `SqlFunction` atribut](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), která označí metodu jako spravované UDF.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample13.vb)]

`udf_ComputeInventoryValue` Metoda aktuálně vrací hodnotu [ `SqlString` objekt](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) a nepřijímá žádné vstupní parametry. Potřebujeme aktualizovat definici metody tak, aby přijímá tři parametry – vstup `UnitPrice`, `UnitsInStock`, a `Discontinued` – a vrátí `SqlMoney` objektu. Logika pro výpočet hodnoty inventáře je stejná jako v T-SQL `udf_ComputeInventoryValue` UDF.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample14.vb)]

Všimněte si, že vstupní parametry metody s UDF jsou jejich odpovídající typy SQL: `SqlMoney` pro `UnitPrice` pole, [ `SqlInt16` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) pro `UnitsInStock`, a [ `SqlBoolean` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) pro `Discontinued`. Tyto datové typy zahrnují typy definované v `Products` tabulky: `UnitPrice` sloupec je typu `money`, `UnitsInStock` sloupec typu `smallint`a `Discontinued` sloupec typu `bit`.

Spustí kód tak, že vytvoříte `SqlMoney` instanci s názvem `inventoryValue` , která je přiřazena hodnota 0. `Products` Umožňuje tabulky pro databázi `NULL` hodnoty v `UnitsInPrice` a `UnitsInStock` sloupce. Proto musíme první kontrola zobrazíte, pokud tyto hodnoty obsahují `NULL` s, která můžeme udělat prostřednictvím `SqlMoney` objektu s [ `IsNull` vlastnost](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx). Pokud mají oba `UnitPrice` a `UnitsInStock` obsahovat jinou hodnotu než`NULL` hodnoty, pak můžeme následujícím způsobem vypočítat `inventoryValue` bude součin dvou. Když se poté `Discontinued` má hodnotu true, pak jsme polovinu hodnotu.

> [!NOTE]
> `SqlMoney` Objektu umožňuje pouze dvě `SqlMoney` instancí se navzájem vynásobeny. Neumožňuje `SqlMoney` instance bude vynásobené hodnotou literálu číslo s plovoucí desetinnou čárkou. Proto se na polovinu `inventoryValue` jsme vynásobit nová `SqlMoney` instanci, která se má hodnota 0,5.


## <a name="step-11-deploying-the-managed-udf"></a>Krok 11: Nasazení spravované UDF

Teď, když, že se vytvořila spravované UDF, jsme připraveni k nasazení k databázi Northwind. Jak jsme viděli v kroku 4, jsou nasazené spravované objekty v SQL serveru projektu kliknutím pravým tlačítkem na název projektu v Průzkumníku řešení a vyberete možnost nasazení v místní nabídce.

Jakmile jste nasadili projekt, vraťte se na SQL Server Management Studio a aktualizujte složku funkce vracející skalární hodnota. Teď byste měli vidět dvě položky:

- `dbo.udf_ComputeInventoryValue` -T-SQL UDF vytvořené v kroku 9, a
- `dbo.udf ComputeInventoryValue_Managed` -spravované UDF vytvořené v kroku 10, který byl právě nasazen.

K otestování této spravované UDF, spusťte následující dotaz z v aplikaci Management Studio:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample15.sql)]

Tento příkaz používá spravovanou `udf ComputeInventoryValue_Managed` UDF místo T-SQL `udf_ComputeInventoryValue` UDF, ale výstup je stejný. Vraťte se do 23 obrázek zobrazíte snímek obrazovky s výstupem UDF.

## <a name="step-12-debugging-the-managed-database-objects"></a>Krok 12: Ladění spravované databázové objekty

V [ladění uložených procedur](debugging-stored-procedures-vb.md) kurzu jsme probírali tři možnosti pro ladění SQL serveru pomocí sady Visual Studio: přímé ladění databáze, ladění aplikací a ladění z projektu SQL Server. Spravovaná databáze objekty nejde ladit, prostřednictvím přímé ladění databáze, ale lze ladit z klientské aplikace a přímo z projektu SQL Server. V pořadí pro ladění pro práci ale databáze systému SQL Server 2005 musí umožňovat SQL/CLR ladění. Vzpomeňte si, že při prvním vytvoření `ManagedDatabaseConstructs` projektu sady Visual Studio výzva nás, jestli jsme chtěli povolit SQL/CLR ladění (viz obrázek 6 v kroku 2). Toto nastavení lze změnit kliknutím pravým tlačítkem na databázi z okna Průzkumníka serveru.


![Ujistěte se, že databáze umožňuje ladění SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image62.png)

**Obrázek 26**: Ujistěte se, že databáze umožňuje ladění SQL/CLR


Představte si, že jsme chtěli ladění `GetProductsWithPriceLessThan` spravované uložené procedury. Začít nastavením zarážky v kódu `GetProductsWithPriceLessThan` metody.


[![Nastavte zarážku v metodě GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image63.png)

**Obrázek 27**: nastavit zarážku `GetProductsWithPriceLessThan` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image65.png))


Umožní s nejdřív se podívejte na ladění spravované databázové objekty z projektu SQL Server. Protože naše řešení obsahuje dva projekty – `ManagedDatabaseConstructs` projekt SQL Server spolu s našeho webu – Pokud chcete ladit z projektu SQL Server, potřebujeme dáte pokyn, aby Visual Studio ke spuštění `ManagedDatabaseConstructs` projektu serveru SQL, když jsme spustit ladění. Klikněte pravým tlačítkem myši `ManagedDatabaseConstructs` projektu v Průzkumníku řešení a zvolte sklad jako spouštěný projekt možnost v místní nabídce.

Když `ManagedDatabaseConstructs` projekt je spustit tak, že ladicí program provede příkazy SQL v `Test.sql` soubor, který je umístěn v `Test Scripts` složky. Například k testování `GetProductsWithPriceLessThan` spravované uložené procedury, nahraďte existující `Test.sql` souboru obsahu pomocí následujícího příkazu, který vyvolá `GetProductsWithPriceLessThan` spravované uložené procedury předávajícího `@CategoryID` hodnota 14.95:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample16.sql)]

Po zadání jste již výše uvedeného skriptu do `Test.sql`, spustit ladění tak, že přejdete do nabídky ladění a zvolíte, spustit ladění nebo stisknutím klávesy F5 nebo zelený přehrát ikonu na panelu nástrojů. To se projekty v řešení sestavit, nasadit spravované databázové objekty k databázi Northwind a následné provádění `Test.sql` skriptu. V tomto okamžiku bude dosaženo zarážkou a jsme krokovat `GetProductsWithPriceLessThan` metoda, zkontrolujte hodnoty vstupní parametry a tak dále.


[![Byla aktivována zarážka v metodě GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image66.png)

**Obrázek 28**: The zarážku v `GetProductsWithPriceLessThan` bylo dosaženo – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image68.png))


Aby objekt databáze SQL k ladění pomocí klientské aplikace je nutné konfiguraci této databáze v zájmu podpory ladění aplikace. Klikněte pravým tlačítkem na databázi v Průzkumníku serveru a ujistěte se, že je zaškrtnutá možnost ladění aplikací. Kromě toho budeme potřebovat ke konfiguraci aplikace ASP.NET pro integraci s ladicím programem SQL a chcete zakázat sdružování připojení. Tyto kroky se podrobně popsány v kroku 2 [ladění uložených procedur](debugging-stored-procedures-vb.md) kurzu.

Po konfiguraci aplikace ASP.NET a databáze webu ASP.NET nastavit jako spouštěný projekt a spusťte ladění. Pokud navštívíte stránku, která volá jedno ze spravovaných objektů, které obsahuje zarážku, aplikace se zastaví a ovládací prvek bude převeden ladicímu programu, ve kterém můžete krokovat kód jak ukazuje obrázek 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Krok 13: Ručně sestavování a nasazování spravovaných databázových objektů

Projekty systému SQL Server usnadňují vytváření, kompilaci a nasazení spravované databázové objekty. Bohužel projekty systému SQL Server jsou k dispozici pouze v edicích systémů týmu a Professional sady Visual Studio. Pokud používáte aplikaci Visual Web Developer nebo standardní edici sady Visual Studio a chcete použít spravované databázové objekty, musíte ručně vytvořit a nasadit. To zahrnuje čtyři kroky:

1. Vytvořit soubor, který obsahuje zdrojový kód pro spravované databázový objekt
2. Zkompilovat do sestavení, objekt
3. Registrace sestavení s databází serveru SQL Server 2005 a
4. Vytvoření objektu databáze v systému SQL Server, který odkazuje na příslušnou metodu v sestavení.

Pro ilustraci tyto úlohy, ať s vytvořte nový spravované uložené procedury, jež vrátí tyto produkty jehož `UnitPrice` je větší než zadanou hodnotou. Vytvořte nový soubor na počítač s názvem `GetProductsWithPriceGreaterThan.vb` a zadejte následující kód do souboru (můžete použít Visual Studio, programu Poznámkový blok nebo libovolného textového editoru k tomu):


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample17.vb)]

Tento kód je téměř shodná s `GetProductsWithPriceLessThan` metoda vytvořili v kroku 5. Pouze rozdíly jsou názvy metod `WHERE` klauzule a název parametru použitý v dotazu. Zpět v `GetProductsWithPriceLessThan` metody `WHERE` klauzule čtení: `WHERE UnitPrice < @MaxPrice`. Tady v `GetProductsWithPriceGreaterThan`, používáme: `WHERE UnitPrice > @MinPrice` .

Nyní potřebujeme zkompilovat do sestavení této třídy. Z příkazového řádku přejděte do adresáře, kam jste uložili `GetProductsWithPriceGreaterThan.vb` a kompilátor jazyka C# (`csc.exe`) ke kompilaci souboru třídy do sestavení:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample18.cmd)]

Pokud ve složce obsahující v `bc.exe` v není v systému s `PATH`, budete muset plně odkazovat na jeho cesty `%WINDOWS%\Microsoft.NET\Framework\version\`, takto:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample19.cmd)]


[![GetProductsWithPriceGreaterThan.vb zkompilovat do sestavení](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image69.png)

**Obrázek 29**: kompilace `GetProductsWithPriceGreaterThan.vb` do sestavení ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image71.png))


`/t` Příznak určuje, že soubor třídy jazyka Visual Basic by měl být zkompilován do knihovny DLL (spíše než spustitelného souboru). `/out` Příznak určuje název výsledné sestavení.

> [!NOTE]
> Místo kompilace `GetProductsWithPriceGreaterThan.vb` soubor třídy z příkazového řádku můžete také použít [Visual Basic Express Edition](https://msdn.microsoft.com/vstudio/express/vb/) nebo vytvořte samostatný projekt knihovny tříd ve standardní edice sady Visual Studio. S ren Jakub Lauritsen prosím poskytl tohoto jazyka Visual Basic Express Edition projektu s kódem `GetProductsWithPriceGreaterThan` uložené procedury a dva spravované uložené procedury a UDF vytvořené v kroku 3, 5 a 10. Projekt s ren S také obsahuje příkazy T-SQL, který je nutné přidat odpovídající databázové objekty.


Kód zkompilovat do sestavení jste připravení registrovat sestavení v rámci databáze systému SQL Server 2005. Můžete to provést prostřednictvím T-SQL, pomocí příkazu `CREATE ASSEMBLY`, nebo SQL Server Management Studio. Umožní s fokus pomocí nástroje Management Studio.

Ze sady Management Studio rozbalte složku programovatelnosti databáze Northwind. Jednou z jejích podsložkách je sestavení. Chcete-li ručně přidat nové sestavení do databáze, klikněte pravým tlačítkem na složku sestavení a nové sestavení zvolte v místní nabídce. Toto zobrazí dialogové okno nového sestavení pole (viz obrázek 30). Klikněte na tlačítko Procházet, vyberte `ManuallyCreatedDBObjects.dll` sestavení jsme právě zkompilován a potom klikněte na tlačítko OK sestavení přidejte do databáze. Neměli byste se setkat `ManuallyCreatedDBObjects.dll` sestavení v Průzkumníku objektů.


[![Přidání sestavení ManuallyCreatedDBObjects.dll do databáze](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image72.png)

**Obrázek 30**: Přidejte `ManuallyCreatedDBObjects.dll` sestavení do databáze ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image74.png))


![ManuallyCreatedDBObjects.dll je uveden v Průzkumníku objektů](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image75.png)

**Obrázek 31**: `ManuallyCreatedDBObjects.dll` je uveden v Průzkumníku objektů


Při sestavení jsme přidali k databázi Northwind, musíme ještě přidružit uloženou proceduru s `GetProductsWithPriceGreaterThan` metodu v sestavení. Chcete-li to provést, otevřete nové okno dotazu a spusťte následující skript:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample20.sql)]

Tím se vytvoří novou úložnou proceduru v databázi Northwind s názvem `GetProductsWithPriceGreaterThan` a přidruží ji k spravované metody `GetProductsWithPriceGreaterThan` (což je ve třídě `StoredProcedures`, což je v sestavení `ManuallyCreatedDBObjects`).

Po spuštění výše uvedeného skriptu, aktualizujte uložené procedury složku v Průzkumníku objektů. Zobrazí se nová položka uložená procedura – `GetProductsWithPriceGreaterThan` – který má ikonu zámku vedle sebe. Pokud chcete otestovat tuto uloženou proceduru, zadejte a v okně dotazu spusťte následující skript:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample21.sql)]

Jak ukazuje obrázek 32 výše uvedený příkaz zobrazí informace o těchto produktů se `UnitPrice` větší než 24,95 $.


[![ManuallyCreatedDBObjects.dll je uveden v Průzkumníku objektů](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image76.png)

**Obrázek 32**: `ManuallyCreatedDBObjects.dll` je uveden v Průzkumníku objektů ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image78.png))


## <a name="summary"></a>Souhrn

Microsoft SQL Server 2005 poskytuje integraci s CLR Common Language Runtime (), což umožňuje databázové objekty, které mají být vytvořeny pomocí spravovaného kódu. Dřív se tyto databázové objekty může vytvořit pouze pomocí jazyka T-SQL, ale teď můžeme vytvořit tyto objekty pomocí programovacích jazyků jako Visual Basic .NET. V tomto kurzu, který jsme vytvořili dva spravované uložené procedury a spravované uživatelem definované funkce.

Visual Studio s SQL Server projekt typu usnadňuje vytváření, sestavování a nasazování spravovaných databázových objektů. Kromě toho nabízí bohatou podporu ladění. Ale typy projektů SQL serveru jsou k dispozici pouze v edicích systémů týmu a Professional sady Visual Studio. Pro ty pomocí Visual Web Developer nebo standardní edici sady Visual Studio, vytváření, kompilace a kroky nasazení je nutné provést ručně, jak jsme viděli v kroku 13.

Všechno nejlepší programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Výhody a nevýhody uživatelsky definovaných funkcí](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Vytváření objektů serveru SQL Server 2005 ve spravovaném kódu](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Vytváření aktivačních událostí pomocí spravovaného kódu v systému SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Postupy: Vytvoření a spuštění modulu CLR SQL serveru uložené procedury](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Postupy: Vytvoření a spuštění uživatelem definované funkce CLR SQL serveru](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Postupy: Úpravy `Test.sql` skript ke spuštění objekty SQL](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Úvod do uživateli definované funkce](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Spravovaný kód a SQL Server 2005 (Video)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Referenční dokumentace jazyka Transact-SQL](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Návod: Vytvoření uložené procedury ve spravovaném kódu](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu se S ren Jakub Lauritsen. Kromě kontroly tohoto článku, S ren také vytvoření projektu Visual C# Express Edition zahrnuté v tomto článku s ke stažení pro ručně kompilování spravovaných databázových objektů. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](debugging-stored-procedures-vb.md)
