---
title: "Nahrávání souborů v ASP.NET Core"
author: ardalis
description: "Jak používat vazby modelu a vysílání datového proudu k nahrání souborů v rozhraní ASP.NET MVC jádra."
ms.author: riande
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/file-uploads
ms.openlocfilehash: 3c5abe84a5c7cc399e0586e680a414fab7a26c1d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="50768-103">Nahrávání souborů v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="50768-103">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="50768-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="50768-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="50768-105">ASP.NET MVC akce podporují nahrávání jeden nebo více souborů pomocí jednoduchého modelu vazba pro menší soubory nebo datové proudy větší soubory.</span><span class="sxs-lookup"><span data-stu-id="50768-105">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

[<span data-ttu-id="50768-106">Zobrazení nebo stažení ukázky z webu GitHub</span><span class="sxs-lookup"><span data-stu-id="50768-106">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="50768-107">Nahrávání malých souborů s vazby modelu</span><span class="sxs-lookup"><span data-stu-id="50768-107">Uploading small files with model binding</span></span>

<span data-ttu-id="50768-108">Pokud chcete nahrát malé soubory, můžete použít vícedílného formuláře HTML nebo vytvořit požadavek POST pomocí jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="50768-108">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="50768-109">Formuláře příklad pomocí syntaxe Razor, která podporuje více odeslané soubory, je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="50768-109">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

<span data-ttu-id="50768-110">Aby bylo možné podporovat nahrávání souborů, musí zadat formuláře HTML `enctype` z `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="50768-110">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="50768-111">`files` Elementu input výše uvedeném podporuje nahrávání více souborů.</span><span class="sxs-lookup"><span data-stu-id="50768-111">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="50768-112">Vynechat `multiple` atribut na tento vstupní element povolit jen jeden soubor k odeslání.</span><span class="sxs-lookup"><span data-stu-id="50768-112">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="50768-113">Výše uvedený kód vykreslení v prohlížeči jako:</span><span class="sxs-lookup"><span data-stu-id="50768-113">The above markup renders in a browser as:</span></span>

![Soubor odeslání formuláře](file-uploads/_static/upload-form.png)

<span data-ttu-id="50768-115">Jednotlivé soubory, které jsou odeslány na server, je možné přistupovat prostřednictvím [vazby modelu](xref:mvc/models/model-binding) pomocí [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="50768-115">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="50768-116">`IFormFile`má následující strukturu:</span><span class="sxs-lookup"><span data-stu-id="50768-116">`IFormFile` has the following structure:</span></span>

```csharp
public interface IFormFile
{
    string ContentType { get; }
    string ContentDisposition { get; }
    IHeaderDictionary Headers { get; }
    long Length { get; }
    string Name { get; }
    string FileName { get; }
    Stream OpenReadStream();
    void CopyTo(Stream target);
    Task CopyToAsync(Stream target, CancellationToken cancellationToken = null);
}
```

> [!WARNING]
> <span data-ttu-id="50768-117">Nemusíte spoléhat na nebo vztahu důvěryhodnosti `FileName` vlastnost bez ověření.</span><span class="sxs-lookup"><span data-stu-id="50768-117">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="50768-118">`FileName` Vlastnost musí být použit pouze pro účely zobrazení.</span><span class="sxs-lookup"><span data-stu-id="50768-118">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="50768-119">Při nahrávání souborů pomocí vazby modelu a `IFormFile` rozhraní, metoda akce může přijímat buď jediný `IFormFile` nebo `IEnumerable<IFormFile>` (nebo `List<IFormFile>`) představující několik souborů.</span><span class="sxs-lookup"><span data-stu-id="50768-119">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="50768-120">V následujícím příkladu prochází nejméně jedné odeslané soubory, uloží je do místního systému souborů a vrátí celkový počet a velikost souborů nahrát.</span><span class="sxs-lookup"><span data-stu-id="50768-120">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

<span data-ttu-id="50768-121">Soubory, které budou pomocí `IFormFile` technika jsou uložená do vyrovnávací paměti v paměti nebo na disku na webovém serveru před zpracováním.</span><span class="sxs-lookup"><span data-stu-id="50768-121">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="50768-122">Uvnitř metody akce `IFormFile` obsah je dostupný jako datový proud.</span><span class="sxs-lookup"><span data-stu-id="50768-122">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="50768-123">Kromě místního systému souborů, soubory Streamovat k [úložiště objektů Azure Blob](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) nebo [Entity Framework](https://docs.microsoft.com/ef/core/index).</span><span class="sxs-lookup"><span data-stu-id="50768-123">In addition to the local file system, files can be streamed to [Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) or [Entity Framework](https://docs.microsoft.com/ef/core/index).</span></span>

<span data-ttu-id="50768-124">Chcete-li uložit data binárních souborů v databázi pomocí Entity Framework, definovat vlastnost typu `byte[]` u entity:</span><span class="sxs-lookup"><span data-stu-id="50768-124">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="50768-125">Zadejte viewmodel vlastnost typu `IFormFile`:</span><span class="sxs-lookup"><span data-stu-id="50768-125">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="50768-126">`IFormFile`můžete použít přímo jako parametru metody akce nebo jako vlastnost viewmodel, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="50768-126">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="50768-127">Kopírování `IFormFile` do datového proudu a uložte ho do bajtového pole:</span><span class="sxs-lookup"><span data-stu-id="50768-127">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

```csharp
// POST: /Account/Register
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Register(RegisterViewModel model)
{
    ViewData["ReturnUrl"] = returnUrl;
    if  (ModelState.IsValid)
    {
        var user = new ApplicationUser {
          UserName = model.Email,
          Email = model.Email
        };
        using (var memoryStream = new MemoryStream())
        {
            await model.AvatarImage.CopyToAsync(memoryStream);
            user.AvatarImage = memoryStream.ToArray();
        }
    // additional logic omitted
    
    // Don't rely on or trust the model.AvatarImage.FileName property 
    // without validation.
}
```

> [!NOTE]
> <span data-ttu-id="50768-128">Buďte opatrní při ukládání binární data v relačních databází, jak může nepříznivě ovlivnit výkon.</span><span class="sxs-lookup"><span data-stu-id="50768-128">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="50768-129">Nahrávání velkých souborů s streamování</span><span class="sxs-lookup"><span data-stu-id="50768-129">Uploading large files with streaming</span></span>

<span data-ttu-id="50768-130">Pokud velikost nebo četnost nahrávání souborů způsobuje problémy prostředků pro aplikace, zvažte streamování nahrávání souborů a místo ukládání do vyrovnávací paměti jako celek, stejně jako výše uvedeném přístupu vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="50768-130">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="50768-131">Při použití `IFormFile` a vazby modelu je mnohem jednodušší řešení, streamování vyžaduje několik kroků k implementaci správně.</span><span class="sxs-lookup"><span data-stu-id="50768-131">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="50768-132">Všechny jednoho z vyrovnávací paměti souboru překročení 64KB přesunout z paměti RAM do dočasného souboru na disku na serveru.</span><span class="sxs-lookup"><span data-stu-id="50768-132">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="50768-133">Prostředky (disk RAM) používané nahrávání souborů závisí na počtu a velikosti nahrávání souběžných souborů.</span><span class="sxs-lookup"><span data-stu-id="50768-133">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="50768-134">Streamování není mnoho o výkonu, je o škálování.</span><span class="sxs-lookup"><span data-stu-id="50768-134">Streaming is not so much about perf, it's about scale.</span></span> <span data-ttu-id="50768-135">Pokud se pokusíte vyrovnávací paměť příliš mnoho nahrávání, dojde k chybě váš web pokud jí dojde místo paměti nebo volného místa.</span><span class="sxs-lookup"><span data-stu-id="50768-135">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="50768-136">Následující příklad ukazuje použití JavaScript nebo úhlová k vysílání datového proudu k akci kontroleru.</span><span class="sxs-lookup"><span data-stu-id="50768-136">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="50768-137">V souboru antiforgery token je generován pomocí vlastní atribut filtru a předán v hlavičkách protokolu HTTP místo v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="50768-137">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="50768-138">Vzhledem k tomu, že metoda akce zpracovává odesílaná data přímo, vazby modelu zakázal jiný filtr.</span><span class="sxs-lookup"><span data-stu-id="50768-138">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="50768-139">V rámci akce, jsou číst obsah formuláře pomocí `MultipartReader`, který čte každého uživatele `MultipartSection`, zpracování souboru nebo ukládání obsahu podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="50768-139">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="50768-140">Jakmile přečetli všechny části akci provede vlastní vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="50768-140">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="50768-141">Úvodní akce načtení formuláře a uloží antiforgery token do souboru cookie (prostřednictvím `GenerateAntiforgeryTokenCookieForAjax` atribut):</span><span class="sxs-lookup"><span data-stu-id="50768-141">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="50768-142">Atribut používá integrované ASP.NET Core [Antiforgery](xref:security/anti-request-forgery) nastavit soubor cookie s tokenu žádosti o podporu:</span><span class="sxs-lookup"><span data-stu-id="50768-142">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

<span data-ttu-id="50768-143">Úhlová automaticky předá antiforgery token v hlavičce žádosti s názvem `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="50768-143">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="50768-144">Aplikace ASP.NET MVC základní nakonfigurovaná odkazovat na tuto hlavičku v jeho konfiguraci v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="50768-144">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

<span data-ttu-id="50768-145">`DisableFormValueModelBinding` Slouží ke zakázat vazby modelu pro atribut, viz následující obrázek, `Upload` metody akce.</span><span class="sxs-lookup"><span data-stu-id="50768-145">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

<span data-ttu-id="50768-146">Vzhledem k tomu, že se zakázanou vazbou modelu, `Upload` nepřijímá parametry metody akce.</span><span class="sxs-lookup"><span data-stu-id="50768-146">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="50768-147">Pracuje přímo `Request` vlastnost `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="50768-147">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="50768-148">A `MultipartReader` slouží k načtení každý oddíl.</span><span class="sxs-lookup"><span data-stu-id="50768-148">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="50768-149">Soubor zůstane uložen, s názvem GUID souboru a data klíč/hodnota je uložena v `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="50768-149">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="50768-150">Jakmile byly načteny všechny části, obsah `KeyValueAccumulator` se používají k vytvoření vazby dat formuláře. typ modelu.</span><span class="sxs-lookup"><span data-stu-id="50768-150">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="50768-151">Kompletní `Upload` metoda jsou uvedeny níže:</span><span class="sxs-lookup"><span data-stu-id="50768-151">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a><span data-ttu-id="50768-152">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="50768-152">Troubleshooting</span></span>

<span data-ttu-id="50768-153">Tady jsou některé běžné problémy při práci s nahrávání souborů a jejich možná řešení.</span><span class="sxs-lookup"><span data-stu-id="50768-153">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="50768-154">Nebyl nalezen došlo k neočekávané chybě služby IIS</span><span class="sxs-lookup"><span data-stu-id="50768-154">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="50768-155">K následující chybě označuje vaší nahrávání souborů překračuje server je nakonfigurován `maxAllowedContentLength`:</span><span class="sxs-lookup"><span data-stu-id="50768-155">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="50768-156">Ve výchozím nastavení je `30000000`, což je přibližně 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="50768-156">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="50768-157">Hodnota lze přizpůsobit úpravou *web.config*:</span><span class="sxs-lookup"><span data-stu-id="50768-157">The value can be customized by editing *web.config*:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- This will handle requests up to 50MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="50768-158">Toto nastavení platí jenom pro službu IIS.</span><span class="sxs-lookup"><span data-stu-id="50768-158">This setting only applies to IIS.</span></span> <span data-ttu-id="50768-159">Při hostování na Kestrel chování nedojde ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="50768-159">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="50768-160">Další informace najdete v tématu [omezení počtu požadavků \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="50768-160">For more information, see [Request Limits \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="50768-161">Výjimka odkazu s hodnotou Null IFormFile</span><span class="sxs-lookup"><span data-stu-id="50768-161">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="50768-162">Pokud je řadič přijímá nahrát soubory pomocí `IFormFile` můžete zjistit, že hodnota je vždy hodnotu null, potvrďte, že je zadání svého formuláře HTML, ale `enctype` hodnotu `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="50768-162">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="50768-163">Pokud není tento atribut nastavený na `<form>` elementu, nedojde, nahrávání souborů a žádné hranice `IFormFile` argumenty bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="50768-163">If this attribute is not set on the `<form>` element, the file upload will not occur and any bound `IFormFile` arguments will be null.</span></span>
