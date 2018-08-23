---
title: Rozložení v ASP.NET Core
author: ardalis
description: Zjistěte, jak používat společné rozložení, sdílet direktivy a spustit běžné kód před vykreslení zobrazení v aplikaci ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/layout
ms.openlocfilehash: ad0b339572f387be8a636204015ffc361947acb8
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753339"
---
# <a name="layout-in-aspnet-core"></a>Rozložení v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/)

Zobrazení často sdílení visual a programové prvků. V tomto článku se dozvíte, jak používat společné rozložení, sdílet direktivy a spustit společný kód před vykreslení zobrazení v aplikaci ASP.NET Core.

## <a name="what-is-a-layout"></a>Co je rozložení

Většina webových aplikací mají společné rozložení, který poskytuje uživatelům konzistentní prostředí při práci ze stránky na stránku. Rozložení obvykle zahrnuje společné prvky uživatelského rozhraní, jako je například aplikace záhlaví, navigaci nebo prvky nabídky a zápatí.

![Příklad rozložení stránky](layout/_static/page-layout.png)

Společné struktury HTML, jako jsou skripty a šablony stylů také často používá mnoho stránek v rámci aplikace. Všechny tyto sdílené prvky mohou být definovány v *rozložení* soubor, který může odkazovat ve všech zobrazeních použít v aplikaci. Rozložení snížit duplicitního kódu v zobrazeních, usnadňuje postupujte [není opakujte sami (zkušební) Princip](http://deviq.com/don-t-repeat-yourself/).

Podle konvence je výchozí rozložení aplikace ASP.NET Core s názvem `_Layout.cshtml`. Šablony projektů Visual Studio ASP.NET Core MVC zahrnuje tento soubor rozložení `Views/Shared` složky:

![zobrazení složky v Průzkumníku řešení](layout/_static/web-project-views.png)

Toto rozložení definuje šablonu nejvyšší úrovně pro zobrazení v aplikaci. Aplikace nevyžadují rozložení a aplikací můžete definovat více než jedno rozložení s různá zobrazení zadání různá rozložení.

Příklad `_Layout.cshtml`:

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>Určení rozložení

Zobrazení Razor je k dispozici `Layout` vlastnost. Jednotlivá zobrazení zadat rozložení tak, že nastavíte tuto vlastnost:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

Zadané rozložení můžete použít úplnou cestu (Příklad: `/Views/Shared/_Layout.cshtml`) nebo částečný název (Příklad: `_Layout`). Když částečný název zadán, bude hledat rozložení souboru pomocí jeho procesu zjišťování standardní zobrazovací modul Razor. Složky spojené kontroleru je nejprve prohledán, za nímž následuje `Shared` složky. Tento proces zjišťování je stejný jako ten, který používá ke zjišťování [částečná zobrazení](partial.md).

Ve výchozím nastavení, musí volat každou rozložení `RenderBody`. Všude, kde volání `RenderBody` je umístěn, bude vykreslen obsah zobrazení.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Oddíly

Rozložení můžete volitelně odkazovat na jeden nebo více *oddíly*, voláním `RenderSection`. Oddíly umožňují organizovat umístění některých prvků stránky. Každé volání `RenderSection` můžete určit, zda je tento oddíl požadované nebo volitelné. Pokud není nalezen požadovaný oddíl, bude vyvolána výjimka. Jednotlivá zobrazení zadejte obsah, který mohl být vykreslen v rámci oddílu pomocí `@section` syntaxi Razor. Pokud zobrazení určující sekci, je nutné vykreslit (nebo dojde k chybě).

Příklad `@section` definice v zobrazení:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

Ve výše uvedeném kódu, skripty pro ověření jsou přidány do `scripts` části zobrazení, která obsahuje formulář. Ostatní zobrazení ve stejné aplikaci nemusí potřebovat další skripty a tak nebude potřeba nadefinovat oddíl skripty.

Oddíly definované v zobrazení jsou k dispozici pouze v její okamžitý rozložení stránky. Nelze se odkazovat z částečných zobrazení, zobrazení komponenty nebo jiných částí systému zobrazení.

### <a name="ignoring-sections"></a>Oddíly se ignoruje

Ve výchozím nastavení text a všechny části na stránce obsahu musí všechny zobrazovat na stránce rozložení. Zobrazovací modul Razor vynucuje Díky sledování, zda byl vykreslen text a každý oddíl.

Chcete-li dát pokyn zobrazovací modul, aby ignoroval text nebo oddíly, zavolejte `IgnoreBody` a `IgnoreSection` metody.

Text a každý oddíl na stránce Razor musí být buď vykreslen nebo ignorovat.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Import sdílených direktivy

Zobrazení pomocí direktivy Razor můžete provést řadu věcí, například obory názvů pro import nebo provádění [injektáž závislostí](dependency-injection.md). Direktivy, které sdílí mnoho zobrazení je možné zadat běžný `_ViewImports.cshtml` souboru. `_ViewImports` Soubor podporuje následující direktivy:

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

Tento soubor nepodporuje další funkce Razor, jako je například funkce a definice části.

Ukázka `_ViewImports.cshtml` souboru:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

`_ViewImports.cshtml` Soubor pro aplikaci ASP.NET Core MVC je obvykle umístěn ve `Views` složky. A `_ViewImports.cshtml` souboru je možné použít v jakékoli složce, ve kterém případu, použijí se jenom u zobrazení v této složce a jejích podsložkách. `_ViewImports` zpracování souborů začínají na kořenové úrovni, a potom pro každou složku k umístění zobrazení, tak nastavení zadané na kořenové úrovni lze přepsat na úrovni složek.

Například, pokud kořenové úrovni `_ViewImports.cshtml` soubor Určuje `@model` a `@addTagHelper`a další `_ViewImports.cshtml` určuje jiný soubor ve složce přidružené kontroleru zobrazení `@model` a přidá další `@addTagHelper`, zobrazení bude mít přístup k oběma pomocných rutin značek a bude používat ten `@model`.

Pokud je položek víc `_ViewImports.cshtml` soubory jsou spuštěny pro zobrazení, kombinované chování direktiv součástí `ViewImports.cshtml` soubory budou následujícím způsobem:

* `@addTagHelper`, `@removeTagHelper`: všech spuštění, v pořadí

* `@tagHelperPrefix`: na ten nejbližší do zobrazení přepíše všechny ostatní

* `@model`: na ten nejbližší do zobrazení přepíše všechny ostatní

* `@inherits`: na ten nejbližší do zobrazení přepíše všechny ostatní

* `@using`: všechny jsou zahrnuty; duplicity se ignorují.

* `@inject`: pro každou vlastnost na ten nejbližší do zobrazení přepíše všechny ostatní se stejným názvem vlastnosti

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Spuštění kódu před každou zobrazení

Pokud máte kód, je potřeba spustit před každé zobrazení, musí být umístěné ve `_ViewStart.cshtml` souboru. Podle konvence `_ViewStart.cshtml` soubor se nachází v `Views` složky. Příkazy uvedené v `_ViewStart.cshtml` jsou spouštěny před každou úplné zobrazení (nikoli rozložení a není částečná zobrazení). Stejně jako [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` jsou hierarchická. Pokud `_ViewStart.cshtml` soubor je definován ve složce přidružené kontroleru zobrazení, se spustí po definovanému v kořenovém adresáři `Views` složku (pokud existuje).

Ukázka `_ViewStart.cshtml` souboru:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Výše uvedeného souboru Určuje, zda budou používat všechna zobrazení `_Layout.cshtml` rozložení.

> [!NOTE]
> Ani `_ViewStart.cshtml` ani `_ViewImports.cshtml` jsou obvykle umístěny v `/Views/Shared` složky. Verze těchto souborů úrovni aplikace by měl být umístěn v přímo `/Views` složky.
