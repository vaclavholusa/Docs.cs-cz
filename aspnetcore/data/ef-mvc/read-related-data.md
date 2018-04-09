---
title: Jádro ASP.NET MVC s EF Core - číst související Data - 6 10
author: tdykstra
description: V tomto kurzu budete číst a zobrazení souvisejících dat – to znamená, data, která rozhraní Entity Framework se načte do navigační vlastnosti.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 6ee4b0db5bf4d1781ce44f1aff8331680ca8686c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="aspnet-core-mvc-with-ef-core---read-related-data---6-of-10"></a><span data-ttu-id="18013-103">Jádro ASP.NET MVC s EF Core - číst související Data - 6 10</span><span class="sxs-lookup"><span data-stu-id="18013-103">ASP.NET Core MVC with EF Core - Read Related Data - 6 of 10</span></span>

<span data-ttu-id="18013-104">Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="18013-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="18013-105">Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="18013-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="18013-106">Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).</span><span class="sxs-lookup"><span data-stu-id="18013-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="18013-107">V tomto kurzu předchozí dokončit školní datový model.</span><span class="sxs-lookup"><span data-stu-id="18013-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="18013-108">V tomto kurzu budete pro čtení a zobrazení souvisejících dat – to znamená, data, která rozhraní Entity Framework se načte do navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="18013-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="18013-109">Následující ilustrace znázorňuje stránky, že bude fungovat s.</span><span class="sxs-lookup"><span data-stu-id="18013-109">The following illustrations show the pages that you'll work with.</span></span>

![Kurzy indexovou stránku](read-related-data/_static/courses-index.png)

![Index stránky vyučující](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="18013-112">Přes, explicitní a opožděného načítání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="18013-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="18013-113">Existuje několik způsobů softwaru relační objekt mapování (ORM), jako je rozhraní Entity Framework můžete načíst související data do navigační vlastnosti entity:</span><span class="sxs-lookup"><span data-stu-id="18013-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="18013-114">Přes načítání.</span><span class="sxs-lookup"><span data-stu-id="18013-114">Eager loading.</span></span> <span data-ttu-id="18013-115">Při čtení je entita, související data načtena společně s jeho.</span><span class="sxs-lookup"><span data-stu-id="18013-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="18013-116">To obvykle vede jednoho připojení dotaz, který načte všechna data, která je potřeba.</span><span class="sxs-lookup"><span data-stu-id="18013-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="18013-117">Zadejte přes načítání v Entity Framework Core pomocí `Include` a `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="18013-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![Příklad přes načítání](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="18013-119">Můžete načíst některá data v samostatné dotazy a EF "oprav" navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="18013-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="18013-120">To znamená EF automaticky přidá samostatně načtené entit, které patří ve vlastnosti navigace entit. dříve načteny.</span><span class="sxs-lookup"><span data-stu-id="18013-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="18013-121">Pro dotaz, který načte související data, můžete použít `Load` metody místo metodu, která vrátí seznam nebo objekt, jako například `ToList` nebo `Single`.</span><span class="sxs-lookup"><span data-stu-id="18013-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Příklad samostatné dotazy](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="18013-123">Explicitní načítání.</span><span class="sxs-lookup"><span data-stu-id="18013-123">Explicit loading.</span></span> <span data-ttu-id="18013-124">Když je nejdřív přečíst entity, není načíst související data.</span><span class="sxs-lookup"><span data-stu-id="18013-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="18013-125">Můžete napsat kód, který načte související data, pokud je to potřeba.</span><span class="sxs-lookup"><span data-stu-id="18013-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="18013-126">Jako v případě eager načítání s samostatné dotazy explicitní načítání více dotazů má za následek odeslal do databáze.</span><span class="sxs-lookup"><span data-stu-id="18013-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="18013-127">Rozdíl je, s explicitní načítání, určuje kód navigační vlastnosti, které mají být načtena.</span><span class="sxs-lookup"><span data-stu-id="18013-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="18013-128">V Entity Framework Core 1.1 můžete použít `Load` metoda udělat explicitní načítání.</span><span class="sxs-lookup"><span data-stu-id="18013-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="18013-129">Příklad:</span><span class="sxs-lookup"><span data-stu-id="18013-129">For example:</span></span>

  ![Příklad explicitní načítání](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="18013-131">Opožděného načítání.</span><span class="sxs-lookup"><span data-stu-id="18013-131">Lazy loading.</span></span> <span data-ttu-id="18013-132">Když je nejdřív přečíst entity, není načíst související data.</span><span class="sxs-lookup"><span data-stu-id="18013-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="18013-133">Ale při prvním pokusu o přístup k navigační vlastnost, lze data potřebná pro tuto navigační vlastnost je automaticky načte.</span><span class="sxs-lookup"><span data-stu-id="18013-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="18013-134">Dotaz je odeslal do databáze při každém pokusu o získání dat z navigační vlastnosti pro první.</span><span class="sxs-lookup"><span data-stu-id="18013-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="18013-135">Základní Entity Framework 1.0 nepodporuje opožděného načítání.</span><span class="sxs-lookup"><span data-stu-id="18013-135">Entity Framework Core 1.0 doesn't support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="18013-136">Důležité informace o výkonu</span><span class="sxs-lookup"><span data-stu-id="18013-136">Performance considerations</span></span>

<span data-ttu-id="18013-137">Pokud víte, že potřebujete souvisejících dat pro každou entitu, načíst, přes načítání často nabízí nejlepší výkon, protože je obvykle efektivnější než samostatné dotazy pro každé entity načíst jediný dotaz odeslal do databáze.</span><span class="sxs-lookup"><span data-stu-id="18013-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="18013-138">Předpokládejme například, že každé oddělení má deset související kurzy.</span><span class="sxs-lookup"><span data-stu-id="18013-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="18013-139">Přes načítání všechna související data by způsobilo právě dotazu jedním (připojit) a jedinou odezvy do databáze.</span><span class="sxs-lookup"><span data-stu-id="18013-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="18013-140">Samostatné dotazu pro kurzy pro každé oddělení by způsobilo 11 zpátečních cest k databázi.</span><span class="sxs-lookup"><span data-stu-id="18013-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="18013-141">Navíc zpátečních cest k databázi jsou obzvláště škodlivé pro výkon při latence je vysoká.</span><span class="sxs-lookup"><span data-stu-id="18013-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="18013-142">Na druhé straně v některých případech je efektivnější samostatné dotazy.</span><span class="sxs-lookup"><span data-stu-id="18013-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="18013-143">Přes načítání všechna související data v jednom dotazu může způsobit velmi složité spojení má být vygenerován, který SQL Server nemůže zpracovat efektivně.</span><span class="sxs-lookup"><span data-stu-id="18013-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="18013-144">Nebo pokud potřebujete pro přístup k navigační vlastnosti entity jenom pro podmnožinu sady entit, které jste zpracování, samostatné dotazy může lépe provést, protože by přes načítání všechno předem načíst více dat, než budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="18013-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="18013-145">Pokud je důležité výkon, je nejvhodnější pro testování výkonu obou směrech, aby bylo možné nejlepší volbou.</span><span class="sxs-lookup"><span data-stu-id="18013-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="18013-146">Vytvořte stránku kurzy, která zobrazí název oddělení</span><span class="sxs-lookup"><span data-stu-id="18013-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="18013-147">Entity kurzu zahrnuje navigační vlastnost, která obsahuje entitu oddělení přiřazený během oddělení.</span><span class="sxs-lookup"><span data-stu-id="18013-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="18013-148">Zobrazí se název přiřazené oddělení v seznamu kurzů, potřebujete získat vlastnosti Name z oddělení entity, která je v `Course.Department` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="18013-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that's in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="18013-149">Vytvoříte řadič s názvem CoursesController pro typ entity kurzu, pomocí stejných možností pro **kontroler MVC se zobrazeními s využitím nástroje Entity Framework** scaffolder, který jste dříve pro studenty řadič, jak je znázorněno Následující obrázek:</span><span class="sxs-lookup"><span data-stu-id="18013-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Přidat řadič kurzy](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="18013-151">Otevřete *CoursesController.cs* a prozkoumat `Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="18013-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="18013-152">Automatické generování uživatelského rozhraní má zadanou přes načítání pro `Department` navigační vlastnost s použitím `Include` metoda.</span><span class="sxs-lookup"><span data-stu-id="18013-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="18013-153">Nahraďte `Index` metoda s následující kód, který používá vhodnější název `IQueryable` kurzu entity, který vrací (`courses` místo `schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="18013-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="18013-154">Otevřete *Views/Courses/Index.cshtml* a nahraďte kód šablony s následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="18013-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="18013-155">Změny se zvýrazněnou:</span><span class="sxs-lookup"><span data-stu-id="18013-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="18013-156">Automaticky generovaný kód, které jste udělali následující změny:</span><span class="sxs-lookup"><span data-stu-id="18013-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="18013-157">Změnit záhlaví od indexu na kurzy.</span><span class="sxs-lookup"><span data-stu-id="18013-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="18013-158">Přidat **číslo** sloupec, který ukazuje `CourseID` hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="18013-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="18013-159">Ve výchozím nastavení nejsou vygeneroval primární klíče, protože jsou obvykle smysl pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="18013-159">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="18013-160">Ale v takovém případě má smysl primární klíč a chcete ji zobrazit.</span><span class="sxs-lookup"><span data-stu-id="18013-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="18013-161">Změnit **oddělení** sloupec, který se zobrazí název oddělení.</span><span class="sxs-lookup"><span data-stu-id="18013-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="18013-162">Kód zobrazí `Name` vlastnost oddělení entity, který je načten do `Department` navigační vlastnost:</span><span class="sxs-lookup"><span data-stu-id="18013-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="18013-163">Spusťte aplikaci a vyberte **kurzy** karty zobrazíte seznam s názvy oddělení.</span><span class="sxs-lookup"><span data-stu-id="18013-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kurzy indexovou stránku](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="18013-165">Vytvořit stránku vyučující, která zobrazuje kurzy a registrace</span><span class="sxs-lookup"><span data-stu-id="18013-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="18013-166">V této části vytvoříte řadič a zobrazení pro entitu lektorem aby bylo možné zobrazit stránku vyučující:</span><span class="sxs-lookup"><span data-stu-id="18013-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Index stránky vyučující](read-related-data/_static/instructors-index.png)

<span data-ttu-id="18013-168">Tato stránka načte a zobrazí související data následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="18013-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="18013-169">Zobrazí seznam vyučující související data ze OfficeAssignment entity.</span><span class="sxs-lookup"><span data-stu-id="18013-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="18013-170">Lektorem a OfficeAssignment entity jsou ve vztahu-nula nebo 1.</span><span class="sxs-lookup"><span data-stu-id="18013-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="18013-171">Přes načítání budete používat pro OfficeAssignment entity.</span><span class="sxs-lookup"><span data-stu-id="18013-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="18013-172">Jak je popsáno výše, je obvykle efektivnější přes načítání, když potřebujete související data pro všechny načtené řádky v primární tabulce.</span><span class="sxs-lookup"><span data-stu-id="18013-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="18013-173">V takovém případě budete chtít zobrazit přiřazení office pro všechny zobrazené vyučující.</span><span class="sxs-lookup"><span data-stu-id="18013-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="18013-174">Když uživatel vybere lektorem, zobrazí se během entit v relaci.</span><span class="sxs-lookup"><span data-stu-id="18013-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="18013-175">Lektorem a kurzu entity jsou v relaci m: n.</span><span class="sxs-lookup"><span data-stu-id="18013-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="18013-176">Přes načítání budete používat pro kurzu entity a jejich oddělení entit v relaci.</span><span class="sxs-lookup"><span data-stu-id="18013-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="18013-177">Samostatné dotazy v tomto případě může být efektivnější, protože potřebujete jenom pro vybrané lektorem kurzy.</span><span class="sxs-lookup"><span data-stu-id="18013-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="18013-178">Však tento příklad ukazuje způsob použití přes načítání pro navigační vlastnosti v rámci entit, které samy navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="18013-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="18013-179">Když uživatel vybere kurzu, zobrazí se související data ze sady entit registrace.</span><span class="sxs-lookup"><span data-stu-id="18013-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="18013-180">Průběh a registrace entity jsou vztah jeden mnoho.</span><span class="sxs-lookup"><span data-stu-id="18013-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="18013-181">Samostatné dotazy budete používat pro registraci a jejich související Student entit.</span><span class="sxs-lookup"><span data-stu-id="18013-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="18013-182">Vytvoření modelu zobrazení pro Index lektorem zobrazení</span><span class="sxs-lookup"><span data-stu-id="18013-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="18013-183">Stránka vyučující zobrazuje data ze tří různých tabulek.</span><span class="sxs-lookup"><span data-stu-id="18013-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="18013-184">Proto vytvoříte zobrazení model, který zahrnuje tři vlastnosti každého která uchovává data pro jednu z tabulek.</span><span class="sxs-lookup"><span data-stu-id="18013-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="18013-185">V *SchoolViewModels* složku vytvořit *InstructorIndexData.cs* a existujícího kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="18013-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="18013-186">Vytvoření řadiče lektorem a zobrazení</span><span class="sxs-lookup"><span data-stu-id="18013-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="18013-187">Vytvoření řadiče vyučující s akcemi čtení/zápisu EF, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="18013-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Přidat řadič vyučující](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="18013-189">Otevřete *InstructorsController.cs* a přidat, pomocí příkazu pro obor názvů ViewModels:</span><span class="sxs-lookup"><span data-stu-id="18013-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="18013-190">Nahraďte metodu Index následující kód do udělat přes načítání souvisejících dat a umístit ho v zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="18013-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="18013-191">Metoda přijímá data volitelné trasy (`id`) a parametr řetězce dotazu (`courseID`), zadejte hodnoty ID vybrané lektorem a vybraný kurz.</span><span class="sxs-lookup"><span data-stu-id="18013-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="18013-192">Parametry jsou poskytovány **vyberte** hypertextové odkazy na stránce.</span><span class="sxs-lookup"><span data-stu-id="18013-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="18013-193">Vytvoření instance modelu zobrazení a vložení v něm seznam vyučující začne kód.</span><span class="sxs-lookup"><span data-stu-id="18013-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="18013-194">Určuje kód přes načítání pro `Instructor.OfficeAssignment` a `Instructor.CourseAssignments` navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="18013-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="18013-195">V rámci `CourseAssignments` vlastnost, `Course` vlastnost je načíst a v rámci které, `Enrollments` a `Department` vlastnosti jsou načíst a v každém `Enrollment` entity `Student` vlastnosti je načtena.</span><span class="sxs-lookup"><span data-stu-id="18013-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="18013-196">Vzhledem k tomu, že zobrazení vždy vyžaduje OfficeAssignment entity, je efektivnější načíst, ve stejném dotazu.</span><span class="sxs-lookup"><span data-stu-id="18013-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="18013-197">Během entity se vyžadují při lektorem je vybrána na webové stránce, takže jeden dotaz je lepší, než více dotazů pouze v případě, že stránka se zobrazí více často ke kurzu vybrané než bez.</span><span class="sxs-lookup"><span data-stu-id="18013-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="18013-198">Kód opakuje `CourseAssignments` a `Course` vzhledem k tomu, že budete potřebovat dvě vlastnosti z `Course`.</span><span class="sxs-lookup"><span data-stu-id="18013-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="18013-199">První řetězec `ThenInclude` volá získá `CourseAssignment.Course`, `Course.Enrollments`, a `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="18013-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="18013-200">V tomto bodě v kódu jiné `ThenInclude` by pro navigační vlastnosti `Student`, které nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="18013-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="18013-201">Ale volání `Include` začíná přes `Instructor` vlastnosti, takže budete muset projít řetězu znovu, tento čas zadání `Course.Department` místo `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="18013-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="18013-202">Následující kód provede, když se lektorem byl vybrán.</span><span class="sxs-lookup"><span data-stu-id="18013-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="18013-203">Vybrané lektorem se načítají ze seznamu vyučující v zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="18013-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="18013-204">Model zobrazení `Courses` vlastnost je pak načten pomocí postupu entity z této lektorem `CourseAssignments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="18013-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="18013-205">`Where` Metoda vrátí kolekci, ale v takovém případě kritéria předaný jejichž metoda výsledkem jsou pouze jednu entitu lektorem nevrátila.</span><span class="sxs-lookup"><span data-stu-id="18013-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="18013-206">`Single` Metoda převede kolekci na jednu entitu lektorem, která umožňuje přístup k dané entity `CourseAssignments` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="18013-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="18013-207">`CourseAssignments` Vlastnost obsahuje `CourseAssignment` entity, z nichž chcete pouze související `Course` entity.</span><span class="sxs-lookup"><span data-stu-id="18013-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="18013-208">Můžete použít `Single` metoda na kolekci, když víte kolekce budou mít jen jednu položku.</span><span class="sxs-lookup"><span data-stu-id="18013-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="18013-209">Jeden metoda vyvolá výjimku, pokud je kolekce do ní předán prázdný nebo pokud existuje více než jednu položku.</span><span class="sxs-lookup"><span data-stu-id="18013-209">The Single method throws an exception if the collection passed to it's empty or if there's more than one item.</span></span> <span data-ttu-id="18013-210">Alternativou je `SingleOrDefault`, která vrací výchozí hodnotu (null v tomto případě) Pokud je kolekce prázdná.</span><span class="sxs-lookup"><span data-stu-id="18013-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="18013-211">Ale v takovém případě stále vznikly by výjimku (z pokusu o vyhledání `Courses` vlastnost na hodnotu Null), a zpráva o výjimce by méně jasně ukazovat na příčinu problému.</span><span class="sxs-lookup"><span data-stu-id="18013-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="18013-212">Při volání `Single` metodu, můžete také předat v Where podmínku namísto volání `Where` metoda samostatně:</span><span class="sxs-lookup"><span data-stu-id="18013-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="18013-213">Namísto:</span><span class="sxs-lookup"><span data-stu-id="18013-213">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="18013-214">Dále pokud jste vybrali kurzu, vybraný kurz se načítají seznam kurzů ve model zobrazení.</span><span class="sxs-lookup"><span data-stu-id="18013-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="18013-215">Potom zobrazení modelu `Enrollments` vlastnosti je načtena s registrace entity z tohoto kurzu `Enrollments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="18013-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="18013-216">Upravit zobrazení lektorem indexu</span><span class="sxs-lookup"><span data-stu-id="18013-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="18013-217">V *Views/Instructors/Index.cshtml*, nahraďte kód šablony s následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="18013-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="18013-218">Změny se zvýrazněnou.</span><span class="sxs-lookup"><span data-stu-id="18013-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="18013-219">Stávající kód, které jste udělali následující změny:</span><span class="sxs-lookup"><span data-stu-id="18013-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="18013-220">Změnit třídu modelu k `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="18013-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="18013-221">Změnit název stránky z **Index** k **vyučující**.</span><span class="sxs-lookup"><span data-stu-id="18013-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="18013-222">Přidat **Office** sloupec, který zobrazuje `item.OfficeAssignment.Location` pouze v případě `item.OfficeAssignment` není null.</span><span class="sxs-lookup"><span data-stu-id="18013-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="18013-223">(Protože je vztah jeden pro žádná nebo jedna, nemusí být související entity OfficeAssignment.)</span><span class="sxs-lookup"><span data-stu-id="18013-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="18013-224">Přidat **kurzy** sloupec, který zobrazuje kurzy výukové podle jednotlivých lektorem.</span><span class="sxs-lookup"><span data-stu-id="18013-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="18013-225">V tématu [explicitní přechod na řádek s `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) Další informace o tuto syntaxi razor.</span><span class="sxs-lookup"><span data-stu-id="18013-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="18013-226">Přidání kódu, která dynamicky přidá `class="success"` k `tr` element vybrané lektorem.</span><span class="sxs-lookup"><span data-stu-id="18013-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="18013-227">Toto nastaví barvu pozadí vybraného řádku použití Bootstrap třídy.</span><span class="sxs-lookup"><span data-stu-id="18013-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="18013-228">Přidat nový hypertextový odkaz s názvem bez přípony **vyberte** bezprostředně před další odkazy v každém řádku, což způsobí, že ID vybrané lektorem k odeslání `Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="18013-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="18013-229">Spusťte aplikaci a vyberte **vyučující** kartě. Na stránce zobrazuje vlastnosti umístění entit v relaci OfficeAssignment a prázdná tabulka buňky po žádné související entity OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="18013-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Vyučující indexovou stránku nic zvolené](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="18013-231">V *Views/Instructors/Index.cshtml* soubor po zavření tabulky – element (na konci souboru), přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="18013-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="18013-232">Tento kód zobrazí seznam kurzů související s lektorem, pokud je vybrána lektorem.</span><span class="sxs-lookup"><span data-stu-id="18013-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="18013-233">Tento kód čte `Courses` vlastnost modelu zobrazení zobrazíte seznam kurzů.</span><span class="sxs-lookup"><span data-stu-id="18013-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="18013-234">Poskytuje také **vyberte** hypertextový odkaz, který odesílá ID vybraný kurz k `Index` metody akce.</span><span class="sxs-lookup"><span data-stu-id="18013-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="18013-235">Aktualizujte stránku a vyberte lektorem.</span><span class="sxs-lookup"><span data-stu-id="18013-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="18013-236">Nyní se zobrazí v mřížce, která zobrazuje kurzy přiřazen k vybrané lektorem a pro každou kurz zobrazí název přiřazené oddělení.</span><span class="sxs-lookup"><span data-stu-id="18013-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Vyučující Index stránky lektorem vybrané](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="18013-238">Za blok kódu, který jste právě přidali přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="18013-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="18013-239">Zobrazí seznam studenty, kteří jsou zaregistrované v kurzu při výběru tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="18013-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="18013-240">Tento kód načte vlastnost registrace zobrazení modelu Chcete-li zobrazit seznam studenty zařazen do kurzu.</span><span class="sxs-lookup"><span data-stu-id="18013-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="18013-241">Aktualizujte stránku znovu a vyberte lektorem.</span><span class="sxs-lookup"><span data-stu-id="18013-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="18013-242">Pak vyberte kurzu chcete zobrazit seznam registrovaných studenty a jejich tříd.</span><span class="sxs-lookup"><span data-stu-id="18013-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![Vyučující Index stránky lektorem a kurzu vybrán](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="18013-244">explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="18013-244">Explicit loading</span></span>

<span data-ttu-id="18013-245">Pokud jste získali seznam vyučující v *InstructorsController.cs*, jste zadali přes načítání pro `CourseAssignments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="18013-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="18013-246">Předpokládejme, že očekává uživatelům jen zřídka rádi viděli v vybrané lektorem a během registrace.</span><span class="sxs-lookup"><span data-stu-id="18013-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="18013-247">V takovém případě můžete chtít načíst data registrace pouze v případě, že se požaduje.</span><span class="sxs-lookup"><span data-stu-id="18013-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="18013-248">Chcete-li zobrazit příklad, jak to provést explicitní načítání, nahraďte `Index` metoda následující kód, který odebere přes načítání pro registraci a načte vlastnosti explicitně.</span><span class="sxs-lookup"><span data-stu-id="18013-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="18013-249">Změny kódu jsou vyznačené.</span><span class="sxs-lookup"><span data-stu-id="18013-249">The code changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="18013-250">Zahodí nový kód *ThenInclude* metoda volá zápisu dat z kódu, který načte lektorem entity.</span><span class="sxs-lookup"><span data-stu-id="18013-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="18013-251">Pokud je vybrána lektorem a kurzu, načte zvýrazněný registrace pro vybraný kurz a Student entit pro každý registrace.</span><span class="sxs-lookup"><span data-stu-id="18013-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="18013-252">Spuštění aplikace, přejděte na stránku vyučující Index a budete vidět žádné rozdíly v co se zobrazí na stránce, i když jste změnili, jak jsou načtena data.</span><span class="sxs-lookup"><span data-stu-id="18013-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="18013-253">Souhrn</span><span class="sxs-lookup"><span data-stu-id="18013-253">Summary</span></span>

<span data-ttu-id="18013-254">Můžete si teď použít přes načítání s jeden dotaz a více dotazů ke čtení související data do navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="18013-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="18013-255">V dalším kurzu dozvíte, jak aktualizovat data v relaci.</span><span class="sxs-lookup"><span data-stu-id="18013-255">In the next tutorial you'll learn how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="18013-256">[Předchozí](complex-data-model.md)
>[další](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="18013-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>  
