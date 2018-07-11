---
title: Částečná zobrazení v ASP.NET Core
author: ardalis
description: Zjistěte, jak je částečné zobrazení zobrazení, který je vykreslen v rámci jiného zobrazení a pokud byste měli použít ve aplikace ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/06/2018
uid: mvc/views/partial
ms.openlocfilehash: 983f3caae34b21b46d8f556e70673cf3c97abbd3
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938456"
---
# <a name="partial-views-in-aspnet-core"></a>Částečná zobrazení v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), a [Scott Sauber](https://twitter.com/scottsauber)

ASP.NET Core MVC podporuje částečné zobrazení, které jsou užitečné ke sdílení opakovaně použitelných částí webových stránek v různých zobrazení.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>Co jsou částečné zobrazení

Částečné zobrazení je zobrazení, který je vykreslen v rámci jiného zobrazení. Do zobrazení volající (nebo nadřazené) vykreslením výstupu protokolu HTML, které jsou generovány spuštěním částečného zobrazení. Podobně jako zobrazení, částečná zobrazení použít *.cshtml* příponu souboru.

Například technologie ASP.NET Core 2.1 **webovou aplikaci** zahrnuje šablony projektu *_CookieConsentPartial.cshtml* částečné zobrazení. Načtení částečné zobrazení v rámci *_Layout.cshtml*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_Layout.cshtml?name=snippet_CookieConsentPartial)]

## <a name="when-to-use-partial-views"></a>Kdy použít částečné zobrazení

Částečná zobrazení jsou efektivní způsob rozdělení velké zobrazení na menší součásti. Mohou snížit duplikace zobrazení obsahu a umožňuje znovu použít zobrazení elementů. Společné prvky rozložení musí být zadán v [_Layout.cshtml](xref:mvc/views/layout). Bez rozložení opakovaně použitelný obsah, lze zapouzdřit do částečné zobrazení.

Na stránce komplexní skládá z několika logických částí je užitečné pro práci s jako vlastní částečné zobrazení jednotlivých prostředků. Izolovaně od zbytku stránky lze zobrazit jednotlivé stránky. Zobrazení samotné stránky bude jednodušší, protože obsahuje pouze celkovou strukturu stránky a volání k vykreslení částečného zobrazení.

> [!TIP]
> Postupujte podle [není opakujte sami Princip](https://deviq.com/don-t-repeat-yourself/) v zobrazení.

## <a name="declare-partial-views"></a>Deklarujte částečné zobrazení

Částečná zobrazení jsou vytvořeny jako regulární zobrazení&mdash;tak, že vytvoříte *.cshtml* soubor *zobrazení* složky. Neexistuje žádné sémantické rozdíly mezi částečné zobrazení a zobrazení regulární; však jejich se vykreslují odlišně. Máte zobrazení, které se vrátí přímo kontroleru [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult), a stejnou zobrazení lze použít jako částečné zobrazení. Hlavní rozdíl mezi vykreslení zobrazení a částečné zobrazení je, že částečná zobrazení nespouštět *soubor _ViewStart.cshtml*. Spuštění regulárního zobrazení *soubor _ViewStart.cshtml*. Další informace o *soubor _ViewStart.cshtml* v [rozložení](xref:mvc/views/layout)).

Jako konvence, názvy souborů částečné zobrazení často začínat `_`. Tyto zásady vytváření názvů není vyžadováno, ale pomáhá vizuálně odlišili od pravidelných zobrazení částečné zobrazení.

## <a name="reference-a-partial-view"></a>Odkaz na částečné zobrazení

V rámci zobrazení stránky existuje několik způsobů k vykreslení částečného zobrazení. Osvědčeným postupem je použití asynchronní vykreslování.

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a>Pomocná rutina částečné značky

Pomocná rutina částečné značky vyžaduje ASP.NET Core 2.1 nebo novější. Vykreslí asynchronně a používá HTML syntaxe:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialTagHelper)]

Další informace naleznete v tématu <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.

::: moniker-end

### <a name="asynchronous-html-helper"></a>Asynchronní pomocné rutiny HTML

Při použití pomocné rutiny HTML, nejlepším postupem je použití [PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync#Microsoft_AspNetCore_Mvc_Rendering_HtmlHelperPartialExtensions_PartialAsync_Microsoft_AspNetCore_Mvc_Rendering_IHtmlHelper_System_String_). Vrátí [IHtmlContent](/dotnet/api/microsoft.aspnetcore.html.ihtmlcontent) typ zabalené v `Task`. Metoda odkazuje prefixu volání s `@`:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialAsync)]

Alternativně můžete vykreslení částečného zobrazení s [RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync). Tato metoda nevrací výsledek. Jejich streamování vykresleného výstupu přímo do odpovědi. Protože metoda nevrací výsledek, musí být volána v rámci bloku kódu Razor:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

Protože streamování výsledek přímo, `RenderPartialAsync` může líp fungovat v některých scénářích. Však doporučuje používat `PartialAsync`.

### <a name="synchronous-html-helper"></a>Synchronní pomocné rutiny HTML

[Částečné](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial) a [RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial) jsou synchronní ekvivalenty `PartialAsync` a `RenderPartialAsync`v uvedeném pořadí. Použití synchronního ekvivalenty se nedoporučuje, protože existují scénáře, ve kterých zablokování. Budoucí verze nebude obsahovat synchronní metody.

> [!IMPORTANT]
> Pokud vaše zobrazení musí spustit kód, použijte [zobrazení komponenty](xref:mvc/views/view-components) místo částečné zobrazení.

::: moniker range=">= aspnetcore-2.1"

V ASP.NET Core 2.1 nebo novější, volání `Partial` nebo `RenderPartial` vede upozornění analyzátor. Například použití `Partial` provede následující upozornění:

> Použití IHtmlHelper.Partial může způsobit zablokování aplikace. Zvažte použití `<partial>` pomocné rutiny značky nebo `IHtmlHelper.PartialAsync`.

Nahraďte volání `@Html.Partial` s `@await Html.PartialAsync` nebo pomocná rutina částečné značky. Další informace o migraci pomocná rutina částečné značky najdete v tématu [migrace ze pomocné rutiny HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).

::: moniker-end

## <a name="partial-view-discovery"></a>Částečné zobrazení zjišťování

Při odkazování na částečné zobrazení, mohou odkazovat na umístění několika způsoby. Příklad:

::: moniker range=">= aspnetcore-2.1"

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
<partial name="_ViewName" />

// A view with this name must be in the same folder
<partial name="_ViewName.cshtml" />

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
<partial name="~/Views/Folder/_ViewName.cshtml" />
<partial name="/Views/Folder/_ViewName.cshtml" />

// Locate the view using a relative path
<partial name="../Account/_LoginPartial.cshtml" />
```

V předchozím příkladu částečné pomocné rutiny značky, který vyžaduje ASP.NET Core 2.1 nebo novější. Následující příklad používá asynchronní pomocných rutin HTML stejný úkol provést.

::: moniker-end

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
@await Html.PartialAsync("_ViewName")

// A view with this name must be in the same folder
@await Html.PartialAsync("_ViewName.cshtml")

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
@await Html.PartialAsync("~/Views/Folder/_ViewName.cshtml")
@await Html.PartialAsync("/Views/Folder/_ViewName.cshtml")

// Locate the view using a relative path
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

V jiné zobrazení složek může mít různé částečná zobrazení se stejným názvem souboru. Při odkazování na zobrazení podle názvu (bez přípony souboru), použijte zobrazení v všechny složky, částečné zobrazení ve stejné složce s nimi. Můžete také určit výchozí částečné zobrazení k použití, je v umístění *Shared* složky. Všechna zobrazení, které nemají vlastní verzi částečného zobrazení používá sdílené částečné zobrazení. Můžete použít výchozí částečné zobrazení (v *Shared*), je přepsán částečné zobrazení se stejným názvem ve stejné složce jako nadřazeného zobrazení.

Částečná zobrazení mohou být *zřetězené*&mdash;částečné zobrazení můžete volat jiné částečné zobrazení (tak dlouho, dokud nevytvoříte smyčku). V každém zobrazení či částečné zobrazení relativní cesty jsou vždy relativní k tohoto zobrazení, ne do kořenového adresáře nebo nadřazeném zobrazení.

> [!NOTE]
> A [Razor](xref:mvc/views/razor) `section` definované v částečné zobrazení je skryt nadřazené položky zobrazení. `section` Bude viditelná jenom pro částečné zobrazení, ve kterém je definována.

## <a name="access-data-from-partial-views"></a>Přístup k datům z částečné zobrazení

Při vytváření instance částečného zobrazení získá kopii objektu nadřazeného zobrazení zůstanou `ViewData` slovníku. Aktualizace provedené v datech v rámci částečného zobrazení se ukládají do nadřazeného zobrazení. `ViewData` provedené změny v částečné zobrazení při vrátí částečného zobrazení.

Můžete předat instanci [objektu ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) částečného zobrazení:

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

Model lze předat do částečné zobrazení. Model může být model zobrazení stránky nebo vlastní objekt. Můžete předat modelu `PartialAsync` nebo `RenderPartialAsync`:

```cshtml
@await Html.PartialAsync("_PartialName", viewModel)
```

Můžete předat instanci `ViewDataDictionary` a model zobrazení pro částečné zobrazení:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_PartialAsync)]

Následující kód ukazuje *Views/Articles/Read.cshtml* zobrazení, která obsahuje dvě částečné zobrazení. Druhý částečné zobrazení předává do modelu a `ViewData` částečného zobrazení. Použít zvýrazněný `ViewDataDictionary` přetížení konstruktoru k předání nového `ViewData` slovníku při zachování stávajících `ViewData` slovníku.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=17-20)]

*Zobrazení/sdílené/_AuthorPartial*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

*_ArticleSection* částečné:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

Za běhu, jsou vykreslovány částečné do nadřazeného zobrazení, které se vykreslí v rámci sdíleného *_Layout.cshtml*.

![Částečná zobrazení výstupu](partial/_static/output.png)

## <a name="additional-resources"></a>Další zdroje

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>

::: moniker-end
