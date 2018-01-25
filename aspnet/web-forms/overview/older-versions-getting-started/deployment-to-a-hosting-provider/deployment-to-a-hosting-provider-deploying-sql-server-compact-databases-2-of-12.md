---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: "Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení databáze SQL Server Compact - 2 12 | Microsoft Docs"
author: tdykstra
description: "Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5296bc1ca3fd0b24123bd79a550a7e2cffc34a44
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení databáze SQL Server Compact - 2 12
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web. Úvod do řady, najdete v části [z prvního kurzu řady](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, naleznete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

Tento kurz ukazuje, jak nastavit dvě databáze systému SQL Server Compact a databázový stroj pro nasazení.

Pro přístup k databázi aplikace Contoso univerzity vyžaduje následující software, který musí být nasazený s aplikací, protože není zahrnutý v rozhraní .NET Framework:

- [Systém SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (databázový stroj).
- [Balíček ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (která umožňují používat systém SQL Server Compact členství technologie ASP.NET)
- [Rozhraní Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First s migrace).

Struktura databáze a některé (ne všechny) dat v aplikačním dvě databáze musí být také nasazen. Obvykle když budete vyvíjet aplikace, zadáte testovací data do databáze, které nechcete použít k nasazení na živý web. Můžete však také zadat produkční data, která chcete nasadit. V tomto kurzu nakonfigurujete projektu univerzity Contoso tak, aby požadovaný software a správná data jsou zahrnuty, když nasazujete.

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact versus SQL Server Express

Ukázková aplikace používá SQL Server Compact 4.0. Tento databázový stroj je relativně nové možnosti pro weby; dřívějších verzích systému SQL Server Compact nefungují ve webovém hostitelském prostředí. Systém SQL Server Compact nabízí několik výhod, které jsou ve srovnání s více běžný scénář vývoj pomocí SQL Server Express a nasazení na plnou instalaci systému SQL Server. V závislosti na poskytovateli hostitelských služeb, které zvolíte systém SQL Server Compact může být levnější chcete nasadit, protože někteří poskytovatelé účtují navíc k podpoře úplnou databázi systému SQL Server. Vzhledem k tomu, že databázový stroj samotné můžete nasadit jako součást webové aplikace není pro systém SQL Server Compact bez dalších poplatků.

Ale můžete také měli vědět jeho omezení. Systém SQL Server Compact nepodporuje uložené procedury, triggery, zobrazení nebo replikace. (Úplný seznam funkcí systému SQL Server, které nepodporují systém SQL Server Compact, najdete v části [rozdíly mezi systém SQL Server Compact a SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Navíc některé nástroje, které můžete použít k manipulaci s schémata a data v systému SQL Server Express a databáze SQL serveru nefungují s systém SQL Server Compact. V sadě Visual Studio s databázemi SQL Server Compact nelze například použít SQL Server Management Studio nebo SQL Server Data Tools. Máte další možnosti pro práci s databázemi SQL Server Compact:

- V Průzkumníku serveru můžete použít v sadě Visual Studio, který nabízí funkce manipulaci s omezenou databáze pro systém SQL Server Compact.
- Můžete použít funkci zpracování databáze [WebMatrix](https://www.microsoft.com/web/webmatrix/), který má více funkcí než Průzkumníka serveru.
- Můžete použít relativně plné třetích stran nebo otevřete source nástroje, jako [SQL Server Compact nástrojů](https://github.com/ErikEJ/SqlCeToolbox) a [SQL Compact dat a schématu skriptu nástroj](https://github.com/ErikEJ/SqlCeToolbox).
- Můžete napsat a spouštět vlastní skripty DDL (data definition language) k manipulaci s schématu databáze.

Můžete začít s SQL Server Compact a pak upgradovat později momentální vašim potřebám. Další kurzy v této série ukazují, jak migrovat ze systému SQL Server Compact do systému SQL Server Express a k systému SQL Server. Pokud vytváříte novou aplikaci a očekávají, že v blízké budoucnosti potřebovat SQL Server, je však pravděpodobně nejlepším řešením začít s SQL Server nebo SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Konfigurace SQL Server Compact databázový stroj pro nasazení

Software vyžadovaný pro přístup k datům v aplikaci Contoso univerzity, byl přidán instalací následujících balíčků NuGet:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (balíčku ASP.NET universal providers)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Odkazují na aktuální verze těchto balíčků, které může být novější než co je nainstalované v projektu starter, který jste stáhli pro účely tohoto kurzu. Pro nasazení do hostujícího zprostředkovatele Ujistěte se, že používáte rozhraní Entity Framework 5.0 nebo novější. Starší verze migrace Code First vyžadují úplný vztah důvěryhodnosti a v mnoha poskytovatelům hostingu aplikace poběží na úrovni Medium Trust. Další informace o úrovni Medium Trust, najdete v článku [nasazení do IIS jako testovacím prostředí](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) kurzu.

Instalace balíčku NuGet obecně postará vše, co potřebujete k nasazení tohoto softwaru s aplikací. V některých případech to zahrnuje úlohy, jako je změna souboru Web.config a přidávání skriptů prostředí PowerShell, které spuštění, kdykoli sestavte řešení. **Pokud chcete přidat podporu pro některé z těchto funkcí (například systém SQL Server Compact a Entity Framework) bez použití NuGet, ujistěte se, že víte, co instalace balíčku NuGet umožňuje, aby je stejný pracovní udělat ručně.**

Existuje jedna výjimka kde NuGet neberou stará se o vše, co musíte udělat, aby se zajistilo úspěšné nasazení. Přidává balíček SqlServerCompact NuGet skript po sestavení do projektu, který kopíruje nativní sestavení, které chcete *x86* a *amd64* podsložky v projektu *bin* složka, ale skript nezahrnuje těchto složek v projektu. V důsledku toho Web Deploy nebude zkopírujte je do cílového webu Pokud je je ručně zadat v projektu. (Toto chování výsledkem výchozí konfigurace nasazení, je jinou možnost, která používáte nebude v těchto kurzech, chcete-li změnit nastavení, které řídí toto chování. Nastavení, které můžete změnit **pouze soubory potřebné ke spuštění aplikace** pod **položky k nasazení** na **balení/publikování webu** kartě **projektu Vlastnosti** okno. Změna tohoto nastavení se nedoporučuje obecně protože může vést k nasazení mnoho další soubory do produkčního prostředí, než je potřeba existuje. Další informace o alternativách najdete v tématu [konfigurace vlastností projektu](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) kurzu.)

Sestavte projekt a potom v **Průzkumníku řešení** klikněte na tlačítko **zobrazit všechny soubory** Pokud jste tak již neučinili. Budete také muset klikněte na tlačítko **aktualizovat**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Rozbalte **bin** složky zobrazíte **amd64** a **x86** složek a potom vyberte tyto složky, klikněte pravým tlačítkem a vyberte **zahrnout do projektu**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Ikony složky změní, složce byl zahrnut v projektu.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Konfigurace migrace Code First pro nasazení databáze aplikace

Když nasadíte databázi aplikace, obvykle nenasazujete jednoduše vývojové databáze se všechna data v ní do produkčního prostředí, protože pouze data v něm je pravděpodobně existuje pouze pro účely testování. Například jsou fiktivních student názvy v testovací databáze. Na druhé straně často nemůžete nasadit právě struktura databáze bez dat v něm vůbec. Některá data v databázi testu může být reálná data a musí být existuje, když uživatelé před zahájením používání aplikace. Například databáze může mít tabulku, která obsahuje hodnoty platný základní nebo názvy skutečné oddělení.

Pro simulaci tomto běžném scénáři, nakonfigurujete metodu první Seed migrace kódu, která vloží do databáze pouze data, která chcete existovat v produkčním prostředí. Tato metoda počáteční hodnoty nebude vložit testovací data, protože se spustí v produkčním prostředí poté, co Code First vytvoří databázi v produkčním prostředí.

V dřívějších verzích Code First před vydáním migrace, bylo běžné metody počáteční hodnoty vložit testovací data také, protože při každé změně modelu během vývoje neprobíhala v databázi úplně odstranit a znovu vytvořit od začátku. Se migrace Code First je testovací data uchovávají změny v databázi, takže není nutné včetně testovací data v metodě počáteční hodnoty. Projekt, který jste stáhli používá metodu před migrací včetně všech dat v metodě počáteční hodnoty inicializátoru třídy. V tomto kurzu budete zakažte třídě inicializátoru a povolit migrace. Potom budete aktualizovat metodu počáteční hodnoty v třídě konfigurace migrace tak, aby pouze data, která má být vložen v produkčním prostředí.

Následující diagram znázorňuje schéma databáze aplikace:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Tyto kurzy budete předpokládá, že `Student` a `Enrollment` tabulky by měla být prázdná, při prvním nasazení webu. Jiné tabulky obsahují data, která musí být předem načtena, kdy aplikace přestane za provozu.

Vzhledem k tomu, že budete používat migrace Code First, již nebudou muset použít **DropCreateDatabaseIfModelChanges** inicializátoru Code First. Kód pro tento inicializátoru je v souboru SchoolInitializer.cs v ContosoUniversity.DAL projektu. Nastavení **appSettings** element v souboru Web.config způsobí, že tento inicializátoru ke spuštění, kdykoli se aplikace pokusí o přístup k databázi první:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Otevřete soubor Web.config aplikace a odeberte elementu, který určuje třídu inicializátoru Code First z prvku appSettings. AppSettings element nyní vypadat třeba takto:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Je také možné zadat třídu inicializátoru provést voláním `Database.SetInitializer` v `Application_Start` metoda v *Global.asax* souboru. Pokud chcete povolit migrace v projektu, který tato metoda určuje inicializátoru, odeberte tohoto řádku kódu.


Dál povolte migrace Code First.

Prvním krokem je zajistit, že je projekt ContosoUniversity nastavit jako spouštěný projekt. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a vyberte **nastavit jako spouštěný projekt**. Migrace Code First Hledat v spouštěný projekt najít připojovací řetězec databáze.

Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven** a potom **Konzola správce balíčků**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

V horní části **Konzola správce balíčků** okno Vyberte ContosoUniversity.DAL jako výchozí projekt a pak at `PM>` řádku zadejte "enable-migrations".

![Povolit migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Tento příkaz vytvoří *Configuration.cs* soubor v nové *migrace* složky ContosoUniversity.DAL projektu.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Můžete vybrat projektu DAL, protože je třeba spustit příkaz "enable-migrations" v projektu, který obsahuje Code First třídy kontextu. Po této třídy v projektu knihovny tříd se migrace Code First Vyhledá připojovací řetězec databáze v počáteční projekt pro řešení. V řešení ContosoUniversity byl nastaven webový projekt jako spouštěný projekt. (Pokud není chcete vytvořit projekt, který má připojovací řetězec jako spouštěný projekt v sadě Visual Studio, můžete projekt po spuštění příkazu prostředí PowerShell. Syntaxe příkazu pro příkaz enable-migrations najdete můžete zadat příkazu "get-help enable-migrations".)

Otevřete soubor Configuration.cs a nahraďte komentáře v `Seed` metoda následujícím kódem:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Odkazy na `List` mít červenými vlnovkami nich, protože nemáte `using` příkaz ještě pro její obor názvů. Pravým tlačítkem na jednu instanci `List` a klikněte na tlačítko **vyřešit**a potom klikněte na **pomocí System.Collections.Generic**.

![Řešení s použitím – příkaz](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Tento výběr nabídky přidá následující kód, který `using` příkazy v horní části souboru.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Přidání kódu k `Seed` metoda je jednou z mnoha způsoby, pevných datových můžete vložit do databáze. Alternativou je přidejte kód, který `Up` a `Down` metody jednotlivé třídy migrace. `Up` a `Down` metody obsahovat kód, který implementuje změny v databázi. Uvidíte je příkladem [nasazení aktualizace databáze](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) kurzu.
> 
> Můžete také psát kód, který spustí příkazy SQL pomocí `Sql` metoda. Například pokud byly přidat nároky sloupec v tabulce oddělení a chtěli k chybě při inicializaci všechny rozpočty na 1 000,00 Kč jako součást migrace, můžete přidat folllowing řádku kódu `Up` metoda, pro které migrace:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> Tento příklad pro tento kurz používá `AddOrUpdate` metoda v `Seed` metoda migrace Code First `Configuration` třídy. Migrace First volání kódu `Seed` po každé migraci, přičemž tuto metodu aktualizace řádků, které již byl vložen a vloží je, pokud dosud neexistují. `AddOrUpdate` Metoda nemusí být nejlepším řešením pro váš scénář. Další informace najdete v tématu [postará s EF 4.3 AddOrUpdate metoda](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.


Stisknutím kombinace kláves CTRL-SHIFT-B a tím projekt sestavit.

Dalším krokem je vytvoření `DbMigration` třídu pro počáteční migrace. Chcete tento migrace k vytvoření nové databáze, takže budete muset odstranit databázi, která již existuje. Databáze systému SQL Server Compact jsou obsaženy v *SDF* soubory *aplikace\_Data* složky. V **Průzkumníku řešení**, rozbalte položku *aplikace\_Data* v projektu ContosoUniversity zobrazíte dvě databáze systému SQL Server Compact, která jsou reprezentované pomocí *SDF*soubory.

Klikněte pravým tlačítkem myši *School.sdf* souboru a klikněte na tlačítko **odstranit**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

V **Konzola správce balíčků** okno, zadejte příkaz "Přidat migrace počáteční" k vytvoření počáteční migrace a pojmenujte ji "Počáteční".

![Přidat migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migrace Code First vytvoří další soubor třídy ve *migrace* složky a tato třída obsahuje kód, který vytvoří schématu databáze.

V **Konzola správce balíčků**, zadejte příkazu "update-database" k vytvoření databáze a spusťte **počáteční hodnoty** metoda.

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Pokud dojde k chybě, která označuje tabulka již existuje a nelze vytvořit, je možné, že jste spustili aplikaci po odstranění databáze a před provedení `update-database`. In that Case, odstraňte *School.sdf* znovu a zkuste zopakovat `update-database` příkaz.)

Spusťte aplikaci. Nyní stránce studenti, kteří je prázdný, ale vyučující stránka obsahuje vyučující. Je to, co zobrazí se v produkčním prostředí poté, co nasadíte aplikaci.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Projekt je nyní připraven k nasazení *školní* databáze.

## <a name="creating-a-membership-database-for-deployment"></a>Vytvoření databáze členství pro nasazení

Aplikace Contoso univerzity používá ověřování systému a forms členství technologie ASP.NET pro ověřování a autorizaci uživatelů. Jeden z jeho stránky je přístup jen správci. Pokud chcete zobrazit tuto stránku, spusťte aplikaci a vyberte **aktualizace kredity** plovoucím panelem nabídce v části **kurzy**. Zobrazí aplikace **protokolu v** stránky, protože pouze správci mají oprávnění využívat **aktualizace kredity** stránky.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Přihlaste se jako "admin" pomocí hesla "Pa$ w0rd" (Všimněte si číslo nula místo písmenem "o" v "w0rd"). Po přihlášení se **aktualizace kredity** zobrazí se stránka.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Při prvním nasazení lokality, je běžné vyloučit většinu nebo všechny uživatelské účty, které vytvoříte pro testování. V takovém případě budete nasazovat účtu správce a žádné uživatelské účty. Místo ručním odstranění testovací účty, vytvoříte novou databázi členství, který má jenom jeden správce uživatelský účet, který je nutné v produkčním prostředí.

> [!NOTE]
> Databáze členství ukládá hodnotu hash hesla účtů. Abyste mohli nasadit účty z jednoho počítače do druhého, ujistěte se, že hash rutiny negenerovat různé hodnoty hash na cílovém serveru, než na zdrojovém počítači. Stejné hodnoty hash se generují při použití balíčku ASP.NET Universal Providers tak dlouho, dokud Neměnit výchozí algoritmus. Výchozí algoritmus HMACSHA256 a je určena v **ověření** atribut  **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)**  element v souboru Web.config.


Databáze členství není spravován migrace Code First a neexistuje žádný automatické inicializátoru, který doplňuje databáze s účty testů (jako je pro databázi škola). Proto aby testovací data k dispozici budete vytvořit kopii testovací databáze před vytvořením nového.

V **Průzkumníku řešení**, přejmenujte *aspnet.sdf* v soubor *aplikace\_Data* složku pro *aspnet Dev.sdf*. (Není vytvořit kopii, stačí ho přejmenovat – vytvoříte novou databázi za chvíli.)

V **Průzkumníku**, ujistěte se, že je vybraný webového projektu (ContosoUniversity, není ContosoUniversity.DAL). Potom v **projektu** nabídce vyberte možnost **konfigurace ASP.NET** ke spuštění **nástroj Správa webu**(WAT).

Vyberte **zabezpečení** kartě.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Klikněte na tlačítko **vytvořit nebo spravovat role** a přidejte **správce** role.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Přejděte zpět **zabezpečení** , klikněte na **vytvořit uživatele**, a přidejte uživatele "admin" jako správce. Dřív, než kliknete **vytvořit uživatele** na tlačítko **vytvořit uživatele** se ujistěte, že jste vybrali **správce** zaškrtávací políčko. Heslo použité v tomto kurzu je "Pa$ w0rd" a můžete zadat všechny e-mailovou adresu.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Zavřete prohlížeč. V **Průzkumníku řešení**, klikněte na tlačítko Aktualizovat zobrazíte nové *aspnet.sdf* souboru.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Klikněte pravým tlačítkem na **aspnet.sdf** a vyberte **zahrnout do projektu**.

## <a name="distinguishing-development-from-production-databases"></a>Rozlišovací vývoj z provozní databáze

V této části přejmenujete databáze tak, aby verze vývoj jsou školní Dev.sdf a aspnet Dev.sdf a produkční verze jsou školní Prod.sdf a aspnet Prod.sdf. Tato akce není nezbytné, ale to tak pomůže zabránit získávání zaměňovat testovací a produkční verze databáze.

V **Průzkumníku řešení**, klikněte na tlačítko **aktualizovat** a rozbalte aplikace\_složky dat najdete v části školní databáze, kterou jste vytvořili dříve; pravým tlačítkem myši a vyberte **zahrnout do projektu** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Přejmenování *aspnet.sdf* k *aspnet Prod.sdf*.

Přejmenování *School.sdf* k *školní Dev.sdf*.

Při spuštění aplikace v sadě Visual Studio nechcete použít *-produkčnímu* verze souborů databáze, kterou chcete použít *- Dev* verze. Proto je nutné změnit připojovací řetězce v souboru Web.config, tak, aby ukazovaly na *- Dev* verze databáze. (Jste dosud nevytvořili soubor školní Prod.sdf, ale je to způsobeno Code First vytvoří databázi v době produkční první spuštění aplikace existuje OK.)

Otevřete aplikační soubor Web.config a vyhledejte připojovací řetězce:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Změňte "aspnet.sdf" na "aspnet-Dev.sdf" a "Školní-Dev.sdf" změnit "School.sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Systém SQL Server Compact databázový stroj a obě databáze je teď připravená k nasazení. V následujícím kurzu nastavíte automatické *Web.config* souboru transformace pro nastavení, které musí být odlišné ve vývoj, testování a provozním prostředí. (Mezi nastavení, které je potřeba změnit jsou připojovací řetězce, ale budete nastavíte tyto změny později při vytváření profilu publikování.)

## <a name="more-information"></a>Další informace

Další informace o NuGet najdete v tématu [spravovat projektu knihovny s NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) a [NuGet dokumentaci](http://docs.nuget.org/docs/start-here/overview). Pokud nechcete použít NuGet, budete potřebovat další informace o analýze balíčku NuGet k určení, jak funguje, když je nainstalovaná. (Například může nakonfigurovat *Web.config* transformace, konfigurovat skripty prostředí PowerShell, aby se spouštěla v čase vytvoření buildu atd.) Další informace o tom, jak NuGet funguje, najdete v části hlavně [vytváření a publikování balíčku](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) a [konfigurační soubor a zdrojový kód transformace](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

>[!div class="step-by-step"]
[Předchozí](deployment-to-a-hosting-provider-introduction-1-of-12.md)
[další](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
