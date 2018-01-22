---
title: "Nahrávání souborů na stránku Razor v ASP.NET Core"
author: guardrex
description: "Zjistěte, jak k nahrání souborů do stránky Razor."
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: a29043ac5d28ae8efcbbd6919e73e3774ba22699
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="34d5e-103">Nahrávání souborů na stránku Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34d5e-103">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="34d5e-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="34d5e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="34d5e-105">V této části je znázorněn nahrávání souborů stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="34d5e-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="34d5e-106">[Film stránky Razor ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) v tento kurz používá jednoduchý model vazby k nahrání souborů, které funguje dobře pro nahrávání souborů malé.</span><span class="sxs-lookup"><span data-stu-id="34d5e-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="34d5e-107">Informace o streamování velkých souborů, v tématu [nahrávání velkých souborů s streamování](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="34d5e-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="34d5e-108">V následujících krocích přidáte funkci nahrávání souboru plán film ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="34d5e-108">In the steps below, you add a movie schedule file upload feature to the sample app.</span></span> <span data-ttu-id="34d5e-109">Je reprezentována plán film `Schedule` třídy.</span><span class="sxs-lookup"><span data-stu-id="34d5e-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="34d5e-110">Třída zahrnuje dvě verze plánu.</span><span class="sxs-lookup"><span data-stu-id="34d5e-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="34d5e-111">Jedna verze je poskytováno zákazníkům, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="34d5e-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="34d5e-112">Jiné verze se má použít pro zaměstnance společnosti `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="34d5e-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="34d5e-113">Každá verze je uloženo jako samostatný soubor.</span><span class="sxs-lookup"><span data-stu-id="34d5e-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="34d5e-114">Tento kurz ukazuje, jak provést dvě nahrávání souborů ze stránky s jediným POŠTOVNÍM k serveru.</span><span class="sxs-lookup"><span data-stu-id="34d5e-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="34d5e-115">Přidání třídy odesílání souborů při odpovědích</span><span class="sxs-lookup"><span data-stu-id="34d5e-115">Add a FileUpload class</span></span>

<span data-ttu-id="34d5e-116">Níže vytvoříte stránky Razor pro zpracování pár nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="34d5e-116">Below, you create a Razor page to handle a pair of file uploads.</span></span> <span data-ttu-id="34d5e-117">Přidat `FileUpload` třídy, která je vázána na stránku získat data plánu.</span><span class="sxs-lookup"><span data-stu-id="34d5e-117">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="34d5e-118">Klikněte pravým tlačítkem *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="34d5e-118">Right click the *Models* folder.</span></span> <span data-ttu-id="34d5e-119">Vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="34d5e-119">Select **Add** > **Class**.</span></span> <span data-ttu-id="34d5e-120">Název třídy **odesílání souborů při odpovědích** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="34d5e-120">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="34d5e-121">Třída nemá vlastnost pro nadpis podle plánu a vlastnost pro každou z dvou verzích plán.</span><span class="sxs-lookup"><span data-stu-id="34d5e-121">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="34d5e-122">Všechny tři vlastnosti jsou požadovány a název musí mít 3 až 60 znaků.</span><span class="sxs-lookup"><span data-stu-id="34d5e-122">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="34d5e-123">Přidejte pomocnou metodu k nahrání souborů</span><span class="sxs-lookup"><span data-stu-id="34d5e-123">Add a helper method to upload files</span></span>

<span data-ttu-id="34d5e-124">Nechcete duplicity kód pro zpracování souborů nahrané plán, přidejte nejprve statickou pomocnou metodu.</span><span class="sxs-lookup"><span data-stu-id="34d5e-124">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="34d5e-125">Vytvoření *nástroje* složky v aplikaci a přidejte *FileHelpers.cs* soubor s následujícím obsahem.</span><span class="sxs-lookup"><span data-stu-id="34d5e-125">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="34d5e-126">Pomocná metoda `ProcessFormFile`, trvá [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) a [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) a vrátí řetězec obsahující velikost souboru a jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="34d5e-126">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="34d5e-127">Se kontroluje, typu obsahu a délka.</span><span class="sxs-lookup"><span data-stu-id="34d5e-127">The content type and length are checked.</span></span> <span data-ttu-id="34d5e-128">Pokud soubor neprojde ověřování pravosti, chyba je přidán do `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="34d5e-128">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a><span data-ttu-id="34d5e-129">Přidání třídy plán</span><span class="sxs-lookup"><span data-stu-id="34d5e-129">Add the Schedule class</span></span>

<span data-ttu-id="34d5e-130">Klikněte pravým tlačítkem *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="34d5e-130">Right click the *Models* folder.</span></span> <span data-ttu-id="34d5e-131">Vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="34d5e-131">Select **Add** > **Class**.</span></span> <span data-ttu-id="34d5e-132">Název třídy **plán** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="34d5e-132">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="34d5e-133">Používá třída `Display` a `DisplayFormat` atributy, které vytvářejí popisné názvy a formátování při vykreslení data plánu.</span><span class="sxs-lookup"><span data-stu-id="34d5e-133">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="34d5e-134">Aktualizace MovieContext</span><span class="sxs-lookup"><span data-stu-id="34d5e-134">Update the MovieContext</span></span>

<span data-ttu-id="34d5e-135">Zadejte `DbSet` v `MovieContext` (*Models/MovieContext.cs*) pro plány:</span><span class="sxs-lookup"><span data-stu-id="34d5e-135">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="34d5e-136">Přidat do databáze v tabulce plán</span><span class="sxs-lookup"><span data-stu-id="34d5e-136">Add the Schedule table to the database</span></span>

<span data-ttu-id="34d5e-137">Otevřete konzolu Správce balíčků (pomocí PMC): **nástroje** > **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="34d5e-137">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Pomocí PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="34d5e-139">V pomocí PMC spuštěním následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="34d5e-139">In the PMC, execute the following commands.</span></span> <span data-ttu-id="34d5e-140">Přidat tyto příkazy `Schedule` tabulky k databázi:</span><span class="sxs-lookup"><span data-stu-id="34d5e-140">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="34d5e-141">Přidat nahrání souboru stránky Razor</span><span class="sxs-lookup"><span data-stu-id="34d5e-141">Add a file upload Razor Page</span></span>

<span data-ttu-id="34d5e-142">V *stránky* složky, vytvoření *plány* složky.</span><span class="sxs-lookup"><span data-stu-id="34d5e-142">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="34d5e-143">V *plány* složku vytvořit stránku s názvem *Index.cshtml* pro nahrávání plán s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="34d5e-143">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="34d5e-144">Každá skupina formuláře zahrnuje  **\<popisek >** který zobrazí název každé vlastnosti třídy.</span><span class="sxs-lookup"><span data-stu-id="34d5e-144">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="34d5e-145">`Display` Atributy v `FileUpload` model poskytují zobrazovaných hodnot pro popisky.</span><span class="sxs-lookup"><span data-stu-id="34d5e-145">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="34d5e-146">Například `UploadPublicSchedule` zobrazovaný název vlastnosti je nastaven s `[Display(Name="Public Schedule")]` a proto zobrazí "Veřejné plán" v popisku, když se vykreslí formulář.</span><span class="sxs-lookup"><span data-stu-id="34d5e-146">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="34d5e-147">Každá skupina formuláře zahrnuje ověřování  **\<span >**.</span><span class="sxs-lookup"><span data-stu-id="34d5e-147">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="34d5e-148">Uživatelský vstup nesplňuje-li vlastnost atributy nastavit v `FileUpload` třídy nebo pokud platí jedna z `ProcessFormFile` metoda souboru ověřování selže, model se nepodaří ověřit.</span><span class="sxs-lookup"><span data-stu-id="34d5e-148">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="34d5e-149">Pokud selže ověření modelu, je generován zprávu užitečné ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="34d5e-149">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="34d5e-150">Například `Title` vlastnost je opatřen poznámkou `[Required]` a `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="34d5e-150">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="34d5e-151">Pokud se uživateli nepodaří zadat název, obdrží zprávu s upozorněním, že je vyžadována hodnota.</span><span class="sxs-lookup"><span data-stu-id="34d5e-151">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="34d5e-152">Pokud uživatel zadá hodnotu menší než 3 znaky nebo víc než 60 znaků, obdrží zprávu s upozorněním, že hodnota má nesprávnou délku.</span><span class="sxs-lookup"><span data-stu-id="34d5e-152">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="34d5e-153">Soubor je zadaný, který nemá žádný obsah, zobrazí se zpráva označující, že soubor je prázdný.</span><span class="sxs-lookup"><span data-stu-id="34d5e-153">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-code-behind-file"></a><span data-ttu-id="34d5e-154">Přidejte soubor kódu</span><span class="sxs-lookup"><span data-stu-id="34d5e-154">Add the code-behind file</span></span>

<span data-ttu-id="34d5e-155">Přidejte soubor kódu (*Index.cshtml.cs*) k *plány* složky:</span><span class="sxs-lookup"><span data-stu-id="34d5e-155">Add the code-behind file (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="34d5e-156">Model stránky (`IndexModel` v *Index.cshtml.cs*) váže `FileUpload` třídy:</span><span class="sxs-lookup"><span data-stu-id="34d5e-156">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="34d5e-157">Model také používá seznam plány (`IList<Schedule>`) k zobrazení plány, které jsou uloženy v databázi na stránce:</span><span class="sxs-lookup"><span data-stu-id="34d5e-157">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="34d5e-158">Při načtení stránky s `OnGetAsync`, `Schedules` je naplněny z databáze a používat ke generování tabulky HTML z načíst plánů:</span><span class="sxs-lookup"><span data-stu-id="34d5e-158">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="34d5e-159">Při odeslání formuláře na server, `ModelState` je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="34d5e-159">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="34d5e-160">Pokud je neplatná, `Schedule` znovu sestaven, a vykreslí stránku s jeden nebo více ověřovacích zpráv s informacemi o tom, proč stránky ověření se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="34d5e-160">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="34d5e-161">Pokud je platná, `FileUpload` vlastnosti se používají v *OnPostAsync* k dokončení nahrávání souborů pro dvě verze plánu a vytvořit nový `Schedule` objekt, který chcete uložit data.</span><span class="sxs-lookup"><span data-stu-id="34d5e-161">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="34d5e-162">Plán pak je uložena do databáze:</span><span class="sxs-lookup"><span data-stu-id="34d5e-162">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="34d5e-163">Odkaz nahrávání souborů stránky Razor</span><span class="sxs-lookup"><span data-stu-id="34d5e-163">Link the file upload Razor Page</span></span>

<span data-ttu-id="34d5e-164">Otevřete *_Layout.cshtml* a přidat odkaz na navigačním panelu k dosažení stránka nahrávání souboru:</span><span class="sxs-lookup"><span data-stu-id="34d5e-164">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="34d5e-165">Přidat stránku potvrďte odstranění plánu</span><span class="sxs-lookup"><span data-stu-id="34d5e-165">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="34d5e-166">Když uživatel klikne na Odstranit plán, kterým chcete šance, že na tlačítko Storno.</span><span class="sxs-lookup"><span data-stu-id="34d5e-166">When the user clicks to delete a schedule, you want them to have a chance to cancel the operation.</span></span> <span data-ttu-id="34d5e-167">Přidat stránku potvrzení odstranění (*Delete.cshtml*) k *plány* složky:</span><span class="sxs-lookup"><span data-stu-id="34d5e-167">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="34d5e-168">Souboru kódu na pozadí (*Delete.cshtml.cs*) načte jeden plán identifikovaný `id` v datech trasy žádosti.</span><span class="sxs-lookup"><span data-stu-id="34d5e-168">The code-behind file (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="34d5e-169">Přidat *Delete.cshtml.cs* do souboru *plány* složky:</span><span class="sxs-lookup"><span data-stu-id="34d5e-169">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="34d5e-170">`OnPostAsync` Metoda zpracovává odstranění plán, podle jeho `id`:</span><span class="sxs-lookup"><span data-stu-id="34d5e-170">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="34d5e-171">Po úspěšném odstranění plán, `RedirectToPage` odešle uživateli zpět na plány *Index.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="34d5e-171">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="34d5e-172">Funkční stránky Razor plány</span><span class="sxs-lookup"><span data-stu-id="34d5e-172">The working Schedules Razor Page</span></span>

<span data-ttu-id="34d5e-173">Při načtení stránky, popisky a vstupy pro plán název, plán veřejné a privátní plán se vykreslují tlačítka Odeslat:</span><span class="sxs-lookup"><span data-stu-id="34d5e-173">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Plány stránky Razor, jak je vidět na počáteční zatížení bez chyby ověření a prázdné pole](uploading-files/_static/browser1.png)

<span data-ttu-id="34d5e-175">Výběr **nahrát** tlačítko bez naplnění žádná pole je v rozporu `[Required]` atributů v modelu.</span><span class="sxs-lookup"><span data-stu-id="34d5e-175">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="34d5e-176">`ModelState` Je neplatný.</span><span class="sxs-lookup"><span data-stu-id="34d5e-176">The `ModelState` is invalid.</span></span> <span data-ttu-id="34d5e-177">Uživateli se zobrazí chybové zprávy ověření:</span><span class="sxs-lookup"><span data-stu-id="34d5e-177">The validation error messages are displayed to the user:</span></span>

![Ověření chybové zprávy zobrazují vedle každého vstupního ovládacího prvku](uploading-files/_static/browser2.png)

<span data-ttu-id="34d5e-179">Zadejte dvě písmena do **název** pole.</span><span class="sxs-lookup"><span data-stu-id="34d5e-179">Type two letters into the **Title** field.</span></span> <span data-ttu-id="34d5e-180">Ověření změny zpráva indikující, že název musí být 3 až 60 znaků:</span><span class="sxs-lookup"><span data-stu-id="34d5e-180">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Změnit název ověřovací zprávy](uploading-files/_static/browser3.png)

<span data-ttu-id="34d5e-182">Pokud jeden nebo více plánů se odešlou, **načíst plány** načíst plány vykreslí část:</span><span class="sxs-lookup"><span data-stu-id="34d5e-182">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabulka načíst plány, zobrazuje název každý plán nahrán datum UTC, veřejné verze velikost souboru a velikost souboru privátní verze](uploading-files/_static/browser4.png)

<span data-ttu-id="34d5e-184">Můžete kliknout na uživatele **odstranit** odkaz z ní k dosažení zobrazení potvrzení odstranění, které mají příležitost potvrdit nebo zrušit operaci odstranění.</span><span class="sxs-lookup"><span data-stu-id="34d5e-184">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="34d5e-185">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="34d5e-185">Troubleshooting</span></span>

<span data-ttu-id="34d5e-186">Pro řešení potíží s informací o s `IFormFile` nahrát, najdete v článku [nahrávání souborů v ASP.NET Core: řešení potíží s](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="34d5e-186">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="34d5e-187">Děkujeme, že dokončení tohoto úvodu do stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="34d5e-187">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="34d5e-188">Děkujeme za jakékoli komentáře, která zůstanou.</span><span class="sxs-lookup"><span data-stu-id="34d5e-188">We appreciate any comments you leave.</span></span> <span data-ttu-id="34d5e-189">[Začínáme s MVC a EF základní](xref:data/ef-mvc/intro) je vynikající postupujte podle kroků až tento kurz.</span><span class="sxs-lookup"><span data-stu-id="34d5e-189">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="34d5e-190">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="34d5e-190">Additional resources</span></span>

* [<span data-ttu-id="34d5e-191">Nahrávání souborů v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34d5e-191">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="34d5e-192">IFormFile</span><span class="sxs-lookup"><span data-stu-id="34d5e-192">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="34d5e-193">Předchozí: ověření</span><span class="sxs-lookup"><span data-stu-id="34d5e-193">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
