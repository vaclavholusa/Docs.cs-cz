---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: ASP.NET Web Pages referenční dokumentace rozhraní API rychlé (Razor) | Microsoft Docs
author: tfitzmac
description: Tato stránka obsahuje seznam s běžně používané objekty, vlastnosti a metody pro programování webových stránek ASP.NET se syntaxí Razor stručný příklady.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 5f9d84f4d453583d7d4eae12e4fc510275255616
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a>ASP.NET Web Pages referenční dokumentace rozhraní API rychlé (Razor)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tato stránka obsahuje seznam s běžně používané objekty, vlastnosti a metody pro programování webových stránek ASP.NET se syntaxí Razor stručný příklady.
> 
> Popisy, které jsou označené jako "(v2)" byly zavedeny v rozhraní ASP.NET Web Pages verze 2.
> 
> Referenční dokumentace rozhraní API, najdete v článku [ASP.NET Web Pages referenční dokumentaci k nástroji](https://go.microsoft.com/fwlink/?LinkId=208659) na webu MSDN.
> 
> ## <a name="software-versions"></a>Verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
>   
> 
> V tomto kurzu funguje taky s ASP.NET Web Pages 2 a ASP.NET Web Pages 1.0 (s výjimkou funkcí označena v2).


Tato stránka obsahuje referenční informace pro následující:

- [Třídy](#Classes)
- [Data](#Data)
- [Pomocné rutiny](#Helpers)
- [Ověřování](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Třídy

### `AppState[key], AppState[index],App`

Obsahuje data, která může být sdílen všechny stránky v aplikaci. Můžete použít dynamická `App` vlastnost, která má přístup ke stejným datům, jako v následujícím příkladu:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Převede řetězcovou hodnotu na logickou hodnotu (true nebo false). Vrátí hodnotu false nebo pro určenou hodnotu, pokud řetězec nepředstavuje true nebo false.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Převede hodnotu řetězce na datum a čas. Vrátí `DateTime.MinValue` nebo pro určenou hodnotu, pokud řetězec nepředstavuje datum a čas.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Převede řetězcovou hodnotu na desítkovou hodnotu. Vrátí 0,0 nebo pro určenou hodnotu, pokud řetězec nepředstavuje desítkovou hodnotu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Převede hodnotu řetězce na typ float. Vrátí 0,0 nebo pro určenou hodnotu, pokud řetězec nepředstavuje desítkovou hodnotu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Převede řetězcovou hodnotu na celé číslo. Vrátí hodnotu 0 nebo pro určenou hodnotu, pokud řetězec nepředstavuje celé číslo.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Vytvoří adresu URL kompatibilních s prohlížeči z místního souboru cestu, části volitelné další cesty.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Vykreslí *hodnotu* jako značka HTML místo vykreslením výstupu jako kódovaný jazykem HTML.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Vrátí hodnotu true Pokud hodnotu lze převést z řetězce na zadaný typ.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Vrátí hodnotu true Pokud objekt nebo proměnná nemá žádnou hodnotu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Vrátí hodnotu true Pokud je požadavek POST. (Počáteční požadavky jsou obvykle GET).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Určuje cestu ke stránce rozložení pro použití na tuto stránku.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Obsahuje data sdílená mezi stránky, stránkami rozložení a částečnými stránkami v aktuální žádosti. Můžete použít dynamická `Page` vlastnost, která má přístup ke stejným datům, jako v následujícím příkladu:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Rozložení stránky) Vykreslí obsah stránky obsahu, která se nenachází v žádné části s názvem.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Vykreslí stránku obsahu pomocí zadané cesty a volitelné doplňující data. Můžete získat hodnoty parametrů navíc z `PageData` pozice (třeba 1) nebo klíče (Příklad 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Rozložení stránky) Vykreslí obsah oddíl, který má název. Nastavit *požadované* na hodnotu false, aby oddíl volitelné.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Získá nebo nastaví hodnotu souboru cookie HTTP.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Získá soubory, které byly odeslány v aktuální žádosti.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Získá data, která byla ve formuláři odeslány (jako řetězce). `Request[key]` kontroluje, jak `Request.Form` a `Request.QueryString` kolekce.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Získá data, která byla zadaná v řetězci dotazu adresy URL. `Request[key]` kontroluje, jak `Request.Form` a `Request.QueryString` kolekce.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Zakáže selektivně žádosti o ověření pro form element, hodnoty řetězce dotazu, soubor cookie nebo hodnotu hlavičky. Ověření žádosti je ve výchozím nastavení povolené a zabrání uživatelům v publikování kódu nebo jiný potenciálně nebezpečný obsah.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Přidá do odpovědi hlavičku HTTP serveru.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Ukládá do mezipaměti výstup stránky po určitou dobu. Volitelně můžete nastavit *klouzavé* resetovat vypršení časového limitu na každé stránce přístup a *varyByParams* pro ukládání do mezipaměti různé verze stránky pro každý řetězec jiný dotaz v požadavku stránky.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Přesměruje požadavek prohlížeče do nového umístění.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Nastaví stavový kód protokolu HTTP, který je odesláno prohlížeči.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Zapíše obsah *data* do odpovědi volitelné typu MIME.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Zapíše obsah souboru do odpovědi.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Rozložení stránky) Definuje oddíl obsahu, který má název.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Dekóduje řetězec, který není kódován jazykem HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Kóduje řetězce pro vykreslování v kódu HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Vrací fyzickou cestu serveru pro zadanou virtuální cestu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Dekóduje text z adresy URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Zakóduje text uvést v adrese URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Získá nebo nastaví hodnotu, která existuje, dokud uživatel nezavře prohlížeč.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Zobrazí řetězcovou reprezentaci objektu hodnoty.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Získá doplňující data z adresy URL (například */MyPage/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Změní heslo pro zadaného uživatele.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Potvrdí na účtu pomocí účtu potvrzovací token.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Vytvoří nový uživatelský účet pomocí zadaného uživatelského jména a hesla. Pokud chcete vyžadovat potvrzovací token, předá hodnotu true pro *requireConfirmationToken.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Získá identifikátor celé číslo pro aktuálně přihlášeného uživatele.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Získá název pro aktuálně přihlášeného uživatele.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Generuje token pro resetování hesla, který lze poslat e-mailem uživateli, aby uživatel můžete resetovat heslo.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Vrátí ID uživatele uživatelského jména.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Vrátí hodnotu true Pokud je aktuální uživatel je přihlášen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Vrátí hodnotu true Pokud je uživatel potvrzený (například prostřednictvím e-mail s potvrzením.).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Vrátí hodnotu true Pokud aktuální uživatelské jméno odpovídá zadanému uživatelskému jménu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Přihlásí uživatele v nastavením ověřovací token v souboru cookie.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Zaznamená uživatele odhlašování odebráním tokenu souboru cookie pro ověřování.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Pokud není uživatel ověřen, nastaví stav protokolu HTTP na 401 (Neautorizováno).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Pokud má aktuální uživatel není členem jedné ze zadaných rolí, nastaví stav protokolu HTTP na 401 (Neautorizováno).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Pokud má aktuální uživatel není uživatel určený parametrem *uživatelské jméno*, nastaví stav protokolu HTTP na 401 (Neautorizováno).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Pokud je platný token pro resetování hesla, změní heslo uživatele k nové heslo.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Data

### `Database.Execute(SQLstatement [,parameters]`

Provede *SQLstatement* (s volitelné parametry) jako je například vložení, DELETE nebo UPDATE a vrací počet příslušné záznamy.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Vrátí sloupec identity z naposledy vloženého řádku.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Otevře se soubor zadaná databáze nebo databáze pomocí pojmenovaného připojovacího řetězce z *Web.config* souboru.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Otevře se v databázi pomocí připojovacího řetězce. (Tím se liší od `Database.Open`, který používá název připojovacího řetězce.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Dotazuje databázi pomocí *SQLstatement* (volitelně předávání parametrů) a vrátí výsledky jako kolekce.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Provede *SQLstatement* (s volitelné parametry) a vrátí jeden záznam.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Provede *SQLstatement* (s volitelné parametry) a vrátí jednu hodnotu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Pomocné rutiny

### `Analytics.GetGoogleHtml(webPropertyId)`

Vykreslí kód jazyka JavaScript Google Analytics pro zadané ID.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Vykreslí kód StatCounter Analytics JavaScript zadaného projektu.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Vykreslí kód Yahoo Analytics JavaScript pro zadaný účet.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Hledání předá do služby Bing. Chcete-li zadat webový server a název vyhledávacího pole hledání, můžete nastavit `Bing.SiteUrl` a `Bing.SiteTitle` vlastnosti. Za normálních okolností nastavte tyto vlastnosti  *\_AppStart* stránky.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Inicializuje grafu.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Přidá do grafu legendu.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Přidá do grafu řady hodnot.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Vrátí hodnotu hash pro zadaná data. Výchozí algoritmus `sha256`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Umožňuje uživatelům Facebook se připojte k stránky.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Vykreslí uživatelského rozhraní pro nahrávání souborů.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Vykreslí zadaný Xbox hráči značku.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Vykreslí zadané e-mailovou adresu pro Gravatar bitovou kopii.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Převede datový objekt na řetězec ve formátu JavaScript Object Notation (JSON).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Převede řetězec vstupní zakódovaná ve formátu JSON na datový objekt, který můžete iterace v nebo můžete vložit do databáze.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Vykreslí sociálních sítí odkazuje pomocí zadaný název a volitelný adresy URL.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Přidruží chybovou zprávu pole formuláře. Použití `ModelState` pomocné rutiny pro přístup k tomuto členu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Přidruží chybovou zprávu formuláře. Použití `ModelState` pomocné rutiny pro přístup k tomuto členu.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Vrátí hodnotu true Pokud nejsou žádné chyby ověření. Použití `ModelState` pomocné rutiny pro přístup k tomuto členu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Vykreslí vlastnosti a hodnoty objekt a všechny podřízené objekty.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Vykreslí ověřovací test nástroje reCAPTCHA.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Nastaví veřejné a soukromé klíče pro službu nástroje reCAPTCHA. Za normálních okolností nastavte tyto vlastnosti  *\_AppStart* stránky.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Vrátí výsledek testu nástroje reCAPTCHA.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Vykreslí stavové informace o rozhraní ASP.NET Web Pages.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Vykreslí Twitter datového proudu pro zadaného uživatele.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Vykreslí Twitter datového proudu pro zadaný hledaný text.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Vykreslí Flash přehrávání videa pro zadaného souboru s volitelné šířku a výšku.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Vykreslí Windows Media player pro zadaného souboru s volitelné šířku a výšku.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Vykreslí přehrávač Silverlight pro zadaný *.xap* soubor s požadované šířku a výšku.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Vrací objekt určeného *klíč*, nebo hodnota null, pokud objekt nebyl nalezen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Odebere objekt určeného *klíč* z mezipaměti.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Vloží *hodnotu* do mezipaměti pod názvem určeného *klíč*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Vytvoří nový `WebGrid` pomocí dat z dotazu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Vykreslí značku pro zobrazení dat v tabulce jazyka HTML.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Vykreslí pager pro `WebGrid` objektu.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Načte obrázek ze zadané cesty.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Přidá zadanou bitovou kopii jako vodoznak.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Přidá zadaný text do bitové kopie.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Převrátí bitovou kopii vodorovně nebo svisle.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Načte obrázek při odeslání bitovou kopii na stránku během nahrávání souborů.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Změní velikost bitovou kopii.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Otočí obrázek vlevo nebo vpravo.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Uloží obrázek na zadanou cestu.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Nastaví heslo pro SMTP server. Za normálních okolností, můžete tuto vlastnost nastavit v  *\_AppStart* stránky.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Odešle e-mailovou zprávu.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Nastaví název serveru SMTP. Za normálních okolností, můžete tuto vlastnost nastavit v<em>\_AppStart</em> stránky.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Nastaví uživatelské jméno pro SMTP server. Za normálních okolností byste měli nastavit tuto vlastnost v  *\_AppStart* stránky.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Ověřování

### `Html.ValidationMessage(field)`

(v2) Vykreslí chybovou zprávu ověření pro zadané pole.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

(v2) Zobrazí seznam všech chyb ověření.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

(v2) Zaregistruje element vstupu uživatele pro zadaný typ ověření.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

(v2) Dynamicky vykreslí atributy třídy CSS pro ověřování na straně klienta, tak, že chybových zpráv ověření. (Vyžaduje odkazovat na příslušné klientského skriptu knihovny a definování tříd CSS.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

(v2) Umožňuje ověření na straně klienta pro vstupní pole uživatele. (Vyžaduje odkazovat na knihovny odpovídající klientského skriptu.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

(v2) Pokud vrátí hodnotu true všechny elementy vstupu uživatele, které jsou registrovaných pro ověření obsahovat platné hodnoty.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

(v2) Určuje, že uživatelé musí zadat hodnotu pro daný element vstupu uživatele.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

(v2) Určuje, že uživatelé musí zadat hodnoty pro jednotlivé elementy vstupu uživatele. Tato metoda neumožňuje zadat vlastní chybové zprávy.

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

(v2) Určuje ověřovací test, při použití `Validation.Add` metoda.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
