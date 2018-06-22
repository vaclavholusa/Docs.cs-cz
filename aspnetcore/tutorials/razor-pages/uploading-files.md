---
title: Nahrání souborů do stránky Razor v ASP.NET Core
author: guardrex
description: Zjistěte, jak k nahrání souborů do stránky Razor.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/12/2017
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 43268e24b67279b57c990a6289922ae38d883221
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275954"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="0f0ba-103">Nahrání souborů do stránky Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f0ba-103">Upload files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="0f0ba-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0f0ba-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0f0ba-105">V této části je znázorněn nahrávání souborů stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="0f0ba-106">[Film stránky Razor ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) v tento kurz používá jednoduchý model vazby k nahrání souborů, které funguje dobře pro nahrávání souborů malé.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="0f0ba-107">Informace o streamování velkých souborů, v tématu [nahrávání velkých souborů s streamování](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="0f0ba-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="0f0ba-108">V následujících krocích se funkce nahrávání souboru plán film přidá do ukázková aplikace.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="0f0ba-109">Je reprezentována plán film `Schedule` třídy.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="0f0ba-110">Třída zahrnuje dvě verze plánu.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="0f0ba-111">Jedna verze je poskytováno zákazníkům, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="0f0ba-112">Jiné verze se má použít pro zaměstnance společnosti `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="0f0ba-113">Každá verze je uloženo jako samostatný soubor.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="0f0ba-114">Tento kurz ukazuje, jak provést dvě nahrávání souborů ze stránky s jediným POŠTOVNÍM k serveru.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="0f0ba-115">Aspekty zabezpečení</span><span class="sxs-lookup"><span data-stu-id="0f0ba-115">Security considerations</span></span>

<span data-ttu-id="0f0ba-116">Upozornění: musí být provedeny, když nabízí uživatelům možnost k nahrání souborů do serveru.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="0f0ba-117">Útočník může spustit [útok DoS](/windows-hardware/drivers/ifs/denial-of-service) a jiným útokům na systém.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="0f0ba-118">Některé kroky zabezpečení, které sníží pravděpodobnost úspěšného útoku jsou:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="0f0ba-119">Nahrání souborů do oblasti nahrávání souboru vyhrazené systému, takže je jednodušší uložit opatření zabezpečení nahraná obsahu.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="0f0ba-120">Když umožňující nahrávání souborů, ujistěte se, oprávnění ke spouštění jsou zakázány na umístění nahrávání.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="0f0ba-121">Použijte soubor bezpečný název určit aplikace, nikoli z vstup uživatele nebo název souboru nahrávaný soubor.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="0f0ba-122">Povolte jenom konkrétní sadu schválený přípon.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="0f0ba-123">Ověřte, že kontrola klienta se provádí na serveru.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="0f0ba-124">Kontrola na straně klienta jsou snadno obejít.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="0f0ba-125">Zkontrolujte velikost nahrávání a zabránit nahrávání větší, než se očekávalo.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="0f0ba-126">Spusťte vyhledávání virů nebo malwaru v nahraném obsah.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="0f0ba-127">Odesílání škodlivý kód do systému je často prvním krokem k provádění kód, který můžete:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="0f0ba-128">Úplné převzetí a systému.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="0f0ba-129">Přetížení systému se výsledek, který systému úplně selže.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="0f0ba-130">Ohrožení dat uživatele nebo systému.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="0f0ba-131">Graffiti se vztahují na veřejné rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="0f0ba-132">Přidání třídy odesílání souborů při odpovědích</span><span class="sxs-lookup"><span data-stu-id="0f0ba-132">Add a FileUpload class</span></span>

<span data-ttu-id="0f0ba-133">Vytvoření stránky Razor pro zpracování pár nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="0f0ba-134">Přidat `FileUpload` třídy, která je vázána na stránku získat data plánu.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="0f0ba-135">Klikněte pravým tlačítkem *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-135">Right click the *Models* folder.</span></span> <span data-ttu-id="0f0ba-136">Vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="0f0ba-137">Název třídy **odesílání souborů při odpovědích** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-137">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="0f0ba-138">Třída nemá vlastnost pro nadpis podle plánu a vlastnost pro každou z dvou verzích plán.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="0f0ba-139">Všechny tři vlastnosti jsou požadovány a název musí mít 3 až 60 znaků.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="0f0ba-140">Přidejte pomocnou metodu k nahrání souborů</span><span class="sxs-lookup"><span data-stu-id="0f0ba-140">Add a helper method to upload files</span></span>

<span data-ttu-id="0f0ba-141">Nechcete duplicity kód pro zpracování souborů nahrané plán, přidejte nejprve statickou pomocnou metodu.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="0f0ba-142">Vytvoření *nástroje* složky v aplikaci a přidejte *FileHelpers.cs* soubor s následujícím obsahem.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="0f0ba-143">Pomocná metoda `ProcessFormFile`, trvá [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) a [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) a vrátí řetězec obsahující velikost souboru a jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="0f0ba-144">Se kontroluje, typu obsahu a délka.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-144">The content type and length are checked.</span></span> <span data-ttu-id="0f0ba-145">Pokud soubor neprojde ověřování pravosti, chyba je přidán do `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a><span data-ttu-id="0f0ba-146">Uložte soubor na disku</span><span class="sxs-lookup"><span data-stu-id="0f0ba-146">Save the file to disk</span></span>

<span data-ttu-id="0f0ba-147">Ukázková aplikace uloží odeslané soubory do pole databáze.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-147">The sample app saves uploaded files into database fields.</span></span> <span data-ttu-id="0f0ba-148">Chcete-li uložit soubor na disku, použijte [FileStream](/dotnet/api/system.io.filestream).</span><span class="sxs-lookup"><span data-stu-id="0f0ba-148">To save a file to disk, use a [FileStream](/dotnet/api/system.io.filestream).</span></span> <span data-ttu-id="0f0ba-149">V následujícím příkladu se zkopíruje soubor držené `FileUpload.UploadPublicSchedule` k `FileStream` v `OnPostAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-149">The following example copies a file held by `FileUpload.UploadPublicSchedule` to a `FileStream` in an `OnPostAsync` method.</span></span> <span data-ttu-id="0f0ba-150">`FileStream` Zapíše soubor na disku `<PATH-AND-FILE-NAME>` poskytuje:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-150">The `FileStream` writes the file to disk at the `<PATH-AND-FILE-NAME>` provided:</span></span>

```csharp
public async Task<IActionResult> OnPostAsync()
{
    // Perform an initial check to catch FileUpload class attribute violations.
    if (!ModelState.IsValid)
    {
        return Page();
    }

    var filePath = "<PATH-AND-FILE-NAME>";

    using (var fileStream = new FileStream(filePath, FileMode.Create))
    {
        await FileUpload.UploadPublicSchedule.CopyToAsync(fileStream);
    }

    return RedirectToPage("./Index");
}
```

<span data-ttu-id="0f0ba-151">Pracovní proces musí mít oprávnění k zápisu do určeného umístění `filePath`.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-151">The worker process must have write permissions to the location specified by `filePath`.</span></span>

> [!NOTE]
> <span data-ttu-id="0f0ba-152">`filePath` *Musí* patří název souboru.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-152">The `filePath` *must* include the file name.</span></span> <span data-ttu-id="0f0ba-153">Pokud není zadán název souboru, [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) je vyvolána za běhu.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-153">If the file name isn't provided, an [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) is thrown at runtime.</span></span>

> [!WARNING]
> <span data-ttu-id="0f0ba-154">Nikdy zachovat odeslané soubory ve stejném stromu pro adresář jako aplikace.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-154">Never persist uploaded files in the same directory tree as the app.</span></span>
>
> <span data-ttu-id="0f0ba-155">Ukázka kódu neposkytuje žádnou serverové ochranu proti nahrávání souborů škodlivý.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-155">The code sample provides no server-side protection against malicious file uploads.</span></span> <span data-ttu-id="0f0ba-156">Informace o omezení útoku při přijetí soubory od uživatelů najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-156">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="0f0ba-157">Nahrávání neomezený souborů</span><span class="sxs-lookup"><span data-stu-id="0f0ba-157">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="0f0ba-158">Zabezpečení Azure: Ujistěte se, že příslušný ovládací prvky jsou zavedené při přijetí soubory od uživatelů</span><span class="sxs-lookup"><span data-stu-id="0f0ba-158">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="0f0ba-159">Uložte soubor do Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="0f0ba-159">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="0f0ba-160">Pokud chcete nahrát obsah souboru do Azure Blob Storage, najdete v části [Začínáme s Azure Blob Storage pomocí rozhraní .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="0f0ba-160">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="0f0ba-161">Téma ukazuje, jak používat [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) Uložit [FileStream](/dotnet/api/system.io.filestream) do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-161">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="0f0ba-162">Přidání třídy plán</span><span class="sxs-lookup"><span data-stu-id="0f0ba-162">Add the Schedule class</span></span>

<span data-ttu-id="0f0ba-163">Klikněte pravým tlačítkem *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-163">Right click the *Models* folder.</span></span> <span data-ttu-id="0f0ba-164">Vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-164">Select **Add** > **Class**.</span></span> <span data-ttu-id="0f0ba-165">Název třídy **plán** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-165">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="0f0ba-166">Používá třída `Display` a `DisplayFormat` atributy, které vytvářejí popisné názvy a formátování při vykreslení data plánu.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-166">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="0f0ba-167">Aktualizace MovieContext</span><span class="sxs-lookup"><span data-stu-id="0f0ba-167">Update the MovieContext</span></span>

<span data-ttu-id="0f0ba-168">Zadejte `DbSet` v `MovieContext` (*Models/MovieContext.cs*) pro plány:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-168">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="0f0ba-169">Přidat do databáze v tabulce plán</span><span class="sxs-lookup"><span data-stu-id="0f0ba-169">Add the Schedule table to the database</span></span>

<span data-ttu-id="0f0ba-170">Otevřete konzolu Správce balíčků (pomocí PMC): **nástroje** > **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-170">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Pomocí PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="0f0ba-172">V pomocí PMC spuštěním následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-172">In the PMC, execute the following commands.</span></span> <span data-ttu-id="0f0ba-173">Přidat tyto příkazy `Schedule` tabulky k databázi:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-173">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="0f0ba-174">Přidat nahrání souboru stránky Razor</span><span class="sxs-lookup"><span data-stu-id="0f0ba-174">Add a file upload Razor Page</span></span>

<span data-ttu-id="0f0ba-175">V *stránky* složky, vytvoření *plány* složky.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-175">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="0f0ba-176">V *plány* složku vytvořit stránku s názvem *Index.cshtml* pro nahrávání plán s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-176">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="0f0ba-177">Každá skupina formuláře zahrnuje  **\<popisek >** který zobrazí název každé vlastnosti třídy.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-177">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="0f0ba-178">`Display` Atributy v `FileUpload` model poskytují zobrazovaných hodnot pro popisky.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-178">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="0f0ba-179">Například `UploadPublicSchedule` zobrazovaný název vlastnosti je nastaven s `[Display(Name="Public Schedule")]` a proto zobrazí "Veřejné plán" v popisku, když se vykreslí formulář.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-179">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="0f0ba-180">Každá skupina formuláře zahrnuje ověřování  **\<span >**.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-180">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="0f0ba-181">Uživatelský vstup nesplňuje-li vlastnost atributy nastavit v `FileUpload` třídy nebo pokud platí jedna z `ProcessFormFile` metoda souboru ověřování selže, model se nepodaří ověřit.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-181">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="0f0ba-182">Pokud selže ověření modelu, je generován zprávu užitečné ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-182">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="0f0ba-183">Například `Title` vlastnost je opatřen poznámkou `[Required]` a `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-183">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="0f0ba-184">Pokud se uživateli nepodaří zadat název, obdrží zprávu s upozorněním, že je vyžadována hodnota.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-184">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="0f0ba-185">Pokud uživatel zadá hodnotu menší než 3 znaky nebo víc než 60 znaků, obdrží zprávu s upozorněním, že hodnota má nesprávnou délku.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-185">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="0f0ba-186">Soubor je zadaný, který nemá žádný obsah, zobrazí se zpráva označující, že soubor je prázdný.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-186">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="0f0ba-187">Přidání stránky modelu</span><span class="sxs-lookup"><span data-stu-id="0f0ba-187">Add the page model</span></span>

<span data-ttu-id="0f0ba-188">Přidání stránky modelu (*Index.cshtml.cs*) k *plány* složky:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-188">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="0f0ba-189">Model stránky (`IndexModel` v *Index.cshtml.cs*) váže `FileUpload` třídy:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-189">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="0f0ba-190">Model také používá seznam plány (`IList<Schedule>`) k zobrazení plány, které jsou uloženy v databázi na stránce:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-190">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="0f0ba-191">Při načtení stránky s `OnGetAsync`, `Schedules` je naplněny z databáze a používat ke generování tabulky HTML z načíst plánů:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-191">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="0f0ba-192">Při odeslání formuláře na server, `ModelState` je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-192">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="0f0ba-193">Pokud je neplatná, `Schedule` znovu sestaven, a vykreslí stránku s jeden nebo více ověřovacích zpráv s informacemi o tom, proč stránky ověření se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-193">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="0f0ba-194">Pokud je platná, `FileUpload` vlastnosti se používají v *OnPostAsync* k dokončení nahrávání souborů pro dvě verze plánu a vytvořit nový `Schedule` objekt, který chcete uložit data.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-194">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="0f0ba-195">Plán pak je uložena do databáze:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-195">The schedule is then saved to the database:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="0f0ba-196">Odkaz nahrávání souborů stránky Razor</span><span class="sxs-lookup"><span data-stu-id="0f0ba-196">Link the file upload Razor Page</span></span>

<span data-ttu-id="0f0ba-197">Otevřete *_Layout.cshtml* a přidat odkaz na navigačním panelu k dosažení stránka nahrávání souboru:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-197">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="0f0ba-198">Přidat stránku potvrďte odstranění plánu</span><span class="sxs-lookup"><span data-stu-id="0f0ba-198">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="0f0ba-199">Když uživatel klikne na Odstranit plán, je k dispozici šanci na tlačítko Storno.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-199">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="0f0ba-200">Přidat stránku potvrzení odstranění (*Delete.cshtml*) k *plány* složky:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-200">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="0f0ba-201">Model stránky (*Delete.cshtml.cs*) načte jeden plán identifikovaný `id` v datech trasy žádosti.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-201">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="0f0ba-202">Přidat *Delete.cshtml.cs* do souboru *plány* složky:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-202">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="0f0ba-203">`OnPostAsync` Metoda zpracovává odstranění plán, podle jeho `id`:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-203">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="0f0ba-204">Po úspěšném odstranění plán, `RedirectToPage` odešle uživateli zpět na plány *Index.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-204">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="0f0ba-205">Funkční stránky Razor plány</span><span class="sxs-lookup"><span data-stu-id="0f0ba-205">The working Schedules Razor Page</span></span>

<span data-ttu-id="0f0ba-206">Při načtení stránky, popisky a vstupy pro plán název, plán veřejné a privátní plán se vykreslují tlačítka Odeslat:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-206">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Plány stránky Razor, jak je vidět na počáteční zatížení bez chyby ověření a prázdné pole](uploading-files/_static/browser1.png)

<span data-ttu-id="0f0ba-208">Výběr **nahrát** tlačítko bez naplnění žádná pole je v rozporu `[Required]` atributů v modelu.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-208">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="0f0ba-209">`ModelState` Je neplatný.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-209">The `ModelState` is invalid.</span></span> <span data-ttu-id="0f0ba-210">Uživateli se zobrazí chybové zprávy ověření:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-210">The validation error messages are displayed to the user:</span></span>

![Ověření chybové zprávy zobrazují vedle každého vstupního ovládacího prvku](uploading-files/_static/browser2.png)

<span data-ttu-id="0f0ba-212">Zadejte dvě písmena do **název** pole.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-212">Type two letters into the **Title** field.</span></span> <span data-ttu-id="0f0ba-213">Ověření změny zpráva indikující, že název musí být 3 až 60 znaků:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-213">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Změnit název ověřovací zprávy](uploading-files/_static/browser3.png)

<span data-ttu-id="0f0ba-215">Pokud jeden nebo více plánů se odešlou, **načíst plány** načíst plány vykreslí část:</span><span class="sxs-lookup"><span data-stu-id="0f0ba-215">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabulka načíst plány, zobrazuje název každý plán nahrán datum UTC, veřejné verze velikost souboru a velikost souboru privátní verze](uploading-files/_static/browser4.png)

<span data-ttu-id="0f0ba-217">Můžete kliknout na uživatele **odstranit** odkaz z ní k dosažení zobrazení potvrzení odstranění, které mají příležitost potvrdit nebo zrušit operaci odstranění.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-217">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="0f0ba-218">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="0f0ba-218">Troubleshooting</span></span>

<span data-ttu-id="0f0ba-219">Pro řešení potíží s informací o s `IFormFile` nahrát, najdete v článku [nahrávání souborů v ASP.NET Core: řešení potíží s](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="0f0ba-219">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="0f0ba-220">Děkujeme, že dokončení tohoto úvodu do stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-220">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="0f0ba-221">Děkujeme za zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-221">We appreciate feedback.</span></span> <span data-ttu-id="0f0ba-222">[Začínáme s MVC a EF základní](xref:data/ef-mvc/intro) je vynikající postupujte podle kroků až tento kurz.</span><span class="sxs-lookup"><span data-stu-id="0f0ba-222">[Get started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0f0ba-223">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0f0ba-223">Additional resources</span></span>

* [<span data-ttu-id="0f0ba-224">Nahrávání souborů v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f0ba-224">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="0f0ba-225">IFormFile</span><span class="sxs-lookup"><span data-stu-id="0f0ba-225">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

> [!div class="step-by-step"]
> [<span data-ttu-id="0f0ba-226">Předchozí: ověření</span><span class="sxs-lookup"><span data-stu-id="0f0ba-226">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
