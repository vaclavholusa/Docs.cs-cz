---
title: Filtr metody pro stránky Razor v ASP.NET Core
author: Rick-Anderson
description: Naučte se vytvořit filtr metody pro stránky Razor v ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/5/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/filter
ms.openlocfilehash: b04253b9240cb88c4f0d3824a4b9fda947d6da08
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/19/2018
ms.locfileid: "31585189"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a>Filtr metody pro stránky Razor v ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Filtry stránky Razor [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) a [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) povolit stránky Razor spustit kód před a po spuštění obslužnou rutinu stránky Razor. Filtry Razor stránce jsou podobná [filtrů Akce ASP.NET Core MVC](xref:mvc/controllers/filters#action-filters), s výjimkou je nelze použít s obslužnými rutinami jednotlivých stránek. 

Filtry Razor stránky:

* Spuštění kódu po nebyla vybrána metoda obslužná rutina, ale předtím, než dojde k vazby modelu.
* Kód spusťte před spuštěním metody obslužné rutiny, po dokončení vazby modelu.
* Po provedení metody obslužná rutina spusťte kód.
* Může být implementováno na stránce nebo globálně.
* Nelze použít s obslužnými rutinami konkrétní stránky.

Kód můžete spustit, než obslužná rutina metoda provádí pomocí konstruktoru stránky nebo middleware, ale pouze filtry stránky Razor mít přístup k [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext). Mít filtry [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) odvozené parametr, který poskytuje přístup k `HttpContext`. Například [implementovat atribut filtru](#ifa) ukázka přidá hlavičku odpovědi, něco, co nelze provést pomocí konstruktorů nebo middleware.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/live/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Stránka filtry Razor poskytují následující metody, které lze použít globálně nebo na úrovni stránky:

* Synchronních metod:

    * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : volá se po nebyla vybrána metoda obslužná rutina, ale model, než dojde k vazby.
    * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : voláno před provedením metody obslužné rutiny, po dokončení vazby modelu.
    * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : voláno po provedení metody obslužné rutiny, před výsledku akce.

* Asynchronní metody:

    * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : volána asynchronně po nebyla vybrána metoda obslužná rutina, ale model, než dojde k vazby.
    * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : volaná asynchronně před vyvoláním metody obslužné rutiny, po dokončení vazby modelu.

> [!NOTE]
> Implementace **buď** synchronní nebo asynchronní verzi rozhraní filtru, nikoli oba dva. Rozhraní framework zkontroluje nejprve Pokud filtr implementuje rozhraní asynchronní, a pokud ano, který volá. Pokud tomu tak není, volá metodu nebo metody rozhraní synchronní. Pokud jsou obě rozhraní implementované, se nazývají asynchronní metody. Stejné pravidlo platí pro přepsání v stránky, implementovat synchronní nebo asynchronní verzi přepsání, nikoli oba dva.

## <a name="implement-razor-page-filters-globally"></a>Globálně implementace filtrů stránky Razor

Následující kód implementuje `IAsyncPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

V předchozí kód [objektu ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) se nevyžaduje. V ukázce slouží k zadání informací o trasování pro aplikace.

Následující kód umožňuje `SampleAsyncPageFilter` v `Startup` třídy:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

Následující kód ukazuje kompletní `Startup` třídy:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

Následující kód volání `AddFolderApplicationModelConvention` pro použití `SampleAsyncPageFilter` na pouze stránky v */subFolder*:

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

Následující kód implementuje synchronní `IPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

Následující kód umožňuje `SamplePageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"
## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Implementace filtru stránek Razor přepsáním metody filtru

Následující kód přepíše synchronní filtry Razor stránky:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

::: moniker-end

<a name="ifa"></a>
## <a name="implement-a-filter-attribute"></a>Implementace atribut filtru

Integrovaný filtr na základě atributů [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) může být rozčlenění filtru. Filtr přidá do odpovědi hlavičku:

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

Následující kód se vztahuje `AddHeader` atribut:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

V tématu [přepsání výchozí pořadí](xref:mvc/controllers/filters#overriding-the-default-order) pokyny k přepsání pořadí.

V tématu [zrušení a krátký circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) pokyny týkající se krátká smyčka kanálu filtru z filtru. 

<a name="auth"></a>
## <a name="authorize-filter-attribute"></a>Autorizovat atribut filtru

[Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) atribut lze použít k `PageModel`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]
