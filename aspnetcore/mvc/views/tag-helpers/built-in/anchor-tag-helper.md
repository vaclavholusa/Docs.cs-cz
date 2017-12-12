---
title: "Pomocník značka ukotvení | Microsoft Docs"
author: pkellner
description: "Ukazuje, jak pracovat s pomocná značka ukotvení"
keywords: "ASP.NET Core, značka pomocné rutiny"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: e3754c4313f01bc746ccb8efe11611ae213e3955
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="anchor-tag-helper"></a>Pomocník značka ukotvení

Podle [Petr Kellner](http://peterkellner.net) 

Pomocník značka ukotvení vylepšuje HTML anchor (`<a ... ></a>`) značky přidáním nové atributy. Vygeneruje odkaz (na `href` značka) je vytvořený pomocí nové atributy. Aby adresa URL může obsahovat volitelné protokol, jako je například https.

Níže mluvčího řadiče se používá v ukázky v tomto dokumentu.

<br/>
**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>Atributy pomocné rutiny značka ukotvení

- - -

### <a name="asp-controller"></a>ASP-controller

`asp-controller`je použito k přidružení řadič, který se použije k vytvoření adresy URL. Řadičů určených musí existovat v aktuálním projektu. Následující kód obsahuje seznam všech mluvčí: 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

Generovaný kód bude:

```html
<a href="/Speaker">All Speakers</a>
```

Pokud `asp-controller` je zadán a `asp-action` není, výchozím `asp-action` bude výchozí metodu řadiče aktuálně prováděné zobrazení. Aby se v předchozím příkladu, pokud `asp-action` je vynechána, a tohoto pomocníka značka ukotvení se generují z *HomeController*na `Index` zobrazení (**/Home**), bude vygenerovaný kód:


```html
<a href="/Home">All Speakers</a>
```

- - -
  
### <a name="asp-action"></a>Akce ASP

`asp-action`Název metody akce v kontroleru, který bude zahrnut v vygenerovaného `href`. Například následující kód nastavit vygenerovaného `href` tak, aby odkazoval na stránku podrobností mluvčího:

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

Generovaný kód bude:

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

Pokud žádné `asp-controller` zadán atribut, použije se výchozí kontroleru volání zobrazení provádění aktuálního zobrazení.  
 
Pokud atribut `asp-action` je `Index`, pak žádná akce je připojena k adrese URL, což výchozí `Index` volané metodě. Akce zadané (nebo uvedena), musí existovat v kontroleru, kterou se odkazuje v `asp-controller`.

- - -
  
<a name="route"></a>
### <a name="asp-route-value"></a>ASP - trasy-{value}

`asp-route-`je předponu trasy zástupnému znaku. Libovolná hodnota, která jste uložili po konci dash, bude vyhodnocen jako potenciální parametr trasy. Pokud není nalezen výchozí trasu, připojí se k vygenerovaný odkaz href jako parametr žádosti a hodnota tuto předponu trasy. V opačném případě se bude nahrazena v šabloně trasy.

Za předpokladu, že je metoda kontroleru definovali takto:

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };      
    return View(viewName, speaker);
}
```

A jste výchozí šablonu trasy definované v vaše *Startup.cs* následujícím způsobem:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

**Cshtml** soubor, který obsahuje pomocný značka ukotvení nezbytných k používání **mluvčího** parametr modelu předané z řadiče zobrazení je následující:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Generovaný kód jazyka HTML se bude následujícím způsobem, protože **id** byl nalezen v výchozí trasu.

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

Pokud předponu trasy není součástí směrování šablony najít, což je případ s následující **cshtml** souboru:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Generovaný kód jazyka HTML se bude následujícím způsobem, protože **speakerid** nebyl nalezen v trasy, která odpovídá:


```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

Pokud má jedna `asp-controller` nebo `asp-action` nejsou zadané, pak stejné zpracování výchozí je následovaný, protože `asp-route` atribut.

- - -

### <a name="asp-route"></a>ASP trasy

`asp-route`poskytuje způsob, jak vytvořit adresu URL, který odkazuje přímo na pojmenovanou trasu. Pomocí směrování atributů, trasu může mít název jak je znázorněno `SpeakerController` a používá v jeho `Evaluations` metoda.

`Name = "speakerevals"`informuje pomocná značka ukotvení ke generování trasu přímo k dané metody kontroleru pomocí adresy URL `/Speaker/Evaluations`. Pokud `asp-controller` nebo `asp-action` je zadán kromě `asp-route`, postupu generované nemusí být očekávat. `asp-route`Nepoužívejte pomocí atributů `asp-controller` nebo `asp-action` aby nedošlo ke konfliktu trasy.

- - -

### <a name="asp-all-route-data"></a>ASP všechny trasy dat

`asp-all-route-data`Umožňuje vytváření slovník párů klíčových hodnot, kde klíč je název parametru a hodnota je hodnota přidružená k tomuto klíči.

Jako v příkladu níže znázorňuje slovníku vložené se vytvoří a data jsou předána do zobrazení razor. Jako alternativu může být data předány také pomocí modelu.

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent" 
   asp-all-route-data="dict">SpeakerEvals</a>
```

Výše uvedený kód generuje následující adresu URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true

Pokud po kliknutí na odkaz, metoda kontroleru `EvaluationsCurrent` je volána. Je volána, protože tento řadič se dvěma řetězcovými parametry, které odpovídají, co byl vytvořen z `asp-all-route-data` slovníku.

Pokud žádné klíče ve slovníku shody směrovat parametry, tyto hodnoty bude nahrazena v postupu podle potřeby a ostatní neodpovídající hodnoty se budou generovat jako parametry žádosti.

- - -

### <a name="asp-fragment"></a>ASP fragment

`asp-fragment`Definuje fragment adresy URL pro připojení k adrese URL. Pomocník značka ukotvení přidá znak hash (#). Pokud jste vytvořit značku:

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

Vygenerovaná adresa URL bude: http://localhost/Speaker/Evaluations#SpeakerEvaluations

Hodnota hash značky jsou užitečné při sestavování aplikací na straně klienta. Mohou být použity pro snadné označování a hledání v jazyce JavaScript, třeba.

- - -

### <a name="asp-area"></a>oblasti ASP

`asp-area`Nastaví název oblasti, který používá ASP.NET Core nastavit odpovídající trasy. Níže jsou příklady jak atribut oblasti způsobí přemapování trasy. Nastavení `asp-area` pro blogy předpony adresáři `Areas/Blogs` pro trasy přidružené kontrolery a zobrazení pro tuto značku ukotvení.

* název projektu

  * *Wwwroot*

  * *Oblasti*

    * *Blogy*

      * *Řadiče*

        * *HomeController.cs*

      * *Zobrazení*

        * *Domovské*

          * *Index.cshtml*
          
          * *AboutBlog.cshtml*
          
  * *Řadiče*
  

        
Určení značku oblasti, který je platný, například ```area="Blogs"``` při odkazování na ```AboutBlog.cshtml``` soubor bude vypadat jako následující využitím pomocné rutiny značka ukotvení.

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

Generovaný kód HTML bude obsahovat segmentu oblasti a bude takto:

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> Pro MVC oblasti pro práci ve webové aplikaci musí šablona trasy obsahovat odkaz na oblasti, pokud existuje. Šablony, které je druhý parametr z `routes.MapRoute` volání metody, se zobrazí jako:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`

- - -

### <a name="asp-protocol"></a>protokol ASP

`asp-protocol` Je pro zadání protokol (například `https`) v svoji adresu URL. Příklad značky pomocné rutiny ukotvení s uvedením protokolu bude vypadat takto:

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

a bude generují kód HTML následujícím způsobem:

```<a href="https://localhost/Home/About">About</a>```

Domény v příkladu je localhost, ale pomocný značka ukotvení používá webu veřejné domény při generování adresy URL.

- - -

## <a name="additional-resources"></a>Další zdroje

* [Oblasti](xref:mvc/controllers/areas)
