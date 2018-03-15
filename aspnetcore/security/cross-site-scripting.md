---
title: "Brání mezi skriptování (XSS) v ASP.NET Core"
author: rick-anderson
description: "Další informace o webů Skriptování a techniky pro vyřešení této chyby zabezpečení v aplikaci ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cross-site-scripting
ms.openlocfilehash: 9e54ee0b1169c01629c3cd91a378509a73c53904
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="preventing-cross-site-scripting-xss-in-aspnet-core"></a><span data-ttu-id="d1b94-103">Brání mezi skriptování (XSS) v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d1b94-103">Preventing Cross-Site Scripting (XSS) in ASP.NET Core</span></span>

<span data-ttu-id="d1b94-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d1b94-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d1b94-105">Webů Skriptování je ohrožení zabezpečení, která umožňuje útočníkovi uložit skripty na straně klienta (obvykle JavaScript) do webové stránky.</span><span class="sxs-lookup"><span data-stu-id="d1b94-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="d1b94-106">Když ostatní uživatelé načíst příslušných stránek, které budou spuštěny skripty útočníci, by ji ke krádeži soubory cookie a relace tokeny, změnit obsah webové stránky prostřednictvím DOM manipulaci nebo přesměrovat na jinou stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="d1b94-106">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="d1b94-107">Skriptování XSS obecně nastane, když aplikace přijímá vstup uživatele a uloží jej na stránce bez ověřování, kódování nebo ho uvozovací znaky.</span><span class="sxs-lookup"><span data-stu-id="d1b94-107">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="d1b94-108">Ochrana proti XSS vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="d1b94-108">Protecting your application against XSS</span></span>

<span data-ttu-id="d1b94-109">Na základní úrovni XSS funguje tak, přičemž aplikace do vkládání `<script>` značka do vykreslované stránky, nebo vložením `On*` událostí do elementu.</span><span class="sxs-lookup"><span data-stu-id="d1b94-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="d1b94-110">Vývojáři by měl použít následující kroky prevence aby nedošlo k zavedení XSS do své aplikace.</span><span class="sxs-lookup"><span data-stu-id="d1b94-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="d1b94-111">Nikdy uvést nedůvěryhodné data do váš vstup, HTML, pokud postupujte podle zbývajících kroků.</span><span class="sxs-lookup"><span data-stu-id="d1b94-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="d1b94-112">Nedůvěryhodné data jsou všechna data, která může být řízené útočník vstupy formuláře HTML, řetězce dotazu, hlaviček protokolu HTTP, i data source z databáze, protože útočník může být schopný porušení vaší databáze, i když jejich nelze porušení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d1b94-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="d1b94-113">Před přepnutím nedůvěryhodné data uvnitř elementu HTML ověřte, zda že se kódovaný jazykem HTML.</span><span class="sxs-lookup"><span data-stu-id="d1b94-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="d1b94-114">Kódování HTML trvá znaky, jako &lt; a změní jejich do bezpečného formuláře jako &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="d1b94-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="d1b94-115">Před uvedením nedůvěryhodné data do atribut HTML ověřte, zda že se kódování atributů HTML.</span><span class="sxs-lookup"><span data-stu-id="d1b94-115">Before putting untrusted data into an HTML attribute ensure it's HTML attribute encoded.</span></span> <span data-ttu-id="d1b94-116">Kódování atributu HTML je nadmnožinou kódování HTML a kóduje dalšími znaky, jako "a".</span><span class="sxs-lookup"><span data-stu-id="d1b94-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="d1b94-117">Před přepnutím nedůvěryhodné data do jazyka JavaScript umístíte data v elementu HTML, jehož obsah je načíst za běhu.</span><span class="sxs-lookup"><span data-stu-id="d1b94-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="d1b94-118">Pokud to není možné zkontrolujte data je zakódován JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d1b94-118">If this isn't possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="d1b94-119">Kódování JavaScript trvá nebezpečné znaky pro jazyk JavaScript a nahradí je jejich šestnáctkově, například &lt; by kódovaná jako `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="d1b94-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="d1b94-120">Před uvedením nedůvěryhodné data do řetězce dotazu adresy URL Ujistěte se, že je kódovaná adresou URL.</span><span class="sxs-lookup"><span data-stu-id="d1b94-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="d1b94-121">Kódování HTML pomocí syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="d1b94-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="d1b94-122">Použít v MVC automaticky modul Razor zakóduje všechny výstup jako zdroj proměnné, pokud pracujete skutečně pevného zabránit, aby ji tak.</span><span class="sxs-lookup"><span data-stu-id="d1b94-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="d1b94-123">Ji používá atribut HTML kódování pravidla při každém použití  *@*  – direktiva.</span><span class="sxs-lookup"><span data-stu-id="d1b94-123">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="d1b94-124">Ve formátu HTML kódování atributu je nadmnožinou kódování HTML, znamená to, že nemusíte sami se týkají s jestli byste měli používat kódování HTML nebo kódování atributu HTML.</span><span class="sxs-lookup"><span data-stu-id="d1b94-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="d1b94-125">Je nutné zajistit, že používáte pouze v kontextu HTML při pokusu o vložení nedůvěryhodné vstup přímo do jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d1b94-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="d1b94-126">Pomocníci značka bude také zakódovat vstup, které můžete použít v parametrech značky.</span><span class="sxs-lookup"><span data-stu-id="d1b94-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="d1b94-127">Proveďte následující zobrazení syntaxe Razor;</span><span class="sxs-lookup"><span data-stu-id="d1b94-127">Take the following Razor view;</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="d1b94-128">Tato zobrazení výstupu obsah *untrustedInput* proměnné.</span><span class="sxs-lookup"><span data-stu-id="d1b94-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="d1b94-129">Tato proměnná zahrnuje některé znaky, které se používají v útoky XSS, konkrétně &lt;, "a &gt;.</span><span class="sxs-lookup"><span data-stu-id="d1b94-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="d1b94-130">Zkoumání zdroj zobrazuje vykreslené výstup kódovaná jako:</span><span class="sxs-lookup"><span data-stu-id="d1b94-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="d1b94-131">ASP.NET MVC základní poskytuje `HtmlString` třídy, který není kódován automaticky při výstupu.</span><span class="sxs-lookup"><span data-stu-id="d1b94-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="d1b94-132">To by nikdy použít v kombinaci s nedůvěryhodné vstup jako to zveřejní XSS ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="d1b94-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="d1b94-133">Kódování JavaScript pomocí syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="d1b94-133">Javascript Encoding using Razor</span></span>

<span data-ttu-id="d1b94-134">Může nastat situace, který chcete vložit hodnotu do jazyka JavaScript ke zpracování v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d1b94-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="d1b94-135">Chcete-li to provést dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="d1b94-135">There are two ways to do this.</span></span> <span data-ttu-id="d1b94-136">Nejbezpečnější způsob, jak vložit jednoduchý hodnoty je hodnota umístit do atribut dat značky a načíst ve vašem JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d1b94-136">The safest way to insert simple values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="d1b94-137">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d1b94-137">For example:</span></span>

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

<span data-ttu-id="d1b94-138">To způsobí následující HTML</span><span class="sxs-lookup"><span data-stu-id="d1b94-138">This will produce the following HTML</span></span>

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

<span data-ttu-id="d1b94-139">Které, když je spuštěna, vykreslí takto:</span><span class="sxs-lookup"><span data-stu-id="d1b94-139">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="d1b94-140">Můžete také zavolat kodér JavaScript přímo,</span><span class="sxs-lookup"><span data-stu-id="d1b94-140">You can also call the JavaScript encoder directly,</span></span>

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

<span data-ttu-id="d1b94-141">To bude vykreslení v prohlížeči takto:</span><span class="sxs-lookup"><span data-stu-id="d1b94-141">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="d1b94-142">Nedůvěryhodné vstup v jazyce JavaScript k vytváření prvků modelu DOM není zřetězení.</span><span class="sxs-lookup"><span data-stu-id="d1b94-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="d1b94-143">Měli byste použít `createElement()` a přiřadit odpovídajícím způsobem, jako hodnoty vlastností `node.TextContent=`, nebo použijte `element.SetAttribute()` / `element[attribute]=` jinak sami umístěte do založené na modelu DOM XSS.</span><span class="sxs-lookup"><span data-stu-id="d1b94-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="d1b94-144">Přístup k kodéry v kódu</span><span class="sxs-lookup"><span data-stu-id="d1b94-144">Accessing encoders in code</span></span>

<span data-ttu-id="d1b94-145">Jsou k dispozici kódu dvěma způsoby kodéry HTML, JavaScript a adresu URL, můžete vložit pomocí [vkládání závislostí](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) nebo můžete použít výchozí kodéry, obsažené v `System.Text.Encodings.Web` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d1b94-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="d1b94-146">Pokud použijete výchozí kodéry pak veškeré použité k znak rozsahy považován za bezpečné se neprojeví – výchozí kodéry nejbezpečnější kódování pravidla možné použít.</span><span class="sxs-lookup"><span data-stu-id="d1b94-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="d1b94-147">Použít konfigurovatelná kodéry prostřednictvím DI vaší konstruktory provést *HtmlEncoder*, *JavaScriptEncoder* a *UrlEncoder* parametr podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="d1b94-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="d1b94-148">Například;</span><span class="sxs-lookup"><span data-stu-id="d1b94-148">For example;</span></span>

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

## <a name="encoding-url-parameters"></a><span data-ttu-id="d1b94-149">Kódování URL parametry</span><span class="sxs-lookup"><span data-stu-id="d1b94-149">Encoding URL Parameters</span></span>

<span data-ttu-id="d1b94-150">Pokud chcete vytvořit adresu URL řetězec dotazu s nedůvěryhodné vstup jako hodnotu pomocí `UrlEncoder` ke kódování hodnota.</span><span class="sxs-lookup"><span data-stu-id="d1b94-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="d1b94-151">Například</span><span class="sxs-lookup"><span data-stu-id="d1b94-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="d1b94-152">Po kódování encodedValue bude obsahovat proměnné `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="d1b94-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="d1b94-153">Mezery, uvozovky, interpunkce a další nebezpečné znaky bude procenta kódovaný k jejich šestnáctkové hodnoty, například znak mezery bude % 20.</span><span class="sxs-lookup"><span data-stu-id="d1b94-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="d1b94-154">Nepoužívejte nedůvěryhodné vstup jako část cesty adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d1b94-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="d1b94-155">Vždycky předáte nedůvěryhodné vstup jako hodnotu řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="d1b94-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="d1b94-156">Přizpůsobení kodéry</span><span class="sxs-lookup"><span data-stu-id="d1b94-156">Customizing the Encoders</span></span>

<span data-ttu-id="d1b94-157">Ve výchozím nastavení kodéry pomocí seznamu bezpečných adres nesmí být v rozsahu základní Latinské kódování Unicode a kódování všechny znaky mimo tento rozsah jako jejich ekvivalenty u kódu znaku.</span><span class="sxs-lookup"><span data-stu-id="d1b94-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="d1b94-158">Toto chování také ovlivní Razor TagHelper a HtmlHelper vykreslování, jak se bude používat kodéry pro výstup vaší řetězce.</span><span class="sxs-lookup"><span data-stu-id="d1b94-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="d1b94-159">Zdůvodnění to je pro ochranu proti chyby neznámý nebo budoucí prohlížeče (předchozí chyby prohlížeče mít přerušovačů až analýza podle zpracování neanglických znaků).</span><span class="sxs-lookup"><span data-stu-id="d1b94-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="d1b94-160">Pokud váš web z umístění provede výrazně využívá jiné znaky než latinku, jako je například čínština, cyrilici nebo jiné toto není pravděpodobně chování, které chcete.</span><span class="sxs-lookup"><span data-stu-id="d1b94-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="d1b94-161">Můžete přizpůsobit bezpečné seznamy kodér zahrnout rozsahy vhodné pro vaši aplikaci při spuštění, v kódu Unicode `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="d1b94-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="d1b94-162">Například pomocí výchozí konfigurace můžete použít Razor HtmlHelper takto;</span><span class="sxs-lookup"><span data-stu-id="d1b94-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="d1b94-163">Při zobrazení zdroji webové stránky uvidíte, že je vykreslena následujícím způsobem s čínštině kódovaný;</span><span class="sxs-lookup"><span data-stu-id="d1b94-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="d1b94-164">Rozšíří znaky považované za bezpečné pomocí kodéru by vložení následující řádek do `ConfigureServices()` metoda v `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="d1b94-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="d1b94-165">Tento příklad rozšiřuje seznamu bezpečných zahrnout CjkUnifiedIdeographs rozsah kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="d1b94-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="d1b94-166">Nyní by se stala Vykreslený výstup</span><span class="sxs-lookup"><span data-stu-id="d1b94-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="d1b94-167">Seznamu bezpečných adres rozsahy jsou zadané jako grafy kódu Unicode, ne jazyky.</span><span class="sxs-lookup"><span data-stu-id="d1b94-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="d1b94-168">[Standardu Unicode](http://unicode.org/) obsahuje seznam [code grafy](http://www.unicode.org/charts/index.html) můžete použít k vyhledání grafu obsahující vaše znaky.</span><span class="sxs-lookup"><span data-stu-id="d1b94-168">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="d1b94-169">Každý kodér, Html, JavaScript a adresu Url, musí být nakonfigurované samostatně.</span><span class="sxs-lookup"><span data-stu-id="d1b94-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="d1b94-170">Přizpůsobení seznamu bezpečných adres ovlivňuje pouze kodéry Source prostřednictvím DI.</span><span class="sxs-lookup"><span data-stu-id="d1b94-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="d1b94-171">Pokud je přímý přístup k kodér prostřednictvím `System.Text.Encodings.Web.*Encoder.Default` pak výchozí základní Latinské se použije pouze safelist.</span><span class="sxs-lookup"><span data-stu-id="d1b94-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="d1b94-172">Kam by měl být kódování provést?</span><span class="sxs-lookup"><span data-stu-id="d1b94-172">Where should encoding take place?</span></span>

<span data-ttu-id="d1b94-173">Obecné přijme, postup je, že kódování probíhá v místě výstup a kódovaného hodnoty by měly být nikdy uložené v databázi.</span><span class="sxs-lookup"><span data-stu-id="d1b94-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="d1b94-174">Kódování v místě výstup umožňuje změnit použití dat, například z HTML na hodnotu řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="d1b94-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="d1b94-175">Také umožňuje snadno hledání dat bez nutnosti ke kódování hodnoty před vyhledáváním a umožňuje využít výhod provedené změny a opravy chyb provedené kodérů.</span><span class="sxs-lookup"><span data-stu-id="d1b94-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="d1b94-176">Ověření jako zabránění techniku XSS</span><span class="sxs-lookup"><span data-stu-id="d1b94-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="d1b94-177">Ověření může být užitečné nástroje omezení útoky XSS.</span><span class="sxs-lookup"><span data-stu-id="d1b94-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="d1b94-178">Například jednoduché číselných řetězců obsahující pouze znaky 0-9 nespustí XSS útoku.</span><span class="sxs-lookup"><span data-stu-id="d1b94-178">For example, a simple numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="d1b94-179">Ověření se stane složitější měli, která chcete přijímat HTML vstup uživatele - analýza elementu input kódu HTML je obtížné, pokud není možné.</span><span class="sxs-lookup"><span data-stu-id="d1b94-179">Validation becomes more complicated should you wish to accept HTML in user input - parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="d1b94-180">Markdownu a dalších formátů text může být bezpečnější možnost pro bohaté vstup.</span><span class="sxs-lookup"><span data-stu-id="d1b94-180">MarkDown and other text formats would be a safer option for rich input.</span></span> <span data-ttu-id="d1b94-181">Se nikdy spoléhají na ověření samostatně.</span><span class="sxs-lookup"><span data-stu-id="d1b94-181">You should never rely on validation alone.</span></span> <span data-ttu-id="d1b94-182">Vždy kódování nedůvěryhodné vstup před výstup, bez ohledu na to, jaké ověření jste provedli.</span><span class="sxs-lookup"><span data-stu-id="d1b94-182">Always encode untrusted input before output, no matter what validation you have performed.</span></span>
