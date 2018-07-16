---
title: Stránky Razor s EF Core v ASP.NET Core – čtení souvisejících dat – 6 8
author: rick-anderson
description: V tomto kurzu čtení a zobrazení souvisejících dat – to znamená, že data, která načte Entity Framework do navigační vlastnosti.
ms.author: riande
ms.date: 11/05/2017
uid: data/ef-rp/read-related-data
ms.openlocfilehash: fa3147cc4ad121784911eef802e04ca91f16448f
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063309"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="16b54-103">Stránky Razor s EF Core v ASP.NET Core – čtení souvisejících dat – 6 8</span><span class="sxs-lookup"><span data-stu-id="16b54-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="16b54-104">Podle [Petr Dykstra](https://github.com/tdykstra), [Jan Macek P](https://twitter.com/thereformedprog), a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="16b54-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="16b54-105">V tomto kurzu související data načíst a zobrazit.</span><span class="sxs-lookup"><span data-stu-id="16b54-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="16b54-106">Související data jsou data, která načte EF Core do navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="16b54-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="16b54-107">Pokud narazíte na potíže nelze vyřešit, stáhněte si [dokončené aplikace pro tuto fázi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="16b54-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="16b54-108">Dokončené stránky pro účely tohoto kurzu na následujících obrázcích:</span><span class="sxs-lookup"><span data-stu-id="16b54-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Kurzy indexová stránka](read-related-data/_static/courses-index.png)

![Instruktoři indexová stránka](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="16b54-111">Nemůžou dočkat, až explicitní a opožděné načtení souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="16b54-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="16b54-112">Existuje několik způsobů, EF Core můžete načíst související data do navigační vlastnosti entity:</span><span class="sxs-lookup"><span data-stu-id="16b54-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="16b54-113">[Předběžné načítání](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="16b54-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="16b54-114">Předběžné načítání je při dotazu na jeden typ entity také načtení souvisejících entit.</span><span class="sxs-lookup"><span data-stu-id="16b54-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="16b54-115">Při čtení je entita, související data načtena.</span><span class="sxs-lookup"><span data-stu-id="16b54-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="16b54-116">Obvykle v důsledku jednoho spojení dotaz, který zkopíruje všechna data, který je nezbytný.</span><span class="sxs-lookup"><span data-stu-id="16b54-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="16b54-117">EF Core vydá pro některé typy nemůžou dočkat, až načítání více dotazů.</span><span class="sxs-lookup"><span data-stu-id="16b54-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="16b54-118">Vydání více dotazů může být efektivnější než v případě u některých dotazů v EF6 tam, kde byla jeden dotaz.</span><span class="sxs-lookup"><span data-stu-id="16b54-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="16b54-119">Předběžné načítání je zadán s `Include` a `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="16b54-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Předběžné načítání příklad](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="16b54-121">Předběžné načítání odešle více dotazů, když je součástí navigace kolekce:</span><span class="sxs-lookup"><span data-stu-id="16b54-121">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="16b54-122">Jeden dotaz pro hlavní dotaz</span><span class="sxs-lookup"><span data-stu-id="16b54-122">One query for the main query</span></span> 
  * <span data-ttu-id="16b54-123">Jeden dotaz pro každou kolekci "edge" ve stromové struktuře zatížení.</span><span class="sxs-lookup"><span data-stu-id="16b54-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="16b54-124">Samostatné dotazy s `Load`: data můžete obnovit v samostatné dotazy a EF Core termín "opravy" navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="16b54-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="16b54-125">"opravy nahoru" znamená, že EF Core automaticky naplní navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="16b54-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="16b54-126">Samostatné dotazy s `Load` se víc explicitní načtení než předběžné načítání.</span><span class="sxs-lookup"><span data-stu-id="16b54-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![Příklad samostatné dotazy](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="16b54-128">Poznámka: EF Core automaticky opravuje vlastnosti navigace s jinými entitami, které byly dříve načtena do instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="16b54-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="16b54-129">I v případě, že jsou data pro navigační vlastnost *není* výslovně zahrnuty, vlastnost pořád naplněný, pokud některé nebo všechny související entity byly dříve načteny.</span><span class="sxs-lookup"><span data-stu-id="16b54-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="16b54-130">[Explicitní načtení](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="16b54-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="16b54-131">Pokud entita je nejdřív přečíst, související data nebude načten.</span><span class="sxs-lookup"><span data-stu-id="16b54-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="16b54-132">Načíst související data, když je potřeba, musí být kód zapsán.</span><span class="sxs-lookup"><span data-stu-id="16b54-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="16b54-133">Explicitní načtení pomocí samostatné dotazy za následek více dotazy odeslané do databáze.</span><span class="sxs-lookup"><span data-stu-id="16b54-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="16b54-134">S explicitní načtení, určuje kód navigačních vlastností, které mají být načteny.</span><span class="sxs-lookup"><span data-stu-id="16b54-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="16b54-135">Použití `Load` metodu explicitní načtení.</span><span class="sxs-lookup"><span data-stu-id="16b54-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="16b54-136">Příklad:</span><span class="sxs-lookup"><span data-stu-id="16b54-136">For example:</span></span>

  ![Příklad explicitní načtení](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="16b54-138">[Opožděné načtení](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="16b54-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="16b54-139">[EF Core v současné době nepodporuje opožděné načtení](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="16b54-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="16b54-140">Pokud entita je nejdřív přečíst, související data nebude načten.</span><span class="sxs-lookup"><span data-stu-id="16b54-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="16b54-141">Při prvním přístupu k vlastnosti navigace se automaticky načte data požadovaná pro tuto navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="16b54-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="16b54-142">Bude odeslán dotaz do databáze pokaždé, když vlastnost navigace pracuje poprvé.</span><span class="sxs-lookup"><span data-stu-id="16b54-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="16b54-143">`Select` Operátor načte pouze souvisejících dat, které jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="16b54-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="16b54-144">Vytvoření stránky kurzů, která zobrazuje název oddělení</span><span class="sxs-lookup"><span data-stu-id="16b54-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="16b54-145">Kurz entita obsahuje vlastnost navigace, která obsahuje `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="16b54-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="16b54-146">`Department` Oddělení, která je přiřazena kurz v entitě.</span><span class="sxs-lookup"><span data-stu-id="16b54-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="16b54-147">Chcete-li zobrazit název přiřazený oddělení v seznamu kurzů:</span><span class="sxs-lookup"><span data-stu-id="16b54-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="16b54-148">Získejte `Name` vlastnost z `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="16b54-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="16b54-149">`Department` Entity pocházejí z `Course.Department` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="16b54-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse. Oddělení](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="16b54-151">Vygenerované uživatelské rozhraní modelu kurzu</span><span class="sxs-lookup"><span data-stu-id="16b54-151">Scaffold the Course model</span></span>

* <span data-ttu-id="16b54-152">Ukončení sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="16b54-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="16b54-153">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="16b54-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="16b54-154">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="16b54-154">Run the following command:</span></span>

  ```console
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

<span data-ttu-id="16b54-155">Předchozí příkaz scaffold `Course` modelu.</span><span class="sxs-lookup"><span data-stu-id="16b54-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="16b54-156">Otevřete projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="16b54-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="16b54-157">Otevřít *Pages/Courses/Index.cshtml.cs* a prozkoumejte `OnGetAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="16b54-157">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="16b54-158">Předběžné načítání pro zadaný modul generování uživatelského rozhraní `Department` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="16b54-158">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="16b54-159">`Include` Metody určuje předběžné načítání.</span><span class="sxs-lookup"><span data-stu-id="16b54-159">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="16b54-160">Spusťte aplikaci a vyberte **kurzy** odkaz.</span><span class="sxs-lookup"><span data-stu-id="16b54-160">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="16b54-161">Zobrazí sloupec oddělení `DepartmentID`, který není užitečné.</span><span class="sxs-lookup"><span data-stu-id="16b54-161">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="16b54-162">Aktualizace `OnGetAsync` metodu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="16b54-162">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="16b54-163">Předchozí kód přidá `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="16b54-163">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="16b54-164">`AsNoTracking` zlepší výkon, protože nebudou pro účely vrácených entit.</span><span class="sxs-lookup"><span data-stu-id="16b54-164">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="16b54-165">Entity, které nejsou sledovat, protože se neaktualizují v rámci aktuálního kontextu.</span><span class="sxs-lookup"><span data-stu-id="16b54-165">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="16b54-166">Aktualizace *Pages/Courses/Index.cshtml* s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="16b54-166">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="16b54-167">Na automaticky generovaný kód se provedly následující změny:</span><span class="sxs-lookup"><span data-stu-id="16b54-167">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="16b54-168">Změnit záhlaví od indexu ke kurzům.</span><span class="sxs-lookup"><span data-stu-id="16b54-168">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="16b54-169">Přidá **číslo** sloupec, který ukazuje `CourseID` hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="16b54-169">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="16b54-170">Ve výchozím nastavení nejsou automaticky generovaný primární klíče, protože jsou obvykle nemá význam pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="16b54-170">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="16b54-171">Ale v tomto případě primární klíč má smysl.</span><span class="sxs-lookup"><span data-stu-id="16b54-171">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="16b54-172">Změnit **oddělení** sloupec, který se zobrazí název oddělení.</span><span class="sxs-lookup"><span data-stu-id="16b54-172">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="16b54-173">Zobrazení kódu `Name` vlastnost `Department` entity, který je načten do `Department` navigační vlastnost:</span><span class="sxs-lookup"><span data-stu-id="16b54-173">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="16b54-174">Spusťte aplikaci a vyberte **kurzy** kartu pro zobrazení seznamu s názvy oddělení.</span><span class="sxs-lookup"><span data-stu-id="16b54-174">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kurzy indexová stránka](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="16b54-176">Načítají se související data s vybranými</span><span class="sxs-lookup"><span data-stu-id="16b54-176">Loading related data with Select</span></span>

<span data-ttu-id="16b54-177">`OnGetAsync` Metoda načte související data se `Include` metody:</span><span class="sxs-lookup"><span data-stu-id="16b54-177">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="16b54-178">`Select` Operátor načte pouze souvisejících dat, které jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="16b54-178">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="16b54-179">Pro jednotlivé položky jako `Department.Name` používá SQL INNER JOIN.</span><span class="sxs-lookup"><span data-stu-id="16b54-179">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="16b54-180">Pro kolekce, používá jiný přístup k databázi, ale postupy zločinců `Include` operátor v kolekcích.</span><span class="sxs-lookup"><span data-stu-id="16b54-180">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="16b54-181">Následující kód načte data související s `Select` metody:</span><span class="sxs-lookup"><span data-stu-id="16b54-181">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="16b54-182">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="16b54-182">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="16b54-183">Zobrazit [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) a [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) kompletní příklad.</span><span class="sxs-lookup"><span data-stu-id="16b54-183">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="16b54-184">Vytvoření stránky školitelů, který ukazuje, kurzy a registrace</span><span class="sxs-lookup"><span data-stu-id="16b54-184">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="16b54-185">V této části se vytvoří Instruktoři stránka.</span><span class="sxs-lookup"><span data-stu-id="16b54-185">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="16b54-186">![Instruktoři indexová stránka](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="16b54-186">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="16b54-187">Tato stránka načte a zobrazí související data následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="16b54-187">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="16b54-188">V seznamu instruktorů zobrazí související data z `OfficeAssignment` entity (Office na předchozím obrázku).</span><span class="sxs-lookup"><span data-stu-id="16b54-188">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="16b54-189">`Instructor` a `OfficeAssignment` entity jsou ve vztahu k nule nebo jednom.</span><span class="sxs-lookup"><span data-stu-id="16b54-189">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="16b54-190">Předběžné načítání se používá pro `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="16b54-190">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="16b54-191">Předběžné načítání je obvykle efektivnější, pokud je potřeba zobrazí související data.</span><span class="sxs-lookup"><span data-stu-id="16b54-191">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="16b54-192">V takovém případě přiřazení sady office pro Instruktoři zobrazují.</span><span class="sxs-lookup"><span data-stu-id="16b54-192">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="16b54-193">Když uživatel vybere instruktorem (Harui na předchozím obrázku), související `Course` entity jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="16b54-193">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="16b54-194">`Instructor` a `Course` entity jsou v relaci m: m.</span><span class="sxs-lookup"><span data-stu-id="16b54-194">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="16b54-195">Předběžné načítání se používá pro `Course` entit a jejich související `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="16b54-195">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="16b54-196">Samostatné dotazy v tomto případě může být mnohem efektivnější, protože jsou potřeba jenom kurzů pro vybranou instruktorem.</span><span class="sxs-lookup"><span data-stu-id="16b54-196">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="16b54-197">Tento příklad ukazuje způsob použití předběžné načítání pro navigační vlastnosti v entitách, které jsou v navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="16b54-197">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="16b54-198">Když uživatel vybere kurzu (chemie na předchozím obrázku), související data z `Enrollments` entity se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="16b54-198">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="16b54-199">Na předchozím obrázku jsou zobrazeny jméno studenta a na podnikové úrovni.</span><span class="sxs-lookup"><span data-stu-id="16b54-199">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="16b54-200">`Course` a `Enrollment` entity jsou v vztah jeden mnoho.</span><span class="sxs-lookup"><span data-stu-id="16b54-200">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="16b54-201">Vytvoření modelu zobrazení pro zobrazení indexu instruktorem</span><span class="sxs-lookup"><span data-stu-id="16b54-201">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="16b54-202">Na stránce Instruktoři zobrazuje data ze tří různých tabulek.</span><span class="sxs-lookup"><span data-stu-id="16b54-202">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="16b54-203">Zobrazení modelu je vytvořen, která obsahuje tři entity představující tři tabulky.</span><span class="sxs-lookup"><span data-stu-id="16b54-203">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="16b54-204">V *SchoolViewModels* složku, vytvořte *InstructorIndexData.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="16b54-204">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="16b54-205">Vygenerované uživatelské rozhraní modelu instruktorem</span><span class="sxs-lookup"><span data-stu-id="16b54-205">Scaffold the Instructor model</span></span>

* <span data-ttu-id="16b54-206">Ukončení sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="16b54-206">Exit Visual Studio.</span></span>
* <span data-ttu-id="16b54-207">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="16b54-207">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="16b54-208">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="16b54-208">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

<span data-ttu-id="16b54-209">Předchozí příkaz scaffold `Instructor` modelu.</span><span class="sxs-lookup"><span data-stu-id="16b54-209">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="16b54-210">Otevřete projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="16b54-210">Open the project in Visual Studio.</span></span>

<span data-ttu-id="16b54-211">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="16b54-211">Build the project.</span></span> <span data-ttu-id="16b54-212">Sestavení generuje chyby.</span><span class="sxs-lookup"><span data-stu-id="16b54-212">The build generates errors.</span></span>

<span data-ttu-id="16b54-213">Globálně změnit `_context.Instructor` k `_context.Instructors` (to znamená, přidejte k "s" `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="16b54-213">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="16b54-214">7 výskyty jsou vyhledána a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="16b54-214">7 occurrences are found and updated.</span></span>

<span data-ttu-id="16b54-215">Spusťte aplikaci a přejděte na stránku Instruktoři.</span><span class="sxs-lookup"><span data-stu-id="16b54-215">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="16b54-216">Nahraďte *Pages/Instructors/Index.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="16b54-216">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-99)]

<span data-ttu-id="16b54-217">`OnGetAsync` Metoda přijímá data volitelné trasy pro ID vybrané instruktorem.</span><span class="sxs-lookup"><span data-stu-id="16b54-217">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="16b54-218">Prozkoumat dotaz *Pages/Instructors/Index.cshtml.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="16b54-218">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="16b54-219">Dotaz má dvě zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="16b54-219">The query has two includes:</span></span>

* <span data-ttu-id="16b54-220">`OfficeAssignment`: Zobrazených v [Instruktoři zobrazení](#IP).</span><span class="sxs-lookup"><span data-stu-id="16b54-220">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="16b54-221">`CourseAssignments`: Což přináší výukové kurzy.</span><span class="sxs-lookup"><span data-stu-id="16b54-221">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="16b54-222">Aktualizace Instruktoři indexovou stránku</span><span class="sxs-lookup"><span data-stu-id="16b54-222">Update the instructors Index page</span></span>

<span data-ttu-id="16b54-223">Aktualizace *Pages/Instructors/Index.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="16b54-223">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="16b54-224">Předchozí kód provede následující změny:</span><span class="sxs-lookup"><span data-stu-id="16b54-224">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="16b54-225">Aktualizace `page` direktiv z `@page` k `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="16b54-225">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="16b54-226">`"{id:int?}"` je šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="16b54-226">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="16b54-227">Šablona trasy změny celé číslo řetězce dotazu v adrese URL data trasy.</span><span class="sxs-lookup"><span data-stu-id="16b54-227">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="16b54-228">Například, že kliknete na **vyberte** instruktorem s pouze odkaz `@page` – direktiva vytvoří adresu URL podobnou následující:</span><span class="sxs-lookup"><span data-stu-id="16b54-228">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="16b54-229">Po direktivě stránky `@page "{id:int?}"`, předchozí adresa URL je:</span><span class="sxs-lookup"><span data-stu-id="16b54-229">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="16b54-230">Je název stránky **Instruktoři**.</span><span class="sxs-lookup"><span data-stu-id="16b54-230">Page title is **Instructors**.</span></span>
* <span data-ttu-id="16b54-231">Přidá **Office** sloupec, který zobrazuje `item.OfficeAssignment.Location` pouze tehdy, pokud `item.OfficeAssignment` není null.</span><span class="sxs-lookup"><span data-stu-id="16b54-231">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="16b54-232">Protože je to vztah jeden: nula nebo 1, nemusí být související entita OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="16b54-232">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="16b54-233">Přidá **kurzy** sloupec, který zobrazuje kurzy vedené jednotlivých kurzů vedených.</span><span class="sxs-lookup"><span data-stu-id="16b54-233">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="16b54-234">Zobrazit [explicitní přechod na řádek s `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) Další informace o tuto syntaxi razor.</span><span class="sxs-lookup"><span data-stu-id="16b54-234">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="16b54-235">Přidání kódu, která dynamicky přidá `class="success"` k `tr` element vybrané instruktorem.</span><span class="sxs-lookup"><span data-stu-id="16b54-235">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="16b54-236">Tím se nastaví barvu pozadí vybraného řádku pomocí Bootstrap třídy.</span><span class="sxs-lookup"><span data-stu-id="16b54-236">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="16b54-237">Přidat nový popisek hypertextový odkaz **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="16b54-237">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="16b54-238">Tento odkaz odešle vybraný instruktorem ID `Index` metoda a nastaví barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="16b54-238">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="16b54-239">Spusťte aplikaci a vyberte **Instruktoři** kartu. Na stránce se zobrazí `Location` (office) z související `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="16b54-239">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="16b54-240">Pokud OfficeAssignment' je null, se zobrazí na prázdné tabulce buňku.</span><span class="sxs-lookup"><span data-stu-id="16b54-240">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Instruktoři indexovou stránku, kterou nevybere](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="16b54-242">Klikněte na **vyberte** odkaz.</span><span class="sxs-lookup"><span data-stu-id="16b54-242">Click on the **Select** link.</span></span> <span data-ttu-id="16b54-243">Změny stylu řádku.</span><span class="sxs-lookup"><span data-stu-id="16b54-243">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="16b54-244">Přidat kurzy vedené vybrané instruktorem</span><span class="sxs-lookup"><span data-stu-id="16b54-244">Add courses taught by selected instructor</span></span>

<span data-ttu-id="16b54-245">Aktualizace `OnGetAsync` metoda *Pages/Instructors/Index.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="16b54-245">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="16b54-246">Přidat `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="16b54-246">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="16b54-247">Zkontrolujte aktualizované dotazu:</span><span class="sxs-lookup"><span data-stu-id="16b54-247">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="16b54-248">Přidá předchozí dotaz `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="16b54-248">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="16b54-249">Následující kód provede při výběru instruktorem (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="16b54-249">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="16b54-250">Vybrané kurzů vedených je načten ze seznamu školitelů v modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="16b54-250">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="16b54-251">Model zobrazení `Courses` vlastnosti je načtena s `Course` entity z této kurzů vedených `CourseAssignments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="16b54-251">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="16b54-252">`Where` Metoda vrátí kolekci.</span><span class="sxs-lookup"><span data-stu-id="16b54-252">The `Where` method returns a collection.</span></span> <span data-ttu-id="16b54-253">V předchozím `Where` metody pouze jediný `Instructor` se vrátí entity.</span><span class="sxs-lookup"><span data-stu-id="16b54-253">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="16b54-254">`Single` Metoda převede kolekci do jednoho `Instructor` entity.</span><span class="sxs-lookup"><span data-stu-id="16b54-254">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="16b54-255">`Instructor` Entita poskytuje přístup k `CourseAssignments` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="16b54-255">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="16b54-256">`CourseAssignments` poskytuje přístup k související `Course` entity.</span><span class="sxs-lookup"><span data-stu-id="16b54-256">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![M:M kurzů vedených kurzy](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="16b54-258">`Single` Metoda se používá v kolekci, pokud kolekce obsahuje pouze jednu položku.</span><span class="sxs-lookup"><span data-stu-id="16b54-258">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="16b54-259">`Single` Metoda vyvolá výjimku, pokud kolekce je prázdná, nebo pokud existuje více než jednu položku.</span><span class="sxs-lookup"><span data-stu-id="16b54-259">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="16b54-260">Alternativou je `SingleOrDefault`, která vrátí výchozí hodnoty (null v tomto případě) Pokud kolekce je prázdná.</span><span class="sxs-lookup"><span data-stu-id="16b54-260">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="16b54-261">Pomocí `SingleOrDefault` na prázdnou kolekci:</span><span class="sxs-lookup"><span data-stu-id="16b54-261">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="16b54-262">Má za následek výjimku (z pokusu o nalezení `Courses` vlastnost na hodnotu Null).</span><span class="sxs-lookup"><span data-stu-id="16b54-262">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="16b54-263">Zpráva o výjimce by méně jasně ukazovat na příčinu problému.</span><span class="sxs-lookup"><span data-stu-id="16b54-263">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="16b54-264">Následující kód naplní model zobrazení `Enrollments` vlastnost při výběru kurzu:</span><span class="sxs-lookup"><span data-stu-id="16b54-264">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="16b54-265">Přidejte následující kód do konce *Pages/Instructors/Index.cshtml* stránky Razor:</span><span class="sxs-lookup"><span data-stu-id="16b54-265">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="16b54-266">Předchozí kód zobrazí seznam kurzů související s instruktorem, pokud je vybrána instruktorem.</span><span class="sxs-lookup"><span data-stu-id="16b54-266">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="16b54-267">Testování aplikace.</span><span class="sxs-lookup"><span data-stu-id="16b54-267">Test the app.</span></span> <span data-ttu-id="16b54-268">Klikněte na **vyberte** odkaz na stránce Instruktoři.</span><span class="sxs-lookup"><span data-stu-id="16b54-268">Click on a **Select** link on the instructors page.</span></span>

![Instruktoři Index stránky kurzů vedených vybrané](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="16b54-270">Zobrazení údajů studentů</span><span class="sxs-lookup"><span data-stu-id="16b54-270">Show student data</span></span>

<span data-ttu-id="16b54-271">V této části se aplikace aktualizuje a zobrazí student data pro vybrané kurzu.</span><span class="sxs-lookup"><span data-stu-id="16b54-271">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="16b54-272">Aktualizovat dotaz `OnGetAsync` metoda ve *Pages/Instructors/Index.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="16b54-272">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="16b54-273">Aktualizace *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="16b54-273">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="16b54-274">Na konec souboru přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="16b54-274">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="16b54-275">Předchozí kód zobrazí seznam studentů, kteří se zaregistrují ve vybraných kurzů.</span><span class="sxs-lookup"><span data-stu-id="16b54-275">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="16b54-276">Aktualizujte stránku a vybrat instruktorem.</span><span class="sxs-lookup"><span data-stu-id="16b54-276">Refresh the page and select an instructor.</span></span> <span data-ttu-id="16b54-277">Vyberte kurzu zobrazíte seznam registrovaná studentů a jejich kvality.</span><span class="sxs-lookup"><span data-stu-id="16b54-277">Select a course to see the list of enrolled students and their grades.</span></span>

![Kurzů vedených instruktory Index stránky a vybrali kurzu](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="16b54-279">Pomocí jednoho</span><span class="sxs-lookup"><span data-stu-id="16b54-279">Using Single</span></span>

<span data-ttu-id="16b54-280">`Single` Můžete předat metodě `Where` podmínku namísto volání metody `Where` metoda samostatně:</span><span class="sxs-lookup"><span data-stu-id="16b54-280">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="16b54-281">Předchozí `Single` přístup nabízí v porovnání s použitím žádné výhody `Where`.</span><span class="sxs-lookup"><span data-stu-id="16b54-281">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="16b54-282">Někteří vývojáři raději `Single` přistupovat ke stylu.</span><span class="sxs-lookup"><span data-stu-id="16b54-282">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="16b54-283">Explicitní načtení</span><span class="sxs-lookup"><span data-stu-id="16b54-283">Explicit loading</span></span>

<span data-ttu-id="16b54-284">Aktuální kód určuje předběžné načítání pro `Enrollments` a `Students`:</span><span class="sxs-lookup"><span data-stu-id="16b54-284">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="16b54-285">Předpokládejme, že uživatelé chtějí zřídka naleznete v tématu registrace v kurzu.</span><span class="sxs-lookup"><span data-stu-id="16b54-285">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="16b54-286">Optimalizace v takovém případě by se pouze načíst data registrací. Pokud je požadováno.</span><span class="sxs-lookup"><span data-stu-id="16b54-286">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="16b54-287">V této části `OnGetAsync` je aktualizované, aby používaly explicitní načtení `Enrollments` a `Students`.</span><span class="sxs-lookup"><span data-stu-id="16b54-287">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="16b54-288">Aktualizace `OnGetAsync` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="16b54-288">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="16b54-289">Předchozí kód klesne *ThenInclude* metoda se volá pro registraci a studentů data.</span><span class="sxs-lookup"><span data-stu-id="16b54-289">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="16b54-290">Pokud je vybrána kurz, načte zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="16b54-290">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="16b54-291">`Enrollment` Entity pro vybraný kurz.</span><span class="sxs-lookup"><span data-stu-id="16b54-291">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="16b54-292">`Student` Entit pro každou `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="16b54-292">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="16b54-293">Všimněte si, že předchozí na komentářích ke kódu `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="16b54-293">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="16b54-294">Navigační vlastnosti lze explicitně načíst pouze u sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="16b54-294">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="16b54-295">Testování aplikace.</span><span class="sxs-lookup"><span data-stu-id="16b54-295">Test the app.</span></span> <span data-ttu-id="16b54-296">Z hlediska uživatelů aplikace chová stejně jako předchozí verze.</span><span class="sxs-lookup"><span data-stu-id="16b54-296">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="16b54-297">Další kurz ukazuje, jak aktualizovat související data.</span><span class="sxs-lookup"><span data-stu-id="16b54-297">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="16b54-298">[Předchozí](xref:data/ef-rp/complex-data-model)
>[další](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="16b54-298">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
