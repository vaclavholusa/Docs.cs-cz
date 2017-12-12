---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: "Pomocí hodnoty řetězců dotazu pro filtrování dat pomocí vazby modelu a webových formulářů | Microsoft Docs"
author: tfitzmac
description: "Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Interakce dat umožňuje vazby modelu další přímo-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 2e5328ccda019462163b984da3661f7322b738df
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="b84cc-104">Hodnoty řetězců dotazu pro filtrování dat pomocí vazby modelu a webové formuláře</span><span class="sxs-lookup"><span data-stu-id="b84cc-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="b84cc-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b84cc-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b84cc-106">Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="b84cc-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="b84cc-107">Vazby modelu umožňuje dat interakce více jednoduché než plánování práce s daty objekty zdrojů (například ObjectDataSource nebo SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="b84cc-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="b84cc-108">Tato řada začíná úvodní informace a přesune do dalších pokročilých konceptů v dalších kurzech.</span><span class="sxs-lookup"><span data-stu-id="b84cc-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="b84cc-109">Tento kurz ukazuje, jak předat hodnotu v řetězci dotazu a použít tuto hodnotu k načtení dat pomocí vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="b84cc-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="b84cc-110">V tomto kurzu vychází vytvořené v projektu [starší](retrieving-data.md) částí řady.</span><span class="sxs-lookup"><span data-stu-id="b84cc-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="b84cc-111">Můžete [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=286116) dokončený projekt v jazyce C# nebo VB.</span><span class="sxs-lookup"><span data-stu-id="b84cc-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="b84cc-112">Kód ke stažení pracuje s Visual Studio 2012 nebo Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b84cc-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="b84cc-113">Používá šablony sady Visual Studio 2012, která se poněkud liší od šablony sady Visual Studio 2013 uvedené v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b84cc-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="b84cc-114">Co budete sestavení</span><span class="sxs-lookup"><span data-stu-id="b84cc-114">What you'll build</span></span>

<span data-ttu-id="b84cc-115">V tomto kurzu budete:</span><span class="sxs-lookup"><span data-stu-id="b84cc-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="b84cc-116">Přidat novou stránku zobrazíte zaregistrovaná kurzy pro student</span><span class="sxs-lookup"><span data-stu-id="b84cc-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="b84cc-117">Načtení zaregistrovaná kurzy pro vybrané student založená na hodnotě v řetězci dotazu</span><span class="sxs-lookup"><span data-stu-id="b84cc-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="b84cc-118">Přidání hypertextového odkazu s hodnotou řetězce dotazu v zobrazení mřížky na novou stránku</span><span class="sxs-lookup"><span data-stu-id="b84cc-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="b84cc-119">Kroky v tomto kurzu jsou dost podobné, co jste udělali v dříve [kurzu](sorting-paging-and-filtering-data.md) k filtrování zobrazených studenty na základě výběru uživatele v rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="b84cc-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="b84cc-120">V tomto kurzu jste použili **řízení** atribut v metodě vyberte, chcete-li určit, že hodnota parametru pochází z ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="b84cc-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="b84cc-121">V tomto kurzu budete používat **řetězce dotazu** atribut v metodě vyberte, chcete-li určit, že hodnota parametru pochází z řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="b84cc-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="b84cc-122">Přidat novou stránku pro zobrazení Studentova kurzy</span><span class="sxs-lookup"><span data-stu-id="b84cc-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="b84cc-123">Přidejte nový webový formulář, který používá stránky předlohy Site.master a název stránky **kurzy**.</span><span class="sxs-lookup"><span data-stu-id="b84cc-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="b84cc-124">V **Courses.aspx** soubor, přidejte zobrazení mřížky kurzy pro vybrané student.</span><span class="sxs-lookup"><span data-stu-id="b84cc-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="b84cc-125">Zadejte metodu vyberte</span><span class="sxs-lookup"><span data-stu-id="b84cc-125">Define the select method</span></span>

<span data-ttu-id="b84cc-126">V **Courses.aspx.cs**, přidáte vyberte metodu s názvem zadaným v zobrazení mřížky **metody SelectMethod** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b84cc-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="b84cc-127">V této metodě budete zadejte dotaz pro načtení Studentova kurzy a určit, že parametr pochází z hodnoty řetězce dotazu se stejným názvem jako parametr.</span><span class="sxs-lookup"><span data-stu-id="b84cc-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="b84cc-128">Nejprve je nutné přidat následující **pomocí** příkazy.</span><span class="sxs-lookup"><span data-stu-id="b84cc-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="b84cc-129">Potom si do Courses.aspx.cs přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="b84cc-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="b84cc-130">Atribut QueryString znamená, že hodnotu řetězce dotazu s názvem StudentID se automaticky přiřadí k parametr v této metodě.</span><span class="sxs-lookup"><span data-stu-id="b84cc-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="b84cc-131">Přidání hypertextového odkazu s hodnotou řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="b84cc-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="b84cc-132">V zobrazení mřížky na Students.aspx přidáte pole hypertextový odkaz, který odkazuje na novou stránku kurzy.</span><span class="sxs-lookup"><span data-stu-id="b84cc-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="b84cc-133">Hypertextový odkaz bude obsahovat hodnotu řetězce dotazu s id Studentova.</span><span class="sxs-lookup"><span data-stu-id="b84cc-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="b84cc-134">V Students.aspx přidejte následující pole do sloupce zobrazení mřížky pod pole pro celkový počet kredity.</span><span class="sxs-lookup"><span data-stu-id="b84cc-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="b84cc-135">Spusťte aplikaci a Všimněte si, že zobrazení mřížky nyní zahrnuje odkaz kurzy.</span><span class="sxs-lookup"><span data-stu-id="b84cc-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Přidání hypertextového odkazu](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="b84cc-137">Když kliknete na jeden z odkazů, uvidíte této Studentova zaregistrovaná kurzy.</span><span class="sxs-lookup"><span data-stu-id="b84cc-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![Zobrazit kurzy](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="b84cc-139">Závěr</span><span class="sxs-lookup"><span data-stu-id="b84cc-139">Conclusion</span></span>

<span data-ttu-id="b84cc-140">V tomto kurzu jste přidali odkaz s hodnotou řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="b84cc-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="b84cc-141">Tuto hodnotu řetězce dotazu jste použili pro hodnotu parametru metody select.</span><span class="sxs-lookup"><span data-stu-id="b84cc-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="b84cc-142">V dalším [kurzu](adding-business-logic-layer.md), kód se přesune ze souborů kódu do vrstvy obchodní logiky a vrstva přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="b84cc-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b84cc-143">[Předchozí](integrating-jquery-ui.md)
[další](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="b84cc-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
