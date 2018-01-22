---
title: "Jádro ASP.NET MVC s EF Core - řazení, filtru, stránkování - 3 10"
author: tdykstra
description: "V tomto kurzu přidáte třídění, filtrování a stránkování funkce na stránku pomocí ASP.NET Core a Entity Framework Core."
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 6da2073b18f6fff9738808c84441e59240caefe3
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a><span data-ttu-id="e6656-103">Řazení, filtrování, stránkování a seskupení – základní EF s kurz k ASP.NET MVC jádra (3 10)</span><span class="sxs-lookup"><span data-stu-id="e6656-103">Sorting, filtering, paging, and grouping - EF Core with ASP.NET Core MVC tutorial (3 of 10)</span></span>

<span data-ttu-id="e6656-104">Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e6656-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e6656-105">Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e6656-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="e6656-106">Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).</span><span class="sxs-lookup"><span data-stu-id="e6656-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="e6656-107">V tomto kurzu předchozí implementována sadu webové stránky pro základní operace CRUD pro studenty entity.</span><span class="sxs-lookup"><span data-stu-id="e6656-107">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="e6656-108">V tomto kurzu přidáte třídění, filtrování a funkce stránkování pro studenty indexovou stránku.</span><span class="sxs-lookup"><span data-stu-id="e6656-108">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="e6656-109">Pokud vytvoříte stránky, který nemá jednoduchý seskupení.</span><span class="sxs-lookup"><span data-stu-id="e6656-109">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="e6656-110">Následující obrázek znázorňuje, co bude stránka vypadat po dokončení.</span><span class="sxs-lookup"><span data-stu-id="e6656-110">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="e6656-111">Záhlaví sloupců jsou odkazy, které uživatel může kliknout na řadit podle sloupce.</span><span class="sxs-lookup"><span data-stu-id="e6656-111">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="e6656-112">Kliknutím na záhlaví opakovaně sloupce Přepne mezi vzestupné a sestupné řazení.</span><span class="sxs-lookup"><span data-stu-id="e6656-112">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Studenti, kteří indexovou stránku](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="e6656-114">Přidat sloupec řazení odkazy na indexovou stránku studenty</span><span class="sxs-lookup"><span data-stu-id="e6656-114">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="e6656-115">K přidání řazení Student indexovou stránku, změníte `Index` metoda studenty řadiče a přidejte do zobrazení indexu Student kód.</span><span class="sxs-lookup"><span data-stu-id="e6656-115">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="e6656-116">Přidání řazení funkce Index – metoda</span><span class="sxs-lookup"><span data-stu-id="e6656-116">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="e6656-117">V *StudentsController.cs*, nahraďte `Index` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e6656-117">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="e6656-118">Tento kód přijme `sortOrder` parametr z řetězce dotazu v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="e6656-118">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="e6656-119">Hodnotu řetězce dotazu je poskytovaný ASP.NET MVC základní jako parametr pro metodu akce.</span><span class="sxs-lookup"><span data-stu-id="e6656-119">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="e6656-120">Parametr bude řetězec, který je buď "Name" nebo "Datum", může volitelně následovat podtržítkem a řetězec "desc" Zadejte sestupném pořadí.</span><span class="sxs-lookup"><span data-stu-id="e6656-120">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="e6656-121">Výchozí pořadí řazení je vzestupně.</span><span class="sxs-lookup"><span data-stu-id="e6656-121">The default sort order is ascending.</span></span>

<span data-ttu-id="e6656-122">Při prvním vyžádání stránky indexu není žádný řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="e6656-122">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="e6656-123">Studenti, kteří se zobrazí ve vzestupném pořadí podle příjmení, což výchozí nastavení podle v případě patří prostřednictvím `switch` příkaz.</span><span class="sxs-lookup"><span data-stu-id="e6656-123">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="e6656-124">Když uživatel klikne na sloupec záhlaví hypertextový odkaz, odpovídající `sortOrder` v řetězci dotazu je zadána hodnota.</span><span class="sxs-lookup"><span data-stu-id="e6656-124">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="e6656-125">Dva `ViewData` prvky (NameSortParm a DateSortParm) se zobrazením používají ke konfiguraci hypertextové odkazy záhlaví sloupce s řetězcové hodnoty odpovídající dotazu.</span><span class="sxs-lookup"><span data-stu-id="e6656-125">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="e6656-126">Jedná se o Ternární příkazy.</span><span class="sxs-lookup"><span data-stu-id="e6656-126">These are ternary statements.</span></span> <span data-ttu-id="e6656-127">První z nich určuje, že pokud `sortOrder` parametr má hodnotu null nebo prázdná, musí být NameSortParm nastavena na "name_desc"; jinak je potřeba ho nastavit na prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e6656-127">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="e6656-128">Tyto dva příkazy zapnutí zobrazení nastavit sloupec hypertextové odkazy záhlaví následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e6656-128">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="e6656-129">Aktuální řazení</span><span class="sxs-lookup"><span data-stu-id="e6656-129">Current sort order</span></span>  | <span data-ttu-id="e6656-130">Poslední název hypertextový odkaz</span><span class="sxs-lookup"><span data-stu-id="e6656-130">Last Name Hyperlink</span></span> | <span data-ttu-id="e6656-131">Datum hypertextový odkaz</span><span class="sxs-lookup"><span data-stu-id="e6656-131">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="e6656-132">Poslední název vzestupné</span><span class="sxs-lookup"><span data-stu-id="e6656-132">Last Name ascending</span></span>  | <span data-ttu-id="e6656-133">descending</span><span class="sxs-lookup"><span data-stu-id="e6656-133">descending</span></span>          | <span data-ttu-id="e6656-134">ascending</span><span class="sxs-lookup"><span data-stu-id="e6656-134">ascending</span></span>      |
| <span data-ttu-id="e6656-135">Poslední sestupném název</span><span class="sxs-lookup"><span data-stu-id="e6656-135">Last Name descending</span></span> | <span data-ttu-id="e6656-136">ascending</span><span class="sxs-lookup"><span data-stu-id="e6656-136">ascending</span></span>           | <span data-ttu-id="e6656-137">ascending</span><span class="sxs-lookup"><span data-stu-id="e6656-137">ascending</span></span>      |
| <span data-ttu-id="e6656-138">Datum vzestupné</span><span class="sxs-lookup"><span data-stu-id="e6656-138">Date ascending</span></span>       | <span data-ttu-id="e6656-139">ascending</span><span class="sxs-lookup"><span data-stu-id="e6656-139">ascending</span></span>           | <span data-ttu-id="e6656-140">descending</span><span class="sxs-lookup"><span data-stu-id="e6656-140">descending</span></span>     |
| <span data-ttu-id="e6656-141">Datum sestupném</span><span class="sxs-lookup"><span data-stu-id="e6656-141">Date descending</span></span>      | <span data-ttu-id="e6656-142">ascending</span><span class="sxs-lookup"><span data-stu-id="e6656-142">ascending</span></span>           | <span data-ttu-id="e6656-143">ascending</span><span class="sxs-lookup"><span data-stu-id="e6656-143">ascending</span></span>      |

<span data-ttu-id="e6656-144">Metoda používá k určení tento sloupec seřadit podle technologie LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="e6656-144">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="e6656-145">Kód vytvoří `IQueryable` proměnná před příkazem switch ho změní v příkazu přepínače a počet volání `ToListAsync` metoda po `switch` příkaz.</span><span class="sxs-lookup"><span data-stu-id="e6656-145">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="e6656-146">Při vytváření a úprava `IQueryable` proměnné, bude odeslán žádný dotaz do databáze.</span><span class="sxs-lookup"><span data-stu-id="e6656-146">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="e6656-147">Dotaz není provést, dokud je převést `IQueryable` objektu do kolekce voláním metody `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="e6656-147">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="e6656-148">Proto tento kód výsledkem jeden dotaz, který není provést, dokud `return View` příkaz.</span><span class="sxs-lookup"><span data-stu-id="e6656-148">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="e6656-149">Tento kód může získat podrobné s velkým počtem sloupců.</span><span class="sxs-lookup"><span data-stu-id="e6656-149">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="e6656-150">[Poslední kurzu této série](advanced.md#dynamic-linq) ukazuje, jak napsat kód, který umožňuje předat název `OrderBy` sloupce v proměnné řetězce.</span><span class="sxs-lookup"><span data-stu-id="e6656-150">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="e6656-151">Přidejte hypertextové odkazy záhlaví sloupců do zobrazení indexu studenty</span><span class="sxs-lookup"><span data-stu-id="e6656-151">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="e6656-152">Nahraďte kód v *Views/Students/Index.cshtml*, s následujícím kódem přidat hypertextové odkazy záhlaví sloupce.</span><span class="sxs-lookup"><span data-stu-id="e6656-152">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="e6656-153">Jsou vyznačené změněných řádků.</span><span class="sxs-lookup"><span data-stu-id="e6656-153">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="e6656-154">Tento kód používá informace v `ViewData` řetězce hodnoty vlastnosti, které chcete nastavit hypertextové odkazy s odpovídající dotazu.</span><span class="sxs-lookup"><span data-stu-id="e6656-154">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="e6656-155">Spuštění aplikace, vyberte **studenty** a klikněte **příjmení** a **datum registrace** záhlaví sloupce. Ověřte, že řazení funguje.</span><span class="sxs-lookup"><span data-stu-id="e6656-155">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Studenti, kteří indexovou stránku v pořadí název](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="e6656-157">Přidat vyhledávací pole na studenty indexovou stránku</span><span class="sxs-lookup"><span data-stu-id="e6656-157">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="e6656-158">Pokud chcete přidat, filtrování studenty indexovou stránku, budete přidat textové pole a tlačítko pro odeslání do zobrazení a provádět odpovídající změny v `Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="e6656-158">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="e6656-159">Textové pole umožňují zadejte řetězec, který chcete vyhledat v název první a poslední název pole.</span><span class="sxs-lookup"><span data-stu-id="e6656-159">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="e6656-160">Přidat další filtrování funkce Index – metoda</span><span class="sxs-lookup"><span data-stu-id="e6656-160">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="e6656-161">V *StudentsController.cs*, nahraďte `Index` metoda následujícím kódem (změny se zvýrazněnou).</span><span class="sxs-lookup"><span data-stu-id="e6656-161">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="e6656-162">Přidali jste `searchString` parametru `Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="e6656-162">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="e6656-163">Hodnota řetězce vyhledávání byl přijat z textové pole, které přidáte do zobrazení indexu.</span><span class="sxs-lookup"><span data-stu-id="e6656-163">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="e6656-164">Jste také přidali do příkazu LINQ where klauzuli, která vybere pouze studenty, jejichž křestní jméno nebo příjmení obsahuje řetězec pro hledání.</span><span class="sxs-lookup"><span data-stu-id="e6656-164">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="e6656-165">Příkaz, který přidá where klauzule se spustí pouze v případě, že je hodnota pro vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="e6656-165">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="e6656-166">Tady jsou volání `Where` metodu `IQueryable` objekt a filtr se zpracují na serveru.</span><span class="sxs-lookup"><span data-stu-id="e6656-166">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="e6656-167">V některých případech může být volání `Where` metoda jako metody rozšíření na kolekci v paměti.</span><span class="sxs-lookup"><span data-stu-id="e6656-167">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="e6656-168">(Například Předpokládejme, že můžete změnit odkaz na `_context.Students` tak místo z EF `DbSet` odkazuje na metodu úložiště, která vrátí `IEnumerable` kolekce.) Výsledek by za normálních okolností stejné, ale v některých případech může být odlišné.</span><span class="sxs-lookup"><span data-stu-id="e6656-168">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="e6656-169">Například implementace rozhraní .NET Framework `Contains` metoda provádí malá a velká písmena porovnání ve výchozím nastavení, ale v systému SQL Server je určena nastavení kolace instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e6656-169">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="e6656-170">Toto nastavení je výchozí pro velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="e6656-170">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="e6656-171">Může zavolat `ToUpper` metoda aby test explicitně velká a malá písmena: *kde (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*.</span><span class="sxs-lookup"><span data-stu-id="e6656-171">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="e6656-172">Zajistí, že výsledky zůstane stejná, pokud změníte kód, abyste mohli používat úložiště, která vrátí hodnotu `IEnumerable` kolekce místo `IQueryable` objektu.</span><span class="sxs-lookup"><span data-stu-id="e6656-172">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="e6656-173">(Při volání `Contains` metodu `IEnumerable` kolekce, získat implementace rozhraní .NET Framework;. při jeho volání na `IQueryable` objektu získat implementace zprostředkovatele databáze.) Je však snížení výkonu pro toto řešení.</span><span class="sxs-lookup"><span data-stu-id="e6656-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there is a performance penalty for this solution.</span></span> <span data-ttu-id="e6656-174">`ToUpper` Kódu by měla být umístěna funkci v klauzuli WHERE příkazu TSQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="e6656-174">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="e6656-175">Pro optimalizaci který by bránily pomocí indexu.</span><span class="sxs-lookup"><span data-stu-id="e6656-175">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="e6656-176">Vzhledem k tomu, že SQL je nainstalován většinou velká a malá písmena, je vyhýbat se `ToUpper` kód, dokud nemigrujete k úložišti dat malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="e6656-176">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="e6656-177">Vyhledávací pole, přidejte do zobrazení indexu studenty</span><span class="sxs-lookup"><span data-stu-id="e6656-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="e6656-178">V *Views/Student/Index.cshtml*, přidejte zvýrazněný kód bezprostředně před otevření před vytvořením popiskem, textové pole, značka tabulky a **vyhledávání** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e6656-178">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="e6656-179">Tento kód používá `<form>` [značky pomocná](xref:mvc/views/tag-helpers/intro) do textového pole pro vyhledávání a tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="e6656-179">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="e6656-180">Ve výchozím nastavení `<form>` značky pomocná odesílat data formuláře s POST, což znamená, že jsou parametry předány v textu zprávy HTTP a není v adrese URL jako řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="e6656-180">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="e6656-181">Když zadáte GET protokolu HTTP, data formuláře je předán v adrese URL jako řetězce dotazu, který uživatelům umožňuje bookmark adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e6656-181">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="e6656-182">Doporučujeme W3C pokyny, které byste měli používat docházet při akci nevede aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="e6656-182">The W3C guidelines recommend that you should use GET when the action does not result in an update.</span></span>

<span data-ttu-id="e6656-183">Spuštění aplikace, vyberte **studenty** kartě, zadejte vyhledávací řetězec a kliknutím na tlačítko Hledat ověřte, zda je funkční filtrování.</span><span class="sxs-lookup"><span data-stu-id="e6656-183">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![Studenti, kteří indexovou stránku s filtrováním](sort-filter-page/_static/filtering.png)

<span data-ttu-id="e6656-185">Všimněte si, že adresa URL obsahuje řetězec pro hledání.</span><span class="sxs-lookup"><span data-stu-id="e6656-185">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="e6656-186">Pokud vytvoříte záložku této stránce, získáte při použití záložky filtrovaný seznam.</span><span class="sxs-lookup"><span data-stu-id="e6656-186">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="e6656-187">Přidání `method="get"` k `form` značka je v tom, co způsobilo řetězec dotazu, který má být vygenerován.</span><span class="sxs-lookup"><span data-stu-id="e6656-187">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="e6656-188">V této fázi, pokud kliknete na odkaz řazení záhlaví sloupce ztratíte hodnota filtru, který jste zadali v **vyhledávání** pole.</span><span class="sxs-lookup"><span data-stu-id="e6656-188">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="e6656-189">Budete to opravíme v další části.</span><span class="sxs-lookup"><span data-stu-id="e6656-189">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="e6656-190">Přidání funkce stránkování pro studenty indexovou stránku</span><span class="sxs-lookup"><span data-stu-id="e6656-190">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="e6656-191">Pokud chcete přidat stránkování studenty indexovou stránku, vytvoříte `PaginatedList` třídu, která využívá `Skip` a `Take` příkazy k filtrování dat na serveru, místo vždy načítání všechny řádky v tabulce.</span><span class="sxs-lookup"><span data-stu-id="e6656-191">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="e6656-192">Pak budete udělat další změny v `Index` metoda a přidejte stránkování tlačítek `Index` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e6656-192">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="e6656-193">Následující obrázek znázorňuje tlačítka stránkování.</span><span class="sxs-lookup"><span data-stu-id="e6656-193">The following illustration shows the paging buttons.</span></span>

![studenti, kteří indexovou stránku s odkazy stránkování](sort-filter-page/_static/paging.png)

<span data-ttu-id="e6656-195">Ve složce projektu vytvořte `PaginatedList.cs`a pak nahraďte kód šablony s následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="e6656-195">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="e6656-196">`CreateAsync` Metody v tomto kódu trvá velikost stránky a číslo stránky a použije příslušné `Skip` a `Take` příkazy `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="e6656-196">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="e6656-197">Když `ToListAsync` se volá na `IQueryable`, vrátí seznam obsahující pouze k požadované stránce.</span><span class="sxs-lookup"><span data-stu-id="e6656-197">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="e6656-198">Vlastnosti `HasPreviousPage` a `HasNextPage` slouží k povolení nebo zakázání **předchozí** a **Další** stránkování tlačítka.</span><span class="sxs-lookup"><span data-stu-id="e6656-198">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="e6656-199">A `CreateAsync` metoda se používá namísto konstruktor k vytvoření `PaginatedList<T>` objekt, protože konstruktory nelze spustit asynchronní kódu.</span><span class="sxs-lookup"><span data-stu-id="e6656-199">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="e6656-200">Přidání funkce stránkování metodu indexu</span><span class="sxs-lookup"><span data-stu-id="e6656-200">Add paging functionality to the Index method</span></span>

<span data-ttu-id="e6656-201">V *StudentsController.cs*, nahraďte `Index` metoda následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="e6656-201">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="e6656-202">Tento kód přidá parametr číslo stránky, aktuální parametr pořadí řazení a aktuální parametr filtru podpis metody.</span><span class="sxs-lookup"><span data-stu-id="e6656-202">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="e6656-203">První stránka se zobrazí, nebo pokud uživatel nebyl klikli stránkování nebo řazení odkaz, všechny parametry budou mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="e6656-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="e6656-204">Po kliknutí na odkaz stránkování proměnnou stránky bude obsahovat číslo stránky pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e6656-204">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="e6656-205">`ViewData` Element s názvem CurrentSort nabízí zobrazení s aktuální pořadí řazení, protože to musí být součástí stránkování odkazy. Chcete-li zachovat stejné při stránkování pořadí řazení.</span><span class="sxs-lookup"><span data-stu-id="e6656-205">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="e6656-206">`ViewData` Element s názvem CurrentFilter nabízí zobrazení s aktuální řetězec filtru.</span><span class="sxs-lookup"><span data-stu-id="e6656-206">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="e6656-207">Tato hodnota musí být součástí odkazy stránkování, aby byla zachována nastavení filtru během stránkování a je nutné ho obnovit do textového pole, pokud se zobrazí stránku znovu.</span><span class="sxs-lookup"><span data-stu-id="e6656-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="e6656-208">Řetězec pro hledání dojde ke změně během stránkování, stránky je nutné ho obnovit hodnotu 1, protože nový filtr může mít za následek různé data k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e6656-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="e6656-209">Řetězec pro hledání se změní, pokud je zadána hodnota do textového pole a stisknutí tlačítka Odeslat.</span><span class="sxs-lookup"><span data-stu-id="e6656-209">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="e6656-210">V takovém případě `searchString` parametr není null.</span><span class="sxs-lookup"><span data-stu-id="e6656-210">In that case, the `searchString` parameter is not null.</span></span>

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="e6656-211">Na konci `Index` metody `PaginatedList.CreateAsync` metoda převede student dotaz na jednu stránku studentů v typu kolekce, která podporuje stránkování.</span><span class="sxs-lookup"><span data-stu-id="e6656-211">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="e6656-212">Této stránce studentů jsou předána do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e6656-212">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="e6656-213">`PaginatedList.CreateAsync` Metoda přebírá číslo stránky.</span><span class="sxs-lookup"><span data-stu-id="e6656-213">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="e6656-214">Dva otazníky představují operátor slučování hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="e6656-214">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="e6656-215">Operátor slučování null definuje výchozí hodnotu pro typ s možnou hodnotou Null; výraz `(page ?? 1)` znamená vrátí hodnotu `page` Pokud má hodnotu, nebo 1-li vrátit `page` má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="e6656-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="e6656-216">Přidat stránkování odkazy na zobrazení indexu studenty</span><span class="sxs-lookup"><span data-stu-id="e6656-216">Add paging links to the Student Index view</span></span>

<span data-ttu-id="e6656-217">V *Views/Students/Index.cshtml*, existujícího kódu nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="e6656-217">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="e6656-218">Změny se zvýrazněnou.</span><span class="sxs-lookup"><span data-stu-id="e6656-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="e6656-219">`@model` Příkaz v horní části stránky určuje, že nyní získá zobrazení `PaginatedList<T>` objektu místo `List<T>` objektu.</span><span class="sxs-lookup"><span data-stu-id="e6656-219">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="e6656-220">Odkazy záhlaví sloupce pomocí řetězce dotazu předat aktuální řetězec pro hledání na řadič, takže uživatel může seřadit v rámci výsledky filtru:</span><span class="sxs-lookup"><span data-stu-id="e6656-220">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="e6656-221">Tlačítka stránkování se zobrazí podle značky pomocné rutiny:</span><span class="sxs-lookup"><span data-stu-id="e6656-221">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="e6656-222">Spusťte aplikaci a přejděte na stránku studenty.</span><span class="sxs-lookup"><span data-stu-id="e6656-222">Run the app and go to the Students page.</span></span>

![studenti, kteří indexovou stránku s odkazy stránkování](sort-filter-page/_static/paging.png)

<span data-ttu-id="e6656-224">Kliknutím na odkazy stránkování v jiné pořadí řazení do Ujistěte se, že funguje stránkování.</span><span class="sxs-lookup"><span data-stu-id="e6656-224">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="e6656-225">Pak zadejte hledaný řetězec a zkuste to znovu a ověřte, že stránkování také funguje správně s řazení a filtrování stránkování.</span><span class="sxs-lookup"><span data-stu-id="e6656-225">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="e6656-226">Vytvořit stránku o, která uvádí statistiku studenty</span><span class="sxs-lookup"><span data-stu-id="e6656-226">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="e6656-227">Pro web společnosti Contoso univerzity **o** stránky, budete zobrazit, kolik studenti, kteří mají zaregistrované pro každé datum registrace.</span><span class="sxs-lookup"><span data-stu-id="e6656-227">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="e6656-228">To vyžaduje seskupování a jednoduché výpočty na skupiny.</span><span class="sxs-lookup"><span data-stu-id="e6656-228">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="e6656-229">K tomu, můžete to udělat následující:</span><span class="sxs-lookup"><span data-stu-id="e6656-229">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="e6656-230">Vytvořte třídu modelu zobrazení pro data, která potřebujete předat do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e6656-230">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="e6656-231">Změňte metodu o v domovské kontroleru.</span><span class="sxs-lookup"><span data-stu-id="e6656-231">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="e6656-232">Upravte o zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e6656-232">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="e6656-233">Vytvoření modelu zobrazení</span><span class="sxs-lookup"><span data-stu-id="e6656-233">Create the view model</span></span>

<span data-ttu-id="e6656-234">Vytvoření *SchoolViewModels* složku *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="e6656-234">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="e6656-235">V nové složky, přidejte soubor třídy *EnrollmentDateGroup.cs* a nahraďte kód šablony s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e6656-235">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="e6656-236">Upravit domácí řadiče</span><span class="sxs-lookup"><span data-stu-id="e6656-236">Modify the Home Controller</span></span>

<span data-ttu-id="e6656-237">V *HomeController.cs*, přidejte následující příkazy v horní části souboru:</span><span class="sxs-lookup"><span data-stu-id="e6656-237">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="e6656-238">Přidání proměnné třídy kontextu databáze okamžitě po otevření složené závorky pro třídu a získat instance kontextu z DI jádro ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="e6656-238">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="e6656-239">Nahraďte `About` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e6656-239">Replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="e6656-240">Příkaz LINQ skupiny entit student datu registrace, vypočítá počet entit v každé skupině a ukládá výsledky do kolekce `EnrollmentDateGroup` zobrazit objekty modelu.</span><span class="sxs-lookup"><span data-stu-id="e6656-240">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="e6656-241">V 1.0 verzi Entity Framework Core celou sadu výsledků je vrácen do klienta a seskupení se provádí na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e6656-241">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="e6656-242">V některých případech to může způsobit problémy s výkonu.</span><span class="sxs-lookup"><span data-stu-id="e6656-242">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="e6656-243">Nezapomeňte testování výkonu s provozním objemů dat a v případě potřeby pomocí nezpracovaná SQL proveďte seskupení na serveru.</span><span class="sxs-lookup"><span data-stu-id="e6656-243">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="e6656-244">Informace o tom, jak používat nezpracovaná SQL najdete v tématu [poslední kurzu této série](advanced.md).</span><span class="sxs-lookup"><span data-stu-id="e6656-244">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="e6656-245">Upravit o zobrazení</span><span class="sxs-lookup"><span data-stu-id="e6656-245">Modify the About View</span></span>

<span data-ttu-id="e6656-246">Nahraďte kód v *Views/Home/About.cshtml* soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e6656-246">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="e6656-247">Spusťte aplikaci a přejděte na stránku o.</span><span class="sxs-lookup"><span data-stu-id="e6656-247">Run the app and go to the About page.</span></span> <span data-ttu-id="e6656-248">Počet studenty pro každé datum registrace se zobrazí v tabulce.</span><span class="sxs-lookup"><span data-stu-id="e6656-248">The count of students for each enrollment date is displayed in a table.</span></span>

![O stránku](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="e6656-250">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e6656-250">Summary</span></span>

<span data-ttu-id="e6656-251">V tomto kurzu jste se seznámili jak provádět, řazení, filtrování, stránkování a seskupení.</span><span class="sxs-lookup"><span data-stu-id="e6656-251">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="e6656-252">V dalším kurzu budete zjistěte, jak zpracovávat změn datových modelů pomocí migrace.</span><span class="sxs-lookup"><span data-stu-id="e6656-252">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e6656-253">[Předchozí](crud.md)
[další](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="e6656-253">[Previous](crud.md)
[Next](migrations.md)</span></span>  
