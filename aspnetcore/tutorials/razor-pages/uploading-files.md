---
title: "Nahrávání souborů na stránku Razor v ASP.NET Core"
author: guardrex
description: "Zjistěte, jak k nahrání souborů do stránky Razor."
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 2bd593b77c10b1b3ab0b73551d01abd0b4187b8d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="0f060-103">Nahrávání souborů na stránku Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f060-103">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="0f060-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0f060-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0f060-105">V této části je znázorněn nahrávání souborů stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="0f060-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="0f060-106">[Film stránky Razor ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) v tento kurz používá jednoduchý model vazby k nahrání souborů, které funguje dobře pro nahrávání souborů malé.</span><span class="sxs-lookup"><span data-stu-id="0f060-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="0f060-107">Informace o streamování velkých souborů, v tématu [nahrávání velkých souborů s streamování](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="0f060-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="0f060-108">V následujících krocích se funkce nahrávání souboru plán film přidá do ukázková aplikace.</span><span class="sxs-lookup"><span data-stu-id="0f060-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="0f060-109">Je reprezentována plán film `Schedule` třídy.</span><span class="sxs-lookup"><span data-stu-id="0f060-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="0f060-110">Třída zahrnuje dvě verze plánu.</span><span class="sxs-lookup"><span data-stu-id="0f060-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="0f060-111">Jedna verze je poskytováno zákazníkům, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="0f060-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="0f060-112">Jiné verze se má použít pro zaměstnance společnosti `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="0f060-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="0f060-113">Každá verze je uloženo jako samostatný soubor.</span><span class="sxs-lookup"><span data-stu-id="0f060-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="0f060-114">Tento kurz ukazuje, jak provést dvě nahrávání souborů ze stránky s jediným POŠTOVNÍM k serveru.</span><span class="sxs-lookup"><span data-stu-id="0f060-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="0f060-115">Aspekty zabezpečení</span><span class="sxs-lookup"><span data-stu-id="0f060-115">Security considerations</span></span>

<span data-ttu-id="0f060-116">Upozornění: musí být provedeny, když nabízí uživatelům možnost k nahrání souborů do serveru.</span><span class="sxs-lookup"><span data-stu-id="0f060-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="0f060-117">Útočník může spustit [útok DoS](/windows-hardware/drivers/ifs/denial-of-service) a jiným útokům na systém.</span><span class="sxs-lookup"><span data-stu-id="0f060-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="0f060-118">Některé kroky zabezpečení, které sníží pravděpodobnost úspěšného útoku jsou:</span><span class="sxs-lookup"><span data-stu-id="0f060-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="0f060-119">Nahrání souborů do oblasti nahrávání souboru vyhrazené systému, takže je jednodušší uložit opatření zabezpečení nahraná obsahu.</span><span class="sxs-lookup"><span data-stu-id="0f060-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="0f060-120">Když umožňující nahrávání souborů, ujistěte se, oprávnění ke spouštění jsou zakázány na umístění nahrávání.</span><span class="sxs-lookup"><span data-stu-id="0f060-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="0f060-121">Použijte soubor bezpečný název určit aplikace, nikoli z vstup uživatele nebo název souboru nahrávaný soubor.</span><span class="sxs-lookup"><span data-stu-id="0f060-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="0f060-122">Povolte jenom konkrétní sadu schválený přípon.</span><span class="sxs-lookup"><span data-stu-id="0f060-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="0f060-123">Ověřte, že kontrola klienta se provádí na serveru.</span><span class="sxs-lookup"><span data-stu-id="0f060-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="0f060-124">Kontrola na straně klienta jsou snadno obejít.</span><span class="sxs-lookup"><span data-stu-id="0f060-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="0f060-125">Zkontrolujte velikost nahrávání a zabránit nahrávání větší, než se očekávalo.</span><span class="sxs-lookup"><span data-stu-id="0f060-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="0f060-126">Spusťte vyhledávání virů nebo malwaru v nahraném obsah.</span><span class="sxs-lookup"><span data-stu-id="0f060-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="0f060-127">Odesílání škodlivý kód do systému je často prvním krokem k provádění kód, který můžete:</span><span class="sxs-lookup"><span data-stu-id="0f060-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="0f060-128">Úplné převzetí a systému.</span><span class="sxs-lookup"><span data-stu-id="0f060-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="0f060-129">Přetížení systému se výsledek, který systému úplně selže.</span><span class="sxs-lookup"><span data-stu-id="0f060-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="0f060-130">Ohrožení dat uživatele nebo systému.</span><span class="sxs-lookup"><span data-stu-id="0f060-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="0f060-131">Graffiti se vztahují na veřejné rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0f060-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="0f060-132">Přidání třídy odesílání souborů při odpovědích</span><span class="sxs-lookup"><span data-stu-id="0f060-132">Add a FileUpload class</span></span>

<span data-ttu-id="0f060-133">Vytvoření stránky Razor pro zpracování pár nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="0f060-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="0f060-134">Přidat `FileUpload` třídy, která je vázána na stránku získat data plánu.</span><span class="sxs-lookup"><span data-stu-id="0f060-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="0f060-135">Klikněte pravým tlačítkem *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="0f060-135">Right click the *Models* folder.</span></span> <span data-ttu-id="0f060-136">Vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="0f060-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="0f060-137">Název třídy **odesílání souborů při odpovědích** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="0f060-137">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="0f060-138">Třída nemá vlastnost pro nadpis podle plánu a vlastnost pro každou z dvou verzích plán.</span><span class="sxs-lookup"><span data-stu-id="0f060-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="0f060-139">Všechny tři vlastnosti jsou požadovány a název musí mít 3 až 60 znaků.</span><span class="sxs-lookup"><span data-stu-id="0f060-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="0f060-140">Přidejte pomocnou metodu k nahrání souborů</span><span class="sxs-lookup"><span data-stu-id="0f060-140">Add a helper method to upload files</span></span>

<span data-ttu-id="0f060-141">Nechcete duplicity kód pro zpracování souborů nahrané plán, přidejte nejprve statickou pomocnou metodu.</span><span class="sxs-lookup"><span data-stu-id="0f060-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="0f060-142">Vytvoření *nástroje* složky v aplikaci a přidejte *FileHelpers.cs* soubor s následujícím obsahem.</span><span class="sxs-lookup"><span data-stu-id="0f060-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="0f060-143">Pomocná metoda `ProcessFormFile`, trvá [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) a [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) a vrátí řetězec obsahující velikost souboru a jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="0f060-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="0f060-144">Se kontroluje, typu obsahu a délka.</span><span class="sxs-lookup"><span data-stu-id="0f060-144">The content type and length are checked.</span></span> <span data-ttu-id="0f060-145">Pokud soubor neprojde ověřování pravosti, chyba je přidán do `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="0f060-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a><span data-ttu-id="0f060-146">Uložte soubor na disku</span><span class="sxs-lookup"><span data-stu-id="0f060-146">Save the file to disk</span></span>

<span data-ttu-id="0f060-147">Ukázková aplikace uloží obsah souboru do pole databáze.</span><span class="sxs-lookup"><span data-stu-id="0f060-147">The sample app saves the file's content into a database field.</span></span> <span data-ttu-id="0f060-148">Pokud chcete uložit obsah souboru na disk, použijte [FileStream](/dotnet/api/system.io.filestream):</span><span class="sxs-lookup"><span data-stu-id="0f060-148">To save the file's content to disk, use a [FileStream](/dotnet/api/system.io.filestream):</span></span>

```csharp
using (var fileStream = new FileStream(filePath, FileMode.Create))
{
    await formFile.CopyToAsync(fileStream);
}
```

<span data-ttu-id="0f060-149">Pracovní proces musí mít oprávnění k zápisu do určeného umístění `filePath`.</span><span class="sxs-lookup"><span data-stu-id="0f060-149">The worker process must have write permissions to the location specified by `filePath`.</span></span>

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="0f060-150">Uložte soubor do Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="0f060-150">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="0f060-151">Pokud chcete nahrát obsah souboru do Azure Blob Storage, najdete v části [Začínáme s Azure Blob Storage pomocí rozhraní .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="0f060-151">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="0f060-152">Téma ukazuje, jak používat [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) Uložit [FileStream](/dotnet/api/system.io.filestream) do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="0f060-152">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="0f060-153">Přidání třídy plán</span><span class="sxs-lookup"><span data-stu-id="0f060-153">Add the Schedule class</span></span>

<span data-ttu-id="0f060-154">Klikněte pravým tlačítkem *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="0f060-154">Right click the *Models* folder.</span></span> <span data-ttu-id="0f060-155">Vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="0f060-155">Select **Add** > **Class**.</span></span> <span data-ttu-id="0f060-156">Název třídy **plán** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="0f060-156">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="0f060-157">Používá třída `Display` a `DisplayFormat` atributy, které vytvářejí popisné názvy a formátování při vykreslení data plánu.</span><span class="sxs-lookup"><span data-stu-id="0f060-157">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="0f060-158">Aktualizace MovieContext</span><span class="sxs-lookup"><span data-stu-id="0f060-158">Update the MovieContext</span></span>

<span data-ttu-id="0f060-159">Zadejte `DbSet` v `MovieContext` (*Models/MovieContext.cs*) pro plány:</span><span class="sxs-lookup"><span data-stu-id="0f060-159">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="0f060-160">Přidat do databáze v tabulce plán</span><span class="sxs-lookup"><span data-stu-id="0f060-160">Add the Schedule table to the database</span></span>

<span data-ttu-id="0f060-161">Otevřete konzolu Správce balíčků (pomocí PMC): **nástroje** > **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="0f060-161">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Pomocí PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="0f060-163">V pomocí PMC spuštěním následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="0f060-163">In the PMC, execute the following commands.</span></span> <span data-ttu-id="0f060-164">Přidat tyto příkazy `Schedule` tabulky k databázi:</span><span class="sxs-lookup"><span data-stu-id="0f060-164">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="0f060-165">Přidat nahrání souboru stránky Razor</span><span class="sxs-lookup"><span data-stu-id="0f060-165">Add a file upload Razor Page</span></span>

<span data-ttu-id="0f060-166">V *stránky* složky, vytvoření *plány* složky.</span><span class="sxs-lookup"><span data-stu-id="0f060-166">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="0f060-167">V *plány* složku vytvořit stránku s názvem *Index.cshtml* pro nahrávání plán s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="0f060-167">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="0f060-168">Každá skupina formuláře zahrnuje  **\<popisek >** který zobrazí název každé vlastnosti třídy.</span><span class="sxs-lookup"><span data-stu-id="0f060-168">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="0f060-169">`Display` Atributy v `FileUpload` model poskytují zobrazovaných hodnot pro popisky.</span><span class="sxs-lookup"><span data-stu-id="0f060-169">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="0f060-170">Například `UploadPublicSchedule` zobrazovaný název vlastnosti je nastaven s `[Display(Name="Public Schedule")]` a proto zobrazí "Veřejné plán" v popisku, když se vykreslí formulář.</span><span class="sxs-lookup"><span data-stu-id="0f060-170">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="0f060-171">Každá skupina formuláře zahrnuje ověřování  **\<span >**.</span><span class="sxs-lookup"><span data-stu-id="0f060-171">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="0f060-172">Uživatelský vstup nesplňuje-li vlastnost atributy nastavit v `FileUpload` třídy nebo pokud platí jedna z `ProcessFormFile` metoda souboru ověřování selže, model se nepodaří ověřit.</span><span class="sxs-lookup"><span data-stu-id="0f060-172">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="0f060-173">Pokud selže ověření modelu, je generován zprávu užitečné ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="0f060-173">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="0f060-174">Například `Title` vlastnost je opatřen poznámkou `[Required]` a `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="0f060-174">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="0f060-175">Pokud se uživateli nepodaří zadat název, obdrží zprávu s upozorněním, že je vyžadována hodnota.</span><span class="sxs-lookup"><span data-stu-id="0f060-175">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="0f060-176">Pokud uživatel zadá hodnotu menší než 3 znaky nebo víc než 60 znaků, obdrží zprávu s upozorněním, že hodnota má nesprávnou délku.</span><span class="sxs-lookup"><span data-stu-id="0f060-176">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="0f060-177">Soubor je zadaný, který nemá žádný obsah, zobrazí se zpráva označující, že soubor je prázdný.</span><span class="sxs-lookup"><span data-stu-id="0f060-177">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="0f060-178">Přidání stránky modelu</span><span class="sxs-lookup"><span data-stu-id="0f060-178">Add the page model</span></span>

<span data-ttu-id="0f060-179">Přidání stránky modelu (*Index.cshtml.cs*) k *plány* složky:</span><span class="sxs-lookup"><span data-stu-id="0f060-179">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="0f060-180">Model stránky (`IndexModel` v *Index.cshtml.cs*) váže `FileUpload` třídy:</span><span class="sxs-lookup"><span data-stu-id="0f060-180">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="0f060-181">Model také používá seznam plány (`IList<Schedule>`) k zobrazení plány, které jsou uloženy v databázi na stránce:</span><span class="sxs-lookup"><span data-stu-id="0f060-181">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="0f060-182">Při načtení stránky s `OnGetAsync`, `Schedules` je naplněny z databáze a používat ke generování tabulky HTML z načíst plánů:</span><span class="sxs-lookup"><span data-stu-id="0f060-182">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="0f060-183">Při odeslání formuláře na server, `ModelState` je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="0f060-183">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="0f060-184">Pokud je neplatná, `Schedule` znovu sestaven, a vykreslí stránku s jeden nebo více ověřovacích zpráv s informacemi o tom, proč stránky ověření se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="0f060-184">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="0f060-185">Pokud je platná, `FileUpload` vlastnosti se používají v *OnPostAsync* k dokončení nahrávání souborů pro dvě verze plánu a vytvořit nový `Schedule` objekt, který chcete uložit data.</span><span class="sxs-lookup"><span data-stu-id="0f060-185">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="0f060-186">Plán pak je uložena do databáze:</span><span class="sxs-lookup"><span data-stu-id="0f060-186">The schedule is then saved to the database:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="0f060-187">Odkaz nahrávání souborů stránky Razor</span><span class="sxs-lookup"><span data-stu-id="0f060-187">Link the file upload Razor Page</span></span>

<span data-ttu-id="0f060-188">Otevřete *_Layout.cshtml* a přidat odkaz na navigačním panelu k dosažení stránka nahrávání souboru:</span><span class="sxs-lookup"><span data-stu-id="0f060-188">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="0f060-189">Přidat stránku potvrďte odstranění plánu</span><span class="sxs-lookup"><span data-stu-id="0f060-189">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="0f060-190">Když uživatel klikne na Odstranit plán, je k dispozici šanci na tlačítko Storno.</span><span class="sxs-lookup"><span data-stu-id="0f060-190">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="0f060-191">Přidat stránku potvrzení odstranění (*Delete.cshtml*) k *plány* složky:</span><span class="sxs-lookup"><span data-stu-id="0f060-191">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="0f060-192">Model stránky (*Delete.cshtml.cs*) načte jeden plán identifikovaný `id` v datech trasy žádosti.</span><span class="sxs-lookup"><span data-stu-id="0f060-192">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="0f060-193">Přidat *Delete.cshtml.cs* do souboru *plány* složky:</span><span class="sxs-lookup"><span data-stu-id="0f060-193">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="0f060-194">`OnPostAsync` Metoda zpracovává odstranění plán, podle jeho `id`:</span><span class="sxs-lookup"><span data-stu-id="0f060-194">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="0f060-195">Po úspěšném odstranění plán, `RedirectToPage` odešle uživateli zpět na plány *Index.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="0f060-195">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="0f060-196">Funkční stránky Razor plány</span><span class="sxs-lookup"><span data-stu-id="0f060-196">The working Schedules Razor Page</span></span>

<span data-ttu-id="0f060-197">Při načtení stránky, popisky a vstupy pro plán název, plán veřejné a privátní plán se vykreslují tlačítka Odeslat:</span><span class="sxs-lookup"><span data-stu-id="0f060-197">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Plány stránky Razor, jak je vidět na počáteční zatížení bez chyby ověření a prázdné pole](uploading-files/_static/browser1.png)

<span data-ttu-id="0f060-199">Výběr **nahrát** tlačítko bez naplnění žádná pole je v rozporu `[Required]` atributů v modelu.</span><span class="sxs-lookup"><span data-stu-id="0f060-199">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="0f060-200">`ModelState` Je neplatný.</span><span class="sxs-lookup"><span data-stu-id="0f060-200">The `ModelState` is invalid.</span></span> <span data-ttu-id="0f060-201">Uživateli se zobrazí chybové zprávy ověření:</span><span class="sxs-lookup"><span data-stu-id="0f060-201">The validation error messages are displayed to the user:</span></span>

![Ověření chybové zprávy zobrazují vedle každého vstupního ovládacího prvku](uploading-files/_static/browser2.png)

<span data-ttu-id="0f060-203">Zadejte dvě písmena do **název** pole.</span><span class="sxs-lookup"><span data-stu-id="0f060-203">Type two letters into the **Title** field.</span></span> <span data-ttu-id="0f060-204">Ověření změny zpráva indikující, že název musí být 3 až 60 znaků:</span><span class="sxs-lookup"><span data-stu-id="0f060-204">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Změnit název ověřovací zprávy](uploading-files/_static/browser3.png)

<span data-ttu-id="0f060-206">Pokud jeden nebo více plánů se odešlou, **načíst plány** načíst plány vykreslí část:</span><span class="sxs-lookup"><span data-stu-id="0f060-206">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabulka načíst plány, zobrazuje název každý plán nahrán datum UTC, veřejné verze velikost souboru a velikost souboru privátní verze](uploading-files/_static/browser4.png)

<span data-ttu-id="0f060-208">Můžete kliknout na uživatele **odstranit** odkaz z ní k dosažení zobrazení potvrzení odstranění, které mají příležitost potvrdit nebo zrušit operaci odstranění.</span><span class="sxs-lookup"><span data-stu-id="0f060-208">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="0f060-209">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="0f060-209">Troubleshooting</span></span>

<span data-ttu-id="0f060-210">Pro řešení potíží s informací o s `IFormFile` nahrát, najdete v článku [nahrávání souborů v ASP.NET Core: řešení potíží s](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="0f060-210">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="0f060-211">Děkujeme, že dokončení tohoto úvodu do stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="0f060-211">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="0f060-212">Děkujeme za zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="0f060-212">We appreciate feedback.</span></span> <span data-ttu-id="0f060-213">[Začínáme s MVC a EF základní](xref:data/ef-mvc/intro) je vynikající postupujte podle kroků až tento kurz.</span><span class="sxs-lookup"><span data-stu-id="0f060-213">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0f060-214">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0f060-214">Additional resources</span></span>

* [<span data-ttu-id="0f060-215">Nahrávání souborů v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f060-215">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="0f060-216">IFormFile</span><span class="sxs-lookup"><span data-stu-id="0f060-216">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="0f060-217">Předchozí: ověření</span><span class="sxs-lookup"><span data-stu-id="0f060-217">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
