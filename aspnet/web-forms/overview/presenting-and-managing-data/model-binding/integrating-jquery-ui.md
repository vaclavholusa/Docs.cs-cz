---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrace s vazby modelu a webových formulářů JQuery UI ovládací prvek Datepicker | Microsoft Docs
author: tfitzmac
description: Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Interakce dat umožňuje vazby modelu další přímo-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 126262b440f3e914a7fac3f0b7eeadb4f648d2bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887909"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="aa7a6-104">Ovládací prvek Datepicker uživatelského rozhraní JQuery integrování vazby modelu a webové formuláře</span><span class="sxs-lookup"><span data-stu-id="aa7a6-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="aa7a6-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="aa7a6-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="aa7a6-106">Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="aa7a6-107">Vazby modelu umožňuje dat interakce více jednoduché než plánování práce s daty objekty zdrojů (například ObjectDataSource nebo SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="aa7a6-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="aa7a6-108">Tato řada začíná úvodní informace a přesune do dalších pokročilých konceptů v dalších kurzech.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="aa7a6-109">Tento kurz ukazuje, jak přidat rozhraní JQuery [ovládací prvek Datepicker pomůcky](http://jqueryui.com/datepicker/) webového formuláře a použití modelu vazby k aktualizaci databáze s vybrané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="aa7a6-110">V tomto kurzu vychází vytvořené v projektu [první](retrieving-data.md) a [druhý](updating-deleting-and-creating-data.md) částí řady.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="aa7a6-111">Můžete [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=286116) dokončený projekt v jazyce C# nebo VB.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="aa7a6-112">Kód ke stažení pracuje s Visual Studio 2012 nebo Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="aa7a6-113">Používá šablony sady Visual Studio 2012, která se poněkud liší od šablony sady Visual Studio 2013 uvedené v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="aa7a6-114">Co budete sestavení</span><span class="sxs-lookup"><span data-stu-id="aa7a6-114">What you'll build</span></span>

<span data-ttu-id="aa7a6-115">V tomto kurzu budete:</span><span class="sxs-lookup"><span data-stu-id="aa7a6-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="aa7a6-116">Přidání vlastnosti modelu, k zaznamenání Studentova datum zápisu</span><span class="sxs-lookup"><span data-stu-id="aa7a6-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="aa7a6-117">Povolit uživateli vybrat datum registrace pomocí widgetu ovládací prvek Datepicker uživatelského rozhraní JQuery</span><span class="sxs-lookup"><span data-stu-id="aa7a6-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="aa7a6-118">Vynutit ověřovacích pravidel pro datum zápisu</span><span class="sxs-lookup"><span data-stu-id="aa7a6-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="aa7a6-119">Ovládací prvek Datepicker uživatelského rozhraní JQuery pomůcky umožňuje uživatelům snadno vyberte datum z kalendáře, která se objeví po uživatel pracuje s polem.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="aa7a6-120">Pomocí tohoto widgetu může být pro uživatele snazší než ručním zadáním datum.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="aa7a6-121">Integrace widgetu ovládací prvek Datepicker do stránky, která používá vazby modelu pro datové operace vyžaduje jenom malé množství další práci.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="aa7a6-122">Přidat novou vlastnost modelu</span><span class="sxs-lookup"><span data-stu-id="aa7a6-122">Add a new property to the model</span></span>

<span data-ttu-id="aa7a6-123">Nejprve budete přidávat **data a času** vlastnosti tak, aby vaše Student modelu a tato změna migrovat do databáze.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="aa7a6-124">Otevřete **UniversityModels.cs**a přidejte do modelu Student zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="aa7a6-125">**RangeAttribute** je součástí vynutit ověřovacích pravidel pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="aa7a6-126">V tomto kurzu budeme předpokládat že Contoso univerzity byla založena na 1. leden 2013, a proto starší registrace data nejsou platná.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="aa7a6-127">V okně správy balíčků přidat migrace spuštěním příkazu **přidat migrace AddEnrollmentDate**.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="aa7a6-128">Všimněte si, že kód migrace přidá do tabulky Student nový sloupec datového typu Datetime.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="aa7a6-129">Odpovídat hodnotě, kterou jste zadali v RangeAttribute, přidejte výchozí hodnota pro nový sloupec, jak je znázorněno v následující zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="aa7a6-130">Uložte změny do souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-130">Save your change to the migration file.</span></span>

<span data-ttu-id="aa7a6-131">Není nutné znovu počáteční hodnoty data.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-131">You do not need to seed the data again.</span></span> <span data-ttu-id="aa7a6-132">Proto otevřete **Configuration.cs** ve složce migrace a odeberte nebo komentář kód **počáteční hodnoty** metoda.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="aa7a6-133">Soubor uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-133">Save and close the file.</span></span>

<span data-ttu-id="aa7a6-134">Nyní, spusťte příkaz **update-database**.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="aa7a6-135">Všimněte si, že sloupec nyní existuje v databázi a všechny existující záznamy mají výchozí hodnota pro EnrollmentDate.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="aa7a6-136">Přidat dynamických ovládacích prvků pro datum zápisu</span><span class="sxs-lookup"><span data-stu-id="aa7a6-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="aa7a6-137">Nyní přidáte ovládací prvky pro zobrazení a úpravy datum registrace.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="aa7a6-138">V tomto okamžiku je upravit hodnotu v textovém poli.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="aa7a6-139">Později v tomto kurzu změníte textového pole na ovládací prvek JQuery Datepicker pomůcky.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="aa7a6-140">Nejprve je důležité si uvědomit, že nepotřebujete, aby všechny změny **AddStudent.aspx** souboru.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="aa7a6-141">Ovládací prvek DynamicEntity automaticky vykreslí novou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="aa7a6-142">Otevřete **Students.aspx**a přidejte následující zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="aa7a6-143">Spusťte aplikaci a Všimněte si, že můžete nastavit hodnotu Datum registrace zadáním datum.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="aa7a6-144">Při přidávání nové student:</span><span class="sxs-lookup"><span data-stu-id="aa7a6-144">When adding a new student:</span></span>

![Nastavte datum](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="aa7a6-146">Nebo úpravou existující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="aa7a6-146">Or, editing an existing value:</span></span>

![Upravit datum](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="aa7a6-148">Zadejte datum funguje, ale nemusí být zkušeností zákazníků, který chcete poskytnout.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="aa7a6-149">V další části povolíte výběrem data prostřednictvím kalendáře.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="aa7a6-150">Nainstalujte balíček NuGet pro práci s uživatelského rozhraní JQuery</span><span class="sxs-lookup"><span data-stu-id="aa7a6-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="aa7a6-151">**Uživatelského rozhraní šťávy** balíček NuGet umožňuje snadné integrace uživatelského rozhraní JQuery pomůcky do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="aa7a6-152">Tento balíček použít, že ji nainstalujte prostřednictvím balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-152">To use this package, install it through NuGet.</span></span>

![Přidat šťávy uživatelského rozhraní](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="aa7a6-154">Verze šťávy uživatelského rozhraní, která po instalaci může dojít ke konfliktu s verzí JQuery ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="aa7a6-155">Než budete pokračovat v tomto kurzu, opakujte spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="aa7a6-156">Pokud dojde k chybě jazyka JavaScript, budete muset sjednotit verze JQuery.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="aa7a6-157">Můžete přidat do složky skriptů (verze 1.8.2 v době psaní tohoto kurzu) očekávaná verze JQuery nebo Site.master zadejte cestu k souboru JQuery.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="aa7a6-158">Přizpůsobení šablony data a času zahrnout ovládací prvek Datepicker pomůcky</span><span class="sxs-lookup"><span data-stu-id="aa7a6-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="aa7a6-159">Ovládací prvek Datepicker pomůcky přidá do šablony dynamickými daty pro úpravy hodnotu data a času.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="aa7a6-160">Přidáním widgetu do šablony je automaticky vykreslen ve formuláři pro přidání nové student a v zobrazení mřížky pro úpravy studenty.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="aa7a6-161">Otevřete **data a času\_Edit.ascx**a přidejte následující zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="aa7a6-162">V souboru kódu na pozadí nastavíte minimální a maximální kalendářní data pro ovládací prvek DatePicker.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="aa7a6-163">Nastavte tyto hodnoty, bude uživatelům zabránit v přechodu pro neplatná data.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="aa7a6-164">Načte minimální a maximální hodnoty z **RangeAttribute** vlastnosti data a času, pokud je zadáno.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="aa7a6-165">Otevřete **data a času\_Edit.ascx.cs**a přidejte následující zvýrazněný kód na stránku\_Load – metoda.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="aa7a6-166">Spuštění webové aplikace a přejděte na stránku AddStudent.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="aa7a6-167">Zadejte hodnoty pro pole a Všimněte si, že když kliknete na textové pole pro datum registrace, kalendář je zobrazen.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![Výběr data](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="aa7a6-169">Vyberte datum a klikněte na **vložit**.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="aa7a6-170">RangeAttribute vynucuje ověření na serveru.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="aa7a6-171">Nastavením vlastnosti minDate ovládací prvek Datepicker, můžete také použít ověření na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="aa7a6-172">Kalendáře neumožňuje uživateli, přejděte na datum před hodnotu minDate.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="aa7a6-173">Pokud chcete upravit záznam v zobrazení mřížky, zobrazí se také kalendáře.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![Ovládací prvek DatePicker v GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="aa7a6-175">Závěr</span><span class="sxs-lookup"><span data-stu-id="aa7a6-175">Conclusion</span></span>

<span data-ttu-id="aa7a6-176">V tomto kurzu jste zjistili, jak začlenit JQuery widget do webové formuláře, který používá vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="aa7a6-177">V dalším [kurzu](using-query-string-values-to-retrieve-data.md), při výběru dat použijete hodnotu řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="aa7a6-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aa7a6-178">[Předchozí](sorting-paging-and-filtering-data.md)
> [další](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="aa7a6-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
