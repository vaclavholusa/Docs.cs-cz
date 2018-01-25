---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: "Parametr vazby v rozhraní ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="9bcbb-102">Parametr vazby v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="9bcbb-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="9bcbb-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9bcbb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9bcbb-104">Když webového rozhraní API volá metodu na řadič, je nutné nastavit hodnoty pro parametry procesu označovaného jako *vazby*.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="9bcbb-105">Tento článek popisuje, jak webové rozhraní API váže parametry a jak můžete přizpůsobit proces vytváření vazby.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="9bcbb-106">Ve výchozím nastavení používá webového rozhraní API pro svázání parametrů následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="9bcbb-107">Pokud má parametr hodnotu typu "jednoduché", se pokusí získat hodnotu z identifikátoru URI webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="9bcbb-108">Jednoduché typy patří .NET [primitivní typy](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **dvojité**, a tak dále), plus **časový interval**, **Data a času**, **Guid**, **decimal**, a **řetězec**, *plus* žádné Zadejte pomocí převaděče typu, který můžete převést na řetězec.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="9bcbb-109">(Další informace o převaděče typů později.)</span><span class="sxs-lookup"><span data-stu-id="9bcbb-109">(More about type converters later.)</span></span>
- <span data-ttu-id="9bcbb-110">Pro komplexní typy, webové rozhraní API se pokusí načíst hodnotu z textu zprávy, použití [formátovací modul typu média](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="9bcbb-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="9bcbb-111">Tady je příklad typické metoda kontroleru webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="9bcbb-112">*Id* parametr &quot;jednoduché&quot; typ, takže webového rozhraní API se pokusí získat hodnotu z identifikátoru URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="9bcbb-113">*Položky* parametr je komplexního typu, takže webového rozhraní API používá formátovací modul typu média pro čtení hodnotu z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="9bcbb-114">K získání hodnoty z identifikátoru URI, vypadá webového rozhraní API v datech trasy a řetězce dotazu identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="9bcbb-115">Data trasy, která se zaplní při směrování systém analyzuje identifikátor URI a odpovídá trasu.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="9bcbb-116">Další informace najdete v tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="9bcbb-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="9bcbb-117">Ve zbývající části tohoto článku I budete ukazují, jak můžete přizpůsobit procesu vázání modelu.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="9bcbb-118">Pro komplexní typy ale používejte formátovací moduly typu média, kdykoli je to možné.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="9bcbb-119">Klíčovým principem HTTP je, že prostředky jsou zasílány v textu zprávy zadejte reprezentace prostředku pomocí vyjednávání obsahu.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="9bcbb-120">Formátovací moduly typu média byly navrženy pro přesně tento účel.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="9bcbb-121">Použití [FromUri]</span><span class="sxs-lookup"><span data-stu-id="9bcbb-121">Using [FromUri]</span></span>

<span data-ttu-id="9bcbb-122">Chcete-li vynutit webového rozhraní API ke čtení pro komplexní typ z identifikátoru URI, přidejte **[FromUri]** atribut parametr.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="9bcbb-123">V následujícím příkladu definuje `GeoPoint` typu, společně s metoda kontroleru, který získá `GeoPoint` z identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="9bcbb-124">Klienta můžete vložit hodnoty zeměpisné šířky a délky v řetězci dotazu a webového rozhraní API je použít k vytvoření `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="9bcbb-125">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="9bcbb-126">Použití [FromBody]</span><span class="sxs-lookup"><span data-stu-id="9bcbb-126">Using [FromBody]</span></span>

<span data-ttu-id="9bcbb-127">Chcete-li vynutit webového rozhraní API pro jednoduchý typ. číst z textu požadavku, přidejte **[FromBody]** atribut parametr:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="9bcbb-128">V tomto příkladu webového rozhraní API použije formátovací modul typu média číst hodnotu *název* z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="9bcbb-129">Zde je požadavek klienta příklad.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="9bcbb-130">Pokud je parametr [FromBody], webového rozhraní API hlavičku Content-Type použije k výběru formátovacího modulu.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="9bcbb-131">V tomto příkladu je typ obsahu &quot;application/json&quot; a textu žádosti je nezpracovaný řetězec formátu JSON (není objekt JSON).</span><span class="sxs-lookup"><span data-stu-id="9bcbb-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="9bcbb-132">Číst z textu zprávy je povoleno maximálně jeden parametr.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="9bcbb-133">Proto to nebude fungovat:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="9bcbb-134">Z důvodu pro toto pravidlo je, že textu žádosti se pravděpodobně uloží do datového proudu bez vyrovnávací paměti, který může číst pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="9bcbb-135">Převaděče typů</span><span class="sxs-lookup"><span data-stu-id="9bcbb-135">Type Converters</span></span>

<span data-ttu-id="9bcbb-136">Webové rozhraní API třídy považovat jednoduchého typu (tak, aby webového rozhraní API se pokusí navázat jej z identifikátoru URI) můžete provést vytvořením **TypeConverter** a poskytování převod řetězce.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="9bcbb-137">Následující kód ukazuje `GeoPoint` třídu, která představuje bod zeměpisné plus **TypeConverter** , která převádí z řetězce `GeoPoint` instance.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="9bcbb-138">`GeoPoint` Třída je upraven pomocí **[TypeConverter]** atribut k určení převaděč typů.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="9bcbb-139">(Tento příklad byl INSPIROVANÉ Karel místo příspěvku na blogu [postupy k vytvoření vazby na vlastní objekty v podpisy akce v MVC nebo WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="9bcbb-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="9bcbb-140">Nyní bude považovat webového rozhraní API `GeoPoint` jako jednoduchý typ, což znamená, se pokusí navázat `GeoPoint` parametry z identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="9bcbb-141">Nemusíte zahrnovat **[FromUri]** na parametru.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="9bcbb-142">Klienta můžete vyvolat metodu s identifikátorem URI takto:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="9bcbb-143">Vazače modelů</span><span class="sxs-lookup"><span data-stu-id="9bcbb-143">Model Binders</span></span>

<span data-ttu-id="9bcbb-144">Flexibilnější možnost než převaděče typů je vytvořit vlastní vazač modelu.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="9bcbb-145">S vazač modelu máte přístup k objektům, jako požadavek HTTP, popis akce a nezpracované hodnoty z dat trasy.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="9bcbb-146">Pokud chcete vytvořit vazač modelu, implementovat **IModelBinder** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="9bcbb-147">Toto rozhraní definuje jedinou metodu **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="9bcbb-148">Tady je vazač modelu pro `GeoPoint` objekty.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="9bcbb-149">Získá vazač modelu nezpracované vstupní hodnoty z *zprostředkovatele hodnot*.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="9bcbb-150">Tento návrh odděluje dvě odlišné funkce:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="9bcbb-151">Zprostředkovatel hodnoty přijímá požadavky protokolu HTTP a naplní slovník páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="9bcbb-152">Vazač modelu používá k naplnění modelu tohoto slovníku.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="9bcbb-153">Výchozí zprostředkovatele hodnot v rozhraní Web API získá hodnoty z data trasy a řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="9bcbb-154">Například pokud je identifikátor URI `http://localhost/api/values/1?location=48,-122`, zprostředkovatele hodnot vytvoří následující páry klíč hodnota:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="9bcbb-155">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="9bcbb-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="9bcbb-156">umístění = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="9bcbb-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="9bcbb-157">(I mě za předpokladu, že výchozí šablonu trasy, která je &quot;rozhraní api nebo {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="9bcbb-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="9bcbb-158">Název parametru pro vazbu je uložen v **ModelBindingContext.ModelName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="9bcbb-159">Vazač modelu vyhledá klíč se tato hodnota ve slovníku.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="9bcbb-160">Pokud hodnota existuje a mohou být převedeny na `GeoPoint`, přiřadí vázaná hodnota pro vazač modelu **ModelBindingContext.Model** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="9bcbb-161">Všimněte si, že vazač modelu není omezeno na převod jednoduchého typu.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="9bcbb-162">V tomto příkladu vazač modelu nejprve hledá v tabulku známé umístění a pokud to nepomůže, používá převod typů.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="9bcbb-163">**Nastavení vazač modelu**</span><span class="sxs-lookup"><span data-stu-id="9bcbb-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="9bcbb-164">Existuje několik způsobů, jak nastavit vazač modelu.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="9bcbb-165">První, můžete přidat **[ModelBinder]** atribut parametr.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="9bcbb-166">Můžete také přidat **[ModelBinder]** atributu typu.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="9bcbb-167">Webové rozhraní API použije vazač modelu zadaného pro všechny parametry daného typu.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="9bcbb-168">Nakonec můžete přidat zprostředkovatele vazače modelu, aby **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="9bcbb-169">Zprostředkovatele vazače modelu je jednoduše objekt pro vytváření třídy, která vytvoří vazač modelu.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="9bcbb-170">Můžete vytvořit poskytovatele odvozování z [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="9bcbb-171">Pokud vaše vazač modelu zpracovává jeden typ, je však jednodušší použít integrované **SimpleModelBinderProvider**, která je určená pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="9bcbb-172">Následující kód ukazuje, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="9bcbb-173">Ke zprostředkovateli vazby modelu, je stále nutné přidat **[ModelBinder]** atribut parametr webového rozhraní API říct, že mají používat vazač modelu a ne formátovací modul typu média.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="9bcbb-174">Ale nyní nemusíte určit typ vazače modelu v atributu:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="9bcbb-175">Zprostředkovatele hodnot</span><span class="sxs-lookup"><span data-stu-id="9bcbb-175">Value Providers</span></span>

<span data-ttu-id="9bcbb-176">Mohu uvedené, že získá vazač modelu hodnoty z zprostředkovatele hodnot.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="9bcbb-177">Zapisovat zprostředkovatele vlastní hodnoty, implementovat **IValueProvider** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="9bcbb-178">Tady je příklad, který vrátí hodnoty ze souborů cookie v požadavku:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="9bcbb-179">Musíte taky vytvořit factory zprostředkovatele hodnot odvozené z **ValueProviderFactory** třídy.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="9bcbb-180">Přidat factory zprostředkovatele hodnot na **HttpConfiguration** následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="9bcbb-181">Webové rozhraní API vytvoří všech zprostředkovatelů hodnot, takže když vazač modelu volá **ValueProvider.GetValue**, vazač modelu obdrží hodnotu z první zprostředkovatele hodnot, který se bude moct ji vytvořit.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="9bcbb-182">Alternativně můžete nastavit factory zprostředkovatele hodnot na úrovni parametr pomocí **položku ValueProvider** atribut následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="9bcbb-183">Tato hodnota informuje webového rozhraní API pro použití vazby modelu s objekt pro vytváření zprostředkovatelů zadanou hodnotu a nechcete použít některou z jiných poskytovatelů registrované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="9bcbb-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="9bcbb-184">HttpParameterBinding</span></span>

<span data-ttu-id="9bcbb-185">Vazače modelů jsou konkrétní instanci obecnější mechanismus.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="9bcbb-186">Pokud si prohlédnete **[ModelBinder]** atribut, zobrazí se, že je odvozena z abstraktní **ParameterBindingAttribute** třídy.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="9bcbb-187">Tato třída definuje jedinou metodu **GetBinding**, která vrátí **HttpParameterBinding** objektu:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="9bcbb-188">**HttpParameterBinding** zodpovídá za vytvoření vazby parametru na hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="9bcbb-189">U **[ModelBinder]**, atribut vrátí **HttpParameterBinding** implementace, která se používá **IModelBinder** k provedení skutečná vazba.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="9bcbb-190">Můžete taky implementovat vlastní **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="9bcbb-191">Předpokládejme například, že chcete získat značky etag binárním rozsáhlým z `if-match` a `if-none-match` hlavičky v požadavku.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="9bcbb-192">Začneme definováním třídy k reprezentaci značky etag binárním rozsáhlým.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="9bcbb-193">Budeme také definovat výčet, který označuje, jestli chcete-li získat ETag z `if-match` záhlaví nebo `if-none-match` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="9bcbb-194">Tady je **HttpParameterBinding** , získá z požadované hlavičky značky ETag a sváže s parametrem typu značka ETag:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="9bcbb-195">**ExecuteBindingAsync** metoda nemá vazbu.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="9bcbb-196">V rámci této metody přidat hodnotu parametru vázané **ActionArgument** slovníku v **objekt HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="9bcbb-197">Pokud vaše **ExecuteBindingAsync** metoda čtení textu zprávy požadavku, přepsat **WillReadBody** vlastnosti tak, aby vrátila hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="9bcbb-198">Text žádosti může být, že bez vyrovnávací paměti datový proud, který lze číst pouze jednou, takže webového rozhraní API vynucuje pravidlo, že maximálně jeden vazby může číst obsah zprávy.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="9bcbb-199">Chcete-li použít vlastní **HttpParameterBinding**, můžete definovat atribut, který je odvozen od **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="9bcbb-200">Pro `ETagParameterBinding`, budeme definovat dva atributy, jeden pro `if-match` hlavičky a jeden pro `if-none-match` hlavičky.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="9bcbb-201">Obě odvozen od abstraktní základní třídu.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="9bcbb-202">Tady je metoda kontroleru, který používá `[IfNoneMatch]` atribut.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="9bcbb-203">Kromě **ParameterBindingAttribute**, je jiná háku pro přidání vlastního **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="9bcbb-204">Na **HttpConfiguration** objekt, **ParameterBindingRules** vlastnost je kolekce anomymous funkce typu (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="9bcbb-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="9bcbb-205">Například můžete přidat pravidlo, které používá libovolný parametr ETag na metodu GET `ETagParameterBinding` s `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="9bcbb-206">Funkce by měla vrátit `null` pro parametry, kde vazby není použitelný.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="9bcbb-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="9bcbb-207">IActionValueBinder</span></span>

<span data-ttu-id="9bcbb-208">Modulární služby, se řídí celý proces vazbu parametru **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="9bcbb-209">Výchozí implementaci **IActionValueBinder** provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="9bcbb-210">Vyhledejte **ParameterBindingAttribute** na parametru.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="9bcbb-211">To zahrnuje **[FromBody]**, **[FromUri]**, a **[ModelBinder]**, nebo vlastní atributy.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="9bcbb-212">Jinak, vyhledejte v **HttpConfiguration.ParameterBindingRules** pro funkci, která vrátí hodnotu null **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="9bcbb-213">Jinak použijte výchozí pravidla, která I popsané.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="9bcbb-214">Pokud typ parametru "jednoduchý" nebo má typ převodník, vazbu z identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="9bcbb-215">Jde o ekvivalent uvedení **[FromUri]** atribut na parametru.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="9bcbb-216">Jinak zkuste parametr číst z textu zprávy.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="9bcbb-217">Jde o ekvivalent uvedení **[FromBody]** na parametru.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="9bcbb-218">Pokud byste chtěli, může nahradit celý **IActionValueBinder** služby s vlastní implementaci.</span><span class="sxs-lookup"><span data-stu-id="9bcbb-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9bcbb-219">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="9bcbb-219">Additional Resources</span></span>

[<span data-ttu-id="9bcbb-220">Ukázka vlastního parametru vazby</span><span class="sxs-lookup"><span data-stu-id="9bcbb-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="9bcbb-221">Jan místo napsali dobrý řadu příspěvcích na blogu o vazbu parametru webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="9bcbb-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="9bcbb-222">Jak funguje webového rozhraní API parametru vazby</span><span class="sxs-lookup"><span data-stu-id="9bcbb-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="9bcbb-223">Vazba parametru styl MVC pro webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9bcbb-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="9bcbb-224">Postup vytvoření vazby na vlastní objekty v podpisy akce v MVC nebo webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9bcbb-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="9bcbb-225">Jak vytvořit vlastní hodnotu poskytovatele v rozhraní Web API</span><span class="sxs-lookup"><span data-stu-id="9bcbb-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="9bcbb-226">Vazba parametru webového rozhraní API pod pokličkou</span><span class="sxs-lookup"><span data-stu-id="9bcbb-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
