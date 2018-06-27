---
title: Stránky Razor s EF jádra ASP.NET Core - CRUD - 2 8
author: rick-anderson
description: Ukazuje, jak vytvořit, číst, aktualizovat, odstraňovat s EF jádra
ms.author: riande
ms.date: 10/15/2017
uid: data/ef-rp/crud
ms.openlocfilehash: 157257d10306ded3456cd66c186a82edf0ba5d65
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961318"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a><span data-ttu-id="bae8f-103">Stránky Razor s EF jádra ASP.NET Core - CRUD - 2 8</span><span class="sxs-lookup"><span data-stu-id="bae8f-103">Razor Pages with EF Core in ASP.NET Core - CRUD - 2 of 8</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="bae8f-104">ASP.NET 2.0 základní verzi v tomto kurzu lze nalézt v [tento PDF soubor](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).</span><span class="sxs-lookup"><span data-stu-id="bae8f-104">The ASP.NET Core 2.0 version of this tutorial can be found in [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bae8f-105">Podle [tní Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bae8f-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="bae8f-106">V tomto kurzu vygenerované CRUD (vytvořit, číst, aktualizovat, odstraňovat) kód je zkontrolovat a upravit.</span><span class="sxs-lookup"><span data-stu-id="bae8f-106">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="bae8f-107">Minimalizovat složitost a zachovat tyto kurzy zaměřené na základní EF, EF základní kód používá v modelech stránky.</span><span class="sxs-lookup"><span data-stu-id="bae8f-107">To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the page models.</span></span> <span data-ttu-id="bae8f-108">Někteří vývojáři použít k vytvoření abstraktní vrstvu mezi uživatelského rozhraní (Razor stránky) a vrstva přístupu k datům služby vrstvy nebo úložiště vzoru v.</span><span class="sxs-lookup"><span data-stu-id="bae8f-108">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="bae8f-109">V tomto kurzu, vytvořit, upravit, odstranit a stránky Razor podrobnosti v *Student* se zkontrolují složky.</span><span class="sxs-lookup"><span data-stu-id="bae8f-109">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are examined.</span></span>

<span data-ttu-id="bae8f-110">Automaticky generovaný kód používá následující vzor pro Create, Edit a Delete stránky:</span><span class="sxs-lookup"><span data-stu-id="bae8f-110">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="bae8f-111">Získání a zobrazit požadovaná data pomocí metody GET protokolu HTTP `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="bae8f-111">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="bae8f-112">Uložit změny do data s metodou HTTP POST `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="bae8f-112">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="bae8f-113">Stránky indexu a podrobnosti o získání a jejich zobrazení požadovaná data pomocí metody GET protokolu HTTP `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="bae8f-113">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="singleordefaultasync-vs-firstordefaultasync"></a><span data-ttu-id="bae8f-114">SingleOrDefaultAsync vs FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="bae8f-114">SingleOrDefaultAsync vs FirstOrDefaultAsync</span></span>

<span data-ttu-id="bae8f-115">Generovaný kód používá [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), což je obecně upřednostňované přes [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="bae8f-115">The generated code uses [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), which is generally preferred over [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

 <span data-ttu-id="bae8f-116">`FirstOrDefaultAsync` je efektivnější než `SingleOrDefaultAsync` v načítání jedna entita:</span><span class="sxs-lookup"><span data-stu-id="bae8f-116">`FirstOrDefaultAsync` is more efficient than `SingleOrDefaultAsync` at fetching one entity:</span></span>

* <span data-ttu-id="bae8f-117">Pokud kód je potřeba ověřit, zda není více než jedna entita vrácená z dotazu.</span><span class="sxs-lookup"><span data-stu-id="bae8f-117">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span>
* <span data-ttu-id="bae8f-118">`SingleOrDefaultAsync` načte více dat a nepotřebné funguje.</span><span class="sxs-lookup"><span data-stu-id="bae8f-118">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="bae8f-119">`SingleOrDefaultAsync` vyvolá výjimku, pokud existuje více než jedna entita, která odpovídá část filtru.</span><span class="sxs-lookup"><span data-stu-id="bae8f-119">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
* <span data-ttu-id="bae8f-120">`FirstOrDefaultAsync` není výjimku, pokud existuje více než jedna entita, která odpovídá část filtru.</span><span class="sxs-lookup"><span data-stu-id="bae8f-120">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="bae8f-121">Asynchronně vyhledá</span><span class="sxs-lookup"><span data-stu-id="bae8f-121">FindAsync</span></span>

<span data-ttu-id="bae8f-122">Hodně automaticky generovaný kód [asynchronně vyhledá](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) lze místě `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="bae8f-122">In much of the scaffolded code, [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync`.</span></span>

<span data-ttu-id="bae8f-123">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="bae8f-123">`FindAsync`:</span></span>

* <span data-ttu-id="bae8f-124">Vyhledá entitu s primární klíč (Primárníklíč).</span><span class="sxs-lookup"><span data-stu-id="bae8f-124">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="bae8f-125">Pokud entity s primárnímu Klíči je sledován pomocí kontextu, je vrácen bez požadavek do databáze.</span><span class="sxs-lookup"><span data-stu-id="bae8f-125">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="bae8f-126">Je jednoduchý a stručné sdělení.</span><span class="sxs-lookup"><span data-stu-id="bae8f-126">Is simple and concise.</span></span>
* <span data-ttu-id="bae8f-127">Je optimalizován pro vyhledání jedné entity.</span><span class="sxs-lookup"><span data-stu-id="bae8f-127">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="bae8f-128">Můžete mít výhody výkonu v některých situacích, ale zřídka se do play pro typické webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="bae8f-128">Can have perf benefits in some situations, but they rarely come into play for typical web apps.</span></span>
* <span data-ttu-id="bae8f-129">Implicitně používá [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) místo [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="bae8f-129">Implicitly uses [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="bae8f-130">Ale pokud budete chtít `Include` jinými entitami, pak `FindAsync` již není vhodné.</span><span class="sxs-lookup"><span data-stu-id="bae8f-130">But if you want to `Include` other entities, then `FindAsync` is no longer appropriate.</span></span> <span data-ttu-id="bae8f-131">To znamená, že budete muset abandon `FindAsync` a přesunout do dotazu průběhu vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="bae8f-131">This means that you may need to abandon `FindAsync` and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="bae8f-132">Stránce s podrobnostmi o přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="bae8f-132">Customize the Details page</span></span>

<span data-ttu-id="bae8f-133">Přejděte do `Pages/Students` stránky.</span><span class="sxs-lookup"><span data-stu-id="bae8f-133">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="bae8f-134">**Upravit**, **podrobnosti**, a **odstranit** generované odkazy [pomocná značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) v *stránek/studenty / Index.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="bae8f-134">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

<span data-ttu-id="bae8f-135">Spusťte aplikaci a vyberte **podrobnosti** odkaz.</span><span class="sxs-lookup"><span data-stu-id="bae8f-135">Run the app and select a **Details** link.</span></span> <span data-ttu-id="bae8f-136">Adresa URL je ve formátu `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="bae8f-136">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="bae8f-137">Student ID je předán pomocí řetězce dotazu (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="bae8f-137">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="bae8f-138">Aktualizovat úpravy, podrobnosti a odstranit stránky Razor k použití `"{id:int}"` šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="bae8f-138">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="bae8f-139">Změňte direktivu stránky pro každou tyto stránek z `@page` k `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="bae8f-139">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="bae8f-140">Požadavek na stránku s šablonou cesty "{id: int}", která nemá **není** zahrnují celé číslo trasy hodnota vrátí protokolu HTTP (nenalezen) chybu 404.</span><span class="sxs-lookup"><span data-stu-id="bae8f-140">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="bae8f-141">Například `http://localhost:5000/Students/Details` vrátí chybu 404.</span><span class="sxs-lookup"><span data-stu-id="bae8f-141">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="bae8f-142">Chcete-li nastavit ID volitelný, připojte `?` pro dané omezení trasy:</span><span class="sxs-lookup"><span data-stu-id="bae8f-142">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="bae8f-143">Spusťte aplikaci, klikněte na odkaz podrobnosti a ověřte adresu URL je předávání ID jako data trasy (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="bae8f-143">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="bae8f-144">Neměnit globálně `@page` k `@page "{id:int}"`, provádění Ano zalomení odkazy na domovský uzel a vytvořte stránky.</span><span class="sxs-lookup"><span data-stu-id="bae8f-144">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="bae8f-145">Přidání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="bae8f-145">Add related data</span></span>

<span data-ttu-id="bae8f-146">Automaticky generovaný kód pro studenty indexovou stránku neobsahuje `Enrollments` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bae8f-146">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="bae8f-147">V této části, obsah `Enrollments` kolekce se zobrazí na stránce Podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bae8f-147">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="bae8f-148">`OnGetAsync` Metodu *Pages/Students/Details.cshtml.cs* používá `FirstOrDefaultAsync` metoda pro načtení jedné `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="bae8f-148">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="bae8f-149">Přidejte následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="bae8f-149">Add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="bae8f-150">[Zahrnout](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) a [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) metody způsobit kontext k načtení `Student.Enrollments` navigační vlastnost a v rámci každé registrace `Enrollment.Course` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bae8f-150">The [Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) and [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="bae8f-151">Tyto metody jsou podrobně v tomto kurzu data související s čtení.</span><span class="sxs-lookup"><span data-stu-id="bae8f-151">These methods are examined in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="bae8f-152">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) metoda zvyšuje výkon v situacích, když entity vrátil se neaktualizují v aktuálním kontextu.</span><span class="sxs-lookup"><span data-stu-id="bae8f-152">The [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="bae8f-153">`AsNoTracking` je popsána dále v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="bae8f-153">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="bae8f-154">Zobrazit související registrace na stránce podrobností</span><span class="sxs-lookup"><span data-stu-id="bae8f-154">Display related enrollments on the Details page</span></span>

<span data-ttu-id="bae8f-155">Otevřete *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bae8f-155">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="bae8f-156">Přidejte následující zvýrazněný kód zobrazíte seznam registrace:</span><span class="sxs-lookup"><span data-stu-id="bae8f-156">Add the following highlighted code to display a list of enrollments:</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

<span data-ttu-id="bae8f-157">Pokud odsazení kód je nesprávný po se vloží kód, stiskněte CTRL-tisíc-D opravte ho.</span><span class="sxs-lookup"><span data-stu-id="bae8f-157">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="bae8f-158">Předchozí kód prochází entity v `Enrollments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bae8f-158">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="bae8f-159">Pro každou registraci zobrazí název kurzu a úrovni.</span><span class="sxs-lookup"><span data-stu-id="bae8f-159">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="bae8f-160">Název postupu načten z kurzu entita, která je uložena v `Course` navigační vlastnost entity registrace.</span><span class="sxs-lookup"><span data-stu-id="bae8f-160">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="bae8f-161">Spuštění aplikace, vyberte **studenty** a klikněte **podrobnosti** odkaz pro student.</span><span class="sxs-lookup"><span data-stu-id="bae8f-161">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="bae8f-162">Zobrazí se seznam kurzů a tříd pro vybrané student.</span><span class="sxs-lookup"><span data-stu-id="bae8f-162">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="bae8f-163">Vytvořit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="bae8f-163">Update the Create page</span></span>

<span data-ttu-id="bae8f-164">Aktualizace `OnPostAsync` metoda v *Pages/Students/Create.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="bae8f-164">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="bae8f-165">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="bae8f-165">TryUpdateModelAsync</span></span>

<span data-ttu-id="bae8f-166">Zkontrolujte [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kódu:</span><span class="sxs-lookup"><span data-stu-id="bae8f-166">Examine the [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="bae8f-167">V předchozí kód `TryUpdateModelAsync<Student>` pokusit o aktualizaci `emptyStudent` objektu z hodnot odeslaného formuláře [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) vlastnost v [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span><span class="sxs-lookup"><span data-stu-id="bae8f-167">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span></span> <span data-ttu-id="bae8f-168">`TryUpdateModelAsync` pouze aktualizace vlastností uvedených (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="bae8f-168">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="bae8f-169">V předchozím příkladu:</span><span class="sxs-lookup"><span data-stu-id="bae8f-169">In the preceding sample:</span></span>

* <span data-ttu-id="bae8f-170">Druhý argument (` "student", // Prefix`) je předponu používá pro vyhledávání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bae8f-170">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="bae8f-171">Není velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="bae8f-171">It's not case sensitive.</span></span>
* <span data-ttu-id="bae8f-172">Hodnoty odeslaného formuláře jsou převedeny na typy v `Student` model pomocí [model vazby](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="bae8f-172">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="bae8f-173">Overposting</span><span class="sxs-lookup"><span data-stu-id="bae8f-173">Overposting</span></span>

<span data-ttu-id="bae8f-174">Pomocí `TryUpdateModel` aktualizovat pole odeslaných hodnot je nejlepším postupem zabezpečení, protože zabraňuje overposting.</span><span class="sxs-lookup"><span data-stu-id="bae8f-174">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="bae8f-175">Předpokládejme například, zahrnuje Student entity `Secret` vlastnost, která tato webová stránka nesmí aktualizovat nebo přidat:</span><span class="sxs-lookup"><span data-stu-id="bae8f-175">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="bae8f-176">I v případě, že aplikace nemá `Secret` pole na vytvoření nebo aktualizaci stránky Razor, se hacker může nastavené `Secret` hodnoty overposting.</span><span class="sxs-lookup"><span data-stu-id="bae8f-176">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="bae8f-177">Hackerům může použijte například nástroj Fiddler, nebo si napsat určitého kódu JavaScript ke zveřejnění `Secret` formuláři hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bae8f-177">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="bae8f-178">Původní kód není omezit pole, která při vytváření Student instance používá vazač modelu.</span><span class="sxs-lookup"><span data-stu-id="bae8f-178">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="bae8f-179">Ať hacker zadaný pro hodnotu `Secret` pole formuláře je aktualizována v databázi.</span><span class="sxs-lookup"><span data-stu-id="bae8f-179">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="bae8f-180">Následující obrázek ukazuje přidání nástroj Fiddler `Secret` pole (s hodnotou "OverPost") na hodnoty odeslaného formuláře.</span><span class="sxs-lookup"><span data-stu-id="bae8f-180">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Přidání tajný pole Fiddler](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="bae8f-182">Hodnota "OverPost" úspěšně přidán do `Secret` vlastnost vloženého řádku.</span><span class="sxs-lookup"><span data-stu-id="bae8f-182">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="bae8f-183">Návrhář aplikace nikdy určená `Secret` vlastnost na stránce vytvořit nastavit.</span><span class="sxs-lookup"><span data-stu-id="bae8f-183">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>

### <a name="view-model"></a><span data-ttu-id="bae8f-184">Model zobrazení</span><span class="sxs-lookup"><span data-stu-id="bae8f-184">View model</span></span>

<span data-ttu-id="bae8f-185">Model zobrazení obvykle obsahuje podmnožinu vlastností obsažených v modelu používá aplikace.</span><span class="sxs-lookup"><span data-stu-id="bae8f-185">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="bae8f-186">Aplikační model se často nazývá modelu domény.</span><span class="sxs-lookup"><span data-stu-id="bae8f-186">The application model is often called the domain model.</span></span> <span data-ttu-id="bae8f-187">Model domény obvykle obsahuje všechny vlastnosti požadované odpovídající entita v databázi.</span><span class="sxs-lookup"><span data-stu-id="bae8f-187">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="bae8f-188">Model zobrazení obsahuje pouze vlastnosti, které jsou nutné pro vrstvu uživatelského rozhraní (například stránka vytvořit).</span><span class="sxs-lookup"><span data-stu-id="bae8f-188">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="bae8f-189">Kromě zobrazení modelu některé aplikace použít vazby modelu nebo vstupní modelu k předávání dat mezi třídy modelu stránky Razor stránky a v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="bae8f-189">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="bae8f-190">Vezměte v úvahu následující `Student` modelu zobrazení:</span><span class="sxs-lookup"><span data-stu-id="bae8f-190">Consider the following `Student` view model:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

<span data-ttu-id="bae8f-191">Zobrazit modely poskytnout alternativní způsob zabránit overposting.</span><span class="sxs-lookup"><span data-stu-id="bae8f-191">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="bae8f-192">Model zobrazení obsahuje pouze vlastnosti zobrazení (zobrazení) a aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="bae8f-192">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="bae8f-193">Následující kód používá `StudentVM` zobrazení modelu vytvářet nové student:</span><span class="sxs-lookup"><span data-stu-id="bae8f-193">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="bae8f-194">[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) metoda nastaví hodnoty tohoto objektu ve čtení hodnoty z jiné [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) objektu.</span><span class="sxs-lookup"><span data-stu-id="bae8f-194">The [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="bae8f-195">`SetValues` používá vlastnost s odpovídajícím názvem.</span><span class="sxs-lookup"><span data-stu-id="bae8f-195">`SetValues` uses property name matching.</span></span> <span data-ttu-id="bae8f-196">Typ modelu zobrazení nemusí být související s typem modelu, pouze musí mít vlastnosti, které odpovídají.</span><span class="sxs-lookup"><span data-stu-id="bae8f-196">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="bae8f-197">Pomocí `StudentVM` vyžaduje [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) aktualizovat, a použít `StudentVM` místo `Student`.</span><span class="sxs-lookup"><span data-stu-id="bae8f-197">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="bae8f-198">Na stránkách Razor `PageModel` odvozené třídy je zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="bae8f-198">In Razor Pages, the `PageModel` derived class is the view model.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="bae8f-199">Upravit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="bae8f-199">Update the Edit page</span></span>

<span data-ttu-id="bae8f-200">Aktualizace modelu stránky pro stránku upravit.</span><span class="sxs-lookup"><span data-stu-id="bae8f-200">Update the page model for the Edit page.</span></span> <span data-ttu-id="bae8f-201">Jsou vyznačené hlavních změn:</span><span class="sxs-lookup"><span data-stu-id="bae8f-201">The major changes are highlighted:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="bae8f-202">Změny kódu jsou podobná stránce vytvořit s několika výjimkami:</span><span class="sxs-lookup"><span data-stu-id="bae8f-202">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="bae8f-203">`OnPostAsync` má volitelný `id` parametr.</span><span class="sxs-lookup"><span data-stu-id="bae8f-203">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="bae8f-204">Aktuální student jsou načtena z databáze, místo vytvoření prázdné student.</span><span class="sxs-lookup"><span data-stu-id="bae8f-204">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="bae8f-205">`FirstOrDefaultAsync` byla nahrazena [asynchronně vyhledá](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span><span class="sxs-lookup"><span data-stu-id="bae8f-205">`FirstOrDefaultAsync` has been replaced with [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span></span> <span data-ttu-id="bae8f-206">`FindAsync` je vhodné použít při výběru entity z primární klíč.</span><span class="sxs-lookup"><span data-stu-id="bae8f-206">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="bae8f-207">V tématu [asynchronně vyhledá](#FindAsync) Další informace.</span><span class="sxs-lookup"><span data-stu-id="bae8f-207">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="bae8f-208">Testování úpravy a vytvářet stránky</span><span class="sxs-lookup"><span data-stu-id="bae8f-208">Test the Edit and Create pages</span></span>

<span data-ttu-id="bae8f-209">Vytvořit a upravit několik student entit.</span><span class="sxs-lookup"><span data-stu-id="bae8f-209">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="bae8f-210">Stav entity</span><span class="sxs-lookup"><span data-stu-id="bae8f-210">Entity States</span></span>

<span data-ttu-id="bae8f-211">Kontext databáze uchovává informace o, jestli jsou synchronizované s jejich odpovídající řádky v databázi entity v paměti.</span><span class="sxs-lookup"><span data-stu-id="bae8f-211">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="bae8f-212">Informace o databázi kontext synchronizace určuje, co se stane, když [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) je volána.</span><span class="sxs-lookup"><span data-stu-id="bae8f-212">The DB context sync information determines what happens when [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span> <span data-ttu-id="bae8f-213">Například když novou entitu je předána [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) metoda, která je nastavena do stavu entity [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span><span class="sxs-lookup"><span data-stu-id="bae8f-213">For example, when a new entity is passed to the [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) method, that entity's state is set to [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span></span> <span data-ttu-id="bae8f-214">Když `SaveChangesAsync` nazývá databáze kontextu problémy příkazu INSERT jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="bae8f-214">When `SaveChangesAsync` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="bae8f-215">Entita může být v jednom z [následující stavy](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span><span class="sxs-lookup"><span data-stu-id="bae8f-215">An entity may be in one of the [following states](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span></span>

* <span data-ttu-id="bae8f-216">`Added`: Entita ještě neexistuje v databázi.</span><span class="sxs-lookup"><span data-stu-id="bae8f-216">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="bae8f-217">`SaveChanges` Metoda problémy příkazu INSERT.</span><span class="sxs-lookup"><span data-stu-id="bae8f-217">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="bae8f-218">`Unchanged`: Žádné změny měly být uloženy s tuto entitu.</span><span class="sxs-lookup"><span data-stu-id="bae8f-218">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="bae8f-219">Entita nemá tento stav, pokud je pro čtení z databáze.</span><span class="sxs-lookup"><span data-stu-id="bae8f-219">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="bae8f-220">`Modified`: Bylo upraveno některé nebo všechny hodnoty vlastností entity.</span><span class="sxs-lookup"><span data-stu-id="bae8f-220">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="bae8f-221">`SaveChanges` Metoda problémy příkazu UPDATE.</span><span class="sxs-lookup"><span data-stu-id="bae8f-221">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="bae8f-222">`Deleted`: Entita byla označena pro odstranění.</span><span class="sxs-lookup"><span data-stu-id="bae8f-222">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="bae8f-223">`SaveChanges` Metoda problémy příkazech DELETE.</span><span class="sxs-lookup"><span data-stu-id="bae8f-223">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="bae8f-224">`Detached`: Entity není sledována aplikací kontext databáze.</span><span class="sxs-lookup"><span data-stu-id="bae8f-224">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="bae8f-225">V aplikace na ploše změny stavu se obvykle nastavuje automaticky.</span><span class="sxs-lookup"><span data-stu-id="bae8f-225">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="bae8f-226">Entita je pro čtení, změny a stav entity automaticky změnit tak, aby `Modified`.</span><span class="sxs-lookup"><span data-stu-id="bae8f-226">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="bae8f-227">Volání metody `SaveChanges` vytvoří prohlášení aktualizace SQL, který aktualizuje pouze změněné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bae8f-227">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="bae8f-228">Ve webové aplikaci `DbContext` entity a zobrazí data je zrušen po vykreslení stránky, který čte.</span><span class="sxs-lookup"><span data-stu-id="bae8f-228">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="bae8f-229">Pokud na stránce `OnPostAsync` metoda je volána, Přišla žádost o nový web s novou instanci třídy `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="bae8f-229">When a page's `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="bae8f-230">Znovu čtení entita v tomto kontextu nové simuluje plochy zpracování.</span><span class="sxs-lookup"><span data-stu-id="bae8f-230">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="bae8f-231">Odstranit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="bae8f-231">Update the Delete page</span></span>

<span data-ttu-id="bae8f-232">V této části je přidán kód k implementaci vlastních chybových zpráv při volání `SaveChanges` selže.</span><span class="sxs-lookup"><span data-stu-id="bae8f-232">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="bae8f-233">Přidejte řetězec tak, aby obsahovala možné chybové zprávy:</span><span class="sxs-lookup"><span data-stu-id="bae8f-233">Add a string to contain possible error messages:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="bae8f-234">Nahraďte `OnGetAsync` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="bae8f-234">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="bae8f-235">Předchozí kód obsahuje volitelný parametr `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="bae8f-235">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="bae8f-236">`saveChangesError` Určuje, zda byla volána metoda po selhání student objekt odstranit.</span><span class="sxs-lookup"><span data-stu-id="bae8f-236">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="bae8f-237">Operace odstranění se nemusí podařit kvůli přechodným potížím se sítí.</span><span class="sxs-lookup"><span data-stu-id="bae8f-237">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="bae8f-238">Přechodné chyby síťovým budou s větší pravděpodobností v cloudu.</span><span class="sxs-lookup"><span data-stu-id="bae8f-238">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="bae8f-239">`saveChangesError`je hodnota false, pokud stránka odstranění `OnGetAsync` je volána z uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bae8f-239">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="bae8f-240">Když `OnGetAsync` volá `OnPostAsync` (protože operace odstranění se nezdařila), `saveChangesError` parametr hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="bae8f-240">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="bae8f-241">Metoda odstranění stránky OnPostAsync</span><span class="sxs-lookup"><span data-stu-id="bae8f-241">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="bae8f-242">Nahraďte `OnPostAsync` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="bae8f-242">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="bae8f-243">Předchozí kód načte vybrané entity, pak zavolá [odebrat](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) metodu a nastavit stav entity `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="bae8f-243">The preceding code retrieves the selected entity, then calls the [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="bae8f-244">Když `SaveChanges` nazývá SQL odstranit se vygeneruje příkaz.</span><span class="sxs-lookup"><span data-stu-id="bae8f-244">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="bae8f-245">Pokud `Remove` selže:</span><span class="sxs-lookup"><span data-stu-id="bae8f-245">If `Remove` fails:</span></span>

* <span data-ttu-id="bae8f-246">Je DB výjimka zachycena.</span><span class="sxs-lookup"><span data-stu-id="bae8f-246">The DB exception is caught.</span></span>
* <span data-ttu-id="bae8f-247">Odstranit stránky `OnGetAsync` metoda je volána s `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="bae8f-247">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="bae8f-248">Aktualizace stránky Razor odstranění</span><span class="sxs-lookup"><span data-stu-id="bae8f-248">Update the Delete Razor Page</span></span>

<span data-ttu-id="bae8f-249">Přidejte následující zvýrazněný chybová zpráva na stránce odstranit Razor.</span><span class="sxs-lookup"><span data-stu-id="bae8f-249">Add the following highlighted error message to the Delete Razor Page.</span></span>
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="bae8f-250">Otestujte odstranit.</span><span class="sxs-lookup"><span data-stu-id="bae8f-250">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="bae8f-251">Běžné chyby</span><span class="sxs-lookup"><span data-stu-id="bae8f-251">Common errors</span></span>

<span data-ttu-id="bae8f-252">Nefungují student domů nebo jiné odkazy:</span><span class="sxs-lookup"><span data-stu-id="bae8f-252">Student/Home or other links don't work:</span></span>

<span data-ttu-id="bae8f-253">Ověřte stránky Razor obsahuje správné `@page` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="bae8f-253">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="bae8f-254">Například by měla Student nebo Domovská stránka Razor **není** obsahovat šablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="bae8f-254">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="bae8f-255">Musí zahrnovat každé stránce Razor `@page` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="bae8f-255">Each Razor Page must include the `@page` directive.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="bae8f-256">[Předchozí](xref:data/ef-rp/intro)
> [další](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="bae8f-256">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
