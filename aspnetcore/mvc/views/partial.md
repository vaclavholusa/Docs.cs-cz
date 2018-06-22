---
title: Částečná zobrazení v ASP.NET Core
author: ardalis
description: Zjistěte, jak je částečné zobrazení zobrazení, který je vykreslen v rámci jiného zobrazení a kdy by měly být použity v aplikacích ASP.NET Core.
ms.author: riande
ms.date: 03/14/2018
uid: mvc/views/partial
ms.openlocfilehash: f3782961a63c08293a483ec7a75dadff2031b131
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279529"
---
# <a name="partial-views-in-aspnet-core"></a>Částečná zobrazení v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), a [Scott Sauber](https://twitter.com/scottsauber)

Jádro ASP.NET MVC podporuje částečné zobrazení, která jsou užitečné, když máte opakovaně použitelné částí webových stránek, které chcete sdílet mezi různá zobrazení.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>Jaké jsou částečné zobrazení?

Částečné zobrazení je zobrazení, který je vykreslen v rámci jiného zobrazení. Výstup ve formátu HTML generované provádění částečné zobrazení vykreslením do zobrazení volání (nebo nadřazené). Podobně jako zobrazení, částečná zobrazení použít *.cshtml* příponu souboru.

## <a name="when-should-i-use-partial-views"></a>Kdy použít částečné zobrazení?

Částečná zobrazení jsou účinný způsob rozdělení velké zobrazení do menší součásti. Můžou snížit duplicitních zobrazení obsahu a umožňují zobrazení prvků znovu použít. Společné prvky rozložení musí být zadán v [_Layout.cshtml](layout.md). Opakovaně použitelný obsah bez rozložení můžete zapouzdřený do částečné zobrazení.

Pokud máte komplexní stránky skládá z několika logických částí, může být užitečné pro práci s každého jednotlivého jako vlastní částečné zobrazení. Každá část stránky lze zobrazit izolovaně od zbytku stránky a zobrazení pro vlastní stránky se mnohem jednodušší, protože pouze obsahuje celkový strukturu stránky a volání k vykreslení částečných zobrazení.

Tip: Použijte [nemáte opakujte sami Princip](http://deviq.com/don-t-repeat-yourself/) v zobrazení.

## <a name="declaring-partial-views"></a>Deklarování částečné zobrazení

Částečná zobrazení vzniknou podobně jako ostatní zobrazení: vytváření *.cshtml* souboru v rámci *zobrazení* složky. Není žádný sémantického rozdíl mezi částečné zobrazení a zobrazení regulární – se pouze vykreslují odlišně. Máte zobrazení, které se vrátí přímo před vytvořením řadiče `ViewResult`, a stejnou zobrazení lze použít jako částečné zobrazení. Hlavní rozdíl mezi způsobu vykreslení zobrazení a částečné zobrazení je, že částečná zobrazení nespouštět *soubor _ViewStart.cshtml* (při zobrazení - další informace o *soubor _ViewStart.cshtml* v [rozložení ](layout.md)).

## <a name="referencing-a-partial-view"></a>Odkazování na částečné zobrazení

Z v rámci zobrazení stránky, existuje několik způsobů ve kterých můžete vykreslení částečného zobrazení. Osvědčeným postupem je použít `Html.PartialAsync`, která vrátí `IHtmlString` a můžete odkazovat pomocí prefixu volání s `@`:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

Můžete vykreslení částečného zobrazení s `RenderPartialAsync`. Tato metoda není vrácení výsledku; je-li datové proudy vykreslené výstup přímo do odpovědi. Protože se nevrací výsledek, musí být volána v rámci bloku kódu Razor:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=11-13)]

Protože proudů výsledek přímo, `RenderPartialAsync` může v některých scénářích líp fungovat. Je však doporučeno, abyste používali `PartialAsync`.

Přestože jsou synchronní ekvivalenty `Html.PartialAsync` (`Html.Partial`) a `Html.RenderPartialAsync` (`Html.RenderPartial`), použijte synchronní ekvivalenty není doporučeno, protože existují scénáře, kde budou zablokování. Synchronní metody není k dispozici v budoucích verzích.

> [!NOTE]
> Pokud vaše zobrazení musí ke spouštění kódu, je použití vzoru doporučené [zobrazení součást](view-components.md) místo částečné zobrazení.

### <a name="partial-view-discovery"></a>Částečné zobrazení zjišťování

Při odkazování na částečné zobrazení, můžete odkazovat na umístění, několika způsoby:

```cshtml
// Uses a view in current folder with this name
// If none is found, searches the Shared folder
@await Html.PartialAsync("ViewName")

// A view with this name must be in the same folder
@await Html.PartialAsync("ViewName.cshtml")

// Locate the view based on the application root
// Paths that start with "/" or "~/" refer to the application root
@await Html.PartialAsync("~/Views/Folder/ViewName.cshtml")
@await Html.PartialAsync("/Views/Folder/ViewName.cshtml")

// Locate the view using relative paths
@await Html.PartialAsync("../Account/LoginPartial.cshtml")
```

Může mít různé částečná zobrazení se stejným názvem v jiné zobrazení složky. Při odkazování na zobrazení podle názvu (bez přípony souboru), použije zobrazení v každé složky ve stejné složce s nimi částečné zobrazení. Můžete také zadat výchozí částečné zobrazení chcete použít, v jeho umístění *sdílené* složky. Sdílené částečné zobrazení použije všechna zobrazení, které nemají své vlastní verzi částečné zobrazení. Můžete mít výchozí částečné zobrazení (v *sdílené*), který přepsat částečné zobrazení se stejným názvem ve stejné složce jako nadřazeného zobrazení.

Částečná zobrazení může být *zřetězené*. To znamená částečné zobrazení můžete volat jiné částečné zobrazení, (, dokud nevytvoříte smyčku). V každém zobrazení či částečné zobrazení relativní cesty vždy jsou relativní vzhledem k zobrazení tohoto zobrazení, není kořenovou nebo nadřazené.

> [!NOTE]
> Pokud je deklarovat [Razor](razor.md) `section` v částečné zobrazení, nebude viditelná pro jeho nadřazených položek; bude omezena na částečné zobrazení.

## <a name="accessing-data-from-partial-views"></a>Přístup k datům z částečné zobrazení

Při vytváření instance částečného zobrazení získá kopii nadřazeného zobrazení zůstanou `ViewData` slovníku. Aktualizace provedené v datech v rámci částečné zobrazení nejsou zachována pro nadřazené zobrazení. `ViewData` změnit v částečné zobrazení bude ztracena, jakmile vrátí částečné zobrazení.

Můžete předat instanci `ViewDataDictionary` částečného zobrazení:

```cshtml
@await Html.PartialAsync("PartialName", customViewData)
```

Model můžete předat také do částečné zobrazení. To může být modelu zobrazení stránky nebo vlastní objekt. Můžete předat modelu pro `PartialAsync` nebo `RenderPartialAsync`:

```cshtml
@await Html.PartialAsync("PartialName", viewModel)
```

Můžete předat instanci `ViewDataDictionary` a modelu zobrazení pro částečné zobrazení:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

Kód níže ukazuje *Views/Articles/Read.cshtml* zobrazení, která obsahuje dvě částečné zobrazení. Druhý částečné zobrazení předává v modelu a `ViewData` částečného zobrazení. Můžete předat nové `ViewData` slovníku a přitom zachovat stávající `ViewData` Pokud použijete konstruktor přetížení `ViewDataDictionary` zvýrazněná níže:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

*Zobrazení nebo sdílené nebo AuthorPartial*:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

*ArticleSection* částečné:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

Za běhu, jsou vykreslovány částečné do nadřazeného zobrazení, které se zobrazí v rámci sdílený *_Layout.cshtml*

![výstup částečné zobrazení](partial/_static/output.png)
