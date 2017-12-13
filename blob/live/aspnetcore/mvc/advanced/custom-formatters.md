---
title: "Vlastní formátování v aplikaci ASP.NET MVC základní webové rozhraní API"
author: tdykstra
description: "Zjistěte, jak vytvořit a použít vlastní formátování webové rozhraní API v ASP.NET Core."
keywords: "ASP.NET Core webové rozhraní api, vlastní formátování"
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 1fb6fdc2-e199-4469-9012-b909d1913422
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 5e665abe10fd7444c3fd5f20cfeca3ef0a5f79d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a><span data-ttu-id="c47e7-104">Vlastní formátování v aplikaci ASP.NET MVC základní webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c47e7-104">Custom formatters in ASP.NET Core MVC web APIs</span></span>

<span data-ttu-id="c47e7-105">podle [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c47e7-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="c47e7-106">Jádro ASP.NET MVC má integrovanou podporu pro výměnu dat ve webové rozhraní API pomocí formátu JSON, XML nebo prostý text.</span><span class="sxs-lookup"><span data-stu-id="c47e7-106">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="c47e7-107">Tento článek ukazuje, jak přidat podporu dalších formátech tak, že vytvoříte vlastní formátování.</span><span class="sxs-lookup"><span data-stu-id="c47e7-107">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="c47e7-108">[Zobrazení nebo stažení ukázky z webu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="c47e7-108">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="c47e7-109">Kdy použít vlastní formátování</span><span class="sxs-lookup"><span data-stu-id="c47e7-109">When to use custom formatters</span></span>

<span data-ttu-id="c47e7-110">Pokud chcete použít vlastní formátování [vyjednávání obsahu](xref:mvc/models/formatting) proces pro podporu typ obsahu, který nepodporuje integrované formátovací moduly (JSON, XML a prostý text).</span><span class="sxs-lookup"><span data-stu-id="c47e7-110">Use a custom formatter when you want the [content negotiation](xref:mvc/models/formatting) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="c47e7-111">Například, pokud některé z klientů pro webového rozhraní API může zpracovat [Protobuf](https://github.com/google/protobuf) formátu, můžete chtít použít Protobuf s těmito klienty, protože je efektivnější.</span><span class="sxs-lookup"><span data-stu-id="c47e7-111">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span>  <span data-ttu-id="c47e7-112">Nebo můžete chtít webového rozhraní API k odeslání kontaktní názvy a adresy v [soubor vCard](https://wikipedia.org/wiki/VCard) formát, běžně používaný formát pro výměnu kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="c47e7-112">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="c47e7-113">Ukázková aplikace součástí v tomto článku implementuje jednoduchý soubor vCard formátování.</span><span class="sxs-lookup"><span data-stu-id="c47e7-113">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="c47e7-114">Přehled o tom, jak používat vlastní formátování</span><span class="sxs-lookup"><span data-stu-id="c47e7-114">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="c47e7-115">Zde jsou kroky, jak vytvořit a používat vlastní formátování:</span><span class="sxs-lookup"><span data-stu-id="c47e7-115">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="c47e7-116">Vytvořte třídu formátování výstupu, pokud chcete serializovat data k odeslání do klienta.</span><span class="sxs-lookup"><span data-stu-id="c47e7-116">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="c47e7-117">Vytvořte třídu vstupní formátovací modul, pokud chcete k deserializaci data přijatá z klienta.</span><span class="sxs-lookup"><span data-stu-id="c47e7-117">Create an input formatter class if you want to deserialize data received from the client.</span></span> 
* <span data-ttu-id="c47e7-118">Přidání instance formátovací moduly, které `InputFormatters` a `OutputFormatters` kolekcí v [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="c47e7-118">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="c47e7-119">Následující části obsahují pokyny a příklady kódu pro každý z těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="c47e7-119">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="c47e7-120">Jak vytvořit vlastní formátování třídu</span><span class="sxs-lookup"><span data-stu-id="c47e7-120">How to create a custom formatter class</span></span>

<span data-ttu-id="c47e7-121">Pokud chcete vytvořit formátování:</span><span class="sxs-lookup"><span data-stu-id="c47e7-121">To create a formatter:</span></span>

* <span data-ttu-id="c47e7-122">Třída odvozena od příslušné základní třídy.</span><span class="sxs-lookup"><span data-stu-id="c47e7-122">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="c47e7-123">Zadejte platné médium typy a kódování v konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="c47e7-123">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="c47e7-124">Přepsání `CanReadType` / `CanWriteType` metody</span><span class="sxs-lookup"><span data-stu-id="c47e7-124">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="c47e7-125">Přepsání `ReadRequestBodyAsync` / `WriteResponseBodyAsync` metody</span><span class="sxs-lookup"><span data-stu-id="c47e7-125">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="c47e7-126">Odvozena od příslušné základní třídy</span><span class="sxs-lookup"><span data-stu-id="c47e7-126">Derive from the appropriate base class</span></span>

<span data-ttu-id="c47e7-127">Text typy médií (například soubor vCard), jsou odvozeny od [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) nebo [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) základní třídy.</span><span class="sxs-lookup"><span data-stu-id="c47e7-127">For text media types (for example, vCard), derive from the [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="c47e7-128">Pro binární typy jsou odvozeny od [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) nebo [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) základní třídy.</span><span class="sxs-lookup"><span data-stu-id="c47e7-128">For binary types, derive from the [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="c47e7-129">Zadejte platné médium typy a kódování</span><span class="sxs-lookup"><span data-stu-id="c47e7-129">Specify valid media types and encodings</span></span>

<span data-ttu-id="c47e7-130">V konstruktoru, určete přidáním do platné médium typy a kódování `SupportedMediaTypes` a `SupportedEncodings` kolekce.</span><span class="sxs-lookup"><span data-stu-id="c47e7-130">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> <span data-ttu-id="c47e7-131">Nelze provést konstruktor vkládání závislostí v třída formátovacího modulu.</span><span class="sxs-lookup"><span data-stu-id="c47e7-131">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="c47e7-132">Například nelze získat protokolovacího nástroje přidáním protokolovacího nástroje parametr konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="c47e7-132">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="c47e7-133">Pro přístup ke službám, budete muset použít objektu context, který získá předané do vaší metody.</span><span class="sxs-lookup"><span data-stu-id="c47e7-133">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="c47e7-134">Příklad kódu [pod](#read-write) ukazuje, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="c47e7-134">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="c47e7-135">Přepsání CanReadType/CanWriteType</span><span class="sxs-lookup"><span data-stu-id="c47e7-135">Override CanReadType/CanWriteType</span></span> 

<span data-ttu-id="c47e7-136">Zadejte typ můžete deserializovat do nebo z serializovat přepsáním `CanReadType` nebo `CanWriteType` metody.</span><span class="sxs-lookup"><span data-stu-id="c47e7-136">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="c47e7-137">Například může být pouze nebudete moct vytvořit soubor vCard text ze `Contact` a naopak.</span><span class="sxs-lookup"><span data-stu-id="c47e7-137">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="c47e7-138">Metoda CanWriteResult</span><span class="sxs-lookup"><span data-stu-id="c47e7-138">The CanWriteResult method</span></span>

<span data-ttu-id="c47e7-139">V některých případech je nutné přepsat `CanWriteResult` místo `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="c47e7-139">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="c47e7-140">Použití `CanWriteResult` Pokud jsou splněny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="c47e7-140">Use `CanWriteResult` if the following conditions are true:</span></span>

  * <span data-ttu-id="c47e7-141">Vaše metoda akce vrací třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="c47e7-141">Your action method returns a model class.</span></span>
  * <span data-ttu-id="c47e7-142">Existují odvozené třídy, které může být vrácena za běhu.</span><span class="sxs-lookup"><span data-stu-id="c47e7-142">There are derived classes which might be returned at runtime.</span></span>
  * <span data-ttu-id="c47e7-143">Je třeba vědět za běhu, který odvozené, že byla vrácena třída akce.</span><span class="sxs-lookup"><span data-stu-id="c47e7-143">You need to know at runtime which derived class was returned by the action.</span></span>  

<span data-ttu-id="c47e7-144">Předpokládejme například, vrátí podpis metody akce `Person` typ, ale může vrátit `Student` nebo `Instructor` typ odvozený z `Person`.</span><span class="sxs-lookup"><span data-stu-id="c47e7-144">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="c47e7-145">Pokud chcete, aby vaše formátování, které bude zpracovávat jenom `Student` objekty, zkontrolujte typ [objekt](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) v kontextu objektu poskytnuté `CanWriteResult` metoda.</span><span class="sxs-lookup"><span data-stu-id="c47e7-145">If you want your formatter to handle only `Student` objects, check the type of [Object](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="c47e7-146">Všimněte si, že není potřeba použít `CanWriteResult` při vrácení metody akce `IActionResult`; v takovém případě `CanWriteType` metoda přijímá typ modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="c47e7-146">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="c47e7-147">Přepsání ReadRequestBodyAsync/WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="c47e7-147">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span> 

<span data-ttu-id="c47e7-148">Vykonávají samotnou práci deserializaci nebo serializace ve `ReadRequestBodyAsync` nebo `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="c47e7-148">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span>  <span data-ttu-id="c47e7-149">Zvýrazněné řádky v následujícím příkladu ukazují, jak získat služby z kontejneru pro vkládání závislosti (je již nelze získat z konstruktoru parametrů).</span><span class="sxs-lookup"><span data-stu-id="c47e7-149">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="c47e7-150">Postup konfigurace MVC používat vlastní formátování</span><span class="sxs-lookup"><span data-stu-id="c47e7-150">How to configure MVC to use a custom formatter</span></span>
 
<span data-ttu-id="c47e7-151">Pokud chcete používat vlastní formátovací modul, přidejte instance třídy pro formátování `InputFormatters` nebo `OutputFormatters` kolekce.</span><span class="sxs-lookup"><span data-stu-id="c47e7-151">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="c47e7-152">Formátovací moduly jsou vyhodnocovány v pořadí, v jakém že jste je vložili.</span><span class="sxs-lookup"><span data-stu-id="c47e7-152">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="c47e7-153">První z nich má přednost před.</span><span class="sxs-lookup"><span data-stu-id="c47e7-153">The first one takes precedence.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="c47e7-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c47e7-154">Next steps</span></span>

<span data-ttu-id="c47e7-155">Najdete v článku [ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), který implementuje jednoduchý soubor vCard vstup a výstup formátování.</span><span class="sxs-lookup"><span data-stu-id="c47e7-155">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span>  <span data-ttu-id="c47e7-156">Aplikace čte a zapisuje vCard, který vypadat podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c47e7-156">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="c47e7-157">Chcete-li zobrazit soubor vCard výstup, spusťte aplikaci a odešlete požadavek Get s přijmout záhlaví "text/vcard" k `http://localhost:63313/api/contacts/` (při spuštění ze sady Visual Studio) nebo `http://localhost:5000/api/contacts/` (při spuštění z příkazového řádku).</span><span class="sxs-lookup"><span data-stu-id="c47e7-157">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="c47e7-158">Pro přidání vizitky ke kolekci v paměti kontaktů, odeslat požadavek Post na stejnou adresu URL, s hlavičku Content-Type "text/vcard" a soubor vCard text v těle naformátován jako v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="c47e7-158">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
