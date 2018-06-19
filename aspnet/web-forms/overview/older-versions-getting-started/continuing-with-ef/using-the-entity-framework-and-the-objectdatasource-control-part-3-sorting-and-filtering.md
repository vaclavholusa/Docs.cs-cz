---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Pomocí rozhraní Entity Framework 4.0 a ovládacího prvku ObjectDataSource, část 3: řazení a filtrování | Microsoft Docs'
author: tdykstra
description: Tento kurz řady staví na webové aplikace Contoso univerzity, který byl vytvořený Začínáme s řadou kurz Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: e412d3ad98a37931e7190a4909cb09fa2abfb3d0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887652"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="f09ac-104">Pomocí rozhraní Entity Framework 4.0 a ovládacího prvku ObjectDataSource, část 3: řazení a filtrování</span><span class="sxs-lookup"><span data-stu-id="f09ac-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>
====================
<span data-ttu-id="f09ac-105">Podle [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f09ac-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="f09ac-106">Tento kurz řady staví na webové aplikace Contoso univerzity, který byl vytvořený [Začínáme s Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) kurz řady.</span><span class="sxs-lookup"><span data-stu-id="f09ac-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="f09ac-107">Pokud nebyla dokončena starší kurzy, jako výchozí bod pro účely tohoto kurzu můžete [stažení aplikace](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , kterou by jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f09ac-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="f09ac-108">Můžete také [stažení aplikace](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) vytvořené dokončení kurzu řady.</span><span class="sxs-lookup"><span data-stu-id="f09ac-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="f09ac-109">Pokud máte dotazy týkající se kurzy, můžete je do příspěvku [ASP.NET Entity Framework fórum](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="f09ac-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="f09ac-110">V tomto kurzu předchozí implementována použitému vzoru vícevrstvé webové aplikace, který používá rozhraní Entity Framework a `ObjectDataSource` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="f09ac-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="f09ac-111">Tento kurz ukazuje, jak provést řazení a filtrování a zpracování stránek podrobností.</span><span class="sxs-lookup"><span data-stu-id="f09ac-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="f09ac-112">Bude potřeba přidat následující vylepšení *Departments.aspx* stránky:</span><span class="sxs-lookup"><span data-stu-id="f09ac-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="f09ac-113">Textové pole, aby uživatelé mohli vybrat oddělení podle názvu.</span><span class="sxs-lookup"><span data-stu-id="f09ac-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="f09ac-114">Seznam kurzů pro každé oddělení, které se zobrazí v mřížce.</span><span class="sxs-lookup"><span data-stu-id="f09ac-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="f09ac-115">Možnost můžete seřadit kliknutím na záhlaví sloupce.</span><span class="sxs-lookup"><span data-stu-id="f09ac-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="f09ac-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f09ac-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="f09ac-117">Přidání možnost řazení GridView sloupce</span><span class="sxs-lookup"><span data-stu-id="f09ac-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="f09ac-118">Otevřete *Departments.aspx* stránky a přidejte `SortParameterName="sortExpression"` atribut `ObjectDataSource` ovládací prvek s názvem `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="f09ac-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="f09ac-119">(Později vytvoříte `GetDepartments` metoda, která přebírá parametr s názvem `sortExpression`.) Značky pro počáteční značka ovládacího prvku nyní podobá následujícímu příkladu.</span><span class="sxs-lookup"><span data-stu-id="f09ac-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="f09ac-120">Přidat `AllowSorting="true"` atribut otevírací značce `GridView` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="f09ac-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="f09ac-121">Značky pro počáteční značka ovládacího prvku nyní podobá následujícímu příkladu.</span><span class="sxs-lookup"><span data-stu-id="f09ac-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="f09ac-122">V *Departments.aspx.cs*, nastavit výchozí pořadí řazení, že zavoláte `GridView` ovládacího prvku `Sort` metoda z `Page_Load` metoda:</span><span class="sxs-lookup"><span data-stu-id="f09ac-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="f09ac-123">Ve třídě obchodní logiky nebo třídu úložiště můžete přidat kód, který seřadí nebo filtry.</span><span class="sxs-lookup"><span data-stu-id="f09ac-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="f09ac-124">Pokud se ve třídě obchodní logiky, řazení a filtrování pracovních bude provedeno po načtení dat z databáze, protože třída obchodní logiky ve spolupráci s `IEnumerable` objekt vrácený úložiště.</span><span class="sxs-lookup"><span data-stu-id="f09ac-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="f09ac-125">Pokud přidáte řazení a filtrování kód ve třídě, úložiště a provést před výrazu LINQ nebo dotaz na objekt byl převeden na `IEnumerable` objektu příkazech bude předána do databáze pro zpracování, které obvykle je efektivnější.</span><span class="sxs-lookup"><span data-stu-id="f09ac-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="f09ac-126">V tomto kurzu budete implementovat řazení a filtrování způsobem, který způsobí, že zpracování musí udělat databáze – to znamená, že v úložišti.</span><span class="sxs-lookup"><span data-stu-id="f09ac-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="f09ac-127">Přidání možnosti třídění, musíte přidat nové metody pro úložiště rozhraní a třídy úložiště i o třídě obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="f09ac-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="f09ac-128">V *ISchoolRepository.cs* soubor, přidejte nový `GetDepartments` metody, která přijímá `sortExpression` parametr, který se použije k řazení seznamu oddělení, která je vrácena:</span><span class="sxs-lookup"><span data-stu-id="f09ac-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="f09ac-129">`sortExpression` Parametr bude určovat tento sloupec seřadit na a směr řazení.</span><span class="sxs-lookup"><span data-stu-id="f09ac-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="f09ac-130">Přidejte kód nové metodu *SchoolRepository.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="f09ac-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="f09ac-131">Změnit existující bez parametrů `GetDepartments` metoda k volání nové metody:</span><span class="sxs-lookup"><span data-stu-id="f09ac-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="f09ac-132">V k testovacímu projektu, přidejte následující metodu do nové *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="f09ac-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="f09ac-133">Pokud se chystáte vytvořit všechny testy jednotek, které závisí na tato metoda vrátí seřazený seznam, musíte před vrácením seřaďte seznam.</span><span class="sxs-lookup"><span data-stu-id="f09ac-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="f09ac-134">Můžete nebude vytvářet testy jako je například v tomto kurzu, metoda může vrátit pouze neseřazené seznam oddělení.</span><span class="sxs-lookup"><span data-stu-id="f09ac-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="f09ac-135">V *SchoolBL.cs* soubor, přidejte následující metodu nové na obchodní logiku třídu:</span><span class="sxs-lookup"><span data-stu-id="f09ac-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="f09ac-136">Tento kód předá parametr řazení metodu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f09ac-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="f09ac-137">Spustit *Departments.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="f09ac-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="f09ac-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f09ac-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="f09ac-139">Nyní můžete kliknout na záhlaví libovolného sloupce řadit podle sloupce.</span><span class="sxs-lookup"><span data-stu-id="f09ac-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="f09ac-140">Pokud je už seřazený sloupec, kliknutím na záhlaví obrátí směr řazení.</span><span class="sxs-lookup"><span data-stu-id="f09ac-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="f09ac-141">Přidání vyhledávací pole</span><span class="sxs-lookup"><span data-stu-id="f09ac-141">Adding a Search Box</span></span>

<span data-ttu-id="f09ac-142">V této části budete přidat textového pole pro vyhledávání, propojovat je se `ObjectDataSource` řízení pomocí parametru řízení a přidání metody do třídy obchodní logiku pro podporu filtrování.</span><span class="sxs-lookup"><span data-stu-id="f09ac-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="f09ac-143">Otevřete *Departments.aspx* stránky a přidejte následující kód mezi záhlaví a první `ObjectDataSource` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="f09ac-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="f09ac-144">V `ObjectDataSource` ovládací prvek s názvem `DepartmentsObjectDataSource`, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="f09ac-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="f09ac-145">Přidat `SelectParameters` element pro parametr s názvem `nameSearchString` hodnota zadaná v, který získá `SearchTextBox` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="f09ac-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="f09ac-146">Změna `SelectMethod` atribut hodnotu `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="f09ac-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="f09ac-147">(Vytvoříte tato metoda později.)</span><span class="sxs-lookup"><span data-stu-id="f09ac-147">(You'll create this method later.)</span></span>

<span data-ttu-id="f09ac-148">Značku `ObjectDataSource` řízení nyní podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="f09ac-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="f09ac-149">V *ISchoolRepository.cs*, přidejte `GetDepartmentsByName` metody, která přijímá obě `sortExpression` a `nameSearchString` parametry:</span><span class="sxs-lookup"><span data-stu-id="f09ac-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="f09ac-150">V *SchoolRepository.cs*, přidejte následující metodu nové:</span><span class="sxs-lookup"><span data-stu-id="f09ac-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="f09ac-151">Tento kód používá `Where` metoda vyberte položky, které obsahují řetězec pro hledání.</span><span class="sxs-lookup"><span data-stu-id="f09ac-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="f09ac-152">Pokud je řetězec prázdný, budou vybrány všechny záznamy.</span><span class="sxs-lookup"><span data-stu-id="f09ac-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="f09ac-153">Všimněte si, že když zadáte metoda volá společně v jednom příkazu takto (`Include`, pak `OrderBy`, pak `Where`), `Where` metoda musí být vždy poslední.</span><span class="sxs-lookup"><span data-stu-id="f09ac-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="f09ac-154">Změnit existující `GetDepartments` metody, která přijímá `sortExpression` parametr volat metodu nové:</span><span class="sxs-lookup"><span data-stu-id="f09ac-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="f09ac-155">V *MockSchoolRepository.cs* v k testovacímu projektu, přidejte následující metodu nové:</span><span class="sxs-lookup"><span data-stu-id="f09ac-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="f09ac-156">V *SchoolBL.cs*, přidejte následující metodu nové:</span><span class="sxs-lookup"><span data-stu-id="f09ac-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="f09ac-157">Spustit *Departments.aspx* stránky a zadejte hledaný řetězec pro Ujistěte se, že funguje logiku pro výběr.</span><span class="sxs-lookup"><span data-stu-id="f09ac-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="f09ac-158">Ponechte textové pole prázdné a zkuste hledání, ujistěte se, že jsou vráceny všechny záznamy.</span><span class="sxs-lookup"><span data-stu-id="f09ac-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="f09ac-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f09ac-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="f09ac-160">Přidat sloupec podrobnosti pro každý řádek tabulky</span><span class="sxs-lookup"><span data-stu-id="f09ac-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="f09ac-161">Dále budete chtít zobrazit všechny kurzy pro každé oddělení zobrazí v pravém buňky v mřížce.</span><span class="sxs-lookup"><span data-stu-id="f09ac-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="f09ac-162">K tomuto účelu použijete vnořený `GridView` řízení a databind jeho data z `Courses` vlastnost navigace `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="f09ac-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="f09ac-163">Otevřete *Departments.aspx* a do značky `GridView` řídit, zadejte obslužnou rutinu pro `RowDataBound` událostí.</span><span class="sxs-lookup"><span data-stu-id="f09ac-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="f09ac-164">Značky pro počáteční značka ovládacího prvku nyní podobá následujícímu příkladu.</span><span class="sxs-lookup"><span data-stu-id="f09ac-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="f09ac-165">Přidejte nový `TemplateField` element po `Administrator` pole šablony:</span><span class="sxs-lookup"><span data-stu-id="f09ac-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="f09ac-166">Tento kód vytvoří vnořený `GridView` ovládací prvek, který se zobrazuje číslo kurzu a název seznam kurzů.</span><span class="sxs-lookup"><span data-stu-id="f09ac-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="f09ac-167">Neurčuje zdroje dat, protože budete databind ho v kódu v `RowDataBound` obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="f09ac-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="f09ac-168">Otevřete *Departments.aspx.cs* a přidejte následující obslužnou rutinu pro `RowDataBound` událostí:</span><span class="sxs-lookup"><span data-stu-id="f09ac-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="f09ac-169">Tento kód získá `Department` entita z argumenty událostí převede `Courses` navigační vlastnost pro `List` kolekce a databinds vnořeného `GridView` do kolekce.</span><span class="sxs-lookup"><span data-stu-id="f09ac-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="f09ac-170">Otevřete *SchoolRepository.cs* souboru a zadejte přes načítání pro `Courses` navigační vlastnost voláním `Include` metoda v objektu dotazu, který vytvoříte v `GetDepartmentsByName` metoda.</span><span class="sxs-lookup"><span data-stu-id="f09ac-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="f09ac-171">`return` Příkaz v `GetDepartmentsByName` metoda nyní podobá následujícímu příkladu.</span><span class="sxs-lookup"><span data-stu-id="f09ac-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="f09ac-172">Spuštění stránky.</span><span class="sxs-lookup"><span data-stu-id="f09ac-172">Run the page.</span></span> <span data-ttu-id="f09ac-173">Kromě řazení a filtrování funkci, která jste přidali dříve prvek GridView nyní zobrazuje podrobnosti o vnořené kurzu pro každé oddělení.</span><span class="sxs-lookup"><span data-stu-id="f09ac-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="f09ac-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f09ac-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="f09ac-175">Tím dokončíte Úvod do scénáře řazení, filtrování a podrobností.</span><span class="sxs-lookup"><span data-stu-id="f09ac-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="f09ac-176">V dalším kurzu se zobrazí, jak bude zpracováván souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="f09ac-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f09ac-177">[Předchozí](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [další](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="f09ac-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
