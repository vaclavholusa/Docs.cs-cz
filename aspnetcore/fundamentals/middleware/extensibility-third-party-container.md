---
title: Middleware aktivaci s použitím kontejner třetích stran v ASP.NET Core
author: guardrex
description: Další informace o použití silného typu middleware aktivace prostřednictvím objektu pro vytváření a kontejner třetích stran v ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/02/2018
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: 81d52aafd4e4d964aaec1c5fe61e585b023ff915
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279503"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a>Middleware aktivaci s použitím kontejner třetích stran v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

Tento článek ukazuje, jak používat [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) a [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) jako bod rozšiřitelnosti pro [middleware](xref:fundamentals/middleware/index) aktivaci s použitím kontejner třetích stran. Úvodní informace o `IMiddlewareFactory` a `IMiddleware`, najdete v článku [aktivace na základě Factory middleware](xref:fundamentals/middleware/extensibility) tématu.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Ukázková aplikace ukazuje middleware aktivace pomocí `IMiddlewareFactory` implementace `SimpleInjectorMiddlewareFactory`. Ukázce se používá [jednoduché Injector](https://simpleinjector.org) kontejneru pro vkládání (DI) závislosti.

Implementace tohoto příkladu middleware zaznamenává hodnotu poskytovanou infrastrukturou parametr řetězce dotazu (`key`). Middleware kontextu vloženého databáze (vymezená service) používá k zaznamenání hodnotu řetězce dotazu v databázi v paměti.

> [!NOTE]
> Použití ukázkové aplikace [jednoduché Injector](https://github.com/simpleinjector/SimpleInjector) výhradně pro demonstrační účely. Použití jednoduchého Injector není o potvrzení. Middleware aktivace přístupy popsaného v dokumentaci k jednoduché Injector a údržby programu z jednoduchého Injector jsou doporučení Githubu problémy. Další informace najdete v tématu [dokumentace jednoduché Injector](https://simpleinjector.readthedocs.io/en/latest/index.html) a [úložiště GitHub jednoduché Injector](https://github.com/simpleinjector/SimpleInjector).

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) poskytuje metody pro vytvoření middleware.

V ukázkové aplikace, se implementuje objekt pro vytváření middleware vytvořit `SimpleInjectorActivatedMiddleware` instance. Objekt factory middleware používá jednoduchý Injector kontejner přeložit middleware:

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definuje middleware pro aplikace požadavku kanálu.

Middleware aktivován `IMiddlewareFactory` implementace (*Middleware/SimpleInjectorActivatedMiddleware.cs*):

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

Vytvoření rozšíření pro middleware (*Middleware/MiddlewareExtensions.cs*):

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

`Startup.ConfigureServices` musíte provést několik úloh:

* Nastavte jednoduché Injector kontejneru.
* Zaregistrujte objekt pro vytváření a middleware.
* Zpřístupnit kontext databáze aplikace z kontejneru jednoduché Injector pro stránky Razor.

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

Middleware je zaregistrován v kanálu zpracování požadavku v `Startup.Configure`:

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=13)]

## <a name="additional-resources"></a>Další zdroje

* [Middleware](xref:fundamentals/middleware/index)
* [Aktivace na základě Factory middlewaru](xref:fundamentals/middleware/extensibility)
* [Jednoduché úložiště Injector GitHub](https://github.com/simpleinjector/SimpleInjector)
* [Jednoduché Injector dokumentace](https://simpleinjector.readthedocs.io/en/latest/index.html)
