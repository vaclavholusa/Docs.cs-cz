---
title: Vlastní vazba modelu v ASP.NET Core
author: ardalis
description: Zjistěte, jak vazby modelu umožňuje akce kontroleru pracovat přímo s typy modelů v ASP.NET Core.
ms.author: riande
ms.date: 04/10/2017
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: dc901aea3c20e7f2e955f39d923216de70ef015b
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090404"
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="8865b-103">Vlastní vazba modelu v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8865b-103">Custom Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="8865b-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="8865b-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="8865b-105">Vazby modelu umožňuje akce kontroleru pracovat přímo s typy modelů (předané jako argumenty metody), spíše než požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="8865b-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="8865b-106">Mapování mezi příchozí žádosti o datové a aplikační modely zařizuje služba vazače modelů.</span><span class="sxs-lookup"><span data-stu-id="8865b-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="8865b-107">Vývojáři mohou rozšířit funkci vazby modelu předdefinovaných implementací vazače modelů vlastní (i když obvykle není nutné napsat vlastního zprostředkovatele).</span><span class="sxs-lookup"><span data-stu-id="8865b-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="8865b-108">Zobrazit nebo stáhnout ukázky z Githubu</span><span class="sxs-lookup"><span data-stu-id="8865b-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="8865b-109">Výchozí omezení vazače modelu</span><span class="sxs-lookup"><span data-stu-id="8865b-109">Default model binder limitations</span></span>

<span data-ttu-id="8865b-110">Vazače modelů výchozí podporu většiny běžných typů dat .NET Core a by měl splňovat potřeby Většina vývojářů.</span><span class="sxs-lookup"><span data-stu-id="8865b-110">The default model binders support most of the common .NET Core data types and should meet most developers' needs.</span></span> <span data-ttu-id="8865b-111">Očekávají se vytvořit vazbu textový vstup z požadavku přímo s typy modelů.</span><span class="sxs-lookup"><span data-stu-id="8865b-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="8865b-112">Můžete potřebovat, který umožňuje transformovat vstupní před jeho vazbu.</span><span class="sxs-lookup"><span data-stu-id="8865b-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="8865b-113">Například pokud máte klíč, který slouží k vyhledání dat modelu.</span><span class="sxs-lookup"><span data-stu-id="8865b-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="8865b-114">Můžete použít vlastní vazač modelu pro načtení dat na základě klíče.</span><span class="sxs-lookup"><span data-stu-id="8865b-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="8865b-115">Kontrola vazby modelu</span><span class="sxs-lookup"><span data-stu-id="8865b-115">Model binding review</span></span>

<span data-ttu-id="8865b-116">Vazby modelu používá konkrétní definice pro typy, které se pracuje.</span><span class="sxs-lookup"><span data-stu-id="8865b-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="8865b-117">A *jednoduchý typ* převést z jednoho řetězce ve vstupu.</span><span class="sxs-lookup"><span data-stu-id="8865b-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="8865b-118">A *komplexní typ* převést z více vstupních hodnot.</span><span class="sxs-lookup"><span data-stu-id="8865b-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="8865b-119">Rozhraní určuje rozdíl založené na existenci `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="8865b-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="8865b-120">Doporučujeme vytvořit konvertor typu, pokud máte jednoduchou `string`  ->  `SomeType` mapování, které nevyžaduje externí prostředky.</span><span class="sxs-lookup"><span data-stu-id="8865b-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="8865b-121">Před vytvořením vlastní vlastní vazač modelu, je vhodné si předtím prostudovali jak existující model jsou implementovány vazače.</span><span class="sxs-lookup"><span data-stu-id="8865b-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="8865b-122">Vezměte v úvahu [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) kterých jde převést pole bajtů řetězce s kódováním base64.</span><span class="sxs-lookup"><span data-stu-id="8865b-122">Consider the [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="8865b-123">Bajtová pole jsou často uložené jako soubory nebo pole objektů BLOB v databázi.</span><span class="sxs-lookup"><span data-stu-id="8865b-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="8865b-124">Práce s ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="8865b-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="8865b-125">Řetězce s kódováním base64 slouží k reprezentaci binární data.</span><span class="sxs-lookup"><span data-stu-id="8865b-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="8865b-126">Na následujícím obrázku například může být zakódován jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="8865b-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="8865b-127">![DotNet bot](custom-model-binding/images/bot.png "dotnet bot")</span><span class="sxs-lookup"><span data-stu-id="8865b-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="8865b-128">Malou část řetězce s kódováním je vidět na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="8865b-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="8865b-129">![DotNet bot kódovaný](custom-model-binding/images/encoded-bot.png "dotnet bot kódování")</span><span class="sxs-lookup"><span data-stu-id="8865b-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="8865b-130">Postupujte podle pokynů [vzorku README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) pro převod řetězce s kódováním base64 do souboru.</span><span class="sxs-lookup"><span data-stu-id="8865b-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="8865b-131">Můžete provést řetězec s kódováním base64 a používat ASP.NET Core MVC `ByteArrayModelBinder` převést do bajtového pole.</span><span class="sxs-lookup"><span data-stu-id="8865b-131">ASP.NET Core MVC can take a base64-encoded string and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="8865b-132">[ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) která implementuje [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) mapuje `byte[]` argumenty `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="8865b-132">The [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="8865b-133">Při vytváření vlastní vlastní vazač modelu, můžete implementovat vlastní `IModelBinderProvider` typ nebo použít [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="8865b-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="8865b-134">Následující příklad ukazuje, jak používat `ByteArrayModelBinder` převést na řetězec s kódováním base64 `byte[]` a uložte výsledky do souboru:</span><span class="sxs-lookup"><span data-stu-id="8865b-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="8865b-135">Můžete vytvořit řetězec s kódováním base64 do této metody rozhraní api pomocí některého nástroje, například [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="8865b-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="8865b-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="8865b-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="8865b-137">Tak dlouho, dokud vazač svázat data požadavku na příslušně pojmenovaných vlastnosti nebo argumenty, proběhne úspěšně vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="8865b-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="8865b-138">Následující příklad ukazuje, jak používat `ByteArrayModelBinder` pomocí zobrazení modelu:</span><span class="sxs-lookup"><span data-stu-id="8865b-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="8865b-139">Ukázka vlastního modelu vazače</span><span class="sxs-lookup"><span data-stu-id="8865b-139">Custom model binder sample</span></span>

<span data-ttu-id="8865b-140">V této části jsme budete implementovat vlastní vazač modelu, který:</span><span class="sxs-lookup"><span data-stu-id="8865b-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="8865b-141">Převede příchozí žádosti o data silného typu klíče argumenty.</span><span class="sxs-lookup"><span data-stu-id="8865b-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="8865b-142">Používá Entity Framework Core se načíst související entity.</span><span class="sxs-lookup"><span data-stu-id="8865b-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="8865b-143">Související entita se předává jako argument do metody akce.</span><span class="sxs-lookup"><span data-stu-id="8865b-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="8865b-144">Následující ukázkový používá `ModelBinder` atribut na `Author` modelu:</span><span class="sxs-lookup"><span data-stu-id="8865b-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="8865b-145">V předchozím kódu `ModelBinder` atribut určuje typ `IModelBinder` , který má použít k vytvoření vazby `Author` parametry akce.</span><span class="sxs-lookup"><span data-stu-id="8865b-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="8865b-146">`AuthorEntityBinder` Pro vazbu se použije `Author` parametru načtení entity ze zdroje dat pomocí Entity Framework Core a `authorId`:</span><span class="sxs-lookup"><span data-stu-id="8865b-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="8865b-147">Následující kód ukazuje způsob použití `AuthorEntityBinder` v metodě akce:</span><span class="sxs-lookup"><span data-stu-id="8865b-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="8865b-148">`ModelBinder` Atribut je možné použít `AuthorEntityBinder` na parametry, které nepoužívají výchozí konvence:</span><span class="sxs-lookup"><span data-stu-id="8865b-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="8865b-149">V tomto příkladu, protože název argumentu není výchozí `authorId`, je zadán pomocí parametru `ModelBinder` atribut.</span><span class="sxs-lookup"><span data-stu-id="8865b-149">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using the `ModelBinder` attribute.</span></span> <span data-ttu-id="8865b-150">Všimněte si, že metoda kontroleru a akce jsou zjednodušené ve srovnání s vyhledávání entit v metodě akce.</span><span class="sxs-lookup"><span data-stu-id="8865b-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="8865b-151">Logika pro načtení Autor pomocí Entity Framework Core se přesune do vazače modelu.</span><span class="sxs-lookup"><span data-stu-id="8865b-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="8865b-152">To může být značné zjednodušení, pokud máte několik metod, které svázat `Author` model a můžete sledovat [zásada SUCHÝCH](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="8865b-152">This can be a considerable simplification when you have several methods that bind to the `Author` model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="8865b-153">Můžete použít `ModelBinder` atribut vlastnosti jednotlivých modelu (například na viewmodel) nebo na parametry metod akce k určení vazač modelu nebo název modelu pro právě tento typ nebo akce.</span><span class="sxs-lookup"><span data-stu-id="8865b-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="8865b-154">Implementace ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="8865b-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="8865b-155">Místo použití atributu, můžete implementovat `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="8865b-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="8865b-156">To je, jak se implementují vestavěnou architekturou vazače.</span><span class="sxs-lookup"><span data-stu-id="8865b-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="8865b-157">Když zadáte typ pracuje vaše vazač, zadejte typ argumentu vytvoří, **není** přijímá vstup vaše vazače.</span><span class="sxs-lookup"><span data-stu-id="8865b-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="8865b-158">Následující zprostředkovatele vazače spolupracuje `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="8865b-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="8865b-159">Když se přidá do kolekce MVC od poskytovatelů, není nutné použít `ModelBinder` atribut na `Author` nebo `Author` silné parametry.</span><span class="sxs-lookup"><span data-stu-id="8865b-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="8865b-160">Poznámka: Vrátí předchozí kód `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="8865b-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="8865b-161">`BinderTypeModelBinder` funguje jako objekt pro vytváření pro vazače modelů a poskytuje injektáž závislostí (DI).</span><span class="sxs-lookup"><span data-stu-id="8865b-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="8865b-162">`AuthorEntityBinder` Vyžaduje DI pro přístup k EF Core.</span><span class="sxs-lookup"><span data-stu-id="8865b-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="8865b-163">Použití `BinderTypeModelBinder` Pokud vyžaduje služby DI vaše vazače modelu.</span><span class="sxs-lookup"><span data-stu-id="8865b-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="8865b-164">Použití zprostředkovatele vazače modelu vlastní, přidejte ji kliknutím na `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8865b-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="8865b-165">Při vyhodnocování vazače modelů, je kolekce zprostředkovatelů zkoumány podle pořadí.</span><span class="sxs-lookup"><span data-stu-id="8865b-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="8865b-166">Prvním poskytovatelem, který vrací vazač se používá.</span><span class="sxs-lookup"><span data-stu-id="8865b-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="8865b-167">Následující obrázek ukazuje výchozí vazače modelů z ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="8865b-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="8865b-168">![Výchozí vazače modelů](custom-model-binding/images/default-model-binders.png "výchozí vazače modelů")</span><span class="sxs-lookup"><span data-stu-id="8865b-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="8865b-169">Vazač modelu integrované volána před vlastní vázací objekt má možnost můžou výsledkem přidání poskytovatele na konec kolekce.</span><span class="sxs-lookup"><span data-stu-id="8865b-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="8865b-170">V tomto příkladu vlastního zprostředkovatele se přidá na začátek kolekce, kterou chcete zajistit, že se používá pro `Author` argumentů.</span><span class="sxs-lookup"><span data-stu-id="8865b-170">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="8865b-171">Doporučení a osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="8865b-171">Recommendations and best practices</span></span>

<span data-ttu-id="8865b-172">Vazače modelů vlastní:</span><span class="sxs-lookup"><span data-stu-id="8865b-172">Custom model binders:</span></span>
- <span data-ttu-id="8865b-173">By se neměly pokoušet stavové kódy nebo vrátit výsledky (například 404 Nenalezeno).</span><span class="sxs-lookup"><span data-stu-id="8865b-173">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="8865b-174">Pokud vazba modelu selže, [filtr akce](xref:mvc/controllers/filters) nebo by měla tuto chybu řešit logiky v rámci samotného metody akce.</span><span class="sxs-lookup"><span data-stu-id="8865b-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="8865b-175">Jsou zvláště užitečná pro odstranění opakování kódu a převeďte společné aspekty z metody akce.</span><span class="sxs-lookup"><span data-stu-id="8865b-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="8865b-176">Obvykle neměl by se převod řetězce na vlastní typ, [ `TypeConverter` ](/dotnet/api/system.componentmodel.typeconverter) je obvykle vhodnější.</span><span class="sxs-lookup"><span data-stu-id="8865b-176">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
