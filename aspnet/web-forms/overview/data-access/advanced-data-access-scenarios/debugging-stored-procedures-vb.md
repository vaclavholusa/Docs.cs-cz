---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
title: Ladění uložené procedury (VB) | Microsoft Docs
author: rick-anderson
description: Edice Visual Studio Professional a Team System umožňují nastavit zarážky a krok v k uloženým procedurám v systému SQL Server, vytváření, ladění, uložené...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 9ed8ccb5-5f31-4eb4-976d-cabf4b45ca09
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
msc.type: authoredcontent
ms.openlocfilehash: 3391a78eaeb0add46e75048069a614ba00628f67
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876446"
---
<a name="debugging-stored-procedures-vb"></a>Ladění uložené procedury (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_VB.zip) nebo [stáhnout PDF](debugging-stored-procedures-vb/_static/datatutorial74vb1.pdf)

> Edice Visual Studio Professional a Team System umožňují nastavit zarážky a kroku k uloženým procedurám v systému SQL Server, vytváření, ladění uložené procedury stejně snadná jako ladění kódu aplikace. Tento kurz představuje přímé databáze ladění, trasování a ladění aplikací uložených procedur.


## <a name="introduction"></a>Úvod

Visual Studio poskytuje bohaté prostředí ladění. S několika stisknutí kláves nebo kliknutí myší je možné použít zarážky zastavit provádění programu a zkontrolujte jeho stavu a řízení toku s. Společně s ladění kódu aplikace, Visual Studio nabízí podporu pro ladění uložené procedury z v systému SQL Server. Stejně jako lze nastavit zarážky v kódu aplikace ASP.NET kódu nebo třída vrstvu obchodní logiky, takže příliš jejich se může v rámci uložené procedury.

V tomto kurzu se podíváme na zanoříte se do uložené procedury z Průzkumníka serveru Visual Studia a jak nastavit zarážky, které jsou dosáhl Pokud uložená procedura je volána z spuštěná aplikace ASP.NET.

> [!NOTE]
> Uložené procedury bohužel pouze může být stupeň do a ladit prostřednictvím verze Professional a systémy týmu sady Visual Studio. Pokud používáte Visual Web Developer nebo standardní verzi sady Visual Studio, jste Vítejte přečtěte společně, protože jsme vás provede kroky potřebnými k ladění uložené procedury, ale nebudete moct replikovat na váš počítač tyto kroky.


## <a name="sql-server-debugging-concepts"></a>Koncepty ladění SQL serveru

Microsoft SQL Server 2005 byla navržená tak, aby nabízí integraci s [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), což je modul runtime používá všechna sestavení .NET. V důsledku toho systému SQL Server 2005 podporuje spravované databázové objekty. To znamená můžete vytvořit databázové objekty, jako jsou uložené procedury a uživatelem definované funkce (UDF) jako metody ve třídě jazyka Visual Basic. To umožňuje tyto uložené procedury a funkce UDF, abyste mohli využívat funkce v rozhraní .NET Framework a z vlastní třídy. Samozřejmě SQL Server 2005 taky poskytuje podporu pro databázové objekty T-SQL.

SQL Server 2005 nabízí podporu ladění pro T-SQL a spravované databázové objekty. Však můžete tyto objekty ladit pouze prostřednictvím edice Visual Studio 2005 Professional a systémy týmu. V tomto kurzu vyzkoušíme ladění T-SQL databázové objekty. Další kurz zjistí ladění spravovaného databázové objekty.

[Přehled T-SQL a CLR ladění SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) položku blogu z [integrace modulu CLR s aplikací SQL Server 2005 team](https://blogs.msdn.com/sqlclr/default.aspx) klade důraz tři způsoby, jak ladit objektů systému SQL Server 2005 ze sady Visual Studio:

- **Přímé ladění databáze (DDD)** – z Průzkumníka serveru můžete jsme krok do libovolného objektu databáze T-SQL, jako je například uložené procedury a funkce UDF. Vyzkoušíme DDD v kroku 1.
- **Ladění aplikace** -jsme nastavte zarážky v rámci objektu databáze a poté spusťte naše aplikace ASP.NET. Po provedení databázový objekt zarážce bude dosaženo a řízení předat k ladicího programu. Všimněte si, že s laděním aplikace jsme nelze kroku do objektu databáze z kódu aplikace. Jsme musí explicitně nastavte zarážky v těchto uložené procedury nebo funkce UDF kde chceme, aby ladicí program na Zastavit. Ladění aplikace je zkontrolován spouštění v kroku 2.
- **Ladění z projektu serveru SQL** -edice Visual Studio Professional a systémy Team zahrnují typ projekt SQL Server, který se běžně používá k vytvoření spravovaného databázové objekty. Vyzkoušíme pomocí SQL serveru projekty a ladění jejich obsah v dalším kurzu.

Visual Studio můžete ladit uložené procedury na místní a vzdálené instance systému SQL Server. Místní instance systému SQL Server je ten, který je nainstalován na stejném počítači jako Visual Studio. Pokud není na počítači pro vývoj databázi systému SQL Server, který používáte, považuje vzdálenou instanci. Tyto kurzy jsme již používáte místní instance systému SQL Server. Ladění uložené procedury na vzdálené instance systému SQL server vyžaduje další konfigurační kroky než při ladění uložené procedury na místní instance.

Pokud používáte místní instanci systému SQL Server, můžete začněte krokem 1 a fungovat až do konce tohoto kurzu. Pokud používáte vzdálenou instanci systému SQL Server, ale budete nejprve zajistit, aby při ladění jste přihlášeni na počítači pro vývoj pomocí uživatelského účtu systému Windows, který má přihlášení systému SQL Server ve vzdálené instanci. Moveover, toto přihlášení databáze a přihlášení k databázi používá k připojení k databázi z spuštěná aplikace ASP.NET musí být členy `sysadmin` role. Zobrazí objekty ladění T-SQL databáze v oddílu vzdálené instance na konci tohoto kurzu pro další informace o konfiguraci sady Visual Studio a ladění vzdálené instance systému SQL Server.

Nakonec uvědomit, že ladění podporu pro databázové objekty T-SQL není jako funkce bohaté jako ladění podporu pro aplikace .NET. Například zarážek podmínky a filtry nejsou podporovány, jen podmnožinu ladění windows jsou k dispozici, nemůžete použít upravit a pokračovat, vykreslení hodnot proměnných nemá a podobně. V tématu [omezení příkazů ladicího programu a funkce](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) Další informace.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Krok 1: Přímo Zanoříte se do uložené procedury

Visual Studio snadno přímo ladění objekt databáze. Umožní s, podívejte se na tom, jak použít funkci přímé ladění databáze (DDD) pro krok do `Products_SelectByCategoryID` uloženou proceduru v databázi Northwind. Jak již název napovídá, `Products_SelectByCategoryID` vrátí informace o produktu pro určité kategorie; byl vytvořen [použití existující uložené procedury, pro typové datové sady s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) kurzu. Začněte tím, že přejdete na Průzkumníka serveru a rozbalte uzel databáze Northwind. Dále přejděte do složky, uložené procedury, klikněte pravým tlačítkem na `Products_SelectByCategoryID` uložené procedury a vyberte možnost krok do uloženou proceduru v místní nabídce. Tato akce spustí ladicí program.

Vzhledem k tomu `Products_SelectByCategoryID` uložená procedura očekává, že `@CategoryID` vstupní parametr, jsme vyzváni k zadání tuto hodnotu. Zadejte 1, který vrátí informace o nápoje.


![Použijte hodnotu 1 pro @CategoryID parametr](debugging-stored-procedures-vb/_static/image1.png)

**Obrázek 1**: použijte hodnotu 1 pro `@CategoryID` parametr


Po poskytnutí hodnota `@CategoryID` se spustí parametr uložené procedury. Namísto spuštění dokončen, ale ladicí program zastaví zpracování na první příkaz. Poznámka: žlutá šipka na okraji označující aktuální umístění v uložené proceduře. Můžete zobrazit a upravit hodnoty parametrů pomocí okna kukátka nebo podržením ukazatele nad název parametru v uložené proceduře.


[![Na první příkaz uložená procedura se zastavil ladicí program](debugging-stored-procedures-vb/_static/image3.png)](debugging-stored-procedures-vb/_static/image2.png)

**Obrázek 2**: na první příkaz uložená procedura se zastavil ladicím programu ([Kliknutím zobrazit obrázek v plné velikosti](debugging-stored-procedures-vb/_static/image4.png))


Projděte jeden příkaz uložené procedury najednou, klikněte na tlačítko Krokovat s přeskočením na panelu nástrojů nebo stiskněte klávesu F10. `Products_SelectByCategoryID` Uložené procedury obsahuje jediný `SELECT` příkaz tak, aby stiskne F10 krok přes jediný příkaz a dokončit provádění uložené procedury. Po dokončení uložené procedury, v okně výstupu se zobrazí jeho výstup a ladicí program bude ukončen.

> [!NOTE]
> Ladění T-SQL dochází na úrovni příkaz; nelze krok do `SELECT` příkaz.


## <a name="step-2-configuring-the-website-for-application-debugging"></a>Krok 2: Konfigurace webu pro ladění aplikace

Při ladění uložené procedury přímo z Průzkumníka serveru je užitečný, v mnoha scénářích jsme zajímá více ladění uložené procedury, když je volána z našich aplikace ASP.NET. Můžeme přidat zarážky uložené procedury z Visual Studia a pak spusťte ladění aplikace ASP.NET. Po vyvolání uložená procedura s zarážky z aplikace provádění se zastaví u zarážky a jsme můžete zobrazit a změnit hodnoty parametru uložené procedury s a projděte jeho příkazy, stejně jako jsme provedli v kroku 1.

Můžeme začít ladění uložené procedury volané z aplikace, potřebujeme dáte pokyn, aby webové aplikace ASP.NET pro integraci s ladicím programem systému SQL Server. Začněte tím, že pravým tlačítkem myši na název webu v Průzkumníku řešení (`ASPNET_Data_Tutorial_74_VB`). Zvolte možnost stránky vlastností v místní nabídce vyberte položku Možnosti spuštění na levé straně a zaškrtnutím políčka systému SQL Server v části ladicí programy (viz obrázek 3).


[![Zaškrtnutím políčka SQL serveru na stránkách vlastností s aplikací](debugging-stored-procedures-vb/_static/image6.png)](debugging-stored-procedures-vb/_static/image5.png)

**Obrázek 3**: Zaškrtnutím políčka SQL Server v aplikaci s stránky vlastností ([Kliknutím zobrazit obrázek v plné velikosti](debugging-stored-procedures-vb/_static/image7.png))


Kromě toho je potřeba aktualizovat připojovací řetězec databáze používá aplikace tak, aby sdružování připojení je zakázána. Při připojení k databázi je ukončeno, odpovídající `SqlConnection` objekt je umístěn v rámci fondu dostupné připojení. Při navazování připojení k databázi, objekt dostupné připojení lze načíst z tohoto fondu namísto nutnosti vytvořit a vytvořit nové připojení. Tato sdružování připojení objektů je vylepšení výkonu a ve výchozím nastavení zapnutá. Ale při ladění budeme rádi vypnout sdružování připojení, protože ladění infrastruktury není správně obnovila při práci s připojením, který byl odebrán z fondu.

Chcete-li sdružování připojení zakázáno, aktualizujte `NORTHWNDConnectionString` v `Web.config` tak, že obsahují nastavení `Pooling=false` .


[!code-xml[Main](debugging-stored-procedures-vb/samples/sample1.xml)]

> [!NOTE]
> Po dokončení ladění systému SQL Server prostřednictvím aplikace ASP.NET je nutné obnovit vyřazenou aplikaci sdružování připojení odebráním `Pooling` nastavení z připojovacího řetězce (nebo nastavením na `Pooling=true` ).


V tomto okamžiku má aplikace ASP.NET nakonfigurované umožňuje sadě Visual Studio k ladění objekty databáze systému SQL Server při vyvolání prostřednictvím webové aplikace. Všechny, které zůstává nyní je přidat zarážky uložené procedury a spuštění ladění!

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Krok 3: Přidání zarážku a ladění

Otevřete `Products_SelectByCategoryID` uložené procedury a nastavit zarážky na začátku `SELECT` příkaz kliknutím na okraji na příslušné místo nebo umístěním kurzoru na začátku `SELECT` příkaz a pak stisknete F9. Jak ukazuje obrázek 4, se zobrazí jako červené kolečko u okraje zarážku.


[![Nastavte zarážky v Products_SelectByCategoryID uložené procedury](debugging-stored-procedures-vb/_static/image9.png)](debugging-stored-procedures-vb/_static/image8.png)

**Obrázek 4**: Nastavte zarážky v `Products_SelectByCategoryID` uloženou proceduru ([Kliknutím zobrazit obrázek v plné velikosti](debugging-stored-procedures-vb/_static/image10.png))


Aby objekt databáze SQL chcete ladit prostřednictvím klientskou aplikaci je nutné, aby databáze nakonfigurovat podporu ladění aplikace. Při prvním nastavení boru přerušení, toto nastavení by měl automaticky být zapnutá, ale je doporučeno znovu zkontrolovat. Klikněte pravým tlačítkem na `NORTHWND.MDF` uzlu v Průzkumníku serveru. V místní nabídce by měla obsahovat zaškrtnuté nabídky položka s názvem ladění aplikace.


![Ujistěte se, že je povolena možnost ladění aplikace](debugging-stored-procedures-vb/_static/image11.png)

**Obrázek 5**: Ujistěte se, že je povolena možnost ladění aplikace


Sada zarážek a povolená možnost ladění aplikace jsou připravené k ladění uložené procedury při volání z aplikace ASP.NET. Spuštění ladicího programu přechodem do nabídky ladění a výběr spustit ladění, stále mačkat F5, nebo klepněte na tlačítko se zeleným přehrání ikonu na panelu nástrojů. Tato akce spuštění ladicího programu a spusťte web.

`Products_SelectByCategoryID` Uloženou proceduru byl vytvořen v [použití existující uložené procedury, pro typové datové sady s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) kurzu. Jeho odpovídající webové stránky (`~/AdvancedDAL/ExistingSprocs.aspx`) obsahuje GridView, která zobrazuje výsledky vrácené tuto uloženou proceduru. Navštivte tuto stránku prostřednictvím prohlížeče. Na stránce zarážka v dosažení `Products_SelectByCategoryID` bude dosaženo uložené procedury a ovládací prvek vrátí k sadě Visual Studio. Stejně jako v kroku 1, můžete krok pomocí uložené procedury s příkazy a zobrazit a upravit hodnoty parametru.


[![Na stránce ExistingSprocs.aspx původně zobrazuje nápoje](debugging-stored-procedures-vb/_static/image13.png)](debugging-stored-procedures-vb/_static/image12.png)

**Obrázek 6**: `ExistingSprocs.aspx` stránky jeho otevření zobrazí nápoje ([Kliknutím zobrazit obrázek v plné velikosti](debugging-stored-procedures-vb/_static/image14.png))


[![Bylo dosaženo uloženou proceduru s zarážek](debugging-stored-procedures-vb/_static/image16.png)](debugging-stored-procedures-vb/_static/image15.png)

**Obrázek 7**: uloženou proceduru s bylo dosaženo zarážky ([Kliknutím zobrazit obrázek v plné velikosti](debugging-stored-procedures-vb/_static/image17.png))


Jako okna kukátka v obrázek 7 znázorňuje, hodnota `@CategoryID` parametru je 1. Důvodem je, že `ExistingSprocs.aspx` stránky jeho otevření zobrazí produkty v kategorii Nápoje, který má `CategoryID` hodnotu 1. Zvolte jinou kategorii z rozevíracího seznamu. To způsobí, že zpětné volání a znovu spustí `Products_SelectByCategoryID` uložené procedury. Zarážce je dosáhl znovu, ale tentokrát `@CategoryID` parametr s hodnotou se vztahuje k položce vybrané rozevíracího seznamu s `CategoryID`.


[![Zvolte jinou kategorii z rozevíracího seznamu](debugging-stored-procedures-vb/_static/image19.png)](debugging-stored-procedures-vb/_static/image18.png)

**Obrázek 8**: Zvolte jiný kategorii z rozevíracího seznamu ([Kliknutím zobrazit obrázek v plné velikosti](debugging-stored-procedures-vb/_static/image20.png))


[![@CategoryID Parametr odráží kategorii vybrané z webové stránky](debugging-stored-procedures-vb/_static/image22.png)](debugging-stored-procedures-vb/_static/image21.png)

**Obrázek 9**: `@CategoryID` parametr odráží vybrat kategorii z webové stránky ([Kliknutím zobrazit obrázek v plné velikosti](debugging-stored-procedures-vb/_static/image23.png))


> [!NOTE]
> Pokud zarážka v `Products_SelectByCategoryID` uložená procedura není dosáhl při návštěvě `ExistingSprocs.aspx` se ujistěte, že políčka systému SQL Server se změnami části ladicí programy aplikace ASP.NET s stránku vlastností, že byl sdružování připojení zakázáno, a že je povolena databáze s možností ladění aplikace. Pokud znovu stále dochází k problémům, restartujte Visual Studio a akci opakujte.


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>T-SQL databázové objekty v instancích vzdáleného ladění

Ladění databázových objektů pomocí sady Visual Studio je poměrně jasné, pokud je instance databáze systému SQL Server na stejném počítači jako Visual Studio. Ale pokud SQL Server a Visual Studio jsou umístěny na různých počítačích pak některé pečlivě konfigurace se používá k získání všechno funguje správně. Existují dvě klíčové úlohy, které jsme se potýkají s:

- Ujistěte se, že přihlášení použité pro připojení k databázi pomocí ADO.NET patří k `sysadmin` role.
- Zkontrolujte, zda je uživatelský účet systému Windows, sada Visual Studio na vývojovém počítači platný účet přihlášení serveru SQL Server, který patří do `sysadmin` role.

Prvním krokem je poměrně jednoduché. Nejprve určete uživatelský účet použitý k připojení k databázi z aplikace ASP.NET a pak z SQL Server Management Studio, přidejte daný účet přihlášení k `sysadmin` role.

V druhé úloze vyžaduje, aby uživatelský účet systému Windows, které používáte k ladění aplikace platného přihlášení na vzdálené databáze. Ale pravděpodobné, že účet systému Windows, které přihlášení do pracovní stanice s není platného přihlášení na serveru SQL Server. Místo přidávání účtu konkrétní přihlášení k systému SQL Server, může být vhodnější určit některé uživatelský účet systému Windows jako ladění účet systému SQL Server. K ladění databázové objekty vzdálené instance systému SQL Server, potom by spusťte Visual Studio pomocí pověření účtu s přihlášení tohoto systému Windows.

Příklad by měly pomoci vysvětlení věcí. Představte si, že je účet systému Windows s názvem `SQLDebug` v doméně systému Windows. Tento účet by bylo potřeba přidat do vzdálené instance systému SQL Server jako platné přihlašovací údaje a jako člena `sysadmin` role. Potom k ladění vzdálené instance systému SQL Server ze sady Visual Studio, by potřebujeme spuštění sady Visual Studio, jako `SQLDebug` uživatele. To může provést protokolování z našich pracovní stanice, přihlaste se zpět jako `SQLDebug`, a opětovném spuštění sady Visual Studio, ale jednodušší by mohla být přihlášení k naší pracovní stanice pomocí vlastních přihlašovacích údajů a pak použijte `runas.exe` ke spuštění sady Visual Studio jako `SQLDebug` uživatele. `runas.exe` Umožňuje provést v rámci guise z jiného uživatelského účtu konkrétní aplikace. Spusťte Visual Studio jako `SQLDebug`, můžete zadat následující příkaz na příkazovém řádku:


[!code-console[Main](debugging-stored-procedures-vb/samples/sample2.cmd)]

Podrobnější vysvětlení tohoto postupu najdete v tématu [William R. Vaughn](http://betav.com/BLOG/billva/) s *Hitchhiker s příručka k sadě Visual Studio a SQL Server, sedmého edice* a také [postupy: nastavení oprávnění serveru SQL pro ladění](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Pokud vývojovém počítači se systémem Windows XP Service Pack 2, budete muset nakonfigurovat bránu povolit vzdálené ladění. [Pokynů k: Povolit ladění na SQL Server 2005](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) článku značí, že to zahrnuje dva kroky: (a) na hostitelském počítači Visual Studio, je nutné přidat `Devenv.exe` do seznamu výjimek a otevřete port TCP 135; a (b) ve vzdáleném počítači (SQL), musíte otevřít TCP 135 portu a přidat `sqlservr.exe` do seznamu výjimek. Pokud zásady vaší domény vyžaduje komunikaci sítě s provést prostřednictvím protokolu IPSec, musíte otevřít porty UDP 4500 a UDP 500.


## <a name="summary"></a>Souhrn

Kromě zajištění ladění podpory pro kód aplikace .NET, Visual Studio také poskytuje celou řadu možností pro SQL Server 2005 ladění. V tomto kurzu jsme se podívali na dva z těchto možností: přímé databáze ladění, trasování a ladění aplikací. K ladění přímo databázového objektu T-SQL, najít objekt pomocí Průzkumníka serveru pak klikněte pravým tlačítkem myši na něm a zvolte Krokovat s vnořením. Tím se spustí ladicí program a zastaví na prvním příkazem v databázi objekt, v tomto okamžiku můžete krok prostřednictvím objektu s příkazy a zobrazit a upravit hodnoty parametrů. V kroku 1 jsme použili tento postup pro krok do `Products_SelectByCategoryID` uložené procedury.

Ladění aplikací umožňuje zarážky nastavit přímo v databázové objekty. Po vyvolání objektu databáze se zarážkami z klientské aplikace (například webovou aplikaci ASP.NET) program zastaví jako převezme ladicího programu. Ladění aplikace je užitečné, protože více jasně ukazuje, jaké aplikace akce způsobí, že objekt konkrétní databáze má být volána. To ale vyžaduje trochu další konfigurace a instalace než přímé ladění databáze.

Databázové objekty můžete ladit prostřednictvím projekty SQL Server. Podíváme se na použití SQL serveru projektů a jak je používat k vytváření a ladění spravované objekty databáze v dalším kurzu.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](protecting-connection-strings-and-other-configuration-information-vb.md)
> [další](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb.md)
