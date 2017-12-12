---
title: "Formátování dat odpovědi v aplikaci ASP.NET MVC jádra"
author: ardalis
description: "Zjistěte, jak k formátování data odpovědi v aplikaci ASP.NET MVC jádra."
keywords: "ASP.NET Core, data odpovědi, IOutputFormatter, IActionResult"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c056df45-d013-4826-91a1-4a092bae1ea5
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: abc125a093ff2cd5a38a537ecdfc795ff03e23f7
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/19/2017
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="7f166-104">Úvod k formátování data odpovědi v aplikaci ASP.NET MVC jádra</span><span class="sxs-lookup"><span data-stu-id="7f166-104">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="7f166-105">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7f166-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7f166-106">Jádro ASP.NET MVC má integrovanou podporu pro formátování data odpovědi pomocí pevné formáty nebo v reakci na požadavky klienta.</span><span class="sxs-lookup"><span data-stu-id="7f166-106">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="7f166-107">[Zobrazení nebo stažení ukázky z webu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span><span class="sxs-lookup"><span data-stu-id="7f166-107">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="7f166-108">Výsledky akce specifické pro formát</span><span class="sxs-lookup"><span data-stu-id="7f166-108">Format-Specific Action Results</span></span>

<span data-ttu-id="7f166-109">Některé typy výsledků akcí, jako jsou specifické pro konkrétní formátu, `JsonResult` a `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="7f166-109">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="7f166-110">Akce může vrátit konkrétní výsledky, které jsou vždy určitým způsobem.</span><span class="sxs-lookup"><span data-stu-id="7f166-110">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="7f166-111">Například vrácení `JsonResult` vrátí data ve formátu JSON, bez ohledu na to předvolby klienta.</span><span class="sxs-lookup"><span data-stu-id="7f166-111">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="7f166-112">Podobně vrácení `ContentResult` vrátí řetězec ve formátu prostého textu formátu dat (jak se jednoduše vrací řetězec).</span><span class="sxs-lookup"><span data-stu-id="7f166-112">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="7f166-113">Akce nemusí vrátit žádnou jednotlivou; MVC podporuje žádnou návratovou hodnotu objektu.</span><span class="sxs-lookup"><span data-stu-id="7f166-113">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="7f166-114">Akce vrátí-li `IActionResult` implementace a řadič dědí z `Controller`, vývojáři mají mnoho pomocné metody odpovídající mnoho z těchto možností.</span><span class="sxs-lookup"><span data-stu-id="7f166-114">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="7f166-115">Výsledky z akcí, které vracejí objekty, které nejsou `IActionResult` typy budou serializovány odpovídající pomocí `IOutputFormatter` implementace.</span><span class="sxs-lookup"><span data-stu-id="7f166-115">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="7f166-116">Vrátit data v konkrétním formátu z řadiče, která dědí z `Controller` základní třídy, použijte metodu integrovaných pomocných `Json` vrátit JSON a `Content` ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="7f166-116">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="7f166-117">Vaše metoda akce by měla vrátit výsledek konkrétní typ (například `JsonResult`) nebo `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="7f166-117">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="7f166-118">Vrací data ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="7f166-118">Returning JSON-formatted data:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="7f166-119">Ukázková odpověď z tuto akci:</span><span class="sxs-lookup"><span data-stu-id="7f166-119">Sample response from this action:</span></span>

![Síťové kartě nástrojů pro vývojáře v Microsoft Edge zobrazující typ obsahu odpovědi je application/json](formatting/_static/json-response.png)

<span data-ttu-id="7f166-121">Všimněte si, že je typ obsahu odpovědi `application/json`, uvedené v seznamu požadavků sítě a v části hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7f166-121">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="7f166-122">Všimněte si také seznam možností, které jsou uvedené v prohlížeči (v tomto případě Microsoft Edge) v hlavičce Accept v části záhlaví požadavku.</span><span class="sxs-lookup"><span data-stu-id="7f166-122">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="7f166-123">Aktuální technika ignoruje tuto hlavičku; obeying ho jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="7f166-123">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="7f166-124">Chcete-li vrátit data ve formátu prostého textu, použijte `ContentResult` a `Content` pomocné rutiny:</span><span class="sxs-lookup"><span data-stu-id="7f166-124">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="7f166-125">Odpověď z tuto akci:</span><span class="sxs-lookup"><span data-stu-id="7f166-125">A response from this action:</span></span>

![Síťové kartě nástrojů pro vývojáře v Microsoft Edge zobrazující typ obsahu odpovědi je text/plain](formatting/_static/text-response.png)

<span data-ttu-id="7f166-127">Poznámka: v tomto případě `Content-Type` vrátil je `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="7f166-127">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="7f166-128">Můžete také dosáhnout tento stejné chování pomocí právě typ odpovědi řetězec:</span><span class="sxs-lookup"><span data-stu-id="7f166-128">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="7f166-129">Pro netriviální akce s více vrátit typy nebo možnosti (například různé stavové kódy HTTP na základě výsledku operace provedené), raději `IActionResult` jako návratový typ.</span><span class="sxs-lookup"><span data-stu-id="7f166-129">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="7f166-130">Vyjednávání obsahu</span><span class="sxs-lookup"><span data-stu-id="7f166-130">Content Negotiation</span></span>

<span data-ttu-id="7f166-131">Vyjednávání obsahu (*conneg* pro zkrácení) nastane, když klient Určuje [hlavička Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="7f166-131">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="7f166-132">Výchozí formát používaný ASP.NET Core MVC je JSON.</span><span class="sxs-lookup"><span data-stu-id="7f166-132">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="7f166-133">Vyjednávání obsahu je implementováno modulem `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="7f166-133">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="7f166-134">Je také součástí stavový kód vráceny výsledky určité akce z pomocné metody (které jsou všechny založené na `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="7f166-134">It is also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="7f166-135">Můžete se taky vrátit typu modelu (třídy, které jste definovali jako typ přenosu dat) a rozhraní budou automaticky zahrnovat ho `ObjectResult` za vás.</span><span class="sxs-lookup"><span data-stu-id="7f166-135">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="7f166-136">Používá následující metody akce `Ok` a `NotFound` pomocné metody:</span><span class="sxs-lookup"><span data-stu-id="7f166-136">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="7f166-137">Odpověď formátu JSON, bude vrácen, pokud byl požadován jiného formátu a server může vracet požadovaný formát.</span><span class="sxs-lookup"><span data-stu-id="7f166-137">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="7f166-138">Můžete použít nástroje, jako je [Fiddler](http://www.telerik.com/fiddler) vytvořit požadavek, který obsahuje hlavičku Accept a určit jiného formátu.</span><span class="sxs-lookup"><span data-stu-id="7f166-138">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="7f166-139">V takovém případě, pokud má server *formátování* , může vytvořit odpověď v požadovaný formát, výsledkem bude vrácen ve formátu preferované klienta.</span><span class="sxs-lookup"><span data-stu-id="7f166-139">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![ZÍSKAT aplikaci Fiddler konzola znázorňující, ručně vytvořit požadavek s hodnotou hlavičky Accept application/XML](formatting/_static/fiddler-composer.png)

<span data-ttu-id="7f166-141">Výše uvedený snímek obrazovky, autora Fiddler používá k vytvoření žádosti, zadání `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="7f166-141">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="7f166-142">Ve výchozím nastavení, ASP.NET MVC základní podporuje pouze formát JSON, takže i když je zadané jiného formátu, výsledek vrácený je stále ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="7f166-142">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="7f166-143">Uvidíte, jak přidat další formátování v další části.</span><span class="sxs-lookup"><span data-stu-id="7f166-143">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="7f166-144">Akce kontroleru vrátit POCOs (prostý staré CLR objekty), v takovém případě se automaticky vytvoří ASP.NET MVC `ObjectResult` pro vás, která zabalí objekt.</span><span class="sxs-lookup"><span data-stu-id="7f166-144">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="7f166-145">Klient získá formátovaný serializovaný objekt (formátu JSON je výchozí nastavení, můžete nakonfigurovat XML nebo jiných formátů).</span><span class="sxs-lookup"><span data-stu-id="7f166-145">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="7f166-146">Pokud se objekt vrácená `null`, pak vrátí rozhraní `204 No Content` odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7f166-146">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="7f166-147">Vrátí typ objektu:</span><span class="sxs-lookup"><span data-stu-id="7f166-147">Returning an object type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="7f166-148">V ukázce obdrží žádost o alias platný Autor odpovědi 200 OK daty vytvářením obsahu.</span><span class="sxs-lookup"><span data-stu-id="7f166-148">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="7f166-149">Žádost o neplatný alias obdrží 204 ne obsahu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7f166-149">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="7f166-150">Níže jsou uvedeny snímky obrazovky zobrazující odpovědi ve formátu XML a JSON.</span><span class="sxs-lookup"><span data-stu-id="7f166-150">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="7f166-151">Proces vyjednávání obsahu</span><span class="sxs-lookup"><span data-stu-id="7f166-151">Content Negotiation Process</span></span>

<span data-ttu-id="7f166-152">Obsahu *vyjednávání* pouze dojde-li `Accept` hlavička se zobrazí v požadavku.</span><span class="sxs-lookup"><span data-stu-id="7f166-152">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="7f166-153">Pokud požadavek obsahuje hlavičku accept, rozhraní se zobrazí výčet typů médií v hlavičce accept v upřednostňovaném pořadí a pokusí najít formátovací modul, který může vytvořit odpověď v jednom z formátů zadaná v hlavičce accept.</span><span class="sxs-lookup"><span data-stu-id="7f166-153">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="7f166-154">V případě, že je nalezeno žádné formátování, které může stát odpovědí na požadavek klienta, rozhraní se pokusí najít první formátovací modul, který může vytvořit odpověď (pokud vývojář možnost nakonfiguroval na `MvcOptions` vrátit 406 Nepřijatelný místo).</span><span class="sxs-lookup"><span data-stu-id="7f166-154">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="7f166-155">Pokud požadavek určuje XML, ale není nakonfigurován formátovací modul XML, bude formátovací modul JSON použít.</span><span class="sxs-lookup"><span data-stu-id="7f166-155">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="7f166-156">Obecně platí Pokud je nakonfigurován žádné formátování který může poskytnout požadovaný formát, pak první formátovací modul, který dokáže formátovat objekt se používá.</span><span class="sxs-lookup"><span data-stu-id="7f166-156">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="7f166-157">Pokud je zadána žádná hlavička první formátovací modul, který dokáže zpracovat objekt, který má být vrácen se použije k serializaci odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7f166-157">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="7f166-158">V takovém případě není k dispozici žádné vyjednávání dochází – server je určení formátu, jaké se bude používat.</span><span class="sxs-lookup"><span data-stu-id="7f166-158">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="7f166-159">Pokud obsahuje hlavičku Accept `*/*`, záhlaví bude ignorována, pokud `RespectBrowserAcceptHeader` je nastaven na hodnotu true na `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="7f166-159">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="7f166-160">Prohlížeče a vyjednávání obsahu</span><span class="sxs-lookup"><span data-stu-id="7f166-160">Browsers and Content Negotiation</span></span>

<span data-ttu-id="7f166-161">Na rozdíl od typických rozhraní API klientů webových prohlížečů mívají k poskytování `Accept` hlavičky, které obsahují širokou škálu formátů, včetně zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="7f166-161">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="7f166-162">Ve výchozím nastavení, když rozhraní zjistí, zda požadavek pochází z prohlížeče, se budou ignorovat `Accept` záhlaví a místo toho návratový obsahu v aplikaci je nakonfigurované výchozí formát (JSON Pokud jinak nakonfigurovat).</span><span class="sxs-lookup"><span data-stu-id="7f166-162">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="7f166-163">To poskytuje jednotný při používání rozhraní API pomocí různých prohlížečích.</span><span class="sxs-lookup"><span data-stu-id="7f166-163">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="7f166-164">Pokud si přejete hlavičky accept prohlížeč dodržet aplikace, to můžete nakonfigurovat jako součást konfigurace MVC nastavením `RespectBrowserAcceptHeader` k `true` v `ConfigureServices` metoda v *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7f166-164">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="7f166-165">Konfigurace formátování</span><span class="sxs-lookup"><span data-stu-id="7f166-165">Configuring Formatters</span></span>

<span data-ttu-id="7f166-166">Pokud aplikace potřebuje pro podporu dalších formátech nad výchozí hodnotu JSON, můžete přidat balíčků NuGet a nakonfigurovat MVC, které je podporují.</span><span class="sxs-lookup"><span data-stu-id="7f166-166">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="7f166-167">Existují samostatné formátování pro vstup a výstup.</span><span class="sxs-lookup"><span data-stu-id="7f166-167">There are separate formatters for input and output.</span></span> <span data-ttu-id="7f166-168">Vstupní formátování používají [vazby modelu](model-binding.md); výstupní formátování slouží k formátování odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7f166-168">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="7f166-169">Můžete také nakonfigurovat [vlastní formátování](../advanced/custom-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="7f166-169">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="7f166-170">Přidání podpory pro formát XML</span><span class="sxs-lookup"><span data-stu-id="7f166-170">Adding XML Format Support</span></span>

<span data-ttu-id="7f166-171">Chcete-li přidat podporu pro formátování XML, nainstalujte `Microsoft.AspNetCore.Mvc.Formatters.Xml` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="7f166-171">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="7f166-172">Přidat XmlSerializerFormatters do konfigurace MVC v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7f166-172">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="7f166-173">Alternativně můžete přidat pouze formátování výstupu:</span><span class="sxs-lookup"><span data-stu-id="7f166-173">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="7f166-174">Tyto dva přístupy se serializovat výsledky pomocí `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="7f166-174">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="7f166-175">Pokud dáváte přednost, můžete použít `System.Runtime.Serialization.DataContractSerializer` přidáním jeho přidružené formátovací modul:</span><span class="sxs-lookup"><span data-stu-id="7f166-175">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="7f166-176">Po přidání podpory pro formátování XML, vaše řadiče metody by měla vrátit příslušný formát na základě daného požadavku `Accept` záhlaví, jako tento Fiddler ukazuje příklad:</span><span class="sxs-lookup"><span data-stu-id="7f166-176">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Konzole Fiddler: nezpracovaná kartě pro požadavek se zobrazují hodnotu hlavičky Accept je application/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="7f166-179">Se zobrazí na kartě kontroly, který byl vyžádán nezpracovaná získat pomocí `Accept: application/xml` hlavičky set.</span><span class="sxs-lookup"><span data-stu-id="7f166-179">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="7f166-180">V podokně dozvíte odpovědi `Content-Type: application/xml` záhlaví a `Author` objekt má byl serializován do formátu XML.</span><span class="sxs-lookup"><span data-stu-id="7f166-180">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="7f166-181">Na kartě autora lze upravit žádost o zadejte `application/json` v `Accept` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="7f166-181">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="7f166-182">Provedení požadavku a odpovědi se bude formátovat jako JSON:</span><span class="sxs-lookup"><span data-stu-id="7f166-182">Execute the request, and the response will be formatted as JSON:</span></span>

![Konzole Fiddler: nezpracovaná kartě pro požadavek se zobrazují hodnotu hlavičky Accept je application/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="7f166-185">Na tomto snímku obrazovky vidíte, nastaví hlavičku ze žádosti `Accept: application/json` a určuje odpověď stejná jako jeho `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="7f166-185">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="7f166-186">`Author` Objektu se zobrazí v textu odpovědi ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="7f166-186">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="7f166-187">Vynucení konkrétní formátu</span><span class="sxs-lookup"><span data-stu-id="7f166-187">Forcing a Particular Format</span></span>

<span data-ttu-id="7f166-188">Pokud chcete omezit formáty odpovědi pro konkrétní akci je možné, můžete použít `[Produces]` filtru.</span><span class="sxs-lookup"><span data-stu-id="7f166-188">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="7f166-189">`[Produces]` Filtru určuje formáty odpovědi pro konkrétní akce (nebo řadič).</span><span class="sxs-lookup"><span data-stu-id="7f166-189">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="7f166-190">Jako většinu [filtry](../controllers/filters.md), to mohou být použity na akce, řadiče nebo globální obor.</span><span class="sxs-lookup"><span data-stu-id="7f166-190">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="7f166-191">`[Produces]` Filtru akce vynutí všechny akce v rámci `AuthorsController` vrátit odpovědí formátu JSON i v případě, že další formátovací moduly byly nakonfigurovány pro aplikace a klient pro zadaný `Accept` hlavičky požadavku liší, k dispozici formát.</span><span class="sxs-lookup"><span data-stu-id="7f166-191">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="7f166-192">V tématu [filtry](../controllers/filters.md) Další informace, včetně použití filtrů globálně.</span><span class="sxs-lookup"><span data-stu-id="7f166-192">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="7f166-193">Speciální případu formátování</span><span class="sxs-lookup"><span data-stu-id="7f166-193">Special Case Formatters</span></span>

<span data-ttu-id="7f166-194">Některých zvláštních případech jsou implementovány pomocí předdefinovaných formátování.</span><span class="sxs-lookup"><span data-stu-id="7f166-194">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="7f166-195">Ve výchozím nastavení `string` návratové typy se bude formátovat jako *text/plain* (*text/html* Pokud požadovaný prostřednictvím `Accept` záhlaví).</span><span class="sxs-lookup"><span data-stu-id="7f166-195">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="7f166-196">Toto chování lze odebrat odstraněním `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="7f166-196">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="7f166-197">Odebrat formátování v `Configure` metoda v *Startup.cs* (zobrazené dole).</span><span class="sxs-lookup"><span data-stu-id="7f166-197">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="7f166-198">Akce, které mají objekt modelu vrátí typ vrátí 204 neodpovídá obsahu při vrácení `null`.</span><span class="sxs-lookup"><span data-stu-id="7f166-198">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="7f166-199">Toto chování lze odebrat odstraněním `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="7f166-199">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="7f166-200">Následující kód odebere `TextOutputFormatter` a `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="7f166-200">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="7f166-201">Bez `TextOutputFormatter`, `string` vrátit typy vrátit 406 Nepřijatelný, např.</span><span class="sxs-lookup"><span data-stu-id="7f166-201">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="7f166-202">Všimněte si, že pokud formátovací modul XML existuje, ji budou zformátovány `string` návratové typy, pokud `TextOutputFormatter` se odebere.</span><span class="sxs-lookup"><span data-stu-id="7f166-202">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="7f166-203">Bez `HttpNoContentOutputFormatter`, hodnotu null. objekty jsou formátovány pomocí nakonfigurované formátovacího modulu.</span><span class="sxs-lookup"><span data-stu-id="7f166-203">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="7f166-204">Například formátovací modul JSON jednoduše vrátí odpověď subjektu, který `null`, zatímco formátovací modul XML vrátí prázdný element XML s atributem `xsi:nil="true"` nastavit.</span><span class="sxs-lookup"><span data-stu-id="7f166-204">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="7f166-205">Mapování adresy URL formát odpovědi</span><span class="sxs-lookup"><span data-stu-id="7f166-205">Response Format URL Mappings</span></span>

<span data-ttu-id="7f166-206">Klienti mohou požadovat konkrétní formátu jako část adresy URL, například na řetězec dotazu nebo části cesty nebo pomocí formátu konkrétní příponu například .xml nebo .json.</span><span class="sxs-lookup"><span data-stu-id="7f166-206">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="7f166-207">Mapování z cestu požadavku musí být zadán v trasy, k níž je pomocí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7f166-207">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="7f166-208">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7f166-208">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="7f166-209">Tato trasa by umožnilo požadovaný formát, který má být zadány jako volitelný soubor rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7f166-209">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="7f166-210">`[FormatFilter]` Atribut zkontroluje existenci hodnoty formátu v `RouteData` a namapujete formát odpovědi na odpovídající formátovací modul, při vytváření odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7f166-210">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="7f166-211">Trasy</span><span class="sxs-lookup"><span data-stu-id="7f166-211">Route</span></span>                      | <span data-ttu-id="7f166-212">Formátování</span><span class="sxs-lookup"><span data-stu-id="7f166-212">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="7f166-213">Výchozí výstupní formátování</span><span class="sxs-lookup"><span data-stu-id="7f166-213">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="7f166-214">Formátovací modul JSON (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="7f166-214">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="7f166-215">Formátovací modul XML (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="7f166-215">The XML formatter (if configured)</span></span>  |
