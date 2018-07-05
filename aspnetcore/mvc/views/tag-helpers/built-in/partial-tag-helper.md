---
title: Pomocná rutina částečné značky v ASP.NET Core
author: scottaddie
description: Zjišťování ASP.NET Core částečné pomocné rutiny značky a role, každá z jeho atributy přehrávání ve vykreslení částečného zobrazení.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/13/2018
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: 0a8caf09d1764278da4a0566844b0efaf4eeb567
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433867"
---
# <a name="partial-tag-helper-in-aspnet-core"></a>Pomocná rutina částečné značky v ASP.NET Core

Podle [Scott Addie](https://github.com/scottaddie)

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="overview"></a>Přehled

Pomocná rutina částečné značky se používá pro vykreslování [částečné zobrazení](xref:mvc/views/partial) v aplikacích pro Razor Pages a MVC. Vezměte v úvahu, že:

* Vyžaduje ASP.NET Core 2.1 nebo novější.
* Představuje alternativu k [pomocné rutiny HTML syntaxe](xref:mvc/views/partial#reference-a-partial-view).
* Vykreslí částečné zobrazení asynchronně.

Možnosti pomocné rutiny HTML pro vykreslení částečného zobrazení:

* [@await Html.PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [@await Html.RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

*Produktu* model se používá v ukázky v tomto dokumentu:

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

Následuje seznam atributů pomocná rutina částečné značky.

## <a name="name"></a>name

`name` Atribut je vyžadován. Označuje název nebo cesta k vykreslení částečného zobrazení. Když je zadaný název částečného zobrazení, [zobrazení zjišťování](xref:mvc/views/overview#view-discovery) zahájit proces. Tento proces je obejít, když je k dispozici explicitní cestu.

Následující kód používá cestu k explicitní, což indikuje, že *_ProductPartial.cshtml* má být načtena z *Shared* složky. Použití [pro](#for) atribut, modelu je předán do částečné zobrazení pro vazbu.

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a>pro

`for` Atribut přiřadí [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) má být porovnán s aktuální model. A `ModelExpression` odvodí, `@Model.` syntaxe. Například `for="Product"` lze použít místo `for="@Model.Product"`. Toto výchozí chování odvození přepsán pomocí `@` symbolů k definování vložený výraz. `for` Atributu nelze použít s [modelu](#model) atribut.

Následující kód načte *_ProductPartial.cshtml*:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

Částečné zobrazení je vázán na příslušné stránce modelu `Product` vlastnost:

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a>model

`model` Atribut přiřadí instance modelu má být předána do částečné zobrazení. `model` Atributu nelze použít s [pro](#for) atribut.

V následujícím kódu novou `Product` objektu je vytvořena instance a předat `model` atribut pro vazbu:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a>view-data

`view-data` Atribut přiřadí [objektu ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) k předání do částečné zobrazení. Následující kód zpřístupňuje celou kolekci slovníku ViewData částečného zobrazení:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

V předchozím kódu `IsNumberReadOnly` nastavena na hodnotu klíče `true` a přidat do kolekce slovníku ViewData. V důsledku toho `ViewData["IsNumberReadOnly"]` zpřístupněna v rámci následující částečné zobrazení:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

V tomto příkladu hodnota `ViewData["IsNumberReadOnly"]` Určuje, zda *číslo* zobrazí pole jako jen pro čtení.

## <a name="additional-resources"></a>Další zdroje

* [Částečná zobrazení](xref:mvc/views/partial)
* [Slabě typované data (ViewData, atribut ViewData a objekt ViewBag)](xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag)
