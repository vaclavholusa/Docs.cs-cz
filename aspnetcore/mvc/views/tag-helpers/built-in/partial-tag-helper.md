---
title: "Pomocník částečné značky"
author: scottaddie
description: "Zjistit pomocná částečné značky ASP.NET Core a roli každý z jeho atributy hrát v vykreslení částečného zobrazení."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: e5c71ccb7a355ffe1c24f389ab490d614d333589
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="partial-tag-helper"></a>Pomocník částečné značky

Podle [Scott Addie](https://github.com/scottaddie)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="overview"></a>Přehled

Částečné pomocná značky se používá pro vykreslování [částečné zobrazení](xref:mvc/views/partial) v aplikacích pro stránky Razor a MVC. Zvažte to:

* Vyžaduje ASP.NET Core 2.1 nebo vyšší.
* Představuje alternativu k [pomocné rutiny HTML syntaxe](xref:mvc/views/partial#referencing-a-partial-view).

Chcete-li recap, možnosti pomocné rutiny HTML pro vykreslení částečného zobrazení patří:

* [@await Html.PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [@await Html.RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

*Produktu* model se používá v ukázky v tomto dokumentu:

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

Následuje inventář atributy částečné pomocná značky.

## <a name="name"></a>name

`name` Atribut je vyžadován. Označuje název nebo cesta částečné zobrazení k vykreslení. Pokud je zadaný název částečného zobrazení, [zobrazení zjišťování](xref:mvc/views/overview#view-discovery) proces je zahájen. Pokud je k dispozici explicitní cestu přeskočí tohoto procesu.

Následující kód používá explicitní cestu, která znamená, že *_ProductPartial.cshtml* je třeba načíst z *sdílené* složky. Pomocí [asp-pro](#asp-for) atribut model je předán částečné zobrazení pro vazbu.

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="asp-for"></a>ASP-pro

`asp-for` Atribut přiřadí [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) má být porovnán s aktuální model. A `ModelExpression` odvodí, že `@Model.` syntaxe. Například `asp-for="Product"` lze použít místo `asp-for="@Model.Product"`. Toto výchozí chování odvození je přepsat pomocí `@` symbol definovat výraz vložené.

Následující kód načte *_ProductPartial.cshtml*:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_AspFor)]

Částečné zobrazení je vázán k modelu přidružené stránky `Product` vlastnost:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="view-data"></a>view-data

`view-data` Atribut přiřadí [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) mají být předána do částečného zobrazení. Následující kód, zpřístupní se celou kolekci ViewData částečného zobrazení:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

V předchozí kód `IsNumberReadOnly` nastavena na hodnotu klíče `true` a přidat do kolekce ViewData. V důsledku toho `ViewData["IsNumberReadOnly"]` zpřístupněna v rámci následující částečné zobrazení:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

V tomto příkladu hodnota `ViewData["IsNumberReadOnly"]` Určuje, zda *číslo* pole se zobrazí jako jen pro čtení.

## <a name="additional-resources"></a>Další zdroje

* [Částečná zobrazení](xref:mvc/views/partial)
* [Slabě typované data (ViewData a ViewBag)](xref:mvc/views/overview#weakly-typed-data-viewdata-and-viewbag)
