---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: "Přidání vrstvu obchodní logiky do projektu, který používá vazby modelu a webové formuláře | Microsoft Docs"
author: tfitzmac
description: "Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Interakce dat umožňuje vazby modelu další přímo-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: ca50690052cca73a718342a9725c8096a72f1187
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="9d203-104">Přidání vrstvu obchodní logiky do projektu, který používá vazby modelu a webové formuláře</span><span class="sxs-lookup"><span data-stu-id="9d203-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="9d203-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9d203-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9d203-106">Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="9d203-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="9d203-107">Vazby modelu umožňuje dat interakce více jednoduché než plánování práce s daty objekty zdrojů (například ObjectDataSource nebo SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="9d203-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="9d203-108">Tato řada začíná úvodní informace a přesune do dalších pokročilých konceptů v dalších kurzech.</span><span class="sxs-lookup"><span data-stu-id="9d203-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="9d203-109">Tento kurz ukazuje, jak použít modelovou vazbu s vrstvu obchodní logiky.</span><span class="sxs-lookup"><span data-stu-id="9d203-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="9d203-110">Nastavíte člen OnCallingDataMethods k určení, že objekt než aktuální stránku se používá k volání metody datového.</span><span class="sxs-lookup"><span data-stu-id="9d203-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="9d203-111">V tomto kurzu vychází vytvořené v projektu [starší](retrieving-data.md) částí řady.</span><span class="sxs-lookup"><span data-stu-id="9d203-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="9d203-112">Můžete [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=286116) dokončený projekt v jazyce C# nebo VB.</span><span class="sxs-lookup"><span data-stu-id="9d203-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="9d203-113">Kód ke stažení pracuje s Visual Studio 2012 nebo Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="9d203-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="9d203-114">Používá šablony sady Visual Studio 2012, která se poněkud liší od šablony sady Visual Studio 2013 uvedené v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="9d203-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="9d203-115">Co budete sestavení</span><span class="sxs-lookup"><span data-stu-id="9d203-115">What you'll build</span></span>

<span data-ttu-id="9d203-116">Vazby modelu umožňuje ukládat kód interakce data v souboru kódu pro webovou stránku nebo v samostatné obchodní logiky třídě.</span><span class="sxs-lookup"><span data-stu-id="9d203-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="9d203-117">Předchozí kurzy ukázaly, jak používat soubory kódu pro kód interakce data.</span><span class="sxs-lookup"><span data-stu-id="9d203-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="9d203-118">Tento postup funguje pro malé weby, ale může vést k kódu opakování a větší potíže při zachování velkých webů.</span><span class="sxs-lookup"><span data-stu-id="9d203-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="9d203-119">Může být také velmi obtížné prostřednictvím kódu programu testování kódu, který se nachází v kódu na pozadí soubory, protože neexistuje žádný abstraktní vrstvu.</span><span class="sxs-lookup"><span data-stu-id="9d203-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="9d203-120">A centralizovat data interakce kód, můžete vytvořit vrstvu obchodní logiky, která obsahuje veškerá logiku pro interakci s daty.</span><span class="sxs-lookup"><span data-stu-id="9d203-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="9d203-121">Potom zavolejte vrstvu obchodní logiky z webových stránek.</span><span class="sxs-lookup"><span data-stu-id="9d203-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="9d203-122">Tento kurz ukazuje, jak přesunout všechny kód, který jste napsali v předchozí kurzech do vrstvy obchodní logiky a potom pomocí tohoto kódu ze stránky.</span><span class="sxs-lookup"><span data-stu-id="9d203-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="9d203-123">V tomto kurzu budete:</span><span class="sxs-lookup"><span data-stu-id="9d203-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="9d203-124">Přesun kód z kódu soubory do vrstvy obchodní logiky</span><span class="sxs-lookup"><span data-stu-id="9d203-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="9d203-125">Změnit vaše data vázaná ovládací prvky pro volání metody v vrstvu obchodní logiky</span><span class="sxs-lookup"><span data-stu-id="9d203-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="9d203-126">Vytvořte vrstvu obchodní logiky</span><span class="sxs-lookup"><span data-stu-id="9d203-126">Create business logic layer</span></span>

<span data-ttu-id="9d203-127">Teď vytvoříte třídu, která je volána z webové stránky.</span><span class="sxs-lookup"><span data-stu-id="9d203-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="9d203-128">Metody v této třídě vypadat podobně jako metody, které jste použili v předchozí kurzy a zahrnují atributy zprostředkovatele hodnot.</span><span class="sxs-lookup"><span data-stu-id="9d203-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="9d203-129">Nejprve přidejte novou složku s názvem **BLL**.</span><span class="sxs-lookup"><span data-stu-id="9d203-129">First, add a new folder called **BLL**.</span></span>

![Přidat složku](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="9d203-131">Ve složce BLL, vytvořte novou třídu s názvem **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="9d203-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="9d203-132">Bude obsahovat všechny operace dat, které původně bydliště v soubory kódu.</span><span class="sxs-lookup"><span data-stu-id="9d203-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="9d203-133">Metody jsou téměř stejné jako metody v souboru kódu na pozadí, ale bude obsahovat některé změny.</span><span class="sxs-lookup"><span data-stu-id="9d203-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="9d203-134">Je nejdůležitější změny si uvědomit, jsou už provádění kódu z v rámci instance **stránky** třídy.</span><span class="sxs-lookup"><span data-stu-id="9d203-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="9d203-135">Obsahuje třídy stránky **TryUpdateModel** metoda a **ModelState** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="9d203-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="9d203-136">Pokud tento kód je přesunuta do vrstvy obchodní logiky, už mít instanci třídy stránky volat tito členové.</span><span class="sxs-lookup"><span data-stu-id="9d203-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="9d203-137">Chcete-li získat chcete tento problém vyřešit, je nutné přidat **ModelMethodContext** do libovolné metody, která přistupuje k TryUpdateModel nebo ModelState parametr.</span><span class="sxs-lookup"><span data-stu-id="9d203-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="9d203-138">Tento parametr ModelMethodContext použijete zavolat TryUpdateModel nebo načíst ModelState.</span><span class="sxs-lookup"><span data-stu-id="9d203-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="9d203-139">Není nutné změnit žádnou na webové stránce, aby se zohlednily tento nový parametr.</span><span class="sxs-lookup"><span data-stu-id="9d203-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="9d203-140">Nahraďte kód v SchoolBL.cs následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="9d203-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="9d203-141">Zkontrolovat, jestli existující stránky k načtení dat z vrstvu obchodní logiky</span><span class="sxs-lookup"><span data-stu-id="9d203-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="9d203-142">Nakonec se převede na stránkách Students.aspx AddStudent.aspx a Courses.aspx z pomocí dotazů v souboru kódu pomocí vrstvu obchodní logiky.</span><span class="sxs-lookup"><span data-stu-id="9d203-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="9d203-143">Soubory kódu pro studenty, AddStudent a kurzy odstranit nebo komentář následující metody dotazu:</span><span class="sxs-lookup"><span data-stu-id="9d203-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="9d203-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="9d203-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="9d203-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="9d203-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="9d203-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="9d203-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="9d203-147">addStudentForm\_metody InsertItem</span><span class="sxs-lookup"><span data-stu-id="9d203-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="9d203-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="9d203-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="9d203-149">Nyní byste měli mít příslušné žádný kód v souboru kódu, který se vztahují na data operací.</span><span class="sxs-lookup"><span data-stu-id="9d203-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="9d203-150">**OnCallingDataMethods** obslužné rutiny události můžete určit objekt, který použijete pro metody data.</span><span class="sxs-lookup"><span data-stu-id="9d203-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="9d203-151">V Students.aspx přidejte hodnotu této obslužné rutiny události a změnit názvy metod dat na názvy metod ve třídě obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="9d203-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="9d203-152">V souboru kódu na pozadí pro Students.aspx definujte obslužné rutiny události pro událost CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="9d203-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="9d203-153">V této obslužné rutiny události zadejte třídě obchodní logiku pro datové operace.</span><span class="sxs-lookup"><span data-stu-id="9d203-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="9d203-154">V AddStudent.aspx proveďte podobné změny.</span><span class="sxs-lookup"><span data-stu-id="9d203-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="9d203-155">V Courses.aspx proveďte podobné změny.</span><span class="sxs-lookup"><span data-stu-id="9d203-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="9d203-156">Spusťte aplikaci a Všimněte si, že všechny stránky fungovat jako měly dříve.</span><span class="sxs-lookup"><span data-stu-id="9d203-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="9d203-157">Logiku ověření také funguje správně.</span><span class="sxs-lookup"><span data-stu-id="9d203-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="9d203-158">Závěr</span><span class="sxs-lookup"><span data-stu-id="9d203-158">Conclusion</span></span>

<span data-ttu-id="9d203-159">V tomto kurzu znovu strukturovaná aplikace k používání vrstva přístupu k datům a vrstvu obchodní logiky.</span><span class="sxs-lookup"><span data-stu-id="9d203-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="9d203-160">Zadali jste, že ovládací prvky datových používat objekt, který není na aktuální stránce pro operace dat.</span><span class="sxs-lookup"><span data-stu-id="9d203-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="9d203-161">Předchozí</span><span class="sxs-lookup"><span data-stu-id="9d203-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
