---
title: "Stránky Razor EF základní - aktualizace související Data - 7, 8"
author: rick-anderson
description: "V tomto kurzu aktualizujete souvisejících dat tím, že aktualizuje polí cizího klíče a navigační vlastnosti."
keywords: "Spojí ASP.NET Core Entity Framework Core, související data"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: f07a33c19ba1be623fae14228f8fbc909d766816
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/02/2017
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a><span data-ttu-id="dd3b7-104">Aktualizace související data - stránky Razor základní EF (7 8)</span><span class="sxs-lookup"><span data-stu-id="dd3b7-104">Updating related data - EF Core Razor Pages (7 of 8)</span></span>

<span data-ttu-id="dd3b7-105">Podle [tní Dykstra](https://github.com/tdykstra), a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dd3b7-105">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="dd3b7-106">Tento kurz představuje aktualizaci související data.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-106">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="dd3b7-107">Pokud narazíte na problémy, které nelze vyřešit, stáhněte si [dokončené aplikace pro tuto fázi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span><span class="sxs-lookup"><span data-stu-id="dd3b7-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="dd3b7-108">Následující ilustrace znázorňuje dokončené stránky.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-108">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="dd3b7-109">![Upravit stránku kurzu](update-related-data/_static/course-edit.png)
![lektorem upravit stránku](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="dd3b7-109">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="dd3b7-110">Zkontrolujte a testovat stránky postupu vytvoření a úpravy.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-110">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="dd3b7-111">Vytvořte nový kurz.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-111">Create a new course.</span></span> <span data-ttu-id="dd3b7-112">Oddělení je vybrána jako svůj primární klíč (celé číslo), nikoli jeho název.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-112">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="dd3b7-113">Upravte nový kurz.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-113">Edit the new course.</span></span> <span data-ttu-id="dd3b7-114">Po dokončení testování odstraňte nový kurz.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-114">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="dd3b7-115">Vytvořte základní třídu sdílet společný kód</span><span class="sxs-lookup"><span data-stu-id="dd3b7-115">Create a base class to share common code</span></span>

<span data-ttu-id="dd3b7-116">Stránky kurzy a vytvořit a kurzy či upravit třeba seznam názvů oddělení.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-116">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="dd3b7-117">Vytvořit *Pages/Courses/DepartmentNamePageModel.cshtml.cs* základní třída pro vytvoření a úpravy stránky:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-117">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="dd3b7-118">Předchozí kód vytvoří [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) obsahovat seznam názvů oddělení.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-118">The preceding code creates a [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="dd3b7-119">Pokud `selectedDepartment` není zadaný, oddělení je vybrána v `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-119">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="dd3b7-120">Vytvořit a upravit třídy modelu stránky odvodí z `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-120">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="dd3b7-121">Přizpůsobení kurzy stránky</span><span class="sxs-lookup"><span data-stu-id="dd3b7-121">Customize the Courses Pages</span></span>

<span data-ttu-id="dd3b7-122">Při vytvoření nové entity kurzu, musí mít relaci s existující oddělení.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="dd3b7-123">Při vytváření kurzu přidat oddělení, obsahuje základní třídu pro vytvoření a úpravy rozevíracího seznamu pro výběr z oddělení.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-123">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="dd3b7-124">Rozevíracím seznamu sad `Course.DepartmentID` vlastnosti cizího klíče (Cizíklíč).</span><span class="sxs-lookup"><span data-stu-id="dd3b7-124">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="dd3b7-125">Základní EF používá `Course.DepartmentID` Cizíklíč načíst `Department` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-125">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Vytvořit kurz](update-related-data/_static/ddl.png)

<span data-ttu-id="dd3b7-127">Aktualizace modelu vytvořit stránku s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-127">Update the Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

<span data-ttu-id="dd3b7-128">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-128">The preceding code:</span></span>

* <span data-ttu-id="dd3b7-129">Odvozená z `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-129">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="dd3b7-130">Používá `TryUpdateModelAsync` aby [overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="dd3b7-130">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="dd3b7-131">Nahradí `ViewData["DepartmentID"]` s `DepartmentNameSL` (ze základní třídy).</span><span class="sxs-lookup"><span data-stu-id="dd3b7-131">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="dd3b7-132">`ViewData["DepartmentID"]`nahradí se silnými typy `DepartmentNameSL`.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-132">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="dd3b7-133">Silného typu modely jsou upřednostňovaná přes slabě typované.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-133">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="dd3b7-134">Další informace najdete v tématu [slabě typované data (ViewData a ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="dd3b7-134">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="dd3b7-135">Kurzy vytvořit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="dd3b7-135">Update the Courses Create page</span></span>

<span data-ttu-id="dd3b7-136">Aktualizace *Pages/Courses/Create.cshtml* s následující kód:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-136">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="dd3b7-137">Předchozí kód provede tyto změny:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-137">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="dd3b7-138">Změní titulek z **DepartmentID** k **oddělení**.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-138">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="dd3b7-139">Nahradí `"ViewBag.DepartmentID"` s `DepartmentNameSL` (ze základní třídy).</span><span class="sxs-lookup"><span data-stu-id="dd3b7-139">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="dd3b7-140">Přidá možnost "Vyberte oddělení".</span><span class="sxs-lookup"><span data-stu-id="dd3b7-140">Adds the "Select Department" option.</span></span> <span data-ttu-id="dd3b7-141">Tato změna vykreslí "Vyberte oddělení" místo první oddělení.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-141">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="dd3b7-142">Přidá ověřovací zprávu, pokud není vybraná oddělení.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-142">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="dd3b7-143">Stránka Razor používá [vyberte pomocná značky](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="dd3b7-143">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="dd3b7-144">Testovací stránka pro vytvoření.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-144">Test the Create page.</span></span> <span data-ttu-id="dd3b7-145">Na stránce vytvořit zobrazuje název oddělení, nikoli ID oddělení.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-145">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="dd3b7-146">Aktualizujte kurzy upravit stránku.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-146">Update the Courses Edit page.</span></span>

<span data-ttu-id="dd3b7-147">Aktualizace modelu stránky upravit následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-147">Update the edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

<span data-ttu-id="dd3b7-148">Změny jsou podobné těm, které jsou provedeny v modelu vytvořit stránku.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-148">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="dd3b7-149">V předchozí kód `PopulateDepartmentsDropDownList` předává v oddělení ID, které oddělení, zadaný v rozevíracím seznamu vyberte.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-149">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="dd3b7-150">Aktualizace *Pages/Courses/Edit.cshtml* s následující kód:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-150">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="dd3b7-151">Předchozí kód provede tyto změny:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-151">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="dd3b7-152">Zobrazí ID kurzu.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-152">Displays the course ID.</span></span> <span data-ttu-id="dd3b7-153">Obecně se nezobrazí, primární klíč (Primárníklíč) entity.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-153">Generally the Primary Key (PK) of an entity is not displayed.</span></span> <span data-ttu-id="dd3b7-154">PKs jsou obvykle smysl pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-154">PKs are usually meaningless to users.</span></span> <span data-ttu-id="dd3b7-155">V takovém případě primárnímu Klíči je číslo kurzu.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-155">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="dd3b7-156">Změní titulek z **DepartmentID** k **oddělení**.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-156">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="dd3b7-157">Nahradí `"ViewBag.DepartmentID"` s `DepartmentNameSL` (ze základní třídy).</span><span class="sxs-lookup"><span data-stu-id="dd3b7-157">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="dd3b7-158">Přidá možnost "Vyberte oddělení".</span><span class="sxs-lookup"><span data-stu-id="dd3b7-158">Adds the "Select Department" option.</span></span> <span data-ttu-id="dd3b7-159">Tato změna vykreslí "Vyberte oddělení" místo první oddělení.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-159">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="dd3b7-160">Přidá ověřovací zprávu, pokud není vybraná oddělení.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-160">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="dd3b7-161">Tato stránka obsahuje skryté pole (`<input type="hidden">`) pro číslo kurzu.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-161">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="dd3b7-162">Přidání `<label>` značky pomocnou metodu s `asp-for="Course.CourseID"` není eliminují nutnost použití skryté pole.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-162">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="dd3b7-163">`<input type="hidden">`je vyžadována pro kurzu číslo, které má být součástí odeslaných dat, když uživatel klikne **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-163">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="dd3b7-164">Otestujte aktualizovaný kódu.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-164">Test the updated code.</span></span> <span data-ttu-id="dd3b7-165">Vytvářet, upravovat a odstraňovat kurzu.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-165">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="dd3b7-166">Podrobnosti o přidání AsNoTracking a odstraňovat modely stránky</span><span class="sxs-lookup"><span data-stu-id="dd3b7-166">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="dd3b7-167">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) můžete zvýšit výkon při sledování se nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-167">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking is not required.</span></span> <span data-ttu-id="dd3b7-168">Přidat `AsNoTracking` stránky modelu odstranit a podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-168">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="dd3b7-169">Následující kód ukazuje v aktualizovaném modelu. odstranění stránky:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-169">The following code shows the updated Delete page model:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="dd3b7-170">Aktualizace `OnGetAsync` metoda v *Pages/Courses/Details.cshtml.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-170">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="dd3b7-171">Upravovat stránky odstranit a podrobnosti</span><span class="sxs-lookup"><span data-stu-id="dd3b7-171">Modify the Delete and Details pages</span></span>

<span data-ttu-id="dd3b7-172">Aktualizujte stránku odstranit Razor následující kód:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-172">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="dd3b7-173">Proveďte stejné změny na stránku podrobností.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-173">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="dd3b7-174">Testování postupu stránek</span><span class="sxs-lookup"><span data-stu-id="dd3b7-174">Test the Course pages</span></span>

<span data-ttu-id="dd3b7-175">Test vytvořit, upravit, podrobnosti a odstranění.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-175">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="dd3b7-176">Aktualizovat lektorem stránky</span><span class="sxs-lookup"><span data-stu-id="dd3b7-176">Update the instructor pages</span></span>

<span data-ttu-id="dd3b7-177">V následujících částech aktualizovat lektorem stránky.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-177">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="dd3b7-178">Přidat umístění kanceláře</span><span class="sxs-lookup"><span data-stu-id="dd3b7-178">Add office location</span></span>

<span data-ttu-id="dd3b7-179">Při úpravě záznamu lektorem, můžete aktualizovat lektorem office přiřazení.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-179">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="dd3b7-180">`Instructor` Entita má vztah jeden pro žádná nebo jedna s `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-180">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="dd3b7-181">Kód lektorem musí zpracovat:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-181">The instructor code must handle:</span></span>

* <span data-ttu-id="dd3b7-182">Pokud uživatel zruší zaškrtnutí přiřazení office, odstraňte `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-182">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="dd3b7-183">Pokud uživatel zadá přiřazení office a byla prázdná, vytvořte novou `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-183">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="dd3b7-184">Pokud uživatel změní přiřazení office, aktualizovat `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-184">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="dd3b7-185">Aktualizace modelu vyučující upravit stránku s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-185">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

<span data-ttu-id="dd3b7-186">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-186">The preceding code:</span></span>

- <span data-ttu-id="dd3b7-187">Získá aktuální `Instructor` entity z databáze pomocí přes načítání pro `OfficeAssignment` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-187">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="dd3b7-188">Aktualizace načtený `Instructor` entity hodnotami z vazače modelu.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-188">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="dd3b7-189">`TryUpdateModel`zabraňuje [overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="dd3b7-189">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="dd3b7-190">Pokud umístění kanceláře je prázdné, nastaví `Instructor.OfficeAssignment` na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-190">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="dd3b7-191">Když `Instructor.OfficeAssignment` je null, související řádek v `OfficeAssignment` tabulka odstraněna.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-191">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="dd3b7-192">Aktualizovat stránku lektorem úpravy</span><span class="sxs-lookup"><span data-stu-id="dd3b7-192">Update the instructor Edit page</span></span>

<span data-ttu-id="dd3b7-193">Aktualizace *Pages/Instructors/Edit.cshtml* umístěním office:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-193">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="dd3b7-194">Ověřte, že můžete změnit umístění sady vyučující office.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-194">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="dd3b7-195">Přidejte na stránku upravit lektorem kurzu přiřazení</span><span class="sxs-lookup"><span data-stu-id="dd3b7-195">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="dd3b7-196">Vyučující může naučit libovolný počet kurzy.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-196">Instructors may teach any number of courses.</span></span> <span data-ttu-id="dd3b7-197">V této části přidáte možnost změnit přiřazení kurzu.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-197">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="dd3b7-198">Následující obrázek ukazuje aktualizované lektorem upravit stránky:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-198">The following image shows the updated instructor Edit page:</span></span>

![Stránka upravit lektorem s kurzy](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="dd3b7-200">`Course`a `Instructor` je v relaci m: n.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-200">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="dd3b7-201">Pokud chcete přidat a odebrat relace, přidávat a odebírat entity z `CourseAssignments` připojení sady entit.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-201">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="dd3b7-202">Zaškrtávací políčka povolte změny kurzy, který je přiřazen lektorem.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-202">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="dd3b7-203">Zaškrtávací políčko se zobrazí na každé kurz v databázi.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-203">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="dd3b7-204">Kurzy, které je přiřazen lektorem jsou zaškrtnutá políčka.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-204">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="dd3b7-205">Uživatel může vyberte nebo zrušte zaškrtnutí políček, chcete-li změnit přiřazení kurzu.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-205">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="dd3b7-206">Kdyby mnohem větší počet kurzů:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-206">If the number of courses were much greater:</span></span>

* <span data-ttu-id="dd3b7-207">Pravděpodobně použijete jiné uživatelské rozhraní pro zobrazení kurzy.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-207">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="dd3b7-208">Metoda manipulace s spojení entity k vytvoření nebo odstranění relace by nezměnila.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-208">The method of manipulating a join entity to create or delete relationships would not change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="dd3b7-209">Přidání třídy pro podporu vytvářet a upravovat stránky lektorem</span><span class="sxs-lookup"><span data-stu-id="dd3b7-209">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="dd3b7-210">Vytvoření *SchoolViewModels/AssignedCourseData.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-210">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="dd3b7-211">`AssignedCourseData` Třída obsahuje dat a vytvořte zaškrtnutí políček u přiřazené kurzy podle lektorem.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-211">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="dd3b7-212">Vytvořte *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* základní třídy:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-212">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="dd3b7-213">`InstructorCoursesPageModel` Je základní třídou budete používat pro úpravy a vytváření modelů stránky.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-213">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="dd3b7-214">`PopulateAssignedCourseData`načte všechny `Course` entity k naplnění `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-214">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="dd3b7-215">Pro každý kurz nastaví kód `CourseID`, název a zda je lektorem přiřazen ke kurzu.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-215">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="dd3b7-216">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) se používá k vytvoření efektivní vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-216">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="dd3b7-217">Model stránky upravit vyučující</span><span class="sxs-lookup"><span data-stu-id="dd3b7-217">Instructors Edit page model</span></span>

<span data-ttu-id="dd3b7-218">Aktualizace modelu lektorem upravit stránku s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-218">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

<span data-ttu-id="dd3b7-219">Předchozí kód zpracovává změny v přiřazení office.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-219">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="dd3b7-220">Aktualizace lektorem Razor zobrazení:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-220">Update the instructor Razor View:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="dd3b7-221">Když vložíte kód v sadě Visual Studio, zalomení řádků se mění tak, aby dělí kód.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-221">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="dd3b7-222">Stisknutím kombinace kláves Ctrl + Z jednou vrátit zpět, automatického formátování.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-222">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="dd3b7-223">CTRL + Z opravy konce řádků tak, aby zobrazují se jako to, co vidíte zde.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-223">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="dd3b7-224">Odsazení nemusí být úplně bez chyby, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, a `@:</tr>` řádky musí být na jeden řádek jak je vidět.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-224">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="dd3b7-225">Blok vybrané nový kód a stiskněte klávesu Tab, třikrát na řádek kód nového existující kód.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-225">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="dd3b7-226">Hlasovat o nebo zkontrolujte stav této chyby [s tímto odkazem](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="dd3b7-226">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="dd3b7-227">Předchozí kód vytvoří tabulky jazyka HTML, který má tři sloupce.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-227">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="dd3b7-228">Každý sloupec má zaškrtávací políčko a popisek obsahující kurzu číslo a název.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-228">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="dd3b7-229">Všechna zaškrtávací políčka mají stejný název ("selectedCourses").</span><span class="sxs-lookup"><span data-stu-id="dd3b7-229">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="dd3b7-230">Použití stejného názvu informuje vazač modelu považovat za skupinu.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-230">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="dd3b7-231">Atribut hodnota z každé zaškrtávací políčko je nastaven na `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-231">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="dd3b7-232">Když je stránka vrácena, vazač modelu předá pole, které se skládá z `CourseID` hodnoty pro pouze zaškrtávací políčka, které jsou vybrány.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-232">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="dd3b7-233">Když zaškrtnutí políček jsou původně vykresleno, kurzy přiřazené lektorem jste zkontrolovali atributy.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-233">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="dd3b7-234">Spusťte aplikaci a otestovat aktualizované vyučující upravit stránku.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-234">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="dd3b7-235">Změňte přiřazení některých kurzu.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-235">Change some course assignments.</span></span> <span data-ttu-id="dd3b7-236">Změny se projeví na indexovou stránku.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-236">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="dd3b7-237">Poznámka: Přístup zde použitý k jejich úpravě lektorem kurzu funguje dobře, pokud je omezený počet kurzy.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-237">Note: The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="dd3b7-238">Pro kolekce, které jsou mnohem větší jiný uživatelského rozhraní a jinou metodu aktualizace by efektivnější funkční a účinnější.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-238">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="dd3b7-239">Aktualizace stránka pro vytvoření vyučující</span><span class="sxs-lookup"><span data-stu-id="dd3b7-239">Update the instructors Create page</span></span>

<span data-ttu-id="dd3b7-240">Aktualizace modelu lektorem vytvořit stránku s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-240">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="dd3b7-241">Předchozí kód je podobná *Pages/Instructors/Edit.cshtml.cs* kódu.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-241">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="dd3b7-242">Aktualizujte stránku vytvořit Razor lektorem následující kód:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-242">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="dd3b7-243">Testovací stránka pro vytvoření lektorem.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-243">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="dd3b7-244">Odstranit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="dd3b7-244">Update the Delete page</span></span>

<span data-ttu-id="dd3b7-245">Aktualizace modelu odstranění stránek s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-245">Update the Delete page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

<span data-ttu-id="dd3b7-246">Předchozí kód provede tyto změny:</span><span class="sxs-lookup"><span data-stu-id="dd3b7-246">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="dd3b7-247">Používá přes načítání pro `CourseAssignments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-247">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="dd3b7-248">`CourseAssignments`musí být zahrnut nebo nejsou odstraněny při odstranění lektorem.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-248">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="dd3b7-249">Abyste se vyhnuli nutnosti přečtěte si je, nakonfigurujte kaskádové odstranění v databázi.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-249">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="dd3b7-250">Pokud lektorem k odstranění je přiřazen jako správce všech oddělení, odebere přiřazení lektorem z těchto oddělení.</span><span class="sxs-lookup"><span data-stu-id="dd3b7-250">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="dd3b7-251">[Předchozí](xref:data/ef-rp/read-related-data)
[další](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="dd3b7-251">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>