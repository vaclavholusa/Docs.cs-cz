---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aktualizace, odstraňování a vytváření dat pomocí vazby modelu a webové formuláře | Microsoft Docs
author: tfitzmac
description: Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Interakce dat umožňuje vazby modelu další přímo-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: e6536f7858afde5faf3aedd34f3cbe95c5ed0d53
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885842"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="506d2-104">Aktualizace, odstraňování a vytváření dat pomocí vazby modelu a webové formuláře</span><span class="sxs-lookup"><span data-stu-id="506d2-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="506d2-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="506d2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="506d2-106">Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="506d2-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="506d2-107">Vazby modelu umožňuje dat interakce více jednoduché než plánování práce s daty objekty zdrojů (například ObjectDataSource nebo SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="506d2-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="506d2-108">Tato řada začíná úvodní informace a přesune do dalších pokročilých konceptů v dalších kurzech.</span><span class="sxs-lookup"><span data-stu-id="506d2-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="506d2-109">Tento kurz ukazuje, jak vytvářet, aktualizovat a odstraňovat data s vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="506d2-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="506d2-110">Bude nastavte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="506d2-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="506d2-111">Metoda DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="506d2-111">DeleteMethod</span></span>
> - <span data-ttu-id="506d2-112">Metodu InsertMethod</span><span class="sxs-lookup"><span data-stu-id="506d2-112">InsertMethod</span></span>
> - <span data-ttu-id="506d2-113">Metoda UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="506d2-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="506d2-114">Tyto vlastnosti zobrazí název metody, která zpracovává odpovídající operaci.</span><span class="sxs-lookup"><span data-stu-id="506d2-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="506d2-115">V rámci této metody poskytují logiku pro interakci s daty.</span><span class="sxs-lookup"><span data-stu-id="506d2-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="506d2-116">V tomto kurzu vychází projektu vytvořeného v prvním [část](retrieving-data.md) řady.</span><span class="sxs-lookup"><span data-stu-id="506d2-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="506d2-117">Můžete [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=286116) dokončený projekt v jazyce C# nebo VB.</span><span class="sxs-lookup"><span data-stu-id="506d2-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="506d2-118">Kód ke stažení pracuje s Visual Studio 2012 nebo Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="506d2-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="506d2-119">Používá šablony sady Visual Studio 2012, která se poněkud liší od šablony sady Visual Studio 2013 uvedené v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="506d2-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="506d2-120">Co budete sestavení</span><span class="sxs-lookup"><span data-stu-id="506d2-120">What you'll build</span></span>

<span data-ttu-id="506d2-121">V tomto kurzu budete:</span><span class="sxs-lookup"><span data-stu-id="506d2-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="506d2-122">Přidání šablony dynamických dat.</span><span class="sxs-lookup"><span data-stu-id="506d2-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="506d2-123">Povolit aktualizaci a odstraňování dat prostřednictvím metody vazby modelu</span><span class="sxs-lookup"><span data-stu-id="506d2-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="506d2-124">Použít pravidla ověření - povolit vytváření nový záznam v databázi</span><span class="sxs-lookup"><span data-stu-id="506d2-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="506d2-125">Přidání šablony dynamických dat.</span><span class="sxs-lookup"><span data-stu-id="506d2-125">Add dynamic data templates</span></span>

<span data-ttu-id="506d2-126">K poskytování nejlepších výsledků a minimalizovat kód opakování, budete používat dynamická data šablony.</span><span class="sxs-lookup"><span data-stu-id="506d2-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="506d2-127">Dynamická data předem připravených šablon můžete snadno integrovat do existující lokalitu pomocí instalace balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="506d2-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="506d2-128">Z **spravovat balíčky NuGet**, nainstalujte **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="506d2-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![Šablony dynamických dat.](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="506d2-130">Všimněte si, že váš projekt nyní obsahuje složku s názvem **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="506d2-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="506d2-131">V této složce zjistíte šablony, které budou automaticky použita pro dynamické ovládacích prvků v webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="506d2-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![Dynamická data složky](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="506d2-133">Povolit aktualizace a odstranění</span><span class="sxs-lookup"><span data-stu-id="506d2-133">Enable updating and deleting</span></span>

<span data-ttu-id="506d2-134">Uživatelé budou moct aktualizovat a odstranit záznamy v databázi je velmi podobný procesu pro načítání dat.</span><span class="sxs-lookup"><span data-stu-id="506d2-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="506d2-135">V **metody UpdateMethod** a **metody DeleteMethod** vlastnosti, zadejte názvy metod, které provádí tyto operace.</span><span class="sxs-lookup"><span data-stu-id="506d2-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="506d2-136">Ovládací rutina GridView můžete také zadat automatické generování upravit a odstranit tlačítka.</span><span class="sxs-lookup"><span data-stu-id="506d2-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="506d2-137">Následující zvýrazněný kód ukazuje dodatky GridView kódu.</span><span class="sxs-lookup"><span data-stu-id="506d2-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="506d2-138">V souboru kódu na pozadí, přidat, pomocí příkazu pro **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="506d2-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="506d2-139">Pak přidejte následující aktualizace a odstranění metody.</span><span class="sxs-lookup"><span data-stu-id="506d2-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="506d2-140">**TryUpdateModel** metoda platí odpovídající hodnoty vázané na data z webového formuláře pro datová položka.</span><span class="sxs-lookup"><span data-stu-id="506d2-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="506d2-141">Datové položky se načítají podle hodnota parametru id.</span><span class="sxs-lookup"><span data-stu-id="506d2-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="506d2-142">Vynutit požadavky na ověření</span><span class="sxs-lookup"><span data-stu-id="506d2-142">Enforce validation requirements</span></span>

<span data-ttu-id="506d2-143">Vynutí použité pro vlastnosti FirstName, LastName a rok ve třídě Student atributy ověření se automaticky při aktualizaci data.</span><span class="sxs-lookup"><span data-stu-id="506d2-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="506d2-144">Ovládací prvky DynamicField přidat klientských a serverových validátory na základě atributů ověření.</span><span class="sxs-lookup"><span data-stu-id="506d2-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="506d2-145">Vlastnosti FirstName a LastName se vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="506d2-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="506d2-146">FirstName nesmí překročit 20 znaků a LastName nemůže být delší než 40 znaků.</span><span class="sxs-lookup"><span data-stu-id="506d2-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="506d2-147">Rok musí být platná hodnota pro výčet AcademicYear.</span><span class="sxs-lookup"><span data-stu-id="506d2-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="506d2-148">Pokud uživatel je v rozporu mezi požadavky na ověřování, aktualizace nebude pokračovat.</span><span class="sxs-lookup"><span data-stu-id="506d2-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="506d2-149">Pokud chcete zobrazit chybová zpráva, přidání ovládacího prvku ValidationSummary výše GridView.</span><span class="sxs-lookup"><span data-stu-id="506d2-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="506d2-150">Chcete-li zobrazit chyby ověření z vazby modelu, nastavte **ShowModelStateErrors** vlastnost nastavena na hodnotu **true**.</span><span class="sxs-lookup"><span data-stu-id="506d2-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="506d2-151">Spusťte webovou aplikaci a aktualizovat a odstranit všechny záznamy.</span><span class="sxs-lookup"><span data-stu-id="506d2-151">Run the web application, and update and delete any of the records.</span></span>

![Aktualizace dat](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="506d2-153">Všimněte si, když v režimu úprav hodnotu pro vlastnost rok automaticky vykresleno jako rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="506d2-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="506d2-154">Vlastnost roku je hodnota výčtu a šablona dynamických dat pro hodnotu výčtu určuje rozevíracího seznamu pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="506d2-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="506d2-155">Tato šablona můžete najít tak, že otevřete **– výčet\_Edit.ascx** souboru v **DynamicData**/**FieldTemplates** složky.</span><span class="sxs-lookup"><span data-stu-id="506d2-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="506d2-156">Pokud jste zadali platné hodnoty, aktualizace se úspěšně dokončí.</span><span class="sxs-lookup"><span data-stu-id="506d2-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="506d2-157">Pokud porušují mezi požadavky na ověřování, aktualizace nebude pokračovat a chybová zpráva se zobrazí nad mřížky.</span><span class="sxs-lookup"><span data-stu-id="506d2-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![Chybová zpráva](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="506d2-159">Přidat nové záznamy</span><span class="sxs-lookup"><span data-stu-id="506d2-159">Add new records</span></span>

<span data-ttu-id="506d2-160">Ovládací prvek GridView nezahrnuje **metodu InsertMethod** vlastnost a nelze jej proto použít pro přidávání nového záznamu s vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="506d2-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="506d2-161">Můžete najít vlastnost v metodu InsertMethod **FormView**, **DetailsView**, nebo **ListView** ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="506d2-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="506d2-162">V tomto kurzu použijete v ovládacím prvku FormView přidejte nový záznam.</span><span class="sxs-lookup"><span data-stu-id="506d2-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="506d2-163">Nejprve přidejte odkaz na novou stránku, kterou vytvoříte pro přidávání nového záznamu.</span><span class="sxs-lookup"><span data-stu-id="506d2-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="506d2-164">Nad ValidationSummary přidejte:</span><span class="sxs-lookup"><span data-stu-id="506d2-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="506d2-165">Nový odkaz se zobrazí v horní části obsahu pro studenty stránku.</span><span class="sxs-lookup"><span data-stu-id="506d2-165">The new link will appear at the top of the content for the Students page.</span></span>

![nové propojení](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="506d2-167">Pak přidejte nový webový formulář pomocí hlavní stránky a pojmenujte ji **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="506d2-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="506d2-168">Vyberte Site.Master jako stránky předlohy.</span><span class="sxs-lookup"><span data-stu-id="506d2-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="506d2-169">Učiní polí pro přidání nové student pomocí **DynamicEntity** ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="506d2-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="506d2-170">Ovládací prvek DynamicEntity vykreslí této upravovat vlastnosti ve třídě zadaná ve vlastnosti typ položky.</span><span class="sxs-lookup"><span data-stu-id="506d2-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="506d2-171">Vlastnost StudentID byla označena **[ScaffoldColumn(false)]** atribut, není vykreslen.</span><span class="sxs-lookup"><span data-stu-id="506d2-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="506d2-172">MainContent zástupného textu stránky AddStudent přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="506d2-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="506d2-173">V souboru kódu na pozadí (AddStudent.aspx.cs), přidejte **pomocí** údajů **ContosoUniversityModelBinding.Models** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="506d2-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="506d2-174">Pak přidejte následující metody, které určete, jak chcete-li vložit nový záznam a obslužné rutiny události pro tlačítko Storno.</span><span class="sxs-lookup"><span data-stu-id="506d2-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="506d2-175">Všechny změny uložte.</span><span class="sxs-lookup"><span data-stu-id="506d2-175">Save all of the changes.</span></span>

<span data-ttu-id="506d2-176">Spustit webovou aplikaci a vytvořit nový student.</span><span class="sxs-lookup"><span data-stu-id="506d2-176">Run the web application and create a new student.</span></span>

![Přidat nový student](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="506d2-178">Klikněte na tlačítko **vložit** a Všimněte si vytvořil nový student.</span><span class="sxs-lookup"><span data-stu-id="506d2-178">Click **Insert** and notice the new student has been created.</span></span>

![Nový student zobrazení](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="506d2-180">Závěr</span><span class="sxs-lookup"><span data-stu-id="506d2-180">Conclusion</span></span>

<span data-ttu-id="506d2-181">V tomto kurzu jste povolili, aktualizaci, odstranění a vytvoření data.</span><span class="sxs-lookup"><span data-stu-id="506d2-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="506d2-182">Můžete zajistit, že ověření pravidla se používají při interakci s daty.</span><span class="sxs-lookup"><span data-stu-id="506d2-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="506d2-183">V dalším [kurzu](sorting-paging-and-filtering-data.md) této série povolíte řazení, stránkování a filtrování dat.</span><span class="sxs-lookup"><span data-stu-id="506d2-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="506d2-184">[Předchozí](retrieving-data.md)
> [další](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="506d2-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
