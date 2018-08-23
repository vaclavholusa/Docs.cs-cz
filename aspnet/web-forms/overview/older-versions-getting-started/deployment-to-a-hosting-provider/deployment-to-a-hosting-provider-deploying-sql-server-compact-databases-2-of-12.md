---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení databází SQL Server Compact - 2 z 12 | Dokumentace Microsoftu'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného projektu webové aplikace...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 378bcc038335ee852cd1a6c6e545eb72c6e0c78b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753266"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení databází SQL Server Compact - 2 z 12
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Můžete také použít Visual Studio 2010 při instalaci aktualizace Publikovat Web. Úvod do řady, naleznete v tématu [z prvního kurzu této série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz ukazuje nasazení funkce zavedená po verzi RC sady Visual Studio 2012, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

Tento kurz ukazuje, jak nastavit dvě databáze systému SQL Server Compact a databázový stroj pro nasazení.

Pro přístup k databázi aplikace Contoso University vyžaduje následující software, který musí být nasazeny s aplikací, protože není zahrnutý v rozhraní .NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (databázový stroj).
- [Balíček ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (umožňující systém členství technologie ASP.NET použití systému SQL Server Compact)
- [Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First s migrace).

Struktura databáze a některých (ne všechny) dat v aplikačním dvě databáze musí být také nasazený. Obvykle když budete vyvíjet aplikace, zadáte testovací data do databáze, které nemá k nasazení do živého webu. Však můžete například zadat některé provozních dat, kterou chcete nasadit. V tomto kurzu nakonfigurujete projekt Contoso University tak, aby vyžadovaném softwaru a správné údaje jsou zahrnuty, když nasazujete.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact a SQL Server Express

Ukázková aplikace používá SQL Server Compact 4.0. Tento databázový stroj je relativně nové možnosti pro stránky starší verze systému SQL Server Compact nefungují v je prostředí hostování webů. SQL Server Compact nabízí několik výhod oproti běžné scénáře vývoj s využitím SQL serveru Express a nasazení na plnou instalaci systému SQL Server. V závislosti na poskytovateli hostitelských služeb, které zvolíte SQL Server Compact může být levnější nasadit, protože někteří poskytovatelé účtovat navíc pro podporu celé databáze serveru SQL Server. Neexistuje žádné další poplatky za SQL Server Compact, protože samotný modul databáze můžete nasadit jako součást webové aplikace.

Však byste také měli vědět, jaká jsou její omezení. SQL Server Compact nepodporuje uložených procedur, aktivačních událostí, zobrazení nebo replikace. (Úplný seznam funkcí systému SQL Server, které nejsou podporované SQL Server Compact, naleznete v tématu [rozdíly mezi SQL Server Compact a SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Kromě toho některé nástroje, které vám umožní pracovat s schémat a dat v systému SQL Server Express a databáze systému SQL Server nebude fungovat s SQL serverem Compact. Například nelze použít SQL Server Management Studio nebo SQL Server Data Tools v sadě Visual Studio, s databázemi SQL Server Compact. Máte další možnosti pro práci s databázemi SQL Server Compact:

- Můžete použít Průzkumníka serveru ve Visual Studiu, která nabízí funkce pro manipulaci s omezené databáze pro SQL Server Compact.
- Můžete použít funkci zpracování databáze [WebMatrix](https://www.microsoft.com/web/webmatrix/), který nabízí víc funkcí než Průzkumníka serveru.
- Můžete použít relativně plně vybavené třetích stran nebo open source nástroje, jako [sada nástrojů SQL Server Compact](https://github.com/ErikEJ/SqlCeToolbox) a [SQL Compact data a schéma skriptu nástroj](https://github.com/ErikEJ/SqlCeToolbox).
- Můžete napsat a spouštět vlastní skripty DDL (data definition language) k manipulaci s schéma databáze.

Můžete začít s SQL serverem Compact a pak upgradovat později, jak se vyvíjí vaše potřeby. Novější kurzy v této sérii ukazují, jak k migraci z SQL Server Compact, SQL Server Express a SQL Server. Pokud vytváříte nové aplikace a očekávají, že v blízké budoucnosti potřebovat systému SQL Server, je však pravděpodobně nejlepší začít s SQL Server nebo SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Konfigurace SQL Server Compact databázový stroj pro nasazení

Software vyžadovaný pro přístup k datům v aplikaci Contoso univerzitě byla přidána instalací následujících balíčků NuGet:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (balíček ASP.NET universal providers)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Odkazují na aktuální verze tyto balíčky, které by mohly být novější než jaká je nainstalována v počáteční projekt, který jste stáhli pro účely tohoto kurzu. Pro nasazení k poskytovateli hostingu Ujistěte se, že používáte Entity Framework 5.0 nebo novější. Starší verze systému migrace Code First vyžadují úplný vztah důvěryhodnosti a v mnoha poskytovatelé hostingu nastavení aplikace poběží na úrovni Medium Trust. Další informace o úrovni Medium Trust, najdete v článku [nasazení do služby IIS jako testovacího prostředí](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) kurzu.

Instalace balíčku NuGet obecně se postará o všechno, co potřebujete k nasazení tohoto softwaru s aplikací. V některých případech to zahrnuje úlohy, jako je změna souboru Web.config a přidávání skriptů PowerShell, které se spustí pokaždé, když vytvoříte řešení. **Pokud chcete přidat podporu pro žádnou z těchto funkcí (jako je například SQL Server Compact a Entity Framework) bez použití NuGet, ujistěte se, že víte, co dělá instalace balíčku NuGet tak, že je možné stejnou práci provést ručně.**

Existuje jedna výjimka kde NuGet není postupujte opatrně všeho, co musíte udělat, aby se zajistilo úspěšné nasazení. Balíček SqlServerCompact NuGet přidá skriptu po sestavení do projektu, který kopíruje nativní sestavení *x86* a *amd64* podsložky v projektu *bin* složka, ale skript neobsahuje tyto složky v projektu. V důsledku toho Web Deploy nebude zkopírujte je do cílového webu není-li ručně je vložíte do projektu. (Toto chování vyplývá z výchozí konfigurace nasazení, je další možností, které nebudete používat v těchto kurzech, chcete-li změnit nastavení, která určuje toto chování. Nastavení, která může měnit je **pouze soubory potřebné ke spuštění aplikace** pod **položky, které chcete nasadit** na **balení/publikování webu** karty **projektu Vlastnosti** okna. Změna tohoto nastavení se nedoporučuje obecně vzhledem k tomu může vyústit v nasazení s mnoha další soubory do produkčního prostředí, než jsou potřeba existuje. Další informace o alternativách najdete v tématu [konfigurace vlastností projektu](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) kurz.)

Sestavte projekt a potom v **Průzkumníka řešení** klikněte na tlačítko **zobrazit všechny soubory** Pokud jste tak již neučinili. Budete také muset klikněte na tlačítko **aktualizovat**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Rozbalte **bin** složky **amd64** a **x86** složek a pak vyberte tyto složky, klikněte pravým tlačítkem a vyberte **zahrnout do projektu**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Ikony složku změní, že složka byla součástí projektu.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Konfigurace migrace Code First pro nasazení aplikace databáze

Při nasazení databáze aplikace obvykle není nasadíte jednoduše databázi vývoj se všechna data v něm do produkčního prostředí, protože velká část dat v něm je pravděpodobně existuje pouze pro účely testování. Jména studentů v testovací databázi jsou například fiktivní. Na druhé straně často se nedají nasadit jenom struktura databáze bez dat v něm vůbec. Některá data v databázi testu může být reálná data a musí se uživatelé začít používat aplikace. Vaše databáze může mít například tabulku, která obsahuje hodnoty platná třída nebo názvy skutečných oddělení.

Pro simulaci tomto běžném scénáři, nakonfigurujete první migrace počáteční kód metodu, která se vloží do databáze pouze data, která má být k dispozici v produkčním prostředí. Tuto metodu počáteční hodnoty nebude testovací data vložit, protože poběží v produkčním prostředí po Code First vytvoří databázi v produkčním prostředí.

V dřívějších verzích Code First před migrací, bylo běžné metody počáteční hodnoty pro vložení testovací data, protože při každé změně modelu během vývoje databáze byl zcela odstranit a znovu vytvářet úplně od začátku. Pomocí migrace Code First testovací data se uchovávají po provedení změn databáze, takže není nutné včetně testovacích dat v metodě počáteční hodnoty. Projekt, který jste si stáhli používá metodu před migrací, včetně všech dat v metodě počáteční hodnoty inicializátor třídy. V tomto kurzu budete zakázat inicializátor třídy a povolení migrace. Metoda počáteční hodnoty ve třídě konfigurace migrace pak budete aktualizovat tak, aby pouze data, která má být vložen do produkčního prostředí.

Následující diagram znázorňuje schéma databáze aplikace:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Pro tyto kurzy vám předpokládá, že `Student` a `Enrollment` tabulky by měla být prázdná, při prvním nasazení webu. Ostatní tabulky obsahují data, která musí být předem načtena, když dostane živé aplikace.

Vzhledem k tomu, že se pomocí migrace Code First, máte již nebude používat **DropCreateDatabaseIfModelChanges** Code First inicializátor. Kód pro tento inicializátor je v souboru SchoolInitializer.cs ContosoUniversity.DAL projektu. Nastavení **appSettings** element v souboru Web.config způsobí, že tento inicializátor pokaždé, když se aplikace snaží o přístup k databázi pro první spuštění:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Otevřete aplikační soubor Web.config a odeberte elementu, který určuje třídu inicializátor Code First z elementu appSettings. AppSettings element nyní vypadá takto:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Jiný způsob, jak určit třídu inicializátoru je provést zavoláním `Database.SetInitializer` v `Application_Start` metodu *Global.asax* souboru. Pokud povolíte migrace v projektu, který tato metoda používá k určení inicializátoru, odeberte tento řádek kódu.


V dalším kroku povolení migrace Code First.

Prvním krokem je zajistit, že je projekt ContosoUniversity nastavit jako spouštěný projekt. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a vyberte **nastavit jako spouštěný projekt**. Migrace Code First bude hledat v projektu po spuštění a najděte připojovací řetězec databáze.

Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven** a potom **Konzola správce balíčků**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

V horní části **Konzola správce balíčků** okno vybrat jako výchozí projekt a potom at ContosoUniversity.DAL `PM>` řádku zadejte "enable migrace".

![Povolit migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Tento příkaz vytvoří *Configuration.cs* souboru v novém *migrace* složky ContosoUniversity.DAL projektu.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Můžete vybrat projekt DAL, protože příkaz "enable migrace" musí být spuštěn v projektu, který obsahuje třídu Code First kontextu. Když tuto třídu v projektu knihovny tříd, migrace Code First hledá připojovací řetězec databáze v projektu po spuštění pro řešení. V řešení ContosoUniversity webový projekt byl nastaven jako spouštěný projekt. (Pokud nechcete určit projekt, který obsahuje připojovací řetězec jako spouštěný projekt v sadě Visual Studio, můžete použít projekt při spuštění v příkazu prostředí PowerShell. Pokud chcete prohlédnout syntaxi příkazu pro příkaz povolení migrace, můžete zadat příkaz "get-help enable migrace".)

Otevřete soubor Configuration.cs a nahraďte komentáře v `Seed` metodu s následujícím kódem:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Odkazy na `List` mít červenou vlnovkou řádky pod nimi, protože není nutné `using` příkaz ještě pro svůj obor názvů. Klikněte pravým tlačítkem na jednu z instancí `List` a klikněte na tlačítko **vyřešit**a potom klikněte na tlačítko **pomocí System.Collections.Generic**.

![Řešení s využitím – příkaz](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Tento výběr nabídky přidá následující kód, který `using` příkazů v horní části souboru.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Přidání kódu `Seed` metoda je jednou z mnoha způsoby, pevných datových můžete vložit do databáze. Alternativou je přidejte kód, který `Up` a `Down` metody třídy každou migraci. `Up` a `Down` metody obsahovat kód, který implementuje změny databáze. Zobrazí se vám příklady v [nasazení aktualizace databáze](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) kurzu.
> 
> Můžete také napsat kód, který se spustí s použitím příkazů jazyka SQL `Sql` metody. Například, pokud bylo přidání rozpočtu sloupce do tabulky oddělení a chcete inicializovat všechna rozpočty na $ 1 000,00 jako součást migrace, můžete přidat folllowing řádku kódu `Up` metoda této migrace:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> Tento příklad ukazuje pro tento kurz používá `AddOrUpdate` metodu `Seed` metoda migrace Code First `Configuration` třídy. Kód volá první migrace `Seed` za každou migraci a tato metoda aktualizuje řádky, které již byl vložen a vloží je, pokud ještě neexistují. `AddOrUpdate` Metody nemusí být nejlepší volbou pro váš scénář. Další informace najdete v tématu [postará metodou AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.


Stisknutím klávesy CTRL-SHIFT-B a sestavte projekt.

Dalším krokem je vytvoření `DbMigration` třídu pro počáteční migraci. Chcete, aby tato migrace k vytvoření nové databáze, takže budete muset odstranit databázi, která již existuje. Databáze systému SQL Server Compact jsou obsaženy v *SDF* soubory *aplikace\_Data* složky. V **Průzkumníka řešení**, rozbalte *aplikace\_Data* zobrazit dvě databáze systému SQL Server Compact do projektu ContosoUniversity, které jsou reprezentovány *SDF*soubory.

Klikněte pravým tlačítkem myši *School.sdf* souboru a klikněte na tlačítko **odstranit**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

V **Konzola správce balíčků** okno, zadejte příkaz "Přidat migrace počáteční" vytvoření počáteční migraci a pojmenujte ho "Počáteční".

![Přidat migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migrace Code First vytvoří další soubor třídy v *migrace* složky a tato třída obsahuje kód, který vytvoří schéma databáze.

V **Konzola správce balíčků**, zadejte příkaz "update databáze" k vytvoření databáze a spusťte **počáteční hodnoty** metody.

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Pokud dojde k chybě, která určuje již existuje a nedá se vytvořit tabulku, je možné, že jste spustili aplikaci po odstranění databáze a předtím, než jste spustili `update-database`. In that Case, odstranit *School.sdf* soubor znovu a zkuste to znovu `update-database` příkazu.)

Spusťte aplikaci. Nyní studenty stránka je prázdná, ale Instruktoři stránka obsahuje Instruktoři. Je to, co zobrazí se v produkčním prostředí po nasazení aplikace.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Projekt je nyní připraven k nasazení *School* databáze.

## <a name="creating-a-membership-database-for-deployment"></a>Vytváří se databáze členství pro nasazení

Aplikace Contoso University používá ověřování formuláře a systém členství technologie ASP.NET k ověřování a autorizaci uživatelů. Jedním z jeho stránky je dostupné jenom správcům. Zobrazit tuto stránku, spusťte aplikaci a vyberte **aktualizace kredity** z kontextovou nabídku pod **kurzy**. Zobrazí aplikace **přihlásit** stránce, protože pouze správci jsou oprávnění k použití **aktualizace kredity** stránky.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Přihlaste se jako "admin" pomocí hesla, které "Pa$ w0rd" (Všimněte si, že číslo nula místo písmeno "o" v "w0rd"). Po přihlášení **aktualizace kredity** zobrazí se stránka.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Při první nasadíte lokalitu, je běžné vyloučit většinu nebo všechny uživatelské účty, které vytvoříte pro testování. V takovém případě budete nasazovat s oprávněními správce a žádné uživatelské účty. Místo ručně odstranit testovací účty, vytvoříte novou databázi členství, který má pouze na jeden uživatelský účet správce, který potřebujete v produkčním prostředí.

> [!NOTE]
> Databáze členství ukládá hodnota hash hesla účtu. Za účelem nasazení účty z jednoho počítače do jiného, ujistěte se, že hash rutiny negenerovat různé hodnoty hash na cílovém serveru, než na zdrojovém počítači. Nich vydá stejné hodnoty hash při použití technologie ASP.NET Universal Providers, dokud nezměníte výchozí algoritmus. Výchozí algoritmus je HMACSHA256 a je uveden v **ověření** atribut **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** element v souboru Web.config.


Databáze členství není spravován migrace Code First a neexistuje žádný inicializátor automatické, který nasazení nasazuje databáze s testovací účty (jako je databáze školy). Proto zajistit testovacích dat, které jsou k dispozici budete vytvořte kopii testovací databázi před vytvořením nového.

V **Průzkumníka řešení**, přejmenovat *aspnet.sdf* ve *aplikace\_Data* složku *aspnet Dev.sdf*. (Není vytvořit kopii, stačí ho přejmenovat – ve chvíli, než vytvoříte novou databázi.)

V **Průzkumníka řešení**, ujistěte se, že je vybraný webový projekt (ContosoUniversity, ne ContosoUniversity.DAL). Pak v **projektu** nabídce vyberte možnost **konfigurace ASP.NET** ke spuštění **nástroje pro správu webu**(WAT).

Vyberte **zabezpečení** kartu.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Klikněte na tlačítko **vytvořit nebo spravovat role** a přidat **správce** role.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Přejděte zpět **zabezpečení** klikněte na tlačítko **Create User**, a přidejte uživatele "admin" jako správce. Před kliknutím na **vytvořit uživatele** tlačítko **vytvořit uživatele** stránky, ujistěte se, že jste vybrali **správce** zaškrtávací políčko. Heslo použité v tomto kurzu je "Pa$ w0rd" a můžete zadat libovolnou e-mailovou adresu.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Zavřete prohlížeč. V **Průzkumníka řešení**, klikněte na tlačítko aktualizace zobrazíte nové *aspnet.sdf* souboru.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Klikněte pravým tlačítkem na **aspnet.sdf** a vyberte **zahrnout do projektu**.

## <a name="distinguishing-development-from-production-databases"></a>Rozlišovací vývoj z provozních databází

V této části přejmenujete databáze tak, aby verze vývoj jsou Dev.sdf školy a je Prod.sdf školy a aspnet Prod.sdf aspnet Dev.sdf a produkční verze. To není nezbytné, ale to uděláte tak vám pomůže pokračovat můžete získat testovací a produkční verze databáze zaměňovat.

V **Průzkumníka řešení**, klikněte na tlačítko **aktualizovat** a rozbalte ji\_složku Data do databáze školy, který jste vytvořili dříve, pravým tlačítkem myši a vyberte **zahrnout do projektu** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Přejmenovat *aspnet.sdf* k *aspnet Prod.sdf*.

Přejmenovat *School.sdf* k *školní Dev.sdf*.

Při spuštění aplikace v sadě Visual Studio, které nechcete použít *-Prod* verze souborů databáze, kterou chcete použít *– vývoj* verze. Proto je nutné změnit připojovací řetězce v souboru Web.config tak, aby ukazovaly na *– vývoj* verze databáze. (Jste ještě nevytvořili soubor Prod.sdf školy, ale, který je v pořádku protože Code First se vytvoří databáze v čase produkční při prvním spuštění aplikace existuje.)

Otevřete aplikační soubor Web.config a vyhledejte připojovací řetězce:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Změňte "aspnet.sdf" na "aspnet Dev.sdf" a změňte "School.sdf" na "School Dev.sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Databázový stroj SQL Server Compact a obě databáze jsou teď připravená k nasazení. V následujícím kurzu nastavíte automatické *Web.config* souboru transformace pro nastavení, která se musí lišit v vývoj, testování a produkční prostředí. (Mezi nastavení, které musí být změněny jsou připojovací řetězce, ale nastavíte tyto změny později při vytváření profilu publikování.)

## <a name="more-information"></a>Další informace

Další informace o NuGet naleznete v tématu [spravovat projektu knihovny se zmírněními hrozeb NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) a [dokumentace pro NuGet](http://docs.nuget.org/docs/start-here/overview). Pokud už nechcete používat NuGet, budete muset zjistěte, jak analyzovat určit, co to dělá při instalaci balíčku NuGet. (Například můžou nakonfigurovat *Web.config* například transformace konfigurace skriptů Powershellu, aby se spouštěla v okamžiku sestavení atd.) Další informace o tom, jak NuGet funguje, najdete v článku zejména [vytváření a publikování balíčku](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) a [konfigurační soubor a zdrojový kód transformace](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [další](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
