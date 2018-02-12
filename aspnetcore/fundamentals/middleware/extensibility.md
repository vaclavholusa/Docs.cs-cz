---
title: "Aktivace na základě Factory middlewaru v ASP.NET Core"
author: guardrex
description: "Další informace o použití silného typu middlewaru s implementací aktivaci založenou na objektu pro vytváření v ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/29/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 5e7e807a0107f1f500d43b5ffd4362b11f53118b
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2018
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>Aktivace na základě Factory middlewaru v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) je bod rozšiřitelnosti pro [middleware](xref:fundamentals/middleware/index) aktivace.

`UseMiddleware`rozšiřující metody zkontrolovat, pokud se implementuje middleware registrovaného typu `IMiddleware`. Pokud ano, `IMiddlewareFactory` instance zaregistrován v kontejneru se používá k překladu `IMiddleware` implementace místo použití logiku aktivace založené na konvenci middleware. Middleware je zaregistrován jako služba vymezenou nebo přechodný v kontejneru služby aplikace.

Výhody:

* Aktivace na základě požadavku (vkládání vymezená služeb)
* Silného typování middlewaru

`IMiddleware`Každý požadavek, je aktivovaná, takže vymezené služby může vložit do konstruktor middlewaru.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Ukázková aplikace ukazuje middleware aktivované tímto:

* Konvence (`ConventionalMiddleware`). Další informace o aktivaci konvenční middleware, najdete v článku [Middleware](xref:fundamentals/middleware/index) tématu.
* [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) implementace (`IMiddlewareMiddleware`). Výchozí hodnota [MiddlewareFactory třída](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) aktivuje middleware.

Implementace middlewaru fungovat stejně jako a zaznamenejte hodnotu poskytovanou infrastrukturou parametr řetězce dotazu (`key`). Middlewares použijte kontextu vloženého databáze (vymezená service) pro hodnotu řetězce dotazu v databázi v paměti.

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definuje middleware pro aplikace požadavku kanálu. [InvokeAsync (položka HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) metoda zpracovává požadavky a vrátí `Task` představující provádění middleware.

Middleware aktivovat pomocí konvence:

[!code-csharp[Main](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Middleware aktivován `MiddlewareFactory`:

[!code-csharp[Main](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

Rozšíření jsou vytvořeny pro middlewares:

[!code-csharp[Main](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Není možné předat objekty factory aktivuje middlewaru s `UseMiddleware`:

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

Middleware factory aktivuje je přidat do kontejneru integrované v *Startup.cs*:

[!code-csharp[Main](extensibility/sample/Startup.cs?name=snippet1&highlight=6)]

Obě middlewares jsou zaregistrovány v kanálu zpracování požadavku v `Configure`:

[!code-csharp[Main](extensibility/sample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) poskytuje metody pro vytvoření middleware. Implementace objektu factory middlewaru je zaregistrován v kontejneru jako vymezené služby.

Výchozí hodnota `IMiddlewareFactory` implementace [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), se nachází v [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) balíčku ([odkaz na zdroj](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).

## <a name="additional-resources"></a>Další zdroje

* [Middleware](xref:fundamentals/middleware/index)
* [Aktivace na základě Factory middlewaru s kontejner třetích stran](xref:fundamentals/middleware/extensibility-third-party-container)
