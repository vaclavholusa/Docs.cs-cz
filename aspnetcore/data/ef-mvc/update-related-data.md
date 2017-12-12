---
title: "Jádro ASP.NET MVC s EF Core - aktualizace související Data – 10 7"
author: tdykstra
description: "V tomto kurzu aktualizujete souvisejících dat tím, že aktualizuje polí cizího klíče a navigační vlastnosti."
keywords: "Spojí ASP.NET Core Entity Framework Core, související data"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 67bd162b-bfb7-4750-9e7f-705228b5288c
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: b59782bccce00f3940da4ec8bcff768aff8fa4ef
ms.sourcegitcommit: ccf08615ad59bc6f654560de33b93396113a2eb0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a><span data-ttu-id="0a8ce-104">Aktualizace souvisejících dat – základní EF s kurz k ASP.NET MVC jádra (7 10)</span><span class="sxs-lookup"><span data-stu-id="0a8ce-104">Updating related data - EF Core with ASP.NET Core MVC tutorial (7 of 10)</span></span>

<span data-ttu-id="0a8ce-105">Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0a8ce-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0a8ce-106">Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="0a8ce-107">Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).</span><span class="sxs-lookup"><span data-stu-id="0a8ce-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="0a8ce-108">V tomto kurzu předchozí zobrazených souvisejících dat; v tomto kurzu aktualizujete souvisejících dat tím, že aktualizuje polí cizího klíče a navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-108">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="0a8ce-109">Následující ilustrace znázorňuje některé stránek, které budete pracovat.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Stránka upravit kurzu](update-related-data/_static/course-edit.png)

![Stránka upravit lektorem](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="0a8ce-112">Přizpůsobení vytvořit a upravit stránky pro kurzy</span><span class="sxs-lookup"><span data-stu-id="0a8ce-112">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="0a8ce-113">Při vytvoření nové entity kurzu, musí mít relaci s existující oddělení.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-113">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="0a8ce-114">K provedení této automaticky generovaný kód obsahuje metody kontroleru a vytvořit a upravit zobrazení, které zahrnují rozevíracího seznamu pro výběr z oddělení.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-114">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="0a8ce-115">Nastaví rozevíracího seznamu `Course.DepartmentID` vlastností cizího klíče, a to je všechno rozhraní Entity Framework potřebuje, aby zatížení `Department` navigační vlastnost s odpovídající entity oddělení.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-115">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="0a8ce-116">Budete používat automaticky generovaný kód, ale to změnit mírně na přidání zpracování chyb a řazení rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-116">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="0a8ce-117">V *CoursesController.cs*, odstraňte čtyři metody vytvoření a úpravy a nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-117">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="0a8ce-118">Po `Edit` metoda HttpPost vytvoření nové metody, která načte informace o oddělení pro rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-118">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="0a8ce-119">`PopulateDepartmentsDropDownList` Metoda získá seznam všech oddělení seřazené podle názvu, vytvoří `SelectList` kolekci rozevíracího seznamu a předává kolekce pro zobrazení v `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-119">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="0a8ce-120">Metodu je možné zadat nepovinný `selectedDepartment` parametr, který umožňuje volací kód, který zadejte položku, která bude vybrána při vykreslení rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-120">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="0a8ce-121">Zobrazení předá název "DepartmentID" `<select>` značky pomocné rutiny a pomocné rutiny pak zná k prohledání `ViewBag` objekt pro `SelectList` s názvem "DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="0a8ce-121">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="0a8ce-122">Třídy MetadataExchangeClientMode `Create` volání metod `PopulateDepartmentsDropDownList` metoda bez nastavení vybrané položky, protože na nový kurz není oddělení ještě vytvořit:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-122">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="0a8ce-123">Třídy MetadataExchangeClientMode `Edit` metoda nastaví vybrané položky podle ID oddělení, které je již přiřazen ke kurzu upravovaný:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-123">The HttpGet `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="0a8ce-124">Metody HttpPost pro obě `Create` a `Edit` také obsahovat kód, který nastaví vybrané položky, když se znovu zobrazit stránku po chybě.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-124">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="0a8ce-125">Tím se zajistí, že když stránky se zobrazí znovu, chcete-li zobrazit chybová zpráva, ať oddělení nebyla vybrána zůstává vybrané.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-125">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="0a8ce-126">Přidáte. Podrobnosti o AsNoTracking a odstraňte metody</span><span class="sxs-lookup"><span data-stu-id="0a8ce-126">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="0a8ce-127">Za účelem optimalizace výkonu podrobnosti o kurzu a odstranění stránky, přidejte `AsNoTracking` zavolá `Details` a třídy MetadataExchangeClientMode `Delete` metody.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-127">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="0a8ce-128">Upravit zobrazení průběhu</span><span class="sxs-lookup"><span data-stu-id="0a8ce-128">Modify the Course views</span></span>

<span data-ttu-id="0a8ce-129">V *Views/Courses/Create.cshtml*, přidejte možnost "Vyberte oddělení" **oddělení** rozevíracího seznamu změňte popisek z **DepartmentID** k  **Oddělení**a přidejte ověřovací zprávu.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-129">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="0a8ce-130">V *Views/Courses/Edit.cshtml*, proveďte požadovanou změnu stejné pro pole oddělení, stejně jako ve *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-130">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="0a8ce-131">Také v *Views/Courses/Edit.cshtml*, přidání pole číslo kurzu před **název** pole.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-131">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="0a8ce-132">Protože číslo kurzu je primární klíč, se zobrazí, ale nelze ho změnit.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-132">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="0a8ce-133">Již existuje skrytá pole (`<input type="hidden">`) pro číslo kurzu v okně Upravit.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-133">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="0a8ce-134">Přidání `<label>` pomocná značka nemá eliminují nutnost použití skryté pole, protože nezpůsobuje kurzu číslo, které má být součástí odeslaných dat, když uživatel klikne **Uložit** na **upravit** stránky.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-134">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="0a8ce-135">V *Views/Courses/Delete.cshtml*, přidání kurzu číslo pole v horní části a změňte ID oddělení název oddělení.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-135">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="0a8ce-136">V *Views/Courses/Details.cshtml*, proveďte požadovanou změnu stejné, který jste právě použili pro *Delete.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-136">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="0a8ce-137">Testování postupu stránek</span><span class="sxs-lookup"><span data-stu-id="0a8ce-137">Test the Course pages</span></span>

<span data-ttu-id="0a8ce-138">Spuštění aplikace, vyberte **kurzy** , klikněte na **vytvořit nový**a zadejte data pro nový kurz:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-138">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Stránka pro vytvoření kurzu](update-related-data/_static/course-create.png)

<span data-ttu-id="0a8ce-140">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-140">Click **Create**.</span></span> <span data-ttu-id="0a8ce-141">S novou během přidán do seznamu se zobrazí stránka kurzy Index.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-141">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="0a8ce-142">Název oddělení v seznamu Index stránky pochází z navigační vlastnost, zobrazující, že byla správně navázat relaci.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-142">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="0a8ce-143">Klikněte na tlačítko **upravit** v postupu v kurzech indexovou stránku.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-143">Click **Edit** on a course in the Courses Index page.</span></span>

![Stránka upravit kurzu](update-related-data/_static/course-edit.png)

<span data-ttu-id="0a8ce-145">Data na stránce změny a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-145">Change data on the page and click **Save**.</span></span> <span data-ttu-id="0a8ce-146">Kurzy indexovou stránku se zobrazí s daty aktualizované kurzu.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-146">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-an-edit-page-for-instructors"></a><span data-ttu-id="0a8ce-147">Přidat stránku upravit pro vyučující</span><span class="sxs-lookup"><span data-stu-id="0a8ce-147">Add an Edit Page for Instructors</span></span>

<span data-ttu-id="0a8ce-148">Při úpravách záznamu lektorem chcete mít možnost aktualizovat lektorem office přiřazení.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-148">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="0a8ce-149">Entitu lektorem má vztah-nula nebo 1 s OfficeAssignment entity, což znamená, že má váš kód pro zpracování těchto situacích:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-149">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="0a8ce-150">Pokud uživatel zruší zaškrtnutí přiřazení office a původně hodnotu, odstraňte OfficeAssignment entity.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-150">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="0a8ce-151">Pokud uživatel zadá hodnotu přiřazení office a původně byla prázdná, vytvořte nové entity OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-151">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="0a8ce-152">Pokud uživatel změní hodnotu přiřazení office, změňte hodnotu ve stávající entitu OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-152">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="0a8ce-153">Aktualizaci řadiče vyučující</span><span class="sxs-lookup"><span data-stu-id="0a8ce-153">Update the Instructors controller</span></span>

<span data-ttu-id="0a8ce-154">V *InstructorsController.cs*, změňte kód v třídy MetadataExchangeClientMode `Edit` metody, které se načte entitu lektorem `OfficeAssignment` navigační vlastnost a volání `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-154">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

<span data-ttu-id="0a8ce-155">Nahraďte HttpPost `Edit` metoda následující kód pro zpracování přiřazení aktualizací office:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-155">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="0a8ce-156">Kód provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-156">The code does the following:</span></span>

-  <span data-ttu-id="0a8ce-157">Změní název metody k `EditPost` protože podpis je nyní stejné jako třídy MetadataExchangeClientMode `Edit` – metoda ( `ActionName` atribut určuje, že `/Edit/` adresa URL je stále používán).</span><span class="sxs-lookup"><span data-stu-id="0a8ce-157">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

-  <span data-ttu-id="0a8ce-158">Získá aktuální entitu lektorem z databáze pomocí přes načítání `OfficeAssignment` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-158">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="0a8ce-159">Je to stejné jako v třídy MetadataExchangeClientMode `Edit` metoda.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-159">This is the same as what you did in the HttpGet `Edit` method.</span></span>

-  <span data-ttu-id="0a8ce-160">Aktualizuje načtenou entitu lektorem hodnotami z vazače modelu.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-160">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="0a8ce-161">`TryUpdateModel` Přetížení umožňuje povolených vlastnosti, které chcete zahrnout.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-161">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="0a8ce-162">To brání přečerpání účtování, jak je popsáno v [druhý kurzu](crud.md).</span><span class="sxs-lookup"><span data-stu-id="0a8ce-162">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets do not play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   <span data-ttu-id="0a8ce-163">Pokud umístění kanceláře je prázdné, nastaví vlastnost Instructor.OfficeAssignment na hodnotu null, takže související řádek v tabulce OfficeAssignment se odstraní.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-163">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets do not play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- <span data-ttu-id="0a8ce-164">Uloží změny do databáze.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-164">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="0a8ce-165">Aktualizace lektorem upravit zobrazení</span><span class="sxs-lookup"><span data-stu-id="0a8ce-165">Update the Instructor Edit view</span></span>

<span data-ttu-id="0a8ce-166">V *Views/Instructors/Edit.cshtml*, přidat nové pole pro úpravy pobočce, na konci před **Uložit** tlačítko:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-166">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="0a8ce-167">Spuštění aplikace, vyberte **vyučující** a pak klikněte **upravit** na lektorem.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-167">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="0a8ce-168">Změna **umístění kanceláře** a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-168">Change the **Office Location** and click **Save**.</span></span>

![Stránka upravit lektorem](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="0a8ce-170">Přidejte na stránku lektorem upravit přiřazení kurzu</span><span class="sxs-lookup"><span data-stu-id="0a8ce-170">Add Course assignments to the Instructor Edit page</span></span>

<span data-ttu-id="0a8ce-171">Vyučující může naučit libovolný počet kurzy.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-171">Instructors may teach any number of courses.</span></span> <span data-ttu-id="0a8ce-172">Nyní budete vylepšit stránce lektorem upravit přidáním možnost změnit přiřazení kurzu pomocí skupiny zaškrtávacích políček, jak je znázorněno na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-172">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Stránka upravit lektorem s kurzy](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="0a8ce-174">Vztah mezi kurzu a lektorem entity je m: n.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-174">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="0a8ce-175">Pokud chcete přidat a odebrat relace, abyste přidávat a odebírat entit do a ze sady entit CourseAssignments spojení.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-175">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="0a8ce-176">Uživatelské rozhraní, které umožňuje změnit které kurzy lektorem je přiřazen je skupina zaškrtávacích políček.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-176">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="0a8ce-177">Zaškrtávací políčko pro každý kurz v databázi se zobrazí, a jsou ty, které lektorem je aktuálně přiřazen k vybrané.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-177">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="0a8ce-178">Uživatel může vyberte nebo zrušte zaškrtnutí políček, chcete-li změnit přiřazení kurzu.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-178">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="0a8ce-179">Pokud byly mnohem větší počet kurzy, by pravděpodobně chtít použít jinou metodu prezentace dat v zobrazení, ale stejnou metodu manipulace s spojení subjekt byste použili k vytvoření nebo odstranění relace.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-179">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="0a8ce-180">Aktualizaci řadiče vyučující</span><span class="sxs-lookup"><span data-stu-id="0a8ce-180">Update the Instructors controller</span></span>

<span data-ttu-id="0a8ce-181">Pokud chcete data k zobrazení seznamu zaškrtávacích políček, použijete třídu modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-181">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="0a8ce-182">Vytvoření *AssignedCourseData.cs* v *SchoolViewModels* složky a nahraďte existující kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-182">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="0a8ce-183">V *InstructorsController.cs*, nahraďte třídy MetadataExchangeClientMode `Edit` metoda následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-183">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="0a8ce-184">Změny se zvýrazněnou.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-184">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="0a8ce-185">Kód přidá přes načítání pro `Courses` navigační vlastnost a volá novou `PopulateAssignedCourseData` metoda podat informace o použití pole zaškrtávací políčko `AssignedCourseData` zobrazit třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-185">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="0a8ce-186">Kód `PopulateAssignedCourseData` metoda čte prostřednictvím všechny entity kurzu Chcete-li načíst seznam kurzů pomocí třídy modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-186">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="0a8ce-187">Pro každý kurz kód ověří, zda během existuje v lektorem `Courses` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-187">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="0a8ce-188">Chcete-li vytvořit efektivní vyhledávání při kontrole, zda je přiřazen kurzu lektorem, jsou vložena kurzy přiřazené lektorem `HashSet` kolekce.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-188">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="0a8ce-189">`Assigned` Je nastavena na hodnotu true pro kurzy lektorem je přiřazena k.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-189">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="0a8ce-190">Zobrazení bude pomocí této vlastnosti k určení, které Kontrola polí musí být zobrazí jako vybraný.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-190">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="0a8ce-191">Nakonec je předán zobrazení v seznamu `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-191">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="0a8ce-192">Dál přidejte kód, který je spuštěn, když uživatel klikne na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-192">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="0a8ce-193">Nahraďte `EditPost` metoda s následující kód a přidat nové metody, která aktualizuje `Courses` navigační vlastnost lektorem entity.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-193">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="0a8ce-194">Podpis metody se liší od třídy MetadataExchangeClientMode teď `Edit` proto název metody změní z `EditPost` zpět na `Edit`.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-194">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="0a8ce-195">Vzhledem k tomu, že zobrazení nemá kolekci entit kurzu, automaticky se nedá aktualizovat vazač modelu `CourseAssignments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-195">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="0a8ce-196">Místo použití vazače modelu k aktualizaci `CourseAssignments` navigační vlastnost, můžete udělat v novém `UpdateInstructorCourses` metoda.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-196">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="0a8ce-197">Proto musíte vyloučit `CourseAssignments` vlastnost z vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-197">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="0a8ce-198">To nevyžaduje všechny změny kód, který volá `TryUpdateModel` vzhledem k tomu, že používáte přetížení povolených a `CourseAssignments` není v seznamu zahrnout.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-198">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="0a8ce-199">Pokud žádná kontrola byly zaškrtnutá políčka, kód v `UpdateInstructorCourses` inicializuje `CourseAssignments` navigační vlastnost s prázdnou kolekci a vrátí:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-199">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="0a8ce-200">Kód pak projde všechny kurzy v databázi a zkontroluje každý kurzu proti těm, které jsou přiřazeny k lektorem a ty, které byly vybrány v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-200">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="0a8ce-201">Pro usnadnění efektivní hledání, jsou uloženy pozdější dvě kolekce v `HashSet` objekty.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-201">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="0a8ce-202">Pokud je zaškrtnuté políčko kurzu ale během není v `Instructor.CourseAssignments` navigační vlastnost během je přidat do kolekce ve vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-202">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="0a8ce-203">Pokud není zaškrtnuté políčko kurzu, ale probíhá během `Instructor.CourseAssignments` navigační vlastnost, během je odebrána z navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-203">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="0a8ce-204">Aktualizujte zobrazení lektorem</span><span class="sxs-lookup"><span data-stu-id="0a8ce-204">Update the Instructor views</span></span>

<span data-ttu-id="0a8ce-205">V *Views/Instructors/Edit.cshtml*, přidat **kurzy** pole s pole zaškrtávacích políček přidáním následující kód bezprostředně po `div` prvky pro **Office**  pole a před `div` element pro **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-205">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE] 
> <span data-ttu-id="0a8ce-206">Když vložíte kód v sadě Visual Studio, změní se tak, aby dělí kód konce řádků.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-206">When you paste the code in Visual Studio, line breaks will be changed in a way that breaks the code.</span></span>  <span data-ttu-id="0a8ce-207">Stisknutím kombinace kláves Ctrl + Z jednou vrátit zpět, automatického formátování.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-207">Press Ctrl+Z one time to undo the automatic formatting.</span></span>  <span data-ttu-id="0a8ce-208">To tak, aby zobrazují se jako to, co vidíte zde opraví konce řádků.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-208">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="0a8ce-209">Odsazení nemusí být úplně bez chyby, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, a `@:</tr>` řádky musí být na jeden řádek znázorněné nebo získáte Chyba za běhu.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-209">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="0a8ce-210">Blok vybrané nový kód a stiskněte klávesu Tab, třikrát na řádek kód nového existující kód.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-210">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="0a8ce-211">Stav tohoto problému můžete zkontrolovat [zde](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="0a8ce-211">You can check the status of this problem [here](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="0a8ce-212">Tento kód vytvoří tabulky jazyka HTML, který má tři sloupce.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-212">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="0a8ce-213">V každém sloupci je zaškrtávací políčko, za nímž následuje popisek, který se skládá z kurzu číslo a název.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-213">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="0a8ce-214">Všechna zaškrtávací políčka mají stejný název ("selectedCourses"), která informuje o vazač modelu, které jsou považovány za skupinu.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-214">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="0a8ce-215">Hodnota atributu každé zaškrtávací políčko je nastavena na hodnotu `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-215">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="0a8ce-216">Když je stránka vrácena, vazač modelu předá pole na řadič, který se skládá z `CourseID` hodnoty pro pouze zaškrtávací políčka, které jsou vybrány.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-216">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="0a8ce-217">Když zaškrtnutí políček jsou původně vykresleno, ty, které jsou pro kurzy přiřazené lektorem jste zkontrolovali atributy, které vybere je (zobrazí se, které je zaškrtnutí).</span><span class="sxs-lookup"><span data-stu-id="0a8ce-217">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="0a8ce-218">Spuštění aplikace, vyberte **vyučující** a klikněte na **upravit** na lektorem zobrazíte **upravit** stránky.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-218">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Stránka upravit lektorem s kurzy](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="0a8ce-220">Změna některých během přiřazení a klikněte na Uložit.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-220">Change some course assignments and click Save.</span></span> <span data-ttu-id="0a8ce-221">Provedené změny se projeví na indexovou stránku.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-221">The changes you make are reflected on the Index page.</span></span>

> [!NOTE] 
> <span data-ttu-id="0a8ce-222">Přístup zde použitý k jejich úpravě lektorem kurzu funguje dobře, pokud je omezený počet kurzy.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-222">The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="0a8ce-223">Pro kolekce, které jsou mnohem větší by bylo zapotřebí různých uživatelského rozhraní a jinou metodu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-223">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="0a8ce-224">Odstranit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="0a8ce-224">Update the Delete page</span></span>

<span data-ttu-id="0a8ce-225">V *InstructorsController.cs*, odstraňte `DeleteConfirmed` metoda a vložte následující kód na příslušné místo.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-225">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="0a8ce-226">Tento kód provede tyto změny:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-226">This code makes the following changes:</span></span>

* <span data-ttu-id="0a8ce-227">Načítání eager nemá `CourseAssignments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-227">Does eager loading for the `CourseAssignments` navigation property.</span></span>  <span data-ttu-id="0a8ce-228">Je nutné zahrnout to nebo EF nebude vědět o souvisejících `CourseAssignment` entity a nedojde k jejich odstranění.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-228">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span>  <span data-ttu-id="0a8ce-229">Aby se zabránilo museli přečtěte si je zde lze nakonfigurovat kaskádové odstranění v databázi.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-229">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="0a8ce-230">Pokud lektorem k odstranění je přiřazen jako správce všech oddělení, odebere přiřazení lektorem z těchto oddělení.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-230">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="0a8ce-231">Přidejte na stránku vytvořit umístění kanceláře a kurzy</span><span class="sxs-lookup"><span data-stu-id="0a8ce-231">Add office location and courses to the Create page</span></span>

<span data-ttu-id="0a8ce-232">V *InstructorsController.cs*, odstraňování třídy MetadataExchangeClientMode a HttpPost `Create` metody a poté přidejte následující kód v místě jejich:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-232">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="0a8ce-233">Tento kód je podobný postupu pro `Edit` metody s výjimkou které žádné kurzy jsou vybrány.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-233">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="0a8ce-234">Třídy MetadataExchangeClientMode `Create` volání metod `PopulateAssignedCourseData` metoda není, protože může být kurzy, ale ve vybrané pořadí zajistit pro prázdnou kolekci `foreach` smyčky v zobrazení (jinak kód zobrazení by throw výjimka odkazu s hodnotou null).</span><span class="sxs-lookup"><span data-stu-id="0a8ce-234">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="0a8ce-235">HttpPost `Create` metoda přidá každý vybraný kurz k `CourseAssignments` navigační vlastnost před zkontroluje chyby ověření a přidá nové lektorem do databáze.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-235">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="0a8ce-236">Kurzy jsou přidány, i když nejsou chyby modelu, takže pokud nejsou chyby modelu (například uživatele s klíči na neplatné datum) a stránce se zobrazí znovu, zobrazí se chybová zpráva, jsou automaticky obnovit kterýkoli výběr kurzu, které byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-236">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="0a8ce-237">Všimněte si, že aby bylo možné přidat kurzy, které `CourseAssignments` navigační vlastnost, je nutné inicializovat vlastnost jako prázdnou kolekci:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-237">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="0a8ce-238">Jako alternativu k to v kódu kontroleru lze nastavit v modelu lektorem změnou metoda getter vlastnosti pro automatické vytvoření kolekce Pokud neexistuje, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="0a8ce-238">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

<span data-ttu-id="0a8ce-239">Pokud změníte `CourseAssignments` vlastnost tímto způsobem můžete odstranit kód inicializace explicitní vlastnost v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-239">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="0a8ce-240">V *Views/Instructor/Create.cshtml*, přidejte office umístění textového pole a zaškrtněte políčka pro kurzy před tlačítko pro odeslání.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-240">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="0a8ce-241">Jako v případě stránce Upravit [opravte formátování Pokud Visual Studio přeformátuje kód při vkládání](#notepad).</span><span class="sxs-lookup"><span data-stu-id="0a8ce-241">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="0a8ce-242">Test spuštění aplikace a vytvoření lektorem.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-242">Test by running the app and creating an instructor.</span></span> 

## <a name="handling-transactions"></a><span data-ttu-id="0a8ce-243">Zpracování transakcí</span><span class="sxs-lookup"><span data-stu-id="0a8ce-243">Handling Transactions</span></span>

<span data-ttu-id="0a8ce-244">Jak je popsáno v [CRUD kurzu](crud.md), rozhraní Entity Framework implicitně implementuje transakce.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-244">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="0a8ce-245">Scénáře, kde můžete potřebovat více řídit – například pokud chcete takové operace, provést v transakci – mimo Entity Framework najdete v tématu [transakce](https://docs.microsoft.com/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="0a8ce-245">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="summary"></a><span data-ttu-id="0a8ce-246">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0a8ce-246">Summary</span></span>

<span data-ttu-id="0a8ce-247">Teď jste dokončili úvod k práci s související data.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-247">You have now completed the introduction to working with related data.</span></span> <span data-ttu-id="0a8ce-248">V dalším kurzu se zobrazí způsobu řešení konfliktů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="0a8ce-248">In the next tutorial you'll see how to handle concurrency conflicts.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0a8ce-249">[Předchozí](read-related-data.md)
[další](concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="0a8ce-249">[Previous](read-related-data.md)
[Next](concurrency.md)</span></span>  
