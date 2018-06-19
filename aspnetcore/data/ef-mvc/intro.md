---
title: Jádro ASP.NET MVC s Entity Framework Core - kurz 1 10
author: rick-anderson
description: ''
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/intro
ms.openlocfilehash: 0eef084c656f68fb1cad9fc0f5cfeee31ff96a30
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153774"
---
# <a name="aspnet-core-mvc-with-entity-framework-core---tutorial-1-of-10"></a>Jádro ASP.NET MVC s Entity Framework Core - kurz 1 10

Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [RP better than MVC](../../includes/RP-EF/rp-over-mvc.md)]

Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC 2.0 základní pomocí základní Entity Framework (EF) 2.0 a Visual Studio 2017.

Vzorová aplikace je web pro fiktivní vysoké školy Contoso. Obsahuje funkce, jako je jejich příchodu student, postupu vytvoření a přiřazení lektorem. Toto je první ze série kurzů, které vysvětlují, jak k vytvoření ukázkové aplikace Contoso univerzity od začátku.

[Stažení nebo zobrazení hotová aplikace.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

Základní EF 2.0 je nejnovější verzi EF, ale ještě nemá všechny funkce EF 6.x. Informace o tom, jak zvolit EF 6.x a EF Core, najdete v části [EF základní vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/). Pokud se rozhodnete EF 6.x, najdete v části [předchozí verzi tento kurz řady](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

> [!NOTE]
> * Verze 1.1 jádro ASP.NET v tomto kurzu, najdete v článku [VS 2017 Update 2 verzi v tomto kurzu ve formátu PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).
> * Verze sady Visual Studio 2015 v tomto kurzu, najdete v článku [VS 2015 verzi ASP.NET Core dokumentaci ve formátu PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="troubleshooting"></a>Poradce při potížích

Pokud narazíte na problém nevyřešíte, obvykle můžete najít řešení tak, že porovnáte svůj kód [dokončený projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final). Seznam běžných chyb a jak je vyřešit, najdete v části [části Poradce při potížích s poslední kurz v této sérii](advanced.md#common-errors). Pokud se nepodařilo najít, co potřebujete existuje, můžete odeslat dotaz na StackOverflow.com pro [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) nebo [EF základní](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP] 
> Toto je řadu 10 kurzy, z nichž každý je založený na co se provádí v dřívější kurzy. Zvažte možnost uložení kopie projektu po každém úspěšném dokončení kurzu. Pak pokud narazíte na problémy, můžete spustit přes z předchozí kurzu místo přejdete zpět na začátek celé řady.

## <a name="the-contoso-university-web-application"></a>Contoso univerzity webové aplikace

Aplikace, kterou jste budete vytvářet v těchto kurzech je jednoduchý univerzity webovou stránku.

Uživatelé mohou zobrazit a aktualizovat studenty, během a lektorem informace. Zde najdete několik z obrazovky, které vytvoříte.

![Studenti, kteří indexovou stránku](intro/_static/students-index.png)

![Stránka upravit studenty](intro/_static/student-edit.png)

Styl uživatelského rozhraní tohoto webu byla choval blízko co je generován integrované šablony tak, aby tento kurz můžete zaměřit především na tom, jak používat rozhraní Entity Framework.

## <a name="create-an-aspnet-core-mvc-web-application"></a>Vytvořit webovou aplikaci ASP.NET MVC jádra

Otevřete Visual Studio a vytvořte nové ASP.NET Core C# webového projektu s názvem "ContosoUniversity".

* Z **soubor** nabídce vyberte možnost **nový > projekt**.

* V levém podokně vyberte **nainstalovaná > Visual C# > Web**.

* Vyberte **webové aplikace ASP.NET Core** šablona projektu.

* Zadejte **ContosoUniversity** jako název a klikněte na tlačítko **OK**.

  ![Dialogové okno Nový projekt](intro/_static/new-project.png)

* Počkejte **nové základní webové aplikace ASP.NET (.NET Core)** zobrazit dialogové okno

* Vyberte **technologii ASP.NET 2.0 základní** a **webové aplikace (Model-View-Controller)** šablony.

  **Poznámka:** tento kurz vyžaduje aplikaci ASP.NET 2.0 jádra a základní EF 2.0 nebo novější – Ujistěte se, že **ASP.NET Core 1.1** není vybraná.

* Zajistěte, aby **ověřování** je nastaven na **bez ověřování**.

* Klikněte na tlačítko **OK**

  ![Dialogové okno Nový projekt ASP.NET](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a>Nastavení stylu lokality

Pár jednoduchými změnami nastaví lokality nabídky, rozložení a domovské stránky.

Otevřete *Views/Shared/_Layout.cshtml* a proveďte následující změny:

* Změňte všechny výskyty "ContosoUniversity" na "Univerzity Contoso". Existují tři události.

* Přidání položek nabídky pro **studenty**, **kurzy**, **vyučující**, a **oddělení**a odstranit **kontaktujte** položku nabídky.

Změny se zvýrazněnou.

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

V *Views/Home/Index.cshtml*, nahraďte obsah souboru následující kód, který nahradí text o ASP.NET a MVC o této aplikaci:

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

Stiskněte klávesu CTRL + F5 a spusťte projekt nebo zvolte **ladění > Spustit bez ladění** z nabídky. Zobrazí na domovské stránce pro stránky, které vytvoříte v těchto kurzech karty.

![Contoso univerzity domovské stránky](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a>Entity Framework Core NuGet balíčky

Podpora jádra EF přidat do projektu, nainstalujte zprostředkovatele databáze, kterou chcete cílit. Tento kurz používá systém SQL Server a poskytovatele balíček je [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/). Tento balíček je součástí [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, takže není nutné ji nainstalovat.

Tento balíček a jeho závislosti (`Microsoft.EntityFrameworkCore` a `Microsoft.EntityFrameworkCore.Relational`) poskytují podporu runtime pro EF. Bude potřeba přidat balíček nástrojů dál v [migrace](migrations.md) kurzu. 

Informace o dalších zprostředkovatelů databáze, které jsou k dispozici pro Entity Framework Core najdete v tématu [databáze zprostředkovatelé](https://docs.microsoft.com/ef/core/providers/).

## <a name="create-the-data-model"></a>Vytvoření datového modelu

Dále vytvoříte tříd entit pro danou aplikaci univerzity Contoso. Budete začínat následující tři entity.

![Diagram modelu dat během. registrace studenty](intro/_static/data-model-diagram.png)

Je vztah jeden mnoho mezi `Student` a `Enrollment` entity, a je mezi vztah jeden mnoho `Course` a `Enrollment` entity. Jinými slovy student může být zaregistrované v libovolný počet kurzy a kurzu může mít libovolný počet studenti, kteří jsou v ní zaregistrované.

V následujících částech vytvoříte třídu pro každé z nich o těchto entitách.

### <a name="the-student-entity"></a>Student entity

![Student entity diagram](intro/_static/student-entity.png)

V *modely* složky, vytvořte soubor třídy s názvem *Student.cs* a nahraďte kód šablony s následujícím kódem.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` Vlastnost bude sloupec primárního klíče tabulky databáze, která odpovídá této třídy. Ve výchozím nastavení, rozhraní Entity Framework interpretuje vlastnost s názvem `ID` nebo `classnameID` jako primární klíč.

`Enrollments` Je navigační vlastnost. Navigační vlastnosti podržte dalšími subjekty, které se vztahují k této entity. V takovém případě `Enrollments` vlastnost `Student entity` bude obsahovat všechny `Enrollment` entit, které se vztahují k které `Student` entity. Jinými slovy, pokud daný řádek Student v databázi existují dvě související registrace řádky (řádky, které obsahují hodnotu primárního klíče tohoto Studentova v jejich StudentID sloupec cizího klíče), který `Student` entity `Enrollments` navigační vlastnost těch, které bude obsahovat dva `Enrollment` entity.

Pokud vlastnost navigace mohou být uloženy více entit (jako relace m: n nebo na více), jeho typ musí být seznam, ve kterém položky možné ji přidat, odstranit nebo aktualizovat, například `ICollection<T>`. Můžete zadat `ICollection<T>` , nebo typu, jako `List<T>` nebo `HashSet<T>`. Pokud zadáte `ICollection<T>`, vytvoří EF `HashSet<T>` kolekce ve výchozím nastavení.

### <a name="the-enrollment-entity"></a>Registrace entity

![Diagram entity registrace](intro/_static/enrollment-entity.png)

V *modely* složku vytvořit *Enrollment.cs* a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` Primární klíč, bude mít vlastnost; používá tuto entitu `classnameID` vzor místo `ID` samostatně jako jste viděli v `Student` entity. By normálně zvolte jeden vzor a použít ho v rámci datového modelu. Zde variaci znázorňuje, které můžete použít buď vzor. V [novější kurzu](inheritance.md), uvidíte, jak pomocí ID bez classname usnadňuje implementaci dědičnosti v datovém modelu.

`Grade` Vlastnost je `enum`. Otazník po `Grade` deklaraci typu znamená, že `Grade` vlastnost umožňuje hodnotu Null. Třída, která má hodnotu null se liší od nulové úrovni – null znamená úrovni není známý nebo ještě nebyly přiřazeny.

`StudentID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Student`. `Enrollment` Entita je spojen s jednou `Student` entit, tak vlastnost mohou obsahovat pouze jeden `Student` entity (na rozdíl od `Student.Enrollments` navigační vlastnost jste viděli dříve, která může pojmout více `Enrollment` entity).

`CourseID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Course`. `Enrollment` Entita je spojen s jednou `Course` entity.

Rozhraní Entity Framework interpretuje vlastnost jako vlastnost cizího klíče, pokud je název `<navigation property name><primary key property name>` (například `StudentID` pro `Student` navigační vlastnost, protože `Student` je primární klíč entity `ID`). Taky vlastnosti cizího klíče jednoduše název `<primary key property name>` (například `CourseID` vzhledem k tomu `Course` je primární klíč entity `CourseID`).

### <a name="the-course-entity"></a>Během entity

![Diagram postupu entity](intro/_static/course-entity.png)

V *modely* složku vytvořit *Course.cs* a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` Je navigační vlastnost. A `Course` entity může souviset s libovolný počet `Enrollment` entity.

Budete říkáme, informace o `DatabaseGenerated` atribut [novější kurzu](complex-data-model.md) této série. V zásadě platí tento atribut slouží k zadání primární klíč pro během, místo aby databázi jeho vygenerování.

## <a name="create-the-database-context"></a>Vytvoření kontextu databáze

Hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model je třídy kontextu databáze. Vytvořit této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy. V kódu zadáte entit, které jsou zahrnuty v datovém modelu. Můžete také upravit chování určité Entity Framework. V tomto projektu je třída s názvem `SchoolContext`.

Ve složce projektu vytvořte složku s názvem *Data*.

V *Data* složce vytvořte nový soubor třídy s názvem *SchoolContext.cs*a kód šablony nahraďte následujícím kódem:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Tento kód vytvoří `DbSet` vlastnosti pro každou sadu entit. V terminologii rozhraní Entity Framework obvykle sadu entit, odpovídá do databázové tabulky a entity odpovídá na řádek v tabulce.

Vám může jste vynechání `DbSet<Enrollment>` a `DbSet<Course>` příkazy a bude fungovat stejně. Rozhraní Entity Framework by mělo zahrnovat je implicitně protože `Student` odkazy na entity `Enrollment` entity a `Enrollment` odkazy na entity `Course` entity.

Při vytvoření databáze EF vytvoří tabulek, které jsou stejné jako názvy `DbSet` názvy vlastností. Názvy vlastností pro kolekce jsou obvykle množném čísle (studenty spíše než Student), ale vývojáři Nesouhlasím o tom, jestli by měl názvy tabulek pluralized nebo ne. Tyto kurzy přepíšete výchozí chování zadáním názvů singulární tabulek v DbContext. K tomu, přidejte následující zvýrazněný kód po poslední DbSet vlastnost.

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Zaregistrovat kontext vkládání závislostí

Implementuje ASP.NET Core [vkládání závislostí](../../fundamentals/dependency-injection.md) ve výchozím nastavení. Služby (například kontext databáze EF) jsou registrovány vkládání závislostí při spuštění aplikace. Součásti, které vyžadují tyto služby (například řadiče MVC) jsou k dispozici tyto služby prostřednictvím konstruktor parametry. Zobrazí kód řadiče konstruktor, který získá instance kontextu později v tomto kurzu.

K registraci `SchoolContext` jako služby, otevřete *Startup.cs*a přidejte zvýrazněné řádky, které se `ConfigureServices` metoda.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

Název připojovacího řetězce, je předaná do kontextu voláním metody na `DbContextOptionsBuilder` objektu. Pro místní vývoj [ASP.NET Core konfigurační systém](xref:fundamentals/configuration/index) čte připojovací řetězec z *appSettings.JSON určený* souboru.

Přidat `using` příkazy pro `ContosoUniversity.Data` a `Microsoft.EntityFrameworkCore` obory názvů a potom sestavte projekt.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Otevřete *appSettings.JSON určený* souboru a přidat připojovací řetězec, jak je znázorněno v následujícím příkladu.

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a>Databáze SQL Server Express LocalDB

Připojovací řetězec Určuje databázi SQL serveru LocalDB. LocalDB je Odlehčená verze SQL Server Express Database Engine a je určena pro vývoj aplikací, není použití v provozním prostředí. LocalDB spustí na vyžádání a běží v uživatelském režimu, takže není žádná komplexní konfigurace. Ve výchozím nastavení, vytvoří instanci LocalDB *.mdf* soubory v databáze `C:/Users/<user>` adresáře.

## <a name="add-code-to-initialize-the-database-with-test-data"></a>Přidat kód pro inicializaci databáze s testovací data

Rozhraní Entity Framework pro vytvoření prázdnou databázi. V této části napíšete metodu, která je volána po vytvoření databáze k naplnění testovacích datech.

Tady budete používat `EnsureCreated` metoda automaticky vytvořit databázi. V [novější kurzu](migrations.md) uvidíte, jak zpracovat změny modelu s použitím migrace Code First, chcete-li změnit schéma databáze místo vyřadit a znovu vytvořit databázi.

V *Data* složky, vytvořte nový soubor třídy s názvem *DbInitializer.cs* a nahraďte následujícím kódem, což způsobí, že databáze, který se má vytvořit v případě potřeby kód šablony a zatížením testovacích dat do nové databáze.

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

Kód ověří, zda existují jakékoli studenty v databázi, a pokud ne, předpokládá, databáze je nový a musí být vyplněn testovacích datech. Načte testovací data do pole místo `List<T>` kolekce za účelem optimalizace výkonu.

V *Program.cs*, změnit `Main` metodu na spuštění aplikace, postupujte takto:

* Instance kontextu databáze získáte z kontejneru pro vkládání závislostí.
* Volejte metodu počáteční hodnoty, předání kontextu.
* Uvolnění kontextu, pokud se provádí metodu počáteční hodnoty.

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

Přidat `using` příkazy:

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

V starší kurzy, se může zobrazit podobné kód `Configure` metoda v *Startup.cs*. Doporučujeme vám, že používáte `Configure` metoda jenom chcete nastavit kanál požadavku. Kód spuštění aplikace patří `Main` metoda.

Nyní při prvním spuštění aplikace, databáze bude vytvořen a osadit testovacích datech. Při každé změně datového modelu, můžete odstranit databázi, aktualizovat metodu počáteční hodnoty a začít znovu s novou databázi stejným způsobem jako. V dalších kurzech uvidíte, jak k úpravám databáze, když datový model změny, bez odstranit a znovu ji vytvořit.

## <a name="create-a-controller-and-views"></a>Vytvoření kontroleru a zobrazení

V dalším kroku použijete modul generování uživatelského rozhraní v sadě Visual Studio přidat řadič MVC a zobrazení, které budou používat EF pro dotazování a uložit data.

Automatické vytváření metody akce CRUD a zobrazení se označuje jako generování uživatelského rozhraní. Generování uživatelského rozhraní se liší od generování kódu, jsou automaticky generovaný kód počáteční bod, který můžete upravit tak, aby vyhovovala vaším požadavkům, zatímco obvykle Neupravovat generovaného kódu. Pokud budete potřebovat k přizpůsobení generovaného kódu, použít částečné třídy nebo při změně věcí znovu vygenerovat kód.

* Klikněte pravým tlačítkem myši **řadiče** složky v **Průzkumníku řešení** a vyberte **Přidat > novou vygenerovanou položku**.

Pokud **přidat závislosti MVC** otevře se dialogové okno:

* [Aktualizovat na nejnovější verzi sady Visual Studio](https://www.visualstudio.com/downloads/). Visual Studio verze starší než 15,5 zobrazit tento dialog.
* Pokud nelze aktualizovat, vyberte **přidat**a pak postupujte podle kroků řadiče přidat.

* V **přidat vygenerované uživatelské rozhraní** dialogové okno:

  * Vyberte **kontroler MVC s zobrazeními, s využitím nástroje Entity Framework**.

  * Klikněte na tlačítko **přidat**.

* V **přidat kontroler** dialogové okno:

  * V **třída modelu** vyberte **Student**.

  * V **třída kontextu dat** vyberte **SchoolContext**.

  * Přijměte výchozí nastavení **StudentsController** jako název.

  * Klikněte na tlačítko **přidat**.

  ![Student vygenerované uživatelské rozhraní](intro/_static/scaffold-student.png)

  Když kliknete na tlačítko **přidat**, modul generování uživatelského rozhraní sady Visual Studio vytvoří *StudentsController.cs* souboru a sadu zobrazení (*.cshtml* soubory), pracovat s řadičem.

(Generování uživatelského rozhraní modulu také můžete vytvořit kontext databáze pro vás Pokud nemáte vytvořte ručně nejprve, jako jste to udělali dříve v tomto kurzu. Můžete zadat novou třídu v kontextu **přidat kontroler** kliknutím na symbol plus napravo od pole **třída kontextu dat**.  Potom vytvoří sada Visual Studio vaší `DbContext` třídy a také řadiče a zobrazení.)

Můžete si všimnout, že trvá kontroleru `SchoolContext` jako parametr konstruktoru.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

Vkládání závislostí ASP.NET se postará o předání instanci `SchoolContext` do kontroleru. Jste nakonfigurovali, že *Startup.cs* výše.

Obsahuje kontroler `Index` metody akce, která zobrazuje všechny studenty v databázi. Metoda získá seznam studenty ze sady načtením studenty entit `Students` vlastností kontextu instance databáze:

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

Později v tomto kurzu získáte informace o asynchronní elementům programování v tomto kódu.

*Views/Students/Index.cshtml* zobrazení zobrazí tento seznam v tabulce:

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

Stiskněte klávesu CTRL + F5 a spusťte projekt nebo zvolte **ladění > Spustit bez ladění** z nabídky.

Klikněte na kartu studenty zobrazíte testovací data, `DbInitializer.Initialize` metoda vložit. V závislosti na tom, jak úzké okno prohlížeče se zobrazí `Student` odkaz karty v horní části stránky nebo budete muset klikněte na ikonu Navigace v pravém horním rohu na odkaz zobrazíte.

![Domovská stránka Contoso univerzity úzké](intro/_static/home-page-narrow.png)

![Studenti, kteří indexovou stránku](intro/_static/students-index.png)

## <a name="view-the-database"></a>Zobrazení databáze

Při spuštění aplikace, `DbInitializer.Initialize` volání metod `EnsureCreated`. EF viděli, že se žádná databáze a proto vytvoří jednu pak zbytek `Initialize` metoda kód naplní databázi s daty. Můžete použít **Průzkumník objektů systému SQL Server** (SSOX) Chcete-li zobrazit databázi v sadě Visual Studio.

Zavřete prohlížeč.

Pokud už není otevřené okno SSOX, vyberte ho v **zobrazení** nabídky v sadě Visual Studio.

V SSOX, klikněte na **\MSSQLLocalDB (localdb) > databáze**a pak klikněte na položku pro název databáze, který je v připojovacím řetězci v vaše *appSettings.JSON určený* souboru.

Rozbalte **tabulky** uzlu zobrazíte tabulky v databázi.

![Tabulky v SSOX](intro/_static/ssox-tables.png)

Klikněte pravým tlačítkem myši **Student** tabulky a klikněte na tlačítko **Data zobrazení** zobrazíte sloupce, které byly vytvořeny a řádky, které byly vloženy do tabulky.

![Tabulka Student v SSOX](intro/_static/ssox-student-table.png)

<em>.Mdf</em> a <em>.ldf</em> databázové soubory jsou v <em>C:\Users\\ <yourusername> </em> složky.

Protože jste volání `EnsureCreated` v metodě inicializátoru, který spouští při spuštění aplikace, můžete dokonce vytvářet teď ke změně `Student` třídy, odstraňte tuto databázi, spusťte aplikaci znovu a databáze bude automaticky znovu vytvořit tak, aby odpovídaly vaší změn. Například, pokud přidáte `EmailAddress` vlastnost, která má `Student` třídy, se zobrazí nový `EmailAddress` sloupec v tabulce znovu vytvořena.

## <a name="conventions"></a>Konvence

Kvůli použití konvence nebo předpoklady, které umožňuje Entity Framework je minimální množství kód, který jste měli k zápisu, aby rozhraní Entity Framework, abyste mohli vytvořit databázi dokončení pro vás.

* Názvy `DbSet` vlastnosti jsou použity jako názvy tabulek. Pro entity není odkazuje `DbSet` vlastnost třídy entita názvy jsou použity jako názvy tabulek.

* Názvy vlastností entity se používají pro názvy sloupců.

* Vlastnosti entity, které jsou s názvem ID nebo classnameID jsou rozpoznat jako vlastnosti primárního klíče.

* Vlastnost interpretována jako vlastností cizího klíče, pokud je název *<navigation property name> <primary key property name>* (například `StudentID` pro `Student` navigační vlastnost, protože `Student` je primární klíč entity `ID`). Taky vlastnosti cizího klíče jednoduše název *<primary key property name>* (například `EnrollmentID` vzhledem k tomu `Enrollment` je primární klíč entity `EnrollmentID`).

Konvenční chování lze přepsat. Například můžete explicitně zadáte názvy tabulek, jako jste viděli dříve v tomto kurzu. A můžete nastavit názvy sloupců a nastavte libovolnou vlastnost jako primární klíč, cizí klíč, nebo jak uvidíte v [novější kurzu](complex-data-model.md) této série.

## <a name="asynchronous-code"></a>Asynchronní kódu

Asynchronní programování je výchozí režim pro ASP.NET Core a EF jádra.

Webový server má omezený počet vláken, které jsou k dispozici, a v situacích, vysokého zatížení všech dostupných vláken může být používán. Pokud k tomu dojde, server nemůže zpracovat nové žádosti o, dokud se uvolní vláken. Synchronní kód může být vázáno mnoho vláken při jejich nejsou to skutečně veškerou práci, protože se čeká vstupně-výstupních operací na dokončení. Asynchronní kód když proces čeká na vstupně-výstupních operací na dokončení, je jeho přístup z více vláken uvolněno pro server, aby používal pro zpracování dalších požadavků. V důsledku toho asynchronní kódu umožňuje prostředky serveru použije efektivněji a serveru zapnutá, abyste zvládli větší provoz bez zpoždění.

Asynchronní kódu v době běhu zavést malé množství režijní náklady na, ale pro situace nízkým provozem, stiskněte klávesu výkonu je nepatrné při pro intenzivní provoz situace, je výrazně potenciální zvýšení výkonu.

V následujícím kódu `async` – klíčové slovo, `Task<T>` vrátit hodnotu, `await` – klíčové slovo, a `ToListAsync` metoda zkontrolujte kód provést asynchronně.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* `async` – Klíčové slovo říká kompilátoru generování zpětných volání pro části těla metody a automaticky vytvářet `Task<IActionResult>` objekt, který je vrácen.

* Návratový typ `Task<IActionResult>` reprezentuje probíhající práce s tím výsledkem typu `IActionResult`.

* `await` – Klíčové slovo způsobí, že kompilátor metodu rozdělit na dvě části. První část končí operace, které se spustí asynchronně. Druhá část je uvést do metody zpětného volání, která je volána po dokončení operace.

* `ToListAsync` je asynchronní verzi `ToList` metoda rozšíření.

Některé věci zajímat, když píšete asynchronní kód, který používá rozhraní Entity Framework:

* Pouze příkazy, které způsobily dotazy nebo příkazy k odeslání do databáze se spustí asynchronně. Například zahrnující `ToListAsync`, `SingleOrDefaultAsync`, a `SaveChangesAsync`. Neobsahuje, například příkazy, které právě změnit `IQueryable`, jako například `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* Kontextu EF není zaručeno bezpečné používání vláken: nemáte pokusí provést více operací paralelně. Při volání metody EF všechny asynchronní, vždy nutné použít `await` – klíčové slovo.

* Pokud chcete využít výhod výkonu asynchronní kódu, ujistěte se, že všechny knihovny balíčky, které používáte (například konkrétní stránkování), také použít asynchronní, pokud volají žádné metody rozhraní Entity Framework, které způsobí dotazy k odeslání do databáze.

Další informace o asynchronní programování v rozhraní .NET najdete v tématu [přehled asynchronních](https://docs.microsoft.com/dotnet/articles/standard/async).

## <a name="summary"></a>Souhrn

Nyní jste vytvořili jednoduchou aplikaci, která se používá k uložení a zobrazení data Entity Framework Core a SQL Server Express LocalDB. V následujícím kurzu se dozvíte jak provádět základní CRUD (vytvořit, číst, aktualizovat, odstraňovat) operace.

> [!div class="step-by-step"]
> [Next](crud.md)
