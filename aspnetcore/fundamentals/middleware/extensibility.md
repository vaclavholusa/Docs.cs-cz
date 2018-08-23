---
title: Middleware založený na objekt pro vytváření technologie aktivace v ASP.NET Core
author: guardrex
description: Další informace o použití middlewaru silného typu pomocí implementace aktivaci založenou na objekt pro vytváření v ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2018
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 346f5e7b28a9cd17a03a864772ed8b2e4be9455b
ms.sourcegitcommit: 2c158fcfd325cad97ead608a816e525fe3dcf757
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/14/2018
ms.locfileid: "41753419"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>Middleware založený na objekt pro vytváření technologie aktivace v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) je bod rozšiřitelnosti pro [middleware](xref:fundamentals/middleware/index) aktivace.

`UseMiddleware` rozšiřující metody zkontrolovat, pokud middlewarem registrovaného typu implementuje `IMiddleware`. Pokud ano, `IMiddlewareFactory` instance zaregistrovaný v kontejneru se používá k překladu `IMiddleware` implementace namísto použití logiky aktivace založené na konvenci middlewaru. Middleware je registrována jako služba s vymezeným oborem nebo přechodné v kontejneru aplikace služby.

Výhody:

* Aktivace každý požadavek (vkládání vymezené služby)
* Silné typování middlewaru

`IMiddleware` Každý požadavek, je aktivovaná, takže vymezené služby můžete být vloženy do konstruktor middlewaru.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Ukázková aplikace předvádí middleware aktivoval:

* Konvence. Další informace o aktivaci konvenční middleware, najdete v článku [Middleware](xref:fundamentals/middleware/index) tématu.
* [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementace. Výchozí hodnota [MiddlewareFactory třídy](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) aktivuje middleware.

Implementace middlewaru fungovat stejně jako a si poznamenejte hodnotu poskytovanou infrastrukturou parametru řetězce dotazu (`key`). Middlewares objekt context vloženého databáze (vymezené služby) slouží k zaznamenání hodnotu řetězce dotazu v databázi v paměti.

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definuje middleware pro kanál žádosti o aplikace. [InvokeAsync (HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) metoda zpracovává požadavky a vrátí `Task` , který představuje spuštění middlewaru.

Middleware aktivoval konvence:

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Middleware aktivoval `MiddlewareFactory`:

[!code-csharp[](extensibility/sample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

Rozšíření jsou vytvořeny pro middlewares:

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Není možné předat objekty factory aktivuje middleware s `UseMiddleware`:

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

Middleware aktivovat objekt pro vytváření se přidá do kontejneru integrované v *Startup.cs*:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=12)]

Obě middlewares zaregistrovaní v kanálu zpracování žádostí v `Configure`:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=14-15)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) poskytuje metody pro vytvoření middlewaru. Implementace objektu factory middlewaru je zaregistrovaný v kontejneru jako služba s vymezeným oborem.

Výchozí hodnota `IMiddlewareFactory` implementaci [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), najdete v [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) balíčku.

## <a name="additional-resources"></a>Další zdroje

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
