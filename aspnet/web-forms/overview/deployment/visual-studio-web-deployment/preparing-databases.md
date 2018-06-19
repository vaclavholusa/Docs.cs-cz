---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: Příprava pro nasazení databáze | Microsoft Docs'
author: tdykstra
description: Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo do hostujícího zprostředkovatele třetí strany podle usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: 61392af322de454687da522055005a670b34f510
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889144"
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: Příprava pro nasazení databáze
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, pomocí sady Visual Studio 2012 nebo Visual Studio 2010. Informace o řadě najdete v tématu [z prvního kurzu řady](introduction.md).


## <a name="overview"></a>Přehled

Tento kurz ukazuje, jak získat projektu připravený pro nasazení databáze. Struktura databáze a některé (ne všechny) dat v aplikačním dvě databáze se musí nasadit do testovacího, pracovní a provozní prostředí.

Obvykle když budete vyvíjet aplikace, zadáte testovací data do databáze, které nechcete použít k nasazení na živý web. Můžete však také může mít některé provozními daty, které chcete nasadit. V tomto kurzu budete konfigurace projektu univerzity Contoso a připravit skripty SQL tak, aby správná data je součástí nasazení.

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Ukázková aplikace používá SQL Server Express LocalDB. SQL Server Express je bezplatná edice systému SQL Server. Běžně používá se při vývoji aplikace je založena na stejném databázového stroje jako plné verze systému SQL Server. Můžete otestovat s SQL Server Express a mít jistotu, že aplikace budou chovat stejné v produkčním prostředí s několika výjimkami pro funkce, které se liší mezi edice serveru SQL.

LocalDB je režim speciální spuštění systému SQL Server Express, který umožňuje pracovat s databázemi jako *.mdf* soubory. Obvykle jsou zachovány soubory databáze LocalDB v *aplikace\_Data* složky webového projektu. Instance funkce uživatel v systému SQL Server Express také umožňuje pracovat s *.mdf* soubory, ale funkce instance uživatel je zastaralý, tedy LocalDB doporučuje se pro práci s *.mdf* soubory.

Obvykle SQL Server Express se nepoužije pro provozní webové aplikace. LocalDB zejména se nedoporučuje pro produkční použití s webovou aplikací protože není určeno k práci se službou IIS.

V sadě Visual Studio 2012 je LocalDB nainstalována ve výchozím nastavení pomocí sady Visual Studio. V sadě Visual Studio 2010 a dřívějších verzích SQL Server Express (bez LocalDB) je nainstalována ve výchozím nastavení pomocí sady Visual Studio; To znamená proč jste ho nainstalovali jako jeden z požadovaných součástí v [z prvního kurzu této série](introduction.md).

Další informace o edicích systému SQL Server, včetně LocalDB, naleznete v následujících zdrojích informací [práce s databází systému SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Rozhraní Entity Framework a Universal Providers

Pro přístup k databázi aplikace Contoso univerzity vyžaduje následující software, který musí být nasazený s aplikací, protože není zahrnutý v rozhraní .NET Framework:

- [Balíček ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (umožňuje systém členství technologie ASP.NET používat Azure SQL Database)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Vzhledem k tomu, že tento software je součástí balíčků NuGet, projekt je již nastavena tak, aby se v projektu nasazený požadovaná sestavení. (Odkazy přejděte na aktuální verze těchto balíčků, které může být novější než co je nainstalované v projektu starter, který jste stáhli pro účely tohoto kurzu).

Pokud provádíte nasazení do hostujícího zprostředkovatele třetí strany místo Azure, ujistěte se, že používáte rozhraní Entity Framework 5.0 nebo novější. Starší verze migrace Code First vyžadují úplný vztah důvěryhodnosti a většina poskytovatelé hostitelských služeb se spustí aplikace na úrovni Medium Trust. Další informace o úrovni Medium Trust, najdete v článku [nasadit do IIS jako testovacím prostředí](deploying-to-iis.md) kurzu.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Konfigurace migrace Code First pro nasazení databáze aplikace

Databáze aplikace Contoso univerzity spravuje Code First a nasadíte ji pomocí migrace Code First. Přehled nasazení databáze pomocí migrace Code First najdete v tématu [z prvního kurzu této série](introduction.md).

Když nasadíte databázi aplikace, obvykle nenasazujete jednoduše vývojové databáze se všechna data v ní do produkčního prostředí, protože pouze data v něm je pravděpodobně existuje pouze pro účely testování. Například jsou fiktivních student názvy v testovací databáze. Na druhé straně často nemůžete nasadit právě struktura databáze bez dat v něm vůbec. Některá data v databázi testu může být reálná data a musí být existuje, když uživatelé před zahájením používání aplikace. Například databáze může mít tabulku, která obsahuje hodnoty platný základní nebo názvy skutečné oddělení.

Pro simulaci tomto běžném scénáři, budete konfigurovat migrace Code First `Seed` metoda, která vloží do databáze pouze data, která chcete existovat v produkčním prostředí. To `Seed` metoda nesmí vložit testovací data, protože se spustí v produkčním prostředí poté, co Code First vytvoří databázi v produkčním prostředí.

V dřívějších verzích Code First před vydáním migrace, bylo běžné `Seed` metody vložit testovacích dat taky, protože při každé změně modelu během vývoje neprobíhala v databázi úplně odstranit a znovu vytvořit od začátku. Pomocí migrace Code First, testovací data se uchovávají po provedení změn databáze, takže včetně testovací data v `Seed` metoda není nutné. Používá metodu včetně všech dat v projektu, který jste stáhli `Seed` metoda inicializátoru třídy. V tomto kurzu budete zakážete inicializátoru třídy a `enable Migrations. Then you'll update the `počáteční hodnoty ' metoda v konfiguraci migrace třídy tak, aby pouze data, která má být vložen v produkčním prostředí.

Následující diagram znázorňuje schéma databáze aplikace:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Tyto kurzy budete předpokládá, že `Student` a `Enrollment` tabulky by měla být prázdná, při prvním nasazení webu. Jiné tabulky obsahují data, která musí být předem načtena, kdy aplikace přestane za provozu.

### <a name="disable-the-initializer"></a>Zakázat inicializátoru

Vzhledem k tomu, že budete používat migrace Code First, není nutné použít `DropCreateDatabaseIfModelChanges` inicializátoru Code First. Kód pro tento inicializátoru je v *SchoolInitializer.cs* v ContosoUniversity.DAL projektu. Nastavení `appSettings` element *Web.config* souboru způsobí, že tento inicializátoru ke spuštění, kdykoli se aplikace pokusí o přístup k databázi první:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Otevřete aplikaci *Web.config* souboru a odeberte nebo komentář `add` element, který určuje třídu inicializátoru Code First. `appSettings` Element nyní vypadat třeba takto:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Je také možné zadat třídu inicializátoru provést voláním `Database.SetInitializer` v `Application_Start` metoda v *Global.asax* souboru. Pokud chcete povolit migrace v projektu, který tato metoda určuje inicializátoru, odeberte tohoto řádku kódu.


> [!NOTE]
> Pokud používáte Visual Studio 2013, přidat následující kroky mezi kroky 2 a 3: Zadejte (a) pomocí PMC v "balíček aktualizace entityframework-verze 6.1.1" získat aktuální verzi EF. A (b) sestavení projektu získat seznam sestavení chyb a opravte je. Odstraňte pomocí příkazů pro obory názvů, které již existují, klikněte pravým tlačítkem myši a kliknutím na tlačítko vyřešit přidat pomocí příkazů, kde jste třeba a změňte výskyty System.Data.EntityState System.Data.Entity.EntityState.


### <a name="enable-code-first-migrations"></a>Povolení migrace Code First

1. Ujistěte se, že je ContosoUniversity projektu (ne ContosoUniversity.DAL) nastavit jako spouštěný projekt. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a vyberte **nastavit jako spouštěný projekt**. Migrace Code First Hledat v spouštěný projekt najít připojovací řetězec databáze.
2. Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven** (nebo **Správce balíčků NuGet**) a potom **Konzola správce balíčků**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. V horní části **Konzola správce balíčků** okno Vyberte ContosoUniversity.DAL jako výchozí projekt a pak at `PM>` řádku zadejte "enable-migrations".

    ![příkaz enable-migrations se](preparing-databases/_static/image4.png)

    (Pokud dojde k chybě o tom, že *enable-migrations se* příkaz nebyl rozpoznán, zadejte příkaz *balíček aktualizace EntityFramework-přeinstalujte* a zkuste to znovu.)

    Tento příkaz vytvoří *migrace* vloží složky v projektu ContosoUniversity.DAL ale v této složce dva soubory: *Configuration.cs* soubor, který můžete použít ke konfiguraci migrace a *InitialCreate.cs* soubor pro první migrace, která vytváří databázi.

    ![Složku migrations](preparing-databases/_static/image5.png)

    Jste vybrali projekt DAL v **výchozí projekt** rozevírací seznam **Konzola správce balíčků** protože `enable-migrations` příkaz musí spustit v projektu, který obsahuje Code First Třída kontextu. Po této třídy v projektu knihovny tříd se migrace Code First Vyhledá připojovací řetězec databáze v počáteční projekt pro řešení. V řešení ContosoUniversity byl nastaven webový projekt jako spouštěný projekt. Pokud nechcete, aby k určení projekt, který má připojovací řetězec jako spouštěný projekt v sadě Visual Studio, můžete zadat projekt po spuštění příkazu prostředí PowerShell. Pokud chcete zobrazit syntaxi příkazu, zadejte příkaz `get-help enable-migrations`.

    `enable-migrations` Příkaz automaticky vytvoří první migrace, protože databáze již existuje. Alternativou je tak, aby měl migrace vytvořit databázi. Chcete-li provést, použijte **Průzkumníka serveru** nebo **Průzkumník objektů systému SQL Server** odstranit databázi ContosoUniversity dříve než povolíte migrace. Jakmile povolíte migrace, vytvořte první migrace ručně zadáním příkazu "Přidat migrace InitialCreate". Potom můžete vytvořit databázi zadáním příkazu "update-database".

### <a name="set-up-the-seed-method"></a>Nastavit Seed – metoda

Pro účely tohoto kurzu přidáte pevných datových tak, že přidáte kód, který `Seed` metoda migrace Code First `Configuration` třídy. Migrace First volání kódu `Seed` metoda po každé migraci.

Vzhledem k tomu `Seed` metoda se spouští po každé migraci, již existuje data v tabulkách po první migraci. Ke zpracování této situace, které budete používat `AddOrUpdate` metoda aktualizace řádků, které již byl vložen a vložte je, pokud dosud neexistují. `AddOrUpdate` Metoda nemusí být nejlepším řešením pro váš scénář. Další informace najdete v tématu [postará s EF 4.3 AddOrUpdate metoda](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.

1. Otevřete *Configuration.cs* souboru a nahraďte komentáře ve `Seed` metoda následujícím kódem:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Odkazy na `List` mít červenými vlnovkami nich, protože nemáte `using` příkaz ještě pro její obor názvů. Pravým tlačítkem na jednu instanci `List` a klikněte na tlačítko **vyřešit**a potom klikněte na **pomocí System.Collections.Generic**.

    ![Řešení s použitím – příkaz](preparing-databases/_static/image6.png)

    Tento výběr nabídky přidá následující kód, který `using` příkazy v horní části souboru.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Stisknutím kombinace kláves CTRL-SHIFT-B a tím projekt sestavit.

Projekt je nyní připraven k nasazení *ContosoUniversity* databáze. Poté, co nasadíte aplikaci, spusťte ji a přejděte na stránku, který přistupuje k databázi, poprvé Code First akce vytvoření databáze a spustí to `Seed` metoda.

> [!NOTE]
> Přidání kódu k `Seed` metoda je jednou z mnoha způsoby, pevných datových můžete vložit do databáze. Alternativou je přidejte kód, který `Up` a `Down` metody jednotlivé třídy migrace. `Up` a `Down` metody obsahovat kód, který implementuje změny v databázi. Uvidíte je příkladem [nasazení aktualizace databáze](deploying-a-database-update.md) kurzu.
> 
> Můžete také psát kód, který spustí příkazy SQL pomocí `Sql` metoda. Například pokud byly přidat nároky sloupec v tabulce oddělení a chtěli k chybě při inicializaci všechny rozpočty na 1 000,00 Kč jako součást migrace, můžete přidat folllowing řádku kódu `Up` metoda, pro které migrace:
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Vytváření skriptů pro nasazení databáze členství

Aplikace Contoso univerzity používá ověřování systému a forms členství technologie ASP.NET pro ověřování a autorizaci uživatelů. **Aktualizace kredity** stránka je přístupné pouze pro uživatele, kteří jsou v roli správce.

Spusťte aplikaci a klikněte na tlačítko **kurzy**a potom klikněte na **aktualizace kredity**.

![Klikněte na tlačítko kredity aktualizace](preparing-databases/_static/image7.png)

**Přihlásit** stránka se zobrazí, protože **aktualizace kredity** stránka vyžaduje oprávnění správce.

Zadejte *správce* jako uživatelské jméno a *devpwd* heslo a klikněte na **přihlásit**.

![Přihlaste se na stránce](preparing-databases/_static/image8.png)

**Aktualizace kredity** se zobrazí stránka.

![Stránka pro aktualizaci kredity](preparing-databases/_static/image9.png)

Informace o uživateli a role je ve *aspnet ContosoUniversity* databáze, která je zadána **objekt DefaultConnection** připojovací řetězec v *Web.config* souboru.

Tato databáze není spravován nástrojem Entity Framework Code First, takže migrace nelze použít k nasazení. Poskytovatele dbDacFx budete používat k nasazení schématu databáze, a budete konfigurovat profil publikování pro spuštění skriptu, která budou vkládat počátečního data do databázové tabulky.

> [!NOTE]
> Nový systém členství technologie ASP.NET (teď s názvem ASP.NET Identity) byla zavedena v systému Visual Studio 2013. Nový systém umožňuje udržovat aplikaci a tabulky členství ve stejné databázi a používáte migrace Code First pro nasazení obou. Ukázková aplikace používá starší systém členství technologie ASP.NET, který nelze nasadit pomocí migrace Code First. Postupy pro nasazení tato databáze členství platí také pro všechny další scénáře, ve které aplikace potřebuje k nasazení databáze serveru SQL Server, který není vytvořen Entity Framework Code First.


Zde příliš, obvykle nechcete, aby stejná data v produkčním prostředí, které máte v vývoj. Při prvním nasazení lokality, je běžné vyloučit většinu nebo všechny uživatelské účty, které vytvoříte pro testování. Proto staženého projektu má dvě databáze členství: *aspnet ContosoUniversity.mdf* s uživatelé development a *aspnet. ContosoUniversity Prod.mdf* s provozním uživateli. V tomto kurzu uživatelská jména jsou stejné ve obou databází: *správce* a *nonadmin*. Jak uživatelé mají heslo *devpwd* v databázi vývoj a *prodpwd* v produkční databázi.

Uživatelé development budete nasazovat do testovacího prostředí a produkční uživatelům pracovní a provozní. K tomu v tomto kurzu, jednu pro vývoj a jeden pro produkční prostředí vytvoříte dva skripty SQL a v dalších kurzech budete konfigurovat proces publikování, abyste je mohli spustit.

> [!NOTE]
> Databáze členství ukládá hodnotu hash hesla účtů. Abyste mohli nasadit účty z jednoho počítače do druhého, ujistěte se, že hash rutiny negenerovat různé hodnoty hash na cílovém serveru, než na zdrojovém počítači. Stejné hodnoty hash se generují při použití balíčku ASP.NET Universal Providers tak dlouho, dokud Neměnit výchozí algoritmus. Výchozí algoritmus HMACSHA256 a je určena v **ověření** atribut **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** element v souboru Web.config.


Skripty nasazení dat můžete vytvořit ručně pomocí SQL Server Management Studio (SSMS) nebo pomocí nástroje třetích stran. Tento zbývající část tohoto kurzu vám ukáže, jak to udělat v aplikaci SSMS, ale pokud nechcete, aby instalaci a použití aplikace SSMS můžete získat skripty z dokončené verze projektu a přejít do části, kde jsou uloženy ve složce řešení.

Pokud chcete nainstalovat aplikace SSMS, nainstalujte ji z [stažení softwaru společnosti Microsoft: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) kliknutím [ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) nebo [ ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Pokud si zvolíte nesprávný pro váš systém se nepodaří nainstalovat, a můžete zkusit jiný.

(Všimněte si, že se jedná o stažení 600 MB. To může trvat dlouhou dobu na instalaci a bude vyžadovat restartování počítače.)

Na první stránce Centrum instalace SQL serveru, klikněte na tlačítko **samostatná instalace nový Server SQL nebo přidání funkcí do existující instalace**a postupujte podle pokynů, přijímá výchozí volby.

### <a name="create-the-development-database-script"></a>Vytvoření databázového skriptu vývoj

1. Spuštění aplikace SSMS.
2. V **připojit k serveru** dialogovém okně zadejte *(localdb) \v11.0* jako **název serveru**, nechte **ověřování** nastavena na **Ověřování systému Windows**a potom klikněte na **Connect**.

    ![Aplikace SSMS připojit k serveru](preparing-databases/_static/image10.png)
3. V **Průzkumník objektů** okně rozbalte **databáze**, klikněte pravým tlačítkem na **aspnet ContosoUniversity**, klikněte na tlačítko **úlohy**a potom klikněte na **Generování skriptů**.

    ![Aplikace SSMS generování skriptů](preparing-databases/_static/image11.png)
4. V **generování a publikovat skripty** dialogové okno, klikněte na tlačítko **nastavit možnosti skriptování**.

    Můžete přeskočit **zvolte objekty** krok, protože výchozí hodnota je **skript celou databázi a všechny databázové objekty** a který je co chcete použít.
5. Klikněte na tlačítko **rozšířené**.

    ![Možnosti skriptování SSMS](preparing-databases/_static/image12.png)
6. V **pokročilé možnosti skriptování** dialogové okno, přejděte dolů k **typy dat do skriptu**a klikněte na tlačítko **pouze Data** možnost v rozevíracím seznamu.
7. Změna **skriptu použít databázi** k **False**. POUŽIJTE příkazy nejsou platné pro databázi SQL Azure a nejsou potřebné pro nasazení do systému SQL Server Express v testovacím prostředí.

    ![Aplikace SSMS dat pouze skriptu, žádný příkaz USE](preparing-databases/_static/image13.png)
8. Click **OK**.
9. V **generování a publikovat skripty** dialogové okno, **název souboru** pole určuje, kde se vytvoří skript. Změňte cestu pro vaše řešení složku (složka, která obsahuje váš soubor ContosoUniversity.sln) a názvu souboru *aspnet. data dev.sql*.
10. Klikněte na tlačítko **Další** přejít na **Souhrn** a pak klikněte **Další** znovu a vytvořit skript.

    ![Aplikace SSMS skript vytvořený](preparing-databases/_static/image14.png)
11. Klikněte na tlačítko **Dokončit**.

### <a name="create-the-production-database-script"></a>Vytvoření skriptu provozní databáze

Vzhledem k tomu, že jste nespustili projektu s provozní databáze, není dosud připojen k instanci LocalDB. Proto je třeba nejprve připojit databázi.

1. V aplikaci SSMS **Průzkumník objektů**, klikněte pravým tlačítkem na **databáze** a klikněte na tlačítko **Attach**.

    ![Připojení aplikace SSMS](preparing-databases/_static/image15.png)
2. V **připojit databáze** dialogové okno, klikněte na tlačítko **přidat** a potom přejděte na *aspnet. ContosoUniversity Prod.mdf* v soubor *aplikace\_ Data* složky.

     ![Aplikace SSMS přidejte soubory MDF připojit](preparing-databases/_static/image16.png)
3. Click **OK**.
4. Postupujte podle stejného postupu, který jste použili k vytvoření skriptu pro produkční soubor dříve. Název souboru skriptu *aspnet. data prod.sql*.

## <a name="summary"></a>Souhrn

Obě databáze jsou teď připravená k nasazení a máte dva skripty nasazení data ve složce řešení.

![Skripty nasazení dat](preparing-databases/_static/image17.png)

V následujícím kurzu jste nakonfigurovali nastavení projektu, které ovlivňují nasazení, a nastavit automatické *Web.config* souboru transformace pro nastavení, které musí být odlišné v nasazené aplikace.

## <a name="more-information"></a>Další informace

Další informace o NuGet najdete v tématu [spravovat projektu knihovny s NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) a [NuGet dokumentaci](http://docs.nuget.org/docs/start-here/overview). Pokud nechcete použít NuGet, budete potřebovat další informace o analýze balíčku NuGet k určení, jak funguje, když je nainstalovaná. (Například může nakonfigurovat *Web.config* transformace, konfigurovat skripty prostředí PowerShell, aby se spouštěla v čase vytvoření buildu atd.) Další informace o tom, jak NuGet funguje, najdete v části [vytváření a publikování balíčku](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) a [konfigurační soubor a zdrojový kód transformace](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Předchozí](introduction.md)
> [další](web-config-transformations.md)
