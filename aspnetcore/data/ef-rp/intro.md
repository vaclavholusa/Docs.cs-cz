---
title: "Stránky Razor Entity Framework základní – tutoriál 1 8"
author: rick-anderson
description: "Ukazuje, jak vytvořit aplikaci stránky Razor pomocí Entity Framework Core"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/intro
ms.openlocfilehash: bea3b12ebe476c4b59abe117393b0ec8bb7f0306
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a>Začínáme s stránky Razor a Entity Framework Core pomocí sady Visual Studio (1 8)

Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC 2.0 základní pomocí základní Entity Framework (EF) 2.0 a Visual Studio 2017.

Ukázková aplikace je web pro fiktivní vysoké školy Contoso. Obsahuje funkce, jako je jejich příchodu student, postupu vytvoření a přiřazení lektorem. Tato stránka je první ze série kurzů, které vysvětlují, jak k vytvoření ukázkové aplikace Contoso univerzity.

[Stažení nebo zobrazení dokončené aplikace.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Pokyny ke stahování](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Požadavky

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

Znalost [stránky Razor](xref:mvc/razor-pages/index). By se měla dokončit programátory [začít pracovat s stránky Razor](xref:tutorials/razor-pages/razor-pages-start) před zahájením této série.

## <a name="troubleshooting"></a>Poradce při potížích

Pokud narazíte na problém nevyřešíte, obvykle můžete najít řešení tak, že porovnáte svůj kód [dokončit fáze](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) nebo [dokončený projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final). Seznam běžných chyb a jak je vyřešit, najdete v části [části Poradce při potížích s poslední kurz v této sérii](xref:data/ef-mvc/advanced#common-errors). Pokud se nepodařilo najít, co potřebujete existuje, můžete odeslat dotaz na [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) pro [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) nebo [EF základní](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP]
> Tato série kurzů staví na co se provádí v dřívější kurzy. Zvažte možnost uložení kopie projektu po každém úspěšném dokončení kurzu. Pokud narazíte na problémy, můžete spustit přes z předchozí kurzu místo přejdete zpět na začátku. Alternativně můžete stáhnout [dokončit fáze](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) a začít od začátku pomocí fázi dokončené.

## <a name="the-contoso-university-web-app"></a>Contoso univerzity webové aplikace

Aplikace vytvořené v těchto kurzech je základní univerzity webovou stránku.

Uživatelé mohou zobrazit a aktualizovat studenty, během a lektorem informace. Zde najdete několik z obrazovky v tomto kurzu vytvořili.

![Studenti, kteří indexovou stránku](intro/_static/students-index.png)

![Stránka upravit studenty](intro/_static/student-edit.png)

Styl uživatelského rozhraní této lokality je blízko co je generován integrované šablony. Kurz zaměřuje se na základní EF s stránky Razor, uživatelské rozhraní ne.

## <a name="create-a-razor-pages-web-app"></a>Vytvoření stránky Razor webové aplikace

* Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.
* Vytvořte novou webovou aplikaci ASP.NET Core. Název projektu **ContosoUniversity**. Je třeba název projektu *ContosoUniversity* tak shoda s obory názvů, pokud kód je zkopírované a vložené.
 ![nové webové aplikace ASP.NET Core](intro/_static/np.png)
* Vyberte **technologii ASP.NET 2.0 základní** v rozevírací nabídce a potom vyberte **webové aplikace**.
 ![Webové aplikace (stránky Razor)](../../mvc/razor-pages/index/_static/np2.png)

Stiskněte klávesu **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** běžet bez připojení ladicího programu

## <a name="set-up-the-site-style"></a>Nastavení stylu lokality

Několik změny nastavení lokality nabídky, rozložení a domovské stránky.

Otevřete *Pages/_Layout.cshtml* a proveďte následující změny:

* Změňte všechny výskyty "ContosoUniversity" na "Contoso univerzity." Existují tři události.

* Přidání položek nabídky pro **studenty**, **kurzy**, **vyučující**, a **oddělení**a odstranit **kontaktujte** položku nabídky.

Změny se zvýrazněnou. (Všechny značky se *není* zobrazit.)

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

V *Pages/Index.cshtml*, nahraďte obsah souboru následující kód, který nahradí text o ASP.NET a MVC o této aplikaci:

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

Stisknutím kombinace kláves CTRL + F5 a spusťte projekt. Domovská stránka se zobrazí s kartami vytvořené v následujících kurzech:

![Contoso univerzity domovské stránky](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>Vytvoření datového modelu

Vytvoření třídy entity pro aplikaci univerzity Contoso. Můžete začít s následující tři entity:

![Diagram modelu dat během. registrace studenty](intro/_static/data-model-diagram.png)

Je vztah jeden mnoho mezi `Student` a `Enrollment` entity. Je vztah jeden mnoho mezi `Course` a `Enrollment` entity. Student může zaregistrovat libovolný počet kurzy. Kurz může mít libovolný počet studenti, kteří jsou v ní zaregistrované.

V následujících částech se vytvoří třídu pro každé z nich o těchto entitách.

### <a name="the-student-entity"></a>Student entity

![Student entity diagram](intro/_static/student-entity.png)

Vytvoření *modely* složky. V *modely* složky, vytvořte soubor třídy s názvem *Student.cs* následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` Vlastnost stane sloupec primárního klíče tabulky databáze (databáze), která odpovídá této třídy. Ve výchozím nastavení, EF základní interpretuje vlastnost s názvem `ID` nebo `classnameID` jako primární klíč.

`Enrollments` Je navigační vlastnost. Navigační vlastnosti propojit s dalšími subjekty, které se vztahují k této entity. V takovém případě `Enrollments` vlastnost `Student entity` obsahuje všechny `Enrollment` entit, které se vztahují k které `Student`. Například pokud studentů řádek v databázi existují dvě související registrace řádky, `Enrollments` navigační vlastnost obsahuje tyto dvě `Enrollment` entity. Související `Enrollment` řádek je řádek, který obsahuje hodnotu primárního klíče tohoto Studentova v `StudentID` sloupce. Předpokládejme například, student s ID = 1, má dva řádky v `Enrollment` tabulky. `Enrollment` Tabulka má dva řádky s `StudentID` = 1. `StudentID`cizí klíč v `Enrollment` tabulku, která určuje studenty ve `Student` tabulky.

Pokud navigační vlastnost může obsahovat více entit, navigační vlastnost musí být typu seznamu, jako například `ICollection<T>`. `ICollection<T>`lze zadat, nebo typu, jako `List<T>` nebo `HashSet<T>`. Když `ICollection<T>` se používá, vytvoří základní EF `HashSet<T>` kolekce ve výchozím nastavení. Navigační vlastnosti, které mají více entit pocházet z relace m: n a jeden mnoho.

### <a name="the-enrollment-entity"></a>Registrace entity

![Diagram entity registrace](intro/_static/enrollment-entity.png)

V *modely* složku vytvořit *Enrollment.cs* následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` Vlastnost je primární klíč. Používá tato entita `classnameID` vzor místo `ID` jako `Student` entity. Vývojáři obvykle zvolte jeden vzor a použít ho v rámci datového modelu. Novější kurzu pomocí ID bez classname se zobrazí usnadňují implementace dědičnosti v datovém modelu.

`Grade` Vlastnost je `enum`. Otazník po `Grade` deklaraci typu znamená, že `Grade` vlastnost umožňuje hodnotu Null. Třída, která má hodnotu null se liší od nulové úrovni – null znamená úrovni není známý nebo ještě nebyly přiřazeny.

`StudentID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Student`. `Enrollment` Entita je spojen s jednou `Student` entit, tak vlastnost obsahuje jediný `Student` entity. `Student` Entity se liší od `Student.Enrollments` navigační vlastnost, která obsahuje více `Enrollment` entity.

`CourseID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Course`. `Enrollment` Entita je spojen s jednou `Course` entity.

Vlastnost EF základní interpretuje jako cizí klíč, pokud je název `<navigation property name><primary key property name>`. Například`StudentID` pro `Student` navigační vlastnost, protože `Student` primárního klíče entity je `ID`. Vlastnosti cizího klíče může nazývat také `<primary key property name>`. Například `CourseID` vzhledem k tomu `Course` je primární klíč entity `CourseID`.

### <a name="the-course-entity"></a>Během entity

![Diagram postupu entity](intro/_static/course-entity.png)

V *modely* složku vytvořit *Course.cs* následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` Je navigační vlastnost. A `Course` entity může souviset s libovolný počet `Enrollment` entity.

`DatabaseGenerated` Místo nutnosti databáze generovat atribut umožňuje aplikacím určit primární klíč.

## <a name="create-the-schoolcontext-db-context"></a>Vytvoření kontextu SchoolContext DB

Hlavní třída, která koordinuje EF základní funkce pro daný datový model je třídy kontextu databáze. Data kontextu je odvozený od `Microsoft.EntityFrameworkCore.DbContext`. Data kontextu určuje entit, které jsou zahrnuty v datovém modelu. V tomto projektu je třída s názvem `SchoolContext`.

Ve složce projektu vytvořte složku s názvem *Data*.

V *Data* vytvořit složku *SchoolContext.cs* následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Tento kód vytvoří `DbSet` vlastnosti pro každou sadu entit. V terminologii EF jádra:

* Obvykle sadu entit odpovídá Databázové tabulce.
* Entity odpovídá na řádek v tabulce.

`DbSet<Enrollment>`a `DbSet<Course>` lze vynechat. Základní EF zahrnuje je implicitně v protože `Student` odkazy na entity `Enrollment` entity a `Enrollment` odkazy na entity `Course` entity. V tomto kurzu zachovat `DbSet<Enrollment>` a `DbSet<Course>` v `SchoolContext`.

Při vytváření databáze EF základní vytvoří tabulek, které jsou stejné jako názvy `DbSet` názvy vlastností. Názvy vlastností pro kolekce jsou obvykle množném čísle (studenty spíše než Student). Vývojáři Nesouhlasím o tom, jestli by měla být názvy tabulek množném čísle. Pro tyto kurzy je výchozí chování přepsat zadáním názvů singulární tabulek v DbContext. Pokud chcete zadat názvy singulární tabulek, přidejte následující zvýrazněný kód:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Zaregistrovat kontext vkládání závislostí

Zahrnuje ASP.NET Core [vkládání závislostí](xref:fundamentals/dependency-injection). Služby (například kontext databáze základní EF) jsou registrovány vkládání závislostí při spuštění aplikace. Součásti, které vyžadují tyto služby (například stránky Razor) jsou k dispozici tyto služby prostřednictvím konstruktor parametry. Později v tomto kurzu se zobrazí kód konstruktor, který získá kontext instanci databáze.

K registraci `SchoolContext` jako služby, otevřete *Startup.cs*a přidejte zvýrazněné řádky, které se `ConfigureServices` metoda.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

Název připojovacího řetězce, je předaná do kontextu voláním metody na `DbContextOptionsBuilder` objektu. Pro místní vývoj [ASP.NET Core konfigurační systém](xref:fundamentals/configuration/index) čte připojovací řetězec z *appSettings.JSON určený* souboru.

Přidat `using` příkazy pro `ContosoUniversity.Data` a `Microsoft.EntityFrameworkCore` obory názvů. Sestavte projekt.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Otevřete *appSettings.JSON určený* souboru a přidat připojovací řetězec, jak je znázorněno v následujícím kódu:

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

Předchozí připojovací řetězec používá `ConnectRetryCount=0` aby [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) z ukotvených.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Určuje připojovací řetězec databáze SQL Server LocalDB DB. LocalDB je Odlehčená verze SQL Server Express Database Engine a je určen pro vývoj aplikací, není použití v provozním prostředí. LocalDB spustí na vyžádání a běží v uživatelském režimu, takže není žádná komplexní konfigurace. Ve výchozím nastavení, vytvoří instanci LocalDB *.mdf* DB soubory `C:/Users/<user>` adresáře.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Přidat kód pro inicializaci databáze s testovací data

Základní EF vytvoří prázdná databáze. V této části *počáteční hodnoty* metoda je zapsán do jeho naplnění testovacích datech.

V *Data* složky, vytvořte nový soubor třídy s názvem *DbInitializer.cs* a přidejte následující kód:

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

Kód kontroluje, zda existují jakékoli studenty v databázi. Pokud nejsou žádné studenty v databázi, databázi osadit testovacích datech. Načte testovací data do pole místo `List<T>` kolekce za účelem optimalizace výkonu.

`EnsureCreated` Metoda automaticky vytvoří databázi pro kontext databáze. Pokud existuje databáze, `EnsureCreated` vrátí beze změny databáze.

V *Program.cs*, změnit `Main` metoda proveďte následující:

* Zjištění instance kontextu DB z kontejneru pro vkládání závislostí.
* Volejte metodu počáteční hodnoty, předání kontextu.
* Kontext Dispose po dokončení metodu počáteční hodnoty.

Následující kód ukazuje aktualizovaný *Program.cs* souboru.

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

Při prvním spuštění aplikace, databáze se vytvoří a nasadí se testovací data. Při aktualizaci datového modelu:
* Odstranění databáze.
* Aktualizujte metodu počáteční hodnoty.
* Spusťte aplikaci a je vytvořen nový dosazená DB. 

V dalších kurzech aktualizaci databáze při data modelu změny, bez odstranit a znovu vytvořit databázi.

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>Přidat vygenerované uživatelské rozhraní nástroje

V této části konzoly Správce balíčků (PMC) slouží k přidání balíčku generování kódu webové sady Visual Studio. Tento balíček je potřeba spustit modul generování uživatelského rozhraní.

Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.

V balíček správce konzoly (pomocí PMC), zadejte následující příkazy:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

Předchozí příkaz přidá do souboru *.csproj balíčky NuGet:

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>Vygenerované uživatelské rozhraní modelu

* Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).
* Spusťte následující příkazy:


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
Pokud je generována k následující chybě:

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

Spusťte příkaz znovu a komentář v dolní části stránky.

Pokud dojde k chybě:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).


Sestavte projekt. Sestavení generuje chyby takto:

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 Globálně změnit `_context.Student` k `_context.Students` (tedy "s" přidat do `Student`). 7 výskytů jsou vyhledána a aktualizovat. Doufáme, opravte [této chyby](https://github.com/aspnet/Scaffolding/issues/633) v příští verzi.

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>Testování aplikace

Spusťte aplikaci a vyberte **studenty** odkaz. V závislosti na šířku prohlížeče **studenty** se zobrazí v horní části stránky. Pokud **studenty** odkaz se nezobrazuje, klikněte na ikonu Navigace v pravém horním rohu.

![Domovská stránka Contoso univerzity úzké](intro/_static/home-page-narrow.png)

Testovací **vytvořit**, **upravit**, a **podrobnosti** odkazy.

## <a name="view-the-db"></a>Zobrazení databáze

Když se aplikace spustí, `DbInitializer.Initialize` volání `EnsureCreated`. `EnsureCreated`zjistí, pokud existuje databáze a v případě potřeby jej vytvoří. Pokud v databázi, nejsou žádné studenty `Initialize` metoda přidá studenty.

Otevřete **Průzkumník objektů systému SQL Server** (SSOX) z **zobrazení** nabídky v sadě Visual Studio.
V SSOX, klikněte na **\MSSQLLocalDB (localdb) > databáze > ContosoUniversity1**.

Rozbalte **tabulky** uzlu.

Klikněte pravým tlačítkem myši **Student** tabulky a klikněte na tlačítko **Data zobrazení** zobrazíte vytvořit sloupce a řádky vloženy do tabulky.

*.Mdf* a *.ldf* soubory databáze jsou v *C:\Users\\ <yourusername>*  složky.

`EnsureCreated`je volána při spuštění aplikace, která umožňuje následující pracovní postup:

* Odstranění databáze.
* Změnit schéma databáze (například přidat `EmailAddress` pole).
* Spusťte aplikaci.

`EnsureCreated`vytvoří databáze s`EmailAddress` sloupce.

## <a name="conventions"></a>Konvence

Kvůli použití konvence nebo předpoklady, které umožňuje v EF základní je minimální množství kód napsaný v pořadí pro základní EF vytvořit úplný DB.

* Názvy `DbSet` vlastnosti jsou použity jako názvy tabulek. Pro entity není odkazuje `DbSet` vlastnost třídy entita názvy jsou použity jako názvy tabulek.

* Názvy vlastností entity se používají pro názvy sloupců.

* Vlastnosti entity, které jsou s názvem ID nebo classnameID jsou rozpoznat jako vlastnosti primárního klíče.

* Vlastnost interpretována jako vlastností cizího klíče, pokud je název  *<navigation property name> <primary key property name>*  (například `StudentID` pro `Student` navigační vlastnost, protože `Student` je primární klíč entity `ID`). Může mít název vlastnosti cizího klíče  *<primary key property name>*  (například `EnrollmentID` vzhledem k tomu `Enrollment` je primární klíč entity `EnrollmentID`).

Konvenční chování lze přepsat. Například názvy tabulek lze explicitně zadat, jak je znázorněno v tomto kurzu. Názvy sloupců může být explicitně nastaveny. Primární a cizí klíče může být explicitně nastaveny.

## <a name="asynchronous-code"></a>Asynchronní kódu

Asynchronní programování je výchozí režim pro ASP.NET Core a EF jádra.

Webový server má omezený počet vláken, které jsou k dispozici, a v situacích, vysokého zatížení všech dostupných vláken může být používán. Pokud k tomu dojde, server nemůže zpracovat nové žádosti o, dokud se uvolní vláken. Synchronní kód může být vázáno mnoho vláken při jejich nejsou to skutečně veškerou práci, protože se čeká vstupně-výstupních operací na dokončení. Asynchronní kód když proces čeká na vstupně-výstupních operací na dokončení, je jeho přístup z více vláken uvolněno pro server, aby používal pro zpracování dalších požadavků. V důsledku toho asynchronní kódu umožňuje prostředky serveru použije efektivněji a serveru zapnutá, abyste zvládli větší provoz bez zpoždění.

Asynchronní kódu v době běhu zavést malé množství režijní náklady. Pro situace, nízkým provozem stiskněte klávesu výkonu je nepatrné při pro intenzivní provoz situace, potenciální zlepšení výkonu je výrazně.

V následujícím kódu `async` – klíčové slovo, `Task<T>` vrátit hodnotu, `await` – klíčové slovo, a `ToListAsync` metoda zkontrolujte kód provést asynchronně.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* `async` – Klíčové slovo instruuje kompilátor, aby:

  * Generovat zpětných volání pro části těla metody.
  * Automaticky vytvářet [úloh](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) objekt, který je vrácen. Další informace najdete v tématu [úloh návratového typu](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Implicitní návratový typ `Task` reprezentuje probíhající práce.

* `await` – Klíčové slovo způsobí, že kompilátor metodu rozdělit na dvě části. První část končí operace, které se spustí asynchronně. Druhá část je uvést do metody zpětného volání, která je volána po dokončení operace.

* `ToListAsync`je asynchronní verzi `ToList` metoda rozšíření.

Třeba mít na paměti při zápisu asynchronní kód, který používá základní EF několik věcí:

* Pouze příkazy, které způsobily dotazy nebo příkazy k odeslání do databáze se spustí asynchronně. Zahrnující, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, a `SaveChangesAsync`. Nezahrnuje příkazy, které právě změnit `IQueryable`, jako například `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* Kontextu EF jádra není zařazování bezpečné: nemáte pokusí provést více operací paralelně. 

* Abyste mohli využívat výhod výkonu asynchronní kódu, ověřte, že knihovna balíčky (například pro stránkování) používat asynchronní, pokud volají EF základní metody, které odesílají dotazy do databáze.

Další informace o asynchronní programování v rozhraní .NET najdete v tématu [přehled asynchronních](https://docs.microsoft.com/dotnet/articles/standard/async).

V dalším kurzu základní CRUD (vytvořit, číst, aktualizovat, odstraňovat) byla.

>[!div class="step-by-step"]
[Next](xref:data/ef-rp/crud)
