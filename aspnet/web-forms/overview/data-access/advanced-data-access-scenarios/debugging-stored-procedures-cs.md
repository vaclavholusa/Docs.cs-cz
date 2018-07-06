---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
title: Ladění uložených procedur (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Edice sady Visual Studio Professional a systém týmu vám umožní nastavit zarážky a vstoupit do uložených procedur v rámci SQL serveru, ladí uložené...
ms.author: aspnetcontent
ms.date: 08/03/2007
ms.assetid: c655c324-2ffa-4c21-8265-a254d79a693d
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
msc.type: authoredcontent
ms.openlocfilehash: e1eb9f6ea7d1704061157cca6898ad8330780929
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824209"
---
<a name="debugging-stored-procedures-c"></a>Ladění uložených procedur (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_CS.zip) nebo [stahovat PDF](debugging-stored-procedures-cs/_static/datatutorial74cs1.pdf)

> Edice sady Visual Studio Professional a systém týmu umožní nastavit zarážky a vstoupit do uložených procedur v rámci SQL serveru, vytváření, ladění uložených procedur stejně snadné jako ladění kódu aplikace. Tento kurz ukazuje přímé ladění databáze a ladění aplikací uložených procedur.


## <a name="introduction"></a>Úvod

Visual Studio nabízí bohaté možnosti ladění. Pomocí několika kliknutí myši, nebo stisknutí kláves ji možné používat zarážky pro zastavení provádění programu a zkontrolujte jeho stav a řízení toku s. Společně s laděním kódu aplikace Visual Studio nabízí podporu pro ladění uložených procedur z v rámci SQL serveru. Stejným způsobem, jako je možné nastavit zarážky v kódu ASP.NET použití modelu code-behind třídy nebo třídy vrstvy obchodní logiky, takže příliš může být vystavili v rámci uložené procedury.

V tomto kurzu se podíváme na krokování s vnořením do uložených procedur z Průzkumníka serveru Visual Studia a jak nastavit zarážky, které jsou přístupů při uložená procedura je volána z běžící aplikaci ASP.NET.

> [!NOTE]
> Uložené procedury bohužel pouze může být vkročili a ladit pomocí verze Professional a systémy týmu sady Visual Studio. Pokud používáte standardní verze sady Visual Studio nebo Visual Web Developer, určitě můžete číst podél projdeme kroky potřebné k ladění uložených procedur, ale nebudete mít k replikaci těchto kroků v tomto počítači.


## <a name="sql-server-debugging-concepts"></a>Koncepty ladění SQL serveru

Microsoft SQL Server 2005 je navržená k poskytování integrace s [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), což je modul runtime používá všechna sestavení rozhraní .NET. V důsledku toho systém SQL Server 2005 podporuje spravovanou databázovou objekty. To znamená můžete vytvořit databázové objekty, jako jsou uložených procedur a uživatelsky definovaných funkcí (UDF) jako metody ve třídě jazyka C#. Díky tomu těchto uložených procedurách a uživatelem definovanými funkcemi využívat základní funkce v rozhraní .NET Framework a ze své vlastní třídy. SQL Server 2005 samozřejmě, také poskytuje podporu pro databázové objekty jazyka T-SQL.

SQL Server 2005 nabízí podporu ladění pro T-SQL a spravované databázové objekty. Však tyto objekty lze pouze ladit pomocí edice sady Visual Studio 2005 Professional a systémy týmu. V tomto kurzu prozkoumáme ladění databázové objekty jazyka T-SQL. Následujícím kurzu zjistí ladění spravované databázové objekty.

[Přehled T-SQL a CLR ladění v systému SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) blogu z [týmu SQL Server 2005 CLR Integration](https://blogs.msdn.com/sqlclr/default.aspx) zvýrazní tři způsoby, chcete-li ladit objektů systému SQL Server 2005 ze sady Visual Studio:

- **Přímé ladění databáze (DDD)** – z Průzkumníka serveru můžete pustíme do jakékoli objekt databáze T-SQL, jako je například uložených procedur a uživatelem definovanými funkcemi. Prozkoumáme DDD v kroku 1.
- **Ladění aplikace** – můžeme nastavit zarážky v rámci objektu databáze a spusťte naši aplikaci ASP.NET. Pokud je spuštěn databázový objekt, bude dosaženo zarážkou a ovládacího prvku nastavená, aby ladicí program. Všimněte si, že s laděním aplikace jsme nelze krokovat s vnořením databázový objekt od kódu aplikace. Zarážky musí explicitně nastavené v těchto uložených procedur a uživatelem definovanými funkcemi kam chceme, aby se ladicí program zastaví. Ladění aplikací je zkontrolován v kroku 2.
- **Ladění z projektu SQL Server** – edice sady Visual Studio Professional a systémy týmu obsahovat typ projektu SQL Server, která se běžně používá k vytvoření spravované databázové objekty. Prozkoumáme pomocí projekty systému SQL Server a ladění jejich obsah v dalším kurzu.

Visual Studio dokáže ladit uložených procedur na místní a vzdálené instance systému SQL Server. Místní instanci systému SQL Server je ten, který je nainstalován ve stejném počítači jako Visual Studio. Pokud není databáze serveru SQL Server, který používáte na svém vývojovém počítači, považuje vzdálenou instanci. Těchto kurzech používáme místní instance systému SQL Server. Ladění uložených procedur ve vzdálené instanci SQL serveru vyžaduje další konfigurační kroky než při ladění uložených procedur na místní instanci.

Pokud použijete místní instanci systému SQL Server, můžete začněte krokem 1 a projít tento kurz na konec. Pokud používáte vzdálenou instanci SQL serveru, ale budete nejprve musíte zajistit, aby při ladění, které se protokolují do svého vývojového počítače s Windows uživatelský účet, který má přihlašovací jméno SQL serveru na vzdálenou instanci. Moveover, přihlášení k této databázi a přihlášení k databázi používaný pro připojení k databázi z běžící aplikaci technologie ASP.NET musí být členy `sysadmin` role. Naleznete v tématu ladění T-SQL databázové objekty v části Vzdálená instance na konci tohoto kurzu pro další informace o konfiguraci sady Visual Studio a ladit vzdálenou instanci systému SQL Server.

A konečně Pochopte, podporu pro databázové objekty jazyka T-SQL ladění není jako funkce bohaté jako podporu ladění pro aplikace .NET. Například podmínky zarážky a filtry nejsou podporovány, jen podmnožinu ladění systému windows jsou k dispozici, nemůžete použít funkce upravit a pokračovat, příkazovém vykreslením zbytečné a podobně. Zobrazit [omezení týkající se příkazy ladicího programu a funkce](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) Další informace.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Krok 1: Přímo krokování s vnořením do uložené procedury

Visual Studio usnadňuje přímo ladit databázový objekt. Umožní s, podívejte se na tom, jak pomocí funkce přímé ladění databáze (DDD) můžete krokovat s vnořením `Products_SelectByCategoryID` uloženou proceduru v databázi Northwind. Jak již název napovídá, `Products_SelectByCategoryID` vrátí informace o produktu pro určité kategorie; byl vytvořen [použití existující uložené procedury, pro zadané datové sady s objekty TableAdapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) kurzu. Začneme přejitím do Průzkumníka serveru a rozbalte uzel databáze Northwind. V dalším kroku k podrobnostem složky uložené procedury, klikněte pravým tlačítkem na `Products_SelectByCategoryID` uložené procedury a zvolte možnost krok do uloženou proceduru v místní nabídce. Tím se spustí ladicí program.

Protože `Products_SelectByCategoryID` uložená procedura očekává, že `@CategoryID` vstupní parametr jsme vyzve k zadání této hodnoty. Zadejte hodnotu 1, která vrátí informace o nápoje.


![Použijte hodnotu 1 pro @CategoryID parametr](debugging-stored-procedures-cs/_static/image1.png)

**Obrázek 1**: použijte hodnotu 1 pro `@CategoryID` parametr


Po zadání hodnoty pro `@CategoryID` parametr uložené procedury je proveden. Místo spuštění k dokončení, ale ladicí program zastaví spuštění na první příkaz. Všimněte si žlutá šipka na okraji označující aktuální umístění v uložené proceduře. Můžete zobrazit a upravit hodnoty parametrů prostřednictvím okna kukátka nebo najede myší název parametru v uložené proceduře.


[![Ladicí program se zastavil na první příkaz uložená procedura](debugging-stored-procedures-cs/_static/image3.png)](debugging-stored-procedures-cs/_static/image2.png)

**Obrázek 2**: The ladicí program se zastavil na první příkaz uloženou proceduru ([kliknutím ji zobrazíte obrázek v plné velikosti](debugging-stored-procedures-cs/_static/image4.png))


Chcete-li si jeden příkaz uloženou proceduru najednou, klikněte na tlačítko Krokovat s přeskočením na panelu nástrojů nebo stiskněte klávesu F10. `Products_SelectByCategoryID` Uložené procedury obsahuje jediný `SELECT` příkazu, takže tím F10 přesune se krokování přes jeden příkaz a dokončení provádění uložené procedury. Po dokončení uložené procedury, výstup se zobrazí v okně Výstup a ladicí program se ukončí.

> [!NOTE]
> T-SQL ladění dochází na úrovni příkazu; nelze krokování s vnořením `SELECT` příkazu.


## <a name="step-2-configuring-the-website-for-application-debugging"></a>Krok 2: Konfigurace webu pro ladění aplikací

Při ladění uloženou proceduru z Průzkumníka serveru po ruce, v mnoha scénářích nás zajímají více ladění uložené procedury, když je volána z naší aplikace technologie ASP.NET. Můžete přidat zarážky pro uloženou proceduru z Visual Studia a potom spusťte ladění aplikace ASP.NET. Při vyvolání uložené procedury se zarážkami z aplikace, provádění se zastaví na zarážce a můžeme zobrazit a změnit hodnoty parametrů uložené procedury s a projít jeho příkazy stejným způsobem, jako jsme to udělali v kroku 1.

Než můžeme začít ladění uložené procedury volané z aplikace, musíte dáte pokyn, aby webová aplikace ASP.NET pro integraci s ladicím programem systému SQL Server. Začněte tím, že pravým tlačítkem myši na název webu v Průzkumníku řešení (`ASPNET_Data_Tutorial_74_CS`). V místní nabídce zvolte možnost stránky vlastností, vyberte položku Možnosti spuštění na levé straně a zaškrtněte políčko systému SQL Server v části ladicí programy (viz obrázek 3).


[![Zaškrtněte políčko SQL serveru na stránkách vlastností s aplikací](debugging-stored-procedures-cs/_static/image6.png)](debugging-stored-procedures-cs/_static/image5.png)

**Obrázek 3**: Zaškrtněte políčko SQL serveru v aplikaci s stránky vlastností ([kliknutím ji zobrazíte obrázek v plné velikosti](debugging-stored-procedures-cs/_static/image7.png))


Kromě toho jsme muset aktualizovat připojovací řetězec databáze používané aplikací tak, aby sdružování připojení je zakázáno. Při připojení k databázi se ukončí, odpovídající `SqlConnection` objekt nachází ve fondu k dispozici připojení. Při navazování připojení k databázi, objekt k dispozici připojení můžete získat z tohoto fondu spíše než by bylo nutné vytvořit a vytvořit nové připojení. Tato sdružování připojení objekty je vylepšení výkonu a je ve výchozím nastavení povolené. Ale při ladění chceme, chcete-li vypnout sdružování připojení, protože ladění infrastruktury není správně navázán při práci s připojením, který byl odebrán z fondu.

Chcete-li sdružování připojení zakázáno, aktualizujte `NORTHWNDConnectionString` v `Web.config` tak, že obsahují nastavení `Pooling=false` .


[!code-xml[Main](debugging-stored-procedures-cs/samples/sample1.xml)]

> [!NOTE]
> Po dokončení ladění systému SQL Server prostřednictvím aplikace ASP.NET je potřeba obnovit odebráním sdružování připojení `Pooling` nastavení z připojovacího řetězce (nebo nastavením na `Pooling=true` ).


Aplikace ASP.NET je v tuto chvíli nakonfigurovaná umožňující Visual Studio pro ladění objektů databáze systému SQL Server při vyvolání prostřednictvím webové aplikace. Vše, co zůstává teď má přidejte zarážku na uložené procedury a spustit ladění!

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Krok 3: Přidání zarážky a ladění

Otevřít `Products_SelectByCategoryID` uložené procedury a nastavte zarážku na začátku `SELECT` příkaz kliknutím na okraji v příslušném umístění nebo umístěním kurzoru na začátku `SELECT` příkazu a stisknutím F9. Jak ukazuje obrázek 4, zarážka se zobrazí jako červená tečka na okraj.


[![Nastavit zarážku Products_SelectByCategoryID uložené procedury](debugging-stored-procedures-cs/_static/image9.png)](debugging-stored-procedures-cs/_static/image8.png)

**Obrázek 4**: nastavit zarážku `Products_SelectByCategoryID` uloženou proceduru ([kliknutím ji zobrazíte obrázek v plné velikosti](debugging-stored-procedures-cs/_static/image10.png))


Aby objekt databáze SQL k ladění pomocí klientské aplikace je nutné konfiguraci této databáze v zájmu podpory ladění aplikace. Při prvním nastavení zarážky, toto nastavení by měl automaticky přepnout, ale je vhodné zkontrolovat. Klikněte pravým tlačítkem na `NORTHWND.MDF` uzlu v Průzkumníku serveru. V místní nabídce by měl obsahovat checked nabídky položky s názvem ladění aplikací.


![Ujistěte se, že je povolená možnost ladění aplikací](debugging-stored-procedures-cs/_static/image11.png)

**Obrázek 5**: Ujistěte se, že je povolená možnost ladění aplikací


Nastavení zarážky a povolená možnost ladění aplikací jsou připravený k ladění uložené procedury při volání z aplikace ASP.NET. Spusťte ladicí program tak, že přejdete do nabídky ladění a Výběr stisknutím klávesy F5 nebo kliknutím na zelený spustit ladění, vyzkoušejte ikonu na panelu nástrojů. To se spustit ladicí program a spusťte web.

`Products_SelectByCategoryID` Uloženou proceduru byl vytvořen v [použití existující uložené procedury, pro zadané datové sady s objekty TableAdapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) kurzu. Jeho odpovídající webové stránky (`~/AdvancedDAL/ExistingSprocs.aspx`) obsahuje GridView zobrazující výsledky vrácené tuto uloženou proceduru. Navštivte tuto stránku v prohlížeči. Při dosažení stránky k zarážce v `Products_SelectByCategoryID` dosáhnou uložené procedury a ovládací prvek vrátí k sadě Visual Studio. Stejně jako v kroku 1, můžete krokovat pro uloženou proceduru s příkazy a zobrazit a upravit hodnoty parametrů.


[![Na stránce ExistingSprocs.aspx zpočátku zobrazí nápoje](debugging-stored-procedures-cs/_static/image13.png)](debugging-stored-procedures-cs/_static/image12.png)

**Obrázek 6**: `ExistingSprocs.aspx` stránky zpočátku zobrazí nápoje ([kliknutím ji zobrazíte obrázek v plné velikosti](debugging-stored-procedures-cs/_static/image14.png))


[![Uložená procedura s bylo dosaženo zarážky](debugging-stored-procedures-cs/_static/image16.png)](debugging-stored-procedures-cs/_static/image15.png)

**Obrázek 7**: bylo dosaženo zarážky s The uloženou proceduru ([kliknutím ji zobrazíte obrázek v plné velikosti](debugging-stored-procedures-cs/_static/image17.png))


Jako okno kukátka v obrázek 7 znázorňuje, hodnota `@CategoryID` parametru je 1. Důvodem je, že `ExistingSprocs.aspx` stránky otevření zobrazí produkty do kategorie Nápoje, který má `CategoryID` hodnotu 1. Z rozevíracího seznamu vyberte jinou kategorii. To způsobí, že zpětné volání a znovu spustí `Products_SelectByCategoryID` uložené procedury. Dosažení zarážky znovu, tentokrát ale `@CategoryID` parametr s hodnotou odpovídá položce vybrané rozevíracího seznamu s `CategoryID`.


[![Z rozevíracího seznamu zvolte jinou kategorii](debugging-stored-procedures-cs/_static/image19.png)](debugging-stored-procedures-cs/_static/image18.png)

**Obrázek 8**: z rozevíracího seznamu zvolte jinou kategorii ([kliknutím ji zobrazíte obrázek v plné velikosti](debugging-stored-procedures-cs/_static/image20.png))


[![@CategoryID Parametr odráží kategorii vybrané z webové stránky](debugging-stored-procedures-cs/_static/image22.png)](debugging-stored-procedures-cs/_static/image21.png)

**Obrázek 9**: `@CategoryID` parametr odráží vybrat kategorii z webové stránky ([kliknutím ji zobrazíte obrázek v plné velikosti](debugging-stored-procedures-cs/_static/image23.png))


> [!NOTE]
> Pokud k zarážce v `Products_SelectByCategoryID` uložená procedura není přístupů při návštěvě `ExistingSprocs.aspx` stránky, ujistěte se, že po kontrole zaškrtávací políčko systému SQL Server v části ladicích programů aplikace ASP.NET s vlastností, že byl sdružování připojení zakázané, a zda je povolena databáze s možností ladění aplikací. Pokud znovu stále dochází k problémům, restartujte Visual Studio a zkuste to znovu.


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Ladění objektů jazyka T-SQL Database na vzdálenou instancí

Ladění databázových objektů pomocí sady Visual Studio je poměrně jednoduché, když je instance databáze serveru SQL Server na stejném počítači jako Visual Studio. Ale pokud SQL Server a Visual Studio jsou umístěny na různých počítačích pak pozor, je vyžadována konfigurace zobrazíte vše funguje správně. Existují dva základní úkoly, které jsme se potýkají s:

- Ujistěte se, že přihlašovací údaje používané pro připojení k databázi pomocí ADO.NET patří do `sysadmin` role.
- Ujistěte se, že uživatelský účet Windows, který se používá sada Visual Studio na vývojovém počítači platný účet přihlášení serveru SQL Server, který patří do `sysadmin` role.

Prvním krokem je poměrně přímočarý. Nejprve, určete uživatelský účet použitý k připojení k databázi z aplikace v ASP.NET a poté z SQL Server Management Studio, přidejte tento přihlašovací účet, který `sysadmin` role.

Druhá úloha vyžaduje, aby uživatelský účet Windows, který používáte k ladění aplikace platné přihlašovací údaje na vzdálené databáze. Ale je pravděpodobné, že má účet Windows, kterým přihlášení do pracovní stanice s není platného přihlášení SQL serveru. Místo přidávání účtu konkrétní přihlášení k systému SQL Server, je lepší volbou je určit některé Windows uživatelský účet jako účet ladění systému SQL Server. Chcete-li ladit databázových objektů vzdálené instance systému SQL Server, by pak spusťte Visual Studio pomocí přihlašovacích údajů tohoto Windows přihlašovací účet s.

Příklad by měly pomoci objasnit věci. Představte si, že je účet Windows s názvem `SQLDebug` v rámci domény Windows. Tento účet by bylo potřeba přidat do vzdálené instance systému SQL Server jako platné přihlašovací údaje a člen `sysadmin` role. Potom, chcete-li ladit vzdálenou instanci systému SQL Server ze sady Visual Studio, musíme spuštění sady Visual Studio jako `SQLDebug` uživatele. To může být provádí protokolování z našich pracovní stanice, přihlaste se zpět jako `SQLDebug`, a potom spustí Visual Studio, ale jednodušší přístup může být přihlášení k naší pracovní stanice pomocí vlastní přihlašovací údaje a pak použijte `runas.exe` ke spuštění sady Visual Studio jako `SQLDebug` uživatele. `runas.exe` Umožňuje konkrétní aplikace, která provede v cestě guise jiný uživatelský účet. Spusťte sadu Visual Studio jako `SQLDebug`, můžete například zadat následující příkaz na příkazovém řádku:


[!code-console[Main](debugging-stored-procedures-cs/samples/sample2.cmd)]

Podrobnější vysvětlení o tomto procesu najdete v tématu [William R. Vaughn](http://betav.com/BLOG/billva/) s *Hitchhiker s příručka k sadě Visual Studio a SQL Server, sedmý Edition* stejně jako [postupy: nastavení oprávnění pro SQL Server pro ladění](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Pokud vývojovém počítači se systémem Windows XP Service Pack 2, budete muset nakonfigurovat bránu pro povolení vzdáleného ladění. [Jak do: Povolí ladění na SQL serveru 2005](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) poznámky článku to probíhá ve dvou krocích: (a) na hostitelském počítači Visual Studio, je nutné přidat `Devenv.exe` do seznamu výjimek a otevřete port TCP 135; a (b) na vzdáleném počítači (SQL), je nutné otevřít TCP 135 portu a přidejte `sqlservr.exe` do seznamu výjimek. Pokud zásady vaší domény vyžadují síťové komunikace, chcete-li to provést prostřednictvím protokolu IPSec, musíte otevřít porty UDP 4500 a UDP 500.


## <a name="summary"></a>Souhrn

Kromě toho, že podpora ladění pro kód aplikace .NET, Visual Studio také poskytuje širokou škálu možností pro SQL Server 2005 ladění. V tomto kurzu jsme se podívali na dvě z těchto možností: přímé ladění databáze a ladění aplikací. Přímo ladit databázový objekt jazyka T-SQL, vyhledejte objekt prostřednictvím Průzkumníka serveru pak klepněte na něj pravým tlačítkem myši a zvolte Krokovat s vnořením. Tím se spustí ladicí program a zastaví na prvním příkazem v objektu databáze, v tomto okamžiku můžete krokovat pro objekt s příkazy a zobrazit a upravit hodnoty parametrů. V kroku 1 jsme použili tento postup můžete krokovat s vnořením `Products_SelectByCategoryID` uložené procedury.

Ladění aplikací umožňuje zarážek pro nastavení přímo v rámci databázové objekty. Při vyvolání objektu databáze se zarážkami z klientské aplikace (například webové aplikace ASP.NET), program zastaví, protože ladicí program má. Ladění aplikací je užitečné, protože více jasně ukazuje, jakou akci aplikace způsobí, že databázových objektů má být volána. Ale vyžaduje trochu další konfigurace a nastavení než přímé ladění databáze.

Databázové objekty lze také ladit přes projekty systému SQL Server. Se podíváme na použití projekty systému SQL Server a jak se dají použít k vytváření a ladění spravované databázové objekty v dalším kurzu.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](protecting-connection-strings-and-other-configuration-information-cs.md)
> [další](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
