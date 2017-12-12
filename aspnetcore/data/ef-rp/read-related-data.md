---
title: "Stránky Razor EF základní - číst související Data - 6, 8"
author: rick-anderson
description: "V tomto kurzu číst a zobrazení souvisejících dat – to znamená, data, která rozhraní Entity Framework se načte do navigační vlastnosti."
keywords: "Spojí ASP.NET Core Entity Framework Core, související data"
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ba9b17ecdcb605d39117d03230b1db37e8e4d0dd
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="cddfc-104">Čtení související data – základní EF s stránky Razor (6 8)</span><span class="sxs-lookup"><span data-stu-id="cddfc-104">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="cddfc-105">Podle [tní Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cddfc-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="cddfc-106">V tomto kurzu související data načíst a zobrazit.</span><span class="sxs-lookup"><span data-stu-id="cddfc-106">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="cddfc-107">Související data jsou data, která načte EF základní do navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="cddfc-107">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="cddfc-108">Pokud narazíte na problémy, které nelze vyřešit, stáhněte si [dokončené aplikace pro tuto fázi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="cddfc-108">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="cddfc-109">Následující ilustrace znázorňuje dokončené stránky v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="cddfc-109">The following illustrations show the completed pages for this tutorial:</span></span>

![Kurzy indexovou stránku](read-related-data/_static/courses-index.png)

![Index stránky vyučující](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="cddfc-112">Přes, explicitní a opožděného načítání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="cddfc-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="cddfc-113">Že EF základní můžete načíst související data do navigační vlastnosti entity několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="cddfc-113">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="cddfc-114">[Přes načítání](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="cddfc-114">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="cddfc-115">Přes načítání je při dotazu pro jeden typ entity také načtení entit v relaci.</span><span class="sxs-lookup"><span data-stu-id="cddfc-115">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="cddfc-116">Při čtení je entita, související data načtena.</span><span class="sxs-lookup"><span data-stu-id="cddfc-116">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="cddfc-117">To obvykle vede jednoho připojení dotaz, který načte všechna data, která je potřeba.</span><span class="sxs-lookup"><span data-stu-id="cddfc-117">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="cddfc-118">Základní EF vydá pro některé typy přes načítání více dotazů.</span><span class="sxs-lookup"><span data-stu-id="cddfc-118">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="cddfc-119">Vydání více dotazů může být efektivnější než v případě pro některé dotazy v EF6 natolik, že jeden dotaz.</span><span class="sxs-lookup"><span data-stu-id="cddfc-119">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="cddfc-120">Je zadaný přes načítání `Include` a `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="cddfc-120">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![Příklad přes načítání](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="cddfc-122">Přes načítání pošle více dotazů, když je součástí nvavigation kolekce:</span><span class="sxs-lookup"><span data-stu-id="cddfc-122">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="cddfc-123">Jeden dotaz pro hlavní dotaz</span><span class="sxs-lookup"><span data-stu-id="cddfc-123">One query for the main query</span></span> 
 * <span data-ttu-id="cddfc-124">Jeden dotaz pro každou kolekci "edge" ve stromové struktuře zatížení.</span><span class="sxs-lookup"><span data-stu-id="cddfc-124">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="cddfc-125">Samostatné dotazy s `Load`: v samostatné dotazy může být načtena data a základní EF "oprav" navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="cddfc-125">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="cddfc-126">"opravy nahoru" znamená, že základní EF automaticky naplní navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="cddfc-126">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="cddfc-127">Samostatné dotazy s `Load` se víc podobá explict načítání než přes načítání.</span><span class="sxs-lookup"><span data-stu-id="cddfc-127">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![Příklad samostatné dotazy](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="cddfc-129">Poznámka: Základní EF automaticky opravuje navigační vlastností s jinými entitami, které byly dříve načteny do instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="cddfc-129">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="cddfc-130">I když se data pro navigační vlastnost *není* výslovně zahrnuty, vlastnost pořád naplněný, pokud některé nebo všechny související entity byly dříve načteny.</span><span class="sxs-lookup"><span data-stu-id="cddfc-130">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="cddfc-131">[Explicitní načítání](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="cddfc-131">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="cddfc-132">Když je nejdřív přečíst entity, není načíst související data.</span><span class="sxs-lookup"><span data-stu-id="cddfc-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="cddfc-133">Kód musí být zapsané do načíst související data, když je to potřeba.</span><span class="sxs-lookup"><span data-stu-id="cddfc-133">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="cddfc-134">Explicitní načítání s samostatné dotazy za následek více dotazů odesílaných do databáze.</span><span class="sxs-lookup"><span data-stu-id="cddfc-134">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="cddfc-135">S explicitní načítání, určuje kód navigační vlastnosti, které mají být načtena.</span><span class="sxs-lookup"><span data-stu-id="cddfc-135">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="cddfc-136">Použití `Load` metoda udělat explicitní načítání.</span><span class="sxs-lookup"><span data-stu-id="cddfc-136">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="cddfc-137">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cddfc-137">For example:</span></span>

 ![Příklad explicitní načítání](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="cddfc-139">[Opožděného načítání](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="cddfc-139">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="cddfc-140">[Základní EF aktuálně nepodporuje opožděného načítání](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="cddfc-140">[EF Core does not currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="cddfc-141">Když je nejdřív přečíst entity, není načíst související data.</span><span class="sxs-lookup"><span data-stu-id="cddfc-141">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="cddfc-142">Při prvním přístupu k navigační vlastnost, lze data potřebná pro tuto navigační vlastnost je automaticky načte.</span><span class="sxs-lookup"><span data-stu-id="cddfc-142">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="cddfc-143">K databázi. pokaždé, když navigační vlastnost přistupuje poprvé bude odeslán dotaz.</span><span class="sxs-lookup"><span data-stu-id="cddfc-143">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="cddfc-144">`Select` Operátor načte pouze související data, které jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="cddfc-144">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="cddfc-145">Vytvořte stránku kurzy, která zobrazí název oddělení</span><span class="sxs-lookup"><span data-stu-id="cddfc-145">Create a Courses page that displays department name</span></span>

<span data-ttu-id="cddfc-146">Navigační vlastnost, která obsahuje zahrnuje entity kurzu `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="cddfc-146">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="cddfc-147">`Department` Oddělení, které během je přiřazena k obsahuje entity.</span><span class="sxs-lookup"><span data-stu-id="cddfc-147">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="cddfc-148">Zobrazí se název přiřazené oddělení v seznamu kurzů:</span><span class="sxs-lookup"><span data-stu-id="cddfc-148">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="cddfc-149">Získat `Name` vlastnost z `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="cddfc-149">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="cddfc-150">`Department` Entity pochází z `Course.Department` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="cddfc-150">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse. Oddělení](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="cddfc-152">Vygenerované uživatelské rozhraní během modelu</span><span class="sxs-lookup"><span data-stu-id="cddfc-152">Scaffold the Course model</span></span>

* <span data-ttu-id="cddfc-153">Ukončete aplikaci Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cddfc-153">Exit Visual Studio.</span></span>
* <span data-ttu-id="cddfc-154">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="cddfc-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="cddfc-155">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cddfc-155">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="cddfc-156">Předchozí příkaz scaffold `Course` modelu.</span><span class="sxs-lookup"><span data-stu-id="cddfc-156">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="cddfc-157">Otevřete projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cddfc-157">Open the project in Visual Studio.</span></span>

<span data-ttu-id="cddfc-158">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="cddfc-158">Build the project.</span></span> <span data-ttu-id="cddfc-159">Sestavení generuje chyby takto:</span><span class="sxs-lookup"><span data-stu-id="cddfc-159">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="cddfc-160">Globálně změnit `_context.Course` k `_context.Courses` (tedy "s" přidat do `Course`).</span><span class="sxs-lookup"><span data-stu-id="cddfc-160">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="cddfc-161">7 výskytů jsou vyhledána a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="cddfc-161">7 occurrences are found and updated.</span></span>

<span data-ttu-id="cddfc-162">Otevřete *Pages/Courses/Index.cshtml.cs* a prozkoumat `OnGetAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="cddfc-162">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="cddfc-163">Přes načítání pro zadaný modul generování uživatelského rozhraní `Department` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="cddfc-163">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="cddfc-164">`Include` Metoda určuje přes načítání.</span><span class="sxs-lookup"><span data-stu-id="cddfc-164">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="cddfc-165">Spusťte aplikaci a vyberte **kurzy** odkaz.</span><span class="sxs-lookup"><span data-stu-id="cddfc-165">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="cddfc-166">Zobrazuje sloupec oddělení `DepartmentID`, což není užitečné.</span><span class="sxs-lookup"><span data-stu-id="cddfc-166">The department column displays the `DepartmentID`, which is not useful.</span></span>

<span data-ttu-id="cddfc-167">Aktualizace `OnGetAsync` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="cddfc-167">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="cddfc-168">Předchozí kód přidá `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="cddfc-168">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="cddfc-169">`AsNoTracking`zvyšuje výkon, protože nejsou sledovat entity vrátila.</span><span class="sxs-lookup"><span data-stu-id="cddfc-169">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="cddfc-170">Entity, které nejsou sledovat, protože se neaktualizují v aktuálním kontextu.</span><span class="sxs-lookup"><span data-stu-id="cddfc-170">The entities are not tracked because they are not updated in the current context.</span></span>

<span data-ttu-id="cddfc-171">Aktualizace *Views/Courses/Index.cshtml* s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="cddfc-171">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="cddfc-172">Na automaticky generovaný kód byly provedeny následující změny:</span><span class="sxs-lookup"><span data-stu-id="cddfc-172">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="cddfc-173">Změnit záhlaví od indexu na kurzy.</span><span class="sxs-lookup"><span data-stu-id="cddfc-173">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="cddfc-174">Přidat **číslo** sloupec, který ukazuje `CourseID` hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="cddfc-174">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="cddfc-175">Ve výchozím nastavení nejsou vygeneroval primární klíče, protože jsou obvykle smysl pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="cddfc-175">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="cddfc-176">Ale v takovém případě primární klíč má smysl.</span><span class="sxs-lookup"><span data-stu-id="cddfc-176">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="cddfc-177">Změnit **oddělení** sloupec, který se zobrazí název oddělení.</span><span class="sxs-lookup"><span data-stu-id="cddfc-177">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="cddfc-178">Kód zobrazí `Name` vlastnost `Department` entity, který je načten do `Department` navigační vlastnost:</span><span class="sxs-lookup"><span data-stu-id="cddfc-178">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="cddfc-179">Spusťte aplikaci a vyberte **kurzy** karty zobrazíte seznam s názvy oddělení.</span><span class="sxs-lookup"><span data-stu-id="cddfc-179">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kurzy indexovou stránku](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="cddfc-181">Načítají se související data pomocí vyberte</span><span class="sxs-lookup"><span data-stu-id="cddfc-181">Loading related data with Select</span></span>

<span data-ttu-id="cddfc-182">`OnGetAsync` Metoda načte související data se `Include` metoda:</span><span class="sxs-lookup"><span data-stu-id="cddfc-182">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="cddfc-183">`Select` Operátor načte pouze související data, které jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="cddfc-183">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="cddfc-184">Pro jednotlivé položky jako `Department.Name` používá SQL vnitřního spojení.</span><span class="sxs-lookup"><span data-stu-id="cddfc-184">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="cddfc-185">Pro kolekce používá jiný přístup k databázi, ale tak.`Include`</span><span class="sxs-lookup"><span data-stu-id="cddfc-185">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="cddfc-186">operátor u kolekcí.</span><span class="sxs-lookup"><span data-stu-id="cddfc-186">operator on collections.</span></span>

<span data-ttu-id="cddfc-187">Následující kód načte související data se `Select` metoda:</span><span class="sxs-lookup"><span data-stu-id="cddfc-187">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="cddfc-188">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="cddfc-188">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="cddfc-189">V tématu [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) a [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) kompletní příklad.</span><span class="sxs-lookup"><span data-stu-id="cddfc-189">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="cddfc-190">Vytvořit stránku vyučující, která zobrazuje kurzy a registrace</span><span class="sxs-lookup"><span data-stu-id="cddfc-190">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="cddfc-191">V této části se vytvoří vyučující stránky.</span><span class="sxs-lookup"><span data-stu-id="cddfc-191">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="cddfc-192">![Index stránky vyučující](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="cddfc-192">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="cddfc-193">Tato stránka načte a zobrazí související data následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="cddfc-193">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="cddfc-194">Zobrazí seznam vyučující souvisejících dat z `OfficeAssignment` entity (Office na předchozím obrázku).</span><span class="sxs-lookup"><span data-stu-id="cddfc-194">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="cddfc-195">`Instructor` a `OfficeAssignment` entity jsou ve vztahu-nula nebo 1.</span><span class="sxs-lookup"><span data-stu-id="cddfc-195">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="cddfc-196">Přes načítání se používá pro `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="cddfc-196">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="cddfc-197">Přes načítání je obvykle efektivnější, když související data se mají zobrazit.</span><span class="sxs-lookup"><span data-stu-id="cddfc-197">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="cddfc-198">V takovém případě se zobrazí přiřazení office pro vyučující.</span><span class="sxs-lookup"><span data-stu-id="cddfc-198">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="cddfc-199">Když uživatel vybere lektorem (Harui na předchozím obrázku), související `Course` entity jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="cddfc-199">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="cddfc-200">`Instructor` a `Course` jsou entit v relaci m: n.</span><span class="sxs-lookup"><span data-stu-id="cddfc-200">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="cddfc-201">Načítání pro nápovědy eager `Course` entity a jejich související `Department` entity se používá.</span><span class="sxs-lookup"><span data-stu-id="cddfc-201">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="cddfc-202">Samostatné dotazy v tomto případě může být efektivnější, protože jsou potřeba jenom kurzy pro vybrané lektorem.</span><span class="sxs-lookup"><span data-stu-id="cddfc-202">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="cddfc-203">Tento příklad ukazuje způsob použití přes načítání pro navigační vlastnosti v entity, které jsou v navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="cddfc-203">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="cddfc-204">Když uživatel vybere kurzu (chemie na předchozím obrázku), související data z `Enrollments` entity se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="cddfc-204">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="cddfc-205">Na předchozím obrázku se zobrazí název student a úrovni.</span><span class="sxs-lookup"><span data-stu-id="cddfc-205">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="cddfc-206">`Course` a `Enrollment` entity jsou v vztah jeden mnoho.</span><span class="sxs-lookup"><span data-stu-id="cddfc-206">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="cddfc-207">Vytvoření modelu zobrazení pro Index lektorem zobrazení</span><span class="sxs-lookup"><span data-stu-id="cddfc-207">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="cddfc-208">Stránka vyučující zobrazuje data ze tří různých tabulek.</span><span class="sxs-lookup"><span data-stu-id="cddfc-208">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="cddfc-209">Model zobrazení je vytvořen, který zahrnuje tři entity představující tři tabulky.</span><span class="sxs-lookup"><span data-stu-id="cddfc-209">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="cddfc-210">V *SchoolViewModels* složku vytvořit *InstructorIndexData.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="cddfc-210">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="cddfc-211">Vygenerované uživatelské rozhraní lektorem modelu</span><span class="sxs-lookup"><span data-stu-id="cddfc-211">Scaffold the Instructor model</span></span>

* <span data-ttu-id="cddfc-212">Ukončete aplikaci Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cddfc-212">Exit Visual Studio.</span></span>
* <span data-ttu-id="cddfc-213">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="cddfc-213">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="cddfc-214">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cddfc-214">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="cddfc-215">Předchozí příkaz scaffold `Instructor` modelu.</span><span class="sxs-lookup"><span data-stu-id="cddfc-215">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="cddfc-216">Otevřete projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cddfc-216">Open the project in Visual Studio.</span></span>

<span data-ttu-id="cddfc-217">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="cddfc-217">Build the project.</span></span> <span data-ttu-id="cddfc-218">Sestavení generuje chyby.</span><span class="sxs-lookup"><span data-stu-id="cddfc-218">The build generates errors.</span></span>

<span data-ttu-id="cddfc-219">Globálně změnit `_context.Instructor` k `_context.Instructors` (tedy "s" přidat do `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="cddfc-219">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="cddfc-220">7 výskytů jsou vyhledána a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="cddfc-220">7 occurrences are found and updated.</span></span>

<span data-ttu-id="cddfc-221">Spusťte aplikaci a přejděte na stránku vyučující.</span><span class="sxs-lookup"><span data-stu-id="cddfc-221">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="cddfc-222">Nahraďte *Pages/Instructors/Index.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="cddfc-222">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="cddfc-223">`OnGetAsync` Metoda přijímá data volitelné trasy pro ID vybrané lektorem.</span><span class="sxs-lookup"><span data-stu-id="cddfc-223">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="cddfc-224">Vyhledejte dotaz na *Pages/Instructors/Index.cshtml* stránky:</span><span class="sxs-lookup"><span data-stu-id="cddfc-224">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="cddfc-225">Dotaz má dva zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="cddfc-225">The query has two includes:</span></span>

* <span data-ttu-id="cddfc-226">`OfficeAssignment`: Zobrazených v [vyučující zobrazení](#IP).</span><span class="sxs-lookup"><span data-stu-id="cddfc-226">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="cddfc-227">`CourseAssignments`: Což přináší v rámci přípravy výukové.</span><span class="sxs-lookup"><span data-stu-id="cddfc-227">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="cddfc-228">Aktualizace indexovou stránku vyučující</span><span class="sxs-lookup"><span data-stu-id="cddfc-228">Update the instructors Index page</span></span>

<span data-ttu-id="cddfc-229">Aktualizace *Pages/Instructors/Index.cshtml* s následující kód:</span><span class="sxs-lookup"><span data-stu-id="cddfc-229">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="cddfc-230">Předchozí kód provede tyto změny:</span><span class="sxs-lookup"><span data-stu-id="cddfc-230">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="cddfc-231">Aktualizace `page` direktivy z `@page` k `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="cddfc-231">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="cddfc-232">`"{id:int?}"`je šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="cddfc-232">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="cddfc-233">Šablona trasy změny řetězce dotazu celé číslo v adrese URL data trasy.</span><span class="sxs-lookup"><span data-stu-id="cddfc-233">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="cddfc-234">Například kliknete na **vyberte** odkaz lektorem při page – direktiva vytvoří adresu URL podobnou následující:</span><span class="sxs-lookup"><span data-stu-id="cddfc-234">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="cddfc-235">Pokud se jedná o direktivu stránky `@page "{id:int?}"`, je předchozí adresa URL:</span><span class="sxs-lookup"><span data-stu-id="cddfc-235">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="cddfc-236">Název stránky je **vyučující**.</span><span class="sxs-lookup"><span data-stu-id="cddfc-236">Page title is **Instructors**.</span></span>
* <span data-ttu-id="cddfc-237">Přidat **Office** sloupec, který zobrazuje `item.OfficeAssignment.Location` pouze v případě `item.OfficeAssignment` není null.</span><span class="sxs-lookup"><span data-stu-id="cddfc-237">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="cddfc-238">Protože tato relace-nula nebo 1, nemusí být související entity OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="cddfc-238">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="cddfc-239">Přidat **kurzy** sloupec, který zobrazuje kurzy výukové podle jednotlivých lektorem.</span><span class="sxs-lookup"><span data-stu-id="cddfc-239">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="cddfc-240">V tématu [explicitní přechod na řádek s `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) Další informace o tuto syntaxi razor.</span><span class="sxs-lookup"><span data-stu-id="cddfc-240">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="cddfc-241">Přidání kódu, která dynamicky přidá `class="success"` k `tr` element vybrané lektorem.</span><span class="sxs-lookup"><span data-stu-id="cddfc-241">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="cddfc-242">Toto nastaví barvu pozadí vybraného řádku použití Bootstrap třídy.</span><span class="sxs-lookup"><span data-stu-id="cddfc-242">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="cddfc-243">Přidat nový hypertextový odkaz s názvem bez přípony **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="cddfc-243">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="cddfc-244">Tento odkaz odešle vybraný lektorem ID, který má `Index` metoda a nastaví barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="cddfc-244">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="cddfc-245">Spusťte aplikaci a vyberte **vyučující** kartě. Na stránce se zobrazuje `Location` (office) z související `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="cddfc-245">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="cddfc-246">Pokud OfficeAssignment' je null, se zobrazí buňky prázdná tabulka.</span><span class="sxs-lookup"><span data-stu-id="cddfc-246">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Vyučující indexovou stránku nic zvolené](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="cddfc-248">Klikněte na **vyberte** odkaz.</span><span class="sxs-lookup"><span data-stu-id="cddfc-248">Click on the **Select** link.</span></span> <span data-ttu-id="cddfc-249">Změny styl řádku.</span><span class="sxs-lookup"><span data-stu-id="cddfc-249">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="cddfc-250">Přidat kurzy výukové podle vybrané lektorem</span><span class="sxs-lookup"><span data-stu-id="cddfc-250">Add courses taught by selected instructor</span></span>

<span data-ttu-id="cddfc-251">Aktualizace `OnGetAsync` metoda v *Pages/Instructors/Index.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="cddfc-251">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="cddfc-252">Zkontrolujte aktualizované dotazu:</span><span class="sxs-lookup"><span data-stu-id="cddfc-252">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="cddfc-253">Přidá předchozího dotazu `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="cddfc-253">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="cddfc-254">Následující kód provede, když je vybrána lektorem (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="cddfc-254">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="cddfc-255">Vybrané lektorem se načítají ze seznamu vyučující v zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="cddfc-255">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="cddfc-256">Model zobrazení `Courses` vlastnosti je načtena s `Course` entity z této lektorem `CourseAssignments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="cddfc-256">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="cddfc-257">`Where` Metoda vrátí kolekci.</span><span class="sxs-lookup"><span data-stu-id="cddfc-257">The `Where` method returns a collection.</span></span> <span data-ttu-id="cddfc-258">V předchozím `Where` metoda jediným `Instructor` se vrátí entity.</span><span class="sxs-lookup"><span data-stu-id="cddfc-258">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="cddfc-259">`Single` Metoda převede kolekci do jednoho `Instructor` entity.</span><span class="sxs-lookup"><span data-stu-id="cddfc-259">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="cddfc-260">`Instructor` Entity poskytuje přístup k `CourseAssignments` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="cddfc-260">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="cddfc-261">`CourseAssignments`poskytuje přístup k související `Course` entity.</span><span class="sxs-lookup"><span data-stu-id="cddfc-261">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![M:M lektorem kurzy](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="cddfc-263">`Single` Metoda se používá na kolekci, pokud kolekce obsahuje pouze jednu položku.</span><span class="sxs-lookup"><span data-stu-id="cddfc-263">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="cddfc-264">`Single` Metoda vyvolá výjimku, pokud je kolekce prázdná, nebo pokud existuje více než jednu položku.</span><span class="sxs-lookup"><span data-stu-id="cddfc-264">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="cddfc-265">Alternativou je `SingleOrDefault`, která vrací výchozí hodnotu (null v tomto případě) Pokud je kolekce prázdná.</span><span class="sxs-lookup"><span data-stu-id="cddfc-265">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="cddfc-266">Pomocí `SingleOrDefault` na prázdnou kolekci:</span><span class="sxs-lookup"><span data-stu-id="cddfc-266">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="cddfc-267">Výsledkem výjimku (z pokusu o vyhledání `Courses` vlastnost na hodnotu Null).</span><span class="sxs-lookup"><span data-stu-id="cddfc-267">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="cddfc-268">Zpráva o výjimce by méně jasně ukazovat na příčinu problému.</span><span class="sxs-lookup"><span data-stu-id="cddfc-268">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="cddfc-269">Následující kód naplní model zobrazení `Enrollments` vlastnost, pokud je vybrána kurzu:</span><span class="sxs-lookup"><span data-stu-id="cddfc-269">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="cddfc-270">Přidejte následující kód do konce *Pages/Courses/Index.cshtml* Razor stránky:</span><span class="sxs-lookup"><span data-stu-id="cddfc-270">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="cddfc-271">Předchozí kód zobrazí seznam kurzů související s lektorem, pokud je vybrána lektorem.</span><span class="sxs-lookup"><span data-stu-id="cddfc-271">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="cddfc-272">Testování aplikací.</span><span class="sxs-lookup"><span data-stu-id="cddfc-272">Test the app.</span></span> <span data-ttu-id="cddfc-273">Klikněte na **vyberte** na stránce vyučující odkaz.</span><span class="sxs-lookup"><span data-stu-id="cddfc-273">Click on a **Select** link on the instructors page.</span></span>

![Vyučující Index stránky lektorem vybrané](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="cddfc-275">Zobrazit data pro studenty</span><span class="sxs-lookup"><span data-stu-id="cddfc-275">Show student data</span></span>

<span data-ttu-id="cddfc-276">V této části je aktualizována aplikace na student data pro vybrané kurzu.</span><span class="sxs-lookup"><span data-stu-id="cddfc-276">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="cddfc-277">Aktualizace dotazu ve `OnGetAsync` metoda v *Pages/Instructors/Index.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="cddfc-277">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="cddfc-278">Aktualizace *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cddfc-278">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="cddfc-279">Na konec souboru přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="cddfc-279">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="cddfc-280">Předchozí kód zobrazí seznam studenty, kteří jsou zaregistrované v vybraný kurz.</span><span class="sxs-lookup"><span data-stu-id="cddfc-280">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="cddfc-281">Aktualizujte stránku a vyberte lektorem.</span><span class="sxs-lookup"><span data-stu-id="cddfc-281">Refresh the page and select an instructor.</span></span> <span data-ttu-id="cddfc-282">Vyberte kurz chcete zobrazit seznam registrovaných studenty a jejich tříd.</span><span class="sxs-lookup"><span data-stu-id="cddfc-282">Select a course to see the list of enrolled students and their grades.</span></span>

![Vyučující Index stránky lektorem a kurzu vybrán](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="cddfc-284">Pomocí jednoho</span><span class="sxs-lookup"><span data-stu-id="cddfc-284">Using Single</span></span>

<span data-ttu-id="cddfc-285">`Single` Metoda můžete předat `Where` podmínku namísto volání `Where` metoda samostatně:</span><span class="sxs-lookup"><span data-stu-id="cddfc-285">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="cddfc-286">Podle předchozích `Single` přístup poskytuje žádné výhod oproti použití `Where`.</span><span class="sxs-lookup"><span data-stu-id="cddfc-286">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="cddfc-287">Někteří vývojáři raději `Single` přístupu stylu.</span><span class="sxs-lookup"><span data-stu-id="cddfc-287">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="cddfc-288">explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="cddfc-288">Explicit loading</span></span>

<span data-ttu-id="cddfc-289">Určuje aktuálního kódu přes načítání pro `Enrollments` a `Students`:</span><span class="sxs-lookup"><span data-stu-id="cddfc-289">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="cddfc-290">Předpokládejme, že uživatelé chtějí zřídka najdete v části registrace v kurzu.</span><span class="sxs-lookup"><span data-stu-id="cddfc-290">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="cddfc-291">V takovém případě optimalizace bude pouze načíst data zápisu, pokud se vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="cddfc-291">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="cddfc-292">V této části `OnGetAsync` je aktualizovat, a použít explicitní načítání `Enrollments` a `Students`.</span><span class="sxs-lookup"><span data-stu-id="cddfc-292">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="cddfc-293">Aktualizace `OnGetAsync` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="cddfc-293">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="cddfc-294">Předchozí kód zahodí *ThenInclude* metoda volá pro registraci a student data.</span><span class="sxs-lookup"><span data-stu-id="cddfc-294">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="cddfc-295">Pokud je vybrána kurzu, načte zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="cddfc-295">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="cddfc-296">`Enrollment` Entity pro vybraný kurz.</span><span class="sxs-lookup"><span data-stu-id="cddfc-296">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="cddfc-297">`Student` Entity pro každou `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="cddfc-297">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="cddfc-298">Všimněte si předchozí kód komentáře se `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="cddfc-298">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="cddfc-299">Navigační vlastnosti mohou být explicitně načteny pouze u sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="cddfc-299">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="cddfc-300">Testování aplikací.</span><span class="sxs-lookup"><span data-stu-id="cddfc-300">Test the app.</span></span> <span data-ttu-id="cddfc-301">Z hlediska uživatelů aplikace se chová stejně jako se jeho předchozí verze.</span><span class="sxs-lookup"><span data-stu-id="cddfc-301">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="cddfc-302">Další kurz ukazuje, jak aktualizovat data v relaci.</span><span class="sxs-lookup"><span data-stu-id="cddfc-302">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="cddfc-303">[Předchozí](xref:data/ef-rp/complex-data-model)
>[další](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="cddfc-303">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>