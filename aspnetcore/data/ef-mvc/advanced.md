---
title: "Jádro ASP.NET MVC s EF Core - rozšířené – 10 z 10"
author: tdykstra
description: "V tomto kurzu zavádí několik témat, které jsou vhodné pro zajímat, když přejdete mimo se základy vývoje webové aplikace ASP.NET, které používají základní Entity Framework."
keywords: "ASP.NET Core, Entity Framework Core, nezpracovaná sql, zkontrolujte sql, použitému vzoru, jednotky práci vzor, detekce automatické změn existující databáze"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 92a2986a-d005-4ff6-9559-6657fd466bb7
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/advanced
ms.openlocfilehash: d63502a32e38eb192b40f21f5cd57d20048154e3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a><span data-ttu-id="2ca19-104">Pokročilá témata – Základní EF s kurz k ASP.NET MVC jádra (10 10)</span><span class="sxs-lookup"><span data-stu-id="2ca19-104">Advanced topics - EF Core with ASP.NET Core MVC tutorial (10 of 10)</span></span>

<span data-ttu-id="2ca19-105">Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2ca19-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2ca19-106">Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2ca19-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="2ca19-107">Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).</span><span class="sxs-lookup"><span data-stu-id="2ca19-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="2ca19-108">V tomto kurzu předchozí implementována tabulky na hierarchii dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="2ca19-108">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="2ca19-109">V tomto kurzu zavádí několik témat, které jsou vhodné pro zajímat, když přejdete mimo základní informace o vývoj webových aplikací ASP.NET Core, které používají základní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2ca19-109">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

## <a name="raw-sql-queries"></a><span data-ttu-id="2ca19-110">Nezpracovaná SQL dotazy</span><span class="sxs-lookup"><span data-stu-id="2ca19-110">Raw SQL Queries</span></span>

<span data-ttu-id="2ca19-111">Jednou z výhod použití rozhraní Entity Framework je, že zabraňuje příkazů kódu příliš úzce na konkrétní metodu ukládání dat.</span><span class="sxs-lookup"><span data-stu-id="2ca19-111">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="2ca19-112">Dělá to pomocí generování SQL dotazy a příkazy, které také s není pro zápis sami.</span><span class="sxs-lookup"><span data-stu-id="2ca19-112">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="2ca19-113">Ale existují výjimečných scénáře, kdy budete muset spustit konkrétní dotazy SQL, které jste vytvořili ručně.</span><span class="sxs-lookup"><span data-stu-id="2ca19-113">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="2ca19-114">Pro tyto scénáře prvního rozhraní API sady Entity Framework kód obsahuje metody, které vám umožní předat příkazy SQL přímo do databáze.</span><span class="sxs-lookup"><span data-stu-id="2ca19-114">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="2ca19-115">Ve verzi 1.0 základní EF máte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="2ca19-115">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="2ca19-116">Použití `DbSet.FromSql` metoda pro dotazy, které vracejí typy entit.</span><span class="sxs-lookup"><span data-stu-id="2ca19-116">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="2ca19-117">Vrácených objektů musí být na typ očekávaný `DbSet` objektu a jsou automaticky sledovány objektem kontext databáze Pokud jste [vypnout sledování](crud.md#no-tracking-queries).</span><span class="sxs-lookup"><span data-stu-id="2ca19-117">The returned objects must be of the type expected by the `DbSet` object, and they are automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="2ca19-118">Použití `Database.ExecuteSqlCommand` pro příkazy nejsou dotazem.</span><span class="sxs-lookup"><span data-stu-id="2ca19-118">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="2ca19-119">Pokud potřebujete spustit dotaz, který vrátí typy, které nejsou entity, můžete pomocí připojení k databázi poskytované EF ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="2ca19-119">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="2ca19-120">Vrácená data není sledována kontext databáze, i když používáte tuto metodu pro načtení typů entit.</span><span class="sxs-lookup"><span data-stu-id="2ca19-120">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="2ca19-121">Jak platí vždy při spuštění příkazů SQL ve webové aplikaci, je nutné provést opatření k ochraně vaší lokality před útoky Injektáž SQL.</span><span class="sxs-lookup"><span data-stu-id="2ca19-121">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="2ca19-122">Jeden způsob, jak to udělat, je použití parametrických dotazů a ujistěte se, že řetězců odeslána webová stránka se nedá interpretovat jako příkazy SQL.</span><span class="sxs-lookup"><span data-stu-id="2ca19-122">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="2ca19-123">V tomto kurzu budete používat parametrizované dotazy při integraci uživatelský vstup do dotazu.</span><span class="sxs-lookup"><span data-stu-id="2ca19-123">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-that-returns-entities"></a><span data-ttu-id="2ca19-124">Volání dotaz, který vrací entity</span><span class="sxs-lookup"><span data-stu-id="2ca19-124">Call a query that returns entities</span></span>

<span data-ttu-id="2ca19-125">`DbSet<TEntity>` Třída poskytuje metodu, která můžete použít k provedení dotazu, který vrací entity typu `TEntity`.</span><span class="sxs-lookup"><span data-stu-id="2ca19-125">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="2ca19-126">Pokud chcete zobrazit, jak to funguje můžete budete změnit kód v `Details` metoda řadiče oddělení.</span><span class="sxs-lookup"><span data-stu-id="2ca19-126">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="2ca19-127">V *DepartmentsController.cs*v `Details` metoda, nahraďte kód, který načte oddělení s `FromSql` volání metody, jak je znázorněno v následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="2ca19-127">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

<span data-ttu-id="2ca19-128">Chcete-li ověřit, že nový kód funguje správně, vyberte **oddělení** kartu a potom **podrobnosti** pro jednu z oddělení.</span><span class="sxs-lookup"><span data-stu-id="2ca19-128">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![Podrobnosti o oddělení](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a><span data-ttu-id="2ca19-130">Volání dotaz, který vrátí jiné typy</span><span class="sxs-lookup"><span data-stu-id="2ca19-130">Call a query that returns other types</span></span>

<span data-ttu-id="2ca19-131">Dříve jste vytvořili mřížka student statistiky o stránky, která vám ukázal, počet studenty pro každé datum registrace.</span><span class="sxs-lookup"><span data-stu-id="2ca19-131">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="2ca19-132">Vy máte data ze sady entit studenty (`_context.Students`) a použít LINQ do projektu výsledky do seznamu `EnrollmentDateGroup` zobrazit objekty modelu.</span><span class="sxs-lookup"><span data-stu-id="2ca19-132">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="2ca19-133">Předpokládejme, že chcete zapsat SQL samotné místo pomocí LINQ.</span><span class="sxs-lookup"><span data-stu-id="2ca19-133">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="2ca19-134">Chcete-li provést, je nutné ke spuštění dotazu SQL, vrátí něco jiného než entity objektů.</span><span class="sxs-lookup"><span data-stu-id="2ca19-134">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="2ca19-135">Jeden způsob, jak to udělat v EF základní 1.0, je napsat kód ADO.NET a získat připojení k databázi z EF.</span><span class="sxs-lookup"><span data-stu-id="2ca19-135">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="2ca19-136">V *HomeController.cs*, nahraďte `About` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2ca19-136">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

<span data-ttu-id="2ca19-137">Přidat pomocí příkazu:</span><span class="sxs-lookup"><span data-stu-id="2ca19-137">Add a using statement:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

<span data-ttu-id="2ca19-138">Spusťte aplikaci a přejděte na stránku o.</span><span class="sxs-lookup"><span data-stu-id="2ca19-138">Run the app and go to the About page.</span></span> <span data-ttu-id="2ca19-139">Zobrazuje stejná data, která předtím.</span><span class="sxs-lookup"><span data-stu-id="2ca19-139">It displays the same data it did before.</span></span>

![O stránku](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="2ca19-141">Volání příkazu update jazyka</span><span class="sxs-lookup"><span data-stu-id="2ca19-141">Call an update query</span></span>

<span data-ttu-id="2ca19-142">Předpokládejme, že chcete provést globální změny v databázi, jako je například změna číslo kredity u každé kurzu Contoso univerzity správci.</span><span class="sxs-lookup"><span data-stu-id="2ca19-142">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="2ca19-143">Pokud univerzity má velký počet kurzy, je neefektivní je načtou všechny jako entity a měnit je jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="2ca19-143">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="2ca19-144">V této části budete implementovat na webové stránce, která umožňuje uživateli zadat faktor, podle kterého chcete změnit číslo kredity u všech kurzů a budete proveďte požadovanou změnu spuštěním příkazu SQL UPDATE.</span><span class="sxs-lookup"><span data-stu-id="2ca19-144">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="2ca19-145">Webová stránka bude vypadat jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="2ca19-145">The web page will look like the following illustration:</span></span>

![Kredity během stránku aktualizace](advanced/_static/update-credits.png)

<span data-ttu-id="2ca19-147">V *CoursesContoller.cs*, přidejte metody UpdateCourseCredits pro třídy MetadataExchangeClientMode a HttpPost:</span><span class="sxs-lookup"><span data-stu-id="2ca19-147">In *CoursesContoller.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

<span data-ttu-id="2ca19-148">Pokud kontroler zpracovává požadavek třídy MetadataExchangeClientMode, nic nevrátí v `ViewData["RowsAffected"]`, a zobrazení zobrazí prázdné textové pole a tlačítko pro odeslání, jak je vidět na předchozím obrázku.</span><span class="sxs-lookup"><span data-stu-id="2ca19-148">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="2ca19-149">Když **aktualizace** po kliknutí na tlačítko, volání metody HttpPost a násobitel má hodnota zadaná v textovém poli.</span><span class="sxs-lookup"><span data-stu-id="2ca19-149">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="2ca19-150">Kód pak provede SQL, který aktualizuje kurzy a vrátí počet ovlivněných řádků zobrazení v `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="2ca19-150">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="2ca19-151">Při zobrazení získá `RowsAffected` hodnotu, zobrazí počet aktualizované řádky.</span><span class="sxs-lookup"><span data-stu-id="2ca19-151">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="2ca19-152">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *zobrazení/kurzy* složku a pak klikněte na tlačítko **Přidat > Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="2ca19-152">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="2ca19-153">V **přidat novou položku** dialogové okno, klikněte na tlačítko **ASP.NET** pod **nainstalovaná** v levém podokně klikněte na **stránka zobrazení MVC**a název nového zobrazení  *UpdateCourseCredits.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2ca19-153">In the **Add New Item** dialog, click **ASP.NET** under **Installed** in the left pane, click **MVC View Page**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="2ca19-154">V *Views/Courses/UpdateCourseCredits.cshtml*, nahraďte kód šablony s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2ca19-154">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

<span data-ttu-id="2ca19-155">Spustit `UpdateCourseCredits` metoda výběrem **kurzy** kartě následným přidáním "/ UpdateCourseCredits" na konec adresy URL v adresním řádku prohlížeče (například: `http://localhost:5813/Courses/UpdateCourseCredits`).</span><span class="sxs-lookup"><span data-stu-id="2ca19-155">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="2ca19-156">Zadejte číslo do textového pole:</span><span class="sxs-lookup"><span data-stu-id="2ca19-156">Enter a number in the text box:</span></span>

![Kredity během stránku aktualizace](advanced/_static/update-credits.png)

<span data-ttu-id="2ca19-158">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="2ca19-158">Click **Update**.</span></span> <span data-ttu-id="2ca19-159">Zobrazí počet ovlivněných řádků:</span><span class="sxs-lookup"><span data-stu-id="2ca19-159">You see the number of rows affected:</span></span>

![Počet ovlivněných řádků kredity během stránku aktualizace](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="2ca19-161">Klikněte na tlačítko **zpět do seznamu** zobrazíte seznam kurzy revidovaný počet kredity.</span><span class="sxs-lookup"><span data-stu-id="2ca19-161">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="2ca19-162">Všimněte si, že produkčním kódu by zajistěte, aby, aktualizuje vždy vést platná data.</span><span class="sxs-lookup"><span data-stu-id="2ca19-162">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="2ca19-163">Kód zjednodušené zobrazeny zde může vynásobte počet kredity dostatečně za následek větší než 5 číslic.</span><span class="sxs-lookup"><span data-stu-id="2ca19-163">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="2ca19-164">( `Credits` Vlastnost má `[Range(0, 5)]` atributů.) Aktualizace dotazu by fungovat, ale neplatná data může vést k neočekávaným výsledkům v dalších částech tohoto systému, které předpokládají, že počet kredity je 5 nebo méně.</span><span class="sxs-lookup"><span data-stu-id="2ca19-164">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="2ca19-165">Další informace o nezpracovanou dotazy SQL najdete v tématu [nezpracovaná dotazy SQL](https://docs.microsoft.com/ef/core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="2ca19-165">For more information about raw SQL queries, see [Raw SQL Queries](https://docs.microsoft.com/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-sent-to-the-database"></a><span data-ttu-id="2ca19-166">Zkontrolujte odeslal do databáze SQL</span><span class="sxs-lookup"><span data-stu-id="2ca19-166">Examine SQL sent to the database</span></span>

<span data-ttu-id="2ca19-167">V některých případech je vhodné se moci zobrazit skutečné dotazy SQL, které se odesílají do databáze.</span><span class="sxs-lookup"><span data-stu-id="2ca19-167">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="2ca19-168">Funkce integrované protokolování pro ASP.NET Core automaticky použije EF základní zápis protokolů, které obsahují SQL pro dotazy a aktualizace.</span><span class="sxs-lookup"><span data-stu-id="2ca19-168">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="2ca19-169">V této části se zobrazí některé příklady protokolování SQL.</span><span class="sxs-lookup"><span data-stu-id="2ca19-169">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="2ca19-170">Otevřete *StudentsController.cs* a v `Details` metoda nastavit zarážky `if (student == null)` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2ca19-170">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="2ca19-171">Spuštění aplikace v režimu ladění a přejděte na stránku podrobností pro student.</span><span class="sxs-lookup"><span data-stu-id="2ca19-171">Run the app in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="2ca19-172">Přejděte na **výstup** okno zobrazení ladění výstupu a zobrazit dotaz:</span><span class="sxs-lookup"><span data-stu-id="2ca19-172">Go to the **Output** window showing debug output, and you see the query:</span></span>

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

<span data-ttu-id="2ca19-173">Můžete si všimnout něco tady, ať vás může překvapí: až 2 řádků vybere SQL (`TOP(2)`) z tabulky osob.</span><span class="sxs-lookup"><span data-stu-id="2ca19-173">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="2ca19-174">`SingleOrDefaultAsync` Metoda se nepřekládá na 1 řádek na serveru.</span><span class="sxs-lookup"><span data-stu-id="2ca19-174">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="2ca19-175">Tady je důvod, proč:</span><span class="sxs-lookup"><span data-stu-id="2ca19-175">Here's why:</span></span>

* <span data-ttu-id="2ca19-176">Pokud dotaz vrátí více řádků, metoda vrátí hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2ca19-176">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="2ca19-177">Pokud chcete zjistit, jestli dotaz vrátí více řádků, EF je nutné zkontrolovat, když vrátí alespoň 2.</span><span class="sxs-lookup"><span data-stu-id="2ca19-177">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="2ca19-178">Všimněte si, že nemusíte používat režim ladění a zastavit zarážku získat výstup protokolování v **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="2ca19-178">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="2ca19-179">Je jenom pohodlný způsob zastavení protokolování v místě, které se chcete podívat na výstup.</span><span class="sxs-lookup"><span data-stu-id="2ca19-179">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="2ca19-180">Pokud jste si udělat, pokračuje protokolování a budete muset posuňte se zpět k vyhledání částí, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="2ca19-180">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="2ca19-181">Úložiště a jednotky pracovních vzorů</span><span class="sxs-lookup"><span data-stu-id="2ca19-181">Repository and unit of work patterns</span></span>

<span data-ttu-id="2ca19-182">Celá řada vývojářů napsat kód pro implementaci úložiště a jednotky pracovních vzorů jako obálku kolem kód, který funguje s platformou Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2ca19-182">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="2ca19-183">Tyto vzory jsou určeny k vytvoření vrstvy abstrakce mezi vrstva přístupu k datům a vrstvu obchodní logiky aplikace.</span><span class="sxs-lookup"><span data-stu-id="2ca19-183">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="2ca19-184">Implementace tyto vzory pomůžou izolovat aplikace od změny v úložišti dat a mohou usnadnit automatizované testování částí nebo testy řízený vývoj (TDD).</span><span class="sxs-lookup"><span data-stu-id="2ca19-184">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="2ca19-185">Ale zápis další kód implementovat tyto vzory není vždy je nejlepší pro aplikace, které používají EF, z několika důvodů:</span><span class="sxs-lookup"><span data-stu-id="2ca19-185">However, writing additional code to implement these patterns is not always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="2ca19-186">Vlastní třídy kontextu EF insulates kódu z daného obchodu data kódu.</span><span class="sxs-lookup"><span data-stu-id="2ca19-186">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="2ca19-187">Třída kontextu EF může fungovat také třídu pracovní jednotky pro databázi aktualizace, který používáte pomocí EF.</span><span class="sxs-lookup"><span data-stu-id="2ca19-187">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="2ca19-188">EF obsahuje funkce pro implementaci TDD bez nutnosti psaní kódu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ca19-188">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="2ca19-189">Informace o tom, jak implementovat úložiště a jednotky pracovních vzorů najdete v tématu [verze Entity Framework 5 tohoto kurzu řady](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="2ca19-189">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="2ca19-190">Entity Framework Core implementuje poskytovatele databáze v paměti, který lze použít pro testování.</span><span class="sxs-lookup"><span data-stu-id="2ca19-190">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="2ca19-191">Další informace najdete v tématu [testování pomocí InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).</span><span class="sxs-lookup"><span data-stu-id="2ca19-191">For more information, see [Testing with InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="2ca19-192">Detekce automatických změn</span><span class="sxs-lookup"><span data-stu-id="2ca19-192">Automatic change detection</span></span>

<span data-ttu-id="2ca19-193">Rozhraní Entity Framework Určuje, jak došlo ke změně entity (a proto aktualizací, které potřebují k odeslání do databáze) tak, že porovnáte aktuální hodnoty entity s původními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="2ca19-193">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="2ca19-194">Původní hodnoty jsou uloženy, když se entita dotaz nebo připojené.</span><span class="sxs-lookup"><span data-stu-id="2ca19-194">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="2ca19-195">Některé metody, které způsobí změnu automatické zjišťování jsou následující:</span><span class="sxs-lookup"><span data-stu-id="2ca19-195">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="2ca19-196">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="2ca19-196">DbContext.SaveChanges</span></span>

* <span data-ttu-id="2ca19-197">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="2ca19-197">DbContext.Entry</span></span>

* <span data-ttu-id="2ca19-198">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="2ca19-198">ChangeTracker.Entries</span></span>

<span data-ttu-id="2ca19-199">Pokud sledujete velký počet entit a jednu z těchto metod zavoláte mnohokrát ve smyčce, můžete dosáhnout výrazné vylepšení výkonu dočasně vypnout pomocí zjišťování automatické změny `ChangeTracker.AutoDetectChangesEnabled` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2ca19-199">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="2ca19-200">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2ca19-200">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a><span data-ttu-id="2ca19-201">Plány Entity Framework Core zdrojového kódu a vývoj</span><span class="sxs-lookup"><span data-stu-id="2ca19-201">Entity Framework Core source code and development plans</span></span>

<span data-ttu-id="2ca19-202">Zdrojový kód pro Entity Framework Core je k dispozici na [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="2ca19-202">The source code for Entity Framework Core is available at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="2ca19-203">Kromě zdrojového kódu, můžete získat noční sestavení, sledování problémů, funkci specifikace, návrh poznámky ze schůzky, [plán pro budoucí vývoj](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap)a další.</span><span class="sxs-lookup"><span data-stu-id="2ca19-203">Besides source code, you can get nightly builds, issue tracking, feature specs, design meeting notes, [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap), and more.</span></span> <span data-ttu-id="2ca19-204">Můžete soubor chyby, a můžete přispívat vlastní vylepšení EF zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="2ca19-204">You can file bugs, and you can contribute your own enhancements to the EF source code.</span></span>

<span data-ttu-id="2ca19-205">I když zdrojový kód je otevřená, Entity Framework Core je plně podporovaný jako produkt společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2ca19-205">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="2ca19-206">Týmem Microsoft Entity Framework udržuje řízení, po kterou jsou přijímány příspěvky a otestuje všechny změny kódu k zajištění kvality jednotlivými verzemi.</span><span class="sxs-lookup"><span data-stu-id="2ca19-206">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="2ca19-207">Zpětné analýzy z existující databáze</span><span class="sxs-lookup"><span data-stu-id="2ca19-207">Reverse engineer from existing database</span></span>

<span data-ttu-id="2ca19-208">Pro zpětnou datového modelu, včetně tříd entit z existující databáze, použijte [vygenerované uživatelské rozhraní dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) příkaz.</span><span class="sxs-lookup"><span data-stu-id="2ca19-208">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="2ca19-209">Najdete v článku [kurzu Začínáme](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="2ca19-209">See the [getting-started tutorial](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a><span data-ttu-id="2ca19-210">Použití dynamické LINQ ke zjednodušení řazení výběr kódu</span><span class="sxs-lookup"><span data-stu-id="2ca19-210">Use dynamic LINQ to simplify sort selection code</span></span>

<span data-ttu-id="2ca19-211">[Třetí kurzu této série](sort-filter-page.md) ukazuje, jak napsat kód LINQ podle pevně kódováno názvy sloupců v `switch` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2ca19-211">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="2ca19-212">Se dvěma sloupci můžete vybírat funguje bez problémů, ale pokud máte mnoho sloupců kód může získat podrobné.</span><span class="sxs-lookup"><span data-stu-id="2ca19-212">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="2ca19-213">Chcete-li tento problém vyřešit, můžete použít `EF.Property` metoda zadat název vlastnosti jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="2ca19-213">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="2ca19-214">Chcete-li vyzkoušet tento přístup, nahraďte `Index` metoda v `StudentsController` následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="2ca19-214">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a><span data-ttu-id="2ca19-215">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2ca19-215">Next steps</span></span>

<span data-ttu-id="2ca19-216">Dokončení této série kurzů na použití Entity Framework Core v aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2ca19-216">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET MVC application.</span></span>

<span data-ttu-id="2ca19-217">Další informace o základní EF najdete v tématu [Entity Framework základní dokumentace](https://docs.microsoft.com/ef/core).</span><span class="sxs-lookup"><span data-stu-id="2ca19-217">For more information about EF Core, see the [Entity Framework Core documentation](https://docs.microsoft.com/ef/core).</span></span> <span data-ttu-id="2ca19-218">Knihy je také k dispozici: [Entity Framework Core v akci](https://www.manning.com/books/entity-framework-core-in-action).</span><span class="sxs-lookup"><span data-stu-id="2ca19-218">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="2ca19-219">Informace o tom, jak nasadit webovou aplikaci, až když jste sestavili najdete v tématu [publikování a nasazení](../../publishing/index.md).</span><span class="sxs-lookup"><span data-stu-id="2ca19-219">For information about how to deploy your web application after you've built it, see [Publishing and deployment](../../publishing/index.md).</span></span>

<span data-ttu-id="2ca19-220">Informace o dalších témat souvisejících s ASP.NET MVC jádra, jako je například ověřování a autorizaci, najdete v článku [ASP.NET Core dokumentaci](https://docs.microsoft.com/aspnet/core/).</span><span class="sxs-lookup"><span data-stu-id="2ca19-220">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="2ca19-221">Potvrzování</span><span class="sxs-lookup"><span data-stu-id="2ca19-221">Acknowledgments</span></span>

<span data-ttu-id="2ca19-222">Tní Dykstra a Rick Anderson (twitter @RickAndMSFT) napsali v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="2ca19-222">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="2ca19-223">Rowan Lukeš, Diego Vega a ostatní členové týmu rozhraní Entity Framework s asistencí s recenze kódu a pomohl ladění problémů, které vznikly při jsme byly pro kurzů k psaní kódu.</span><span class="sxs-lookup"><span data-stu-id="2ca19-223">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span>

## <a name="common-errors"></a><span data-ttu-id="2ca19-224">Běžné chyby</span><span class="sxs-lookup"><span data-stu-id="2ca19-224">Common errors</span></span>  

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="2ca19-225">ContosoUniversity.dll využívá jiný proces</span><span class="sxs-lookup"><span data-stu-id="2ca19-225">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="2ca19-226">Chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="2ca19-226">Error message:</span></span>

> <span data-ttu-id="2ca19-227">Nelze otevřít.... bin\Debug\netcoreapp1.0\ContosoUniversity.dll' pro zápis – ' proces nemá přístup k souboru '... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll, protože je stále používán jiným procesem.</span><span class="sxs-lookup"><span data-stu-id="2ca19-227">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="2ca19-228">Řešení:</span><span class="sxs-lookup"><span data-stu-id="2ca19-228">Solution:</span></span>

<span data-ttu-id="2ca19-229">Zastavte lokalitu ve službě IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2ca19-229">Stop the site in IIS Express.</span></span> <span data-ttu-id="2ca19-230">Přejděte k panelu systému Windows najít službu IIS Express, klikněte pravým tlačítkem na ikonu, vyberte lokalitu, univerzity Contoso a potom klikněte **zastavit lokality**.</span><span class="sxs-lookup"><span data-stu-id="2ca19-230">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="2ca19-231">Migrace vygeneroval s žádný kód v nahoru a dolů metody</span><span class="sxs-lookup"><span data-stu-id="2ca19-231">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="2ca19-232">Možná příčina:</span><span class="sxs-lookup"><span data-stu-id="2ca19-232">Possible cause:</span></span>

<span data-ttu-id="2ca19-233">Příkazy rozhraní příkazového řádku EF není automaticky zavřete a uložte soubory kódu.</span><span class="sxs-lookup"><span data-stu-id="2ca19-233">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="2ca19-234">Pokud máte neuložené změny při spuštění `migrations add` příkaz EF nebude možné najít změny.</span><span class="sxs-lookup"><span data-stu-id="2ca19-234">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="2ca19-235">Řešení:</span><span class="sxs-lookup"><span data-stu-id="2ca19-235">Solution:</span></span>

<span data-ttu-id="2ca19-236">Spustit `migrations remove` příkaz, uložte změny kódu a znovu spusťte `migrations add` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2ca19-236">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="2ca19-237">Při spuštěné aktualizaci databáze došlo k chybám</span><span class="sxs-lookup"><span data-stu-id="2ca19-237">Errors while running database update</span></span>

<span data-ttu-id="2ca19-238">Je možné získat další chyby při provádění změn schématu v databázi, která obsahuje stávající data.</span><span class="sxs-lookup"><span data-stu-id="2ca19-238">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="2ca19-239">Pokud dojde k chybám migrace, které nelze vyřešit, můžete změnit název databáze v připojovacím řetězci nebo odstraňte tuto databázi.</span><span class="sxs-lookup"><span data-stu-id="2ca19-239">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="2ca19-240">S novou databázi nejsou žádná data k migraci a příkazu update-database je mnohem pravděpodobnější dokončeno bez chyb.</span><span class="sxs-lookup"><span data-stu-id="2ca19-240">With a new database, there is no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="2ca19-241">Nejjednodušší způsob je přejmenovat databázi v *appSettings.JSON určený*.</span><span class="sxs-lookup"><span data-stu-id="2ca19-241">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="2ca19-242">Při příštím spuštění `database update`, bude vytvořena nová databáze.</span><span class="sxs-lookup"><span data-stu-id="2ca19-242">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="2ca19-243">Chcete-li odstranit databázi v SSOX, klikněte pravým tlačítkem na databázi, klikněte na **odstranit**a potom v **odstranit databázi** dialogové okno pole vyberte **ukončete stávající připojení** a klikněte na tlačítko  **OK**.</span><span class="sxs-lookup"><span data-stu-id="2ca19-243">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="2ca19-244">Chcete-li odstranit databázi pomocí rozhraní příkazového řádku, spusťte `database drop` rozhraní příkazového řádku příkaz:</span><span class="sxs-lookup"><span data-stu-id="2ca19-244">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="2ca19-245">Chyba vyhledáním instance systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="2ca19-245">Error locating SQL Server instance</span></span>

<span data-ttu-id="2ca19-246">Chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="2ca19-246">Error Message:</span></span>

> <span data-ttu-id="2ca19-247">Při navazování připojení k systému SQL Server došlo k chybě související se sítí nebo konkrétní instancí.</span><span class="sxs-lookup"><span data-stu-id="2ca19-247">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="2ca19-248">Server nebyl nalezen nebo není přístupný.</span><span class="sxs-lookup"><span data-stu-id="2ca19-248">The server was not found or was not accessible.</span></span> <span data-ttu-id="2ca19-249">Ověřte, zda je název instance správný a zda systém SQL Server je nakonfigurován k povolení vzdálených připojení.</span><span class="sxs-lookup"><span data-stu-id="2ca19-249">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="2ca19-250">(Zprostředkovatel: rozhraní sítě systému SQL, chyba: 26 - chyba vyhledání Server nebo instanci zadáno)</span><span class="sxs-lookup"><span data-stu-id="2ca19-250">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="2ca19-251">Řešení:</span><span class="sxs-lookup"><span data-stu-id="2ca19-251">Solution:</span></span>

<span data-ttu-id="2ca19-252">Zkontrolujte připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="2ca19-252">Check the connection string.</span></span> <span data-ttu-id="2ca19-253">Pokud jste odstranili ručně databázový soubor, změňte název databáze v řetězci konstrukce začít znovu s novou databázi.</span><span class="sxs-lookup"><span data-stu-id="2ca19-253">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="2ca19-254">Předchozí</span><span class="sxs-lookup"><span data-stu-id="2ca19-254">Previous</span></span>](inheritance.md)
