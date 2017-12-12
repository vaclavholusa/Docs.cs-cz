---
title: "Brání skriptování mezi servery"
author: rick-anderson
description: "Toto téma představuje webů Skriptování a techniky pro vyřešení této chyby zabezpečení v aplikaci ASP.NET Core."
keywords: "ASP.NET Core, XSS, ohrožení zabezpečení"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 95790927-2bfe-445e-b1fd-429c2c7030ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cross-site-scripting
ms.openlocfilehash: fdb26a8338b98135cfc3f6bce9d87285e9a7eb12
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="preventing-cross-site-scripting"></a><span data-ttu-id="dc900-104">Brání skriptování mezi servery</span><span class="sxs-lookup"><span data-stu-id="dc900-104">Preventing Cross-Site Scripting</span></span>

<span data-ttu-id="dc900-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dc900-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dc900-106">Webů Skriptování je ohrožení zabezpečení, která umožňuje útočníkovi uložit skripty na straně klienta (obvykle JavaScript) do webové stránky.</span><span class="sxs-lookup"><span data-stu-id="dc900-106">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="dc900-107">Když ostatní uživatelé načíst příslušných stránek, které budou spuštěny skripty útočníci, by ji ke krádeži soubory cookie a relace tokeny, změnit obsah webové stránky prostřednictvím DOM manipulaci nebo přesměrovat na jinou stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="dc900-107">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="dc900-108">Skriptování XSS obecně nastane, když aplikace přijímá vstup uživatele a uloží jej na stránce bez ověřování, kódování nebo ho uvozovací znaky.</span><span class="sxs-lookup"><span data-stu-id="dc900-108">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="dc900-109">Ochrana proti XSS vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="dc900-109">Protecting your application against XSS</span></span>

<span data-ttu-id="dc900-110">Na základní úrovni XSS funguje tak, přičemž aplikace do vkládání `<script>` značka do vykreslované stránky, nebo vložením `On*` událostí do elementu.</span><span class="sxs-lookup"><span data-stu-id="dc900-110">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="dc900-111">Vývojáři by měl použít následující kroky prevence aby nedošlo k zavedení XSS do své aplikace.</span><span class="sxs-lookup"><span data-stu-id="dc900-111">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="dc900-112">Nikdy uvést nedůvěryhodné data do váš vstup, HTML, pokud postupujte podle zbývajících kroků.</span><span class="sxs-lookup"><span data-stu-id="dc900-112">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="dc900-113">Nedůvěryhodné data jsou všechna data, která může být řízené útočník vstupy formuláře HTML, řetězce dotazu, hlaviček protokolu HTTP, i data source z databáze, protože útočník může být schopný porušení vaší databáze, i když jejich nelze porušení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="dc900-113">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="dc900-114">Před přepnutím nedůvěryhodné data uvnitř elementu HTML ověřte, zda že se kódovaný jazykem HTML.</span><span class="sxs-lookup"><span data-stu-id="dc900-114">Before putting untrusted data inside an HTML element ensure it is HTML encoded.</span></span> <span data-ttu-id="dc900-115">Kódování HTML trvá znaky, jako &lt; a změní jejich do bezpečného formuláře jako &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="dc900-115">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="dc900-116">Před uvedením nedůvěryhodné data do atribut HTML ověřte, zda že se kódování atributů HTML.</span><span class="sxs-lookup"><span data-stu-id="dc900-116">Before putting untrusted data into an HTML attribute ensure it is HTML attribute encoded.</span></span> <span data-ttu-id="dc900-117">Kódování atributu HTML je nadmnožinou kódování HTML a kóduje dalšími znaky, jako "a".</span><span class="sxs-lookup"><span data-stu-id="dc900-117">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="dc900-118">Před přepnutím nedůvěryhodné data do jazyka JavaScript umístíte data v elementu HTML, jehož obsah je načíst za běhu.</span><span class="sxs-lookup"><span data-stu-id="dc900-118">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="dc900-119">Pokud to není možné zkontrolujte data je zakódován JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dc900-119">If this is not possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="dc900-120">Kódování JavaScript trvá nebezpečné znaky pro jazyk JavaScript a nahradí je jejich šestnáctkově, například &lt; by kódovaná jako `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="dc900-120">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="dc900-121">Před uvedením nedůvěryhodné data do řetězce dotazu adresy URL Ujistěte se, že je kódovaná adresou URL.</span><span class="sxs-lookup"><span data-stu-id="dc900-121">Before putting untrusted data into a URL query string ensure it is URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="dc900-122">Kódování HTML pomocí syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="dc900-122">HTML Encoding using Razor</span></span>

<span data-ttu-id="dc900-123">Použít v MVC automaticky modul Razor zakóduje všechny výstup jako zdroj proměnné, pokud pracujete skutečně pevného zabránit, aby ji tak.</span><span class="sxs-lookup"><span data-stu-id="dc900-123">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="dc900-124">Ji používá atribut HTML kódování pravidla při každém použití  *@*  – direktiva.</span><span class="sxs-lookup"><span data-stu-id="dc900-124">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="dc900-125">Ve formátu HTML kódování atributu je nadmnožinou kódování HTML, znamená to, že nemusíte sami se týkají s jestli byste měli používat kódování HTML nebo kódování atributu HTML.</span><span class="sxs-lookup"><span data-stu-id="dc900-125">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="dc900-126">Je nutné zajistit, že používáte pouze v kontextu HTML při pokusu o vložení nedůvěryhodné vstup přímo do jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dc900-126">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="dc900-127">Pomocníci značka bude také zakódovat vstup, které můžete použít v parametrech značky.</span><span class="sxs-lookup"><span data-stu-id="dc900-127">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="dc900-128">Proveďte následující zobrazení syntaxe Razor;</span><span class="sxs-lookup"><span data-stu-id="dc900-128">Take the following Razor view;</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="dc900-129">Tato zobrazení výstupu obsah *untrustedInput* proměnné.</span><span class="sxs-lookup"><span data-stu-id="dc900-129">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="dc900-130">Tato proměnná zahrnuje některé znaky, které se používají v útoky XSS, konkrétně &lt;, "a &gt;.</span><span class="sxs-lookup"><span data-stu-id="dc900-130">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="dc900-131">Zkoumání zdroj zobrazuje vykreslené výstup kódovaná jako:</span><span class="sxs-lookup"><span data-stu-id="dc900-131">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="dc900-132">ASP.NET MVC základní poskytuje `HtmlString` třídy, který není kódován automaticky při výstupu.</span><span class="sxs-lookup"><span data-stu-id="dc900-132">ASP.NET Core MVC provides an `HtmlString` class which is not automatically encoded upon output.</span></span> <span data-ttu-id="dc900-133">To by nikdy použít v kombinaci s nedůvěryhodné vstup jako to zveřejní XSS ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="dc900-133">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="dc900-134">Kódování JavaScript pomocí syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="dc900-134">Javascript Encoding using Razor</span></span>

<span data-ttu-id="dc900-135">Může nastat situace, který chcete vložit hodnotu do jazyka JavaScript ke zpracování v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dc900-135">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="dc900-136">Chcete-li to provést dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="dc900-136">There are two ways to do this.</span></span> <span data-ttu-id="dc900-137">Nejbezpečnější způsob, jak vložit jednoduchý hodnoty je hodnota umístit do atribut dat značky a načíst ve vašem JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dc900-137">The safest way to insert simple values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="dc900-138">Příklad:</span><span class="sxs-lookup"><span data-stu-id="dc900-138">For example:</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="dc900-139">To způsobí následující HTML</span><span class="sxs-lookup"><span data-stu-id="dc900-139">This will produce the following HTML</span></span>

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="dc900-140">Které, když je spuštěna, vykreslí takto:</span><span class="sxs-lookup"><span data-stu-id="dc900-140">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="dc900-141">Můžete také zavolat kodér JavaScript přímo,</span><span class="sxs-lookup"><span data-stu-id="dc900-141">You can also call the JavaScript encoder directly,</span></span>

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="dc900-142">To bude vykreslení v prohlížeči takto:</span><span class="sxs-lookup"><span data-stu-id="dc900-142">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="dc900-143">Řetězení není nedůvěryhodné vstup v jazyce JavaScript k vytvoření DOM elementů.</span><span class="sxs-lookup"><span data-stu-id="dc900-143">Do not concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="dc900-144">Měli byste použít `createElement()` a přiřadit odpovídajícím způsobem, jako hodnoty vlastností `node.TextContent=`, nebo použijte `element.SetAttribute()` / `element[attribute]=` jinak sami umístěte do založené na modelu DOM XSS.</span><span class="sxs-lookup"><span data-stu-id="dc900-144">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="dc900-145">Přístup k kodéry v kódu</span><span class="sxs-lookup"><span data-stu-id="dc900-145">Accessing encoders in code</span></span>

<span data-ttu-id="dc900-146">Jsou k dispozici kódu dvěma způsoby kodéry HTML, JavaScript a adresu URL, můžete vložit pomocí [vkládání závislostí](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) nebo můžete použít výchozí kodéry, obsažené v `System.Text.Encodings.Web` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="dc900-146">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="dc900-147">Pokud použijete výchozí kodéry pak veškeré použité na rozsahy znak považován za bezpečné se neprojeví, – výchozí kodéry nejbezpečnější kódování pravidla možné použít.</span><span class="sxs-lookup"><span data-stu-id="dc900-147">If you use the default encoders then any  you applied to character ranges to be treated as safe will not take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="dc900-148">Použít konfigurovatelná kodéry prostřednictvím DI vaší konstruktory provést *HtmlEncoder*, *JavaScriptEncoder* a *UrlEncoder* parametr podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="dc900-148">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="dc900-149">Například;</span><span class="sxs-lookup"><span data-stu-id="dc900-149">For example;</span></span>

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a><span data-ttu-id="dc900-150">Kódování URL parametry</span><span class="sxs-lookup"><span data-stu-id="dc900-150">Encoding URL Parameters</span></span>

<span data-ttu-id="dc900-151">Pokud chcete vytvořit adresu URL řetězec dotazu s nedůvěryhodné vstup jako hodnotu pomocí `UrlEncoder` ke kódování hodnota.</span><span class="sxs-lookup"><span data-stu-id="dc900-151">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="dc900-152">Například</span><span class="sxs-lookup"><span data-stu-id="dc900-152">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="dc900-153">Po kódování encodedValue bude obsahovat proměnné `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="dc900-153">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="dc900-154">Mezery, uvozovky, interpunkce a další nebezpečné znaky bude procenta kódovaný k jejich šestnáctkové hodnoty, například znak mezery bude % 20.</span><span class="sxs-lookup"><span data-stu-id="dc900-154">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="dc900-155">Nepoužívejte nedůvěryhodné vstup jako část cesty adresy URL.</span><span class="sxs-lookup"><span data-stu-id="dc900-155">Do not use untrusted input as part of a URL path.</span></span> <span data-ttu-id="dc900-156">Vždycky předáte nedůvěryhodné vstup jako hodnotu řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="dc900-156">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="dc900-157">Přizpůsobení kodéry</span><span class="sxs-lookup"><span data-stu-id="dc900-157">Customizing the Encoders</span></span>

<span data-ttu-id="dc900-158">Ve výchozím nastavení kodéry pomocí seznamu bezpečných adres nesmí být v rozsahu základní Latinské kódování Unicode a kódování všechny znaky mimo tento rozsah jako jejich ekvivalenty u kódu znaku.</span><span class="sxs-lookup"><span data-stu-id="dc900-158">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="dc900-159">Toto chování také ovlivní Razor TagHelper a HtmlHelper vykreslování, jak se bude používat kodéry pro výstup vaší řetězce.</span><span class="sxs-lookup"><span data-stu-id="dc900-159">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="dc900-160">Zdůvodnění to je pro ochranu proti chyby neznámý nebo budoucí prohlížeče (předchozí chyby prohlížeče mít přerušovačů až analýza podle zpracování neanglických znaků).</span><span class="sxs-lookup"><span data-stu-id="dc900-160">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="dc900-161">Pokud váš web z umístění provede výrazně využívá jiné znaky než latinku, jako je například čínština, cyrilici nebo jiné toto není pravděpodobně chování, které chcete.</span><span class="sxs-lookup"><span data-stu-id="dc900-161">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="dc900-162">Můžete přizpůsobit bezpečné seznamy kodér zahrnout rozsahy vhodné pro vaši aplikaci při spuštění, v kódu Unicode `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="dc900-162">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="dc900-163">Například pomocí výchozí konfigurace můžete použít Razor HtmlHelper takto;</span><span class="sxs-lookup"><span data-stu-id="dc900-163">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="dc900-164">Při zobrazení zdroji webové stránky uvidíte, že je vykreslena následujícím způsobem s čínštině kódovaný;</span><span class="sxs-lookup"><span data-stu-id="dc900-164">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="dc900-165">Rozšíří znaky považované za bezpečné pomocí kodéru by vložení následující řádek do `ConfigureServices()` metoda v `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="dc900-165">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="dc900-166">Tento příklad rozšiřuje seznamu bezpečných zahrnout CjkUnifiedIdeographs rozsah kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="dc900-166">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="dc900-167">Nyní by se stala Vykreslený výstup</span><span class="sxs-lookup"><span data-stu-id="dc900-167">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="dc900-168">Seznamu bezpečných adres rozsahy jsou zadané jako grafy kódu Unicode, ne jazyky.</span><span class="sxs-lookup"><span data-stu-id="dc900-168">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="dc900-169">[Standardu Unicode](http://unicode.org/) obsahuje seznam [code grafy](http://www.unicode.org/charts/index.html) můžete použít k vyhledání grafu obsahující vaše znaky.</span><span class="sxs-lookup"><span data-stu-id="dc900-169">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="dc900-170">Každý kodér, Html, JavaScript a adresu Url, musí být nakonfigurované samostatně.</span><span class="sxs-lookup"><span data-stu-id="dc900-170">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="dc900-171">Přizpůsobení seznamu bezpečných adres ovlivňuje pouze kodéry Source prostřednictvím DI.</span><span class="sxs-lookup"><span data-stu-id="dc900-171">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="dc900-172">Pokud je přímý přístup k kodér prostřednictvím `System.Text.Encodings.Web.*Encoder.Default` pak výchozí základní Latinské se použije pouze safelist.</span><span class="sxs-lookup"><span data-stu-id="dc900-172">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="dc900-173">Kam by měl být kódování provést?</span><span class="sxs-lookup"><span data-stu-id="dc900-173">Where should encoding take place?</span></span>

<span data-ttu-id="dc900-174">Obecné přijme, postup je, že kódování probíhá v místě výstup a kódovaného hodnoty by měly být nikdy uložené v databázi.</span><span class="sxs-lookup"><span data-stu-id="dc900-174">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="dc900-175">Kódování v místě výstup umožňuje změnit použití dat, například z HTML na hodnotu řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="dc900-175">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="dc900-176">Také umožňuje snadno hledání dat bez nutnosti ke kódování hodnoty před vyhledáváním a umožňuje využít výhod provedené změny a opravy chyb provedené kodérů.</span><span class="sxs-lookup"><span data-stu-id="dc900-176">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="dc900-177">Ověření jako zabránění techniku XSS</span><span class="sxs-lookup"><span data-stu-id="dc900-177">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="dc900-178">Ověření může být užitečné nástroje omezení útoky XSS.</span><span class="sxs-lookup"><span data-stu-id="dc900-178">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="dc900-179">Například jednoduché číselných řetězců obsahující pouze znaky 0-9 nespustí XSS útoku.</span><span class="sxs-lookup"><span data-stu-id="dc900-179">For example, a simple numeric string containing only the characters 0-9 will not trigger an XSS attack.</span></span> <span data-ttu-id="dc900-180">Ověření se stane složitější měli, která chcete přijímat HTML vstup uživatele - analýza elementu input kódu HTML je obtížné, pokud není možné.</span><span class="sxs-lookup"><span data-stu-id="dc900-180">Validation becomes more complicated should you wish to accept HTML in user input - parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="dc900-181">Markdownu a dalších formátů text může být bezpečnější možnost pro bohaté vstup.</span><span class="sxs-lookup"><span data-stu-id="dc900-181">MarkDown and other text formats would be a safer option for rich input.</span></span> <span data-ttu-id="dc900-182">Se nikdy spoléhají na ověření samostatně.</span><span class="sxs-lookup"><span data-stu-id="dc900-182">You should never rely on validation alone.</span></span> <span data-ttu-id="dc900-183">Vždy kódování nedůvěryhodné vstup před výstup, bez ohledu na to, jaké ověření jste provedli.</span><span class="sxs-lookup"><span data-stu-id="dc900-183">Always encode untrusted input before output, no matter what validation you have performed.</span></span>
