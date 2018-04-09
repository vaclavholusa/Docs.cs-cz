---
title: Vkládání závislostí do zobrazení v ASP.NET Core
author: ardalis
description: Zjistěte, jak ASP.NET Core podporuje vkládání závislostí do zobrazení MVC.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/dependency-injection
ms.openlocfilehash: cc34b9069ec062f08644c0026c1ccdcd00f667ac
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a>Vkládání závislostí do zobrazení v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/)

Jádro ASP.NET podporuje [vkládání závislostí](xref:fundamentals/dependency-injection) do zobrazení. To může být užitečné pro zobrazení konkrétní služby, jako je lokalizace nebo data, vyžaduje se jenom pro naplnění zobrazení. Snažte se zachovat [oddělené oblasti zájmu](http://deviq.com/separation-of-concerns/) mezi kontrolery a zobrazení. Většinu dat, která se zobrazí zobrazení by měl být předána z řadiče.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="a-simple-example"></a>Jednoduchý příklad

Službu můžete vložit do zobrazení pomocí `@inject` – direktiva. Si můžete představit `@inject` jako přidání vlastnosti do zobrazení a naplnění danou vlastnost pomocí DI.

Syntaxe `@inject`: `@inject <type> <name>`

Příklad `@inject` v akci:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Toto zobrazení ukazuje seznam `ToDoItem` instance, společně s souhrn zobrazující celkové statistiky. Souhrn se naplní ze vloženého `StatisticsService`. Tato služba je zaregistrovaný pro vkládání závislostí v `ConfigureServices` v *Startup.cs*:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

`StatisticsService` Provede pár výpočtů na sadu `ToDoItem` instance, které se přistupuje prostřednictvím úložiště:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

Ukázka úložiště používá kolekci v paměti. Implementace výše uvedeném (který funguje na všechna data v paměti) se nedoporučuje pro velké, vzdáleně používaná datové sady.

Ukázka zobrazuje data z model vázán k zobrazení a službu vloženy do zobrazení:

![Chcete-li zobrazit výpis celkový počet položek, Dokončit položky, průměrná priority a seznam úloh s jejich úroveň priority a logické hodnoty, která určuje dokončení.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Naplnění vyhledávání dat

Vkládání zobrazení může být užitečné k naplnění možnosti v prvky uživatelského rozhraní, jako je například rozevíracích seznamů. Vezměte v úvahu formuláře profilu uživatele, který zahrnuje možnosti pro zadání pohlaví, stav a další předvolby. Vykreslování formuláře, pomocí standardní přístup MVC by vyžadovaly kontroleru k žádosti o služby přístup dat pro každou z těchto sad možností a poté načíst model nebo `ViewBag` s každou sadu možností svázat.

Alternativní způsob vloží přímo do zobrazení získat možnosti služby. To minimalizuje množství vyžaduje adaptérem přesun tato zobrazení element konstrukce logiku do samotného zobrazení kód. Akce kontroleru zobrazit formulář Úpravy profilu stačí předat formuláře instance profil:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

Formuláře HTML, který používá k aktualizaci těchto předvolby zahrnuje rozevíracích seznamů pro tři vlastnosti:

![Aktualizujte profil zobrazení s formuláři povolení položka názvu, pohlaví, stavu a Oblíbené barev.](dependency-injection/_static/updateprofile.png)

Tyto seznamy jsou naplněny službu, která je prováděno do zobrazení:

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

`ProfileOptionsService` Je úrovni uživatelského rozhraní služba navržená k poskytování právě data potřebná pro tento formát:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> Nezapomeňte si zaregistrovat typy bude požadovat pomocí vkládání závislostí v `ConfigureServices` metoda v *Startup.cs*.

## <a name="overriding-services"></a>Přepsání služby

Kromě vložení nové služby, můžete tento postup použít také k přepsání dříve vloženého services na stránce. Následující obrázek znázorňuje všechna pole, které jsou k dispozici na stránce použít v prvním příkladu:

![IntelliSense kontextové nabídky na zadaný seznam Html, součást, StatsService a adresu Url pole symbol @](dependency-injection/_static/razor-fields.png)

Jak vidíte, pole výchozí obsahovat `Html`, `Component`, a `Url` (a taky `StatsService` , jsme vložit). Pokud například chcete nahradit vlastní pomocné rutiny HTML výchozí, by mohl snadno provedete použitím `@inject`:

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Pokud chcete rozšířit stávající služby, můžete použít tento postup jednoduše při dědění z nebo zabalení stávající implementaci vlastními.

## <a name="see-also"></a>Viz také

* Simon Timms Blog: [získávání vyhledávání dat do zobrazení](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
