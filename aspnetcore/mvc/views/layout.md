---
title: "Rozložení"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: f225e2a93edfc552961f9f16294bc0ace6eb0002
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="layout"></a>Rozložení

Podle [Steve Smith](https://ardalis.com/)

Zobrazení často sdílet visual a programovací elementy. V tomto článku se naučíte používat běžné rozložení, sdílet direktivy a spustit běžné kód před vykreslování zobrazení v aplikaci ASP.NET.

## <a name="what-is-a-layout"></a>Co je rozložení

Většina webových aplikací mít běžné rozložení, které poskytuje uživateli konzistentního prostředí jako stisku stránky. Rozložení obvykle obsahuje společné elementy uživatelského rozhraní, například záhlaví aplikace, navigace nebo nabídky elementy a zápatí stránky.

![Příklad rozložení stránky](layout/_static/page-layout.png)

Počet stránek v aplikaci také často používají společné struktury HTML, například skriptů a šablon. Všechny tyto sdílené prvky může být definována v *rozložení* souboru, který může odkazovat potom všechna zobrazení používaných v rámci aplikace. Rozložení snížit duplicitní kódu v zobrazeních, pomoci jim postupujte podle kroků [nemáte opakujte sami (suchého) Princip](http://deviq.com/don-t-repeat-yourself/).

Podle konvence je výchozí rozložení pro aplikaci ASP.NET s názvem `_Layout.cshtml`. Šablona projektu Visual Studio jádro ASP.NET MVC zahrnuje tento soubor rozložení `Views/Shared` složky:

![zobrazení složky v Průzkumníku řešení](layout/_static/web-project-views.png)

Toto rozložení definuje šablonu nejvyšší úrovně pro zobrazení v aplikaci. Aplikace nevyžadují rozložení a aplikace můžete definovat více než jedno rozložení s různá zobrazení určení různých rozložení.

Příklad `_Layout.cshtml`:

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>Určení rozložení

Zobrazení syntaxe Razor je k dispozici `Layout` vlastnost. Jednotlivá zobrazení zadejte rozložení nastavením této vlastnosti:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

Rozložení zadaný můžete použít úplnou cestu (Příklad: `/Views/Shared/_Layout.cshtml`) nebo částečný název (Příklad: `_Layout`). Pokud je zadaný částečný název, bude zobrazovací modul Razor hledat soubor rozložení pomocí jeho procesu zjišťování standardní. Řadič spojená je prohledána složka nejprve následuje `Shared` složky. Tento proces zjišťování je stejný jako ten, který používá ke zjišťování [částečná zobrazení](partial.md).

Ve výchozím nastavení, musí volat každé rozložení `RenderBody`. Kdekoli volání `RenderBody` je umístěna, bude vykreslen obsah zobrazení.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Oddíly

Rozložení můžete volitelně odkazovat na jeden nebo více *části*, voláním `RenderSection`. Oddíly umožňují uspořádat, kde má být umístěn některé prvky na stránce. Každé volání `RenderSection` můžete určit, zda je daný oddíl požadované nebo volitelné. Pokud není nalezen požadovaný oddíl, bude vyvolána výjimka. Jednotlivá zobrazení zadejte obsah, mají být zpracovány v oddílu pomocí `@section` syntaxi Razor. Pokud zobrazení určující sekci, musí být vykreslen (nebo dojde k chybě).

Příklad `@section` definice v zobrazení:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

Ve výše uvedeném kódu, ověřování skriptů jsou přidány do `scripts` část zobrazení, která zahrnuje formuláře. Ostatní zobrazení ve stejné aplikaci, nemusí vyžadovat další skripty a proto nebude muset definovat skripty oddíl.

Oddíly, které jsou definované v zobrazení jsou k dispozici pouze v jeho okamžitou rozložení stránky. Se nelze odkazovat z částečné., zobrazení součásti nebo dalšími částmi systému zobrazení.

### <a name="ignoring-sections"></a>Ignoruje části

Ve výchozím nastavení text a všechny části obsahu stránce musí všechny pro vykreslení ke stránce rozložení. Zobrazovací modul Razor vynucuje tato sledování, zda subjektem a každá část byl vykreslen.

Chcete-li určit, aby modul zobrazení ignorovat textu nebo částech, zavolejte `IgnoreBody` a `IgnoreSection` metody.

Text a každé části na stránce Razor musí být buď vykresluje nebo ignorovat.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Import sdílených direktivy

Zobrazení pomocí direktivy Razor můžete provést celou řadu věcí, jako je například import obory názvů nebo provádění [vkládání závislostí](dependency-injection.md). Direktivy sdíleny mnoho zobrazení je možné zadat společného `_ViewImports.cshtml` souboru. `_ViewImports` Soubor podporuje následující direktivy:

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

Soubor nepodporuje jiných součástí Razor, například funkce a definice části.

Ukázka `_ViewImports.cshtml` souboru:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

`_ViewImports.cshtml` Souboru u aplikace ASP.NET MVC základní obvykle umístěny v `Views` složky. A `_ViewImports.cshtml` soubor může být umístěn v rámci libovolné složky, ve kterém případu, uplatní se jenom u zobrazení v této složce a jejích podsložkách. `_ViewImports`zpracování souborů začínající na kořenové úrovni a potom pro každou složku plán pro umístění zobrazení sebe, tak na kořenové úrovni zadané nastavení může být přepsána na úrovni složek.

Například, pokud kořenové úrovni `_ViewImports.cshtml` soubor Určuje `@model` a `@addTagHelper`a jiné `_ViewImports.cshtml` soubor ve složce přidružené řadiče zobrazení určuje jinou `@model` a přidá další `@addTagHelper`, zobrazení bude mít přístup k Pomocníci obě značky a bude používat k tomu `@model`.

Pokud více `_ViewImports.cshtml` soubory spouštějí pro zobrazení, kombinaci chování direktivy součástí `ViewImports.cshtml` soubory bude vypadat takto:

* `@addTagHelper`, `@removeTagHelper`: všechny spuštění, v pořadí

* `@tagHelperPrefix`: byl nejblíže k zobrazení přepíše všechny ostatní

* `@model`: byl nejblíže k zobrazení přepíše všechny ostatní

* `@inherits`: byl nejblíže k zobrazení přepíše všechny ostatní

* `@using`: všechny jsou zahrnuty; duplicity se ignorují.

* `@inject`: pro každou vlastnost nejbližší jeden k zobrazení přepíše všechny ostatní se stejným názvem vlastnosti

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Spuštění kódu před každou zobrazení

Pokud máte kód budete muset spustit před každé zobrazení, mají být umístěny do `_ViewStart.cshtml` souboru. Podle konvence `_ViewStart.cshtml` soubor je umístěný ve `Views` složky. Příkazy uvedené v `_ViewStart.cshtml` spouštějí před každé úplné zobrazení (není rozložení a není částečná zobrazení). Jako [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` je hierarchická. Pokud `_ViewStart.cshtml` soubor je definované ve složce přidružené řadiče zobrazení, se spustí po definovanému v kořenovém `Views` složku (pokud existuje).

Ukázka `_ViewStart.cshtml` souboru:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Výše uvedeného souboru Určuje, že bude používat všechna zobrazení `_Layout.cshtml` rozložení.

> [!NOTE]
> Ani `_ViewStart.cshtml` ani `_ViewImports.cshtml` jsou obvykle umístěny v `/Views/Shared` složky. Verze těchto souborů úrovni aplikace musí být umístěny přímo v `/Views` složky.
