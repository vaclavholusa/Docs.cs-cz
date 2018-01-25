---
title: "Formátování dat odpovědi v aplikaci ASP.NET MVC jádra"
author: ardalis
description: "Zjistěte, jak k formátování data odpovědi v aplikaci ASP.NET MVC jádra."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ddda494e0db22031af9d20325e14e8458756cbfd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="44e63-103">Úvod k formátování data odpovědi v aplikaci ASP.NET MVC jádra</span><span class="sxs-lookup"><span data-stu-id="44e63-103">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="44e63-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="44e63-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="44e63-105">Jádro ASP.NET MVC má integrovanou podporu pro formátování data odpovědi pomocí pevné formáty nebo v reakci na požadavky klienta.</span><span class="sxs-lookup"><span data-stu-id="44e63-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="44e63-106">[Zobrazení nebo stažení ukázky z webu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span><span class="sxs-lookup"><span data-stu-id="44e63-106">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="44e63-107">Výsledky akce specifické pro formát</span><span class="sxs-lookup"><span data-stu-id="44e63-107">Format-Specific Action Results</span></span>

<span data-ttu-id="44e63-108">Některé typy výsledků akcí, jako jsou specifické pro konkrétní formátu, `JsonResult` a `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="44e63-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="44e63-109">Akce může vrátit konkrétní výsledky, které jsou vždy určitým způsobem.</span><span class="sxs-lookup"><span data-stu-id="44e63-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="44e63-110">Například vrácení `JsonResult` vrátí data ve formátu JSON, bez ohledu na to předvolby klienta.</span><span class="sxs-lookup"><span data-stu-id="44e63-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="44e63-111">Podobně vrácení `ContentResult` vrátí řetězec ve formátu prostého textu formátu dat (jak se jednoduše vrací řetězec).</span><span class="sxs-lookup"><span data-stu-id="44e63-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="44e63-112">Akce nemusí vrátit žádnou jednotlivou; MVC podporuje žádnou návratovou hodnotu objektu.</span><span class="sxs-lookup"><span data-stu-id="44e63-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="44e63-113">Akce vrátí-li `IActionResult` implementace a řadič dědí z `Controller`, vývojáři mají mnoho pomocné metody odpovídající mnoho z těchto možností.</span><span class="sxs-lookup"><span data-stu-id="44e63-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="44e63-114">Výsledky z akcí, které vracejí objekty, které nejsou `IActionResult` typy budou serializovány odpovídající pomocí `IOutputFormatter` implementace.</span><span class="sxs-lookup"><span data-stu-id="44e63-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="44e63-115">Vrátit data v konkrétním formátu z řadiče, která dědí z `Controller` základní třídy, použijte metodu integrovaných pomocných `Json` vrátit JSON a `Content` ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="44e63-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="44e63-116">Vaše metoda akce by měla vrátit výsledek konkrétní typ (například `JsonResult`) nebo `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="44e63-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="44e63-117">Vrací data ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="44e63-117">Returning JSON-formatted data:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="44e63-118">Ukázková odpověď z tuto akci:</span><span class="sxs-lookup"><span data-stu-id="44e63-118">Sample response from this action:</span></span>

![Síťové kartě nástrojů pro vývojáře v Microsoft Edge zobrazující typ obsahu odpovědi je application/json](formatting/_static/json-response.png)

<span data-ttu-id="44e63-120">Všimněte si, že je typ obsahu odpovědi `application/json`, uvedené v seznamu požadavků sítě a v části hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="44e63-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="44e63-121">Všimněte si také seznam možností, které jsou uvedené v prohlížeči (v tomto případě Microsoft Edge) v hlavičce Accept v části záhlaví požadavku.</span><span class="sxs-lookup"><span data-stu-id="44e63-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="44e63-122">Aktuální technika ignoruje tuto hlavičku; obeying ho jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="44e63-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="44e63-123">Chcete-li vrátit data ve formátu prostého textu, použijte `ContentResult` a `Content` pomocné rutiny:</span><span class="sxs-lookup"><span data-stu-id="44e63-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="44e63-124">Odpověď z tuto akci:</span><span class="sxs-lookup"><span data-stu-id="44e63-124">A response from this action:</span></span>

![Síťové kartě nástrojů pro vývojáře v Microsoft Edge zobrazující typ obsahu odpovědi je text/plain](formatting/_static/text-response.png)

<span data-ttu-id="44e63-126">Poznámka: v tomto případě `Content-Type` vrátil je `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="44e63-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="44e63-127">Můžete také dosáhnout tento stejné chování pomocí právě typ odpovědi řetězec:</span><span class="sxs-lookup"><span data-stu-id="44e63-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="44e63-128">Pro netriviální akce s více vrátit typy nebo možnosti (například různé stavové kódy HTTP na základě výsledku operace provedené), raději `IActionResult` jako návratový typ.</span><span class="sxs-lookup"><span data-stu-id="44e63-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="44e63-129">Vyjednávání obsahu</span><span class="sxs-lookup"><span data-stu-id="44e63-129">Content Negotiation</span></span>

<span data-ttu-id="44e63-130">Vyjednávání obsahu (*conneg* pro zkrácení) nastane, když klient Určuje [hlavička Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="44e63-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="44e63-131">Výchozí formát používaný ASP.NET Core MVC je JSON.</span><span class="sxs-lookup"><span data-stu-id="44e63-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="44e63-132">Vyjednávání obsahu je implementováno modulem `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="44e63-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="44e63-133">Je také součástí stavový kód vráceny výsledky určité akce z pomocné metody (které jsou všechny založené na `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="44e63-133">It's also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="44e63-134">Můžete se taky vrátit typu modelu (třídy, které jste definovali jako typ přenosu dat) a rozhraní budou automaticky zahrnovat ho `ObjectResult` za vás.</span><span class="sxs-lookup"><span data-stu-id="44e63-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="44e63-135">Používá následující metody akce `Ok` a `NotFound` pomocné metody:</span><span class="sxs-lookup"><span data-stu-id="44e63-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="44e63-136">Odpověď formátu JSON, bude vrácen, pokud byl požadován jiného formátu a server může vracet požadovaný formát.</span><span class="sxs-lookup"><span data-stu-id="44e63-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="44e63-137">Můžete použít nástroje, jako je [Fiddler](http://www.telerik.com/fiddler) vytvořit požadavek, který obsahuje hlavičku Accept a určit jiného formátu.</span><span class="sxs-lookup"><span data-stu-id="44e63-137">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="44e63-138">V takovém případě, pokud má server *formátování* , může vytvořit odpověď v požadovaný formát, výsledkem bude vrácen ve formátu preferované klienta.</span><span class="sxs-lookup"><span data-stu-id="44e63-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![ZÍSKAT aplikaci Fiddler konzola znázorňující, ručně vytvořit požadavek s hodnotou hlavičky Accept application/XML](formatting/_static/fiddler-composer.png)

<span data-ttu-id="44e63-140">Výše uvedený snímek obrazovky, autora Fiddler používá k vytvoření žádosti, zadání `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="44e63-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="44e63-141">Ve výchozím nastavení, ASP.NET MVC základní podporuje pouze formát JSON, takže i když je zadané jiného formátu, výsledek vrácený je stále ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="44e63-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="44e63-142">Uvidíte, jak přidat další formátování v další části.</span><span class="sxs-lookup"><span data-stu-id="44e63-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="44e63-143">Akce kontroleru vrátit POCOs (prostý staré CLR objekty), v takovém případě se automaticky vytvoří ASP.NET MVC `ObjectResult` pro vás, která zabalí objekt.</span><span class="sxs-lookup"><span data-stu-id="44e63-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="44e63-144">Klient získá formátovaný serializovaný objekt (formátu JSON je výchozí nastavení, můžete nakonfigurovat XML nebo jiných formátů).</span><span class="sxs-lookup"><span data-stu-id="44e63-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="44e63-145">Pokud se objekt vrácená `null`, pak vrátí rozhraní `204 No Content` odpovědi.</span><span class="sxs-lookup"><span data-stu-id="44e63-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="44e63-146">Vrátí typ objektu:</span><span class="sxs-lookup"><span data-stu-id="44e63-146">Returning an object type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="44e63-147">V ukázce obdrží žádost o alias platný Autor odpovědi 200 OK daty vytvářením obsahu.</span><span class="sxs-lookup"><span data-stu-id="44e63-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="44e63-148">Žádost o neplatný alias obdrží 204 ne obsahu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="44e63-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="44e63-149">Níže jsou uvedeny snímky obrazovky zobrazující odpovědi ve formátu XML a JSON.</span><span class="sxs-lookup"><span data-stu-id="44e63-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="44e63-150">Proces vyjednávání obsahu</span><span class="sxs-lookup"><span data-stu-id="44e63-150">Content Negotiation Process</span></span>

<span data-ttu-id="44e63-151">Obsahu *vyjednávání* pouze dojde-li `Accept` hlavička se zobrazí v požadavku.</span><span class="sxs-lookup"><span data-stu-id="44e63-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="44e63-152">Pokud požadavek obsahuje hlavičku accept, rozhraní se zobrazí výčet typů médií v hlavičce accept v upřednostňovaném pořadí a pokusí najít formátovací modul, který může vytvořit odpověď v jednom z formátů zadaná v hlavičce accept.</span><span class="sxs-lookup"><span data-stu-id="44e63-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="44e63-153">V případě, že je nalezeno žádné formátování, které může stát odpovědí na požadavek klienta, rozhraní se pokusí najít první formátovací modul, který může vytvořit odpověď (pokud vývojář možnost nakonfiguroval na `MvcOptions` vrátit 406 Nepřijatelný místo).</span><span class="sxs-lookup"><span data-stu-id="44e63-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="44e63-154">Pokud požadavek určuje XML, ale není nakonfigurován formátovací modul XML, bude formátovací modul JSON použít.</span><span class="sxs-lookup"><span data-stu-id="44e63-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="44e63-155">Obecně platí Pokud je nakonfigurován žádné formátování který může poskytnout požadovaný formát, pak první formátovací modul, který dokáže formátovat objekt se používá.</span><span class="sxs-lookup"><span data-stu-id="44e63-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="44e63-156">Pokud je zadána žádná hlavička první formátovací modul, který dokáže zpracovat objekt, který má být vrácen se použije k serializaci odpovědi.</span><span class="sxs-lookup"><span data-stu-id="44e63-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="44e63-157">V takovém případě není k dispozici žádné vyjednávání dochází – server je určení formátu, jaké se bude používat.</span><span class="sxs-lookup"><span data-stu-id="44e63-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="44e63-158">Pokud obsahuje hlavičku Accept `*/*`, záhlaví bude ignorována, pokud `RespectBrowserAcceptHeader` je nastaven na hodnotu true na `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="44e63-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="44e63-159">Prohlížeče a vyjednávání obsahu</span><span class="sxs-lookup"><span data-stu-id="44e63-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="44e63-160">Na rozdíl od typických rozhraní API klientů webových prohlížečů mívají k poskytování `Accept` hlavičky, které obsahují širokou škálu formátů, včetně zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="44e63-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="44e63-161">Ve výchozím nastavení, když rozhraní zjistí, zda požadavek pochází z prohlížeče, se budou ignorovat `Accept` záhlaví a místo toho návratový obsahu v aplikaci je nakonfigurované výchozí formát (JSON Pokud jinak nakonfigurovat).</span><span class="sxs-lookup"><span data-stu-id="44e63-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="44e63-162">To poskytuje jednotný při používání rozhraní API pomocí různých prohlížečích.</span><span class="sxs-lookup"><span data-stu-id="44e63-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="44e63-163">Pokud si přejete hlavičky accept prohlížeč dodržet aplikace, to můžete nakonfigurovat jako součást konfigurace MVC nastavením `RespectBrowserAcceptHeader` k `true` v `ConfigureServices` metoda v *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="44e63-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="44e63-164">Konfigurace formátování</span><span class="sxs-lookup"><span data-stu-id="44e63-164">Configuring Formatters</span></span>

<span data-ttu-id="44e63-165">Pokud aplikace potřebuje pro podporu dalších formátech nad výchozí hodnotu JSON, můžete přidat balíčků NuGet a nakonfigurovat MVC, které je podporují.</span><span class="sxs-lookup"><span data-stu-id="44e63-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="44e63-166">Existují samostatné formátování pro vstup a výstup.</span><span class="sxs-lookup"><span data-stu-id="44e63-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="44e63-167">Vstupní formátování používají [vazby modelu](model-binding.md); výstupní formátování slouží k formátování odpovědi.</span><span class="sxs-lookup"><span data-stu-id="44e63-167">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="44e63-168">Můžete také nakonfigurovat [vlastní formátování](../advanced/custom-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="44e63-168">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="44e63-169">Přidání podpory pro formát XML</span><span class="sxs-lookup"><span data-stu-id="44e63-169">Adding XML Format Support</span></span>

<span data-ttu-id="44e63-170">Chcete-li přidat podporu pro formátování XML, nainstalujte `Microsoft.AspNetCore.Mvc.Formatters.Xml` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="44e63-170">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="44e63-171">Přidat XmlSerializerFormatters do konfigurace MVC v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="44e63-171">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="44e63-172">Alternativně můžete přidat pouze formátování výstupu:</span><span class="sxs-lookup"><span data-stu-id="44e63-172">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="44e63-173">Tyto dva přístupy se serializovat výsledky pomocí `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="44e63-173">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="44e63-174">Pokud dáváte přednost, můžete použít `System.Runtime.Serialization.DataContractSerializer` přidáním jeho přidružené formátovací modul:</span><span class="sxs-lookup"><span data-stu-id="44e63-174">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="44e63-175">Po přidání podpory pro formátování XML, vaše řadiče metody by měla vrátit příslušný formát na základě daného požadavku `Accept` záhlaví, jako tento Fiddler ukazuje příklad:</span><span class="sxs-lookup"><span data-stu-id="44e63-175">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Konzole Fiddler: nezpracovaná kartě pro požadavek se zobrazují hodnotu hlavičky Accept je application/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="44e63-178">Se zobrazí na kartě kontroly, který byl vyžádán nezpracovaná získat pomocí `Accept: application/xml` hlavičky set.</span><span class="sxs-lookup"><span data-stu-id="44e63-178">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="44e63-179">V podokně dozvíte odpovědi `Content-Type: application/xml` záhlaví a `Author` objekt má byl serializován do formátu XML.</span><span class="sxs-lookup"><span data-stu-id="44e63-179">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="44e63-180">Na kartě autora lze upravit žádost o zadejte `application/json` v `Accept` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="44e63-180">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="44e63-181">Provedení požadavku a odpovědi se bude formátovat jako JSON:</span><span class="sxs-lookup"><span data-stu-id="44e63-181">Execute the request, and the response will be formatted as JSON:</span></span>

![Konzole Fiddler: nezpracovaná kartě pro požadavek se zobrazují hodnotu hlavičky Accept je application/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="44e63-184">Na tomto snímku obrazovky vidíte, nastaví hlavičku ze žádosti `Accept: application/json` a určuje odpověď stejná jako jeho `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="44e63-184">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="44e63-185">`Author` Objektu se zobrazí v textu odpovědi ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="44e63-185">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="44e63-186">Vynucení konkrétní formátu</span><span class="sxs-lookup"><span data-stu-id="44e63-186">Forcing a Particular Format</span></span>

<span data-ttu-id="44e63-187">Pokud chcete omezit formáty odpovědi pro konkrétní akci je možné, můžete použít `[Produces]` filtru.</span><span class="sxs-lookup"><span data-stu-id="44e63-187">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="44e63-188">`[Produces]` Filtru určuje formáty odpovědi pro konkrétní akce (nebo řadič).</span><span class="sxs-lookup"><span data-stu-id="44e63-188">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="44e63-189">Jako většinu [filtry](../controllers/filters.md), to mohou být použity na akce, řadiče nebo globální obor.</span><span class="sxs-lookup"><span data-stu-id="44e63-189">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="44e63-190">`[Produces]` Filtru akce vynutí všechny akce v rámci `AuthorsController` vrátit odpovědí formátu JSON i v případě, že další formátovací moduly byly nakonfigurovány pro aplikace a klient pro zadaný `Accept` hlavičky požadavku liší, k dispozici formát.</span><span class="sxs-lookup"><span data-stu-id="44e63-190">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="44e63-191">V tématu [filtry](../controllers/filters.md) Další informace, včetně použití filtrů globálně.</span><span class="sxs-lookup"><span data-stu-id="44e63-191">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="44e63-192">Speciální případu formátování</span><span class="sxs-lookup"><span data-stu-id="44e63-192">Special Case Formatters</span></span>

<span data-ttu-id="44e63-193">Některých zvláštních případech jsou implementovány pomocí předdefinovaných formátování.</span><span class="sxs-lookup"><span data-stu-id="44e63-193">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="44e63-194">Ve výchozím nastavení `string` návratové typy se bude formátovat jako *text/plain* (*text/html* Pokud požadovaný prostřednictvím `Accept` záhlaví).</span><span class="sxs-lookup"><span data-stu-id="44e63-194">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="44e63-195">Toto chování lze odebrat odstraněním `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="44e63-195">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="44e63-196">Odebrat formátování v `Configure` metoda v *Startup.cs* (zobrazené dole).</span><span class="sxs-lookup"><span data-stu-id="44e63-196">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="44e63-197">Akce, které mají objekt modelu vrátí typ vrátí 204 neodpovídá obsahu při vrácení `null`.</span><span class="sxs-lookup"><span data-stu-id="44e63-197">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="44e63-198">Toto chování lze odebrat odstraněním `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="44e63-198">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="44e63-199">Následující kód odebere `TextOutputFormatter` a `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="44e63-199">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="44e63-200">Bez `TextOutputFormatter`, `string` vrátit typy vrátit 406 Nepřijatelný, např.</span><span class="sxs-lookup"><span data-stu-id="44e63-200">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="44e63-201">Všimněte si, že pokud formátovací modul XML existuje, ji budou zformátovány `string` návratové typy, pokud `TextOutputFormatter` se odebere.</span><span class="sxs-lookup"><span data-stu-id="44e63-201">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="44e63-202">Bez `HttpNoContentOutputFormatter`, hodnotu null. objekty jsou formátovány pomocí nakonfigurované formátovacího modulu.</span><span class="sxs-lookup"><span data-stu-id="44e63-202">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="44e63-203">Například formátovací modul JSON jednoduše vrátí odpověď subjektu, který `null`, zatímco formátovací modul XML vrátí prázdný element XML s atributem `xsi:nil="true"` nastavit.</span><span class="sxs-lookup"><span data-stu-id="44e63-203">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="44e63-204">Mapování adresy URL formát odpovědi</span><span class="sxs-lookup"><span data-stu-id="44e63-204">Response Format URL Mappings</span></span>

<span data-ttu-id="44e63-205">Klienti mohou požadovat konkrétní formátu jako část adresy URL, například na řetězec dotazu nebo části cesty nebo pomocí formátu konkrétní příponu například .xml nebo .json.</span><span class="sxs-lookup"><span data-stu-id="44e63-205">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="44e63-206">Mapování z cestu požadavku musí být zadán v trasy, k níž je pomocí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="44e63-206">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="44e63-207">Příklad:</span><span class="sxs-lookup"><span data-stu-id="44e63-207">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="44e63-208">Tato trasa by umožnilo požadovaný formát, který má být zadány jako volitelný soubor rozšíření.</span><span class="sxs-lookup"><span data-stu-id="44e63-208">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="44e63-209">`[FormatFilter]` Atribut zkontroluje existenci hodnoty formátu v `RouteData` a namapujete formát odpovědi na odpovídající formátovací modul, při vytváření odpovědi.</span><span class="sxs-lookup"><span data-stu-id="44e63-209">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="44e63-210">Trasy</span><span class="sxs-lookup"><span data-stu-id="44e63-210">Route</span></span>                      | <span data-ttu-id="44e63-211">Formátování</span><span class="sxs-lookup"><span data-stu-id="44e63-211">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="44e63-212">Výchozí výstupní formátování</span><span class="sxs-lookup"><span data-stu-id="44e63-212">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="44e63-213">Formátovací modul JSON (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="44e63-213">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="44e63-214">Formátovací modul XML (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="44e63-214">The XML formatter (if configured)</span></span>  |
