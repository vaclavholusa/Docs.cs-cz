---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: Webové stránky ASP.NET (Razor) – rozhraní API rychlé odkazy | Dokumentace Microsoftu
author: tfitzmac
description: Tato stránka obsahuje seznam s krátkou příklady z nejčastěji používaných objektů, vlastnosti a metody pro programování rozhraní ASP.NET Web Pages se syntaxí Razor.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: a10446eb621feb26c485384700ad9980cccf5d8b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752199"
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a><span data-ttu-id="cd6d6-103">Webové stránky ASP.NET (Razor) – rozhraní API rychlé odkazy</span><span class="sxs-lookup"><span data-stu-id="cd6d6-103">ASP.NET Web Pages (Razor) API Quick Reference</span></span>
====================
<span data-ttu-id="cd6d6-104">podle [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cd6d6-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cd6d6-105">Tato stránka obsahuje seznam s krátkou příklady z nejčastěji používaných objektů, vlastnosti a metody pro programování rozhraní ASP.NET Web Pages se syntaxí Razor.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-105">This page contains a list with brief examples of the most commonly used objects, properties, and methods for programming ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="cd6d6-106">Popisy označené "(v2)" byla zavedena v rozhraní ASP.NET Web Pages verze 2.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-106">Descriptions marked with "(v2)" were introduced in ASP.NET Web Pages version 2.</span></span>
> 
> <span data-ttu-id="cd6d6-107">Referenční dokumentace rozhraní API najdete v článku [ASP.NET Web Pages referenční dokumentaci](https://go.microsoft.com/fwlink/?LinkId=208659) na webové stránce MSDN.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-107">For API reference documentation, see the [ASP.NET Web Pages Reference Documentation](https://go.microsoft.com/fwlink/?LinkId=208659) on MSDN.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="cd6d6-108">Verze softwaru</span><span class="sxs-lookup"><span data-stu-id="cd6d6-108">Software versions</span></span>
> 
> 
> - <span data-ttu-id="cd6d6-109">Webové stránky ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="cd6d6-109">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="cd6d6-110">V tomto kurzu se také pracuje s ASP.NET Web Pages 2 a ASP.NET Web Pages 1.0 (s výjimkou funkce označené v2).</span><span class="sxs-lookup"><span data-stu-id="cd6d6-110">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0 (except for features marked v2).</span></span>


<span data-ttu-id="cd6d6-111">Tato stránka obsahuje referenční informace pro následující:</span><span class="sxs-lookup"><span data-stu-id="cd6d6-111">This page contains reference information for the following:</span></span>

- [<span data-ttu-id="cd6d6-112">Třídy</span><span class="sxs-lookup"><span data-stu-id="cd6d6-112">Classes</span></span>](#Classes)
- [<span data-ttu-id="cd6d6-113">Data</span><span class="sxs-lookup"><span data-stu-id="cd6d6-113">Data</span></span>](#Data)
- [<span data-ttu-id="cd6d6-114">Pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="cd6d6-114">Helpers</span></span>](#Helpers)
- [<span data-ttu-id="cd6d6-115">Ověřování</span><span class="sxs-lookup"><span data-stu-id="cd6d6-115">Validation</span></span>](#Validation)

<a id="Classes"></a>
## <a name="classes"></a><span data-ttu-id="cd6d6-116">Třídy</span><span class="sxs-lookup"><span data-stu-id="cd6d6-116">Classes</span></span>

### `AppState[key], AppState[index],App`

<span data-ttu-id="cd6d6-117">Obsahuje data, která můžete sdílet jakékoli stránky v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-117">Contains data that can be shared by any pages in the application.</span></span> <span data-ttu-id="cd6d6-118">Můžete použít dynamické `App` vlastnost přístup ke stejným datům, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="cd6d6-118">You can use the dynamic `App` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

<span data-ttu-id="cd6d6-119">Převede řetězcovou hodnotu na logickou hodnotu (true/false).</span><span class="sxs-lookup"><span data-stu-id="cd6d6-119">Converts a string value to a Boolean value (true/false).</span></span> <span data-ttu-id="cd6d6-120">Vrátí hodnotu false nebo zadanou hodnotu, pokud řetězec nepředstavuje true nebo false.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-120">Returns false or the specified value if the string does not represent true/false.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

<span data-ttu-id="cd6d6-121">Převede řetězcové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-121">Converts a string value to date/time.</span></span> <span data-ttu-id="cd6d6-122">Vrátí `DateTime.MinValue` nebo zadanou hodnotu, pokud řetězec nepředstavuje datum/čas.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-122">Returns `DateTime.MinValue` or the specified value if the string does not represent a date/time.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

<span data-ttu-id="cd6d6-123">Převede řetězcovou hodnotu na desítkovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-123">Converts a string value to a decimal value.</span></span> <span data-ttu-id="cd6d6-124">Vrátí 0,0 nebo pokud řetězec nepředstavuje desítkovou hodnotu zadanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-124">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

<span data-ttu-id="cd6d6-125">Převede řetězcovou hodnotu plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-125">Converts a string value to a float.</span></span> <span data-ttu-id="cd6d6-126">Vrátí 0,0 nebo pokud řetězec nepředstavuje desítkovou hodnotu zadanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-126">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

<span data-ttu-id="cd6d6-127">Převede řetězcovou hodnotu na celé číslo.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-127">Converts a string value to an integer.</span></span> <span data-ttu-id="cd6d6-128">Vrátí hodnotu 0 nebo zadanou hodnotu, pokud řetězec nepředstavuje celé číslo.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-128">Returns 0 or the specified value if the string does not represent an integer.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

<span data-ttu-id="cd6d6-129">Vytvoří adresu URL pro kompatibilní s prohlížečem z místní cesta k souboru, s částmi volitelné další cestu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-129">Creates a browser-compatible URL from a local file path, with optional additional path parts.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

<span data-ttu-id="cd6d6-130">Vykreslí *hodnota* jako značka HTML místo vykreslením výstupu jako kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-130">Renders *value* as HTML markup instead of rendering it as HTML-encoded output.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

<span data-ttu-id="cd6d6-131">Vrátí true, pokud hodnotu lze převést z řetězce na zadaný typ.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-131">Returns true if the value can be converted from a string to the specified type.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

<span data-ttu-id="cd6d6-132">Vrátí true, pokud objekt nebo proměnná nemá žádnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-132">Returns true if the object or variable has no value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

<span data-ttu-id="cd6d6-133">Vrátí true, pokud je žádost POST.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-133">Returns true if the request is a POST.</span></span> <span data-ttu-id="cd6d6-134">(Původní požadavky jsou obvykle GET).</span><span class="sxs-lookup"><span data-stu-id="cd6d6-134">(Initial requests are usually a GET.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

<span data-ttu-id="cd6d6-135">Určuje cestu ke stránce rozložení chcete použít pro tuto stránku.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-135">Specifies the path of a layout page to apply to this page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

<span data-ttu-id="cd6d6-136">Obsahuje data sdílená mezi stránky, stránkami rozložení a částečnými stránkami aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-136">Contains data shared between the page, layout pages, and partial pages in the current request.</span></span> <span data-ttu-id="cd6d6-137">Můžete použít dynamické `Page` vlastnost přístup ke stejným datům, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="cd6d6-137">You can use the dynamic `Page` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

<span data-ttu-id="cd6d6-138">(Rozložení stránky) Vykreslí obsah stránky obsahu, který se nenachází v libovolné pojmenovaných oddílů.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-138">(Layout pages) Renders the content of a content page that is not in any named sections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

<span data-ttu-id="cd6d6-139">Vykreslí stránku obsahu pomocí zadané cesty a volitelné doplňující data.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-139">Renders a content page using the specified path and optional extra data.</span></span> <span data-ttu-id="cd6d6-140">Můžete získat hodnoty nadbytečné parametry z `PageData` pozice (např. 1) nebo klíče (např. 2).</span><span class="sxs-lookup"><span data-stu-id="cd6d6-140">You can get the values of the extra parameters from `PageData` by position (example 1) or key (example 2).</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

<span data-ttu-id="cd6d6-141">(Rozložení stránky) Vykreslí obsah oddíl, který má název.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-141">(Layout pages) Renders a content section that has a name.</span></span> <span data-ttu-id="cd6d6-142">Nastavte *požadované* na hodnotu false, aby oddíl nepovinný.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-142">Set *required* to false to make a section optional.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

<span data-ttu-id="cd6d6-143">Získá nebo nastaví hodnotu souboru cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-143">Gets or sets the value of an HTTP cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

<span data-ttu-id="cd6d6-144">Získá soubory, které byly odeslány v aktuální požadavek.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-144">Gets the files that were uploaded in the current request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

<span data-ttu-id="cd6d6-145">Získá data, která byla publikována ve formě (jako řetězce).</span><span class="sxs-lookup"><span data-stu-id="cd6d6-145">Gets data that was posted in a form (as strings).</span></span> <span data-ttu-id="cd6d6-146">`Request[key]` kontroluje i `Request.Form` a `Request.QueryString` kolekce.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-146">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

<span data-ttu-id="cd6d6-147">Získá data, která byla zadaná v řetězci dotazu adresy URL.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-147">Gets data that was specified in the URL query string.</span></span> <span data-ttu-id="cd6d6-148">`Request[key]` kontroluje i `Request.Form` a `Request.QueryString` kolekce.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-148">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

<span data-ttu-id="cd6d6-149">Ověření pro element formuláře, hodnoty řetězce dotazu, soubor cookie nebo hodnotu hlavičky požadavku selektivně zakáže.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-149">Selectively disables request validation for a form element, query-string value, cookie, or header value.</span></span> <span data-ttu-id="cd6d6-150">Žádost o ověření je ve výchozím nastavení povolené a zabrání uživatelům v publikování značky nebo další potenciálně nebezpečný obsah.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-150">Request validation is enabled by default and prevents users from posting markup or other potentially dangerous content.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

<span data-ttu-id="cd6d6-151">Přidá do odpovědi hlavičku HTTP serveru.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-151">Adds an HTTP server header to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

<span data-ttu-id="cd6d6-152">Ukládá do mezipaměti výstup stránky na určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-152">Caches the page output for a specified time.</span></span> <span data-ttu-id="cd6d6-153">Volitelně můžete nastavit *klouzavé* resetovat vypršení časového limitu na každé stránce přístup a *varyByParams* pro ukládání do mezipaměti různé verze stránky pro každý řetězec dotazu v žádosti o stránku.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-153">Optionally set *sliding* to reset the timeout on each page access and *varyByParams* to cache different versions of the page for each different query string in the page request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

<span data-ttu-id="cd6d6-154">Přesměruje žádost prohlížeče na nové umístění.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-154">Redirects the browser request to a new location.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

<span data-ttu-id="cd6d6-155">Nastaví stavový kód HTTP odeslané do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-155">Sets the HTTP status code sent to the browser.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

<span data-ttu-id="cd6d6-156">Zapíše obsah *dat* do odpovědi pomocí volitelného typu MIME.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-156">Writes the contents of *data* to the response with an optional MIME type.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

<span data-ttu-id="cd6d6-157">Zapíše obsah souboru do odpovědi.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-157">Writes the contents of a file to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

<span data-ttu-id="cd6d6-158">(Rozložení stránky) Definuje oddíl obsahu, který má název.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-158">(Layout pages) Defines a content section that has a name.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

<span data-ttu-id="cd6d6-159">Dekóduje řetězec kódovaný jazykem HTML.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-159">Decodes a string that is HTML encoded.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

<span data-ttu-id="cd6d6-160">Zakóduje řetězec pro vykreslování v kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-160">Encodes a string for rendering in HTML markup.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

<span data-ttu-id="cd6d6-161">Vrací fyzickou cestu serveru pro zadanou virtuální cestu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-161">Returns the server physical path for the specified virtual path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

<span data-ttu-id="cd6d6-162">Dekóduje text z adresy URL.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-162">Decodes text from a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

<span data-ttu-id="cd6d6-163">Zakóduje text do adresy URL.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-163">Encodes text to put in a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

<span data-ttu-id="cd6d6-164">Získá nebo nastaví hodnotu, která existuje, dokud uživatel nezavře prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-164">Gets or sets a value that exists until the user closes the browser.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

<span data-ttu-id="cd6d6-165">Zobrazí řetězec představující hodnotu objektu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-165">Displays a string representation of the object's value.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

<span data-ttu-id="cd6d6-166">Získá doplňující data z adresy URL (například */MyPage/ExtraData*).</span><span class="sxs-lookup"><span data-stu-id="cd6d6-166">Gets additional data from the URL (for example, */MyPage/ExtraData*).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

<span data-ttu-id="cd6d6-167">Změní heslo pro zadaného uživatele.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-167">Changes the password for the specified user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

<span data-ttu-id="cd6d6-168">Potvrzuje se tím účet pomocí účtu potvrzovacím tokenem.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-168">Confirms an account using the account confirmation token.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

<span data-ttu-id="cd6d6-169">Vytvoří nový uživatelský účet pomocí zadaného uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-169">Creates a new user account with the specified user name and password.</span></span> <span data-ttu-id="cd6d6-170">Tak, aby vyžadovala potvrzovací token, předejte hodnotu true pro *requireConfirmationToken.*</span><span class="sxs-lookup"><span data-stu-id="cd6d6-170">To require a confirmation token, pass true for *requireConfirmationToken.*</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

<span data-ttu-id="cd6d6-171">Získá identifikátor celočíselné aktuálně přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-171">Gets the integer identifier for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

<span data-ttu-id="cd6d6-172">Získá název aktuálně přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-172">Gets the name for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

<span data-ttu-id="cd6d6-173">Generuje token resetování hesla, který můžete poslat v e-mailu uživatele tak, aby uživatel můžete resetovat heslo.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-173">Generates a password-reset token that can be sent in email to a user so that the user can reset the password.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

<span data-ttu-id="cd6d6-174">Vrátí ID uživatele uživatelského jména.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-174">Returns the user ID from the user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

<span data-ttu-id="cd6d6-175">Vrátí true, pokud je aktuální uživatel přihlášen.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-175">Returns true if the current user is logged in.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

<span data-ttu-id="cd6d6-176">Vrátí true, pokud byl uživatel potvrzen (například přes e-mail s potvrzením).</span><span class="sxs-lookup"><span data-stu-id="cd6d6-176">Returns true if the user has been confirmed (for example, through a confirmation email).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

<span data-ttu-id="cd6d6-177">Vrátí true, pokud aktuální uživatelské jméno odpovídá zadanému uživatelskému jménu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-177">Returns true if the current user's name matches the specified user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

<span data-ttu-id="cd6d6-178">Přihlásí uživatele v nastavením ověřovací token v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-178">Logs the user in by setting an authentication token in the cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

<span data-ttu-id="cd6d6-179">Protokoly out uživatele tak, že odeberete token souboru cookie pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-179">Logs the user out by removing the authentication token cookie.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

<span data-ttu-id="cd6d6-180">Pokud uživatel není ověřen, nastaví stav protokolu HTTP na 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="cd6d6-180">If the user is not authenticated, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

<span data-ttu-id="cd6d6-181">Pokud má aktuální uživatel není členem jedné ze zadaných rolí, nastaví stav protokolu HTTP na 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="cd6d6-181">If the current user is not a member of one of the specified roles, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

<span data-ttu-id="cd6d6-182">Pokud má aktuální uživatel není uživatel určený parametrem *uživatelské jméno*, nastaví stav protokolu HTTP na 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="cd6d6-182">If the current user is not the user specified by *username*, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

<span data-ttu-id="cd6d6-183">Pokud je platný token pro resetování hesla, změní heslo uživatele na nové heslo.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-183">If the password reset token is valid, changes the user's password to the new password.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a><span data-ttu-id="cd6d6-184">Data</span><span class="sxs-lookup"><span data-stu-id="cd6d6-184">Data</span></span>

### `Database.Execute(SQLstatement [,parameters]`

<span data-ttu-id="cd6d6-185">Spustí *Příkaz_sql* (s volitelnými parametry) například vložit, odstranit nebo aktualizovat a vrátí počet záznamů ovlivněný.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-185">Executes *SQLstatement* (with optional parameters) such as INSERT, DELETE, or UPDATE and returns a count of affected records.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

<span data-ttu-id="cd6d6-186">Vrátí sloupec identity naposledy vloženého řádku.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-186">Returns the identity column from the most recently inserted row.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

<span data-ttu-id="cd6d6-187">Otevře se soubor zadaná databáze nebo databáze pomocí pojmenovaného připojovacího řetězce z *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-187">Opens either the specified database file or the database specified using a named connection string from the *Web.config* file.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

<span data-ttu-id="cd6d6-188">Otevře se v databázi pomocí připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-188">Opens a database using the connection string.</span></span> <span data-ttu-id="cd6d6-189">(Tím se liší od `Database.Open`, která využívá název připojovacího řetězce.)</span><span class="sxs-lookup"><span data-stu-id="cd6d6-189">(This contrasts with `Database.Open`, which uses a connection string name.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

<span data-ttu-id="cd6d6-190">Dotazuje databázi pomocí *Příkaz_sql* (volitelně předávání parametrů) a vrátí výsledky jako kolekci.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-190">Queries the database using *SQLstatement* (optionally passing parameters) and returns the results as a collection.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

<span data-ttu-id="cd6d6-191">Spustí *Příkaz_sql* (s volitelnými parametry) a vrátí jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-191">Executes *SQLstatement* (with optional parameters) and returns a single record.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

<span data-ttu-id="cd6d6-192">Spustí *Příkaz_sql* (s volitelnými parametry) a vrátí jednu hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-192">Executes *SQLstatement* (with optional parameters) and returns a single value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a><span data-ttu-id="cd6d6-193">Pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="cd6d6-193">Helpers</span></span>

### `Analytics.GetGoogleHtml(webPropertyId)`

<span data-ttu-id="cd6d6-194">Vykreslí kód jazyka JavaScript v Google Analytics pro zadané ID.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-194">Renders the Google Analytics JavaScript code for the specified ID.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

<span data-ttu-id="cd6d6-195">Vykreslí kód Analytics StatCounter jazyka JavaScript pro zadaný projekt.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-195">Renders the StatCounter Analytics JavaScript code for the specified project.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

<span data-ttu-id="cd6d6-196">Vykreslí kód Analytics Yahoo jazyka JavaScript pro zadaný účet.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-196">Renders the Yahoo Analytics JavaScript code for the specified account.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

<span data-ttu-id="cd6d6-197">Předá vyhledávání Bingu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-197">Passes a search to Bing.</span></span> <span data-ttu-id="cd6d6-198">Chcete-li určit lokalitu pro účely vyhledávání a název vyhledávacího pole, můžete nastavit `Bing.SiteUrl` a `Bing.SiteTitle` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-198">To specify the site to search and a title for the search box, you can set the `Bing.SiteUrl` and `Bing.SiteTitle` properties.</span></span> <span data-ttu-id="cd6d6-199">Obvykle nastavte tyto vlastnosti  *\_AppStart* stránky.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-199">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

<span data-ttu-id="cd6d6-200">Inicializuje grafu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-200">Initializes a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

<span data-ttu-id="cd6d6-201">Přidá do grafu legendu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-201">Adds a legend to a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

<span data-ttu-id="cd6d6-202">Přidává řadu hodnot do grafu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-202">Adds a series of values to the chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

<span data-ttu-id="cd6d6-203">Vrátí hodnotu hash pro zadaná data.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-203">Returns a hash for the specified data.</span></span> <span data-ttu-id="cd6d6-204">Výchozí algoritmus je `sha256`.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-204">The default algorithm is `sha256`.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

<span data-ttu-id="cd6d6-205">Umožňuje uživatelům Facebooku připojení na stránky.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-205">Lets Facebook users make a connection to pages.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

<span data-ttu-id="cd6d6-206">Vykreslí uživatelského rozhraní pro nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-206">Renders UI for uploading files.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

<span data-ttu-id="cd6d6-207">Vykreslí zadané značky hráče Xbox.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-207">Renders the specified Xbox gamer tag.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

<span data-ttu-id="cd6d6-208">Vykreslí obraz Gravatar zadané e-mailových adres.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-208">Renders the Gravatar image for the specified email address.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

<span data-ttu-id="cd6d6-209">Převede datový objekt na řetězec ve formátu JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="cd6d6-209">Converts a data object to a string in the JavaScript Object Notation (JSON) format.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

<span data-ttu-id="cd6d6-210">Převede vstupní řetězec kódovaný ve formátu JSON na datový objekt, který může iterovat nebo vložit do databáze.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-210">Converts a JSON-encoded input string to a data object that you can iterate over or insert into a database.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

<span data-ttu-id="cd6d6-211">Vykreslí sociálních sítí odkazuje pomocí zadaný název a volitelný adresy URL.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-211">Renders social networking links using the specified title and optional URL.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

<span data-ttu-id="cd6d6-212">Přidruží chybovou zprávu s polem formuláře.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-212">Associates an error message with a form field.</span></span> <span data-ttu-id="cd6d6-213">Použití `ModelState` pomocná rutina pro přístup k tomuto členu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-213">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

<span data-ttu-id="cd6d6-214">Přidruží chybovou zprávu s formuláři.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-214">Associates an error message with a form.</span></span> <span data-ttu-id="cd6d6-215">Použití `ModelState` pomocná rutina pro přístup k tomuto členu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-215">Use the `ModelState` helper to access this member.</span></span>

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

<span data-ttu-id="cd6d6-216">Vrátí true, pokud nejsou žádné chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-216">Returns true if there are no validation errors.</span></span> <span data-ttu-id="cd6d6-217">Použití `ModelState` pomocná rutina pro přístup k tomuto členu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-217">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

<span data-ttu-id="cd6d6-218">Vykreslí vlastnosti a hodnoty objektu a všech podřízených objektů.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-218">Renders the properties and values of an object and any child objects.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

<span data-ttu-id="cd6d6-219">Vykreslí ověřovací test nástroje reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-219">Renders the reCAPTCHA verification test.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

<span data-ttu-id="cd6d6-220">Nastaví veřejného a privátního klíče pro služby nástroje reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-220">Sets public and private keys for the reCAPTCHA service.</span></span> <span data-ttu-id="cd6d6-221">Obvykle nastavte tyto vlastnosti  *\_AppStart* stránky.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-221">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

<span data-ttu-id="cd6d6-222">Vrátí výsledek testu nástroje reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-222">Returns the result of the reCAPTCHA test.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

<span data-ttu-id="cd6d6-223">Vykreslí stavové informace o rozhraní ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-223">Renders status information about ASP.NET Web Pages.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

<span data-ttu-id="cd6d6-224">Vykreslí Twitteru datového proudu pro zadaného uživatele.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-224">Renders a Twitter stream for the specified user.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

<span data-ttu-id="cd6d6-225">Vykreslí Twitteru datového proudu pro zadaný hledaný text.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-225">Renders a Twitter stream for the specified search text.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

<span data-ttu-id="cd6d6-226">Vykreslí Flash přehrávač videa pro zadaný soubor s volitelné šířku a výšku.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-226">Renders a Flash video player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

<span data-ttu-id="cd6d6-227">Vykreslí Windows Media player pro zadaný soubor s volitelné šířku a výšku.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-227">Renders a Windows Media player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

<span data-ttu-id="cd6d6-228">Vykreslí přehrávač Silverlight pro zadaný rozbočovač *.xap* soubor se požadovaná šířka a výška.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-228">Renders a Silverlight player for the specified *.xap* file with required width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

<span data-ttu-id="cd6d6-229">Vrátí objekt definovaný podle objektu *klíč*, nebo hodnota null, pokud objekt nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-229">Returns the object specified by *key*, or null if the object is not found.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

<span data-ttu-id="cd6d6-230">Odebere objekt definovaný podle objektu *klíč* z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-230">Removes the object specified by *key* from the cache.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

<span data-ttu-id="cd6d6-231">Vloží *hodnotu* do mezipaměti v rámci určeného parametrem *klíč*.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-231">Puts *value* into the cache under the name specified by *key*.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

<span data-ttu-id="cd6d6-232">Vytvoří novou `WebGrid` pomocí dat z dotazu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-232">Creates a new `WebGrid` object using data from a query.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

<span data-ttu-id="cd6d6-233">Vykreslí značku pro zobrazení dat v tabulce jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-233">Renders markup to display data in an HTML table.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

<span data-ttu-id="cd6d6-234">Vykreslí stránkování pro `WebGrid` objektu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-234">Renders a pager for the `WebGrid` object.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

<span data-ttu-id="cd6d6-235">Načte obrázek ze zadané cesty.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-235">Loads an image from the specified path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

<span data-ttu-id="cd6d6-236">Přidá zadanou image jako vodoznak.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-236">Adds the specified image as a watermark.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

<span data-ttu-id="cd6d6-237">Přidá zadaný text do obrázku.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-237">Adds the specified text to the image.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

<span data-ttu-id="cd6d6-238">Převrátí obrázek vodorovně nebo svisle.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-238">Flips the image horizontally or vertically.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

<span data-ttu-id="cd6d6-239">Načte bitovou kopii při publikování image na stránku při nahrávání souboru.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-239">Loads an image when an image is posted to a page during a file upload.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

<span data-ttu-id="cd6d6-240">Změní velikost bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-240">Resizes an the image.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

<span data-ttu-id="cd6d6-241">Otočí obrázek vlevo nebo vpravo.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-241">Rotates the image to the left or the right.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

<span data-ttu-id="cd6d6-242">Obrázek uloží do zadané cesty.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-242">Saves the image to the specified path.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

<span data-ttu-id="cd6d6-243">Nastaví heslo pro SMTP server.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-243">Sets the password for the SMTP server.</span></span> <span data-ttu-id="cd6d6-244">Za normálních okolností byste tuto vlastnost nastavit  *\_AppStart* stránky.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-244">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

<span data-ttu-id="cd6d6-245">Odešle e-mailovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-245">Sends an email message.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

<span data-ttu-id="cd6d6-246">Nastaví název serveru SMTP.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-246">Sets the SMTP server name.</span></span> <span data-ttu-id="cd6d6-247">Za normálních okolností byste tuto vlastnost nastavit<em>\_AppStart</em> stránky.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-247">Normally you set this property in the<em>\_AppStart</em> page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

<span data-ttu-id="cd6d6-248">Nastaví uživatelské jméno pro SMTP server.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-248">Sets the user name for the SMTP server.</span></span> <span data-ttu-id="cd6d6-249">Obvykle tuto vlastnost měli nastavit  *\_AppStart* stránky.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-249">Normally you should set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a><span data-ttu-id="cd6d6-250">Ověřování</span><span class="sxs-lookup"><span data-stu-id="cd6d6-250">Validation</span></span>

### `Html.ValidationMessage(field)`

<span data-ttu-id="cd6d6-251">(v2) Vykreslí chybovou zprávu ověření pro zadané pole.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-251">(v2) Renders a validation error message for the specified field.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

<span data-ttu-id="cd6d6-252">(v2) Zobrazí seznam všech chyb ověření.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-252">(v2) Displays a list of all validation errors.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

<span data-ttu-id="cd6d6-253">(v2) Zaregistruje element vstupu uživatele pro zadaný typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-253">(v2) Registers a user input element for the specified type of validation.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

<span data-ttu-id="cd6d6-254">(v2) Dynamicky vykreslí atributy třídy šablony stylů CSS pro ověřování na straně klienta, aby lze formátovat chybových zpráv ověření.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-254">(v2) Dynamically renders CSS class attributes for client-side validation so that you can format validation error messages.</span></span> <span data-ttu-id="cd6d6-255">(Vyžaduje odkazovat na knihovny příslušný skript klienta a definovat třídy šablony stylů CSS.)</span><span class="sxs-lookup"><span data-stu-id="cd6d6-255">(Requires that you reference the appropriate client-script libraries and that you define CSS classes.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

<span data-ttu-id="cd6d6-256">(v2) Umožňuje ověřování na straně klienta pro uživatele vstupní pole.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-256">(v2) Enables client-side validation for the user input field.</span></span> <span data-ttu-id="cd6d6-257">(Vyžaduje, že odkazujete na příslušný skript klienta knihovny.)</span><span class="sxs-lookup"><span data-stu-id="cd6d6-257">(Requires that you reference the appropriate client-script libraries.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

<span data-ttu-id="cd6d6-258">(v2) Vrátí true, pokud všechny elementy vstupu uživatele, které jsou registrovaných pro ověření obsahovat platné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-258">(v2) Returns true if all user input elements that are registred for validation contain valid values.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

<span data-ttu-id="cd6d6-259">(v2) Určuje, že uživatelé musí zadat hodnotu pro daný element vstupu uživatele.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-259">(v2) Specifies that users must provide a value for the user input element.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

<span data-ttu-id="cd6d6-260">(v2) Určuje, že uživatelé musí zadat hodnoty pro jednotlivé elementy vstupu uživatele.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-260">(v2) Specifies that users must provide values for each of the user input elements.</span></span> <span data-ttu-id="cd6d6-261">Tato metoda neumožňuje zadat vlastní chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-261">This method does not let you specify a custom error message.</span></span>

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

<span data-ttu-id="cd6d6-262">(v2) Určuje ověřovací test, pokud použijete `Validation.Add` metody.</span><span class="sxs-lookup"><span data-stu-id="cd6d6-262">(v2) Specifies a validation test when you use the `Validation.Add` method.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
