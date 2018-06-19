---
title: Nahrávání souborů v ASP.NET Core
author: ardalis
description: Jak používat vazby modelu a vysílání datového proudu k nahrání souborů v rozhraní ASP.NET MVC jádra.
manager: wpickett
ms.author: riande
ms.date: 07/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/file-uploads
ms.openlocfilehash: 7ba4f6d9e3901c310fe9fa7a70382d9243d8b347
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
ms.locfileid: "32740410"
---
# <a name="file-uploads-in-aspnet-core"></a>Nahrávání souborů v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/)

ASP.NET MVC akce podporují nahrávání jeden nebo více souborů pomocí jednoduchého modelu vazba pro menší soubory nebo datové proudy větší soubory.

[Zobrazení nebo stažení ukázky z webu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>Nahrávání malých souborů s vazby modelu

Pokud chcete nahrát malé soubory, můžete použít vícedílného formuláře HTML nebo vytvořit požadavek POST pomocí jazyka JavaScript. Formuláře příklad pomocí syntaxe Razor, která podporuje více odeslané soubory, je zobrazena níže:

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

Aby bylo možné podporovat nahrávání souborů, musí zadat formuláře HTML `enctype` z `multipart/form-data`. `files` Elementu input výše uvedeném podporuje nahrávání více souborů. Vynechat `multiple` atribut na tento vstupní element povolit jen jeden soubor k odeslání. Výše uvedený kód vykreslení v prohlížeči jako:

![Soubor odeslání formuláře](file-uploads/_static/upload-form.png)

Jednotlivé soubory, které jsou odeslány na server, je možné přistupovat prostřednictvím [vazby modelu](xref:mvc/models/model-binding) pomocí [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) rozhraní. `IFormFile` má následující strukturu:

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
> Nemusíte spoléhat na nebo vztahu důvěryhodnosti `FileName` vlastnost bez ověření. `FileName` Vlastnost musí být použit pouze pro účely zobrazení.

Při nahrávání souborů pomocí vazby modelu a `IFormFile` rozhraní, metoda akce může přijímat buď jediný `IFormFile` nebo `IEnumerable<IFormFile>` (nebo `List<IFormFile>`) představující několik souborů. V následujícím příkladu prochází nejméně jedné odeslané soubory, uloží je do místního systému souborů a vrátí celkový počet a velikost souborů nahrát.

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

Soubory, které budou pomocí `IFormFile` technika jsou uložená do vyrovnávací paměti v paměti nebo na disku na webovém serveru před zpracováním. Uvnitř metody akce `IFormFile` obsah je dostupný jako datový proud. Kromě místního systému souborů, soubory Streamovat k [úložiště objektů Azure Blob](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) nebo [Entity Framework](https://docs.microsoft.com/ef/core/index).

Chcete-li uložit data binárních souborů v databázi pomocí Entity Framework, definovat vlastnost typu `byte[]` u entity:

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

Zadejte viewmodel vlastnost typu `IFormFile`:

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile` můžete použít přímo jako parametru metody akce nebo jako vlastnost viewmodel, jak je uvedeno výše.

Kopírování `IFormFile` do datového proudu a uložte ho do bajtového pole:

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
> Buďte opatrní při ukládání binární data v relačních databází, jak může nepříznivě ovlivnit výkon.

## <a name="uploading-large-files-with-streaming"></a>Nahrávání velkých souborů s streamování

Pokud velikost nebo četnost nahrávání souborů způsobuje problémy prostředků pro aplikace, zvažte streamování nahrávání souborů a místo ukládání do vyrovnávací paměti jako celek, stejně jako výše uvedeném přístupu vazby modelu. Při použití `IFormFile` a vazby modelu je mnohem jednodušší řešení, streamování vyžaduje několik kroků k implementaci správně.

> [!NOTE]
> Všechny jednoho z vyrovnávací paměti souboru překročení 64KB přesunout z paměti RAM do dočasného souboru na disku na serveru. Prostředky (disk RAM) používané nahrávání souborů závisí na počtu a velikosti nahrávání souběžných souborů. Streamování není mnoho o výkonu se jedná o škálování. Pokud se pokusíte vyrovnávací paměť příliš mnoho nahrávání, dojde k chybě váš web pokud jí dojde místo paměti nebo volného místa.

Následující příklad ukazuje použití JavaScript nebo úhlová k vysílání datového proudu k akci kontroleru. V souboru antiforgery token je generován pomocí vlastní atribut filtru a předán v hlavičkách protokolu HTTP místo v textu požadavku. Vzhledem k tomu, že metoda akce zpracovává odesílaná data přímo, vazby modelu zakázal jiný filtr. V rámci akce, jsou číst obsah formuláře pomocí `MultipartReader`, který čte každého uživatele `MultipartSection`, zpracování souboru nebo ukládání obsahu podle potřeby. Jakmile přečetli všechny části akci provede vlastní vazby modelu.

Úvodní akce načtení formuláře a uloží antiforgery token do souboru cookie (prostřednictvím `GenerateAntiforgeryTokenCookieForAjax` atribut):

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

Atribut používá integrované ASP.NET Core [Antiforgery](xref:security/anti-request-forgery) nastavit soubor cookie s tokenu žádosti o podporu:

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

Úhlová automaticky předá antiforgery token v hlavičce žádosti s názvem `X-XSRF-TOKEN`. Aplikace ASP.NET MVC základní nakonfigurovaná odkazovat na tuto hlavičku v jeho konfiguraci v *Startup.cs*:

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

`DisableFormValueModelBinding` Slouží ke zakázat vazby modelu pro atribut, viz následující obrázek, `Upload` metody akce.

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

Vzhledem k tomu, že se zakázanou vazbou modelu, `Upload` nepřijímá parametry metody akce. Pracuje přímo `Request` vlastnost `ControllerBase`. A `MultipartReader` slouží k načtení každý oddíl. Soubor zůstane uložen, s názvem GUID souboru a data klíč/hodnota je uložena v `KeyValueAccumulator`. Jakmile byly načteny všechny části, obsah `KeyValueAccumulator` se používají k vytvoření vazby dat formuláře. typ modelu.

Kompletní `Upload` metoda jsou uvedeny níže:

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>Poradce při potížích

Tady jsou některé běžné problémy při práci s nahrávání souborů a jejich možná řešení.

### <a name="unexpected-not-found-error-with-iis"></a>Nebyl nalezen došlo k neočekávané chybě služby IIS

K následující chybě označuje vaší nahrávání souborů překračuje server je nakonfigurován `maxAllowedContentLength`:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

Ve výchozím nastavení je `30000000`, což je přibližně 28.6 MB. Hodnota lze přizpůsobit úpravou *web.config*:

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

Toto nastavení platí jenom pro službu IIS. Při hostování na Kestrel chování nedojde ve výchozím nastavení. Další informace najdete v tématu [omezení počtu požadavků \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).

### <a name="null-reference-exception-with-iformfile"></a>Výjimka odkazu s hodnotou Null IFormFile

Pokud je řadič přijímá nahrát soubory pomocí `IFormFile` můžete zjistit, že hodnota je vždy hodnotu null, potvrďte, že je zadání svého formuláře HTML, ale `enctype` hodnotu `multipart/form-data`. Pokud není tento atribut nastavený na `<form>` elementu, nedojde, nahrávání souborů a žádné hranice `IFormFile` argumenty bude mít hodnotu null.
