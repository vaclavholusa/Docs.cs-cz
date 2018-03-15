---
title: "Stránky Razor s EF jádra ASP.NET Core - CRUD - 2 8"
author: rick-anderson
description: "Ukazuje, jak vytvořit, číst, aktualizovat, odstraňovat s EF jádra"
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/crud
ms.openlocfilehash: d46292e07b65322d85f8f0fb43c8be4af4ccb47e
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a><span data-ttu-id="3687c-103">Stránky Razor s EF jádra ASP.NET Core - CRUD - 2 8</span><span class="sxs-lookup"><span data-stu-id="3687c-103">Razor Pages with EF Core in ASP.NET Core - CRUD - 2 of 8</span></span>

<span data-ttu-id="3687c-104">Podle [tní Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3687c-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="3687c-105">V tomto kurzu vygenerované CRUD (vytvořit, číst, aktualizovat, odstraňovat) kód je zkontrolovat a upravit.</span><span class="sxs-lookup"><span data-stu-id="3687c-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="3687c-106">Poznámka: Minimalizovat složitost a zachovat tyto kurzy zaměřené na základní EF, EF základní kódu použít v modelech stránky stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="3687c-106">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages page models.</span></span> <span data-ttu-id="3687c-107">Někteří vývojáři použít k vytvoření abstraktní vrstvu mezi uživatelského rozhraní (Razor stránky) a vrstva přístupu k datům služby vrstvy nebo úložiště vzoru v.</span><span class="sxs-lookup"><span data-stu-id="3687c-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="3687c-108">V tomto kurzu, vytvořit, upravit, odstranit a stránky Razor podrobnosti v *Student* složky jsou upraveny.</span><span class="sxs-lookup"><span data-stu-id="3687c-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="3687c-109">Automaticky generovaný kód používá následující vzor pro Create, Edit a Delete stránky:</span><span class="sxs-lookup"><span data-stu-id="3687c-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="3687c-110">Získání a zobrazit požadovaná data pomocí metody GET protokolu HTTP `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="3687c-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="3687c-111">Uložit změny do data s metodou HTTP POST `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="3687c-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="3687c-112">Stránky indexu a podrobnosti o získání a jejich zobrazení požadovaná data pomocí metody GET protokolu HTTP `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="3687c-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="3687c-113">Nahraďte SingleOrDefaultAsync FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="3687c-113">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="3687c-114">Generovaný kód používá [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) načíst požadovaná entita.</span><span class="sxs-lookup"><span data-stu-id="3687c-114">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="3687c-115">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) je efektivnější v načítání jedna entita:</span><span class="sxs-lookup"><span data-stu-id="3687c-115">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="3687c-116">Pokud kód je potřeba ověřit, zda není více než jedna entita vrácená z dotazu.</span><span class="sxs-lookup"><span data-stu-id="3687c-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="3687c-117">`SingleOrDefaultAsync` načte více dat a nepotřebné funguje.</span><span class="sxs-lookup"><span data-stu-id="3687c-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="3687c-118">`SingleOrDefaultAsync` vyvolá výjimku, pokud existuje více než jedna entita, která odpovídá část filtru.</span><span class="sxs-lookup"><span data-stu-id="3687c-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="3687c-119">`FirstOrDefaultAsync` není výjimku, pokud existuje více než jedna entita, která odpovídá část filtru.</span><span class="sxs-lookup"><span data-stu-id="3687c-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<span data-ttu-id="3687c-120">Globálně nahradit `SingleOrDefaultAsync` s `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="3687c-120">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="3687c-121">`SingleOrDefaultAsync` se používá na 5 místech:</span><span class="sxs-lookup"><span data-stu-id="3687c-121">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="3687c-122">`OnGetAsync` na stránce Podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3687c-122">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="3687c-123">`OnGetAsync` a `OnPostAsync` na stránkách upravit a odstranit.</span><span class="sxs-lookup"><span data-stu-id="3687c-123">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="3687c-124">Asynchronně vyhledá</span><span class="sxs-lookup"><span data-stu-id="3687c-124">FindAsync</span></span>

<span data-ttu-id="3687c-125">Hodně automaticky generovaný kód [asynchronně vyhledá](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) lze místě `FirstOrDefaultAsync` nebo `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="3687c-125">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="3687c-126">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="3687c-126">`FindAsync`:</span></span>

* <span data-ttu-id="3687c-127">Vyhledá entitu s primární klíč (Primárníklíč).</span><span class="sxs-lookup"><span data-stu-id="3687c-127">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="3687c-128">Pokud entity s primárnímu Klíči je sledován pomocí kontextu, je vrácen bez požadavek do databáze.</span><span class="sxs-lookup"><span data-stu-id="3687c-128">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="3687c-129">Je jednoduchý a stručné sdělení.</span><span class="sxs-lookup"><span data-stu-id="3687c-129">Is simple and concise.</span></span>
* <span data-ttu-id="3687c-130">Je optimalizován pro vyhledání jedné entity.</span><span class="sxs-lookup"><span data-stu-id="3687c-130">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="3687c-131">Můžete mít výhody výkonu v některých situacích, ale zřídka se do play pro scénáře webového normální.</span><span class="sxs-lookup"><span data-stu-id="3687c-131">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="3687c-132">Implicitně používá [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) místo [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="3687c-132">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="3687c-133">Ale pokud chcete zahrnout ostatní entity, pak najít již není vhodné.</span><span class="sxs-lookup"><span data-stu-id="3687c-133">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="3687c-134">To znamená, že budete muset abandon najít a přesunout do dotazu průběhu vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3687c-134">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="3687c-135">Stránce s podrobnostmi o přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="3687c-135">Customize the Details page</span></span>

<span data-ttu-id="3687c-136">Přejděte do `Pages/Students` stránky.</span><span class="sxs-lookup"><span data-stu-id="3687c-136">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="3687c-137">**Upravit**, **podrobnosti**, a **odstranit** generované odkazy [pomocná značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) v *stránek/studenty / Index.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="3687c-137">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="3687c-138">Vyberte podrobnosti odkaz.</span><span class="sxs-lookup"><span data-stu-id="3687c-138">Select a Details link.</span></span> <span data-ttu-id="3687c-139">Adresa URL je ve formátu `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="3687c-139">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="3687c-140">Student ID je předán pomocí řetězce dotazu (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="3687c-140">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="3687c-141">Aktualizovat úpravy, podrobnosti a odstranit stránky Razor k použití `"{id:int}"` šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="3687c-141">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="3687c-142">Změňte direktivu stránky pro každou tyto stránek z `@page` k `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="3687c-142">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="3687c-143">Požadavek na stránku s šablonou cesty "{id: int}", která nemá **není** zahrnují celé číslo trasy hodnota vrátí protokolu HTTP (nenalezen) chybu 404.</span><span class="sxs-lookup"><span data-stu-id="3687c-143">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="3687c-144">Například `http://localhost:5000/Students/Details` vrátí chybu 404.</span><span class="sxs-lookup"><span data-stu-id="3687c-144">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="3687c-145">Chcete-li nastavit ID volitelný, připojte `?` pro dané omezení trasy:</span><span class="sxs-lookup"><span data-stu-id="3687c-145">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="3687c-146">Spusťte aplikaci, klikněte na odkaz podrobnosti a ověřte adresu URL je předávání ID jako data trasy (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="3687c-146">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="3687c-147">Neměnit globálně `@page` k `@page "{id:int}"`, provádění Ano zalomení odkazy na domovský uzel a vytvořte stránky.</span><span class="sxs-lookup"><span data-stu-id="3687c-147">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="3687c-148">Přidání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="3687c-148">Add related data</span></span>

<span data-ttu-id="3687c-149">Automaticky generovaný kód pro studenty indexovou stránku neobsahuje `Enrollments` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3687c-149">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="3687c-150">V této části, obsah `Enrollments` kolekce se zobrazí na stránce Podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3687c-150">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="3687c-151">`OnGetAsync` Metodu *Pages/Students/Details.cshtml.cs* používá `FirstOrDefaultAsync` metoda pro načtení jedné `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="3687c-151">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="3687c-152">Přidejte následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="3687c-152">Add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="3687c-153">`Include` a `ThenInclude` metody způsobit kontext k načtení `Student.Enrollments` navigační vlastnost a v rámci každé registrace `Enrollment.Course` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3687c-153">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="3687c-154">Tyto metody jsou podrobně v tomto kurzu data související s čtení.</span><span class="sxs-lookup"><span data-stu-id="3687c-154">These methods are examined in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="3687c-155">`AsNoTracking` Metoda zvyšuje výkon v situacích, když entity vrátil se neaktualizují v aktuálním kontextu.</span><span class="sxs-lookup"><span data-stu-id="3687c-155">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="3687c-156">`AsNoTracking` je popsána dále v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="3687c-156">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="3687c-157">Zobrazit související registrace na stránce podrobností</span><span class="sxs-lookup"><span data-stu-id="3687c-157">Display related enrollments on the Details page</span></span>

<span data-ttu-id="3687c-158">Otevřete *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3687c-158">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="3687c-159">Přidejte následující zvýrazněný kód zobrazíte seznam registrace:</span><span class="sxs-lookup"><span data-stu-id="3687c-159">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

<span data-ttu-id="3687c-160">Pokud odsazení kód je nesprávný po se vloží kód, stiskněte CTRL-tisíc-D opravte ho.</span><span class="sxs-lookup"><span data-stu-id="3687c-160">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="3687c-161">Předchozí kód prochází entity v `Enrollments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3687c-161">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="3687c-162">Pro každou registraci zobrazí název kurzu a úrovni.</span><span class="sxs-lookup"><span data-stu-id="3687c-162">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="3687c-163">Název postupu načten z kurzu entita, která je uložena v `Course` navigační vlastnost entity registrace.</span><span class="sxs-lookup"><span data-stu-id="3687c-163">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="3687c-164">Spuštění aplikace, vyberte **studenty** a klikněte **podrobnosti** odkaz pro student.</span><span class="sxs-lookup"><span data-stu-id="3687c-164">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="3687c-165">Zobrazí se seznam kurzů a tříd pro vybrané student.</span><span class="sxs-lookup"><span data-stu-id="3687c-165">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="3687c-166">Vytvořit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="3687c-166">Update the Create page</span></span>

<span data-ttu-id="3687c-167">Aktualizace `OnPostAsync` metoda v *Pages/Students/Create.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="3687c-167">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="3687c-168">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="3687c-168">TryUpdateModelAsync</span></span>

<span data-ttu-id="3687c-169">Zkontrolujte [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kódu:</span><span class="sxs-lookup"><span data-stu-id="3687c-169">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="3687c-170">V předchozí kód `TryUpdateModelAsync<Student>` pokusit o aktualizaci `emptyStudent` objektu z hodnot odeslaného formuláře [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) vlastnost v [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="3687c-170">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="3687c-171">`TryUpdateModelAsync` pouze aktualizace vlastností uvedených (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="3687c-171">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="3687c-172">V předchozím příkladu:</span><span class="sxs-lookup"><span data-stu-id="3687c-172">In the preceding sample:</span></span>

* <span data-ttu-id="3687c-173">Druhý argument (` "student", // Prefix`) je předponu používá pro vyhledávání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3687c-173">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="3687c-174">Není velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="3687c-174">It's not case sensitive.</span></span>
* <span data-ttu-id="3687c-175">Hodnoty odeslaného formuláře jsou převedeny na typy v `Student` model pomocí [model vazby](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="3687c-175">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="3687c-176">Overposting</span><span class="sxs-lookup"><span data-stu-id="3687c-176">Overposting</span></span>

<span data-ttu-id="3687c-177">Pomocí `TryUpdateModel` aktualizovat pole odeslaných hodnot je nejlepším postupem zabezpečení, protože zabraňuje overposting.</span><span class="sxs-lookup"><span data-stu-id="3687c-177">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="3687c-178">Předpokládejme například, zahrnuje Student entity `Secret` vlastnost, která tato webová stránka nesmí aktualizovat nebo přidat:</span><span class="sxs-lookup"><span data-stu-id="3687c-178">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="3687c-179">I v případě, že aplikace nemá `Secret` pole na vytvoření nebo aktualizaci stránky Razor, se hacker může nastavené `Secret` hodnoty overposting.</span><span class="sxs-lookup"><span data-stu-id="3687c-179">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="3687c-180">Hackerům může použijte například nástroj Fiddler, nebo si napsat určitého kódu JavaScript ke zveřejnění `Secret` formuláři hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3687c-180">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="3687c-181">Původní kód není omezit pole, která při vytváření Student instance používá vazač modelu.</span><span class="sxs-lookup"><span data-stu-id="3687c-181">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="3687c-182">Ať hacker zadaný pro hodnotu `Secret` pole formuláře je aktualizována v databázi.</span><span class="sxs-lookup"><span data-stu-id="3687c-182">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="3687c-183">Následující obrázek ukazuje přidání nástroj Fiddler `Secret` pole (s hodnotou "OverPost") na hodnoty odeslaného formuláře.</span><span class="sxs-lookup"><span data-stu-id="3687c-183">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Přidání tajný pole Fiddler](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="3687c-185">Hodnota "OverPost" úspěšně přidán do `Secret` vlastnost vloženého řádku.</span><span class="sxs-lookup"><span data-stu-id="3687c-185">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="3687c-186">Návrhář aplikace nikdy určená `Secret` vlastnost na stránce vytvořit nastavit.</span><span class="sxs-lookup"><span data-stu-id="3687c-186">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="3687c-187">Model zobrazení</span><span class="sxs-lookup"><span data-stu-id="3687c-187">View model</span></span>

<span data-ttu-id="3687c-188">Model zobrazení obvykle obsahuje podmnožinu vlastností obsažených v modelu používá aplikace.</span><span class="sxs-lookup"><span data-stu-id="3687c-188">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="3687c-189">Aplikační model se často nazývá modelu domény.</span><span class="sxs-lookup"><span data-stu-id="3687c-189">The application model is often called the domain model.</span></span> <span data-ttu-id="3687c-190">Model domény obvykle obsahuje všechny vlastnosti požadované odpovídající entita v databázi.</span><span class="sxs-lookup"><span data-stu-id="3687c-190">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="3687c-191">Model zobrazení obsahuje pouze vlastnosti, které jsou nutné pro vrstvu uživatelského rozhraní (například stránka vytvořit).</span><span class="sxs-lookup"><span data-stu-id="3687c-191">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="3687c-192">Kromě zobrazení modelu některé aplikace použít vazby modelu nebo vstupní modelu k předávání dat mezi třídy modelu stránky Razor stránky a v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="3687c-192">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="3687c-193">Vezměte v úvahu následující `Student` modelu zobrazení:</span><span class="sxs-lookup"><span data-stu-id="3687c-193">Consider the following `Student` view model:</span></span>

[!code-csharp[](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="3687c-194">Zobrazit modely poskytnout alternativní způsob zabránit overposting.</span><span class="sxs-lookup"><span data-stu-id="3687c-194">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="3687c-195">Model zobrazení obsahuje pouze vlastnosti zobrazení (zobrazení) a aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="3687c-195">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="3687c-196">Následující kód používá `StudentVM` zobrazení modelu vytvářet nové student:</span><span class="sxs-lookup"><span data-stu-id="3687c-196">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="3687c-197">[SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) metoda nastaví hodnoty tohoto objektu ve čtení hodnoty z jiné [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) objektu.</span><span class="sxs-lookup"><span data-stu-id="3687c-197">The [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="3687c-198">`SetValues` používá vlastnost s odpovídajícím názvem.</span><span class="sxs-lookup"><span data-stu-id="3687c-198">`SetValues` uses property name matching.</span></span> <span data-ttu-id="3687c-199">Typ modelu zobrazení nemusí být související s typem modelu, pouze musí mít vlastnosti, které odpovídají.</span><span class="sxs-lookup"><span data-stu-id="3687c-199">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="3687c-200">Pomocí `StudentVM` vyžaduje [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) aktualizovat, a použít `StudentVM` místo `Student`.</span><span class="sxs-lookup"><span data-stu-id="3687c-200">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="3687c-201">Na stránkách Razor `PageModel` odvozené třídy je zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="3687c-201">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="3687c-202">Upravit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="3687c-202">Update the Edit page</span></span>

<span data-ttu-id="3687c-203">Aktualizace modelu stránky pro stránku upravit.</span><span class="sxs-lookup"><span data-stu-id="3687c-203">Update the page model for the Edit page.</span></span> <span data-ttu-id="3687c-204">Jsou vyznačené hlavních změn:</span><span class="sxs-lookup"><span data-stu-id="3687c-204">The major changes are highlighted:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="3687c-205">Změny kódu jsou podobná stránce vytvořit s několika výjimkami:</span><span class="sxs-lookup"><span data-stu-id="3687c-205">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="3687c-206">`OnPostAsync` má volitelný `id` parametr.</span><span class="sxs-lookup"><span data-stu-id="3687c-206">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="3687c-207">Aktuální student jsou načtena z databáze, místo vytvoření prázdné student.</span><span class="sxs-lookup"><span data-stu-id="3687c-207">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="3687c-208">`FirstOrDefaultAsync` byla nahrazena [asynchronně vyhledá](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="3687c-208">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="3687c-209">`FindAsync` je vhodné použít při výběru entity z primární klíč.</span><span class="sxs-lookup"><span data-stu-id="3687c-209">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="3687c-210">V tématu [asynchronně vyhledá](#FindAsync) Další informace.</span><span class="sxs-lookup"><span data-stu-id="3687c-210">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="3687c-211">Testování úpravy a vytvářet stránky</span><span class="sxs-lookup"><span data-stu-id="3687c-211">Test the Edit and Create pages</span></span>

<span data-ttu-id="3687c-212">Vytvořit a upravit několik student entit.</span><span class="sxs-lookup"><span data-stu-id="3687c-212">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="3687c-213">Stav entity</span><span class="sxs-lookup"><span data-stu-id="3687c-213">Entity States</span></span>

<span data-ttu-id="3687c-214">Kontext databáze uchovává informace o, jestli jsou synchronizované s jejich odpovídající řádky v databázi entity v paměti.</span><span class="sxs-lookup"><span data-stu-id="3687c-214">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="3687c-215">Informace o databázi kontext synchronizace určuje, co se stane, když `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="3687c-215">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="3687c-216">Například když novou entitu je předána `Add` metoda, která je nastavena do stavu entity `Added`.</span><span class="sxs-lookup"><span data-stu-id="3687c-216">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="3687c-217">Když `SaveChanges` nazývá databáze kontextu problémy příkazu INSERT jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="3687c-217">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="3687c-218">Entita může být v jednom z následujících stavů:</span><span class="sxs-lookup"><span data-stu-id="3687c-218">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="3687c-219">`Added`: Entita ještě neexistuje v databázi.</span><span class="sxs-lookup"><span data-stu-id="3687c-219">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="3687c-220">`SaveChanges` Metoda problémy příkazu INSERT.</span><span class="sxs-lookup"><span data-stu-id="3687c-220">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="3687c-221">`Unchanged`: Žádné změny měly být uloženy s tuto entitu.</span><span class="sxs-lookup"><span data-stu-id="3687c-221">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="3687c-222">Entita nemá tento stav, pokud je pro čtení z databáze.</span><span class="sxs-lookup"><span data-stu-id="3687c-222">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="3687c-223">`Modified`: Bylo upraveno některé nebo všechny hodnoty vlastností entity.</span><span class="sxs-lookup"><span data-stu-id="3687c-223">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="3687c-224">`SaveChanges` Metoda problémy příkazu UPDATE.</span><span class="sxs-lookup"><span data-stu-id="3687c-224">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="3687c-225">`Deleted`: Entita byla označena pro odstranění.</span><span class="sxs-lookup"><span data-stu-id="3687c-225">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="3687c-226">`SaveChanges` Metoda problémy příkazech DELETE.</span><span class="sxs-lookup"><span data-stu-id="3687c-226">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="3687c-227">`Detached`: Entity není sledována aplikací kontext databáze.</span><span class="sxs-lookup"><span data-stu-id="3687c-227">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="3687c-228">V aplikace na ploše změny stavu se obvykle nastavuje automaticky.</span><span class="sxs-lookup"><span data-stu-id="3687c-228">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="3687c-229">Entita je pro čtení, změny a stav entity automaticky změnit tak, aby `Modified`.</span><span class="sxs-lookup"><span data-stu-id="3687c-229">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="3687c-230">Volání metody `SaveChanges` vytvoří prohlášení aktualizace SQL, který aktualizuje pouze změněné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3687c-230">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="3687c-231">Ve webové aplikaci `DbContext` entity a zobrazí data je zrušen po vykreslení stránky, který čte.</span><span class="sxs-lookup"><span data-stu-id="3687c-231">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="3687c-232">Pokud na stránce `OnPostAsync` metoda je volána, Přišla žádost o nový web s novou instanci třídy `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="3687c-232">When a page's `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="3687c-233">Znovu čtení entita v tomto kontextu nové simuluje plochy zpracování.</span><span class="sxs-lookup"><span data-stu-id="3687c-233">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="3687c-234">Odstranit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="3687c-234">Update the Delete page</span></span>

<span data-ttu-id="3687c-235">V této části je přidán kód k implementaci vlastních chybových zpráv při volání `SaveChanges` selže.</span><span class="sxs-lookup"><span data-stu-id="3687c-235">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="3687c-236">Přidejte řetězec tak, aby obsahovala možné chybové zprávy:</span><span class="sxs-lookup"><span data-stu-id="3687c-236">Add a string to contain possible error messages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="3687c-237">Nahraďte `OnGetAsync` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="3687c-237">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="3687c-238">Předchozí kód obsahuje volitelný parametr `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="3687c-238">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="3687c-239">`saveChangesError` Určuje, zda byla volána metoda po selhání student objekt odstranit.</span><span class="sxs-lookup"><span data-stu-id="3687c-239">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="3687c-240">Operace odstranění se nemusí podařit kvůli přechodným potížím se sítí.</span><span class="sxs-lookup"><span data-stu-id="3687c-240">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="3687c-241">Přechodné chyby síťovým budou s větší pravděpodobností v cloudu.</span><span class="sxs-lookup"><span data-stu-id="3687c-241">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="3687c-242">`saveChangesError`je hodnota false, pokud stránka odstranění `OnGetAsync` je volána z uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3687c-242">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="3687c-243">Když `OnGetAsync` volá `OnPostAsync` (protože operace odstranění se nezdařila), `saveChangesError` parametr hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="3687c-243">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="3687c-244">Metoda odstranění stránky OnPostAsync</span><span class="sxs-lookup"><span data-stu-id="3687c-244">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="3687c-245">Nahraďte `OnPostAsync` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="3687c-245">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="3687c-246">Předchozí kód načte vybrané entity, pak zavolá `Remove` metodu a nastavit stav entity `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="3687c-246">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="3687c-247">Když `SaveChanges` nazývá SQL odstranit se vygeneruje příkaz.</span><span class="sxs-lookup"><span data-stu-id="3687c-247">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="3687c-248">Pokud `Remove` selže:</span><span class="sxs-lookup"><span data-stu-id="3687c-248">If `Remove` fails:</span></span>

* <span data-ttu-id="3687c-249">Je DB výjimka zachycena.</span><span class="sxs-lookup"><span data-stu-id="3687c-249">The DB exception is caught.</span></span>
* <span data-ttu-id="3687c-250">Odstranit stránky `OnGetAsync` metoda je volána s `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="3687c-250">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="3687c-251">Aktualizace stránky Razor odstranění</span><span class="sxs-lookup"><span data-stu-id="3687c-251">Update the Delete Razor Page</span></span>

<span data-ttu-id="3687c-252">Přidejte následující zvýrazněný chybová zpráva na stránce odstranit Razor.</span><span class="sxs-lookup"><span data-stu-id="3687c-252">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="3687c-253">Otestujte odstranit.</span><span class="sxs-lookup"><span data-stu-id="3687c-253">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="3687c-254">Běžné chyby</span><span class="sxs-lookup"><span data-stu-id="3687c-254">Common errors</span></span>

<span data-ttu-id="3687c-255">Nefungují student domů nebo jiné odkazy:</span><span class="sxs-lookup"><span data-stu-id="3687c-255">Student/Home or other links don't work:</span></span>

<span data-ttu-id="3687c-256">Ověřte stránky Razor obsahuje správné `@page` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="3687c-256">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="3687c-257">Například by měla Student nebo Domovská stránka Razor **není** obsahovat šablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="3687c-257">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="3687c-258">Musí zahrnovat každé stránce Razor `@page` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="3687c-258">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3687c-259">[Předchozí](xref:data/ef-rp/intro)
[další](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="3687c-259">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
