---
title: "Značka Pomocník jádro ASP.NET MVC do mezipaměti"
author: pkellner
description: "Ukazuje, jak pracovat s pomocná značky mezipaměti"
keywords: "ASP.NET Core, značka pomocné rutiny"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a012
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 74080d089dc7a72da96f9f18d613cb313cd930db
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Značka Pomocník jádro ASP.NET MVC do mezipaměti

Podle [Petr Kellner](http://peterkellner.net) 

Pomocník značky mezipaměti umožňuje výrazně zlepšit výkon vaší aplikace ASP.NET Core pomocí ukládání do mezipaměti jeho obsah do vnitřní mezipaměti poskytovatele ASP.NET Core.

Nastaví výchozí zobrazovací modul Razor `expires-after` než 20 minut.

Následující kód Razor ukládá do mezipaměti data a času:

```cshtml
<cache>@DateTime.Now</cache>
```

První požadavek na stránku, který obsahuje `CacheTagHelper` se zobrazí aktuální datum a čas. Další požadavky se zobrazí hodnota uložená v mezipaměti, dokud mezipaměti vyprší platnost (výchozí nastavení 20 minut) nebo vyřazování podle přetížení paměti.

Můžete nastavit dobu uložení do mezipaměti s následujícími atributy:

## <a name="cache-tag-helper-attributes"></a>Mezipaměti atributů značky pomocné rutiny

- - -

### <a name="enabled"></a>povoleno    


| Typ atributu    | Platné hodnoty      |
|----------------   |----------------   |
| Logická hodnota           | "true" (výchozí)  |
|                   | "false"   |


Určuje, zda je ukládat do mezipaměti obsah uzavřené do pomocné rutiny značky mezipaměti. Výchozí hodnota je `true`.  Pokud nastavena na `false` tohoto pomocníka značky mezipaměti bude mít neplatí ukládání do mezipaměti pro vykreslený výstup.

Příklad:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>vyprší dne 

| Typ atributu    | Příklad hodnoty     |
|----------------   |----------------   |
| DateTimeOffset    | "@new DateTime(2025,1,29,17,02,0)"    |


Nastaví datum vypršení platnosti absolutní. V následujícím příkladu bude ukládat do mezipaměti obsah pomocná značky mezipaměti až 17:02:00 na 29 leden 2025.

Příklad:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>Po vypršení platnosti

| Typ atributu    | Příklad hodnoty     |
|----------------   |----------------   |
| Časový interval    | "@TimeSpan.FromSeconds(120)"    |


Nastaví dobu od prvního požadavku pro ukládání do mezipaměti obsah. 

Příklad:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>klouzavé vypršení platnosti

| Typ atributu    | Příklad hodnoty     |
|----------------   |----------------   |
| Časový interval    | "@TimeSpan.FromSeconds(60)"     |


Nastaví dobu, která by měla být vyřazena položku mezipaměti, pokud není přístup.

Příklad:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>měnit podle hlavičky

| Typ atributu    | Příklad hodnoty                |
|----------------   |----------------               |
| String            | "User-Agent"                  |
|                   | "User-Agent, kódování obsahu" |

Přijme hodnotu jedné hlavičky nebo seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, když se změní, oddělených čárkami. Následující příklad monitoruje hodnota hlavičky `User-Agent`. V příkladu se uloží obsah do mezipaměti pro každý jiný `User-Agent` webového serveru.

Příklad:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a>se liší podle dotazu

| Typ atributu    | Příklad hodnoty                |
|----------------   |----------------               |
| String            | "Zkontrolujte"                |
|                   | "Zkontrolujte modelu" |

Přijme jeden záhlaví hodnotu nebo seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, když se změní hodnota hlavičky oddělené čárkami. V následujícím příkladu vypadá na hodnoty `Make` a `Model`.

Příklad:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>se liší podle trasy

| Typ atributu    | Příklad hodnoty                |
|----------------   |----------------               |
| String            | "Zkontrolujte"                |
|                   | "Zkontrolujte modelu" |

Přijme hodnotu jedné hlavičky nebo seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, když se změna hodnoty parametru data trasy oddělených čárkami. Příklad:

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
*Index.cshtml*

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a>se liší podle cookie

| Typ atributu    | Příklad hodnoty                |
|----------------   |----------------               |
| String            | ". AspNetCore.Identity.Application"                |
|                   | ". AspNetCore.Identity.Application,HairColor" |

Přijme jeden záhlaví hodnotu nebo seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, pokud se změní (s) hodnoty hlavičky oddělené čárkami. V následujícím příkladu vypadá v souboru cookie přidruženého ASP.NET Identity. Když je uživatel ověřen žádosti soubor cookie nastavit který aktivuje aktualizace mezipaměti.

Příklad:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a>se liší podle uživatele

| Typ atributu    | Příklad hodnoty                |
|----------------   |----------------               |
| Boolean             | "true"                  |
|                     | "Nepravda" (výchozí) |

Určuje, zda má mezipaměti resetovat při změně přihlášeného uživatele (nebo objekt kontextu zabezpečení). Aktuální uživatel je také označován jako objekt kontextu požadavku a lze je zobrazit v zobrazení syntaxe Razor pod položkou `@User.Identity.Name`.

Následující příklad hledána v aktuálně přihlášeného uživatele.  

Příklad:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Pomocí tohoto atributu udržuje obsah v mezipaměti prostřednictvím cyklus přihlášení a odhlášení.  Při použití `vary-by-user="true"`, přihlášení a odhlášení akce zruší platnost mezipaměti pro ověřené uživatele.  Mezipaměti je neplatná, protože byl vygenerován novou hodnotu jedinečný soubor cookie na přihlášení. Mezipaměti bude zachována pro anonymní stavu, pokud žádný soubor cookie je k dispozici nebo vypršela platnost. To znamená, že pokud je přihlášen žádný uživatel, se zachová mezipaměti.

- - -

### <a name="vary-by"></a>se liší podle

| Typ atributu    | Příklad hodnoty                |
|----------------   |----------------               |
| String             | "@Model"                 |


Umožňuje přizpůsobení získá jaké data uložena do mezipaměti. Při aktualizaci objektu odkazuje atributu řetězec hodnotu změny, obsah pomocná značky mezipaměti. Zřetězení řetězců modelu hodnot často jsou přiřazeny tomuto atributu.  Efektivní, to znamená, že aktualizace zřetězených hodnot zruší platnost mezipaměti.

V následujícím příkladu se předpokládá, metoda kontroleru vykreslování zobrazení součtů celočíselnou hodnotu dva parametry trasy, `myParam1` a `myParam2`a vrátí, jako vlastnost jeden model. Při změně této součet obsah pomocná značky mezipaměti je vykreslen a uložili do mezipaměti znovu.  

Příklad:

Akce:

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

*Index.cshtml*

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a>Priorita

| Typ atributu    | Příklad hodnoty                |
|----------------   |----------------               |
| CacheItemPriority  | "Vysoká"                   |
|                    | "Nízká" |
|                    | "NeverRemove" |
|                    | "Normální" |

Obsahuje mezipaměti vyřazení pokyny k poskytovateli předdefinované mezipaměti. Webový server bude vyřazení `Low` nejprve mezipaměti položky, když je paměť přetížena.

Příklad:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority` Atribut nezaručí konkrétní úroveň mezipaměti uchování. `CacheItemPriority`je pouze návrhu. Nastavení tohoto atributu na `NeverRemove` není zaručeno, že budou vždy zachována mezipaměti. V tématu [další prostředky](#additional-resources) Další informace.

Pomocník značky mezipaměti je závislá na [služby mezipaměti paměti](xref:performance/caching/memory). Pomocník značky mezipaměti přidá službu, pokud nebyl přidán.

## <a name="additional-resources"></a>Další zdroje

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
