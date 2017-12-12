---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: "ASP.NET Web Pages referenční dokumentace rozhraní API rychlé (Razor) | Microsoft Docs"
author: tfitzmac
description: "Tato stránka obsahuje seznam s běžně používané objekty, vlastnosti a metody pro programování webových stránek ASP.NET se syntaxí Razor stručný příklady."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 35f91f4dbea4881d9dabc4ab7c6b96dbb6a01ea2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a><span data-ttu-id="19d74-103">ASP.NET Web Pages referenční dokumentace rozhraní API rychlé (Razor)</span><span class="sxs-lookup"><span data-stu-id="19d74-103">ASP.NET Web Pages (Razor) API Quick Reference</span></span>
====================
<span data-ttu-id="19d74-104">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="19d74-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="19d74-105">Tato stránka obsahuje seznam s běžně používané objekty, vlastnosti a metody pro programování webových stránek ASP.NET se syntaxí Razor stručný příklady.</span><span class="sxs-lookup"><span data-stu-id="19d74-105">This page contains a list with brief examples of the most commonly used objects, properties, and methods for programming ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="19d74-106">Popisy, které jsou označené jako "(v2)" byly zavedeny v rozhraní ASP.NET Web Pages verze 2.</span><span class="sxs-lookup"><span data-stu-id="19d74-106">Descriptions marked with "(v2)" were introduced in ASP.NET Web Pages version 2.</span></span>
> 
> <span data-ttu-id="19d74-107">Referenční dokumentace rozhraní API, najdete v článku [ASP.NET Web Pages referenční dokumentaci k nástroji](https://go.microsoft.com/fwlink/?LinkId=208659) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="19d74-107">For API reference documentation, see the [ASP.NET Web Pages Reference Documentation](https://go.microsoft.com/fwlink/?LinkId=208659) on MSDN.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="19d74-108">Verze softwaru</span><span class="sxs-lookup"><span data-stu-id="19d74-108">Software versions</span></span>
> 
> 
> - <span data-ttu-id="19d74-109">Rozhraní ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="19d74-109">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="19d74-110">V tomto kurzu funguje taky s ASP.NET Web Pages 2 a ASP.NET Web Pages 1.0 (s výjimkou funkcí označena v2).</span><span class="sxs-lookup"><span data-stu-id="19d74-110">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0 (except for features marked v2).</span></span>


<span data-ttu-id="19d74-111">Tato stránka obsahuje referenční informace pro následující:</span><span class="sxs-lookup"><span data-stu-id="19d74-111">This page contains reference information for the following:</span></span>

- [<span data-ttu-id="19d74-112">Třídy</span><span class="sxs-lookup"><span data-stu-id="19d74-112">Classes</span></span>](#Classes)
- [<span data-ttu-id="19d74-113">Data</span><span class="sxs-lookup"><span data-stu-id="19d74-113">Data</span></span>](#Data)
- [<span data-ttu-id="19d74-114">Pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="19d74-114">Helpers</span></span>](#Helpers)
- [<span data-ttu-id="19d74-115">Ověření</span><span class="sxs-lookup"><span data-stu-id="19d74-115">Validation</span></span>](#Validation)

<a id="Classes"></a>
## <a name="classes"></a><span data-ttu-id="19d74-116">Třídy</span><span class="sxs-lookup"><span data-stu-id="19d74-116">Classes</span></span>

### `AppState[key], AppState[index],App`

<span data-ttu-id="19d74-117">Obsahuje data, která může být sdílen všechny stránky v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="19d74-117">Contains data that can be shared by any pages in the application.</span></span> <span data-ttu-id="19d74-118">Můžete použít dynamická `App` vlastnost, která má přístup ke stejným datům, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="19d74-118">You can use the dynamic `App` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

<span data-ttu-id="19d74-119">Převede řetězcovou hodnotu na logickou hodnotu (true nebo false).</span><span class="sxs-lookup"><span data-stu-id="19d74-119">Converts a string value to a Boolean value (true/false).</span></span> <span data-ttu-id="19d74-120">Vrátí hodnotu false nebo pro určenou hodnotu, pokud řetězec nepředstavuje true nebo false.</span><span class="sxs-lookup"><span data-stu-id="19d74-120">Returns false or the specified value if the string does not represent true/false.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

<span data-ttu-id="19d74-121">Převede hodnotu řetězce na datum a čas.</span><span class="sxs-lookup"><span data-stu-id="19d74-121">Converts a string value to date/time.</span></span> <span data-ttu-id="19d74-122">Vrátí `DateTime.MinValue` nebo pro určenou hodnotu, pokud řetězec nepředstavuje datum a čas.</span><span class="sxs-lookup"><span data-stu-id="19d74-122">Returns `DateTime.MinValue` or the specified value if the string does not represent a date/time.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

<span data-ttu-id="19d74-123">Převede řetězcovou hodnotu na desítkovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="19d74-123">Converts a string value to a decimal value.</span></span> <span data-ttu-id="19d74-124">Vrátí 0,0 nebo pro určenou hodnotu, pokud řetězec nepředstavuje desítkovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="19d74-124">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

<span data-ttu-id="19d74-125">Převede hodnotu řetězce na typ float.</span><span class="sxs-lookup"><span data-stu-id="19d74-125">Converts a string value to a float.</span></span> <span data-ttu-id="19d74-126">Vrátí 0,0 nebo pro určenou hodnotu, pokud řetězec nepředstavuje desítkovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="19d74-126">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

<span data-ttu-id="19d74-127">Převede řetězcovou hodnotu na celé číslo.</span><span class="sxs-lookup"><span data-stu-id="19d74-127">Converts a string value to an integer.</span></span> <span data-ttu-id="19d74-128">Vrátí hodnotu 0 nebo pro určenou hodnotu, pokud řetězec nepředstavuje celé číslo.</span><span class="sxs-lookup"><span data-stu-id="19d74-128">Returns 0 or the specified value if the string does not represent an integer.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

<span data-ttu-id="19d74-129">Vytvoří adresu URL kompatibilních s prohlížeči z místního souboru cestu, části volitelné další cesty.</span><span class="sxs-lookup"><span data-stu-id="19d74-129">Creates a browser-compatible URL from a local file path, with optional additional path parts.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

<span data-ttu-id="19d74-130">Vykreslí *hodnotu* jako značka HTML místo vykreslením výstupu jako kódovaný jazykem HTML.</span><span class="sxs-lookup"><span data-stu-id="19d74-130">Renders *value* as HTML markup instead of rendering it as HTML-encoded output.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

<span data-ttu-id="19d74-131">Vrátí hodnotu true Pokud hodnotu lze převést z řetězce na zadaný typ.</span><span class="sxs-lookup"><span data-stu-id="19d74-131">Returns true if the value can be converted from a string to the specified type.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

<span data-ttu-id="19d74-132">Vrátí hodnotu true Pokud objekt nebo proměnná nemá žádnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="19d74-132">Returns true if the object or variable has no value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

<span data-ttu-id="19d74-133">Vrátí hodnotu true Pokud je požadavek POST.</span><span class="sxs-lookup"><span data-stu-id="19d74-133">Returns true if the request is a POST.</span></span> <span data-ttu-id="19d74-134">(Počáteční požadavky jsou obvykle GET).</span><span class="sxs-lookup"><span data-stu-id="19d74-134">(Initial requests are usually a GET.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

<span data-ttu-id="19d74-135">Určuje cestu ke stránce rozložení pro použití na tuto stránku.</span><span class="sxs-lookup"><span data-stu-id="19d74-135">Specifies the path of a layout page to apply to this page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

<span data-ttu-id="19d74-136">Obsahuje data sdílená mezi stránky, stránkami rozložení a částečnými stránkami v aktuální žádosti.</span><span class="sxs-lookup"><span data-stu-id="19d74-136">Contains data shared between the page, layout pages, and partial pages in the current request.</span></span> <span data-ttu-id="19d74-137">Můžete použít dynamická `Page` vlastnost, která má přístup ke stejným datům, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="19d74-137">You can use the dynamic `Page` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

<span data-ttu-id="19d74-138">(Rozložení stránky) Vykreslí obsah stránky obsahu, která se nenachází v žádné části s názvem.</span><span class="sxs-lookup"><span data-stu-id="19d74-138">(Layout pages) Renders the content of a content page that is not in any named sections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

<span data-ttu-id="19d74-139">Vykreslí stránku obsahu pomocí zadané cesty a volitelné doplňující data.</span><span class="sxs-lookup"><span data-stu-id="19d74-139">Renders a content page using the specified path and optional extra data.</span></span> <span data-ttu-id="19d74-140">Můžete získat hodnoty parametrů navíc z `PageData` pozice (třeba 1) nebo klíče (Příklad 2).</span><span class="sxs-lookup"><span data-stu-id="19d74-140">You can get the values of the extra parameters from `PageData` by position (example 1) or key (example 2).</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

<span data-ttu-id="19d74-141">(Rozložení stránky) Vykreslí obsah oddíl, který má název.</span><span class="sxs-lookup"><span data-stu-id="19d74-141">(Layout pages) Renders a content section that has a name.</span></span> <span data-ttu-id="19d74-142">Nastavit *požadované* na hodnotu false, aby oddíl volitelné.</span><span class="sxs-lookup"><span data-stu-id="19d74-142">Set *required* to false to make a section optional.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

<span data-ttu-id="19d74-143">Získá nebo nastaví hodnotu souboru cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="19d74-143">Gets or sets the value of an HTTP cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

<span data-ttu-id="19d74-144">Získá soubory, které byly odeslány v aktuální žádosti.</span><span class="sxs-lookup"><span data-stu-id="19d74-144">Gets the files that were uploaded in the current request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

<span data-ttu-id="19d74-145">Získá data, která byla ve formuláři odeslány (jako řetězce).</span><span class="sxs-lookup"><span data-stu-id="19d74-145">Gets data that was posted in a form (as strings).</span></span> <span data-ttu-id="19d74-146">`Request[key]`kontroluje, jak `Request.Form` a `Request.QueryString` kolekce.</span><span class="sxs-lookup"><span data-stu-id="19d74-146">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

<span data-ttu-id="19d74-147">Získá data, která byla zadaná v řetězci dotazu adresy URL.</span><span class="sxs-lookup"><span data-stu-id="19d74-147">Gets data that was specified in the URL query string.</span></span> <span data-ttu-id="19d74-148">`Request[key]`kontroluje, jak `Request.Form` a `Request.QueryString` kolekce.</span><span class="sxs-lookup"><span data-stu-id="19d74-148">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

<span data-ttu-id="19d74-149">Zakáže selektivně žádosti o ověření pro form element, hodnoty řetězce dotazu, soubor cookie nebo hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="19d74-149">Selectively disables request validation for a form element, query-string value, cookie, or header value.</span></span> <span data-ttu-id="19d74-150">Ověření žádosti je ve výchozím nastavení povolené a zabrání uživatelům v publikování kódu nebo jiný potenciálně nebezpečný obsah.</span><span class="sxs-lookup"><span data-stu-id="19d74-150">Request validation is enabled by default and prevents users from posting markup or other potentially dangerous content.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

<span data-ttu-id="19d74-151">Přidá do odpovědi hlavičku HTTP serveru.</span><span class="sxs-lookup"><span data-stu-id="19d74-151">Adds an HTTP server header to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

<span data-ttu-id="19d74-152">Ukládá do mezipaměti výstup stránky po určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="19d74-152">Caches the page output for a specified time.</span></span> <span data-ttu-id="19d74-153">Volitelně můžete nastavit *klouzavé* resetovat vypršení časového limitu na každé stránce přístup a *varyByParams* pro ukládání do mezipaměti různé verze stránky pro každý řetězec jiný dotaz v požadavku stránky.</span><span class="sxs-lookup"><span data-stu-id="19d74-153">Optionally set *sliding* to reset the timeout on each page access and *varyByParams* to cache different versions of the page for each different query string in the page request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

<span data-ttu-id="19d74-154">Přesměruje požadavek prohlížeče do nového umístění.</span><span class="sxs-lookup"><span data-stu-id="19d74-154">Redirects the browser request to a new location.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

<span data-ttu-id="19d74-155">Nastaví stavový kód protokolu HTTP, který je odesláno prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="19d74-155">Sets the HTTP status code sent to the browser.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

<span data-ttu-id="19d74-156">Zapíše obsah *data* do odpovědi volitelné typu MIME.</span><span class="sxs-lookup"><span data-stu-id="19d74-156">Writes the contents of *data* to the response with an optional MIME type.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

<span data-ttu-id="19d74-157">Zapíše obsah souboru do odpovědi.</span><span class="sxs-lookup"><span data-stu-id="19d74-157">Writes the contents of a file to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

<span data-ttu-id="19d74-158">(Rozložení stránky) Definuje oddíl obsahu, který má název.</span><span class="sxs-lookup"><span data-stu-id="19d74-158">(Layout pages) Defines a content section that has a name.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

<span data-ttu-id="19d74-159">Dekóduje řetězec, který není kódován jazykem HTML.</span><span class="sxs-lookup"><span data-stu-id="19d74-159">Decodes a string that is HTML encoded.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

<span data-ttu-id="19d74-160">Kóduje řetězce pro vykreslování v kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="19d74-160">Encodes a string for rendering in HTML markup.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

<span data-ttu-id="19d74-161">Vrací fyzickou cestu serveru pro zadanou virtuální cestu.</span><span class="sxs-lookup"><span data-stu-id="19d74-161">Returns the server physical path for the specified virtual path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

<span data-ttu-id="19d74-162">Dekóduje text z adresy URL.</span><span class="sxs-lookup"><span data-stu-id="19d74-162">Decodes text from a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

<span data-ttu-id="19d74-163">Zakóduje text uvést v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="19d74-163">Encodes text to put in a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

<span data-ttu-id="19d74-164">Získá nebo nastaví hodnotu, která existuje, dokud uživatel nezavře prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="19d74-164">Gets or sets a value that exists until the user closes the browser.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

<span data-ttu-id="19d74-165">Zobrazí řetězcovou reprezentaci objektu hodnoty.</span><span class="sxs-lookup"><span data-stu-id="19d74-165">Displays a string representation of the object's value.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

<span data-ttu-id="19d74-166">Získá doplňující data z adresy URL (například */MyPage/ExtraData*).</span><span class="sxs-lookup"><span data-stu-id="19d74-166">Gets additional data from the URL (for example, */MyPage/ExtraData*).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

<span data-ttu-id="19d74-167">Změní heslo pro zadaného uživatele.</span><span class="sxs-lookup"><span data-stu-id="19d74-167">Changes the password for the specified user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

<span data-ttu-id="19d74-168">Potvrdí na účtu pomocí účtu potvrzovací token.</span><span class="sxs-lookup"><span data-stu-id="19d74-168">Confirms an account using the account confirmation token.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

<span data-ttu-id="19d74-169">Vytvoří nový uživatelský účet pomocí zadaného uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="19d74-169">Creates a new user account with the specified user name and password.</span></span> <span data-ttu-id="19d74-170">Pokud chcete vyžadovat potvrzovací token, předá hodnotu true pro *requireConfirmationToken.*</span><span class="sxs-lookup"><span data-stu-id="19d74-170">To require a confirmation token, pass true for *requireConfirmationToken.*</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

<span data-ttu-id="19d74-171">Získá identifikátor celé číslo pro aktuálně přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="19d74-171">Gets the integer identifier for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

<span data-ttu-id="19d74-172">Získá název pro aktuálně přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="19d74-172">Gets the name for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

<span data-ttu-id="19d74-173">Generuje token pro resetování hesla, který lze poslat e-mailem uživateli, aby uživatel můžete resetovat heslo.</span><span class="sxs-lookup"><span data-stu-id="19d74-173">Generates a password-reset token that can be sent in email to a user so that the user can reset the password.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

<span data-ttu-id="19d74-174">Vrátí ID uživatele uživatelského jména.</span><span class="sxs-lookup"><span data-stu-id="19d74-174">Returns the user ID from the user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

<span data-ttu-id="19d74-175">Vrátí hodnotu true Pokud je aktuální uživatel je přihlášen.</span><span class="sxs-lookup"><span data-stu-id="19d74-175">Returns true if the current user is logged in.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

<span data-ttu-id="19d74-176">Vrátí hodnotu true Pokud je uživatel potvrzený (například prostřednictvím e-mail s potvrzením.).</span><span class="sxs-lookup"><span data-stu-id="19d74-176">Returns true if the user has been confirmed (for example, through a confirmation email).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

<span data-ttu-id="19d74-177">Vrátí hodnotu true Pokud aktuální uživatelské jméno odpovídá zadanému uživatelskému jménu.</span><span class="sxs-lookup"><span data-stu-id="19d74-177">Returns true if the current user's name matches the specified user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

<span data-ttu-id="19d74-178">Přihlásí uživatele v nastavením ověřovací token v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="19d74-178">Logs the user in by setting an authentication token in the cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

<span data-ttu-id="19d74-179">Zaznamená uživatele odhlašování odebráním tokenu souboru cookie pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="19d74-179">Logs the user out by removing the authentication token cookie.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

<span data-ttu-id="19d74-180">Pokud není uživatel ověřen, nastaví stav protokolu HTTP na 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="19d74-180">If the user is not authenticated, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

<span data-ttu-id="19d74-181">Pokud má aktuální uživatel není členem jedné ze zadaných rolí, nastaví stav protokolu HTTP na 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="19d74-181">If the current user is not a member of one of the specified roles, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

<span data-ttu-id="19d74-182">Pokud má aktuální uživatel není uživatel určený parametrem *uživatelské jméno*, nastaví stav protokolu HTTP na 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="19d74-182">If the current user is not the user specified by *username*, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

<span data-ttu-id="19d74-183">Pokud je platný token pro resetování hesla, změní heslo uživatele k nové heslo.</span><span class="sxs-lookup"><span data-stu-id="19d74-183">If the password reset token is valid, changes the user's password to the new password.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a><span data-ttu-id="19d74-184">Data</span><span class="sxs-lookup"><span data-stu-id="19d74-184">Data</span></span>

### `Database.Execute(SQLstatement [,parameters]`

<span data-ttu-id="19d74-185">Provede *SQLstatement* (s volitelné parametry) jako je například vložení, DELETE nebo UPDATE a vrací počet příslušné záznamy.</span><span class="sxs-lookup"><span data-stu-id="19d74-185">Executes *SQLstatement* (with optional parameters) such as INSERT, DELETE, or UPDATE and returns a count of affected records.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

<span data-ttu-id="19d74-186">Vrátí sloupec identity z naposledy vloženého řádku.</span><span class="sxs-lookup"><span data-stu-id="19d74-186">Returns the identity column from the most recently inserted row.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

<span data-ttu-id="19d74-187">Otevře se soubor zadaná databáze nebo databáze pomocí pojmenovaného připojovacího řetězce z *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="19d74-187">Opens either the specified database file or the database specified using a named connection string from the *Web.config* file.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

<span data-ttu-id="19d74-188">Otevře se v databázi pomocí připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="19d74-188">Opens a database using the connection string.</span></span> <span data-ttu-id="19d74-189">(Tím se liší od `Database.Open`, který používá název připojovacího řetězce.)</span><span class="sxs-lookup"><span data-stu-id="19d74-189">(This contrasts with `Database.Open`, which uses a connection string name.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

<span data-ttu-id="19d74-190">Dotazuje databázi pomocí *SQLstatement* (volitelně předávání parametrů) a vrátí výsledky jako kolekce.</span><span class="sxs-lookup"><span data-stu-id="19d74-190">Queries the database using *SQLstatement* (optionally passing parameters) and returns the results as a collection.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

<span data-ttu-id="19d74-191">Provede *SQLstatement* (s volitelné parametry) a vrátí jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="19d74-191">Executes *SQLstatement* (with optional parameters) and returns a single record.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

<span data-ttu-id="19d74-192">Provede *SQLstatement* (s volitelné parametry) a vrátí jednu hodnotu.</span><span class="sxs-lookup"><span data-stu-id="19d74-192">Executes *SQLstatement* (with optional parameters) and returns a single value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a><span data-ttu-id="19d74-193">Pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="19d74-193">Helpers</span></span>

### `Analytics.GetGoogleHtml(webPropertyId)`

<span data-ttu-id="19d74-194">Vykreslí kód jazyka JavaScript Google Analytics pro zadané ID.</span><span class="sxs-lookup"><span data-stu-id="19d74-194">Renders the Google Analytics JavaScript code for the specified ID.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

<span data-ttu-id="19d74-195">Vykreslí kód StatCounter Analytics JavaScript zadaného projektu.</span><span class="sxs-lookup"><span data-stu-id="19d74-195">Renders the StatCounter Analytics JavaScript code for the specified project.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

<span data-ttu-id="19d74-196">Vykreslí kód Yahoo Analytics JavaScript pro zadaný účet.</span><span class="sxs-lookup"><span data-stu-id="19d74-196">Renders the Yahoo Analytics JavaScript code for the specified account.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

<span data-ttu-id="19d74-197">Hledání předá do služby Bing.</span><span class="sxs-lookup"><span data-stu-id="19d74-197">Passes a search to Bing.</span></span> <span data-ttu-id="19d74-198">Chcete-li zadat webový server a název vyhledávacího pole hledání, můžete nastavit `Bing.SiteUrl` a `Bing.SiteTitle` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="19d74-198">To specify the site to search and a title for the search box, you can set the `Bing.SiteUrl` and `Bing.SiteTitle` properties.</span></span> <span data-ttu-id="19d74-199">Za normálních okolností nastavte tyto vlastnosti  *\_AppStart* stránky.</span><span class="sxs-lookup"><span data-stu-id="19d74-199">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

<span data-ttu-id="19d74-200">Inicializuje grafu.</span><span class="sxs-lookup"><span data-stu-id="19d74-200">Initializes a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

<span data-ttu-id="19d74-201">Přidá do grafu legendu.</span><span class="sxs-lookup"><span data-stu-id="19d74-201">Adds a legend to a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

<span data-ttu-id="19d74-202">Přidá do grafu řady hodnot.</span><span class="sxs-lookup"><span data-stu-id="19d74-202">Adds a series of values to the chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

<span data-ttu-id="19d74-203">Vrátí hodnotu hash pro zadaná data.</span><span class="sxs-lookup"><span data-stu-id="19d74-203">Returns a hash for the specified data.</span></span> <span data-ttu-id="19d74-204">Výchozí algoritmus `sha256`.</span><span class="sxs-lookup"><span data-stu-id="19d74-204">The default algorithm is `sha256`.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

<span data-ttu-id="19d74-205">Umožňuje uživatelům Facebook se připojte k stránky.</span><span class="sxs-lookup"><span data-stu-id="19d74-205">Lets Facebook users make a connection to pages.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

<span data-ttu-id="19d74-206">Vykreslí uživatelského rozhraní pro nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="19d74-206">Renders UI for uploading files.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

<span data-ttu-id="19d74-207">Vykreslí zadaný Xbox hráči značku.</span><span class="sxs-lookup"><span data-stu-id="19d74-207">Renders the specified Xbox gamer tag.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

<span data-ttu-id="19d74-208">Vykreslí zadané e-mailovou adresu pro Gravatar bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="19d74-208">Renders the Gravatar image for the specified email address.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

<span data-ttu-id="19d74-209">Převede datový objekt na řetězec ve formátu JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="19d74-209">Converts a data object to a string in the JavaScript Object Notation (JSON) format.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

<span data-ttu-id="19d74-210">Převede řetězec vstupní zakódovaná ve formátu JSON na datový objekt, který můžete iterace v nebo můžete vložit do databáze.</span><span class="sxs-lookup"><span data-stu-id="19d74-210">Converts a JSON-encoded input string to a data object that you can iterate over or insert into a database.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

<span data-ttu-id="19d74-211">Vykreslí sociálních sítí odkazuje pomocí zadaný název a volitelný adresy URL.</span><span class="sxs-lookup"><span data-stu-id="19d74-211">Renders social networking links using the specified title and optional URL.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

<span data-ttu-id="19d74-212">Přidruží chybovou zprávu pole formuláře.</span><span class="sxs-lookup"><span data-stu-id="19d74-212">Associates an error message with a form field.</span></span> <span data-ttu-id="19d74-213">Použití `ModelState` pomocné rutiny pro přístup k tomuto členu.</span><span class="sxs-lookup"><span data-stu-id="19d74-213">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

<span data-ttu-id="19d74-214">Přidruží chybovou zprávu formuláře.</span><span class="sxs-lookup"><span data-stu-id="19d74-214">Associates an error message with a form.</span></span> <span data-ttu-id="19d74-215">Použití `ModelState` pomocné rutiny pro přístup k tomuto členu.</span><span class="sxs-lookup"><span data-stu-id="19d74-215">Use the `ModelState` helper to access this member.</span></span>

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

<span data-ttu-id="19d74-216">Vrátí hodnotu true Pokud nejsou žádné chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="19d74-216">Returns true if there are no validation errors.</span></span> <span data-ttu-id="19d74-217">Použití `ModelState` pomocné rutiny pro přístup k tomuto členu.</span><span class="sxs-lookup"><span data-stu-id="19d74-217">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

<span data-ttu-id="19d74-218">Vykreslí vlastnosti a hodnoty objekt a všechny podřízené objekty.</span><span class="sxs-lookup"><span data-stu-id="19d74-218">Renders the properties and values of an object and any child objects.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

<span data-ttu-id="19d74-219">Vykreslí ověřovací test nástroje reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="19d74-219">Renders the reCAPTCHA verification test.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

<span data-ttu-id="19d74-220">Nastaví veřejné a soukromé klíče pro službu nástroje reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="19d74-220">Sets public and private keys for the reCAPTCHA service.</span></span> <span data-ttu-id="19d74-221">Za normálních okolností nastavte tyto vlastnosti  *\_AppStart* stránky.</span><span class="sxs-lookup"><span data-stu-id="19d74-221">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

<span data-ttu-id="19d74-222">Vrátí výsledek testu nástroje reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="19d74-222">Returns the result of the reCAPTCHA test.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

<span data-ttu-id="19d74-223">Vykreslí stavové informace o rozhraní ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="19d74-223">Renders status information about ASP.NET Web Pages.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

<span data-ttu-id="19d74-224">Vykreslí Twitter datového proudu pro zadaného uživatele.</span><span class="sxs-lookup"><span data-stu-id="19d74-224">Renders a Twitter stream for the specified user.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

<span data-ttu-id="19d74-225">Vykreslí Twitter datového proudu pro zadaný hledaný text.</span><span class="sxs-lookup"><span data-stu-id="19d74-225">Renders a Twitter stream for the specified search text.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

<span data-ttu-id="19d74-226">Vykreslí Flash přehrávání videa pro zadaného souboru s volitelné šířku a výšku.</span><span class="sxs-lookup"><span data-stu-id="19d74-226">Renders a Flash video player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

<span data-ttu-id="19d74-227">Vykreslí Windows Media player pro zadaného souboru s volitelné šířku a výšku.</span><span class="sxs-lookup"><span data-stu-id="19d74-227">Renders a Windows Media player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

<span data-ttu-id="19d74-228">Vykreslí přehrávač Silverlight pro zadaný *.xap* soubor s požadované šířku a výšku.</span><span class="sxs-lookup"><span data-stu-id="19d74-228">Renders a Silverlight player for the specified *.xap* file with required width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

<span data-ttu-id="19d74-229">Vrací objekt určeného *klíč*, nebo hodnota null, pokud objekt nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="19d74-229">Returns the object specified by *key*, or null if the object is not found.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

<span data-ttu-id="19d74-230">Odebere objekt určeného *klíč* z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="19d74-230">Removes the object specified by *key* from the cache.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

<span data-ttu-id="19d74-231">Vloží *hodnotu* do mezipaměti pod názvem určeného *klíč*.</span><span class="sxs-lookup"><span data-stu-id="19d74-231">Puts *value* into the cache under the name specified by *key*.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

<span data-ttu-id="19d74-232">Vytvoří nový `WebGrid` pomocí dat z dotazu.</span><span class="sxs-lookup"><span data-stu-id="19d74-232">Creates a new `WebGrid` object using data from a query.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

<span data-ttu-id="19d74-233">Vykreslí značku pro zobrazení dat v tabulce jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="19d74-233">Renders markup to display data in an HTML table.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

<span data-ttu-id="19d74-234">Vykreslí pager pro `WebGrid` objektu.</span><span class="sxs-lookup"><span data-stu-id="19d74-234">Renders a pager for the `WebGrid` object.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

<span data-ttu-id="19d74-235">Načte obrázek ze zadané cesty.</span><span class="sxs-lookup"><span data-stu-id="19d74-235">Loads an image from the specified path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

<span data-ttu-id="19d74-236">Přidá zadanou bitovou kopii jako vodoznak.</span><span class="sxs-lookup"><span data-stu-id="19d74-236">Adds the specified image as a watermark.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

<span data-ttu-id="19d74-237">Přidá zadaný text do bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="19d74-237">Adds the specified text to the image.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

<span data-ttu-id="19d74-238">Převrátí bitovou kopii vodorovně nebo svisle.</span><span class="sxs-lookup"><span data-stu-id="19d74-238">Flips the image horizontally or vertically.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

<span data-ttu-id="19d74-239">Načte obrázek při odeslání bitovou kopii na stránku během nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="19d74-239">Loads an image when an image is posted to a page during a file upload.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

<span data-ttu-id="19d74-240">Změní velikost bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="19d74-240">Resizes an the image.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

<span data-ttu-id="19d74-241">Otočí obrázek vlevo nebo vpravo.</span><span class="sxs-lookup"><span data-stu-id="19d74-241">Rotates the image to the left or the right.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

<span data-ttu-id="19d74-242">Uloží obrázek na zadanou cestu.</span><span class="sxs-lookup"><span data-stu-id="19d74-242">Saves the image to the specified path.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

<span data-ttu-id="19d74-243">Nastaví heslo pro SMTP server.</span><span class="sxs-lookup"><span data-stu-id="19d74-243">Sets the password for the SMTP server.</span></span> <span data-ttu-id="19d74-244">Za normálních okolností, můžete tuto vlastnost nastavit v  *\_AppStart* stránky.</span><span class="sxs-lookup"><span data-stu-id="19d74-244">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

<span data-ttu-id="19d74-245">Odešle e-mailovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="19d74-245">Sends an email message.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

<span data-ttu-id="19d74-246">Nastaví název serveru SMTP.</span><span class="sxs-lookup"><span data-stu-id="19d74-246">Sets the SMTP server name.</span></span> <span data-ttu-id="19d74-247">Za normálních okolností, můžete tuto vlastnost nastavit v*\_AppStart* stránky.</span><span class="sxs-lookup"><span data-stu-id="19d74-247">Normally you set this property in the*\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

<span data-ttu-id="19d74-248">Nastaví uživatelské jméno pro SMTP server.</span><span class="sxs-lookup"><span data-stu-id="19d74-248">Sets the user name for the SMTP server.</span></span> <span data-ttu-id="19d74-249">Za normálních okolností byste měli nastavit tuto vlastnost v  *\_AppStart* stránky.</span><span class="sxs-lookup"><span data-stu-id="19d74-249">Normally you should set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a><span data-ttu-id="19d74-250">Ověřování</span><span class="sxs-lookup"><span data-stu-id="19d74-250">Validation</span></span>

### `Html.ValidationMessage(field)`

<span data-ttu-id="19d74-251">(v2) Vykreslí chybovou zprávu ověření pro zadané pole.</span><span class="sxs-lookup"><span data-stu-id="19d74-251">(v2) Renders a validation error message for the specified field.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

<span data-ttu-id="19d74-252">(v2) Zobrazí seznam všech chyb ověření.</span><span class="sxs-lookup"><span data-stu-id="19d74-252">(v2) Displays a list of all validation errors.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

<span data-ttu-id="19d74-253">(v2) Zaregistruje element vstupu uživatele pro zadaný typ ověření.</span><span class="sxs-lookup"><span data-stu-id="19d74-253">(v2) Registers a user input element for the specified type of validation.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

<span data-ttu-id="19d74-254">(v2) Dynamicky vykreslí atributy třídy CSS pro ověřování na straně klienta, tak, že chybových zpráv ověření.</span><span class="sxs-lookup"><span data-stu-id="19d74-254">(v2) Dynamically renders CSS class attributes for client-side validation so that you can format validation error messages.</span></span> <span data-ttu-id="19d74-255">(Vyžaduje odkazovat na příslušné klientského skriptu knihovny a definování tříd CSS.)</span><span class="sxs-lookup"><span data-stu-id="19d74-255">(Requires that you reference the appropriate client-script libraries and that you define CSS classes.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

<span data-ttu-id="19d74-256">(v2) Umožňuje ověření na straně klienta pro vstupní pole uživatele.</span><span class="sxs-lookup"><span data-stu-id="19d74-256">(v2) Enables client-side validation for the user input field.</span></span> <span data-ttu-id="19d74-257">(Vyžaduje odkazovat na knihovny odpovídající klientského skriptu.)</span><span class="sxs-lookup"><span data-stu-id="19d74-257">(Requires that you reference the appropriate client-script libraries.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

<span data-ttu-id="19d74-258">(v2) Pokud vrátí hodnotu true všechny elementy vstupu uživatele, které jsou registrovaných pro ověření obsahovat platné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="19d74-258">(v2) Returns true if all user input elements that are registred for validation contain valid values.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

<span data-ttu-id="19d74-259">(v2) Určuje, že uživatelé musí zadat hodnotu pro daný element vstupu uživatele.</span><span class="sxs-lookup"><span data-stu-id="19d74-259">(v2) Specifies that users must provide a value for the user input element.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

<span data-ttu-id="19d74-260">(v2) Určuje, že uživatelé musí zadat hodnoty pro jednotlivé elementy vstupu uživatele.</span><span class="sxs-lookup"><span data-stu-id="19d74-260">(v2) Specifies that users must provide values for each of the user input elements.</span></span> <span data-ttu-id="19d74-261">Tato metoda neumožňuje zadat vlastní chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="19d74-261">This method does not let you specify a custom error message.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

<span data-ttu-id="19d74-262">(v2) Určuje ověřovací test, při použití `Validation.Add` metoda.</span><span class="sxs-lookup"><span data-stu-id="19d74-262">(v2) Specifies a validation test when you use the `Validation.Add` method.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
