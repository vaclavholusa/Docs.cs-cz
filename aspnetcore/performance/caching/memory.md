---
title: "Ukládání do mezipaměti v paměti v ASP.NET Core"
author: rick-anderson
description: "Zjistěte, jak data v paměti jádra ASP.NET do mezipaměti."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/memory
ms.openlocfilehash: 4219cae4e3d3f9d15afe6725b21cc8966979d95c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="in-memory-caching-in-aspnet-core"></a>Ukládání do mezipaměti v paměti v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Jan Luo](https://github.com/JunTaoLuo), a [Steve Smith](https://ardalis.com/)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>Základní informace o ukládání do mezipaměti

Ukládání do mezipaměti může výrazně zlepšit výkon a škálovatelnost aplikace, protože se sníží pracovní vyžadovaných ke generování obsahu. Ukládání do mezipaměti funguje nejlépe s daty, která se zřídka mění. Ukládání do mezipaměti zhotoví kopii dat, která lze vrátit mnohem rychlejší než z původního zdroje. Měli byste zápisu a testování aplikace nikdy závislý na data uložená v mezipaměti.

Jádro ASP.NET podporuje několik různé mezipaměti. Nejjednodušší cache je založená na [IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache), která představuje mezipaměť uložené v paměti webového serveru. Aplikace, které se spouštějí na serverové farmě několika serverů by měla zajistěte, aby byly relací trvalé při použití mezipaměti v paměti. Trvalé relace Ujistěte se, že následné žádosti z klienta všechny přejít na stejném serveru. Například použití webů Azure aplikace [směrování žádostí na aplikace](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) směrovat všechny následné požadavky na stejném serveru.

Vyžadovat non trvalé relací ve webové farmě [distribuované mezipaměti](distributed.md) se chcete vyhnout potížím konzistence mezipaměti. Pro některé aplikace může podporovat distribuované mezipaměti vyšší škálování než mezipaměti v paměti. Pomocí distribuované mezipaměti snižování zátěže mezipaměti paměti externího procesu. 

`IMemoryCache` Mezipaměti bude vyřazení položky mezipaměti přetížena paměť, pokud [mezipaměti s prioritou](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority) je nastaven na `CacheItemPriority.NeverRemove`. Můžete nastavit `CacheItemPriority` upravit prioritu mezipaměti vyloučí položky paměť přetížena.

Mezipaměť v paměti můžete ukládat jakýkoli objekt; rozhraní distribuované mezipaměti je omezený na `byte[]`.

## <a name="using-imemorycache"></a>Pomocí IMemoryCache

Ukládání do mezipaměti v paměti je *služby* , je na něj odkazovat z vaší aplikace pomocí [vkládání závislostí](../../fundamentals/dependency-injection.md). Volání `AddMemoryCache` v `ConfigureServices`:

[!code-csharp[Main](memory/sample/WebCache/Startup.cs?highlight=8)] 

Požadavku `IMemoryCache` instance v konstruktoru:

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-)] 

`IMemoryCache`vyžaduje balíček NuGet "Microsoft.Extensions.Caching.Memory".

Následující kód používá [TryGetValue](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) ke kontrole, pokud je aktuální čas v mezipaměti. Pokud položka není v mezipaměti, nový záznam je vytvořen a přidán do mezipaměti s [nastavit](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_).

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Zobrazí se aktuální čas a čas v mezipaměti:

[!code-html[Main](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Uložená v mezipaměti `DateTime` hodnota zůstane v mezipaměti, přestože jsou požadavky v rámci časového limitu (a žádné vyřazení z důvodu přetížení paměti). Následující obrázek ukazuje aktuální čas a starší čas načten z mezipaměti:

![Zobrazení index se zobrazí dva různé časy.](memory/_static/time.png)

Následující kód používá [GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) a [GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) data do mezipaměti. 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Následující kód volání [získat](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) se načíst uložené v mezipaměti Doba:

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

V tématu [IMemoryCache metody](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache) a [CacheExtensions metody](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions) popis metod mezipaměti.

## <a name="using-memorycacheentryoptions"></a>Pomocí MemoryCacheEntryOptions

Následující ukázka:

- Nastaví dobu absolutní vypršení platnosti. Toto je maximální dobu, kterou můžete uložit do mezipaměti na položku a zabrání vzniku příliš zastaralá při nepřetržitě prodloužení klouzavé vypršení platnosti položky.
- Nastaví klouzavou dobu vypršení platnosti. Hodiny klouzavé vypršení platnosti se obnoví požadavků, které přístup k této položky v mezipaměti.
- Nastaví prioritu mezipaměti `CacheItemPriority.NeverRemove`. 
- Nastaví [PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) , bude volána po vyřazování položku z mezipaměti. Zpětné volání je spuštěn v jiném podprocesu z kód, který odebere položku z mezipaměti.

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a>Závislosti mezipaměti

Následující příklad ukazuje, jak vypršení platnosti položky mezipaměti, pokud vyprší závislé položky. A `CancellationChangeToken` se přidá do položky v mezipaměti. Když `Cancel` se volá na `CancellationTokenSource`, jsou vyřazování obě položky mezipaměti. 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Použití `CancellationTokenSource` umožňuje více záznamů mezipaměti určených k vyloučení jako skupina. S `using` vzor ve výše uvedeném kódu položky mezipaměti vytvořit uvnitř `using` bloku zdědí nastavení vypršení platnosti a aktivační události.

## <a name="additional-notes"></a>Další poznámky

- Při použití zpětné volání znovu naplnit položku mezipaměti:

  - Více požadavků můžete najít hodnota uložená v mezipaměti klíče prázdný protože zpětné volání nebylo dokončeno. 
  - Výsledkem může být několik vláken opětovného vyplnění položka v mezipaměti.

- Když jedna položka v mezipaměti se používá k vytvoření druhého, podřízená zkopíruje vypršení platnosti tokenů a nastavení na základě času vypršení platnosti nadřazené položce. Podřízená není ručního odebrání vypršela platnost, nebo aktualizace nadřazené položky.

## <a name="additional-resources"></a>Další zdroje

* [Práce s distribuované mezipaměti](xref:performance/caching/distributed)
* [Detekovat změny s tokeny změn](xref:fundamentals/primitives/change-tokens)
* [Ukládání odpovědí do mezipaměti](xref:performance/caching/response)
* [Middleware pro ukládání odpovědí do mezipaměti](xref:performance/caching/middleware)
* [Uložení pomocné rutiny značky do mezipaměti](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Pomocná rutina značek v distribuované mezipaměti](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
