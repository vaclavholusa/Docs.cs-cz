---
title: "Vkládání závislostí do zobrazení"
author: ardalis
description: 
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 80fb9e43-e4db-4af2-b2a8-e1364a712f69
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: 4586f50bc663b7269914dfff28b61342e3991a48
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="dependency-injection-into-views"></a>Vkládání závislostí do zobrazení

Podle [Steve Smith](https://ardalis.com/)

Jádro ASP.NET podporuje [vkládání závislostí](xref:fundamentals/dependency-injection) do zobrazení. To může být užitečné pro zobrazení konkrétní služby, jako je lokalizace nebo data, vyžaduje se jenom pro naplnění zobrazení. Snažte se zachovat [oddělené oblasti zájmu](http://deviq.com/separation-of-concerns/) mezi kontrolery a zobrazení. Většinu dat, která se zobrazí zobrazení by měl být předána z řadiče.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="a-simple-example"></a>Jednoduchý příklad

Službu můžete vložit do zobrazení pomocí `@inject` – direktiva. Si můžete představit `@inject` jako přidání vlastnosti do zobrazení a naplnění danou vlastnost pomocí DI.

Syntaxe `@inject`:`@inject <type> <name>`

Příklad `@inject` v akci:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Toto zobrazení ukazuje seznam `ToDoItem` instance, společně s souhrn zobrazující celkové statistiky. Souhrn se naplní ze vloženého `StatisticsService`. Tato služba je zaregistrovaný pro vkládání závislostí v `ConfigureServices` v *Startup.cs*:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

`StatisticsService` Provede pár výpočtů na sadu `ToDoItem` instance, které se přistupuje prostřednictvím úložiště:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

Ukázka úložiště používá kolekci v paměti. Implementace výše uvedeném (který funguje na všechna data v paměti) se nedoporučuje pro velké, vzdáleně používaná datové sady.

Ukázka zobrazuje data z model vázán k zobrazení a službu vloženy do zobrazení:

![Chcete-li zobrazit výpis celkový počet položek, Dokončit položky, průměrná priority a seznam úloh s jejich úroveň priority a logické hodnoty, která určuje dokončení.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Naplnění vyhledávání dat

Vkládání zobrazení může být užitečné k naplnění možnosti v prvky uživatelského rozhraní, jako je například rozevíracích seznamů. Vezměte v úvahu formuláře profilu uživatele, který zahrnuje možnosti pro zadání pohlaví, stav a další předvolby. Vykreslování formuláře, pomocí standardní přístup MVC by vyžadovaly kontroleru k žádosti o služby přístup dat pro každou z těchto sad možností a poté načíst model nebo `ViewBag` s každou sadu možností svázat.

Alternativní způsob vloží přímo do zobrazení získat možnosti služby. To minimalizuje množství vyžaduje adaptérem přesun tato zobrazení element konstrukce logiku do samotného zobrazení kód. Akce kontroleru zobrazit formulář Úpravy profilu stačí předat formuláře instance profil:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

Formuláře HTML, který používá k aktualizaci těchto předvolby zahrnuje rozevíracích seznamů pro tři vlastnosti:

![Aktualizujte profil zobrazení s formuláři povolení položka názvu, pohlaví, stavu a Oblíbené barev.](dependency-injection/_static/updateprofile.png)

Tyto seznamy jsou naplněny službu, která je prováděno do zobrazení:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

`ProfileOptionsService` Je úrovni uživatelského rozhraní služba navržená k poskytování právě data potřebná pro tento formát:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> Nezapomeňte si zaregistrovat typy bude požadovat pomocí vkládání závislostí v `ConfigureServices` metoda v *Startup.cs*.

## <a name="overriding-services"></a>Přepsání služby

Kromě vložení nové služby, můžete tento postup použít také k přepsání dříve vloženého services na stránce. Následující obrázek znázorňuje všechna pole, které jsou k dispozici na stránce použít v prvním příkladu:

![IntelliSense kontextové nabídky na zadaný seznam Html, součást, StatsService a adresu Url pole symbol @](dependency-injection/_static/razor-fields.png)

Jak vidíte, pole výchozí obsahovat `Html`, `Component`, a `Url` (a taky `StatsService` , jsme vložit). Pokud například chcete nahradit vlastní pomocné rutiny HTML výchozí, by mohl snadno provedete použitím `@inject`:

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Pokud chcete rozšířit stávající služby, můžete použít tento postup jednoduše při dědění z nebo zabalení stávající implementaci vlastními.

## <a name="see-also"></a>Viz také

* Simon Timms Blog: [získávání vyhledávání dat do zobrazení](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
