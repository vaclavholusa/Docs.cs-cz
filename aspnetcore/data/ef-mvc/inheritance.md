---
title: "Jádro ASP.NET MVC s EF Core - dědičnosti - 9, 10"
author: tdykstra
description: "Tento kurz vám ukáže, jak implementovat dědičnosti v datovém modelu Entity Framework Core v aplikaci ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 55221846422def25452bc148b68573a02299bbfe
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a><span data-ttu-id="26dbd-103">Dědičnost – základní EF s kurz k ASP.NET MVC jádra (9, 10)</span><span class="sxs-lookup"><span data-stu-id="26dbd-103">Inheritance - EF Core with ASP.NET Core MVC tutorial (9 of 10)</span></span>

<span data-ttu-id="26dbd-104">Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="26dbd-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="26dbd-105">Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26dbd-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="26dbd-106">Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).</span><span class="sxs-lookup"><span data-stu-id="26dbd-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="26dbd-107">V tomto kurzu předchozí zpracování výjimky souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="26dbd-107">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="26dbd-108">Tento kurz vám ukáže, jak implementovat dědičnosti v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="26dbd-108">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="26dbd-109">V objektově orientované programování, můžete dědičnost usnadňuje opakované použití kódu.</span><span class="sxs-lookup"><span data-stu-id="26dbd-109">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="26dbd-110">V tomto kurzu změníte `Instructor` a `Student` tak, aby se odvozena od třídy `Person` základní třída, která obsahuje vlastnosti, jako `LastName` , které jsou společné pro vyučující a studenti.</span><span class="sxs-lookup"><span data-stu-id="26dbd-110">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="26dbd-111">Nebude přidat nebo změnit jakékoli webové stránky, ale budete měnit kód, a tyto změny se automaticky zobrazí v databázi.</span><span class="sxs-lookup"><span data-stu-id="26dbd-111">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="26dbd-112">Možnosti pro mapování dědičnosti tabulkách databáze</span><span class="sxs-lookup"><span data-stu-id="26dbd-112">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="26dbd-113">`Instructor` a `Student` třídy v datovém modelu školní mají několik vlastností, které jsou stejné:</span><span class="sxs-lookup"><span data-stu-id="26dbd-113">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Třídy Student a lektorem](inheritance/_static/no-inheritance.png)

<span data-ttu-id="26dbd-115">Předpokládejme, že chcete odstranit redundantní kód pro vlastnosti, které jsou sdíleny `Instructor` a `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="26dbd-115">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="26dbd-116">Nebo chcete napsat služba, která dokáže formátovat názvy bez caring, zda název pochází lektorem nebo student.</span><span class="sxs-lookup"><span data-stu-id="26dbd-116">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="26dbd-117">Můžete vytvořit `Person` základní třídu, která obsahuje pouze ty sdílené vlastnosti, ujistěte se, `Instructor` a `Student` třídy dědí ze základní třídy, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="26dbd-117">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Studenty a lektorem třídy odvozené od třídy osoba](inheritance/_static/inheritance.png)

<span data-ttu-id="26dbd-119">Existuje několik způsobů, které tato struktura dědičnosti může být reprezentován v databázi.</span><span class="sxs-lookup"><span data-stu-id="26dbd-119">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="26dbd-120">Můžete mít tabulku osoba, která obsahuje informace o studenty a vyučující v jediné tabulce.</span><span class="sxs-lookup"><span data-stu-id="26dbd-120">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="26dbd-121">Některé sloupce může použít pouze pro vyučující (HireDate), některé jenom pro studenty (EnrollmentDate), některé na obou (LastName, FirstName).</span><span class="sxs-lookup"><span data-stu-id="26dbd-121">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="26dbd-122">Obvykle se mají sloupce diskriminátoru k určení typů, které se každý řádek představuje.</span><span class="sxs-lookup"><span data-stu-id="26dbd-122">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="26dbd-123">Například možná sloupce diskriminátoru pro vyučující a "Student" pro studenty, "Lektorem".</span><span class="sxs-lookup"><span data-stu-id="26dbd-123">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Příklad tabulky podle hierarchie](inheritance/_static/tph.png)

<span data-ttu-id="26dbd-125">Tento vzor generování strukturu dědičnosti entity z jedné tabulky databáze se nazývá tabulky za hierarchie dědičnosti (TPH).</span><span class="sxs-lookup"><span data-stu-id="26dbd-125">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="26dbd-126">Alternativou je ke změně databáze vzhled strukturu dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="26dbd-126">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="26dbd-127">Například můžete mít jenom pole název v tabulce osoby a mít samostatné lektorem a Student tabulky s poli datum.</span><span class="sxs-lookup"><span data-stu-id="26dbd-127">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Každý typ tabulky dědičnosti](inheritance/_static/tpt.png)

<span data-ttu-id="26dbd-129">Tento vzor vytvoření databázové tabulky pro každou třídu entity nazývá za dědičnost typů (TPT).</span><span class="sxs-lookup"><span data-stu-id="26dbd-129">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="26dbd-130">Další možností je ještě mapování všechny typy neabstraktní na jednotlivé tabulky.</span><span class="sxs-lookup"><span data-stu-id="26dbd-130">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="26dbd-131">Všechny vlastnosti třídy, včetně zděděné vlastnosti, mapování na sloupce odpovídající tabulky.</span><span class="sxs-lookup"><span data-stu-id="26dbd-131">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="26dbd-132">Tento vzor nazývá dědičnost tabulky na konkrétní třídy (TPC).</span><span class="sxs-lookup"><span data-stu-id="26dbd-132">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="26dbd-133">Pokud jste implementovali TPC dědičnosti pro osoby, Student a lektorem třídy, jako je uvedené výše, vypadat Student a lektorem tabulky nejsou jiné po implementace dědičnosti než dříve.</span><span class="sxs-lookup"><span data-stu-id="26dbd-133">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="26dbd-134">TPC a TPH vzory dědičnosti obecně poskytovat lepší výkon než TPT vzory dědičnosti, protože TPT vzory může mít za následek komplexní spojení dotazy.</span><span class="sxs-lookup"><span data-stu-id="26dbd-134">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="26dbd-135">Tento kurz ukazuje, jak implementovat dědičnost TPH.</span><span class="sxs-lookup"><span data-stu-id="26dbd-135">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="26dbd-136">TPH je pouze dědičnosti vzor, který podporuje základní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="26dbd-136">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="26dbd-137">Co můžete to udělat, je vytvořit `Person` třídy, změňte `Instructor` a `Student` odvozovat ze tříd `Person`, přidejte novou třídu do `DbContext`a vytvořte migrace.</span><span class="sxs-lookup"><span data-stu-id="26dbd-137">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP] 
> <span data-ttu-id="26dbd-138">Zvažte možnost uložení kopie projektu před prováděním následujících změn.</span><span class="sxs-lookup"><span data-stu-id="26dbd-138">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="26dbd-139">Pak pokud narazíte na problémy je nutné začít znovu, je snazší spuštění z projektu uložený místo Prohodit kroky v tomto kurzu nebo má zpět na začátek celé řady.</span><span class="sxs-lookup"><span data-stu-id="26dbd-139">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="26dbd-140">Vytvořte třídu osoba</span><span class="sxs-lookup"><span data-stu-id="26dbd-140">Create the Person class</span></span>

<span data-ttu-id="26dbd-141">Ve složce modely vytvořte Person.cs a nahraďte kód šablony s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="26dbd-141">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="26dbd-142">Ujistěte se, studenty a lektorem třídy dědí osoba</span><span class="sxs-lookup"><span data-stu-id="26dbd-142">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="26dbd-143">V *Instructor.cs*, lektorem třída odvozena od třídy osoby a odeberte pole klíče a název.</span><span class="sxs-lookup"><span data-stu-id="26dbd-143">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="26dbd-144">Kód bude vypadat jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="26dbd-144">The code will look like the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="26dbd-145">Provést stejné změny v *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="26dbd-145">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="26dbd-146">Přidat typ uživatel entity do datového modelu</span><span class="sxs-lookup"><span data-stu-id="26dbd-146">Add the Person entity type to the data model</span></span>

<span data-ttu-id="26dbd-147">Přidejte typ entity osoby k *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="26dbd-147">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="26dbd-148">Jsou vyznačené nové řádky.</span><span class="sxs-lookup"><span data-stu-id="26dbd-148">The new lines are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="26dbd-149">To je vše, chcete-li nakonfigurovat tabulky za hierarchie dědičnosti vyžadující rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="26dbd-149">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="26dbd-150">Jak zjistíte, když se aktualizuje databázi, bude mít tabulku osoba místo Student a lektorem tabulky.</span><span class="sxs-lookup"><span data-stu-id="26dbd-150">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="26dbd-151">Vytvářet a přizpůsobovat migrace kódu</span><span class="sxs-lookup"><span data-stu-id="26dbd-151">Create and customize migration code</span></span>

<span data-ttu-id="26dbd-152">Uložte změny a sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="26dbd-152">Save your changes and build the project.</span></span> <span data-ttu-id="26dbd-153">Potom otevřete příkazové okno ve složce projektu a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="26dbd-153">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="26dbd-154">Nemůžete spustit `database update` ještě příkaz.</span><span class="sxs-lookup"><span data-stu-id="26dbd-154">Don't run the `database update` command yet.</span></span> <span data-ttu-id="26dbd-155">Tento příkaz povede ke ztrátě dat. protože bude odpojit tabulku lektorem a přejmenovat tabulku Student osobě.</span><span class="sxs-lookup"><span data-stu-id="26dbd-155">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="26dbd-156">Je třeba zadat vlastní kód, aby byla zachována stávající data.</span><span class="sxs-lookup"><span data-stu-id="26dbd-156">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="26dbd-157">Otevřete *migrace nebo\<časové razítko > _Inheritance.cs* a nahraďte `Up` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="26dbd-157">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="26dbd-158">Tento kód má na starosti následující úlohy aktualizace databáze:</span><span class="sxs-lookup"><span data-stu-id="26dbd-158">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="26dbd-159">Odebere omezení cizího klíče a indexy, které ukazují na Student tabulku.</span><span class="sxs-lookup"><span data-stu-id="26dbd-159">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="26dbd-160">Tabulku lektorem jako uživatel přejmenuje a provede změny, které jsou potřeba pro ni k ukládání dat Student:</span><span class="sxs-lookup"><span data-stu-id="26dbd-160">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="26dbd-161">Přidá EnrollmentDate s možnou hodnotou Null pro studenty.</span><span class="sxs-lookup"><span data-stu-id="26dbd-161">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="26dbd-162">Přidá sloupce diskriminátoru označuje, zda řádek pro student nebo lektorem.</span><span class="sxs-lookup"><span data-stu-id="26dbd-162">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="26dbd-163">Vzhledem k tomu, že řádky student nebude mít přijetím kalendářních dat, díky HireDate s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="26dbd-163">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="26dbd-164">Přidá dočasné pole, které se použije k aktualizaci cizí klíče, které odkazují na studenty.</span><span class="sxs-lookup"><span data-stu-id="26dbd-164">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="26dbd-165">Při kopírování studenty do tabulky osob získají nové hodnot primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="26dbd-165">When you copy students into the Person table they will get new primary key values.</span></span>

* <span data-ttu-id="26dbd-166">Kopíruje data z tabulky studenty do tabulky osob.</span><span class="sxs-lookup"><span data-stu-id="26dbd-166">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="26dbd-167">To způsobí, že studentů přiřazovány nové hodnot primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="26dbd-167">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="26dbd-168">Opravy hodnoty cizího klíče, které odkazují na studenty.</span><span class="sxs-lookup"><span data-stu-id="26dbd-168">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="26dbd-169">Znovu vytvoří omezení cizího klíče a indexy, nyní je odkazující na tabulky osoba.</span><span class="sxs-lookup"><span data-stu-id="26dbd-169">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="26dbd-170">(Při použití GUID místo celé číslo jako typ primární klíče, student hodnot primárního klíče nebude muset změnit a některé z těchto kroků by byly vynechány.)</span><span class="sxs-lookup"><span data-stu-id="26dbd-170">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="26dbd-171">Spustit `database update` příkaz:</span><span class="sxs-lookup"><span data-stu-id="26dbd-171">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="26dbd-172">(V produkční systému by uděláte odpovídající změny `Down` metodu v případě, kdy bylo nutné použít, se vrátíte k předchozí verzi databáze.</span><span class="sxs-lookup"><span data-stu-id="26dbd-172">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="26dbd-173">V tomto kurzu nebudete používat `Down` metoda.)</span><span class="sxs-lookup"><span data-stu-id="26dbd-173">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE] 
> <span data-ttu-id="26dbd-174">Je možné získat další chyby při provádění změn schématu v databázi, která obsahuje stávající data.</span><span class="sxs-lookup"><span data-stu-id="26dbd-174">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="26dbd-175">Pokud dojde k chybám migrace, které nelze vyřešit, můžete změnit název databáze v připojovacím řetězci nebo odstraňte tuto databázi.</span><span class="sxs-lookup"><span data-stu-id="26dbd-175">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="26dbd-176">S novou databázi nejsou žádná data k migraci a příkazu update-database je pravděpodobnější dokončeno bez chyb.</span><span class="sxs-lookup"><span data-stu-id="26dbd-176">With a new database, there's no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="26dbd-177">Pokud chcete odstranit databázi, použijte SSOX nebo spusťte `database drop` rozhraní příkazového řádku příkaz.</span><span class="sxs-lookup"><span data-stu-id="26dbd-177">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="26dbd-178">Testování s použitím dědičnosti implementována</span><span class="sxs-lookup"><span data-stu-id="26dbd-178">Test with inheritance implemented</span></span>

<span data-ttu-id="26dbd-179">Spusťte aplikaci a zkuste stránky.</span><span class="sxs-lookup"><span data-stu-id="26dbd-179">Run the app and try various pages.</span></span> <span data-ttu-id="26dbd-180">Všechno funguje stejným způsobem jako předtím.</span><span class="sxs-lookup"><span data-stu-id="26dbd-180">Everything works the same as it did before.</span></span>

<span data-ttu-id="26dbd-181">V **Průzkumník objektů systému SQL Server**, rozbalte položku **datové připojení nebo SchoolContext** a potom **tabulky**, a zjistíte, že byly nahrazeny Student a lektorem tabulky Tabulka osoby.</span><span class="sxs-lookup"><span data-stu-id="26dbd-181">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="26dbd-182">Otevřete návrháře tabulky osoby a obsahuje všechny sloupce, které používají jako Student a lektorem tabulky.</span><span class="sxs-lookup"><span data-stu-id="26dbd-182">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Osoba tabulky v SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="26dbd-184">Klikněte pravým tlačítkem na tabulky osoba a pak klikněte na **zobrazit Data tabulky** zobrazíte sloupce diskriminátoru.</span><span class="sxs-lookup"><span data-stu-id="26dbd-184">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Osoba tabulky v SSOX - data tabulky](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="26dbd-186">Souhrn</span><span class="sxs-lookup"><span data-stu-id="26dbd-186">Summary</span></span>

<span data-ttu-id="26dbd-187">Jste implementovali tabulky za hierarchie dědičnosti pro `Person`, `Student`, a `Instructor` třídy.</span><span class="sxs-lookup"><span data-stu-id="26dbd-187">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="26dbd-188">Další informace o dědičnosti v Entity Framework Core najdete v tématu [dědičnosti](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="26dbd-188">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="26dbd-189">V dalším kurzu se zobrazí, jak bude zpracováván celou řadu relativně pokročilých scénářích rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="26dbd-189">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="26dbd-190">[Předchozí](concurrency.md)
[další](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="26dbd-190">[Previous](concurrency.md)
[Next](advanced.md)</span></span>  
