---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrace s vazby modelu a webových formulářů JQuery UI Datepicker | Dokumentace Microsoftu
author: tfitzmac
description: V této sérii kurzů ukazuje základní aspekty v použití vazby modelu s projektem aplikace webových formulářů ASP.NET. Data interakce díky vazby modelu další přímo-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 81cc5f010f2c6c7707223679717e34108a5a7aa3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753020"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="74fbd-104">Integrace prvku Datepicker uživatelského rozhraní JQuery s vazby modelu a webové formuláře</span><span class="sxs-lookup"><span data-stu-id="74fbd-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="74fbd-105">podle [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="74fbd-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="74fbd-106">V této sérii kurzů ukazuje základní aspekty v použití vazby modelu s projektem aplikace webových formulářů ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="74fbd-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="74fbd-107">Vazby modelu díky dat interakce více přímočaré než pracující s daty objektů zdroje (například ObjectDataSource nebo SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="74fbd-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="74fbd-108">Tato série začíná úvodní materiály a přesune pokročilejších pojmech v budoucích kurzech.</span><span class="sxs-lookup"><span data-stu-id="74fbd-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="74fbd-109">Tento kurz ukazuje, jak přidat uživatelské rozhraní JQuery [Datepicker widgetu](http://jqueryui.com/datepicker/) webový formulář a vazba k aktualizaci databáze s vybranou hodnotu použití modelu.</span><span class="sxs-lookup"><span data-stu-id="74fbd-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="74fbd-110">V tomto kurzu vychází z vytvořeného v projektu [první](retrieving-data.md) a [druhý](updating-deleting-and-creating-data.md) části této série.</span><span class="sxs-lookup"><span data-stu-id="74fbd-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="74fbd-111">Je možné [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=286116) dokončený projekt v jazyce C# nebo VB.</span><span class="sxs-lookup"><span data-stu-id="74fbd-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="74fbd-112">Ke stažení kódu funguje pomocí sady Visual Studio 2012 nebo Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="74fbd-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="74fbd-113">Používá šablonu Visual Studio 2012, která se trochu liší od sady Visual Studio 2013 šablonu uvedenou v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="74fbd-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="74fbd-114">Co budete vytvářet</span><span class="sxs-lookup"><span data-stu-id="74fbd-114">What you'll build</span></span>

<span data-ttu-id="74fbd-115">V tomto kurzu budete:</span><span class="sxs-lookup"><span data-stu-id="74fbd-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="74fbd-116">Přidání vlastnosti do modelu k zaznamenání student získal datum registrace</span><span class="sxs-lookup"><span data-stu-id="74fbd-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="74fbd-117">Povolit uživateli vybrat datum registrace pomocí widgetu JQuery UI Datepicker</span><span class="sxs-lookup"><span data-stu-id="74fbd-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="74fbd-118">Vynutit ověřovacích pravidel pro datum registrace</span><span class="sxs-lookup"><span data-stu-id="74fbd-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="74fbd-119">Ve widgetu JQuery UI Datepicker umožňuje uživatelům snadno vybrat datum z kalendáře, která se zobrazí, když uživatel pracuje s polem.</span><span class="sxs-lookup"><span data-stu-id="74fbd-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="74fbd-120">Pomocí tohoto widgetu můžete být uživatelům pohodlnější než ručním zadáním datum.</span><span class="sxs-lookup"><span data-stu-id="74fbd-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="74fbd-121">Integrace stránku, která používá vazbu modelu pro operace s daty v ovládacím prvku Datepicker vyžaduje jenom malé množství další práce.</span><span class="sxs-lookup"><span data-stu-id="74fbd-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="74fbd-122">Přidat nové vlastnosti do modelu</span><span class="sxs-lookup"><span data-stu-id="74fbd-122">Add a new property to the model</span></span>

<span data-ttu-id="74fbd-123">Nejprve přidejte **data a času** vlastností pro své studenty pro model a migrovat tuto změnu do databáze.</span><span class="sxs-lookup"><span data-stu-id="74fbd-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="74fbd-124">Otevřít **UniversityModels.cs**a přidat zvýrazněný kód do modelu studentů.</span><span class="sxs-lookup"><span data-stu-id="74fbd-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="74fbd-125">**RangeAttribute** je zahrnuta k vynucení ověřovacích pravidel pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="74fbd-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="74fbd-126">Pro účely tohoto kurzu budeme předpokládat, že Contoso univerzitě byla založena na 1. lednu 2013, a proto dřívější registrace data nejsou platná.</span><span class="sxs-lookup"><span data-stu-id="74fbd-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="74fbd-127">V okně Správa balíčků přidejte migraci spuštěním příkazu **přidat migrace AddEnrollmentDate**.</span><span class="sxs-lookup"><span data-stu-id="74fbd-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="74fbd-128">Všimněte si, že migrace kódu přidá nový sloupec Datum a čas do tabulky studentů.</span><span class="sxs-lookup"><span data-stu-id="74fbd-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="74fbd-129">Tak, aby odpovídaly hodnota, kterou jste zadali v RangeAttribute, přidejte výchozí hodnotu pro nový sloupec, jak je znázorněno v následující zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="74fbd-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="74fbd-130">Uložte změny do souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="74fbd-130">Save your change to the migration file.</span></span>

<span data-ttu-id="74fbd-131">Nemusíte znovu počáteční hodnoty data.</span><span class="sxs-lookup"><span data-stu-id="74fbd-131">You do not need to seed the data again.</span></span> <span data-ttu-id="74fbd-132">Proto otevřete **Configuration.cs** ve složce migrace a odstranit nebo okomentovat kód v **počáteční hodnoty** metody.</span><span class="sxs-lookup"><span data-stu-id="74fbd-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="74fbd-133">Soubor uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="74fbd-133">Save and close the file.</span></span>

<span data-ttu-id="74fbd-134">Nyní, spusťte příkaz **aktualizace databáze**.</span><span class="sxs-lookup"><span data-stu-id="74fbd-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="74fbd-135">Všimněte si, že sloupec nyní existuje v databázi a všechny existující záznamy mají výchozí hodnotu pro EnrollmentDate.</span><span class="sxs-lookup"><span data-stu-id="74fbd-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="74fbd-136">Přidat dynamické ovládací prvky pro datum registrace</span><span class="sxs-lookup"><span data-stu-id="74fbd-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="74fbd-137">Nyní přidáte ovládací prvky pro zobrazení a úpravy datum registrace.</span><span class="sxs-lookup"><span data-stu-id="74fbd-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="74fbd-138">V tomto okamžiku je hodnota upravovat pomocí textového pole.</span><span class="sxs-lookup"><span data-stu-id="74fbd-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="74fbd-139">V pozdější části kurzu změníte do textového pole na ovládacím prvku JQuery Datepicker.</span><span class="sxs-lookup"><span data-stu-id="74fbd-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="74fbd-140">Za prvé, je důležité si uvědomit, že není potřeba provádět změny na **AddStudent.aspx** souboru.</span><span class="sxs-lookup"><span data-stu-id="74fbd-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="74fbd-141">Ovládací prvek DynamicEntity se automaticky generují nové vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="74fbd-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="74fbd-142">Otevřít **Students.aspx**a přidejte následující zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="74fbd-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="74fbd-143">Spusťte aplikaci a Všimněte si, že hodnota datum registrace můžete nastavit tak, že zadáte datum.</span><span class="sxs-lookup"><span data-stu-id="74fbd-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="74fbd-144">Při přidání nového studenta:</span><span class="sxs-lookup"><span data-stu-id="74fbd-144">When adding a new student:</span></span>

![nastavit datum](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="74fbd-146">Nebo úpravu existující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="74fbd-146">Or, editing an existing value:</span></span>

![Upravit datum](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="74fbd-148">Zadáte datum funguje, ale nemusí být uživatelské prostředí, které chcete poskytnout.</span><span class="sxs-lookup"><span data-stu-id="74fbd-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="74fbd-149">V další části vám umožní výběr data prostřednictvím kalendáře.</span><span class="sxs-lookup"><span data-stu-id="74fbd-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="74fbd-150">Nainstalujte balíček NuGet pro práci s uživatelským rozhraním JQuery</span><span class="sxs-lookup"><span data-stu-id="74fbd-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="74fbd-151">**Uživatelského rozhraní džusu** balíček NuGet umožňuje snadnou integraci widgety uživatelského rozhraní JQuery do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="74fbd-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="74fbd-152">Pokud chcete použít tento balíček, ji nainstalujte prostřednictvím balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="74fbd-152">To use this package, install it through NuGet.</span></span>

![Přidat džusu uživatelského rozhraní](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="74fbd-154">Verze džusu uživatelského rozhraní, který nainstalujete dojít ke konfliktu s verzí JQuery ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="74fbd-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="74fbd-155">Než budete pokračovat s tímto kurzem, spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="74fbd-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="74fbd-156">Pokud dojde k chybě jazyka JavaScript, budete muset sjednotit JQuery verze.</span><span class="sxs-lookup"><span data-stu-id="74fbd-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="74fbd-157">Můžete přidat do složky skriptů (verze 1.8.2 v době psaní tohoto kurzu) očekávaná verze JQuery nebo v Site.master zadejte cestu k souboru JQuery.</span><span class="sxs-lookup"><span data-stu-id="74fbd-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="74fbd-158">Přizpůsobení šablony data a času zahrnout Datepicker widgetu</span><span class="sxs-lookup"><span data-stu-id="74fbd-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="74fbd-159">Přidejte ovládací prvek Datepicker widgetu do šablony dynamických dat pro hodnotu data a času úpravy.</span><span class="sxs-lookup"><span data-stu-id="74fbd-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="74fbd-160">Do šablony přidáte widgetu, automaticky je vykreslen ve formuláři pro přidání nového studenta a v mřížkovém zobrazení pro úpravy studenty.</span><span class="sxs-lookup"><span data-stu-id="74fbd-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="74fbd-161">Otevřít **data a času\_Edit.ascx**a přidejte následující zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="74fbd-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="74fbd-162">V souboru kódu na pozadí nastavíte minimální a maximální datum pro ovládací prvek DatePicker.</span><span class="sxs-lookup"><span data-stu-id="74fbd-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="74fbd-163">Nastavením těchto hodnot se uživatelům zabránit v přechodu na neplatná data.</span><span class="sxs-lookup"><span data-stu-id="74fbd-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="74fbd-164">Načte minimální a maximální hodnoty z **RangeAttribute** u vlastnosti data a času, pokud je zadáno.</span><span class="sxs-lookup"><span data-stu-id="74fbd-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="74fbd-165">Otevřít **data a času\_Edit.ascx.cs**a přidejte následující zvýrazněný kód na stránku\_načíst metodu.</span><span class="sxs-lookup"><span data-stu-id="74fbd-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="74fbd-166">Spusťte webovou aplikaci a přejděte na stránku AddStudent.</span><span class="sxs-lookup"><span data-stu-id="74fbd-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="74fbd-167">Zadejte hodnoty pro pole a Všimněte si, že po kliknutí na textové pole pro datum registrace se zobrazí v kalendáři.</span><span class="sxs-lookup"><span data-stu-id="74fbd-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![Výběr data](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="74fbd-169">Vyberte datum a klikněte na tlačítko **vložit**.</span><span class="sxs-lookup"><span data-stu-id="74fbd-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="74fbd-170">RangeAttribute vynucuje ověření na serveru.</span><span class="sxs-lookup"><span data-stu-id="74fbd-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="74fbd-171">Nastavením vlastnosti minDate na ovládací prvek Datepicker můžete také použít ověření na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="74fbd-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="74fbd-172">Kalendář, nebude uživatel přejít na datum před hodnotu minDate.</span><span class="sxs-lookup"><span data-stu-id="74fbd-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="74fbd-173">Při úpravě záznamu v zobrazení mřížky se zobrazí také v kalendáři.</span><span class="sxs-lookup"><span data-stu-id="74fbd-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![Ovládací prvek DatePicker v prvku GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="74fbd-175">Závěr</span><span class="sxs-lookup"><span data-stu-id="74fbd-175">Conclusion</span></span>

<span data-ttu-id="74fbd-176">V tomto kurzu jste zjistili, jak začlenit JQuery widgetu do webového formuláře, který používá vazbu modelu.</span><span class="sxs-lookup"><span data-stu-id="74fbd-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="74fbd-177">V dalším [kurzu](using-query-string-values-to-retrieve-data.md), při výběru dat použijete hodnotu řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="74fbd-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="74fbd-178">[Předchozí](sorting-paging-and-filtering-data.md)
> [další](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="74fbd-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
