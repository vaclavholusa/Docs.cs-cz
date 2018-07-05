---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Vazby parametrů ve webovém rozhraní API technologie ASP.NET | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7519ad92334690817ae64b994762fd68e76ebc9e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377806"
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="a2b1a-102">Parametr vazby v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="a2b1a-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="a2b1a-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a2b1a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a2b1a-104">Když webové rozhraní API volá metodu na řadiči, je nutné nastavit hodnoty pro parametry procesu nazývaného *vazby*.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="a2b1a-105">Tento článek popisuje, jak webové rozhraní API vytvoří vazbu parametrů a jak můžete přizpůsobit proces vytváření vazby.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="a2b1a-106">Ve výchozím nastavení používá webového rozhraní API pro svázání parametrů následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="a2b1a-107">Pokud je parametr typu "simple", se pokusí získat hodnotu z identifikátoru URI webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="a2b1a-108">Jednoduché typy zahrnují .NET [primitivní typy](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**a tak dále), a navíc **TimeSpan**, **Data a času**, **Guid**, **desítkové**, a **řetězec**, *plus* jakékoli Typ s konvertor typu, který lze převést z řetězce.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="a2b1a-109">(Další informace o typ převaděče později.)</span><span class="sxs-lookup"><span data-stu-id="a2b1a-109">(More about type converters later.)</span></span>
- <span data-ttu-id="a2b1a-110">Pro komplexní typy, webové rozhraní API se pokusí načíst hodnotu z textu zprávy, použití [formátovací modul typu média](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="a2b1a-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="a2b1a-111">Tady je příklad typické metody kontroleru webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="a2b1a-112">*Id* parametr je &quot;jednoduché&quot; zadejte, takže webového rozhraní API se pokusí získat hodnotu z identifikátoru URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="a2b1a-113">*Položky* parametr je komplexní typ, takže webového rozhraní API používá formátovací modul typu média načíst hodnotu z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="a2b1a-114">Pro získání hodnoty z identifikátoru URI, webové rozhraní API vyhledá v datech trasy a řetězci dotazu identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="a2b1a-115">Data trasy, která se zaplní při směrování systém analyzuje identifikátoru URI a odpovídá trasu.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="a2b1a-116">Další informace najdete v tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="a2b1a-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="a2b1a-117">Ve zbývající části tohoto článku ukážeme, jak můžete přizpůsobit proces vytváření vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="a2b1a-118">Pro komplexní typy ale zvažte formátovací moduly typu médií, kdykoli je to možné.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="a2b1a-119">Klíčovým principem HTTP je, že prostředky se odesílají v textu zprávy, použití vyjednávání obsahu k určení reprezentaci prostředku.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="a2b1a-120">Formátovací moduly typu médií byly navrženy pro přesně tento účel.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="a2b1a-121">Použití [FromUri]</span><span class="sxs-lookup"><span data-stu-id="a2b1a-121">Using [FromUri]</span></span>

<span data-ttu-id="a2b1a-122">Chcete-li vynutit webového rozhraní API pro čtení komplexní typ z identifikátoru URI, přidejte **[FromUri]** atribut parametru.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="a2b1a-123">Následující příklad definuje `GeoPoint` typ spolu s řadiči metodu, která získá `GeoPoint` z identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="a2b1a-124">Klienta můžete umístit hodnoty zeměpisné šířky a délky v řetězci dotazu a webového rozhraní API je použít k vytvoření `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="a2b1a-125">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="a2b1a-126">Použití [FromBody]</span><span class="sxs-lookup"><span data-stu-id="a2b1a-126">Using [FromBody]</span></span>

<span data-ttu-id="a2b1a-127">Chcete-li vynutit webového rozhraní API pro jednoduchý typ. číst z textu požadavku, přidejte **[FromBody]** atribut parametru:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="a2b1a-128">V tomto příkladu webového rozhraní API používat formátovací modul typu média načíst hodnotu z *název* z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="a2b1a-129">Tady je příklad žádosti klienta.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="a2b1a-130">Pokud má parametr [FromBody], webového rozhraní API používá hlavičku Content-Type pro výběr formátovacím modulem.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="a2b1a-131">V tomto příkladu je typem obsahu &quot;application/json&quot; a text požadavku má Nezpracovaný řetězec formátu JSON (není objekt JSON).</span><span class="sxs-lookup"><span data-stu-id="a2b1a-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="a2b1a-132">Maximálně jeden parametr smí číst z textu zprávy.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="a2b1a-133">Takže to nebude fungovat:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="a2b1a-134">Důvod pro toto pravidlo je, že text požadavku můžou být uložená v datovém proudu bez vyrovnávací paměti, který může číst pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="a2b1a-135">Převaděče typů</span><span class="sxs-lookup"><span data-stu-id="a2b1a-135">Type Converters</span></span>

<span data-ttu-id="a2b1a-136">Provedete webového rozhraní API třídy považovat jednoduchý typ (tak, aby webové rozhraní API se pokusí navázat z identifikátoru URI) tak, že vytvoříte **TypeConverter** a převodu řetězce.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="a2b1a-137">Následující kód ukazuje `GeoPoint` třídu, která představuje bod zeměpisné navíc **TypeConverter** , která převede z řetězce na `GeoPoint` instancí.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="a2b1a-138">`GeoPoint` Třídy je doplněn **[TypeConverter]** atributy konvertor typu.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="a2b1a-139">(V tomto příkladu se inspirovat Mike koutů blogový příspěvek [jak vytvořit vazbu na vlastních objektech v signaturách akce v MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="a2b1a-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="a2b1a-140">Nyní bude zacházet s webovým rozhraním API `GeoPoint` jako jednoduchý typ., což znamená, ho se pokusí vytvořit vazbu `GeoPoint` parametry z identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="a2b1a-141">Nemusíte zahrnovat **[FromUri]** u parametru.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="a2b1a-142">Klient může vyvolat metodu s identifikátorem URI tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="a2b1a-143">Vazače modelů</span><span class="sxs-lookup"><span data-stu-id="a2b1a-143">Model Binders</span></span>

<span data-ttu-id="a2b1a-144">Víc možností než konvertor typu je vytvořit vlastní vazač modelu.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="a2b1a-145">Díky vazač modelu máte přístup k objektům, jako požadavek HTTP, popis akce a nezpracované hodnoty z dat trasy.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="a2b1a-146">Chcete-li vytvořit vazač modelu, implementovat **IModelBinder** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="a2b1a-147">Toto rozhraní definuje jedinou metodu **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="a2b1a-148">Tady je vazač modelu pro `GeoPoint` objekty.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="a2b1a-149">Získá vazač modelu nezpracované vstupní hodnoty z *zprostředkovatele hodnot*.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="a2b1a-150">Tento návrh odděluje dvě různé funkce:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="a2b1a-151">Zprostředkovatel hodnoty trvá požadavek HTTP a naplní slovník párů klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="a2b1a-152">Vazač modelu používá k naplnění modelu tohoto slovníku.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="a2b1a-153">Výchozí zprostředkovatel hodnoty v rozhraní Web API získá hodnoty z dat trasy a řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="a2b1a-154">Například, pokud je identifikátor URI `http://localhost/api/values/1?location=48,-122`, zprostředkovatele hodnot vytvoří následující páry klíč hodnota:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="a2b1a-155">ID = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="a2b1a-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="a2b1a-156">umístění = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="a2b1a-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="a2b1a-157">(I jsem za předpokladu, že výchozí šablona trasy, která je &quot;rozhraní api / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="a2b1a-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="a2b1a-158">Název parametru pro vazbu je uložen v **ModelBindingContext.ModelName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="a2b1a-159">Vazač modelu hledá klíče této hodnoty ve slovníku.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="a2b1a-160">Pokud hodnota existuje a mohou být převedeny na `GeoPoint`, vazače modelu přiřadí hodnotu do proměnné vázané **ModelBindingContext.Model** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="a2b1a-161">Všimněte si, že není omezena pouze na převod jednoduchý typ vazače modelu.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="a2b1a-162">V tomto příkladu nejprve hledá vazače modelu v tabulce ze známého umístění a pokud selže, používá převod typů.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="a2b1a-163">**Nastavení vazače modelu**</span><span class="sxs-lookup"><span data-stu-id="a2b1a-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="a2b1a-164">Existuje několik způsobů, jak nastavit vazač modelu.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="a2b1a-165">Nejprve můžete přidat **[ModelBinder]** atribut parametru.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="a2b1a-166">Můžete také přidat **[ModelBinder]** atribut typu.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="a2b1a-167">Webové rozhraní API používat vazač modelu zadaného pro všechny parametry typu.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="a2b1a-168">Nakonec můžete přidat poskytovatel vazač modelu pro **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="a2b1a-169">Poskytovatel vazač modelu je jednoduše třídu objektů factory vytvoří vazač modelu.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="a2b1a-170">Můžete vytvořit poskytovatele odvozením z [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="a2b1a-171">Pokud vaše vazač modelu zpracovává jeden typ, je ale jednodušší použít integrovaný **SimpleModelBinderProvider**, která je určená pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="a2b1a-172">Následující kód ukazuje, jak to provést.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="a2b1a-173">U nějakého poskytovatele vazby modelu je stále potřeba přidat **[ModelBinder]** atribut parametru webového rozhraní API říct, že by měl používat vazač modelu a formátování typu média.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="a2b1a-174">Ale nyní není nutné zadat typ vazače modelu v atributu:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="a2b1a-175">Zprostředkovatele hodnot</span><span class="sxs-lookup"><span data-stu-id="a2b1a-175">Value Providers</span></span>

<span data-ttu-id="a2b1a-176">Jsem se zmiňoval, že získá vazač modelu hodnot zprostředkovatele hodnot.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="a2b1a-177">Chcete-li napsat vlastní hodnotu zprostředkovatele, implementovat **IValueProvider** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="a2b1a-178">Tady je příklad, který načítá hodnoty ze souborů cookie v požadavku:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="a2b1a-179">Musíte také vytvořit factory zprostředkovatele hodnot odvozené ze **ValueProviderFactory** třídy.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="a2b1a-180">Přidat factory zprostředkovatele hodnot pro **HttpConfiguration** následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="a2b1a-181">Webové rozhraní API lze kombinovat všech zprostředkovatelů hodnot, takže když volá vazač modelu **ValueProvider.GetValue**, vazače modelu přijímá hodnotu z první zprostředkovatele hodnot, aby bylo možné ji vytvořit.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="a2b1a-182">Alternativně můžete nastavit factory zprostředkovatele hodnot na úrovni parametr pomocí **položku ValueProvider** atribut následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="a2b1a-183">To říká webového rozhraní API pro použití vazby modelu s factory zprostředkovatele hodnot pro zadaný a ne k používání některé z dalších poskytovatelů registrované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="a2b1a-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="a2b1a-184">HttpParameterBinding</span></span>

<span data-ttu-id="a2b1a-185">Vazače modelů jsou konkrétní instanci obecnější mechanismus.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="a2b1a-186">Když se podíváte na **[ModelBinder]** atribut, zobrazí se, že se odvozuje od abstraktní **ParameterBindingAttribute** třídy.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="a2b1a-187">Tato třída definuje jedinou metodu **GetBinding**, která vrátí **HttpParameterBinding** objektu:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="a2b1a-188">**HttpParameterBinding** zodpovídá za vazby parametru na hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="a2b1a-189">V případě třídy **[ModelBinder]**, atribut vrátí **HttpParameterBinding** implementace, která se používá **IModelBinder** provádět skutečná vazba.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="a2b1a-190">Můžete také implementovat vlastní **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="a2b1a-191">Předpokládejme například, že chcete získat značek etag z `if-match` a `if-none-match` hlavičky v požadavku.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="a2b1a-192">Začneme tak, že definujete třídu k vyjádření značek ETag.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="a2b1a-193">Budeme také definovat výčet, který označuje, jestli se má získat značku ETag ze `if-match` záhlaví nebo `if-none-match` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="a2b1a-194">Tady je **HttpParameterBinding** , který získá z požadovaného záhlaví ETag a provádí vazbu na parametr typu ETag:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="a2b1a-195">**ExecuteBindingAsync** metoda nemá vazbu.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="a2b1a-196">V rámci této metody přidat hodnotu vazby parametru **ActionArgument** slovníku **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="a2b1a-197">Pokud vaše **ExecuteBindingAsync** metoda načte text zprávy s požadavkem, přepsat **WillReadBody** vlastnost vrátí hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="a2b1a-198">Text žádosti může být, že bez vyrovnávací paměti datového proudu, který lze číst pouze jednou, takže webového rozhraní API vynucuje pravidla, že nejvýše jedna vazba můžete číst hlavní část textu zprávy.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="a2b1a-199">Chcete-li použít vlastní **HttpParameterBinding**, můžete definovat, která je odvozena z atributu **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="a2b1a-200">Pro `ETagParameterBinding`, budeme definovat dva atributy, jeden pro `if-match` záhlaví a jeden pro `if-none-match` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="a2b1a-201">Obě jsou odvozeny od abstraktní základní třídy.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="a2b1a-202">Tady je metoda kontroleru, který používá `[IfNoneMatch]` atribut.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="a2b1a-203">Kromě **ParameterBindingAttribute**, existuje jiný vidlici pro přidání vlastního **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="a2b1a-204">Na **HttpConfiguration** objektu, **ParameterBindingRules** vlastnost je kolekce anomymous funkce typu (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="a2b1a-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="a2b1a-205">Například můžete přidat pravidlo, které používá žádné značky ETag parametry pro metodu GET `ETagParameterBinding` s `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="a2b1a-206">Funkce by měla vrátit `null` pro parametry, pokud vazba není použitelný.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="a2b1a-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="a2b1a-207">IActionValueBinder</span></span>

<span data-ttu-id="a2b1a-208">Celý proces vazbu parametru je řízen modulární služby **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="a2b1a-209">Výchozí implementace **IActionValueBinder** provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="a2b1a-210">Vyhledejte **ParameterBindingAttribute** u parametru.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="a2b1a-211">Jedná se o **[FromBody]**, **[FromUri]**, a **[ModelBinder]**, nebo vlastní atributy.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="a2b1a-212">V opačném případě Hledat v **HttpConfiguration.ParameterBindingRules** pro funkci, která vrací jinou hodnotu null **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="a2b1a-213">Jinak použijte výchozí pravidla, které můžu je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="a2b1a-214">Pokud typ parametru je "jednoduchý" nebo má konvertor typu, navázat z identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="a2b1a-215">Jedná se o ekvivalent při vložení **[FromUri]** atribut parametru.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="a2b1a-216">V opačném případě pokusu o čtení parametr z textu zprávy.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="a2b1a-217">Jedná se o ekvivalent při vložení **[FromBody]** u parametru.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="a2b1a-218">Pokud byste chtěli, může nahradit celou **IActionValueBinder** služba s vlastní implementaci.</span><span class="sxs-lookup"><span data-stu-id="a2b1a-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a2b1a-219">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="a2b1a-219">Additional Resources</span></span>

[<span data-ttu-id="a2b1a-220">Ukázka vlastního parametru vazby</span><span class="sxs-lookup"><span data-stu-id="a2b1a-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="a2b1a-221">Pozastavení provádění získání Mike napsal dobré řadě blogových příspěvků o vazbu parametru webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="a2b1a-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="a2b1a-222">Jak webové rozhraní API nepodporuje parametr vazby</span><span class="sxs-lookup"><span data-stu-id="a2b1a-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="a2b1a-223">Vazby parametru styl MVC pro webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="a2b1a-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="a2b1a-224">Jak vytvořit vazbu na vlastních objektech v signaturách akce v MVC nebo webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="a2b1a-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="a2b1a-225">Jak vytvořit vlastní hodnotu zprostředkovatele v rozhraní Web API</span><span class="sxs-lookup"><span data-stu-id="a2b1a-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="a2b1a-226">Vazby parametru webového rozhraní API pohled pod kapotu</span><span class="sxs-lookup"><span data-stu-id="a2b1a-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
