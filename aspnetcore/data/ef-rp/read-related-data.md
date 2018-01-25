---
title: "Stránky Razor EF základní - číst související Data - 6, 8"
author: rick-anderson
description: "V tomto kurzu číst a zobrazení souvisejících dat – to znamená, data, která rozhraní Entity Framework se načte do navigační vlastnosti."
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 532020a8fe4c5a0312cbd89278e61f614b1825f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="12eec-103">Čtení související data – základní EF s stránky Razor (6 8)</span><span class="sxs-lookup"><span data-stu-id="12eec-103">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="12eec-104">Podle [tní Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="12eec-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="12eec-105">V tomto kurzu související data načíst a zobrazit.</span><span class="sxs-lookup"><span data-stu-id="12eec-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="12eec-106">Související data jsou data, která načte EF základní do navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="12eec-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="12eec-107">Pokud narazíte na problémy, které nelze vyřešit, stáhněte si [dokončené aplikace pro tuto fázi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="12eec-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="12eec-108">Následující ilustrace znázorňuje dokončené stránky v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="12eec-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Kurzy indexovou stránku](read-related-data/_static/courses-index.png)

![Index stránky vyučující](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="12eec-111">Přes, explicitní a opožděného načítání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="12eec-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="12eec-112">Že EF základní můžete načíst související data do navigační vlastnosti entity několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="12eec-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="12eec-113">[Přes načítání](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="12eec-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="12eec-114">Přes načítání je při dotazu pro jeden typ entity také načtení entit v relaci.</span><span class="sxs-lookup"><span data-stu-id="12eec-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="12eec-115">Při čtení je entita, související data načtena.</span><span class="sxs-lookup"><span data-stu-id="12eec-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="12eec-116">To obvykle vede jednoho připojení dotaz, který načte všechna data, která je potřeba.</span><span class="sxs-lookup"><span data-stu-id="12eec-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="12eec-117">Základní EF vydá pro některé typy přes načítání více dotazů.</span><span class="sxs-lookup"><span data-stu-id="12eec-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="12eec-118">Vydání více dotazů může být efektivnější než v případě pro některé dotazy v EF6 natolik, že jeden dotaz.</span><span class="sxs-lookup"><span data-stu-id="12eec-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="12eec-119">Je zadaný přes načítání `Include` a `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="12eec-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![Příklad přes načítání](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="12eec-121">Přes načítání pošle více dotazů, když je součástí nvavigation kolekce:</span><span class="sxs-lookup"><span data-stu-id="12eec-121">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="12eec-122">Jeden dotaz pro hlavní dotaz</span><span class="sxs-lookup"><span data-stu-id="12eec-122">One query for the main query</span></span> 
 * <span data-ttu-id="12eec-123">Jeden dotaz pro každou kolekci "edge" ve stromové struktuře zatížení.</span><span class="sxs-lookup"><span data-stu-id="12eec-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="12eec-124">Samostatné dotazy s `Load`: v samostatné dotazy může být načtena data a základní EF "oprav" navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="12eec-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="12eec-125">"opravy nahoru" znamená, že základní EF automaticky naplní navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="12eec-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="12eec-126">Samostatné dotazy s `Load` se víc podobá explict načítání než přes načítání.</span><span class="sxs-lookup"><span data-stu-id="12eec-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![Příklad samostatné dotazy](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="12eec-128">Poznámka: Základní EF automaticky opravuje navigační vlastností s jinými entitami, které byly dříve načteny do instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="12eec-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="12eec-129">I když se data pro navigační vlastnost *není* výslovně zahrnuty, vlastnost pořád naplněný, pokud některé nebo všechny související entity byly dříve načteny.</span><span class="sxs-lookup"><span data-stu-id="12eec-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="12eec-130">[Explicitní načítání](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="12eec-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="12eec-131">Když je nejdřív přečíst entity, není načíst související data.</span><span class="sxs-lookup"><span data-stu-id="12eec-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="12eec-132">Kód musí být zapsané do načíst související data, když je to potřeba.</span><span class="sxs-lookup"><span data-stu-id="12eec-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="12eec-133">Explicitní načítání s samostatné dotazy za následek více dotazů odesílaných do databáze.</span><span class="sxs-lookup"><span data-stu-id="12eec-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="12eec-134">S explicitní načítání, určuje kód navigační vlastnosti, které mají být načtena.</span><span class="sxs-lookup"><span data-stu-id="12eec-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="12eec-135">Použití `Load` metoda udělat explicitní načítání.</span><span class="sxs-lookup"><span data-stu-id="12eec-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="12eec-136">Příklad:</span><span class="sxs-lookup"><span data-stu-id="12eec-136">For example:</span></span>

 ![Příklad explicitní načítání](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="12eec-138">[Opožděného načítání](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="12eec-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="12eec-139">[Základní EF aktuálně nepodporuje opožděného načítání](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="12eec-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="12eec-140">Když je nejdřív přečíst entity, není načíst související data.</span><span class="sxs-lookup"><span data-stu-id="12eec-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="12eec-141">Při prvním přístupu k navigační vlastnost, lze data potřebná pro tuto navigační vlastnost je automaticky načte.</span><span class="sxs-lookup"><span data-stu-id="12eec-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="12eec-142">K databázi. pokaždé, když navigační vlastnost přistupuje poprvé bude odeslán dotaz.</span><span class="sxs-lookup"><span data-stu-id="12eec-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="12eec-143">`Select` Operátor načte pouze související data, které jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="12eec-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="12eec-144">Vytvořte stránku kurzy, která zobrazí název oddělení</span><span class="sxs-lookup"><span data-stu-id="12eec-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="12eec-145">Navigační vlastnost, která obsahuje zahrnuje entity kurzu `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="12eec-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="12eec-146">`Department` Oddělení, které během je přiřazena k obsahuje entity.</span><span class="sxs-lookup"><span data-stu-id="12eec-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="12eec-147">Zobrazí se název přiřazené oddělení v seznamu kurzů:</span><span class="sxs-lookup"><span data-stu-id="12eec-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="12eec-148">Získat `Name` vlastnost z `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="12eec-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="12eec-149">`Department` Entity pochází z `Course.Department` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="12eec-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="12eec-151">Vygenerované uživatelské rozhraní během modelu</span><span class="sxs-lookup"><span data-stu-id="12eec-151">Scaffold the Course model</span></span>

* <span data-ttu-id="12eec-152">Ukončete aplikaci Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="12eec-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="12eec-153">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="12eec-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="12eec-154">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="12eec-154">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="12eec-155">Předchozí příkaz scaffold `Course` modelu.</span><span class="sxs-lookup"><span data-stu-id="12eec-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="12eec-156">Otevřete projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="12eec-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="12eec-157">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="12eec-157">Build the project.</span></span> <span data-ttu-id="12eec-158">Sestavení generuje chyby takto:</span><span class="sxs-lookup"><span data-stu-id="12eec-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="12eec-159">Globálně změnit `_context.Course` k `_context.Courses` (tedy "s" přidat do `Course`).</span><span class="sxs-lookup"><span data-stu-id="12eec-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="12eec-160">7 výskytů jsou vyhledána a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="12eec-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="12eec-161">Otevřete *Pages/Courses/Index.cshtml.cs* a prozkoumat `OnGetAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="12eec-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="12eec-162">Přes načítání pro zadaný modul generování uživatelského rozhraní `Department` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="12eec-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="12eec-163">`Include` Metoda určuje přes načítání.</span><span class="sxs-lookup"><span data-stu-id="12eec-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="12eec-164">Spusťte aplikaci a vyberte **kurzy** odkaz.</span><span class="sxs-lookup"><span data-stu-id="12eec-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="12eec-165">Zobrazuje sloupec oddělení `DepartmentID`, který není nijak užitečné.</span><span class="sxs-lookup"><span data-stu-id="12eec-165">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="12eec-166">Aktualizace `OnGetAsync` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="12eec-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="12eec-167">Předchozí kód přidá `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="12eec-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="12eec-168">`AsNoTracking`zvyšuje výkon, protože nejsou sledovat entity vrátila.</span><span class="sxs-lookup"><span data-stu-id="12eec-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="12eec-169">Entity, které nejsou sledovat, protože se neaktualizují v aktuálním kontextu.</span><span class="sxs-lookup"><span data-stu-id="12eec-169">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="12eec-170">Aktualizace *Views/Courses/Index.cshtml* s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="12eec-170">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="12eec-171">Na automaticky generovaný kód byly provedeny následující změny:</span><span class="sxs-lookup"><span data-stu-id="12eec-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="12eec-172">Změnit záhlaví od indexu na kurzy.</span><span class="sxs-lookup"><span data-stu-id="12eec-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="12eec-173">Přidat **číslo** sloupec, který ukazuje `CourseID` hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="12eec-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="12eec-174">Ve výchozím nastavení nejsou vygeneroval primární klíče, protože jsou obvykle smysl pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="12eec-174">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="12eec-175">Ale v takovém případě primární klíč má smysl.</span><span class="sxs-lookup"><span data-stu-id="12eec-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="12eec-176">Změnit **oddělení** sloupec, který se zobrazí název oddělení.</span><span class="sxs-lookup"><span data-stu-id="12eec-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="12eec-177">Kód zobrazí `Name` vlastnost `Department` entity, který je načten do `Department` navigační vlastnost:</span><span class="sxs-lookup"><span data-stu-id="12eec-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="12eec-178">Spusťte aplikaci a vyberte **kurzy** karty zobrazíte seznam s názvy oddělení.</span><span class="sxs-lookup"><span data-stu-id="12eec-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kurzy indexovou stránku](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="12eec-180">Načítají se související data pomocí vyberte</span><span class="sxs-lookup"><span data-stu-id="12eec-180">Loading related data with Select</span></span>

<span data-ttu-id="12eec-181">`OnGetAsync` Metoda načte související data se `Include` metoda:</span><span class="sxs-lookup"><span data-stu-id="12eec-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="12eec-182">`Select` Operátor načte pouze související data, které jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="12eec-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="12eec-183">Pro jednotlivé položky jako `Department.Name` používá SQL vnitřního spojení.</span><span class="sxs-lookup"><span data-stu-id="12eec-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="12eec-184">Pro kolekce používá jiný přístup k databázi, ale tak.`Include`</span><span class="sxs-lookup"><span data-stu-id="12eec-184">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="12eec-185">operátor u kolekcí.</span><span class="sxs-lookup"><span data-stu-id="12eec-185">operator on collections.</span></span>

<span data-ttu-id="12eec-186">Následující kód načte související data se `Select` metoda:</span><span class="sxs-lookup"><span data-stu-id="12eec-186">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="12eec-187">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="12eec-187">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="12eec-188">V tématu [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) a [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) kompletní příklad.</span><span class="sxs-lookup"><span data-stu-id="12eec-188">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="12eec-189">Vytvořit stránku vyučující, která zobrazuje kurzy a registrace</span><span class="sxs-lookup"><span data-stu-id="12eec-189">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="12eec-190">V této části se vytvoří vyučující stránky.</span><span class="sxs-lookup"><span data-stu-id="12eec-190">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="12eec-191">![Index stránky vyučující](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="12eec-191">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="12eec-192">Tato stránka načte a zobrazí související data následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="12eec-192">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="12eec-193">Zobrazí seznam vyučující souvisejících dat z `OfficeAssignment` entity (Office na předchozím obrázku).</span><span class="sxs-lookup"><span data-stu-id="12eec-193">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="12eec-194">`Instructor` a `OfficeAssignment` entity jsou ve vztahu-nula nebo 1.</span><span class="sxs-lookup"><span data-stu-id="12eec-194">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="12eec-195">Přes načítání se používá pro `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="12eec-195">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="12eec-196">Přes načítání je obvykle efektivnější, když související data se mají zobrazit.</span><span class="sxs-lookup"><span data-stu-id="12eec-196">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="12eec-197">V takovém případě se zobrazí přiřazení office pro vyučující.</span><span class="sxs-lookup"><span data-stu-id="12eec-197">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="12eec-198">Když uživatel vybere lektorem (Harui na předchozím obrázku), související `Course` entity jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="12eec-198">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="12eec-199">`Instructor` a `Course` jsou entit v relaci m: n.</span><span class="sxs-lookup"><span data-stu-id="12eec-199">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="12eec-200">Načítání pro nápovědy eager `Course` entity a jejich související `Department` entity se používá.</span><span class="sxs-lookup"><span data-stu-id="12eec-200">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="12eec-201">Samostatné dotazy v tomto případě může být efektivnější, protože jsou potřeba jenom kurzy pro vybrané lektorem.</span><span class="sxs-lookup"><span data-stu-id="12eec-201">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="12eec-202">Tento příklad ukazuje způsob použití přes načítání pro navigační vlastnosti v entity, které jsou v navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="12eec-202">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="12eec-203">Když uživatel vybere kurzu (chemie na předchozím obrázku), související data z `Enrollments` entity se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="12eec-203">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="12eec-204">Na předchozím obrázku se zobrazí název student a úrovni.</span><span class="sxs-lookup"><span data-stu-id="12eec-204">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="12eec-205">`Course` a `Enrollment` entity jsou v vztah jeden mnoho.</span><span class="sxs-lookup"><span data-stu-id="12eec-205">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="12eec-206">Vytvoření modelu zobrazení pro Index lektorem zobrazení</span><span class="sxs-lookup"><span data-stu-id="12eec-206">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="12eec-207">Stránka vyučující zobrazuje data ze tří různých tabulek.</span><span class="sxs-lookup"><span data-stu-id="12eec-207">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="12eec-208">Model zobrazení je vytvořen, který zahrnuje tři entity představující tři tabulky.</span><span class="sxs-lookup"><span data-stu-id="12eec-208">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="12eec-209">V *SchoolViewModels* složku vytvořit *InstructorIndexData.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="12eec-209">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="12eec-210">Vygenerované uživatelské rozhraní lektorem modelu</span><span class="sxs-lookup"><span data-stu-id="12eec-210">Scaffold the Instructor model</span></span>

* <span data-ttu-id="12eec-211">Ukončete aplikaci Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="12eec-211">Exit Visual Studio.</span></span>
* <span data-ttu-id="12eec-212">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="12eec-212">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="12eec-213">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="12eec-213">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="12eec-214">Předchozí příkaz scaffold `Instructor` modelu.</span><span class="sxs-lookup"><span data-stu-id="12eec-214">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="12eec-215">Otevřete projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="12eec-215">Open the project in Visual Studio.</span></span>

<span data-ttu-id="12eec-216">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="12eec-216">Build the project.</span></span> <span data-ttu-id="12eec-217">Sestavení generuje chyby.</span><span class="sxs-lookup"><span data-stu-id="12eec-217">The build generates errors.</span></span>

<span data-ttu-id="12eec-218">Globálně změnit `_context.Instructor` k `_context.Instructors` (tedy "s" přidat do `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="12eec-218">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="12eec-219">7 výskytů jsou vyhledána a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="12eec-219">7 occurrences are found and updated.</span></span>

<span data-ttu-id="12eec-220">Spusťte aplikaci a přejděte na stránku vyučující.</span><span class="sxs-lookup"><span data-stu-id="12eec-220">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="12eec-221">Nahraďte *Pages/Instructors/Index.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="12eec-221">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="12eec-222">`OnGetAsync` Metoda přijímá data volitelné trasy pro ID vybrané lektorem.</span><span class="sxs-lookup"><span data-stu-id="12eec-222">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="12eec-223">Vyhledejte dotaz na *Pages/Instructors/Index.cshtml* stránky:</span><span class="sxs-lookup"><span data-stu-id="12eec-223">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="12eec-224">Dotaz má dva zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="12eec-224">The query has two includes:</span></span>

* <span data-ttu-id="12eec-225">`OfficeAssignment`: Zobrazených v [vyučující zobrazení](#IP).</span><span class="sxs-lookup"><span data-stu-id="12eec-225">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="12eec-226">`CourseAssignments`: Což přináší v rámci přípravy výukové.</span><span class="sxs-lookup"><span data-stu-id="12eec-226">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="12eec-227">Aktualizace indexovou stránku vyučující</span><span class="sxs-lookup"><span data-stu-id="12eec-227">Update the instructors Index page</span></span>

<span data-ttu-id="12eec-228">Aktualizace *Pages/Instructors/Index.cshtml* s následující kód:</span><span class="sxs-lookup"><span data-stu-id="12eec-228">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="12eec-229">Předchozí kód provede tyto změny:</span><span class="sxs-lookup"><span data-stu-id="12eec-229">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="12eec-230">Aktualizace `page` direktivy z `@page` k `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="12eec-230">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="12eec-231">`"{id:int?}"`je šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="12eec-231">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="12eec-232">Šablona trasy změny řetězce dotazu celé číslo v adrese URL data trasy.</span><span class="sxs-lookup"><span data-stu-id="12eec-232">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="12eec-233">Například kliknete na **vyberte** odkaz lektorem při page – direktiva vytvoří adresu URL podobnou následující:</span><span class="sxs-lookup"><span data-stu-id="12eec-233">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="12eec-234">Pokud se jedná o direktivu stránky `@page "{id:int?}"`, je předchozí adresa URL:</span><span class="sxs-lookup"><span data-stu-id="12eec-234">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="12eec-235">Název stránky je **vyučující**.</span><span class="sxs-lookup"><span data-stu-id="12eec-235">Page title is **Instructors**.</span></span>
* <span data-ttu-id="12eec-236">Přidat **Office** sloupec, který zobrazuje `item.OfficeAssignment.Location` pouze v případě `item.OfficeAssignment` není null.</span><span class="sxs-lookup"><span data-stu-id="12eec-236">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="12eec-237">Protože tato relace-nula nebo 1, nemusí být související entity OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="12eec-237">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="12eec-238">Přidat **kurzy** sloupec, který zobrazuje kurzy výukové podle jednotlivých lektorem.</span><span class="sxs-lookup"><span data-stu-id="12eec-238">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="12eec-239">V tématu [explicitní přechod na řádek s `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) Další informace o tuto syntaxi razor.</span><span class="sxs-lookup"><span data-stu-id="12eec-239">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="12eec-240">Přidání kódu, která dynamicky přidá `class="success"` k `tr` element vybrané lektorem.</span><span class="sxs-lookup"><span data-stu-id="12eec-240">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="12eec-241">Toto nastaví barvu pozadí vybraného řádku použití Bootstrap třídy.</span><span class="sxs-lookup"><span data-stu-id="12eec-241">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="12eec-242">Přidat nový hypertextový odkaz s názvem bez přípony **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="12eec-242">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="12eec-243">Tento odkaz odešle vybraný lektorem ID, který má `Index` metoda a nastaví barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="12eec-243">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="12eec-244">Spusťte aplikaci a vyberte **vyučující** kartě. Na stránce se zobrazuje `Location` (office) z související `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="12eec-244">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="12eec-245">Pokud OfficeAssignment' je null, se zobrazí buňky prázdná tabulka.</span><span class="sxs-lookup"><span data-stu-id="12eec-245">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Vyučující indexovou stránku nic zvolené](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="12eec-247">Klikněte na **vyberte** odkaz.</span><span class="sxs-lookup"><span data-stu-id="12eec-247">Click on the **Select** link.</span></span> <span data-ttu-id="12eec-248">Změny styl řádku.</span><span class="sxs-lookup"><span data-stu-id="12eec-248">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="12eec-249">Přidat kurzy výukové podle vybrané lektorem</span><span class="sxs-lookup"><span data-stu-id="12eec-249">Add courses taught by selected instructor</span></span>

<span data-ttu-id="12eec-250">Aktualizace `OnGetAsync` metoda v *Pages/Instructors/Index.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="12eec-250">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="12eec-251">Zkontrolujte aktualizované dotazu:</span><span class="sxs-lookup"><span data-stu-id="12eec-251">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="12eec-252">Přidá předchozího dotazu `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="12eec-252">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="12eec-253">Následující kód provede, když je vybrána lektorem (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="12eec-253">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="12eec-254">Vybrané lektorem se načítají ze seznamu vyučující v zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="12eec-254">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="12eec-255">Model zobrazení `Courses` vlastnosti je načtena s `Course` entity z této lektorem `CourseAssignments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="12eec-255">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="12eec-256">`Where` Metoda vrátí kolekci.</span><span class="sxs-lookup"><span data-stu-id="12eec-256">The `Where` method returns a collection.</span></span> <span data-ttu-id="12eec-257">V předchozím `Where` metoda jediným `Instructor` se vrátí entity.</span><span class="sxs-lookup"><span data-stu-id="12eec-257">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="12eec-258">`Single` Metoda převede kolekci do jednoho `Instructor` entity.</span><span class="sxs-lookup"><span data-stu-id="12eec-258">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="12eec-259">`Instructor` Entity poskytuje přístup k `CourseAssignments` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="12eec-259">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="12eec-260">`CourseAssignments`poskytuje přístup k související `Course` entity.</span><span class="sxs-lookup"><span data-stu-id="12eec-260">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![M:M lektorem kurzy](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="12eec-262">`Single` Metoda se používá na kolekci, pokud kolekce obsahuje pouze jednu položku.</span><span class="sxs-lookup"><span data-stu-id="12eec-262">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="12eec-263">`Single` Metoda vyvolá výjimku, pokud je kolekce prázdná, nebo pokud existuje více než jednu položku.</span><span class="sxs-lookup"><span data-stu-id="12eec-263">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="12eec-264">Alternativou je `SingleOrDefault`, která vrací výchozí hodnotu (null v tomto případě) Pokud je kolekce prázdná.</span><span class="sxs-lookup"><span data-stu-id="12eec-264">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="12eec-265">Pomocí `SingleOrDefault` na prázdnou kolekci:</span><span class="sxs-lookup"><span data-stu-id="12eec-265">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="12eec-266">Výsledkem výjimku (z pokusu o vyhledání `Courses` vlastnost na hodnotu Null).</span><span class="sxs-lookup"><span data-stu-id="12eec-266">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="12eec-267">Zpráva o výjimce by méně jasně ukazovat na příčinu problému.</span><span class="sxs-lookup"><span data-stu-id="12eec-267">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="12eec-268">Následující kód naplní model zobrazení `Enrollments` vlastnost, pokud je vybrána kurzu:</span><span class="sxs-lookup"><span data-stu-id="12eec-268">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="12eec-269">Přidejte následující kód do konce *Pages/Courses/Index.cshtml* Razor stránky:</span><span class="sxs-lookup"><span data-stu-id="12eec-269">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="12eec-270">Předchozí kód zobrazí seznam kurzů související s lektorem, pokud je vybrána lektorem.</span><span class="sxs-lookup"><span data-stu-id="12eec-270">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="12eec-271">Testování aplikací.</span><span class="sxs-lookup"><span data-stu-id="12eec-271">Test the app.</span></span> <span data-ttu-id="12eec-272">Klikněte na **vyberte** na stránce vyučující odkaz.</span><span class="sxs-lookup"><span data-stu-id="12eec-272">Click on a **Select** link on the instructors page.</span></span>

![Vyučující Index stránky lektorem vybrané](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="12eec-274">Zobrazit data pro studenty</span><span class="sxs-lookup"><span data-stu-id="12eec-274">Show student data</span></span>

<span data-ttu-id="12eec-275">V této části je aktualizována aplikace na student data pro vybrané kurzu.</span><span class="sxs-lookup"><span data-stu-id="12eec-275">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="12eec-276">Aktualizace dotazu ve `OnGetAsync` metoda v *Pages/Instructors/Index.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="12eec-276">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="12eec-277">Aktualizace *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="12eec-277">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="12eec-278">Na konec souboru přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="12eec-278">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="12eec-279">Předchozí kód zobrazí seznam studenty, kteří jsou zaregistrované v vybraný kurz.</span><span class="sxs-lookup"><span data-stu-id="12eec-279">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="12eec-280">Aktualizujte stránku a vyberte lektorem.</span><span class="sxs-lookup"><span data-stu-id="12eec-280">Refresh the page and select an instructor.</span></span> <span data-ttu-id="12eec-281">Vyberte kurz chcete zobrazit seznam registrovaných studenty a jejich tříd.</span><span class="sxs-lookup"><span data-stu-id="12eec-281">Select a course to see the list of enrolled students and their grades.</span></span>

![Vyučující Index stránky lektorem a kurzu vybrán](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="12eec-283">Pomocí jednoho</span><span class="sxs-lookup"><span data-stu-id="12eec-283">Using Single</span></span>

<span data-ttu-id="12eec-284">`Single` Metoda můžete předat `Where` podmínku namísto volání `Where` metoda samostatně:</span><span class="sxs-lookup"><span data-stu-id="12eec-284">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="12eec-285">Podle předchozích `Single` přístup poskytuje žádné výhod oproti použití `Where`.</span><span class="sxs-lookup"><span data-stu-id="12eec-285">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="12eec-286">Někteří vývojáři raději `Single` přístupu stylu.</span><span class="sxs-lookup"><span data-stu-id="12eec-286">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="12eec-287">explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="12eec-287">Explicit loading</span></span>

<span data-ttu-id="12eec-288">Určuje aktuálního kódu přes načítání pro `Enrollments` a `Students`:</span><span class="sxs-lookup"><span data-stu-id="12eec-288">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="12eec-289">Předpokládejme, že uživatelé chtějí zřídka najdete v části registrace v kurzu.</span><span class="sxs-lookup"><span data-stu-id="12eec-289">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="12eec-290">V takovém případě optimalizace bude pouze načíst data zápisu, pokud se vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="12eec-290">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="12eec-291">V této části `OnGetAsync` je aktualizovat, a použít explicitní načítání `Enrollments` a `Students`.</span><span class="sxs-lookup"><span data-stu-id="12eec-291">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="12eec-292">Aktualizace `OnGetAsync` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="12eec-292">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="12eec-293">Předchozí kód zahodí *ThenInclude* metoda volá pro registraci a student data.</span><span class="sxs-lookup"><span data-stu-id="12eec-293">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="12eec-294">Pokud je vybrána kurzu, načte zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="12eec-294">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="12eec-295">`Enrollment` Entity pro vybraný kurz.</span><span class="sxs-lookup"><span data-stu-id="12eec-295">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="12eec-296">`Student` Entity pro každou `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="12eec-296">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="12eec-297">Všimněte si předchozí kód komentáře se `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="12eec-297">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="12eec-298">Navigační vlastnosti mohou být explicitně načteny pouze u sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="12eec-298">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="12eec-299">Testování aplikací.</span><span class="sxs-lookup"><span data-stu-id="12eec-299">Test the app.</span></span> <span data-ttu-id="12eec-300">Z hlediska uživatelů aplikace se chová stejně jako se jeho předchozí verze.</span><span class="sxs-lookup"><span data-stu-id="12eec-300">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="12eec-301">Další kurz ukazuje, jak aktualizovat data v relaci.</span><span class="sxs-lookup"><span data-stu-id="12eec-301">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="12eec-302">[Předchozí](xref:data/ef-rp/complex-data-model)
>[další](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="12eec-302">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
