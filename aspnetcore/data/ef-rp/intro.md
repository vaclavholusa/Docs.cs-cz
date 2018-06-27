---
title: Stránky Razor Entity Framework základní v ASP.NET Core - kurz 1 8
author: rick-anderson
description: Ukazuje, jak vytvořit aplikaci stránky Razor pomocí Entity Framework Core
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/intro
ms.openlocfilehash: 97ae8a0f268d3ca6fb2deee72128714ab6e89266
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961357"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>Stránky Razor Entity Framework základní v ASP.NET Core - kurz 1 8

::: moniker range="= aspnetcore-2.0"
ASP.NET 2.0 základní verzi v tomto kurzu lze nalézt v [tento PDF soubor](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).
::: moniker-end
::: moniker range=">= aspnetcore-2.1"

Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikaci ASP.NET Core Razor stránky pomocí základní Entity Framework (EF).

Ukázková aplikace je web pro fiktivní vysoké školy Contoso. Obsahuje funkce, jako je jejich příchodu student, postupu vytvoření a přiřazení lektorem. Tato stránka je první ze série kurzů, které vysvětlují, jak k vytvoření ukázkové aplikace Contoso univerzity.

[Stažení nebo zobrazení dokončené aplikace.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Pokyny ke stahování](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Požadavky

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[! Zahrnout [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

[! [] – INCLUDE (~ / includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]

------

Znalost [stránky Razor](xref:razor-pages/index). By se měla dokončit programátory [začít pracovat s stránky Razor](xref:tutorials/razor-pages/razor-pages-start) před zahájením této série.

## <a name="troubleshooting"></a>Poradce při potížích

Pokud narazíte na problém nevyřešíte, obvykle můžete najít řešení tak, že porovnáte svůj kód [dokončený projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). Je dobrým způsobem, jak získat nápovědu zveřejněním dotaz a [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) pro [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) nebo [EF základní](https://stackoverflow.com/questions/tagged/entity-framework-core).

## <a name="the-contoso-university-web-app"></a>Contoso univerzity webové aplikace

Aplikace vytvořené v těchto kurzech je základní univerzity webovou stránku.

Uživatelé mohou zobrazit a aktualizovat studenty, během a lektorem informace. Zde najdete několik z obrazovky v tomto kurzu vytvořili.

![Studenti, kteří indexovou stránku](intro/_static/students-index.png)

![Stránka upravit studenty](intro/_static/student-edit.png)

Styl uživatelského rozhraní této lokality je blízko co je generován integrované šablony. Kurz zaměřuje se na základní EF s stránky Razor, uživatelské rozhraní ne.

## <a name="create-the-contosouniversity-razor-pages-web-app"></a>Vytvoření stránky Razor ContosoUniversity webové aplikace

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.
* Vytvořte novou webovou aplikaci ASP.NET Core. Název projektu **ContosoUniversity**. Je třeba název projektu *ContosoUniversity* tak shoda s obory názvů, pokud kód je zkopírované a vložené.
* Vyberte **ASP.NET Core 2.1** v rozevírací nabídce a potom vyberte **webové aplikace**.

Pro Image předchozích kroků, najdete v části [vytvořit webovou aplikaci Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).
Spusťte aplikaci.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a>Nastavení stylu lokality

Několik změny nastavení lokality nabídky, rozložení a domovské stránky. Aktualizace *Pages/Shared/_Layout.cshtml* s následujícími změnami:

* Změňte všechny výskyty "ContosoUniversity" na "Univerzity Contoso". Existují tři události.

* Přidání položek nabídky pro **studenty**, **kurzy**, **vyučující**, a **oddělení**a odstranit **kontaktujte** položku nabídky.

Změny se zvýrazněnou. (Všechny značky se *není* zobrazit.)

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

V *Pages/Index.cshtml*, nahraďte obsah souboru následující kód, který nahradí text o ASP.NET a MVC o této aplikaci:

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a>Vytvoření datového modelu

Vytvoření třídy entity pro aplikaci univerzity Contoso. Můžete začít s následující tři entity:

![Diagram modelu dat během. registrace studenty](intro/_static/data-model-diagram.png)

Je vztah jeden mnoho mezi `Student` a `Enrollment` entity. Je vztah jeden mnoho mezi `Course` a `Enrollment` entity. Student může zaregistrovat libovolný počet kurzy. Kurz může mít libovolný počet studenti, kteří jsou v ní zaregistrované.

V následujících částech se vytvoří třídu pro každé z nich o těchto entitách.

### <a name="the-student-entity"></a>Student entity

![Student entity diagram](intro/_static/student-entity.png)

Vytvoření *modely* složky. V *modely* složky, vytvořte soubor třídy s názvem *Student.cs* následujícím kódem:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

`ID` Vlastnost stane sloupec primárního klíče tabulky databáze (databáze), která odpovídá této třídy. Ve výchozím nastavení, EF základní interpretuje vlastnost s názvem `ID` nebo `classnameID` jako primární klíč. V `classnameID`, `classname` je název třídy. Alternativním automaticky rozpozná, primární klíč je `StudentID` v předchozím příkladu.

`Enrollments` Je navigační vlastnost. Navigační vlastnosti propojit s dalšími subjekty, které se vztahují k této entity. V takovém případě `Enrollments` vlastnost `Student entity` obsahuje všechny `Enrollment` entit, které se vztahují k které `Student`. Například pokud studentů řádek v databázi existují dvě související registrace řádky, `Enrollments` navigační vlastnost obsahuje tyto dvě `Enrollment` entity. Související `Enrollment` řádek je řádek, který obsahuje hodnotu primárního klíče tohoto Studentova v `StudentID` sloupce. Předpokládejme například, student s ID = 1, má dva řádky v `Enrollment` tabulky. `Enrollment` Tabulka má dva řádky s `StudentID` = 1. `StudentID` cizí klíč v `Enrollment` tabulku, která určuje studenty ve `Student` tabulky.

Pokud navigační vlastnost může obsahovat více entit, navigační vlastnost musí být typu seznamu, jako například `ICollection<T>`. `ICollection<T>` lze zadat, nebo typu, jako `List<T>` nebo `HashSet<T>`. Když `ICollection<T>` se používá, vytvoří základní EF `HashSet<T>` kolekce ve výchozím nastavení. Navigační vlastnosti, které mají více entit pocházet z relace m: n a jeden mnoho.

### <a name="the-enrollment-entity"></a>Registrace entity

![Diagram entity registrace](intro/_static/enrollment-entity.png)

V *modely* složku vytvořit *Enrollment.cs* následujícím kódem:

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` Vlastnost je primární klíč. Používá tato entita `classnameID` vzor místo `ID` jako `Student` entity. Vývojáři obvykle zvolte jeden vzor a použít ho v rámci datového modelu. Novější kurzu pomocí ID bez classname se zobrazí usnadňují implementace dědičnosti v datovém modelu.

`Grade` Vlastnost je `enum`. Otazník po `Grade` deklaraci typu znamená, že `Grade` vlastnost umožňuje hodnotu Null. Třída, která má hodnotu null se liší od nulové úrovni – null znamená úrovni není známý nebo ještě nebyly přiřazeny.

`StudentID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Student`. `Enrollment` Entita je spojen s jednou `Student` entit, tak vlastnost obsahuje jediný `Student` entity. `Student` Entity se liší od `Student.Enrollments` navigační vlastnost, která obsahuje více `Enrollment` entity.

`CourseID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Course`. `Enrollment` Entita je spojen s jednou `Course` entity.

Vlastnost EF základní interpretuje jako cizí klíč, pokud je název `<navigation property name><primary key property name>`. Například`StudentID` pro `Student` navigační vlastnost, protože `Student` primárního klíče entity je `ID`. Vlastnosti cizího klíče může nazývat také `<primary key property name>`. Například `CourseID` vzhledem k tomu `Course` je primární klíč entity `CourseID`.

### <a name="the-course-entity"></a>Během entity

![Diagram postupu entity](intro/_static/course-entity.png)

V *modely* složku vytvořit *Course.cs* následujícím kódem:

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

`Enrollments` Je navigační vlastnost. A `Course` entity může souviset s libovolný počet `Enrollment` entity.

`DatabaseGenerated` Místo nutnosti databáze generovat atribut umožňuje aplikacím určit primární klíč.

## <a name="scaffold-the-student-model"></a>Vygenerované uživatelské rozhraní student modelu

V této části se vygeneroval student modelu. To znamená vytvoří nástroj pro generování uživatelského rozhraní stránky pro operace vytvoření, čtení, aktualizace a odstranění (CRUD) pro studenty model.

* Sestavte projekt.
* Vytvořte *stránek/studenty* složky.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* V **Průzkumníku řešení**, klikněte pravým tlačítkem na *stránek/studenty* složky > **přidat** > **novou vygenerovanou položku**.
* V **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **stránky Razor pomocí Entity Framework (CRUD)** > **přidat**.

Dokončení **přidat stránky Razor pomocí Entity Framework (CRUD)** dialogové okno:

* V **třída modelu** rozevíracího seznamu, vyberte **Student (ContosoUniversity.Models)**.
* V **třída kontextu dat** řádek, vyberte **+** (a) přihlásit a změňte vygenerovaný název **ContosoUniversity.Models.SchoolContext**.
* V **třída kontextu dat** rozevíracího seznamu, vyberte **ContosoUniversity.Models.SchoolContext**
* Vyberte **přidat**.

![Dialogové okno CRUD](intro/_static/s1.png)

V tématu [vygenerovat model film](xref:tutorials/razor-pages/model#scaffold-the-movie-model) Pokud máte potíže s předchozím kroku.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Spusťte následující příkazy k automatickému vygenerování student modelu.

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

Proces vygenerované uživatelské rozhraní vytvořené a změněné následující soubory:

### <a name="files-created"></a>Vytvořené soubory

* *Stránky/studenty* vytvořit, odstranit, podrobnosti, úpravy, Index.
* *Data/ContosoUniversityContext.cs*

### <a name="files-updates"></a>Soubory aktualizací

* *Startup.cs* : změny tohoto souboru v jsou podrobně popsané v další části.
* *appSettings.JSON určený* : Přidání připojovací řetězec použitý pro připojení k místní databázi.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Zkontrolujte kontext zaregistrována vkládání závislostí

ASP.NET Core je vytvořené s [vkládání závislostí](xref:fundamentals/dependency-injection). Služby (například kontext databáze základní EF) jsou registrovány vkládání závislostí při spuštění aplikace. Součásti, které vyžadují tyto služby (například stránky Razor) jsou k dispozici tyto služby prostřednictvím konstruktor parametry. Později v tomto kurzu se zobrazí kód konstruktor, který získá kontext instanci databáze.

Nástroj pro generování uživatelského rozhraní automaticky vytvořen kontext databáze a zaregistrována kontejneru pro vkládání závislostí.

Zkontrolujte `ConfigureServices` metoda v *Startup.cs*. Zvýrazněný řádek byl přidán modulem scaffolder:

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

Název připojovacího řetězce, je předaná do kontextu voláním metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) objektu. Pro místní vývoj [ASP.NET Core konfigurační systém](xref:fundamentals/configuration/index) čte připojovací řetězec z *appSettings.JSON určený* souboru.

## <a name="update-main"></a>Aktualizovat hlavní

V *Program.cs*, změnit `Main` metoda proveďte následující:

* Zjištění instance kontextu DB z kontejneru pro vkládání závislostí.
* Volání [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).
* Uvolnění kontextu při `EnsureCreated` metoda dokončí.

Následující kód ukazuje aktualizovaný *Program.cs* souboru.

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

`EnsureCreated` zajišťuje, že existuje databáze pro daný kontext. Pokud existuje, nebyla provedena žádná akce. Pokud neexistuje, pak se vytvoří databáze a všechny jeho schématu. `EnsureCreated` k vytvoření databáze nepoužívá migrace. Databázi, která je vytvořena s `EnsureCreated` nelze později aktualizovat pomocí migrace.

`EnsureCreated` je volána při spuštění aplikace, která umožňuje následující pracovní postup:

* Odstranění databáze.
* Změnit schéma databáze (například přidat `EmailAddress` pole).
* Spusťte aplikaci.
* `EnsureCreated` vytvoří databáze s`EmailAddress` sloupce.

`EnsureCreated` je vhodné v rané fázi vývoj, pokud je schéma rychle vyvíjejí. Později v tomto kurzu se odstraní databázi a migrace se používají.

### <a name="test-the-app"></a>Testování aplikace

Spusťte aplikaci a přijměte zásady souboru cookie. Tato aplikace nepodporuje uchovává osobní údaje. Další informace o souboru cookie zásady v [podporu Evropa obecné Data Protection nařízení (GDPR)](xref:security/gdpr).

* Vyberte **studenty** odkazu a potom **vytvořit nový**.
* Upravit podrobnosti, testování a odstraňte odkazy.

## <a name="examine-the-schoolcontext-db-context"></a>Zkontrolujte kontext SchoolContext databáze

Hlavní třída, která koordinuje EF základní funkce pro daný datový model je třídy kontextu databáze. Data kontextu je odvozený od [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Data kontextu určuje entit, které jsou zahrnuty v datovém modelu. V tomto projektu je třída s názvem `SchoolContext`.

Aktualizace *SchoolContext.cs* následujícím kódem:

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

Zvýrazněný kód vytvoří [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) vlastnosti pro každou sadu entit. V terminologii EF jádra:

* Obvykle sadu entit odpovídá Databázové tabulce.
* Entity odpovídá na řádek v tabulce.

`DbSet<Enrollment>` a `DbSet<Course>` může být vynechán. Základní EF zahrnuje je implicitně v protože `Student` odkazy na entity `Enrollment` entity a `Enrollment` odkazy na entity `Course` entity. V tomto kurzu zachovat `DbSet<Enrollment>` a `DbSet<Course>` v `SchoolContext`.

### <a name="sql-server-express-localdb"></a>Databáze SQL Server Express LocalDB

Určuje připojovací řetězec [SQL serveru LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb). LocalDB je Odlehčená verze SQL Server Express Database Engine a je určen pro vývoj aplikací, není použití v provozním prostředí. LocalDB spustí na vyžádání a běží v uživatelském režimu, takže není žádná komplexní konfigurace. Ve výchozím nastavení, vytvoří instanci LocalDB *.mdf* DB soubory `C:/Users/<user>` adresáře.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Přidat kód pro inicializaci databáze s testovací data

Základní EF vytvoří prázdná databáze. V této části `Initialize` metoda je zapsán do jeho naplnění testovacích datech.

V *Data* složky, vytvořte nový soubor třídy s názvem *DbInitializer.cs* a přidejte následující kód:

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

Kód kontroluje, zda existují jakékoli studenty v databázi. Pokud nejsou žádné studenty v databázi, databázi je inicializován testovacích datech. Načte testovací data do pole místo `List<T>` kolekce za účelem optimalizace výkonu.

`EnsureCreated` Metoda automaticky vytvoří databázi pro kontext databáze. Pokud existuje databáze, `EnsureCreated` vrátí beze změny databáze.

V *Program.cs*, změnit `Main` metoda k volání `Initialize`:

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

Odstraňte všechny záznamy studentů a restartujte aplikaci. Pokud databáze není inicializován, nastavit bod přerušení `Initialize` a diagnostikovat problém.

## <a name="view-the-db"></a>Zobrazení databáze

Otevřete **Průzkumník objektů systému SQL Server** (SSOX) z **zobrazení** nabídky v sadě Visual Studio.
V SSOX, klikněte na **\MSSQLLocalDB (localdb) > databáze > ContosoUniversity1**.

Rozbalte **tabulky** uzlu.

Klikněte pravým tlačítkem myši **Student** tabulky a klikněte na tlačítko **Data zobrazení** zobrazíte vytvořit sloupce a řádky vloženy do tabulky.

## <a name="asynchronous-code"></a>Asynchronní kódu

Asynchronní programování je výchozí režim pro ASP.NET Core a EF jádra.

Webový server má omezený počet vláken, které jsou k dispozici, a v situacích, vysokého zatížení všech dostupných vláken může být používán. Pokud k tomu dojde, server nemůže zpracovat nové žádosti o, dokud se uvolní vláken. Synchronní kód může být vázáno mnoho vláken při jejich nejsou to skutečně veškerou práci, protože se čeká vstupně-výstupních operací na dokončení. Asynchronní kód když proces čeká na vstupně-výstupních operací na dokončení, je jeho přístup z více vláken uvolněno pro server, aby používal pro zpracování dalších požadavků. V důsledku toho asynchronní kódu umožňuje prostředky serveru použije efektivněji a serveru zapnutá, abyste zvládli větší provoz bez zpoždění.

Asynchronní kódu v době běhu zavést malé množství režijní náklady. Pro situace, nízkým provozem stiskněte klávesu výkonu je nepatrné při pro intenzivní provoz situace, potenciální zlepšení výkonu je výrazně.

V následujícím kódu [asynchronní](/dotnet/csharp/language-reference/keywords/async) – klíčové slovo, `Task<T>` vrátit hodnotu, `await` – klíčové slovo, a `ToListAsync` metoda zkontrolujte kód provést asynchronně.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* `async` – Klíčové slovo instruuje kompilátor, aby:
  * Generovat zpětných volání pro části těla metody.
  * Automaticky vytvářet [úloh](/dotnet/api/system.threading.tasks.task) objekt, který je vrácen. Další informace najdete v tématu [úloh návratového typu](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Implicitní návratový typ `Task` reprezentuje probíhající práce.
* `await` – Klíčové slovo způsobí, že kompilátor metodu rozdělit na dvě části. První část končí operace, které se spustí asynchronně. Druhá část je uvést do metody zpětného volání, která je volána po dokončení operace.
* `ToListAsync` je asynchronní verzi `ToList` metoda rozšíření.

Třeba mít na paměti při zápisu asynchronní kód, který používá základní EF několik věcí:

* Pouze příkazy, které způsobily dotazy nebo příkazy k odeslání do databáze se spustí asynchronně. Zahrnující, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, a `SaveChangesAsync`. Neobsahuje příkazy, které právě změnit `IQueryable`, jako například `var students = context.Students.Where(s => s.LastName == "Davolio")`.
* Kontextu EF jádra není zaručeno bezpečné používání vláken: nemáte pokusí provést více operací paralelně.
* Abyste mohli využívat výhod výkonu asynchronní kódu, ověřte, že knihovna balíčky (například pro stránkování) používat asynchronní, pokud volají EF základní metody, které odesílají dotazy do databáze.

Další informace o asynchronní programování v rozhraní .NET najdete v tématu [přehled asynchronních](/dotnet/articles/standard/async) a [asynchronní programování pomocí modifikátoru async a operátoru await](/dotnet/csharp/programming-guide/concepts/async/).

V dalším kurzu základní CRUD (vytvořit, číst, aktualizovat, odstraňovat) byla.
::: moniker-end

> [!div class="step-by-step"]
> [Next](xref:data/ef-rp/crud)