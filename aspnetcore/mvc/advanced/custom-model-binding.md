---
title: "Vazby vlastní modelu"
author: ardalis
description: "Přizpůsobení vazby modelu v aplikaci ASP.NET MVC jádra."
manager: wpickett
ms.author: riande
ms.date: 04/10/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 4b59a70c488c08f7d3ebf0ea2344107cf3fe6eff
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="custom-model-binding"></a><span data-ttu-id="c1e4e-103">Vazby vlastní modelu</span><span class="sxs-lookup"><span data-stu-id="c1e4e-103">Custom Model Binding</span></span>

<span data-ttu-id="c1e4e-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="c1e4e-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="c1e4e-105">Vazby modelu umožňuje akce kontroleru pracovat přímo s modelu typy (předané jako argumenty metoda), spíš než požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="c1e4e-106">Mapování mezi modely dat a aplikací příchozí požadavek se zpracovává souborem vazače modelů.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="c1e4e-107">Vazba funkce integrované modelu mohou vývojáři rozšířit implementací vazače modelů vlastní (i když obvykle nemusíte napsat vlastního zprostředkovatele).</span><span class="sxs-lookup"><span data-stu-id="c1e4e-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="c1e4e-108">Zobrazení nebo stažení ukázky z webu GitHub</span><span class="sxs-lookup"><span data-stu-id="c1e4e-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="c1e4e-109">Výchozí omezení vazač modelu</span><span class="sxs-lookup"><span data-stu-id="c1e4e-109">Default model binder limitations</span></span>

<span data-ttu-id="c1e4e-110">Vazače modelů výchozí podporují většinu běžné typy dat .NET Core a by měl splňovat požadavky Většina vývojářů.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-110">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="c1e4e-111">Očekávané při vytváření vazby textový vstup z požadavku přímo do modelu typy.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="c1e4e-112">Možná budete muset transformace vstupu před jeho vazby.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="c1e4e-113">Pokud například máte klíč, který slouží k vyhledávání dat modelu.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="c1e4e-114">Můžete použít vlastní vazač modelu pro načtení dat. na základě klíče.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="c1e4e-115">Zkontrolujte vazby modelu</span><span class="sxs-lookup"><span data-stu-id="c1e4e-115">Model binding review</span></span>

<span data-ttu-id="c1e4e-116">Vazby modelu používá pro typy, které funguje na určité definice.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="c1e4e-117">A *jednoduchý typ* je převést z jednoho řetězce ve vstupu.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="c1e4e-118">A *komplexní typ* je převést z více vstupních hodnot.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="c1e4e-119">Rozhraní framework určuje rozdíl podle existenci `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="c1e4e-120">Doporučujeme vytvořit převaděč typu, pokud máte jednoduchou `string`  ->  `SomeType` mapování, která nevyžaduje externím prostředkům.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="c1e4e-121">Před vytvořením vlastní vlastní vazač modelu, je vhodné prostudovali jak existující model jsou implementované vazače.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="c1e4e-122">Vezměte v úvahu [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) kterých jde převést kódováním base64 řetězce na pole bajtů.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-122">Consider the [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="c1e4e-123">Bajtová pole jsou často uložené jako soubory nebo pole objektů BLOB databáze.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="c1e4e-124">Práce ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="c1e4e-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="c1e4e-125">Řetězce s kódováním base64 lze použít k reprezentování binární data.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="c1e4e-126">Například na následujícím obrázku může být kódovaný jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="c1e4e-127">![DotNet robota](custom-model-binding/images/bot.png "robota dotnet.")</span><span class="sxs-lookup"><span data-stu-id="c1e4e-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="c1e4e-128">Malá část řetězec s kódováním je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="c1e4e-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="c1e4e-129">![DotNet robota kódovaný](custom-model-binding/images/encoded-bot.png "kódovaný robota dotnet.")</span><span class="sxs-lookup"><span data-stu-id="c1e4e-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="c1e4e-130">Postupujte podle pokynů [tohoto příkladu README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) převést řetězec s kódováním base64 do souboru.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="c1e4e-131">Můžete využít řetězce kódováním base64 a používat rozhraní ASP.NET MVC jádra `ByteArrayModelBinder` ji převést do bajtového pole.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-131">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="c1e4e-132">[ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) které implementuje [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) mapuje `byte[]` argumenty, které mají `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="c1e4e-132">The [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

<span data-ttu-id="c1e4e-133">Při vytváření vlastní vlastní vazač modelu, můžete implementovat vlastní `IModelBinderProvider` zadejte, nebo použijte [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="c1e4e-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="c1e4e-134">Následující příklad ukazuje, jak používat `ByteArrayModelBinder` převést řetězec s kódováním base64 tak, aby `byte[]` a výsledek uložit do souboru:</span><span class="sxs-lookup"><span data-stu-id="c1e4e-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="c1e4e-135">Můžete odeslat řetězec s kódováním base64, pomocí této metody rozhraní api pomocí nástroje, například [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="c1e4e-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="c1e4e-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="c1e4e-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="c1e4e-137">Tak dlouho, dokud vazač svázat data požadavku na příslušně pojmenovaných vlastnosti nebo argumenty, bude úspěšné vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="c1e4e-138">Následující příklad ukazuje, jak používat `ByteArrayModelBinder` s modelem zobrazení:</span><span class="sxs-lookup"><span data-stu-id="c1e4e-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="c1e4e-139">Ukázky vazač vlastní modelu</span><span class="sxs-lookup"><span data-stu-id="c1e4e-139">Custom model binder sample</span></span>

<span data-ttu-id="c1e4e-140">V této části jsme budete implementovat vlastní vazač modelu který:</span><span class="sxs-lookup"><span data-stu-id="c1e4e-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="c1e4e-141">Převede příchozí data požadavku na silného typu klíče argumenty.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="c1e4e-142">Používá Entity Framework Core se načíst související entity.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="c1e4e-143">Předá přidružené entity jako argument pro metodu akce.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="c1e4e-144">Následující ukázkové používá `ModelBinder` atributu u `Author` modelu:</span><span class="sxs-lookup"><span data-stu-id="c1e4e-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="c1e4e-145">V předchozí kód `ModelBinder` atribut určuje typ `IModelBinder` který se používá k vytvoření vazby `Author` parametrů akcí.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="c1e4e-146">`AuthorEntityBinder` Se používá k vytvoření vazby `Author` parametr podle načtení entity ze zdroje dat pomocí Entity Framework Core a `authorId`:</span><span class="sxs-lookup"><span data-stu-id="c1e4e-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="c1e4e-147">Následující kód ukazuje způsob použití `AuthorEntityBinder` v metodu akce:</span><span class="sxs-lookup"><span data-stu-id="c1e4e-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="c1e4e-148">`ModelBinder` Atributu lze použít `AuthorEntityBinder` parametry, které nepoužívají výchozí konvence:</span><span class="sxs-lookup"><span data-stu-id="c1e4e-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="c1e4e-149">V tomto příkladu, protože název argument není výchozí `authorId`, je zadán parametr používání `ModelBinder` atribut.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-149">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="c1e4e-150">Všimněte si, že kontroleru a akce metoda jednodušší ve srovnání s vyhledávání entity v metodě akce.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="c1e4e-151">Logika pro načtení Autor pomocí Entity Framework Core je přesunuta do vazače modelu.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="c1e4e-152">To může být výrazně zjednodušení, pokud máte několik metod, které vytvořit vazbu na modelu vytvořit a pomáhají vám postupovat podle [SUCHÝCH Princip](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="c1e4e-152">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="c1e4e-153">Můžete použít `ModelBinder` atribut vlastnosti jednotlivých modelu (například na viewmodel) nebo pro parametry metody akce k určení vazač modelu nebo název modelu pro právě tento typ nebo akci.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="c1e4e-154">Implementace ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="c1e4e-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="c1e4e-155">Místo použití atributu, můžete implementovat `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="c1e4e-156">Toto je, jak jsou implementované vestavěnou architekturou vazače.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="c1e4e-157">Když zadáte typ vaší vazač funguje na, zadejte typ argumentu vytváří, **není** přijímá vaší vazač vstupu.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="c1e4e-158">Následující zprostředkovatele vazače pracuje `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="c1e4e-159">Při jejím přidání do kolekce MVC poskytovatelů, nemusíte používat `ModelBinder` atributu u `Author` nebo `Author` zadali parametry.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="c1e4e-160">Poznámka: Předchozí kód vrátí `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="c1e4e-161">`BinderTypeModelBinder` funguje jako objekt pro vytváření pro vazače modelů a poskytuje vkládání závislostí (DI).</span><span class="sxs-lookup"><span data-stu-id="c1e4e-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="c1e4e-162">`AuthorEntityBinder` Vyžaduje DI pro přístup k EF jádra.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="c1e4e-163">Použití `BinderTypeModelBinder` Pokud vaše vazač modelu vyžaduje služby od DI.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="c1e4e-164">Pokud chcete používat zprostředkovatele vazače modelu vlastní, přidejte ho `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c1e4e-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="c1e4e-165">Při vyhodnocování vazače modelů, je zkontrolován kolekce zprostředkovatelů v pořadí.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="c1e4e-166">První poskytovatele, který vrací vazač používá.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="c1e4e-167">Následující obrázek znázorňuje výchozí vazače modelů z ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="c1e4e-168">![Výchozí vazače modelů](custom-model-binding/images/default-model-binders.png "výchozí vazače modelů")</span><span class="sxs-lookup"><span data-stu-id="c1e4e-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="c1e4e-169">Přidání poskytovatele na konec kolekce může mít za následek vazač modelu předdefinované volána před vaše vlastní vazač má možnost.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="c1e4e-170">V tomto příkladu, poběží vlastní zprostředkovatel se přidá na začátek kolekce zajistit, že se používá pro `Author` argumentů akce.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-170">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="c1e4e-171">Doporučení a osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="c1e4e-171">Recommendations and best practices</span></span>

<span data-ttu-id="c1e4e-172">Vazače modelů vlastní:</span><span class="sxs-lookup"><span data-stu-id="c1e4e-172">Custom model binders:</span></span>
- <span data-ttu-id="c1e4e-173">Neměli nastavit stavové kódy, nebo může vracet výsledky (například 404 není nalezena).</span><span class="sxs-lookup"><span data-stu-id="c1e4e-173">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="c1e4e-174">V případě selhání vazby modelu [filtr akce](xref:mvc/controllers/filters) nebo logiky v rámci samotné metoda akce by měla řídit tyto chyby.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="c1e4e-175">Jsou velmi užitečné k odstranění opakovaných kódu a mezi vyjímání obavy z metody akce.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="c1e4e-176">Obvykle nepoužívejte převést řetězec do vlastního typu, [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) je obvykle lepší volbou.</span><span class="sxs-lookup"><span data-stu-id="c1e4e-176">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
