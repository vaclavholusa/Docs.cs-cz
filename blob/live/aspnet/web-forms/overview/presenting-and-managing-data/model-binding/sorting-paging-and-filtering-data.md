---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: "Řazení, stránkování a filtrování dat pomocí vazby modelu a webové formuláře | Microsoft Docs"
author: tfitzmac
description: "Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Interakce dat umožňuje vazby modelu další přímo-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 94fc84533be5fcbcf0612fcdcabea7dee738d89b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="e615e-104">Řazení, stránkování a filtrování dat pomocí vazby modelu a webové formuláře</span><span class="sxs-lookup"><span data-stu-id="e615e-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="e615e-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e615e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e615e-106">Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="e615e-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="e615e-107">Vazby modelu umožňuje dat interakce více jednoduché než plánování práce s daty objekty zdrojů (například ObjectDataSource nebo SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="e615e-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="e615e-108">Tato řada začíná úvodní informace a přesune do dalších pokročilých konceptů v dalších kurzech.</span><span class="sxs-lookup"><span data-stu-id="e615e-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="e615e-109">Tento kurz ukazuje, jak přidat řazení, stránkování a filtrování dat pomocí vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="e615e-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="e615e-110">V tomto kurzu vychází projektu vytvořeného v prvním [část](retrieving-data.md) řady.</span><span class="sxs-lookup"><span data-stu-id="e615e-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="e615e-111">Můžete [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=286116) dokončený projekt v jazyce C# nebo VB.</span><span class="sxs-lookup"><span data-stu-id="e615e-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="e615e-112">Kód ke stažení pracuje s Visual Studio 2012 nebo Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e615e-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="e615e-113">Používá šablony sady Visual Studio 2012, která se poněkud liší od šablony sady Visual Studio 2013 uvedené v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e615e-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="e615e-114">Co budete sestavení</span><span class="sxs-lookup"><span data-stu-id="e615e-114">What you'll build</span></span>

<span data-ttu-id="e615e-115">V tomto kurzu budete:</span><span class="sxs-lookup"><span data-stu-id="e615e-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="e615e-116">Povolit řazení a stránkování dat</span><span class="sxs-lookup"><span data-stu-id="e615e-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="e615e-117">Povolit filtrování dat na základě výběru uživatelem.</span><span class="sxs-lookup"><span data-stu-id="e615e-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="e615e-118">Přidat řazení</span><span class="sxs-lookup"><span data-stu-id="e615e-118">Add sorting</span></span>

<span data-ttu-id="e615e-119">Povolení řazení v GridView je velmi snadné.</span><span class="sxs-lookup"><span data-stu-id="e615e-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="e615e-120">V souboru Student.aspx jednoduše nastavit **AllowSorting** k **true** v GridView.</span><span class="sxs-lookup"><span data-stu-id="e615e-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="e615e-121">Není nutné nastavovat **SortExpression** hodnotu pro každý sloupec, jako DataField automaticky použije.</span><span class="sxs-lookup"><span data-stu-id="e615e-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="e615e-122">GridView upravuje dotazu zahrnout uspořádání dat podle vybrané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e615e-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="e615e-123">Následující zvýrazněný kód ukazuje přidání, že budete muset udělat umožňující řazení.</span><span class="sxs-lookup"><span data-stu-id="e615e-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="e615e-124">Spuštění webové aplikace a testování řazení záznamů student podle hodnot v různé sloupce.</span><span class="sxs-lookup"><span data-stu-id="e615e-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![studenti, kteří řazení](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="e615e-126">Přidat stránkování</span><span class="sxs-lookup"><span data-stu-id="e615e-126">Add paging</span></span>

<span data-ttu-id="e615e-127">Povolení stránkování je také velmi snadné.</span><span class="sxs-lookup"><span data-stu-id="e615e-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="e615e-128">V GridView, nastavte **AllowPaging** vlastnost **true** a nastavte **PageSize** vlastnost počet záznamů, které chcete zobrazit na každé stránce.</span><span class="sxs-lookup"><span data-stu-id="e615e-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="e615e-129">V tomto kurzu můžete ho nastavit na 4.</span><span class="sxs-lookup"><span data-stu-id="e615e-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="e615e-130">Spusťte webovou aplikaci a Všimněte si, že teď záznamy dělí přes více stránek více než 4 záznamy, které jsou zobrazeny na jedné stránce.</span><span class="sxs-lookup"><span data-stu-id="e615e-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![Přidat stránkování](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="e615e-132">Při provádění dotazu odložené ke zlepšení efektivity aplikace.</span><span class="sxs-lookup"><span data-stu-id="e615e-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="e615e-133">Místo načítání celá sada dat, upraví GridView dotaz pro načtení záznamů pouze pro aktuální stránku.</span><span class="sxs-lookup"><span data-stu-id="e615e-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="e615e-134">Filtrování záznamů pomocí výběr uživatele</span><span class="sxs-lookup"><span data-stu-id="e615e-134">Filter records by user selection</span></span>

<span data-ttu-id="e615e-135">Vazby modelu přidá několik atributů, které vám umožní určit, jak nastavit hodnotu pro parametr v metodu vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="e615e-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="e615e-136">Tyto atributy jsou v **System.Web.ModelBinding** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="e615e-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="e615e-137">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="e615e-137">They include:</span></span>

- <span data-ttu-id="e615e-138">Ovládací prvek</span><span class="sxs-lookup"><span data-stu-id="e615e-138">Control</span></span>
- <span data-ttu-id="e615e-139">Soubor cookie</span><span class="sxs-lookup"><span data-stu-id="e615e-139">Cookie</span></span>
- <span data-ttu-id="e615e-140">Formulář</span><span class="sxs-lookup"><span data-stu-id="e615e-140">Form</span></span>
- <span data-ttu-id="e615e-141">Profil</span><span class="sxs-lookup"><span data-stu-id="e615e-141">Profile</span></span>
- <span data-ttu-id="e615e-142">Řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="e615e-142">QueryString</span></span>
- <span data-ttu-id="e615e-143">Objekt RouteData</span><span class="sxs-lookup"><span data-stu-id="e615e-143">RouteData</span></span>
- <span data-ttu-id="e615e-144">Relace</span><span class="sxs-lookup"><span data-stu-id="e615e-144">Session</span></span>
- <span data-ttu-id="e615e-145">UserProfile</span><span class="sxs-lookup"><span data-stu-id="e615e-145">UserProfile</span></span>
- <span data-ttu-id="e615e-146">Stav zobrazení</span><span class="sxs-lookup"><span data-stu-id="e615e-146">ViewState</span></span>

<span data-ttu-id="e615e-147">V tomto kurzu použijete hodnotu ovládacího prvku pro filtrování záznamů, které se zobrazují v GridView.</span><span class="sxs-lookup"><span data-stu-id="e615e-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="e615e-148">Přidáte **řízení** atribut metodu dotazu, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="e615e-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="e615e-149">V [později](using-query-string-values-to-retrieve-data.md) kurzu použijete **řetězce dotazu** atribut parametr k určení, že hodnota parametru pochází z hodnotu řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="e615e-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="e615e-150">Nejprve přidejte výše ValidationSummary, rozevírací seznam pro filtrování, která studenti, kteří jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="e615e-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="e615e-151">V souboru kódu změňte vyberte metodu získat hodnotu z ovládacího prvku a nastavte název parametru k názvu ovládací prvek, který obsahuje hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e615e-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="e615e-152">Je nutné přidat **pomocí** příkaz pro **System.Web.ModelBinding** obor názvů přeložit atribut ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="e615e-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="e615e-153">Následující kód ukazuje znovu fungovala vyfiltrujete vrácená data na základě hodnoty z rozevíracího seznamu vyberte metodu.</span><span class="sxs-lookup"><span data-stu-id="e615e-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="e615e-154">Přidávání atribut ovládacího prvku před parametr určuje, že hodnota pro tento parametr pochází z ovládacího prvku se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="e615e-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="e615e-155">Spuštění webové aplikace a vyberte z rozevíracího seznamu pro filtrování seznamu studenty různé hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e615e-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![studenti, kteří filtru](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="e615e-157">Závěr</span><span class="sxs-lookup"><span data-stu-id="e615e-157">Conclusion</span></span>

<span data-ttu-id="e615e-158">V tomto kurzu jste povolili, řazení a stránkování data.</span><span class="sxs-lookup"><span data-stu-id="e615e-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="e615e-159">Můžete také povolit, filtrování dat podle hodnoty ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="e615e-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="e615e-160">V dalším [kurzu](integrating-jquery-ui.md) uživatelského rozhraní se zlepšila integrací widget uživatelského rozhraní JQuery do šablony dynamická data.</span><span class="sxs-lookup"><span data-stu-id="e615e-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e615e-161">[Předchozí](updating-deleting-and-creating-data.md)
[další](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="e615e-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
