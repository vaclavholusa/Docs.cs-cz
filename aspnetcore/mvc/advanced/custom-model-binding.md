---
title: "Vlastní Model vazby v ASP.NET Core"
author: ardalis
description: "Zjistěte, jak vazby modelu umožňuje akce kontroleru pracovat přímo s typy modelu v ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 04/10/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 941aa9e3ff4e4a75714e11b79d913418d0514d1e
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="custom-model-binding-in-aspnet-core"></a>Vlastní Model vazby v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/)

Vazby modelu umožňuje akce kontroleru pracovat přímo s modelu typy (předané jako argumenty metoda), spíš než požadavky HTTP. Mapování mezi modely dat a aplikací příchozí požadavek se zpracovává souborem vazače modelů. Vazba funkce integrované modelu mohou vývojáři rozšířit implementací vazače modelů vlastní (i když obvykle nemusíte napsat vlastního zprostředkovatele).

[Zobrazení nebo stažení ukázky z webu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Výchozí omezení vazač modelu

Vazače modelů výchozí podporují většinu běžné typy dat .NET Core a by měl splňovat požadavky Většina vývojářů. Očekávané při vytváření vazby textový vstup z požadavku přímo do modelu typy. Možná budete muset transformace vstupu před jeho vazby. Pokud například máte klíč, který slouží k vyhledávání dat modelu. Můžete použít vlastní vazač modelu pro načtení dat. na základě klíče.

## <a name="model-binding-review"></a>Zkontrolujte vazby modelu

Vazby modelu používá pro typy, které funguje na určité definice. A *jednoduchý typ* je převést z jednoho řetězce ve vstupu. A *komplexní typ* je převést z více vstupních hodnot. Rozhraní framework určuje rozdíl podle existenci `TypeConverter`. Doporučujeme vytvořit převaděč typu, pokud máte jednoduchou `string`  ->  `SomeType` mapování, která nevyžaduje externím prostředkům.

Před vytvořením vlastní vlastní vazač modelu, je vhodné prostudovali jak existující model jsou implementované vazače. Vezměte v úvahu [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) kterých jde převést kódováním base64 řetězce na pole bajtů. Bajtová pole jsou často uložené jako soubory nebo pole objektů BLOB databáze.

### <a name="working-with-the-bytearraymodelbinder"></a>Práce ByteArrayModelBinder

Řetězce s kódováním base64 lze použít k reprezentování binární data. Například na následujícím obrázku může být kódovaný jako řetězec.

![DotNet robota](custom-model-binding/images/bot.png "robota dotnet.")

Malá část řetězec s kódováním je znázorněno na následujícím obrázku:

![DotNet robota kódovaný](custom-model-binding/images/encoded-bot.png "kódovaný robota dotnet.")

Postupujte podle pokynů [tohoto příkladu README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) převést řetězec s kódováním base64 do souboru.

Můžete využít řetězce kódováním base64 a používat rozhraní ASP.NET MVC jádra `ByteArrayModelBinder` ji převést do bajtového pole. [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) které implementuje [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) mapuje `byte[]` argumenty, které mají `ByteArrayModelBinder`:

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

Při vytváření vlastní vlastní vazač modelu, můžete implementovat vlastní `IModelBinderProvider` zadejte, nebo použijte [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).

Následující příklad ukazuje, jak používat `ByteArrayModelBinder` převést řetězec s kódováním base64 tak, aby `byte[]` a výsledek uložit do souboru:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

Můžete odeslat řetězec s kódováním base64, pomocí této metody rozhraní api pomocí nástroje, například [Postman](https://www.getpostman.com/):

![postman](custom-model-binding/images/postman.png "postman")

Tak dlouho, dokud vazač svázat data požadavku na příslušně pojmenovaných vlastnosti nebo argumenty, bude úspěšné vazby modelu. Následující příklad ukazuje, jak používat `ByteArrayModelBinder` s modelem zobrazení:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Ukázky vazač vlastní modelu

V této části jsme budete implementovat vlastní vazač modelu který:

- Převede příchozí data požadavku na silného typu klíče argumenty.
- Používá Entity Framework Core se načíst související entity.
- Předá přidružené entity jako argument pro metodu akce.

Následující ukázkové používá `ModelBinder` atributu u `Author` modelu:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

V předchozí kód `ModelBinder` atribut určuje typ `IModelBinder` který se používá k vytvoření vazby `Author` parametrů akcí. 

`AuthorEntityBinder` Se používá k vytvoření vazby `Author` parametr podle načtení entity ze zdroje dat pomocí Entity Framework Core a `authorId`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

Následující kód ukazuje způsob použití `AuthorEntityBinder` v metodu akce:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

`ModelBinder` Atributu lze použít `AuthorEntityBinder` parametry, které nepoužívají výchozí konvence:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

V tomto příkladu, protože název argument není výchozí `authorId`, je zadán parametr používání `ModelBinder` atribut. Všimněte si, že kontroleru a akce metoda jednodušší ve srovnání s vyhledávání entity v metodě akce. Logika pro načtení Autor pomocí Entity Framework Core je přesunuta do vazače modelu. To může být výrazně zjednodušení, pokud máte několik metod, které vytvořit vazbu na modelu vytvořit a pomáhají vám postupovat podle [SUCHÝCH Princip](http://deviq.com/don-t-repeat-yourself/).

Můžete použít `ModelBinder` atribut vlastnosti jednotlivých modelu (například na viewmodel) nebo pro parametry metody akce k určení vazač modelu nebo název modelu pro právě tento typ nebo akci.

### <a name="implementing-a-modelbinderprovider"></a>Implementace ModelBinderProvider

Místo použití atributu, můžete implementovat `IModelBinderProvider`. Toto je, jak jsou implementované vestavěnou architekturou vazače. Když zadáte typ vaší vazač funguje na, zadejte typ argumentu vytváří, **není** přijímá vaší vazač vstupu. Následující zprostředkovatele vazače pracuje `AuthorEntityBinder`. Při jejím přidání do kolekce MVC poskytovatelů, nemusíte používat `ModelBinder` atributu u `Author` nebo `Author` zadali parametry.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Poznámka: Předchozí kód vrátí `BinderTypeModelBinder`. `BinderTypeModelBinder` funguje jako objekt pro vytváření pro vazače modelů a poskytuje vkládání závislostí (DI). `AuthorEntityBinder` Vyžaduje DI pro přístup k EF jádra. Použití `BinderTypeModelBinder` Pokud vaše vazač modelu vyžaduje služby od DI.

Pokud chcete používat zprostředkovatele vazače modelu vlastní, přidejte ho `ConfigureServices`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

Při vyhodnocování vazače modelů, je zkontrolován kolekce zprostředkovatelů v pořadí. První poskytovatele, který vrací vazač používá.

Následující obrázek znázorňuje výchozí vazače modelů z ladicího programu.

![Výchozí vazače modelů](custom-model-binding/images/default-model-binders.png "výchozí vazače modelů")

Přidání poskytovatele na konec kolekce může mít za následek vazač modelu předdefinované volána před vaše vlastní vazač má možnost. V tomto příkladu, poběží vlastní zprostředkovatel se přidá na začátek kolekce zajistit, že se používá pro `Author` argumentů akce.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>Doporučení a osvědčené postupy

Vazače modelů vlastní:
- Neměli nastavit stavové kódy, nebo může vracet výsledky (například 404 není nalezena). V případě selhání vazby modelu [filtr akce](xref:mvc/controllers/filters) nebo logiky v rámci samotné metoda akce by měla řídit tyto chyby.
- Jsou velmi užitečné k odstranění opakovaných kódu a mezi vyjímání obavy z metody akce.
- Obvykle nepoužívejte převést řetězec do vlastního typu, [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) je obvykle lepší volbou.
