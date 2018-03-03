---
title: "Pomocník značka ukotvení"
author: pkellner
description: "Zjistit ASP.NET pomocné rutiny značka ukotvení základní atributy a roli, kterou každý atribut hraje v rozšíření chování značky HTML anchor."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 2d829b637f963c3e421fc4b89486709e38c06ab6
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="anchor-tag-helper"></a>Pomocník značka ukotvení

Podle [Petr Kellner](http://peterkellner.net) a [Scott Addie](https://github.com/scottaddie)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

[Pomocná značka ukotvení](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) rozšiřuje standardní anchor HTML (`<a ... ></a>`) značky přidáním nové atributy. Podle konvence, mají předponu názvy atributů `asp-`. Element anchor vykreslené `href` hodnota atributu je určen podle hodnot `asp-` atributy.

*SpeakerController* se používá v ukázky v tomto dokumentu:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

Inventář `asp-` atributy způsobem.

## <a name="asp-controller"></a>ASP-controller

[Asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) atribut přiřadí o řadič ke generování adresy URL. Následující kód obsahuje seznam všech mluvčí:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

Generovaný kód HTML:

```html
<a href="/Speaker">All Speakers</a>
```

Pokud `asp-controller` zadán atribut a `asp-action` není výchozí `asp-action` hodnota je přidružené k aktuálně prováděné zobrazení akce kontroleru. Pokud `asp-action` je vynechaný předchozí kód a pomocné značka ukotvení se používá v *HomeController*na *Index* zobrazení (*/Home*), je generovaný kód HTML:

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a>Akce ASP

[Asp akce](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) hodnota atributu představuje název akce kontroleru součástí vygenerovaného `href` atribut. Následující kód nastaví vygenerovaného `href` hodnota atributu na stránku hodnocení mluvčího:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

Generovaný kód HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Pokud žádné `asp-controller` atribut je určena, bude použita výchozí kontroleru volání zobrazení provádění aktuálního zobrazení.

Pokud `asp-action` hodnota atributu je `Index`, pak žádná akce je připojena k adrese URL, vedoucí k vyvolání výchozí `Index` akce. Akce zadané (nebo uvedena), musí existovat v kontroleru, kterou se odkazuje v `asp-controller`.

## <a name="asp-route-value"></a>asp-route-{value}

[Asp - trasy-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) atribut umožňuje předponu trasy zástupný znak. Všechny hodnoty zabírá `{value}` zástupný symbol interpretována jako potenciální parametr trasy. Pokud není nalezen výchozí trasu, připojí se tato předpona trasy k vygenerovaného `href` atribut jako parametr žádosti a hodnotu. Jinak je nahrazena v šabloně trasy.

Vezměte v úvahu následující akce kontroleru:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

Pomocí výchozí šablony trasy definované v *Startup.Configure*:

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

Zobrazení MVC používá model, poskytuje akce, následujícím způsobem:

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

Výchozí trasu `{id?}` , odpovídal zástupný symbol. Generovaný kód HTML:

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

Předpokládejme, že předpona trasy není součástí odpovídající směrování šablony, stejně jako u následující zobrazení MVC:

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

Následující kód HTML je vygenerovat, protože `speakerid` nebyl nalezen v odpovídající trasy:

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

Pokud má jedna `asp-controller` nebo `asp-action` nejsou zadané, pak stejné zpracování výchozí je následovaný, protože `asp-route` atribut.

## <a name="asp-route"></a>ASP trasy

[Asp trasy](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) atribut se používá pro vytvoření adresy URL připojení přímo k pojmenovanou trasu. Pomocí [atributy směrování](xref:mvc/controllers/routing#attribute-routing), může mít název trasy, jak je znázorněno `SpeakerController` a používá v jeho `Evaluations` akce:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

V následující kód `asp-route` pojmenovanou trasu odkazuje atribut:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

Pomocník značka ukotvení vytváří trasu přímo k této akci kontroleru pomocí adresy URL */mluvčího/hodnocení*. Generovaný kód HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Pokud `asp-controller` nebo `asp-action` je zadán kromě `asp-route`, postupu generované nemusí být očekávat. Aby nedošlo ke konfliktu trasy `asp-route` by neměl být použit s `asp-controller` a `asp-action` atributy.

## <a name="asp-all-route-data"></a>asp-all-route-data

[Asp všechny trasy dat](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) atribut podporuje vytváření slovník páry klíč hodnota. Klíč je název parametru a hodnota je hodnota parametru.

V následujícím příkladu je slovník inicializovat a předána do zobrazení Razor. Alternativně může být předán data pomocí modelu.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

Předchozí kód generuje následující HTML:

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

`asp-all-route-data` Slovníku se sloučí k vytvoření řetězec dotazu, který splňuje požadavky přetížené `Evaluations` akce:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

Pokud se ve slovníku všechny klíče shodují parametry trasy, tyto hodnoty jsou nahrazena v postupu podle potřeby. Neodpovídající hodnoty jsou generovány jako parametry žádosti.

## <a name="asp-fragment"></a>asp-fragment

[Asp fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) atribut definuje fragment adresy URL pro připojení k adrese URL. Pomocník značka ukotvení přidá znak hash (#). Vezměte v úvahu následující kód:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

Generovaný kód HTML:

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

Hodnota hash značky jsou užitečné při vytváření aplikace na straně klienta. Mohou být použity pro snadné označování a hledání v jazyce JavaScript, třeba.

## <a name="asp-area"></a>asp-area

[Asp oblasti](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) atribut nastaví název oblasti, použije se k nastavení odpovídající trasy. Následující příklad znázorňuje, jak atribut oblasti způsobí přemapování trasy. Nastavení `asp-area` na "Blogy" předpony adresáři *oblasti nebo blogy* pro trasy přidružené kontrolery a zobrazení pro tuto značku ukotvení.

* **< název projektu\>**
  * **wwwroot**
  * **Oblasti**
    * **Blogy**
      * **Kontrolery**
        * *HomeController.cs*
      * **Zobrazení**
        * **domácí**
          * *AboutBlog.cshtml*
          * *Index.cshtml*
        * *_ViewStart.cshtml*
  * **Kontrolery**

Zadané předchozí hierarchii adresářů, kód tak, aby odkazovaly *AboutBlog.cshtml* souboru je:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

Generovaný kód HTML:

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> Pro oblasti pro práci v aplikaci MVC musí šablona trasy obsahovat odkaz na oblasti, pokud existuje. Tato šablona je reprezentována druhý parametr `routes.MapRoute` volání metody *Startup.Configure*: [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a>asp-protocol

[Asp protokol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) atribut je pro zadání protokol (například `https`) v svoji adresu URL. Příklad:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

Generovaný kód HTML:

```html
<a href="https://localhost/Home/About">About</a>
```

Název hostitele v příkladu je localhost, ale pomocný značka ukotvení používá webu veřejné domény při generování adresy URL.

## <a name="asp-host"></a>asp-host

[Asp hostitele](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) atribut je pro zadání názvu hostitele v svoji adresu URL. Příklad:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

Generovaný kód HTML:

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a>stránka ASP

[Stránka asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) atribut se používá s stránky Razor. Použijte je k nastavení značku ukotvení `href` hodnota atributu na konkrétní stránku. Prefixu název stránky s lomítkem ("/") vytvoří adresu URL.

Následující příklad ukazuje na účastníka Razor stránky:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

Generovaný kód HTML:

```html
<a href="/Attendee">All Attendees</a>
```

`asp-page` Atribut je vzájemně se vylučuje s `asp-route`, `asp-controller`, a `asp-action` atributy. Ale `asp-page` lze použít s `asp-route-{value}` řídit směrování, jak ukazuje následující kód:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

Generovaný kód HTML:

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a>asp-page-handler

[Rutina stránky asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) atribut se používá s stránky Razor. Je určený pro propojení na konkrétní stránky obslužné rutiny.

Vezměte v úvahu obslužná rutina následující stránky:

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

Model stránky přidruženému značek odkazy na `OnGetProfile` obslužná rutina stránky. Všimněte si, že `On<Verb>` předpona názvu stránky obslužná rutina metoda je vynechán v `asp-page-handler` hodnota atributu. Pokud to byly asynchronní metodu, `Async` přípona by být příliš vynechán.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

Generovaný kód HTML:

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>Další zdroje

* [Oblasti](xref:mvc/controllers/areas)
* [Úvod do stránky Razor](xref:mvc/razor-pages/index)
