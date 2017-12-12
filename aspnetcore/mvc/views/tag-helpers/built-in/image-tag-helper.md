---
title: "Obrázek značky pomocná | Microsoft Docs"
author: pkellner
description: "Ukazuje, jak pracovat s pomocná značka obrázku"
keywords: "ASP.NET Core, značka pomocné rutiny"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a013
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 0d55514508b963ce05031f89a20af696f5d4a670
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="imagetaghelper"></a><span data-ttu-id="cfa70-104">ImageTagHelper</span><span class="sxs-lookup"><span data-stu-id="cfa70-104">ImageTagHelper</span></span>

<span data-ttu-id="cfa70-105">Podle [Petr Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="cfa70-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="cfa70-106">Pomocná značka obrázku rozšiřuje `img` (`<img>`) značky.</span><span class="sxs-lookup"><span data-stu-id="cfa70-106">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="cfa70-107">Vyžaduje `src` značky a taky `boolean` atribut `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="cfa70-107">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="cfa70-108">Pokud zdroj bitové kopie (`src`) je statický soubor na webovém serveru hostitele se připojí jedinečný mezipaměti nejnovějších řetězec jako parametr dotazu na zdroj bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="cfa70-108">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="cfa70-109">Tím se zajistí, že pokud se změní soubor na webovém serveru hostitele, adresu URL jedinečný požadavku je vygenerováno, která zahrnuje parametr aktualizované žádosti.</span><span class="sxs-lookup"><span data-stu-id="cfa70-109">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="cfa70-110">Mezipaměť nejnovějších řetězec je jedinečnou hodnotu představující hodnotu hash souboru statické bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="cfa70-110">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="cfa70-111">Pokud zdroj bitové kopie (`src`) není statický soubor (například soubor nebo vzdálenou adresou URL neexistuje na serveru), `<img>` tagu `src` atribut je generována žádné mezipaměti nejnovějších parametr řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="cfa70-111">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="cfa70-112">Atributy pomocné rutiny značka obrázku</span><span class="sxs-lookup"><span data-stu-id="cfa70-112">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="cfa70-113">ASP připojit verze</span><span class="sxs-lookup"><span data-stu-id="cfa70-113">asp-append-version</span></span>

<span data-ttu-id="cfa70-114">-Li zadána spolu s `src` atribut pomocná značka obrázku je volána.</span><span class="sxs-lookup"><span data-stu-id="cfa70-114">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="cfa70-115">Příklad platné `img` Pomocník značky:</span><span class="sxs-lookup"><span data-stu-id="cfa70-115">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="cfa70-116">Pokud se statický soubor existuje v adresáři *... Wwwroot/Images/asplogo.PNG* generovaný kód jazyka html je podobný následujícímu (hodnota hash bude jiný):</span><span class="sxs-lookup"><span data-stu-id="cfa70-116">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="cfa70-117">Hodnota přiřazená k parametr `v` je hodnota hash souboru na disku.</span><span class="sxs-lookup"><span data-stu-id="cfa70-117">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="cfa70-118">Pokud webový server nemůže získat přístup pro čtení ke statickému souboru odkazováno, ne `v` parametry se přidá do `src` atribut.</span><span class="sxs-lookup"><span data-stu-id="cfa70-118">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="cfa70-119">src</span><span class="sxs-lookup"><span data-stu-id="cfa70-119">src</span></span>

<span data-ttu-id="cfa70-120">Pokud chcete aktivovat pomocná značka obrázku, src atribut je požadován na `<img>` elementu.</span><span class="sxs-lookup"><span data-stu-id="cfa70-120">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="cfa70-121">Pomocník značka obrázku používá `Cache` zprostředkovatele na serveru místní web ukládat počítané `Sha512` daného souboru.</span><span class="sxs-lookup"><span data-stu-id="cfa70-121">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="cfa70-122">Pokud je soubor požádat znovu `Sha512` nemusí být přepočítána.</span><span class="sxs-lookup"><span data-stu-id="cfa70-122">If the file is requested again the `Sha512` does not need to be recalculated.</span></span> <span data-ttu-id="cfa70-123">Je mezipaměť zneplatněna pomocí souboru sledovacího procesu, který je připojen k souboru po souboru `Sha512` se počítá.</span><span class="sxs-lookup"><span data-stu-id="cfa70-123">The Cache is invalidated by a file watcher that is attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cfa70-124">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cfa70-124">Additional resources</span></span>

* <xref:performance/caching/memory>
