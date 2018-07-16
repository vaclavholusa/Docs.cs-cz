---
title: ASP.NET Core MVC s EF Core – čtení souvisejících dat – 6 10
author: rick-anderson
description: V tomto kurzu budete čtení a zobrazení souvisejících dat – to znamená, že data, která načte Entity Framework do navigační vlastnosti.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: a310c9e4b9cec6e2ab2477461f395c9bbd3fa364
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063283"
---
# <a name="aspnet-core-mvc-with-ef-core---read-related-data---6-of-10"></a><span data-ttu-id="debae-103">ASP.NET Core MVC s EF Core – čtení souvisejících dat – 6 10</span><span class="sxs-lookup"><span data-stu-id="debae-103">ASP.NET Core MVC with EF Core - Read Related Data - 6 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="debae-104">Podle [Petr Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="debae-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="debae-105">Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET Core MVC pomocí Entity Framework Core a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="debae-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="debae-106">Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="debae-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="debae-107">V předchozím kurzu jste dokončili školní datového modelu.</span><span class="sxs-lookup"><span data-stu-id="debae-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="debae-108">V tomto kurzu budete čtení a zobrazení souvisejících dat – to znamená, že data, která načte Entity Framework do navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="debae-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="debae-109">Na následujících obrázcích stránky, kterou budete pracovat.</span><span class="sxs-lookup"><span data-stu-id="debae-109">The following illustrations show the pages that you'll work with.</span></span>

![Kurzy indexová stránka](read-related-data/_static/courses-index.png)

![Instruktoři indexová stránka](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="debae-112">Nemůžou dočkat, až explicitní a opožděné načtení souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="debae-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="debae-113">Existuje několik způsobů objektově-relační mapování (ORM) určené softwaru, jako je rozhraní Entity Framework mohou načíst související data do navigační vlastnosti entity:</span><span class="sxs-lookup"><span data-stu-id="debae-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="debae-114">Předběžné načítání.</span><span class="sxs-lookup"><span data-stu-id="debae-114">Eager loading.</span></span> <span data-ttu-id="debae-115">Při čtení entity související data načíst společně.</span><span class="sxs-lookup"><span data-stu-id="debae-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="debae-116">Obvykle v důsledku jednoho spojení dotaz, který zkopíruje všechna data, který je nezbytný.</span><span class="sxs-lookup"><span data-stu-id="debae-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="debae-117">Předběžné načítání zadáte v Entity Framework Core pomocí `Include` a `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="debae-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![Předběžné načítání příklad](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="debae-119">Můžete načíst některá data v samostatné dotazy a EF termín "opravy" navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="debae-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="debae-120">To znamená EF automaticky přidá samostatně načtený entity, které patří, kde ve vlastnosti navigace entit dříve načtená.</span><span class="sxs-lookup"><span data-stu-id="debae-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="debae-121">Pro dotaz, který načte související data, můžete použít `Load` metoda místo metodu, která vrátí seznam nebo objekt, jako například `ToList` nebo `Single`.</span><span class="sxs-lookup"><span data-stu-id="debae-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Příklad samostatné dotazy](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="debae-123">Explicitní načtení.</span><span class="sxs-lookup"><span data-stu-id="debae-123">Explicit loading.</span></span> <span data-ttu-id="debae-124">Pokud entita je nejdřív přečíst, související data nebude načten.</span><span class="sxs-lookup"><span data-stu-id="debae-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="debae-125">Můžete napsat kód, který načte související data, pokud je to potřeba.</span><span class="sxs-lookup"><span data-stu-id="debae-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="debae-126">Stejně jako v případě eager načítání s samostatné dotazy explicitní načítání více dotazů za následek odeslán do databáze.</span><span class="sxs-lookup"><span data-stu-id="debae-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="debae-127">Rozdíl je, že s explicitní načtení, určuje kód navigačních vlastností, které mají být načteny.</span><span class="sxs-lookup"><span data-stu-id="debae-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="debae-128">V Entity Framework Core 1.1 můžete použít `Load` metodu explicitní načtení.</span><span class="sxs-lookup"><span data-stu-id="debae-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="debae-129">Příklad:</span><span class="sxs-lookup"><span data-stu-id="debae-129">For example:</span></span>

  ![Příklad explicitní načtení](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="debae-131">Opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="debae-131">Lazy loading.</span></span> <span data-ttu-id="debae-132">Pokud entita je nejdřív přečíst, související data nebude načten.</span><span class="sxs-lookup"><span data-stu-id="debae-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="debae-133">Ale při prvním pokusu o přístup k vlastnosti navigace data požadovaná pro tuto navigační vlastnost je automaticky načte.</span><span class="sxs-lookup"><span data-stu-id="debae-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="debae-134">Bude odeslán dotaz do databáze při každém pokusu o získání dat z navigační vlastnost poprvé.</span><span class="sxs-lookup"><span data-stu-id="debae-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="debae-135">Entity Framework Core 1.0 nepodporuje opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="debae-135">Entity Framework Core 1.0 doesn't support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="debae-136">Důležité informace o výkonu</span><span class="sxs-lookup"><span data-stu-id="debae-136">Performance considerations</span></span>

<span data-ttu-id="debae-137">Pokud víte, že budete potřebovat pro každou entitu načíst související data, nemůžou dočkat, až načítání často nabízí nejlepší výkon, protože jednoho dotazu odeslaného do databáze je obvykle mnohem efektivnější než samostatné dotazy pro každou entitu načíst.</span><span class="sxs-lookup"><span data-stu-id="debae-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="debae-138">Předpokládejme například, že každé oddělení má deset související kurzy.</span><span class="sxs-lookup"><span data-stu-id="debae-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="debae-139">Předběžné načítání všechna související data výsledkem budou pouze jednou (spojení) dotazu a jedinou odezvy k databázi.</span><span class="sxs-lookup"><span data-stu-id="debae-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="debae-140">Samostatný dotaz kurzy pro každé oddělení způsobí jedenáct zpátečních cest k databázi.</span><span class="sxs-lookup"><span data-stu-id="debae-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="debae-141">Další zpátečních cest k databázi jsou zvlášť ztráty výkonu, když má vysokou latenci.</span><span class="sxs-lookup"><span data-stu-id="debae-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="debae-142">Na druhé straně v některých případech je efektivnější samostatné dotazy.</span><span class="sxs-lookup"><span data-stu-id="debae-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="debae-143">Předběžné načítání všechna související data v jednom dotazu může vést k velmi složité spojení být vytvořen, kterou SQL Server nemůže zpracovat efektivně.</span><span class="sxs-lookup"><span data-stu-id="debae-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="debae-144">Nebo pokud potřebujete přístup k entity navigační vlastnosti pouze pro dílčí sadu entit, které jste zpracování, samostatné dotazy může lépe provést, protože nemůžou dočkat, až načítání všechno, co ještě před zahájením by byl načten více dat, než potřebujete.</span><span class="sxs-lookup"><span data-stu-id="debae-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="debae-145">Pokud je nejdůležitější výkon, je nejvhodnější pro testování výkonu oba způsoby, aby bylo možné správně se rozhodnout.</span><span class="sxs-lookup"><span data-stu-id="debae-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="debae-146">Vytvoření stránky kurzů, která zobrazuje název oddělení</span><span class="sxs-lookup"><span data-stu-id="debae-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="debae-147">Kurz entita obsahuje navigační vlastnost obsahující tuto entitu oddělení oddělení, která je přiřazena kurzu.</span><span class="sxs-lookup"><span data-stu-id="debae-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="debae-148">Chcete-li zobrazit název přiřazený oddělení v seznamu kurzů, je potřeba získat název vlastnosti z oddělení entity, která je v `Course.Department` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="debae-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that's in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="debae-149">Vytvořit řadič CoursesController pro typ entity kurz pomocí stejných možností pro **kontroler MVC se zobrazeními, s použitím Entity Framework** generátor, který jste provedli dříve pro studenty řadič, jak je znázorněno Následující obrázek:</span><span class="sxs-lookup"><span data-stu-id="debae-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Přidat kontroler kurzy](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="debae-151">Otevřít *CoursesController.cs* a prozkoumejte `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="debae-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="debae-152">Automatické generování uživatelského rozhraní má zadanou předběžné načítání pro `Department` navigační vlastnost s použitím `Include` metody.</span><span class="sxs-lookup"><span data-stu-id="debae-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="debae-153">Nahradit `Index` metodu s následujícím kódem, který používá více vhodný název pro `IQueryable` , který vrací kurz entity (`courses` místo `schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="debae-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="debae-154">Otevřít *Views/Courses/Index.cshtml* a nahraďte kód šablony následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="debae-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="debae-155">Změny jsou zvýrazněny:</span><span class="sxs-lookup"><span data-stu-id="debae-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="debae-156">Automaticky generovaný kód, které jste udělali následující změny:</span><span class="sxs-lookup"><span data-stu-id="debae-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="debae-157">Změnit záhlaví od indexu ke kurzům.</span><span class="sxs-lookup"><span data-stu-id="debae-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="debae-158">Přidá **číslo** sloupec, který ukazuje `CourseID` hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="debae-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="debae-159">Ve výchozím nastavení nejsou automaticky generovaný primární klíče, protože jsou obvykle nemá význam pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="debae-159">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="debae-160">Ale v tomto případě má smysl primární klíč a chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="debae-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="debae-161">Změnit **oddělení** sloupec, který se zobrazí název oddělení.</span><span class="sxs-lookup"><span data-stu-id="debae-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="debae-162">Zobrazení kódu `Name` vlastnost entity oddělení, který je načten do `Department` navigační vlastnost:</span><span class="sxs-lookup"><span data-stu-id="debae-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="debae-163">Spusťte aplikaci a vyberte **kurzy** kartu pro zobrazení seznamu s názvy oddělení.</span><span class="sxs-lookup"><span data-stu-id="debae-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kurzy indexová stránka](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="debae-165">Vytvoření stránky školitelů, který ukazuje, kurzy a registrace</span><span class="sxs-lookup"><span data-stu-id="debae-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="debae-166">V této části vytvoříte kontroler a zobrazení entity kurzů vedených mohla zobrazit na stránce Instruktoři:</span><span class="sxs-lookup"><span data-stu-id="debae-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Instruktoři indexová stránka](read-related-data/_static/instructors-index.png)

<span data-ttu-id="debae-168">Tato stránka načte a zobrazí související data následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="debae-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="debae-169">Seznam Instruktoři zobrazí související data z OfficeAssignment entity.</span><span class="sxs-lookup"><span data-stu-id="debae-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="debae-170">Entity instruktorem a OfficeAssignment jsou ve vztahu k nule nebo jednom.</span><span class="sxs-lookup"><span data-stu-id="debae-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="debae-171">Předběžné načítání budete používat pro OfficeAssignment entity.</span><span class="sxs-lookup"><span data-stu-id="debae-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="debae-172">Jak jsme vysvětlili výše, předběžné načítání je obvykle mnohem efektivnější, když potřebujete související data pro všechny načtené řádky v tabulce primární.</span><span class="sxs-lookup"><span data-stu-id="debae-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="debae-173">V takovém případě budete chtít zobrazit přiřazení office pro všechny zobrazené Instruktoři.</span><span class="sxs-lookup"><span data-stu-id="debae-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="debae-174">Když uživatel vybere instruktorem, se zobrazí související entity kurzu.</span><span class="sxs-lookup"><span data-stu-id="debae-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="debae-175">Entity instruktorem a kurzu jsou v relaci m: m.</span><span class="sxs-lookup"><span data-stu-id="debae-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="debae-176">Předběžné načítání budete používat pro kurz entit a jejich souvisejících entit oddělení.</span><span class="sxs-lookup"><span data-stu-id="debae-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="debae-177">Samostatné dotazy v tomto případě může být mnohem efektivnější, protože potřebujete jenom pro vybrané kurzů vedených kurzů.</span><span class="sxs-lookup"><span data-stu-id="debae-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="debae-178">Však tento příklad ukazuje způsob použití předběžné načítání pro navigační vlastnosti v rámci entity, které představují samy o sobě v navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="debae-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="debae-179">Když uživatel vybere kurzu, zobrazí se související data ze sady entit registrace.</span><span class="sxs-lookup"><span data-stu-id="debae-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="debae-180">Kurz a registraci entity jsou v vztah jeden mnoho.</span><span class="sxs-lookup"><span data-stu-id="debae-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="debae-181">Pro registraci a jejich souvisejících entit Student použijete samostatné dotazy.</span><span class="sxs-lookup"><span data-stu-id="debae-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="debae-182">Vytvoření modelu zobrazení pro zobrazení indexu instruktorem</span><span class="sxs-lookup"><span data-stu-id="debae-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="debae-183">Na stránce Instruktoři zobrazuje data ze tří různých tabulek.</span><span class="sxs-lookup"><span data-stu-id="debae-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="debae-184">Proto vytvoříte zobrazení modelu, který obsahuje tři vlastnosti každé obsahující data pro jednu z tabulek.</span><span class="sxs-lookup"><span data-stu-id="debae-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="debae-185">V *SchoolViewModels* složku, vytvořte *InstructorIndexData.cs* a nahraďte existující kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="debae-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="debae-186">Vytvoření kontroleru instruktorem a zobrazení</span><span class="sxs-lookup"><span data-stu-id="debae-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="debae-187">Vytvoření řadiče Instruktoři s akcemi čtení/zápisu EF, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="debae-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Přidat kontroler Instruktoři](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="debae-189">Otevřít *InstructorsController.cs* a přidejte sadu pomocí příkazu pro modely ViewModels obor názvů:</span><span class="sxs-lookup"><span data-stu-id="debae-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="debae-190">Metoda Index nahraďte následující kód, který se nemůžou dočkat, až načítání souvisejících dat a vložit ho do modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="debae-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="debae-191">Metoda přijímá data volitelné trasy (`id`) a parametru řetězce dotazu (`courseID`), které poskytují hodnoty ID vybrané instruktorem a vybrané kurzu.</span><span class="sxs-lookup"><span data-stu-id="debae-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="debae-192">Parametry jsou poskytovány **vyberte** hypertextové odkazy na stránky.</span><span class="sxs-lookup"><span data-stu-id="debae-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="debae-193">Vytváření instance zobrazení modelu a jeho uvedením seznamu instruktorů začíná kód.</span><span class="sxs-lookup"><span data-stu-id="debae-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="debae-194">Předběžné načítání pro Určuje kód `Instructor.OfficeAssignment` a `Instructor.CourseAssignments` navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="debae-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="debae-195">V rámci `CourseAssignments` vlastnost, `Course` vlastnosti je načtena a v rámci, která je `Enrollments` a `Department` vlastnosti jsou načtena a v každé `Enrollment` entity `Student` načtena vlastnost.</span><span class="sxs-lookup"><span data-stu-id="debae-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="debae-196">Protože zobrazení vždy vyžaduje OfficeAssignment entity, je efektivnější pro načtení, které ve stejném dotazu.</span><span class="sxs-lookup"><span data-stu-id="debae-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="debae-197">Kurz entity jsou požadovány při výběru instruktorem na webové stránce, takže pomocí jediného dotazu je lepší než více dotazů jenom v případě, že na stránce se zobrazí častěji kurzu vybrali než bez.</span><span class="sxs-lookup"><span data-stu-id="debae-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="debae-198">Kód se opakuje `CourseAssignments` a `Course` vzhledem k tomu, že budete potřebovat dvě vlastnosti z `Course`.</span><span class="sxs-lookup"><span data-stu-id="debae-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="debae-199">První řetězec `ThenInclude` volá získá `CourseAssignment.Course`, `Course.Enrollments`, a `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="debae-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="debae-200">V tomto bodě v kódu jiné `ThenInclude` budou platit pro navigační vlastnosti `Student`, které nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="debae-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="debae-201">Ale volání `Include` začne přehrávat znovu, s `Instructor` vlastnosti, takže budete muset projít řetězu znovu tento čas zadání `Course.Department` místo `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="debae-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="debae-202">Následující kód provede při instruktorem byla vybrána.</span><span class="sxs-lookup"><span data-stu-id="debae-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="debae-203">Vybrané kurzů vedených je načten ze seznamu školitelů v modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="debae-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="debae-204">Model zobrazení `Courses` vlastnost je pak načten pomocí kurzu entity z této kurzů vedených `CourseAssignments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="debae-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="debae-205">`Where` Metoda vrátí kolekci, ale v tomto případě kritéria předat výsledek metody v pouze jednu entitu kurzů vedených se vrací.</span><span class="sxs-lookup"><span data-stu-id="debae-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="debae-206">`Single` Metoda převede kolekci na jednu entitu instruktorem, která umožňuje přístup k dané entity `CourseAssignments` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="debae-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="debae-207">`CourseAssignments` Obsahuje vlastnost `CourseAssignment` entity, z nichž chcete pouze související `Course` entity.</span><span class="sxs-lookup"><span data-stu-id="debae-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="debae-208">Můžete použít `Single` metody na kolekci, když víte, kolekce budou mít pouze jednu položku.</span><span class="sxs-lookup"><span data-stu-id="debae-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="debae-209">Jedinou metodu vyvolá výjimku, pokud do něho předaný kolekce je prázdná, nebo pokud existuje více než jednu položku.</span><span class="sxs-lookup"><span data-stu-id="debae-209">The Single method throws an exception if the collection passed to it's empty or if there's more than one item.</span></span> <span data-ttu-id="debae-210">Alternativou je `SingleOrDefault`, která vrátí výchozí hodnoty (null v tomto případě) Pokud kolekce je prázdná.</span><span class="sxs-lookup"><span data-stu-id="debae-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="debae-211">Ale v tomto případě bude stále výsledkem výjimku (z pokusu o nalezení `Courses` vlastnost na hodnotu Null), a zpráva o výjimce by méně jasně ukazovat na příčinu problému.</span><span class="sxs-lookup"><span data-stu-id="debae-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="debae-212">Při volání `Single` metodu, můžete také předat Where podmínku namísto volání metody `Where` metoda samostatně:</span><span class="sxs-lookup"><span data-stu-id="debae-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="debae-213">Namísto:</span><span class="sxs-lookup"><span data-stu-id="debae-213">Instead of:</span></span>

```csharp
.Where(i => i.ID == id.Value).Single()
```

<span data-ttu-id="debae-214">Dále pokud jste vybrali kurz, vybrané kurzu se načte z seznam kurzů v zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="debae-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="debae-215">Potom zobrazení modelu `Enrollments` s registrací entity z tohoto kurzu je načtena vlastnost `Enrollments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="debae-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="debae-216">Úprava zobrazení indexu instruktorem</span><span class="sxs-lookup"><span data-stu-id="debae-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="debae-217">V *Views/Instructors/Index.cshtml*, nahraďte kód šablony následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="debae-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="debae-218">Změny jsou zvýrazněné.</span><span class="sxs-lookup"><span data-stu-id="debae-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="debae-219">Stávající kód, které jste udělali následující změny:</span><span class="sxs-lookup"><span data-stu-id="debae-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="debae-220">Změnit třídu modelu `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="debae-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="debae-221">Změnil se název stránky z **Index** k **Instruktoři**.</span><span class="sxs-lookup"><span data-stu-id="debae-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="debae-222">Přidá **Office** sloupec, který zobrazuje `item.OfficeAssignment.Location` pouze tehdy, pokud `item.OfficeAssignment` není null.</span><span class="sxs-lookup"><span data-stu-id="debae-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="debae-223">(Protože je to vztah jeden: nula nebo 1, nemusí být související entita OfficeAssignment.)</span><span class="sxs-lookup"><span data-stu-id="debae-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="debae-224">Přidá **kurzy** sloupec, který zobrazuje kurzy vedené jednotlivých kurzů vedených.</span><span class="sxs-lookup"><span data-stu-id="debae-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="debae-225">Zobrazit [explicitní přechod na řádek s `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) Další informace o tuto syntaxi razor.</span><span class="sxs-lookup"><span data-stu-id="debae-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="debae-226">Přidání kódu, která dynamicky přidá `class="success"` k `tr` element vybrané instruktorem.</span><span class="sxs-lookup"><span data-stu-id="debae-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="debae-227">Tím se nastaví barvu pozadí vybraného řádku pomocí Bootstrap třídy.</span><span class="sxs-lookup"><span data-stu-id="debae-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="debae-228">Přidat nový popisek hypertextový odkaz **vyberte** bezprostředně před další odkazy na každém řádku, což způsobí, že ID vybraných kurzů vedených k odeslání do `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="debae-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="debae-229">Spusťte aplikaci a vyberte **Instruktoři** kartu. Na stránce zobrazí vlastnost umístění související entity OfficeAssignment a na prázdné tabulce buňku, když není žádná související entita OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="debae-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Instruktoři indexovou stránku, kterou nevybere](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="debae-231">V *Views/Instructors/Index.cshtml* soubor po zavření tabulky prvku (na konci souboru), přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="debae-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="debae-232">Tento kód zobrazí seznam kurzů související s instruktorem, pokud je vybrána instruktorem.</span><span class="sxs-lookup"><span data-stu-id="debae-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="debae-233">Tento kód čte `Courses` vlastnost model zobrazení zobrazíte seznam kurzů.</span><span class="sxs-lookup"><span data-stu-id="debae-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="debae-234">Poskytuje také **vyberte** hypertextový odkaz, který odesílá ID vybrané kurz `Index` metody akce.</span><span class="sxs-lookup"><span data-stu-id="debae-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="debae-235">Aktualizujte stránku a vybrat instruktorem.</span><span class="sxs-lookup"><span data-stu-id="debae-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="debae-236">Nyní uvidíte tabulku, která zobrazuje kurzy přiřazen k vybrané instruktorem a jednotlivých kurzů se zobrazí název přiřazený oddělení.</span><span class="sxs-lookup"><span data-stu-id="debae-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Instruktoři Index stránky kurzů vedených vybrané](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="debae-238">Za blok kódu, který jste právě přidali přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="debae-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="debae-239">Zobrazí seznam studentů, kteří se zaregistrují v kurzu při výběru tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="debae-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="debae-240">Tento kód načte vlastnost registrace modelu zobrazení zobrazíte seznam studentů zaregistrovaný do kurzu.</span><span class="sxs-lookup"><span data-stu-id="debae-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="debae-241">Znovu aktualizujte stránku a vybrat instruktorem.</span><span class="sxs-lookup"><span data-stu-id="debae-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="debae-242">Vyberte kurzu zobrazíte seznam registrovaná studentů a jejich kvality.</span><span class="sxs-lookup"><span data-stu-id="debae-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![Kurzů vedených instruktory Index stránky a vybrali kurzu](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="debae-244">Explicitní načtení</span><span class="sxs-lookup"><span data-stu-id="debae-244">Explicit loading</span></span>

<span data-ttu-id="debae-245">Když jste získali seznam školitelů v *InstructorsController.cs*, jste zadali pro předběžné načítání `CourseAssignments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="debae-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="debae-246">Předpokládejme, že jste očekávali uživatelům jen zřídka byste rádi viděli registrací ve vybrané instruktorem a kurzu.</span><span class="sxs-lookup"><span data-stu-id="debae-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="debae-247">V takovém případě můžete chtít načíst data registrací. pouze v případě, že je požadováno.</span><span class="sxs-lookup"><span data-stu-id="debae-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="debae-248">Chcete-li zobrazit příklad toho, jak provést explicitní načtení, nahraďte `Index` metody následující kód, který odebere předběžné načítání pro registraci a načte vlastnosti explicitně.</span><span class="sxs-lookup"><span data-stu-id="debae-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="debae-249">Změny kódu jsou zvýrazněné.</span><span class="sxs-lookup"><span data-stu-id="debae-249">The code changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="debae-250">Nový kód klesne *ThenInclude* metoda se volá pro zápis dat z kódu, která načte entity instruktorem.</span><span class="sxs-lookup"><span data-stu-id="debae-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="debae-251">Pokud se vybere instruktorem a kurz, zvýrazněný kód načte registrace pro vybraný kurz a Student entit pro každou registrace.</span><span class="sxs-lookup"><span data-stu-id="debae-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="debae-252">Spustit aplikaci, přejděte na stránku Instruktoři Index a zobrazí obsah zobrazený na stránce, nijak neliší, i když jste změnili, jak načíst data.</span><span class="sxs-lookup"><span data-stu-id="debae-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="debae-253">Souhrn</span><span class="sxs-lookup"><span data-stu-id="debae-253">Summary</span></span>

<span data-ttu-id="debae-254">Teď používáte předběžné načítání jednoho dotazu a s více dotazy ke čtení souvisejících dat do navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="debae-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="debae-255">V dalším kurzu dozvíte, jak aktualizovat související data.</span><span class="sxs-lookup"><span data-stu-id="debae-255">In the next tutorial you'll learn how to update related data.</span></span>

::: moniker-end

>[!div class="step-by-step"]
><span data-ttu-id="debae-256">[Předchozí](complex-data-model.md)
>[další](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="debae-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>
