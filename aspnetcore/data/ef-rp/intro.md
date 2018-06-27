---
title: Stránky Razor Entity Framework základní v ASP.NET Core - kurz 1 8
author: rick-anderson
description: Ukazuje, jak vytvořit aplikaci stránky Razor pomocí Entity Framework Core
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/intro
ms.openlocfilehash: 97ae8a0f268d3ca6fb2deee72128714ab6e89266
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961357"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="f7858-103">Stránky Razor Entity Framework základní v ASP.NET Core - kurz 1 8</span><span class="sxs-lookup"><span data-stu-id="f7858-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="f7858-104">ASP.NET 2.0 základní verzi v tomto kurzu lze nalézt v [tento PDF soubor](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).</span><span class="sxs-lookup"><span data-stu-id="f7858-104">The ASP.NET Core 2.0 version of this tutorial can be found in [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f7858-105">Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f7858-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f7858-106">Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikaci ASP.NET Core Razor stránky pomocí základní Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="f7858-106">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="f7858-107">Ukázková aplikace je web pro fiktivní vysoké školy Contoso.</span><span class="sxs-lookup"><span data-stu-id="f7858-107">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="f7858-108">Obsahuje funkce, jako je jejich příchodu student, postupu vytvoření a přiřazení lektorem.</span><span class="sxs-lookup"><span data-stu-id="f7858-108">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="f7858-109">Tato stránka je první ze série kurzů, které vysvětlují, jak k vytvoření ukázkové aplikace Contoso univerzity.</span><span class="sxs-lookup"><span data-stu-id="f7858-109">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="f7858-110">Stažení nebo zobrazení dokončené aplikace.</span><span class="sxs-lookup"><span data-stu-id="f7858-110">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="f7858-111">[Pokyny ke stahování](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="f7858-111">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f7858-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f7858-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f7858-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7858-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f7858-114">[! Zahrnout [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="f7858-114">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f7858-115">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="f7858-115">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f7858-116">[! [] – INCLUDE (~ / includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="f7858-116">[!INCLUDE [](~/includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span></span>

------

<span data-ttu-id="f7858-117">Znalost [stránky Razor](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="f7858-117">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="f7858-118">By se měla dokončit programátory [začít pracovat s stránky Razor](xref:tutorials/razor-pages/razor-pages-start) před zahájením této série.</span><span class="sxs-lookup"><span data-stu-id="f7858-118">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f7858-119">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="f7858-119">Troubleshooting</span></span>

<span data-ttu-id="f7858-120">Pokud narazíte na problém nevyřešíte, obvykle můžete najít řešení tak, že porovnáte svůj kód [dokončený projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="f7858-120">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="f7858-121">Je dobrým způsobem, jak získat nápovědu zveřejněním dotaz a [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) pro [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) nebo [EF základní](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="f7858-121">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="f7858-122">Contoso univerzity webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f7858-122">The Contoso University web app</span></span>

<span data-ttu-id="f7858-123">Aplikace vytvořené v těchto kurzech je základní univerzity webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="f7858-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="f7858-124">Uživatelé mohou zobrazit a aktualizovat studenty, během a lektorem informace.</span><span class="sxs-lookup"><span data-stu-id="f7858-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="f7858-125">Zde najdete několik z obrazovky v tomto kurzu vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f7858-125">Here are a few of the screens created in the tutorial.</span></span>

![Studenti, kteří indexovou stránku](intro/_static/students-index.png)

![Stránka upravit studenty](intro/_static/student-edit.png)

<span data-ttu-id="f7858-128">Styl uživatelského rozhraní této lokality je blízko co je generován integrované šablony.</span><span class="sxs-lookup"><span data-stu-id="f7858-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="f7858-129">Kurz zaměřuje se na základní EF s stránky Razor, uživatelské rozhraní ne.</span><span class="sxs-lookup"><span data-stu-id="f7858-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="f7858-130">Vytvoření stránky Razor ContosoUniversity webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f7858-130">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f7858-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7858-131">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f7858-132">Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="f7858-132">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f7858-133">Vytvořte novou webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f7858-133">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="f7858-134">Název projektu **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="f7858-134">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="f7858-135">Je třeba název projektu *ContosoUniversity* tak shoda s obory názvů, pokud kód je zkopírované a vložené.</span><span class="sxs-lookup"><span data-stu-id="f7858-135">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="f7858-136">Vyberte **ASP.NET Core 2.1** v rozevírací nabídce a potom vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f7858-136">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="f7858-137">Pro Image předchozích kroků, najdete v části [vytvořit webovou aplikaci Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).</span><span class="sxs-lookup"><span data-stu-id="f7858-137">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).</span></span>
<span data-ttu-id="f7858-138">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f7858-138">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f7858-139">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="f7858-139">.NET Core CLI</span></span>](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a><span data-ttu-id="f7858-140">Nastavení stylu lokality</span><span class="sxs-lookup"><span data-stu-id="f7858-140">Set up the site style</span></span>

<span data-ttu-id="f7858-141">Několik změny nastavení lokality nabídky, rozložení a domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="f7858-141">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="f7858-142">Aktualizace *Pages/Shared/_Layout.cshtml* s následujícími změnami:</span><span class="sxs-lookup"><span data-stu-id="f7858-142">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="f7858-143">Změňte všechny výskyty "ContosoUniversity" na "Univerzity Contoso".</span><span class="sxs-lookup"><span data-stu-id="f7858-143">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="f7858-144">Existují tři události.</span><span class="sxs-lookup"><span data-stu-id="f7858-144">There are three occurrences.</span></span>

* <span data-ttu-id="f7858-145">Přidání položek nabídky pro **studenty**, **kurzy**, **vyučující**, a **oddělení**a odstranit **kontaktujte** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="f7858-145">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="f7858-146">Změny se zvýrazněnou.</span><span class="sxs-lookup"><span data-stu-id="f7858-146">The changes are highlighted.</span></span> <span data-ttu-id="f7858-147">(Všechny značky se *není* zobrazit.)</span><span class="sxs-lookup"><span data-stu-id="f7858-147">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="f7858-148">V *Pages/Index.cshtml*, nahraďte obsah souboru následující kód, který nahradí text o ASP.NET a MVC o této aplikaci:</span><span class="sxs-lookup"><span data-stu-id="f7858-148">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="f7858-149">Vytvoření datového modelu</span><span class="sxs-lookup"><span data-stu-id="f7858-149">Create the data model</span></span>

<span data-ttu-id="f7858-150">Vytvoření třídy entity pro aplikaci univerzity Contoso.</span><span class="sxs-lookup"><span data-stu-id="f7858-150">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="f7858-151">Můžete začít s následující tři entity:</span><span class="sxs-lookup"><span data-stu-id="f7858-151">Start with the following three entities:</span></span>

![Diagram modelu dat během. registrace studenty](intro/_static/data-model-diagram.png)

<span data-ttu-id="f7858-153">Je vztah jeden mnoho mezi `Student` a `Enrollment` entity.</span><span class="sxs-lookup"><span data-stu-id="f7858-153">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="f7858-154">Je vztah jeden mnoho mezi `Course` a `Enrollment` entity.</span><span class="sxs-lookup"><span data-stu-id="f7858-154">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="f7858-155">Student může zaregistrovat libovolný počet kurzy.</span><span class="sxs-lookup"><span data-stu-id="f7858-155">A student can enroll in any number of courses.</span></span> <span data-ttu-id="f7858-156">Kurz může mít libovolný počet studenti, kteří jsou v ní zaregistrované.</span><span class="sxs-lookup"><span data-stu-id="f7858-156">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="f7858-157">V následujících částech se vytvoří třídu pro každé z nich o těchto entitách.</span><span class="sxs-lookup"><span data-stu-id="f7858-157">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="f7858-158">Student entity</span><span class="sxs-lookup"><span data-stu-id="f7858-158">The Student entity</span></span>

![Student entity diagram](intro/_static/student-entity.png)

<span data-ttu-id="f7858-160">Vytvoření *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="f7858-160">Create a *Models* folder.</span></span> <span data-ttu-id="f7858-161">V *modely* složky, vytvořte soubor třídy s názvem *Student.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f7858-161">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="f7858-162">`ID` Vlastnost stane sloupec primárního klíče tabulky databáze (databáze), která odpovídá této třídy.</span><span class="sxs-lookup"><span data-stu-id="f7858-162">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="f7858-163">Ve výchozím nastavení, EF základní interpretuje vlastnost s názvem `ID` nebo `classnameID` jako primární klíč.</span><span class="sxs-lookup"><span data-stu-id="f7858-163">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="f7858-164">V `classnameID`, `classname` je název třídy.</span><span class="sxs-lookup"><span data-stu-id="f7858-164">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="f7858-165">Alternativním automaticky rozpozná, primární klíč je `StudentID` v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="f7858-165">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="f7858-166">`Enrollments` Je navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f7858-166">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="f7858-167">Navigační vlastnosti propojit s dalšími subjekty, které se vztahují k této entity.</span><span class="sxs-lookup"><span data-stu-id="f7858-167">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="f7858-168">V takovém případě `Enrollments` vlastnost `Student entity` obsahuje všechny `Enrollment` entit, které se vztahují k které `Student`.</span><span class="sxs-lookup"><span data-stu-id="f7858-168">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="f7858-169">Například pokud studentů řádek v databázi existují dvě související registrace řádky, `Enrollments` navigační vlastnost obsahuje tyto dvě `Enrollment` entity.</span><span class="sxs-lookup"><span data-stu-id="f7858-169">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="f7858-170">Související `Enrollment` řádek je řádek, který obsahuje hodnotu primárního klíče tohoto Studentova v `StudentID` sloupce.</span><span class="sxs-lookup"><span data-stu-id="f7858-170">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="f7858-171">Předpokládejme například, student s ID = 1, má dva řádky v `Enrollment` tabulky.</span><span class="sxs-lookup"><span data-stu-id="f7858-171">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="f7858-172">`Enrollment` Tabulka má dva řádky s `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="f7858-172">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="f7858-173">`StudentID` cizí klíč v `Enrollment` tabulku, která určuje studenty ve `Student` tabulky.</span><span class="sxs-lookup"><span data-stu-id="f7858-173">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="f7858-174">Pokud navigační vlastnost může obsahovat více entit, navigační vlastnost musí být typu seznamu, jako například `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="f7858-174">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="f7858-175">`ICollection<T>` lze zadat, nebo typu, jako `List<T>` nebo `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="f7858-175">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="f7858-176">Když `ICollection<T>` se používá, vytvoří základní EF `HashSet<T>` kolekce ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f7858-176">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="f7858-177">Navigační vlastnosti, které mají více entit pocházet z relace m: n a jeden mnoho.</span><span class="sxs-lookup"><span data-stu-id="f7858-177">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="f7858-178">Registrace entity</span><span class="sxs-lookup"><span data-stu-id="f7858-178">The Enrollment entity</span></span>

![Diagram entity registrace](intro/_static/enrollment-entity.png)

<span data-ttu-id="f7858-180">V *modely* složku vytvořit *Enrollment.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f7858-180">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="f7858-181">`EnrollmentID` Vlastnost je primární klíč.</span><span class="sxs-lookup"><span data-stu-id="f7858-181">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="f7858-182">Používá tato entita `classnameID` vzor místo `ID` jako `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="f7858-182">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="f7858-183">Vývojáři obvykle zvolte jeden vzor a použít ho v rámci datového modelu.</span><span class="sxs-lookup"><span data-stu-id="f7858-183">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="f7858-184">Novější kurzu pomocí ID bez classname se zobrazí usnadňují implementace dědičnosti v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="f7858-184">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="f7858-185">`Grade` Vlastnost je `enum`.</span><span class="sxs-lookup"><span data-stu-id="f7858-185">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="f7858-186">Otazník po `Grade` deklaraci typu znamená, že `Grade` vlastnost umožňuje hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="f7858-186">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="f7858-187">Třída, která má hodnotu null se liší od nulové úrovni – null znamená úrovni není známý nebo ještě nebyly přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="f7858-187">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="f7858-188">`StudentID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Student`.</span><span class="sxs-lookup"><span data-stu-id="f7858-188">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="f7858-189">`Enrollment` Entita je spojen s jednou `Student` entit, tak vlastnost obsahuje jediný `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="f7858-189">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="f7858-190">`Student` Entity se liší od `Student.Enrollments` navigační vlastnost, která obsahuje více `Enrollment` entity.</span><span class="sxs-lookup"><span data-stu-id="f7858-190">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="f7858-191">`CourseID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Course`.</span><span class="sxs-lookup"><span data-stu-id="f7858-191">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="f7858-192">`Enrollment` Entita je spojen s jednou `Course` entity.</span><span class="sxs-lookup"><span data-stu-id="f7858-192">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="f7858-193">Vlastnost EF základní interpretuje jako cizí klíč, pokud je název `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="f7858-193">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="f7858-194">Například`StudentID` pro `Student` navigační vlastnost, protože `Student` primárního klíče entity je `ID`.</span><span class="sxs-lookup"><span data-stu-id="f7858-194">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="f7858-195">Vlastnosti cizího klíče může nazývat také `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="f7858-195">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="f7858-196">Například `CourseID` vzhledem k tomu `Course` je primární klíč entity `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="f7858-196">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="f7858-197">Během entity</span><span class="sxs-lookup"><span data-stu-id="f7858-197">The Course entity</span></span>

![Diagram postupu entity](intro/_static/course-entity.png)

<span data-ttu-id="f7858-199">V *modely* složku vytvořit *Course.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f7858-199">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="f7858-200">`Enrollments` Je navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f7858-200">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="f7858-201">A `Course` entity může souviset s libovolný počet `Enrollment` entity.</span><span class="sxs-lookup"><span data-stu-id="f7858-201">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="f7858-202">`DatabaseGenerated` Místo nutnosti databáze generovat atribut umožňuje aplikacím určit primární klíč.</span><span class="sxs-lookup"><span data-stu-id="f7858-202">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="f7858-203">Vygenerované uživatelské rozhraní student modelu</span><span class="sxs-lookup"><span data-stu-id="f7858-203">Scaffold the student model</span></span>

<span data-ttu-id="f7858-204">V této části se vygeneroval student modelu.</span><span class="sxs-lookup"><span data-stu-id="f7858-204">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="f7858-205">To znamená vytvoří nástroj pro generování uživatelského rozhraní stránky pro operace vytvoření, čtení, aktualizace a odstranění (CRUD) pro studenty model.</span><span class="sxs-lookup"><span data-stu-id="f7858-205">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="f7858-206">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="f7858-206">Build the project.</span></span>
* <span data-ttu-id="f7858-207">Vytvořte *stránek/studenty* složky.</span><span class="sxs-lookup"><span data-stu-id="f7858-207">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f7858-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7858-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f7858-209">V **Průzkumníku řešení**, klikněte pravým tlačítkem na *stránek/studenty* složky > **přidat** > **novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="f7858-209">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="f7858-210">V **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **stránky Razor pomocí Entity Framework (CRUD)** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f7858-210">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="f7858-211">Dokončení **přidat stránky Razor pomocí Entity Framework (CRUD)** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="f7858-211">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="f7858-212">V **třída modelu** rozevíracího seznamu, vyberte **Student (ContosoUniversity.Models)**.</span><span class="sxs-lookup"><span data-stu-id="f7858-212">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="f7858-213">V **třída kontextu dat** řádek, vyberte **+** (a) přihlásit a změňte vygenerovaný název **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="f7858-213">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="f7858-214">V **třída kontextu dat** rozevíracího seznamu, vyberte **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="f7858-214">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="f7858-215">Vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f7858-215">Select **Add**.</span></span>

![Dialogové okno CRUD](intro/_static/s1.png)

<span data-ttu-id="f7858-217">V tématu [vygenerovat model film](xref:tutorials/razor-pages/model#scaffold-the-movie-model) Pokud máte potíže s předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="f7858-217">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f7858-218">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="f7858-218">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f7858-219">Spusťte následující příkazy k automatickému vygenerování student modelu.</span><span class="sxs-lookup"><span data-stu-id="f7858-219">Run the following commands to scaffold the student model.</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

<span data-ttu-id="f7858-220">Proces vygenerované uživatelské rozhraní vytvořené a změněné následující soubory:</span><span class="sxs-lookup"><span data-stu-id="f7858-220">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="f7858-221">Vytvořené soubory</span><span class="sxs-lookup"><span data-stu-id="f7858-221">Files created</span></span>

* <span data-ttu-id="f7858-222">*Stránky/studenty* vytvořit, odstranit, podrobnosti, úpravy, Index.</span><span class="sxs-lookup"><span data-stu-id="f7858-222">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="f7858-223">*Data/ContosoUniversityContext.cs*</span><span class="sxs-lookup"><span data-stu-id="f7858-223">*Data/ContosoUniversityContext.cs*</span></span>

### <a name="files-updates"></a><span data-ttu-id="f7858-224">Soubory aktualizací</span><span class="sxs-lookup"><span data-stu-id="f7858-224">Files updates</span></span>

* <span data-ttu-id="f7858-225">*Startup.cs* : změny tohoto souboru v jsou podrobně popsané v další části.</span><span class="sxs-lookup"><span data-stu-id="f7858-225">*Startup.cs* : Changes to this file in are detailed the next section.</span></span>
* <span data-ttu-id="f7858-226">*appSettings.JSON určený* : Přidání připojovací řetězec použitý pro připojení k místní databázi.</span><span class="sxs-lookup"><span data-stu-id="f7858-226">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="f7858-227">Zkontrolujte kontext zaregistrována vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="f7858-227">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="f7858-228">ASP.NET Core je vytvořené s [vkládání závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f7858-228">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f7858-229">Služby (například kontext databáze základní EF) jsou registrovány vkládání závislostí při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="f7858-229">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="f7858-230">Součásti, které vyžadují tyto služby (například stránky Razor) jsou k dispozici tyto služby prostřednictvím konstruktor parametry.</span><span class="sxs-lookup"><span data-stu-id="f7858-230">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="f7858-231">Později v tomto kurzu se zobrazí kód konstruktor, který získá kontext instanci databáze.</span><span class="sxs-lookup"><span data-stu-id="f7858-231">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="f7858-232">Nástroj pro generování uživatelského rozhraní automaticky vytvořen kontext databáze a zaregistrována kontejneru pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="f7858-232">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="f7858-233">Zkontrolujte `ConfigureServices` metoda v *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="f7858-233">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="f7858-234">Zvýrazněný řádek byl přidán modulem scaffolder:</span><span class="sxs-lookup"><span data-stu-id="f7858-234">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="f7858-235">Název připojovacího řetězce, je předaná do kontextu voláním metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) objektu.</span><span class="sxs-lookup"><span data-stu-id="f7858-235">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="f7858-236">Pro místní vývoj [ASP.NET Core konfigurační systém](xref:fundamentals/configuration/index) čte připojovací řetězec z *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="f7858-236">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="f7858-237">Aktualizovat hlavní</span><span class="sxs-lookup"><span data-stu-id="f7858-237">Update main</span></span>

<span data-ttu-id="f7858-238">V *Program.cs*, změnit `Main` metoda proveďte následující:</span><span class="sxs-lookup"><span data-stu-id="f7858-238">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="f7858-239">Zjištění instance kontextu DB z kontejneru pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="f7858-239">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="f7858-240">Volání [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="f7858-240">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="f7858-241">Uvolnění kontextu při `EnsureCreated` metoda dokončí.</span><span class="sxs-lookup"><span data-stu-id="f7858-241">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="f7858-242">Následující kód ukazuje aktualizovaný *Program.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="f7858-242">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="f7858-243">`EnsureCreated` zajišťuje, že existuje databáze pro daný kontext.</span><span class="sxs-lookup"><span data-stu-id="f7858-243">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="f7858-244">Pokud existuje, nebyla provedena žádná akce.</span><span class="sxs-lookup"><span data-stu-id="f7858-244">If it exists, no action is taken.</span></span> <span data-ttu-id="f7858-245">Pokud neexistuje, pak se vytvoří databáze a všechny jeho schématu.</span><span class="sxs-lookup"><span data-stu-id="f7858-245">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="f7858-246">`EnsureCreated` k vytvoření databáze nepoužívá migrace.</span><span class="sxs-lookup"><span data-stu-id="f7858-246">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="f7858-247">Databázi, která je vytvořena s `EnsureCreated` nelze později aktualizovat pomocí migrace.</span><span class="sxs-lookup"><span data-stu-id="f7858-247">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="f7858-248">`EnsureCreated` je volána při spuštění aplikace, která umožňuje následující pracovní postup:</span><span class="sxs-lookup"><span data-stu-id="f7858-248">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="f7858-249">Odstranění databáze.</span><span class="sxs-lookup"><span data-stu-id="f7858-249">Delete the DB.</span></span>
* <span data-ttu-id="f7858-250">Změnit schéma databáze (například přidat `EmailAddress` pole).</span><span class="sxs-lookup"><span data-stu-id="f7858-250">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="f7858-251">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f7858-251">Run the app.</span></span>
* <span data-ttu-id="f7858-252">`EnsureCreated` vytvoří databáze s`EmailAddress` sloupce.</span><span class="sxs-lookup"><span data-stu-id="f7858-252">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="f7858-253">`EnsureCreated` je vhodné v rané fázi vývoj, pokud je schéma rychle vyvíjejí.</span><span class="sxs-lookup"><span data-stu-id="f7858-253">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="f7858-254">Později v tomto kurzu se odstraní databázi a migrace se používají.</span><span class="sxs-lookup"><span data-stu-id="f7858-254">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="f7858-255">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="f7858-255">Test the app</span></span>

<span data-ttu-id="f7858-256">Spusťte aplikaci a přijměte zásady souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="f7858-256">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="f7858-257">Tato aplikace nepodporuje uchovává osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="f7858-257">This app doesn't keep personal information.</span></span> <span data-ttu-id="f7858-258">Další informace o souboru cookie zásady v [podporu Evropa obecné Data Protection nařízení (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="f7858-258">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="f7858-259">Vyberte **studenty** odkazu a potom **vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="f7858-259">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="f7858-260">Upravit podrobnosti, testování a odstraňte odkazy.</span><span class="sxs-lookup"><span data-stu-id="f7858-260">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="f7858-261">Zkontrolujte kontext SchoolContext databáze</span><span class="sxs-lookup"><span data-stu-id="f7858-261">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="f7858-262">Hlavní třída, která koordinuje EF základní funkce pro daný datový model je třídy kontextu databáze.</span><span class="sxs-lookup"><span data-stu-id="f7858-262">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="f7858-263">Data kontextu je odvozený od [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="f7858-263">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="f7858-264">Data kontextu určuje entit, které jsou zahrnuty v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="f7858-264">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="f7858-265">V tomto projektu je třída s názvem `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="f7858-265">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="f7858-266">Aktualizace *SchoolContext.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f7858-266">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="f7858-267">Zvýrazněný kód vytvoří [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) vlastnosti pro každou sadu entit.</span><span class="sxs-lookup"><span data-stu-id="f7858-267">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="f7858-268">V terminologii EF jádra:</span><span class="sxs-lookup"><span data-stu-id="f7858-268">In EF Core terminology:</span></span>

* <span data-ttu-id="f7858-269">Obvykle sadu entit odpovídá Databázové tabulce.</span><span class="sxs-lookup"><span data-stu-id="f7858-269">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="f7858-270">Entity odpovídá na řádek v tabulce.</span><span class="sxs-lookup"><span data-stu-id="f7858-270">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="f7858-271">`DbSet<Enrollment>` a `DbSet<Course>` může být vynechán.</span><span class="sxs-lookup"><span data-stu-id="f7858-271">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="f7858-272">Základní EF zahrnuje je implicitně v protože `Student` odkazy na entity `Enrollment` entity a `Enrollment` odkazy na entity `Course` entity.</span><span class="sxs-lookup"><span data-stu-id="f7858-272">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="f7858-273">V tomto kurzu zachovat `DbSet<Enrollment>` a `DbSet<Course>` v `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="f7858-273">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="f7858-274">Databáze SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="f7858-274">SQL Server Express LocalDB</span></span>

<span data-ttu-id="f7858-275">Určuje připojovací řetězec [SQL serveru LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="f7858-275">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="f7858-276">LocalDB je Odlehčená verze SQL Server Express Database Engine a je určen pro vývoj aplikací, není použití v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f7858-276">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="f7858-277">LocalDB spustí na vyžádání a běží v uživatelském režimu, takže není žádná komplexní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f7858-277">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="f7858-278">Ve výchozím nastavení, vytvoří instanci LocalDB *.mdf* DB soubory `C:/Users/<user>` adresáře.</span><span class="sxs-lookup"><span data-stu-id="f7858-278">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="f7858-279">Přidat kód pro inicializaci databáze s testovací data</span><span class="sxs-lookup"><span data-stu-id="f7858-279">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="f7858-280">Základní EF vytvoří prázdná databáze.</span><span class="sxs-lookup"><span data-stu-id="f7858-280">EF Core creates an empty DB.</span></span> <span data-ttu-id="f7858-281">V této části `Initialize` metoda je zapsán do jeho naplnění testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="f7858-281">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="f7858-282">V *Data* složky, vytvořte nový soubor třídy s názvem *DbInitializer.cs* a přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="f7858-282">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="f7858-283">Kód kontroluje, zda existují jakékoli studenty v databázi.</span><span class="sxs-lookup"><span data-stu-id="f7858-283">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="f7858-284">Pokud nejsou žádné studenty v databázi, databázi je inicializován testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="f7858-284">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="f7858-285">Načte testovací data do pole místo `List<T>` kolekce za účelem optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="f7858-285">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="f7858-286">`EnsureCreated` Metoda automaticky vytvoří databázi pro kontext databáze.</span><span class="sxs-lookup"><span data-stu-id="f7858-286">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="f7858-287">Pokud existuje databáze, `EnsureCreated` vrátí beze změny databáze.</span><span class="sxs-lookup"><span data-stu-id="f7858-287">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="f7858-288">V *Program.cs*, změnit `Main` metoda k volání `Initialize`:</span><span class="sxs-lookup"><span data-stu-id="f7858-288">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

<span data-ttu-id="f7858-289">Odstraňte všechny záznamy studentů a restartujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f7858-289">Delete any student records and restart the app.</span></span> <span data-ttu-id="f7858-290">Pokud databáze není inicializován, nastavit bod přerušení `Initialize` a diagnostikovat problém.</span><span class="sxs-lookup"><span data-stu-id="f7858-290">If the DB is not initialized, set a break point in `Initialize` to diagnose the problem.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="f7858-291">Zobrazení databáze</span><span class="sxs-lookup"><span data-stu-id="f7858-291">View the DB</span></span>

<span data-ttu-id="f7858-292">Otevřete **Průzkumník objektů systému SQL Server** (SSOX) z **zobrazení** nabídky v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f7858-292">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="f7858-293">V SSOX, klikněte na **\MSSQLLocalDB (localdb) > databáze > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="f7858-293">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="f7858-294">Rozbalte **tabulky** uzlu.</span><span class="sxs-lookup"><span data-stu-id="f7858-294">Expand the **Tables** node.</span></span>

<span data-ttu-id="f7858-295">Klikněte pravým tlačítkem myši **Student** tabulky a klikněte na tlačítko **Data zobrazení** zobrazíte vytvořit sloupce a řádky vloženy do tabulky.</span><span class="sxs-lookup"><span data-stu-id="f7858-295">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="f7858-296">Asynchronní kódu</span><span class="sxs-lookup"><span data-stu-id="f7858-296">Asynchronous code</span></span>

<span data-ttu-id="f7858-297">Asynchronní programování je výchozí režim pro ASP.NET Core a EF jádra.</span><span class="sxs-lookup"><span data-stu-id="f7858-297">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="f7858-298">Webový server má omezený počet vláken, které jsou k dispozici, a v situacích, vysokého zatížení všech dostupných vláken může být používán.</span><span class="sxs-lookup"><span data-stu-id="f7858-298">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="f7858-299">Pokud k tomu dojde, server nemůže zpracovat nové žádosti o, dokud se uvolní vláken.</span><span class="sxs-lookup"><span data-stu-id="f7858-299">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="f7858-300">Synchronní kód může být vázáno mnoho vláken při jejich nejsou to skutečně veškerou práci, protože se čeká vstupně-výstupních operací na dokončení.</span><span class="sxs-lookup"><span data-stu-id="f7858-300">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="f7858-301">Asynchronní kód když proces čeká na vstupně-výstupních operací na dokončení, je jeho přístup z více vláken uvolněno pro server, aby používal pro zpracování dalších požadavků.</span><span class="sxs-lookup"><span data-stu-id="f7858-301">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="f7858-302">V důsledku toho asynchronní kódu umožňuje prostředky serveru použije efektivněji a serveru zapnutá, abyste zvládli větší provoz bez zpoždění.</span><span class="sxs-lookup"><span data-stu-id="f7858-302">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="f7858-303">Asynchronní kódu v době běhu zavést malé množství režijní náklady.</span><span class="sxs-lookup"><span data-stu-id="f7858-303">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="f7858-304">Pro situace, nízkým provozem stiskněte klávesu výkonu je nepatrné při pro intenzivní provoz situace, potenciální zlepšení výkonu je výrazně.</span><span class="sxs-lookup"><span data-stu-id="f7858-304">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="f7858-305">V následujícím kódu [asynchronní](/dotnet/csharp/language-reference/keywords/async) – klíčové slovo, `Task<T>` vrátit hodnotu, `await` – klíčové slovo, a `ToListAsync` metoda zkontrolujte kód provést asynchronně.</span><span class="sxs-lookup"><span data-stu-id="f7858-305">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="f7858-306">`async` – Klíčové slovo instruuje kompilátor, aby:</span><span class="sxs-lookup"><span data-stu-id="f7858-306">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="f7858-307">Generovat zpětných volání pro části těla metody.</span><span class="sxs-lookup"><span data-stu-id="f7858-307">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="f7858-308">Automaticky vytvářet [úloh](/dotnet/api/system.threading.tasks.task) objekt, který je vrácen.</span><span class="sxs-lookup"><span data-stu-id="f7858-308">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="f7858-309">Další informace najdete v tématu [úloh návratového typu](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="f7858-309">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="f7858-310">Implicitní návratový typ `Task` reprezentuje probíhající práce.</span><span class="sxs-lookup"><span data-stu-id="f7858-310">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="f7858-311">`await` – Klíčové slovo způsobí, že kompilátor metodu rozdělit na dvě části.</span><span class="sxs-lookup"><span data-stu-id="f7858-311">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="f7858-312">První část končí operace, které se spustí asynchronně.</span><span class="sxs-lookup"><span data-stu-id="f7858-312">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="f7858-313">Druhá část je uvést do metody zpětného volání, která je volána po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="f7858-313">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="f7858-314">`ToListAsync` je asynchronní verzi `ToList` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="f7858-314">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="f7858-315">Třeba mít na paměti při zápisu asynchronní kód, který používá základní EF několik věcí:</span><span class="sxs-lookup"><span data-stu-id="f7858-315">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="f7858-316">Pouze příkazy, které způsobily dotazy nebo příkazy k odeslání do databáze se spustí asynchronně.</span><span class="sxs-lookup"><span data-stu-id="f7858-316">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="f7858-317">Zahrnující, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, a `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="f7858-317">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="f7858-318">Neobsahuje příkazy, které právě změnit `IQueryable`, jako například `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="f7858-318">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="f7858-319">Kontextu EF jádra není zaručeno bezpečné používání vláken: nemáte pokusí provést více operací paralelně.</span><span class="sxs-lookup"><span data-stu-id="f7858-319">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="f7858-320">Abyste mohli využívat výhod výkonu asynchronní kódu, ověřte, že knihovna balíčky (například pro stránkování) používat asynchronní, pokud volají EF základní metody, které odesílají dotazy do databáze.</span><span class="sxs-lookup"><span data-stu-id="f7858-320">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="f7858-321">Další informace o asynchronní programování v rozhraní .NET najdete v tématu [přehled asynchronních](/dotnet/articles/standard/async) a [asynchronní programování pomocí modifikátoru async a operátoru await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="f7858-321">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="f7858-322">V dalším kurzu základní CRUD (vytvořit, číst, aktualizovat, odstraňovat) byla.</span><span class="sxs-lookup"><span data-stu-id="f7858-322">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="f7858-323">Next</span><span class="sxs-lookup"><span data-stu-id="f7858-323">Next</span></span>](xref:data/ef-rp/crud)