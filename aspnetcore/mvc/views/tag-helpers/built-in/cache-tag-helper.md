---
title: Pomocná rutina značek v ASP.NET Core MVC do mezipaměti
author: pkellner
description: Ukazuje, jak pracovat s pomocná rutina značek v mezipaměti
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 425d8c2235f0070665bc0c967d2498f2cff2a4a6
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755512"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Pomocná rutina značek v ASP.NET Core MVC do mezipaměti

Podle [Peter Kellner](http://peterkellner.net) 

Pomocná rutina značek mezipaměti umožňuje výrazně zlepšit výkon vaší aplikace ASP.NET Core pomocí ukládání do mezipaměti obsah k poskytovateli vnitřní mezipaměti ASP.NET Core.

Nastaví výchozí zobrazovací modul Razor `expires-after` až 20 minut.

Následující kód Razor ukládá datum a čas:

```cshtml
<cache>@DateTime.Now</cache>
```

První požadavek na stránku, která obsahuje `CacheTagHelper` zobrazí aktuální datum a čas. Další požadavky se zobrazí hodnota uložená v mezipaměti, dokud mezipaměti vyprší (výchozí nastavení 20 minut) nebo vyřadí se tlaku na paměť.

Můžete nastavit dobu uložení do mezipaměti s následujícími atributy:

## <a name="cache-tag-helper-attributes"></a>Atributy pomocné rutiny značky do mezipaměti

- - -

### <a name="enabled"></a>Povoleno    


| Typ atributu    | Platné hodnoty      |
|----------------   |----------------   |
| Logická hodnota           | "true" (výchozí)  |
|                   | "false"   |


Určuje, zda se uloží do mezipaměti obsah uzavřená v pomocné rutiny značky mezipaměti. Výchozí hodnota je `true`.  Pokud nastavit `false` této pomocné rutiny značky mezipaměti se neprojeví ukládání do mezipaměti na vykresleného výstupu.

Příklad:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>vyprší dne 

| Typ atributu |           Příklad hodnoty            |
|----------------|------------------------------------|
| DateTimeOffset | "@new DateTime(2025,1,29,17,02,0)" |

Nastaví datum absolutní vypršení platnosti. V následujícím příkladu bude ukládat do mezipaměti obsah pomocné rutiny značky mezipaměti až do 17:02:00 na 29. ledna 2025.

Příklad:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>Po vypršení platnosti

| Typ atributu |        Příklad hodnoty         |
|----------------|------------------------------|
|    Časový interval    | "@TimeSpan.FromSeconds(120)" |

Nastaví dobu od prvního požadavku pro ukládání do mezipaměti obsah. 

Příklad:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>klouzavé vypršení platnosti

| Typ atributu |        Příklad hodnoty        |
|----------------|-----------------------------|
|    Časový interval    | "@TimeSpan.FromSeconds(60)" |

Nastaví čas, který se položka mezipaměti by měla být vyřazena, pokud není přístup.

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

Přijímá hodnotu jedné hlavičce nebo čárkou oddělený seznam hodnot hlavičky, které aktivují aktualizace mezipaměti při změnách. Následující příklad monitoruje hodnota hlavičky `User-Agent`. V příkladu uloží do mezipaměti obsah pro každé jiné `User-Agent` uvedené na webový server.

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
| String            | "Vytvořit"                |
|                   | "Značku, Model" |

Přijímá hodnotu jedné hlavičce nebo čárkou oddělený seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, když se změní hodnota hlavičky. Následující příklad zobrazuje hodnoty `Make` a `Model`.

Příklad:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>se liší podle postupu

| Typ atributu    | Příklad hodnoty                |
|----------------   |----------------               |
| String            | "Vytvořit"                |
|                   | "Značku, Model" |

Přijímá hodnotu jedné hlavičce nebo čárkou oddělený seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, pokud změna hodnoty parametru data trasy. Příklad:

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

Přijímá hodnotu jedné hlavičce nebo čárkou oddělený seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, pokud se změní (s) hodnoty hlavičky. V následujícím příkladu vypadá v souboru cookie přidruženého k ASP.NET Core Identity. Když je ověření uživatele žádosti soubor cookie nastavení aktivuje aktualizace mezipaměti.

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

Určuje, zda mezipaměti by měla resetovat při změně přihlášeného uživatele (nebo objekt kontextu zabezpečení). Aktuální uživatel se také označuje jako instanční objekt kontextu požadavku a lze je zobrazit v zobrazení Razor odkazem `@User.Identity.Name`.

Následující příklad zjistí aktuálně přihlášeného uživatele.  

Příklad:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Pomocí tohoto atributu uchovává obsah v mezipaměti v průběhu cyklu přihlášení a odhlášení.  Při použití `vary-by-user="true"`, přihlášení a odhlášení akce zruší platnost mezipaměti pro ověřeného uživatele.  Mezipaměť je neplatná, protože nová hodnota jedinečný soubor cookie byl vygenerován při přihlášení. Mezipaměť je zachován z důvodu anonymní stavu Pokud je k dispozici žádný soubor cookie nebo vypršela platnost. To znamená, že pokud není přihlášen žádný uživatel, bude udržovat mezipaměti.

- - -

### <a name="vary-by"></a>se liší podle

| Typ atributu | Příklad hodnoty |
|----------------|----------------|
|     String     |    "@Model"    |

Umožňuje přizpůsobení získá jaká data uložená v mezipaměti. Při aktualizaci objektu, odkazuje atribut řetězec hodnotu změny, obsah pomocné rutiny značky mezipaměti. Často zřetězení hodnoty modelu jsou přiřazeny k tomuto atributu.  Efektivně, to znamená, že aktualizace zřetězených hodnot zruší platnost mezipaměti.

V následujícím příkladu se předpokládá metoda kontroleru vykreslení zobrazení součtů celočíselnou hodnotu dva parametry trasy `myParam1` a `myParam2`a vrátí ji jako vlastnost jednoho modelu. Při změně tohoto součtu obsah pomocné rutiny značky mezipaměti je vykreslen a uložili do mezipaměti znovu.  

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

Obsahuje pokyny k vyřazení mezipaměti integrovanou mezipaměť na poskytovatele. Webový server vyřazení `Low` nejprve mezipaměti položky, když je přetížena paměť.

Příklad:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority` Atribut nezaručuje konkrétní úroveň mezipaměti dobu uchování. `CacheItemPriority` je jenom návrh. Nastavení tohoto atributu na `NeverRemove` nezaručuje, že budou vždy uchovávat mezipaměti. Zobrazit [další prostředky](#additional-resources) Další informace.

Pomocná rutina značek mezipaměti je závislá na [služby mezipaměti paměti](xref:performance/caching/memory). Pomocná rutina značek mezipaměti přidá službu, pokud nebyla přidána.

## <a name="additional-resources"></a>Další zdroje

* [Mezipaměť v paměti](xref:performance/caching/memory)
* [Úvod do systému Identity](xref:security/authentication/identity)
