---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Použití rozhraní Entity Framework 4.0 a ovládací prvek ObjectDataSource, 3. část: řazení a filtrování | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů staví na Contoso University webovou aplikaci, která se vytvořila Začínáme s Entity Framework 4.0 série kurzů. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 48d859191877ba951e233f19873d52625fe180ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378907"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="2f77b-104">Použití rozhraní Entity Framework 4.0 a ovládací prvek ObjectDataSource, 3. část: řazení a filtrování</span><span class="sxs-lookup"><span data-stu-id="2f77b-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>
====================
<span data-ttu-id="2f77b-105">podle [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2f77b-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="2f77b-106">V této sérii kurzů staví na Contoso University webovou aplikaci, která se vytvořila [Začínáme s Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série kurzů.</span><span class="sxs-lookup"><span data-stu-id="2f77b-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="2f77b-107">Pokud nebyla dokončena v předchozích kurzech, jako výchozí bod pro účely tohoto kurzu můžete [stáhnout aplikaci](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , kterou by jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2f77b-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="2f77b-108">Můžete také [stáhnout aplikaci](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , který vytvoří kompletní série kurzů.</span><span class="sxs-lookup"><span data-stu-id="2f77b-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="2f77b-109">Pokud máte dotazy týkající se těchto kurzů, můžete je publikovat [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f77b-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="2f77b-110">V předchozím kurzu jste implementovali úložiště vzor n vrstvá webové aplikace, která používá Entity Framework a `ObjectDataSource` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="2f77b-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="2f77b-111">Tento kurz ukazuje, jak provést řazení a filtrování a zpracování scénářů hlavní podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2f77b-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="2f77b-112">Přidejte následující vylepšení *Departments.aspx* stránky:</span><span class="sxs-lookup"><span data-stu-id="2f77b-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="2f77b-113">Textové pole, chcete-li povolit uživatelům výběr oddělení podle názvu.</span><span class="sxs-lookup"><span data-stu-id="2f77b-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="2f77b-114">Seznam kurzů pro každé oddělení, který se zobrazí v mřížce.</span><span class="sxs-lookup"><span data-stu-id="2f77b-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="2f77b-115">Možnost řadit kliknutím na záhlaví sloupců.</span><span class="sxs-lookup"><span data-stu-id="2f77b-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="2f77b-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2f77b-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="2f77b-117">Přidání možnosti řazení sloupce GridView</span><span class="sxs-lookup"><span data-stu-id="2f77b-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="2f77b-118">Otevřít *Departments.aspx* stránky a přidat `SortParameterName="sortExpression"` atribut `ObjectDataSource` ovládací prvek s názvem `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="2f77b-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="2f77b-119">(Později vytvoříte `GetDepartments` metodu, která přebírá parametr s názvem `sortExpression`.) Značky pro počáteční značka ovládacího prvku teď vypadá podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="2f77b-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="2f77b-120">Přidat `AllowSorting="true"` atribut otevírací značce `GridView` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="2f77b-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="2f77b-121">Značky pro počáteční značka ovládacího prvku teď vypadá podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="2f77b-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="2f77b-122">V *Departments.aspx.cs*, nastavit výchozí pořadí řazení voláním `GridView` ovládacího prvku `Sort` metodu z `Page_Load` metody:</span><span class="sxs-lookup"><span data-stu-id="2f77b-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="2f77b-123">Můžete přidat kód, který seřadí nebo filtry ve třídě obchodní logiku nebo třídu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2f77b-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="2f77b-124">Pokud ji uděláte ve třídě obchodní logiky, řazení a filtrování pracovních se provede po načtení dat z databáze, protože třída obchodní logiky pracuje s `IEnumerable` vrácený úložiště.</span><span class="sxs-lookup"><span data-stu-id="2f77b-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="2f77b-125">Pokud přidáte řazení a filtrování kód v třídě úložiště a proveďte před výrazu LINQ nebo dotaz na objekt byl převeden na `IEnumerable` objektu, vaše příkazy se budou předávat na databázi pro zpracování, což je obvykle efektivnější.</span><span class="sxs-lookup"><span data-stu-id="2f77b-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="2f77b-126">V tomto kurzu budete implementovat řazení a filtrování způsobem, který způsobí, že zpracování, aby prováděla databáze – to znamená, že v úložišti.</span><span class="sxs-lookup"><span data-stu-id="2f77b-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="2f77b-127">Přidání možnosti třídění, musíte přidat nové metody pro úložiště rozhraní a třídy úložiště i jako třída obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="2f77b-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="2f77b-128">V *ISchoolRepository.cs* přidejte nový `GetDepartments` metodu, která přebírá `sortExpression` parametr, který se použije k seřazení seznamu oddělení, která je vrácena:</span><span class="sxs-lookup"><span data-stu-id="2f77b-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="2f77b-129">`sortExpression` Parametr bude určovat sloupec řazení a směr řazení.</span><span class="sxs-lookup"><span data-stu-id="2f77b-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="2f77b-130">Přidejte kód pro novou metodu pro *SchoolRepository.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="2f77b-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="2f77b-131">Změnit existující konstruktor bez parametrů `GetDepartments` metoda zavolat novou metodu:</span><span class="sxs-lookup"><span data-stu-id="2f77b-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="2f77b-132">V testovacím projektu přidejte následující novou metodu pro *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="2f77b-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="2f77b-133">Pokud se chystáte vytvořit všechny testy jednotek, které závisí tato metoda vrátí seřazený seznam, je třeba seřadit seznam před jeho vrácením.</span><span class="sxs-lookup"><span data-stu-id="2f77b-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="2f77b-134">Můžete nebude vytvářet testy tímto způsobem. v tomto kurzu, metoda může vrátit pouze netříděný seznam oddělení.</span><span class="sxs-lookup"><span data-stu-id="2f77b-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="2f77b-135">V *SchoolBL.cs* přidejte následující novou metodu do třídy obchodní logiky:</span><span class="sxs-lookup"><span data-stu-id="2f77b-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="2f77b-136">Tento kód předá parametr řazení metodu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2f77b-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="2f77b-137">Spustit *Departments.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="2f77b-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="2f77b-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="2f77b-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="2f77b-139">Nyní můžete kliknout na záhlaví libovolného sloupce pro řazení podle sloupce.</span><span class="sxs-lookup"><span data-stu-id="2f77b-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="2f77b-140">Pokud sloupec je už seřazený, kliknutím na záhlaví obrátí směr řazení.</span><span class="sxs-lookup"><span data-stu-id="2f77b-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="2f77b-141">Přidání vyhledávacího pole</span><span class="sxs-lookup"><span data-stu-id="2f77b-141">Adding a Search Box</span></span>

<span data-ttu-id="2f77b-142">V této části budete přidání textového pole hledání, propojení tak, `ObjectDataSource` řídit pomocí parametru ovládacího prvku a přidejte metodu do třídy obchodní logiky chcete podporovat filtrování.</span><span class="sxs-lookup"><span data-stu-id="2f77b-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="2f77b-143">Otevřít *Departments.aspx* stránku a přidejte následující kód mezi názvem a prvním `ObjectDataSource` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="2f77b-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="2f77b-144">V `ObjectDataSource` ovládací prvek s názvem `DepartmentsObjectDataSource`, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2f77b-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="2f77b-145">Přidat `SelectParameters` – element pro parametr s názvem `nameSearchString` , který získá hodnota zadaná v `SearchTextBox` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="2f77b-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="2f77b-146">Změnit `SelectMethod` atribut hodnotu `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="2f77b-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="2f77b-147">(Vytvoříte tato metoda později.)</span><span class="sxs-lookup"><span data-stu-id="2f77b-147">(You'll create this method later.)</span></span>

<span data-ttu-id="2f77b-148">Zápis `ObjectDataSource` ovládací prvek teď vypadá podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2f77b-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="2f77b-149">V *ISchoolRepository.cs*, přidejte `GetDepartmentsByName` metodu, která přebírá obě `sortExpression` a `nameSearchString` parametry:</span><span class="sxs-lookup"><span data-stu-id="2f77b-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="2f77b-150">V *SchoolRepository.cs*, přidejte následující novou metodu:</span><span class="sxs-lookup"><span data-stu-id="2f77b-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="2f77b-151">Tento kód používá `Where` pro výběr položek, které obsahují hledaný řetězec.</span><span class="sxs-lookup"><span data-stu-id="2f77b-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="2f77b-152">Pokud hledaný řetězec je prázdný, budou vybrány všechny záznamy.</span><span class="sxs-lookup"><span data-stu-id="2f77b-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="2f77b-153">Všimněte si, že při zadání metody volá společně v jednom příkazu takto (`Include`, pak `OrderBy`, pak `Where`), `Where` metoda musí být vždy poslední.</span><span class="sxs-lookup"><span data-stu-id="2f77b-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="2f77b-154">Změnit existující `GetDepartments` metodu, která přebírá `sortExpression` parametr zavolat novou metodu:</span><span class="sxs-lookup"><span data-stu-id="2f77b-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="2f77b-155">V *MockSchoolRepository.cs* v testovacím projektu přidejte následující novou metodu:</span><span class="sxs-lookup"><span data-stu-id="2f77b-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="2f77b-156">V *SchoolBL.cs*, přidejte následující novou metodu:</span><span class="sxs-lookup"><span data-stu-id="2f77b-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="2f77b-157">Spustit *Departments.aspx* stránku a zadejte hledaný řetězec, abyste měli jistotu, že funguje logiku výběru.</span><span class="sxs-lookup"><span data-stu-id="2f77b-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="2f77b-158">Do textového pole ponechte prázdné a opakujte vyhledávání, abyste měli jistotu, že se vrátí všechny záznamy.</span><span class="sxs-lookup"><span data-stu-id="2f77b-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="2f77b-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="2f77b-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="2f77b-160">Přidání sloupce podrobností pro každý řádek mřížky</span><span class="sxs-lookup"><span data-stu-id="2f77b-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="2f77b-161">V dalším kroku budete chtít zobrazit všechny kurzy pro každé oddělení zobrazí v pravém buňky v mřížce.</span><span class="sxs-lookup"><span data-stu-id="2f77b-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="2f77b-162">K tomuto účelu použijete vnořený `GridView` ovládacího prvku a databind tak data z `Courses` vlastnost navigace `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="2f77b-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="2f77b-163">Otevřít *Departments.aspx* a do značky `GridView` řídit, zadejte obslužnou rutinu pro `RowDataBound` událostí.</span><span class="sxs-lookup"><span data-stu-id="2f77b-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="2f77b-164">Značky pro počáteční značka ovládacího prvku teď vypadá podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="2f77b-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="2f77b-165">Přidat nový `TemplateField` elementu po `Administrator` pole šablony:</span><span class="sxs-lookup"><span data-stu-id="2f77b-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="2f77b-166">Tento kód vytvoří vnořený `GridView` ovládací prvek, který zobrazuje číslo kurzu a název seznamu kurzů.</span><span class="sxs-lookup"><span data-stu-id="2f77b-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="2f77b-167">Neurčuje zdroj dat, protože databind, je nutné ho v kódu v `RowDataBound` obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="2f77b-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="2f77b-168">Otevřít *Departments.aspx.cs* a přidejte následující obslužnou rutinu `RowDataBound` události:</span><span class="sxs-lookup"><span data-stu-id="2f77b-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="2f77b-169">Tento kód získá `Department` entity z argumenty události, převede `Courses` navigační vlastnost pro `List` kolekce a databinds vnořeného `GridView` do kolekce.</span><span class="sxs-lookup"><span data-stu-id="2f77b-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="2f77b-170">Otevřít *SchoolRepository.cs* souboru a zadejte předběžné načítání pro `Courses` navigační vlastnost voláním `Include` metoda v objektu dotazu, který vytvoříte v `GetDepartmentsByName` metody.</span><span class="sxs-lookup"><span data-stu-id="2f77b-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="2f77b-171">`return` Výroky `GetDepartmentsByName` metoda teď vypadá podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="2f77b-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="2f77b-172">Spuštění stránky.</span><span class="sxs-lookup"><span data-stu-id="2f77b-172">Run the page.</span></span> <span data-ttu-id="2f77b-173">Kromě řazení a filtrování funkce, která jste přidali dříve ovládací prvek GridView nyní zobrazuje podrobnosti o vnořené kurzu pro každé oddělení.</span><span class="sxs-lookup"><span data-stu-id="2f77b-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="2f77b-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="2f77b-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="2f77b-175">Dokončení tohoto postupu Úvod do scénáře řazení, filtrování a hlavní podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2f77b-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="2f77b-176">V dalším kurzu zobrazí se vám způsob zpracování souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="2f77b-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2f77b-177">[Předchozí](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [další](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="2f77b-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
