---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: Příprava nasazení databáze | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, podle usin...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: a9ddeda3bfe4315c835cd447f6178669797dceb2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803188"
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: Příprava nasazení databáze
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, s použitím sady Visual Studio 2012 nebo Visual Studio 2010. Informace o této sérii, naleznete v tématu [z prvního kurzu této série](introduction.md).


## <a name="overview"></a>Přehled

Tento kurz ukazuje, jak získat projekt připravené na nasazení databáze. Struktura databáze a některých (ne všechny) dat v aplikačním dvě databáze se musí nasadit do testu, přípravném nebo produkčním prostředí.

Obvykle když budete vyvíjet aplikace, zadáte testovací data do databáze, které nemá k nasazení do živého webu. Můžete však také může mít některé provozních dat, kterou chcete nasadit. V tomto kurzu budete konfigurace projektu Contoso University a přípravné skripty, které SQL tak, aby správná data je součástí nasazení.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Ukázková aplikace používá SQL Server Express LocalDB. SQL Server Express je bezplatná edice systému SQL Server. To se obvykle používá při vývoji aplikace je založena na stejný databázový stroj jako plné verze SQL serveru. Můžete otestovat pomocí SQL Server Express a být jistí, že se aplikace chová stejně v produkčním prostředí s několika výjimkami, pro funkce, které se liší edice SQL serveru.

LocalDB je ve speciálním režimu provádění SQL Server Express, která umožňuje pracovat s databází jako *.mdf* soubory. Obvykle jsou uloženy soubory databáze LocalDB *aplikace\_Data* složce webového projektu. Funkce instance uživatele v SQL serveru Express také umožňuje pracovat s *.mdf* soubory, ale uživatelské instance funkce je zastaralá možnost; proto se doporučuje LocalDB pro práci s *.mdf* soubory.

Obvykle SQL Server Express se nepoužívá pro produkční webové aplikace. LocalDB zejména se nedoporučuje pro produkční použití s webovou aplikací vzhledem k tomu, že není určená pro práci se službou IIS.

V sadě Visual Studio 2012 je nainstalována LocalDB ve výchozím nastavení se sadou Visual Studio. V sadě Visual Studio 2010 a starší SQL Server Express (bez LocalDB) je nainstalovaná ve výchozím nastavení se sadou Visual Studio; To znamená proč jste ho nainstalovali jako jeden z požadovaných součástí v [prvního kurzu této série](introduction.md).

Další informace o edicích systému SQL Server, včetně LocalDB, naleznete na následujících odkazech [práci s databází SQL serveru](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework a Universal Providers

Pro přístup k databázi aplikace Contoso University vyžaduje následující software, který musí být nasazeny s aplikací, protože není zahrnutý v rozhraní .NET Framework:

- [Balíček ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (umožňuje systém členství technologie ASP.NET použití Azure SQL Database)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Protože tento software je zahrnuta v balíčcích NuGet, projekt již nastaven tak, aby se požadované sestavení v projektu nasazený. (Odkazy bodu v aktuální verzi tyto balíčky, které by mohly být novější než jaká je nainstalována v počáteční projekt, který jste stáhli pro účely tohoto kurzu).

Pokud provádíte nasazení k poskytovateli hostingu třetí strany místo Azure, ujistěte se, že používáte Entity Framework 5.0 nebo novější. Starší verze systému migrace Code First vyžadují úplný vztah důvěryhodnosti a většina poskytovatelů hostingu se spustí vaše aplikace na úrovni Medium Trust. Další informace o úrovni Medium Trust, najdete v článku [nasazení do služby IIS jako testovacího prostředí](deploying-to-iis.md) kurzu.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Konfigurace migrace Code First pro nasazení aplikace databáze

Databáze aplikace Contoso University spravuje Code First a nasadíte ji pomocí migrace Code First. Přehled nasazení databáze pomocí migrace Code First najdete v tématu [prvního kurzu této série](introduction.md).

Při nasazení databáze aplikace obvykle není nasadíte jednoduše databázi vývoj se všechna data v něm do produkčního prostředí, protože velká část dat v něm je pravděpodobně existuje pouze pro účely testování. Jména studentů v testovací databázi jsou například fiktivní. Na druhé straně často se nedají nasadit jenom struktura databáze bez dat v něm vůbec. Některá data v databázi testu může být reálná data a musí se uživatelé začít používat aplikace. Vaše databáze může mít například tabulku, která obsahuje hodnoty platná třída nebo názvy skutečných oddělení.

Pro simulaci tomto běžném scénáři, které nakonfigurujete migrace Code First `Seed` metodu, která se vloží do databáze pouze data, která má být k dispozici v produkčním prostředí. To `Seed` metoda by neměla testovací data vložit, protože poběží v produkčním prostředí po Code First vytvoří databázi v produkčním prostředí.

V dřívějších verzích Code First před migrací, bylo běžné `Seed` metody pro vložení testovacích dat, protože při každé změně modelu během vývoje databáze byl zcela odstranit a znovu vytvářet úplně od začátku. Pomocí migrace Code First, testovací data se uchovávají po provedení změn databáze, takže včetně testovací data v `Seed` metoda není nutné. Projekt, který jste si stáhli používá metodu všechna data, včetně `Seed` metoda inicializátor třídy. V tomto kurzu budete zakázat inicializátor třídy a `enable Migrations. Then you'll update the `počáteční hodnoty ' metody v konfiguraci migrace třídu tak, aby pouze data, která má být vložen do produkčního prostředí.

Následující diagram znázorňuje schéma databáze aplikace:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Pro tyto kurzy vám předpokládá, že `Student` a `Enrollment` tabulky by měla být prázdná, při prvním nasazení webu. Ostatní tabulky obsahují data, která musí být předem načtena, když dostane živé aplikace.

### <a name="disable-the-initializer"></a>Zakázat inicializátor

Protože se pomocí migrace Code First, není potřeba použít `DropCreateDatabaseIfModelChanges` Code First inicializátor. Kód pro tento inicializátor je v *SchoolInitializer.cs* soubor v projektu ContosoUniversity.DAL. Nastavení v `appSettings` elementu *Web.config* soubor způsobí, že tento inicializátor pokaždé, když se aplikace snaží o přístup k databázi pro první spuštění:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Otevřete aplikaci *Web.config* souboru a odstranit nebo okomentovat `add` prvek, který určuje třídu Code First inicializátor. `appSettings` Prvek nyní vypadá takto:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Jiný způsob, jak určit třídu inicializátoru je provést zavoláním `Database.SetInitializer` v `Application_Start` metodu *Global.asax* souboru. Pokud povolíte migrace v projektu, který tato metoda používá k určení inicializátoru, odeberte tento řádek kódu.


> [!NOTE]
> Pokud používáte Visual Studio 2013, přidat následující kroky mezi kroky 2 a 3: (a) v konzole PMC zadejte "entityframework update-package-verze 6.1.1" získání aktuální verze EF. A (b) sestavte projekt, aby získat seznam chyb sestavení a opravte je. Odstranit pomocí příkazů pro obory názvů, které už existují, klikněte pravým tlačítkem a kliknutím na tlačítko Přidat direktivu using příkazy, kde jsou potřeba vyřešit a změnit výskyty System.Data.EntityState System.Data.Entity.EntityState.


### <a name="enable-code-first-migrations"></a>Povolení migrace Code First

1. Ujistěte se, že je projekt ContosoUniversity (ne ContosoUniversity.DAL) nastavit jako spouštěný projekt. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a vyberte **nastavit jako spouštěný projekt**. Migrace Code First bude hledat v projektu po spuštění a najděte připojovací řetězec databáze.
2. Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven** (nebo **Správce balíčků NuGet**) a potom **Konzola správce balíčků**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. V horní části **Konzola správce balíčků** okno vybrat jako výchozí projekt a potom at ContosoUniversity.DAL `PM>` řádku zadejte "enable migrace".

    ![příkaz povolení migrace](preparing-databases/_static/image4.png)

    (Pokud dojde k chybě výrokem *povolení migrace* podpříkaz rozpoznán, zadejte příkaz *EntityFramework update-package-přeinstalujte* a zkuste to znovu.)

    Tento příkaz vytvoří *migrace* složky v projektu ContosoUniversity.DAL a umístí do této složky dva soubory: *Configuration.cs* soubor, který můžete použít ke konfiguraci migrace a *InitialCreate.cs* soubor pro první migrace, který vytvoří databázi.

    ![Migrace složky](preparing-databases/_static/image5.png)

    Vybraný projekt DAL v **výchozí projekt** rozevírací seznam **Konzola správce balíčků** protože `enable-migrations` příkazu musí být spuštěn v projektu, který obsahuje Code First Context – třída. Když tuto třídu v projektu knihovny tříd, migrace Code First hledá připojovací řetězec databáze v projektu po spuštění pro řešení. V řešení ContosoUniversity webový projekt byl nastaven jako spouštěný projekt. Pokud nechcete určit projekt, který obsahuje připojovací řetězec jako spouštěný projekt v sadě Visual Studio, můžete zadat projekt po spuštění příkazu prostředí PowerShell. Chcete-li zobrazit syntaxi příkazu, zadejte příkaz `get-help enable-migrations`.

    `enable-migrations` Příkaz automaticky vytvořit první migrace, protože databáze již existuje. Alternativou je mít migrace databázi vytvořit. K tomuto účelu použijte **Průzkumníka serveru** nebo **Průzkumník objektů systému SQL Server** odstranit databázi ContosoUniversity a teprve potom povolte migrace. Po povolení migrace ručně vytvořte první migraci tak, že zadáte příkaz "Přidat migrace InitialCreate". Potom můžete vytvořit databázi zadáním příkazu "update databáze".

### <a name="set-up-the-seed-method"></a>Nastavit Seed – metoda

Pro účely tohoto kurzu přidáte pevných datových přidáním kódu `Seed` metoda migrace Code First `Configuration` třídy. Kód volá první migrace `Seed` za každou migraci.

Vzhledem k tomu, `Seed` metoda se spouští po každé migrace, datového již existuje v tabulkách po první migraci. Tuto situaci použijete `AddOrUpdate` způsob aktualizace řádků, které již byl vložen, nebo je vložit, pokud ještě neexistují. `AddOrUpdate` Metody nemusí být nejlepší volbou pro váš scénář. Další informace najdete v tématu [postará metodou AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.

1. Otevřít *Configuration.cs* souboru a nahraďte komentáře v `Seed` metodu s následujícím kódem:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Odkazy na `List` mít červenou vlnovkou řádky pod nimi, protože není nutné `using` příkaz ještě pro svůj obor názvů. Klikněte pravým tlačítkem na jednu z instancí `List` a klikněte na tlačítko **vyřešit**a potom klikněte na tlačítko **pomocí System.Collections.Generic**.

    ![Řešení s využitím – příkaz](preparing-databases/_static/image6.png)

    Tento výběr nabídky přidá následující kód, který `using` příkazů v horní části souboru.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Stisknutím klávesy CTRL-SHIFT-B a sestavte projekt.

Projekt je nyní připraven k nasazení *ContosoUniversity* databáze. Po nasazení aplikace, při prvním spuštění a přejděte na stránku, která přistupuje k databázi, Code First vytvoří databázi a spustit `Seed` metody.

> [!NOTE]
> Přidání kódu `Seed` metoda je jednou z mnoha způsoby, pevných datových můžete vložit do databáze. Alternativou je přidejte kód, který `Up` a `Down` metody třídy každou migraci. `Up` a `Down` metody obsahovat kód, který implementuje změny databáze. Zobrazí se vám příklady v [nasazení aktualizace databáze](deploying-a-database-update.md) kurzu.
> 
> Můžete také napsat kód, který se spustí s použitím příkazů jazyka SQL `Sql` metody. Například, pokud bylo přidání rozpočtu sloupce do tabulky oddělení a chcete inicializovat všechna rozpočty na $ 1 000,00 jako součást migrace, můžete přidat folllowing řádku kódu `Up` metoda této migrace:
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Vytváření skriptů pro nasazení databáze členství

Aplikace Contoso University používá ověřování formuláře a systém členství technologie ASP.NET k ověřování a autorizaci uživatelů. **Aktualizace kredity** stránka je přístupný jenom uživatelům, kteří jsou v roli správce.

Spusťte aplikaci a klikněte na tlačítko **kurzy**a potom klikněte na tlačítko **aktualizace kredity**.

![Klikněte na tlačítko aktualizace kredity](preparing-databases/_static/image7.png)

**Přihlášení** stránka se zobrazí, protože **aktualizace kredity** stránka vyžaduje oprávnění správce.

Zadejte *správce* jako uživatelské jméno a *devpwd* jako heslo a klikněte na **přihlášení**.

![S přihlašovací stránkou](preparing-databases/_static/image8.png)

**Aktualizace kredity** se zobrazí stránka.

![Stránka pro aktualizaci kredity](preparing-databases/_static/image9.png)

Informace o uživateli a role je v *aspnet ContosoUniversity* databázi, která je určená **objekt DefaultConnection** připojovací řetězec *Web.config* souboru.

Tato databáze nespravuje Entity Framework Code First, takže migrace nelze použít k jejímu nasazení. Budete používat poskytovatele dbDacFx se nasadit schéma databáze a budete konfigurovat profil publikování pro spuštění skriptu, který se vloží počáteční data do tabulky databáze.

> [!NOTE]
> Nový systém členství technologie ASP.NET (nyní s názvem ASP.NET Identity) byla zavedena v systému Visual Studio 2013. Nový systém umožňuje zachovat aplikaci a tabulky členství ve stejné databázi a můžete použít migrace Code First pro nasazení obou. Ukázková aplikace používá starší systém členství technologie ASP.NET, které nelze nasadit pomocí migrace Code First. Postupy pro nasazení této databáze členství platí také pro všechny další scénáře, ve kterém se vaše aplikace potřebuje k nasazení databáze SQL serveru, který není vytvořený pomocí platformy Entity Framework Code First.


Zde příliš, obvykle nechcete stejná data v produkčním prostředí, které máte ve vývoji. Při první nasadíte lokalitu, je běžné vyloučit většinu nebo všechny uživatelské účty, které vytvoříte pro testování. Proto staženého projektu má dvě databáze členství: *aspnet ContosoUniversity.mdf* s uživateli vývoje a *aspnet. ContosoUniversity Prod.mdf* s produkční uživatele. Pro účely tohoto kurzu uživatelská jména jsou stejné v obou databázích: *správce* a *text nonadmin*. Oba uživatelé mají heslo *devpwd* databáze vývoje a *prodpwd* v provozní databázi.

Nasadíte uživatelům vývoje pro testovací prostředí a produkční uživatele do pracovního a produkčního prostředí. K tomu vytvoříte dva skripty SQL v tomto kurzu, jeden pro vývoj a jeden pro produkční prostředí a v budoucích kurzech nakonfigurujete procesu publikování pro jejich spuštění.

> [!NOTE]
> Databáze členství ukládá hodnota hash hesla účtu. Za účelem nasazení účty z jednoho počítače do jiného, ujistěte se, že hash rutiny negenerovat různé hodnoty hash na cílovém serveru, než na zdrojovém počítači. Nich vydá stejné hodnoty hash při použití technologie ASP.NET Universal Providers, dokud nezměníte výchozí algoritmus. Výchozí algoritmus je HMACSHA256 a je uveden v **ověření** atribut **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** element v souboru Web.config.


Skripty nasazení dat. můžete vytvořit ručně, pomocí SQL Server Management Studio (SSMS), nebo pomocí nástroje třetích stran. Tato zbývající část tohoto kurzu ukazují, jak to udělat v aplikaci SSMS, ale pokud nechcete nainstalovat a používat SSMS můžete získat skripty z úplnou verzi projektu a přeskočit k části, kde jsou uloženy ve složce řešení.

Pokud chcete nainstalovat aplikaci SSMS, nainstalujte ji z [Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) kliknutím [ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) nebo [ ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Pokud se rozhodnete je nesprávný pro váš systém nebude schopen provést instalaci a zkuste druhou.

(Všimněte si, že se jedná o stažení 600 MB. Může trvat dlouhou dobu instalace a bude vyžadovat restartování počítače.)

Na první stránce Centrum instalace SQL serveru, klikněte na tlačítko **samostatná instalace nového serveru SQL Server nebo přidání funkcí do existující instalace**a postupujte podle pokynů, přijímat výchozí volby.

### <a name="create-the-development-database-script"></a>Vytvořit skript databáze vývoje

1. Spusťte aplikaci SSMS.
2. V **připojit k serveru** dialogového okna zadejte *(localdb) \v11.0* jako **název serveru**, ponechte **ověřování** nastavena na **Ověřování Windows**a potom klikněte na tlačítko **připojit**.

    ![SSMS připojit k serveru](preparing-databases/_static/image10.png)
3. V **Průzkumník objektů** okna, rozbalte **databází**, klikněte pravým tlačítkem na **aspnet ContosoUniversity**, klikněte na tlačítko **úlohy**a potom klikněte na **Generování skriptů**.

    ![SSMS generování skriptů](preparing-databases/_static/image11.png)
4. V **generovat a publikovat skripty** dialogové okno, klikněte na tlačítko **nastavit možnosti skriptování**.

    Můžete přeskočit **zvolte objekty** krok, protože výchozí hodnota je **celou databázi a všechny objekty databáze skriptu** a to je, co chcete.
5. Klikněte na tlačítko **Advanced**.

    ![Možnosti skriptování aplikace SSMS](preparing-databases/_static/image12.png)
6. V **rozšířené možnosti skriptování** dialogové okno, přejděte dolů k **typy shromažďovaných údajů skript**a klikněte na tlačítko **jenom Data** možnost v rozevíracím seznamu.
7. Změna **skript použít databázi** k **False**. POUŽIJTE příkazy nejsou platné pro službu Azure SQL Database a nejsou potřebné pro nasazení systému SQL Server Express v testovacím prostředí.

    ![SSMS Data pouze pro skript, bez použití příkazu](preparing-databases/_static/image13.png)
8. Klikněte na tlačítko **OK**.
9. V **generovat a publikovat skripty** dialogovém okně **název_souboru** pole určuje, kde se vytvoří skript. Změňte cestu na složku řešení (složka, která obsahuje váš soubor ContosoUniversity.sln) a název souboru *aspnet-data-dev.sql*.
10. Klikněte na tlačítko **Další** přejdete **Souhrn** kartu a potom klikněte na tlačítko **Další** znovu a vytvořit skript.

    ![Vytvoření skriptu aplikace SSMS](preparing-databases/_static/image14.png)
11. Klikněte na tlačítko **Dokončit**.

### <a name="create-the-production-database-script"></a>Vytvoření produkční databáze skriptu

Protože nebyly spusťte projekt s produkční databází, není dosud připojen k instanci LocalDB. Proto budete muset nejprve připojte databázi.

1. V SSMS **Průzkumník objektů**, klikněte pravým tlačítkem na **databází** a klikněte na tlačítko **připojit**.

    ![Připojení aplikace SSMS](preparing-databases/_static/image15.png)
2. V **připojit databáze** dialogové okno, klikněte na tlačítko **přidat** a přejděte na *aspnet. ContosoUniversity Prod.mdf* soubor *aplikace\_ Data* složky.

     ![Přidat aplikace SSMS souboru .mdf připojení](preparing-databases/_static/image16.png)
3. Klikněte na tlačítko **OK**.
4. Postupujte stejným způsobem, který jste předtím použili ke vytvořit skript pro soubor produkčního prostředí. Název souboru skriptu *aspnet-data-prod.sql*.

## <a name="summary"></a>Souhrn

Obě databáze jsou teď připravená k nasazení a budete mít dva skripty nasazení dat. ve složce řešení.

![Skripty nasazení dat.](preparing-databases/_static/image17.png)

V následujícím kurzu nakonfigurujete nastavení projektu, které ovlivní také nasazení, a nastavit automatické *Web.config* souboru transformace pro nastavení, která se musí lišit v nasazené aplikaci.

## <a name="more-information"></a>Další informace

Další informace o NuGet naleznete v tématu [spravovat projektu knihovny se zmírněními hrozeb NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) a [dokumentace pro NuGet](http://docs.nuget.org/docs/start-here/overview). Pokud už nechcete používat NuGet, budete muset zjistěte, jak analyzovat určit, co to dělá při instalaci balíčku NuGet. (Například můžou nakonfigurovat *Web.config* například transformace konfigurace skriptů Powershellu, aby se spouštěla v okamžiku sestavení atd.) Další informace o tom, jak NuGet funguje, najdete v článku [vytváření a publikování balíčku](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) a [konfigurační soubor a zdrojový kód transformace](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Předchozí](introduction.md)
> [další](web-config-transformations.md)
