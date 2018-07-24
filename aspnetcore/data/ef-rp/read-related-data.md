---
title: Stránky Razor s EF Core v ASP.NET Core – čtení souvisejících dat – 6 8
author: rick-anderson
description: V tomto kurzu čtení a zobrazení souvisejících dat – to znamená, že data, která načte Entity Framework do navigační vlastnosti.
ms.author: riande
ms.date: 11/05/2017
uid: data/ef-rp/read-related-data
ms.openlocfilehash: bb1d087a5449c6e26c40e572d161dd9644ac2323
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219339"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="855a0-103">Stránky Razor s EF Core v ASP.NET Core – čtení souvisejících dat – 6 8</span><span class="sxs-lookup"><span data-stu-id="855a0-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="855a0-104">Podle [Petr Dykstra](https://github.com/tdykstra), [Jan Macek P](https://twitter.com/thereformedprog), a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="855a0-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="855a0-105">V tomto kurzu související data načíst a zobrazit.</span><span class="sxs-lookup"><span data-stu-id="855a0-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="855a0-106">Související data jsou data, která načte EF Core do navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="855a0-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="855a0-107">Pokud narazíte na potíže nelze vyřešit, stáhněte si [dokončené aplikace pro tuto fázi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="855a0-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="855a0-108">Dokončené stránky pro účely tohoto kurzu na následujících obrázcích:</span><span class="sxs-lookup"><span data-stu-id="855a0-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Kurzy indexová stránka](read-related-data/_static/courses-index.png)

![Instruktoři indexová stránka](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="855a0-111">Nemůžou dočkat, až explicitní a opožděné načtení souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="855a0-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="855a0-112">Existuje několik způsobů, EF Core můžete načíst související data do navigační vlastnosti entity:</span><span class="sxs-lookup"><span data-stu-id="855a0-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="855a0-113">[Předběžné načítání](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="855a0-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="855a0-114">Předběžné načítání je při dotazu na jeden typ entity také načtení souvisejících entit.</span><span class="sxs-lookup"><span data-stu-id="855a0-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="855a0-115">Při čtení je entita, související data načtena.</span><span class="sxs-lookup"><span data-stu-id="855a0-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="855a0-116">Obvykle v důsledku jednoho spojení dotaz, který zkopíruje všechna data, který je nezbytný.</span><span class="sxs-lookup"><span data-stu-id="855a0-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="855a0-117">EF Core vydá pro některé typy nemůžou dočkat, až načítání více dotazů.</span><span class="sxs-lookup"><span data-stu-id="855a0-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="855a0-118">Vydání více dotazů může být efektivnější než v případě u některých dotazů v EF6 tam, kde byla jeden dotaz.</span><span class="sxs-lookup"><span data-stu-id="855a0-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="855a0-119">Předběžné načítání je zadán s `Include` a `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="855a0-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Předběžné načítání příklad](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="855a0-121">Předběžné načítání odešle více dotazů, když je součástí navigace kolekce:</span><span class="sxs-lookup"><span data-stu-id="855a0-121">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="855a0-122">Jeden dotaz pro hlavní dotaz</span><span class="sxs-lookup"><span data-stu-id="855a0-122">One query for the main query</span></span> 
  * <span data-ttu-id="855a0-123">Jeden dotaz pro každou kolekci "edge" ve stromové struktuře zatížení.</span><span class="sxs-lookup"><span data-stu-id="855a0-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="855a0-124">Samostatné dotazy s `Load`: data můžete obnovit v samostatné dotazy a EF Core termín "opravy" navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="855a0-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="855a0-125">"opravy nahoru" znamená, že EF Core automaticky naplní navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="855a0-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="855a0-126">Samostatné dotazy s `Load` se víc explicitní načtení než předběžné načítání.</span><span class="sxs-lookup"><span data-stu-id="855a0-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![Příklad samostatné dotazy](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="855a0-128">Poznámka: EF Core automaticky opravuje vlastnosti navigace s jinými entitami, které byly dříve načtena do instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="855a0-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="855a0-129">I v případě, že jsou data pro navigační vlastnost *není* výslovně zahrnuty, vlastnost pořád naplněný, pokud některé nebo všechny související entity byly dříve načteny.</span><span class="sxs-lookup"><span data-stu-id="855a0-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="855a0-130">[Explicitní načtení](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="855a0-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="855a0-131">Pokud entita je nejdřív přečíst, související data nebude načten.</span><span class="sxs-lookup"><span data-stu-id="855a0-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="855a0-132">Načíst související data, když je potřeba, musí být kód zapsán.</span><span class="sxs-lookup"><span data-stu-id="855a0-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="855a0-133">Explicitní načtení pomocí samostatné dotazy za následek více dotazy odeslané do databáze.</span><span class="sxs-lookup"><span data-stu-id="855a0-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="855a0-134">S explicitní načtení, určuje kód navigačních vlastností, které mají být načteny.</span><span class="sxs-lookup"><span data-stu-id="855a0-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="855a0-135">Použití `Load` metodu explicitní načtení.</span><span class="sxs-lookup"><span data-stu-id="855a0-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="855a0-136">Příklad:</span><span class="sxs-lookup"><span data-stu-id="855a0-136">For example:</span></span>

  ![Příklad explicitní načtení](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="855a0-138">[Opožděné načtení](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="855a0-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="855a0-139">[EF Core v současné době nepodporuje opožděné načtení](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="855a0-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="855a0-140">Pokud entita je nejdřív přečíst, související data nebude načten.</span><span class="sxs-lookup"><span data-stu-id="855a0-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="855a0-141">Při prvním přístupu k vlastnosti navigace se automaticky načte data požadovaná pro tuto navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="855a0-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="855a0-142">Bude odeslán dotaz do databáze pokaždé, když vlastnost navigace pracuje poprvé.</span><span class="sxs-lookup"><span data-stu-id="855a0-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="855a0-143">`Select` Operátor načte pouze souvisejících dat, které jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="855a0-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="855a0-144">Vytvoření stránky kurzů, která zobrazuje název oddělení</span><span class="sxs-lookup"><span data-stu-id="855a0-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="855a0-145">Kurz entita obsahuje vlastnost navigace, která obsahuje `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="855a0-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="855a0-146">`Department` Oddělení, která je přiřazena kurz v entitě.</span><span class="sxs-lookup"><span data-stu-id="855a0-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="855a0-147">Chcete-li zobrazit název přiřazený oddělení v seznamu kurzů:</span><span class="sxs-lookup"><span data-stu-id="855a0-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="855a0-148">Získejte `Name` vlastnost z `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="855a0-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="855a0-149">`Department` Entity pocházejí z `Course.Department` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="855a0-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse. Oddělení](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="855a0-151">Vygenerované uživatelské rozhraní modelu kurzu</span><span class="sxs-lookup"><span data-stu-id="855a0-151">Scaffold the Course model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="855a0-152">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="855a0-152">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="855a0-153">Postupujte podle pokynů v [generování uživatelského rozhraní modelu student](xref:data/ef-rp/intro#scaffold-the-student-model) a použít `Course` pro třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="855a0-153">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Course` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="855a0-154">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="855a0-154">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="855a0-155">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="855a0-155">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

------

<span data-ttu-id="855a0-156">Předchozí příkaz scaffold `Course` modelu.</span><span class="sxs-lookup"><span data-stu-id="855a0-156">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="855a0-157">Otevřete projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="855a0-157">Open the project in Visual Studio.</span></span>

<span data-ttu-id="855a0-158">Otevřít *Pages/Courses/Index.cshtml.cs* a prozkoumejte `OnGetAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="855a0-158">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="855a0-159">Předběžné načítání pro zadaný modul generování uživatelského rozhraní `Department` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="855a0-159">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="855a0-160">`Include` Metody určuje předběžné načítání.</span><span class="sxs-lookup"><span data-stu-id="855a0-160">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="855a0-161">Spusťte aplikaci a vyberte **kurzy** odkaz.</span><span class="sxs-lookup"><span data-stu-id="855a0-161">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="855a0-162">Zobrazí sloupec oddělení `DepartmentID`, který není užitečné.</span><span class="sxs-lookup"><span data-stu-id="855a0-162">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="855a0-163">Aktualizace `OnGetAsync` metodu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="855a0-163">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="855a0-164">Předchozí kód přidá `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="855a0-164">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="855a0-165">`AsNoTracking` zlepší výkon, protože nebudou pro účely vrácených entit.</span><span class="sxs-lookup"><span data-stu-id="855a0-165">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="855a0-166">Entity, které nejsou sledovat, protože se neaktualizují v rámci aktuálního kontextu.</span><span class="sxs-lookup"><span data-stu-id="855a0-166">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="855a0-167">Aktualizace *Pages/Courses/Index.cshtml* s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="855a0-167">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="855a0-168">Na automaticky generovaný kód se provedly následující změny:</span><span class="sxs-lookup"><span data-stu-id="855a0-168">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="855a0-169">Změnit záhlaví od indexu ke kurzům.</span><span class="sxs-lookup"><span data-stu-id="855a0-169">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="855a0-170">Přidá **číslo** sloupec, který ukazuje `CourseID` hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="855a0-170">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="855a0-171">Ve výchozím nastavení nejsou automaticky generovaný primární klíče, protože jsou obvykle nemá význam pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="855a0-171">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="855a0-172">Ale v tomto případě primární klíč má smysl.</span><span class="sxs-lookup"><span data-stu-id="855a0-172">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="855a0-173">Změnit **oddělení** sloupec, který se zobrazí název oddělení.</span><span class="sxs-lookup"><span data-stu-id="855a0-173">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="855a0-174">Zobrazení kódu `Name` vlastnost `Department` entity, který je načten do `Department` navigační vlastnost:</span><span class="sxs-lookup"><span data-stu-id="855a0-174">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="855a0-175">Spusťte aplikaci a vyberte **kurzy** kartu pro zobrazení seznamu s názvy oddělení.</span><span class="sxs-lookup"><span data-stu-id="855a0-175">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kurzy indexová stránka](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="855a0-177">Načítají se související data s vybranými</span><span class="sxs-lookup"><span data-stu-id="855a0-177">Loading related data with Select</span></span>

<span data-ttu-id="855a0-178">`OnGetAsync` Metoda načte související data se `Include` metody:</span><span class="sxs-lookup"><span data-stu-id="855a0-178">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="855a0-179">`Select` Operátor načte pouze souvisejících dat, které jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="855a0-179">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="855a0-180">Pro jednotlivé položky jako `Department.Name` používá SQL INNER JOIN.</span><span class="sxs-lookup"><span data-stu-id="855a0-180">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="855a0-181">Pro kolekce, používá jiný přístup k databázi, ale postupy zločinců `Include` operátor v kolekcích.</span><span class="sxs-lookup"><span data-stu-id="855a0-181">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="855a0-182">Následující kód načte data související s `Select` metody:</span><span class="sxs-lookup"><span data-stu-id="855a0-182">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="855a0-183">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="855a0-183">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="855a0-184">Zobrazit [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) a [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) kompletní příklad.</span><span class="sxs-lookup"><span data-stu-id="855a0-184">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="855a0-185">Vytvoření stránky školitelů, který ukazuje, kurzy a registrace</span><span class="sxs-lookup"><span data-stu-id="855a0-185">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="855a0-186">V této části se vytvoří Instruktoři stránka.</span><span class="sxs-lookup"><span data-stu-id="855a0-186">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="855a0-187">![Instruktoři indexová stránka](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="855a0-187">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="855a0-188">Tato stránka načte a zobrazí související data následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="855a0-188">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="855a0-189">V seznamu instruktorů zobrazí související data z `OfficeAssignment` entity (Office na předchozím obrázku).</span><span class="sxs-lookup"><span data-stu-id="855a0-189">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="855a0-190">`Instructor` a `OfficeAssignment` entity jsou ve vztahu k nule nebo jednom.</span><span class="sxs-lookup"><span data-stu-id="855a0-190">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="855a0-191">Předběžné načítání se používá pro `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="855a0-191">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="855a0-192">Předběžné načítání je obvykle efektivnější, pokud je potřeba zobrazí související data.</span><span class="sxs-lookup"><span data-stu-id="855a0-192">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="855a0-193">V takovém případě přiřazení sady office pro Instruktoři zobrazují.</span><span class="sxs-lookup"><span data-stu-id="855a0-193">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="855a0-194">Když uživatel vybere instruktorem (Harui na předchozím obrázku), související `Course` entity jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="855a0-194">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="855a0-195">`Instructor` a `Course` entity jsou v relaci m: m.</span><span class="sxs-lookup"><span data-stu-id="855a0-195">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="855a0-196">Předběžné načítání se používá pro `Course` entit a jejich související `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="855a0-196">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="855a0-197">Samostatné dotazy v tomto případě může být mnohem efektivnější, protože jsou potřeba jenom kurzů pro vybranou instruktorem.</span><span class="sxs-lookup"><span data-stu-id="855a0-197">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="855a0-198">Tento příklad ukazuje způsob použití předběžné načítání pro navigační vlastnosti v entitách, které jsou v navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="855a0-198">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="855a0-199">Když uživatel vybere kurzu (chemie na předchozím obrázku), související data z `Enrollments` entity se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="855a0-199">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="855a0-200">Na předchozím obrázku jsou zobrazeny jméno studenta a na podnikové úrovni.</span><span class="sxs-lookup"><span data-stu-id="855a0-200">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="855a0-201">`Course` a `Enrollment` entity jsou v vztah jeden mnoho.</span><span class="sxs-lookup"><span data-stu-id="855a0-201">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="855a0-202">Vytvoření modelu zobrazení pro zobrazení indexu instruktorem</span><span class="sxs-lookup"><span data-stu-id="855a0-202">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="855a0-203">Na stránce Instruktoři zobrazuje data ze tří různých tabulek.</span><span class="sxs-lookup"><span data-stu-id="855a0-203">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="855a0-204">Zobrazení modelu je vytvořen, která obsahuje tři entity představující tři tabulky.</span><span class="sxs-lookup"><span data-stu-id="855a0-204">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="855a0-205">V *SchoolViewModels* složku, vytvořte *InstructorIndexData.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="855a0-205">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="855a0-206">Vygenerované uživatelské rozhraní modelu instruktorem</span><span class="sxs-lookup"><span data-stu-id="855a0-206">Scaffold the Instructor model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="855a0-207">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="855a0-207">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="855a0-208">Postupujte podle pokynů v [generování uživatelského rozhraní modelu student](xref:data/ef-rp/intro#scaffold-the-student-model) a použít `Instructor` pro třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="855a0-208">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Instructor` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="855a0-209">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="855a0-209">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="855a0-210">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="855a0-210">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

------

<span data-ttu-id="855a0-211">Předchozí příkaz scaffold `Instructor` modelu.</span><span class="sxs-lookup"><span data-stu-id="855a0-211">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="855a0-212">Spusťte aplikaci a přejděte na stránku Instruktoři.</span><span class="sxs-lookup"><span data-stu-id="855a0-212">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="855a0-213">Nahraďte *Pages/Instructors/Index.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="855a0-213">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="855a0-214">`OnGetAsync` Metoda přijímá data volitelné trasy pro ID vybrané instruktorem.</span><span class="sxs-lookup"><span data-stu-id="855a0-214">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="855a0-215">Prozkoumat dotaz *Pages/Instructors/Index.cshtml.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="855a0-215">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="855a0-216">Dotaz má dvě zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="855a0-216">The query has two includes:</span></span>

* <span data-ttu-id="855a0-217">`OfficeAssignment`: Zobrazených v [Instruktoři zobrazení](#IP).</span><span class="sxs-lookup"><span data-stu-id="855a0-217">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="855a0-218">`CourseAssignments`: Což přináší výukové kurzy.</span><span class="sxs-lookup"><span data-stu-id="855a0-218">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="855a0-219">Aktualizace Instruktoři indexovou stránku</span><span class="sxs-lookup"><span data-stu-id="855a0-219">Update the instructors Index page</span></span>

<span data-ttu-id="855a0-220">Aktualizace *Pages/Instructors/Index.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="855a0-220">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="855a0-221">Předchozí kód provede následující změny:</span><span class="sxs-lookup"><span data-stu-id="855a0-221">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="855a0-222">Aktualizace `page` direktiv z `@page` k `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="855a0-222">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="855a0-223">`"{id:int?}"` je šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="855a0-223">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="855a0-224">Šablona trasy změny celé číslo řetězce dotazu v adrese URL data trasy.</span><span class="sxs-lookup"><span data-stu-id="855a0-224">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="855a0-225">Například, že kliknete na **vyberte** instruktorem s pouze odkaz `@page` – direktiva vytvoří adresu URL podobnou následující:</span><span class="sxs-lookup"><span data-stu-id="855a0-225">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="855a0-226">Po direktivě stránky `@page "{id:int?}"`, předchozí adresa URL je:</span><span class="sxs-lookup"><span data-stu-id="855a0-226">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="855a0-227">Je název stránky **Instruktoři**.</span><span class="sxs-lookup"><span data-stu-id="855a0-227">Page title is **Instructors**.</span></span>
* <span data-ttu-id="855a0-228">Přidá **Office** sloupec, který zobrazuje `item.OfficeAssignment.Location` pouze tehdy, pokud `item.OfficeAssignment` není null.</span><span class="sxs-lookup"><span data-stu-id="855a0-228">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="855a0-229">Protože je to vztah jeden: nula nebo 1, nemusí být související entita OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="855a0-229">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="855a0-230">Přidá **kurzy** sloupec, který zobrazuje kurzy vedené jednotlivých kurzů vedených.</span><span class="sxs-lookup"><span data-stu-id="855a0-230">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="855a0-231">Zobrazit [explicitní přechod na řádek s `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) Další informace o tuto syntaxi razor.</span><span class="sxs-lookup"><span data-stu-id="855a0-231">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="855a0-232">Přidání kódu, která dynamicky přidá `class="success"` k `tr` element vybrané instruktorem.</span><span class="sxs-lookup"><span data-stu-id="855a0-232">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="855a0-233">Tím se nastaví barvu pozadí vybraného řádku pomocí Bootstrap třídy.</span><span class="sxs-lookup"><span data-stu-id="855a0-233">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="855a0-234">Přidat nový popisek hypertextový odkaz **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="855a0-234">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="855a0-235">Tento odkaz odešle vybraný instruktorem ID `Index` metoda a nastaví barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="855a0-235">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="855a0-236">Spusťte aplikaci a vyberte **Instruktoři** kartu. Na stránce se zobrazí `Location` (office) z související `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="855a0-236">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="855a0-237">Pokud OfficeAssignment' je null, se zobrazí na prázdné tabulce buňku.</span><span class="sxs-lookup"><span data-stu-id="855a0-237">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Instruktoři indexovou stránku, kterou nevybere](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="855a0-239">Klikněte na **vyberte** odkaz.</span><span class="sxs-lookup"><span data-stu-id="855a0-239">Click on the **Select** link.</span></span> <span data-ttu-id="855a0-240">Změny stylu řádku.</span><span class="sxs-lookup"><span data-stu-id="855a0-240">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="855a0-241">Přidat kurzy vedené vybrané instruktorem</span><span class="sxs-lookup"><span data-stu-id="855a0-241">Add courses taught by selected instructor</span></span>

<span data-ttu-id="855a0-242">Aktualizace `OnGetAsync` metoda *Pages/Instructors/Index.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="855a0-242">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="855a0-243">Přidat `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="855a0-243">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="855a0-244">Zkontrolujte aktualizované dotazu:</span><span class="sxs-lookup"><span data-stu-id="855a0-244">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="855a0-245">Přidá předchozí dotaz `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="855a0-245">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="855a0-246">Následující kód provede při výběru instruktorem (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="855a0-246">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="855a0-247">Vybrané kurzů vedených je načten ze seznamu školitelů v modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="855a0-247">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="855a0-248">Model zobrazení `Courses` vlastnosti je načtena s `Course` entity z této kurzů vedených `CourseAssignments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="855a0-248">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="855a0-249">`Where` Metoda vrátí kolekci.</span><span class="sxs-lookup"><span data-stu-id="855a0-249">The `Where` method returns a collection.</span></span> <span data-ttu-id="855a0-250">V předchozím `Where` metody pouze jediný `Instructor` se vrátí entity.</span><span class="sxs-lookup"><span data-stu-id="855a0-250">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="855a0-251">`Single` Metoda převede kolekci do jednoho `Instructor` entity.</span><span class="sxs-lookup"><span data-stu-id="855a0-251">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="855a0-252">`Instructor` Entita poskytuje přístup k `CourseAssignments` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="855a0-252">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="855a0-253">`CourseAssignments` poskytuje přístup k související `Course` entity.</span><span class="sxs-lookup"><span data-stu-id="855a0-253">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![M:M kurzů vedených kurzy](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="855a0-255">`Single` Metoda se používá v kolekci, pokud kolekce obsahuje pouze jednu položku.</span><span class="sxs-lookup"><span data-stu-id="855a0-255">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="855a0-256">`Single` Metoda vyvolá výjimku, pokud kolekce je prázdná, nebo pokud existuje více než jednu položku.</span><span class="sxs-lookup"><span data-stu-id="855a0-256">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="855a0-257">Alternativou je `SingleOrDefault`, která vrátí výchozí hodnoty (null v tomto případě) Pokud kolekce je prázdná.</span><span class="sxs-lookup"><span data-stu-id="855a0-257">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="855a0-258">Pomocí `SingleOrDefault` na prázdnou kolekci:</span><span class="sxs-lookup"><span data-stu-id="855a0-258">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="855a0-259">Má za následek výjimku (z pokusu o nalezení `Courses` vlastnost na hodnotu Null).</span><span class="sxs-lookup"><span data-stu-id="855a0-259">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="855a0-260">Zpráva o výjimce by méně jasně ukazovat na příčinu problému.</span><span class="sxs-lookup"><span data-stu-id="855a0-260">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="855a0-261">Následující kód naplní model zobrazení `Enrollments` vlastnost při výběru kurzu:</span><span class="sxs-lookup"><span data-stu-id="855a0-261">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="855a0-262">Přidejte následující kód do konce *Pages/Instructors/Index.cshtml* stránky Razor:</span><span class="sxs-lookup"><span data-stu-id="855a0-262">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="855a0-263">Předchozí kód zobrazí seznam kurzů související s instruktorem, pokud je vybrána instruktorem.</span><span class="sxs-lookup"><span data-stu-id="855a0-263">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="855a0-264">Testování aplikace.</span><span class="sxs-lookup"><span data-stu-id="855a0-264">Test the app.</span></span> <span data-ttu-id="855a0-265">Klikněte na **vyberte** odkaz na stránce Instruktoři.</span><span class="sxs-lookup"><span data-stu-id="855a0-265">Click on a **Select** link on the instructors page.</span></span>

![Instruktoři Index stránky kurzů vedených vybrané](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="855a0-267">Zobrazení údajů studentů</span><span class="sxs-lookup"><span data-stu-id="855a0-267">Show student data</span></span>

<span data-ttu-id="855a0-268">V této části se aplikace aktualizuje a zobrazí student data pro vybrané kurzu.</span><span class="sxs-lookup"><span data-stu-id="855a0-268">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="855a0-269">Aktualizovat dotaz `OnGetAsync` metoda ve *Pages/Instructors/Index.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="855a0-269">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="855a0-270">Aktualizace *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="855a0-270">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="855a0-271">Na konec souboru přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="855a0-271">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="855a0-272">Předchozí kód zobrazí seznam studentů, kteří se zaregistrují ve vybraných kurzů.</span><span class="sxs-lookup"><span data-stu-id="855a0-272">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="855a0-273">Aktualizujte stránku a vybrat instruktorem.</span><span class="sxs-lookup"><span data-stu-id="855a0-273">Refresh the page and select an instructor.</span></span> <span data-ttu-id="855a0-274">Vyberte kurzu zobrazíte seznam registrovaná studentů a jejich kvality.</span><span class="sxs-lookup"><span data-stu-id="855a0-274">Select a course to see the list of enrolled students and their grades.</span></span>

![Kurzů vedených instruktory Index stránky a vybrali kurzu](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="855a0-276">Pomocí jednoho</span><span class="sxs-lookup"><span data-stu-id="855a0-276">Using Single</span></span>

<span data-ttu-id="855a0-277">`Single` Můžete předat metodě `Where` podmínku namísto volání metody `Where` metoda samostatně:</span><span class="sxs-lookup"><span data-stu-id="855a0-277">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="855a0-278">Předchozí `Single` přístup nabízí v porovnání s použitím žádné výhody `Where`.</span><span class="sxs-lookup"><span data-stu-id="855a0-278">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="855a0-279">Někteří vývojáři raději `Single` přistupovat ke stylu.</span><span class="sxs-lookup"><span data-stu-id="855a0-279">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="855a0-280">explicitní načtení</span><span class="sxs-lookup"><span data-stu-id="855a0-280">Explicit loading</span></span>

<span data-ttu-id="855a0-281">Aktuální kód určuje předběžné načítání pro `Enrollments` a `Students`:</span><span class="sxs-lookup"><span data-stu-id="855a0-281">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="855a0-282">Předpokládejme, že uživatelé chtějí zřídka naleznete v tématu registrace v kurzu.</span><span class="sxs-lookup"><span data-stu-id="855a0-282">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="855a0-283">Optimalizace v takovém případě by se pouze načíst data registrací. Pokud je požadováno.</span><span class="sxs-lookup"><span data-stu-id="855a0-283">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="855a0-284">V této části `OnGetAsync` je aktualizované, aby používaly explicitní načtení `Enrollments` a `Students`.</span><span class="sxs-lookup"><span data-stu-id="855a0-284">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="855a0-285">Aktualizace `OnGetAsync` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="855a0-285">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="855a0-286">Předchozí kód klesne *ThenInclude* metoda se volá pro registraci a studentů data.</span><span class="sxs-lookup"><span data-stu-id="855a0-286">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="855a0-287">Pokud je vybrána kurz, načte zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="855a0-287">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="855a0-288">`Enrollment` Entity pro vybraný kurz.</span><span class="sxs-lookup"><span data-stu-id="855a0-288">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="855a0-289">`Student` Entit pro každou `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="855a0-289">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="855a0-290">Všimněte si, že předchozí na komentářích ke kódu `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="855a0-290">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="855a0-291">Navigační vlastnosti lze explicitně načíst pouze u sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="855a0-291">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="855a0-292">Testování aplikace.</span><span class="sxs-lookup"><span data-stu-id="855a0-292">Test the app.</span></span> <span data-ttu-id="855a0-293">Z hlediska uživatelů aplikace chová stejně jako předchozí verze.</span><span class="sxs-lookup"><span data-stu-id="855a0-293">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="855a0-294">Další kurz ukazuje, jak aktualizovat související data.</span><span class="sxs-lookup"><span data-stu-id="855a0-294">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="855a0-295">[Předchozí](xref:data/ef-rp/complex-data-model)
>[další](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="855a0-295">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
