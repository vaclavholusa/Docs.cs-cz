---
title: Směrování v ASP.NET Core
author: ardalis
description: Zjistěte, jak je zodpovědný za mapování příchozího požadavku k obslužné rutině trasy funkci směrování ASP.NET Core.
ms.author: riande
ms.date: 07/25/2018
uid: fundamentals/routing
ms.openlocfilehash: 19265ac4d19915462c50628061600b1fde04694b
ms.sourcegitcommit: c8e62aa766641aa55105f7db79cdf2b27a6e5977
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39254880"
---
# <a name="routing-in-aspnet-core"></a>Směrování v ASP.NET Core

Podle [Ryanem Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), a [Rick Anderson](https://twitter.com/RickAndMSFT)

Funkce směrování je zodpovědná za mapování příchozího požadavku k obslužné rutině trasy. Trasy jsou definovány v aplikaci ASP.NET a nakonfigurovaná při spuštění aplikace. Trasy můžete volitelně extrakci hodnot z adresy URL, obsaženy v požadavku a tyto hodnoty je pak možné pro zpracování požadavku. Pomocí informací o směrování z aplikace technologie ASP.NET, funkci směrování je také možnost k vygenerování adres URL, které se mapují k obslužné rutině trasy. Proto směrování najdete obslužná rutina trasy na základě adresy URL nebo adresa URL odpovídající obslužná rutina trasy na základě informací o obslužnou rutinu trasy.

>[!IMPORTANT]
> Tento dokument popisuje nízké úrovně ASP.NET Core směrování. ASP.NET Core MVC směrovací, naleznete v tématu [trasy na akce kontroleru](../mvc/controllers/routing.md)

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>Základní informace o směrování

Směrování používá *trasy* (implementace [IRouter](/dotnet/api/microsoft.aspnetcore.routing.irouter)) na:

* příchozí požadavky na mapování *trasy obslužné rutiny*

* Generování adresy URL použité v odpovědi

Obecně platí aplikace má jedné kolekce tras. Když přijde požadavek, kolekce tras se zpracovávají v pořadí. Příchozí žádost vypadá pro trasu, která odpovídá adrese URL požadavku voláním `RouteAsync` metodu na každý postup k dispozici v kolekci tras. Naopak odpověď pomocí směrování k vygenerování adres URL (například pro přesměrování nebo odkazy) na základě informací o směrování a proto vyhnout se tak nutnosti pevně zakódovat adresy URL, která pomáhá udržovatelnost.

Směrování je připojen k [middleware](xref:fundamentals/middleware/index) profilace podle `RouterMiddleware` třídy. [ASP.NET Core MVC](xref:mvc/overview) přidá do kanálu middleware směrování jako součást konfigurace. Další informace o použití směrování jako samostatné komponenty, naleznete v tématu [pomocí směrování middleware](#using-routing-middleware).

<a name="url-matching-ref"></a>

### <a name="url-matching"></a>Adresa URL odpovídající

Adresa URL odpovídající je proces, ve které směrování odešle příchozí požadavek na *obslužná rutina*. Tento proces je obvykle na základě dat v cestě adresy URL, ale je možné rozšířit na zvažte všechna data v žádosti. Schopnost expedovat požadavky k oddělení obslužné rutiny je klíčem k škálování velikosti a složitosti aplikace.

Zadejte příchozí požadavky `RouterMiddleware`, který volá `RouteAsync` metodu na každý postup v pořadí. `IRouter` Vybere instanci, jestli se má *zpracování* požadavku tak, že nastavíte `RouteContext.Handler` na jinou hodnotu null `RequestDelegate`. Pokud trasa nastaví obslužnou rutinu pro žádosti, zpracování zastaví a obslužná rutina trasy se vyvolá pro zpracování žádosti. Pokud jsou použity všechny trasy a nenajde žádná obslužná rutina požadavku, middleware volá *Další* a vyvolat další middleware v kanálu požadavku.

Primární vstup do `RouteAsync` je `RouteContext.HttpContext` přidruženého k aktuální žádosti. `RouteContext.Handler` a `RouteContext.RouteData` jsou výstupy, které se nastaví po trasa odpovídá.

Shoda během `RouteAsync` bude také nastavit vlastnosti `RouteContext.RouteData` na odpovídající hodnoty podle zpracování žádostí, dosud provedli. Pokud trasa odpovídá požadavku, `RouteContext.RouteData` bude obsahovat informace o stavu důležité informace o *výsledek*.

`RouteData.Values` je slovník *hodnot trasy* vytvořenými trasy. Tyto hodnoty jsou obvykle určeny tokenizaci adresu URL a je možné přijímat uživatelský vstup, nebo další dispatching rozhodnutí v aplikaci.

`RouteData.DataTokens`  je kontejner objektů a dat o další data související s porovnávané trasy. `DataTokens` jsou k dispozici pro podporu přiřazování stavu, ve kterém data s každou trasu, aby aplikace mohly rozhodnutí později založené na směrování, která odpovídá. Tyto hodnoty jsou definovány pro vývojáře a proveďte **není** ovlivňují chování směrování žádným způsobem. Kromě toho hodnoty dočasně uložily v datové tokeny, jež může být libovolného typu, na rozdíl od hodnoty trasy, které musí být snadno převést do a z řetězce.

`RouteData.Routers` je seznam tras, které účastnila úspěšně odpovídající požadavek. Trasy můžete vnořit do jiného a `Routers` vlastnost odpovídá cestě prostřednictvím logického stromu tras, z kterých vzniklo shoda. Obecně první položky v `Routers` je kolekce tras. proto byste měli použít pro generování adresy URL. Poslední položky v `Routers` je obslužná rutina trasy, který odpovídá.

### <a name="url-generation"></a>Generování adresy URL

Generování adresy URL je proces, podle kterých směrování, můžete vytvořit cestu adresy URL na základě sady hodnot trasy. To umožňuje logické rozdělení mezi vaší obslužné rutiny a adresy URL, které k nim přístup.

Generování adresy URL následuje podobné iterativní proces, ale začíná uživatele nebo architekturní kód volání do `GetVirtualPath` metody kolekce tras. Každý *trasy* pak bude mít jeho `GetVirtualPath` metodu volat v pořadí, dokud nebude nenulovým `VirtualPathData` je vrácena.

Primární vstupů na `GetVirtualPath` jsou:

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

Trasy primárně používat hodnoty trasy poskytované `Values` a `AmbientValues` rozhodnout, kde je možné generovat adresu URL a jaké hodnoty je třeba zahrnout. `AmbientValues` Sada hodnot trasy, které byly vytvořeny z odpovídajících aktuální požadavek systému směrování. Naproti tomu `Values` jsou hodnoty trasy, které určují, jak generovat požadovanou adresu URL pro aktuální operaci. `HttpContext` Je k dispozici v případě, že trasy je potřeba získat služby nebo další data související s aktuálním kontextu.

Tip: Představit `Values` jako přepsání pro sadu `AmbientValues`. Generování adresy URL se pokusí znovu použít hodnoty trasy z aktuální žádosti k tomu, aby k vygenerování adres URL pro odkazů pomocí stejného postupu nebo hodnoty trasy.

Výstup `GetVirtualPath` je `VirtualPathData`. `VirtualPathData` je paralelní z `RouteData`; obsahuje `VirtualPath` pro výstupní adresy URL a některé další vlastnosti, které by měl nastavit trasy.

`VirtualPathData.VirtualPath` Obsahuje vlastnost *virtuální cesta* vytvářených trasy. V závislosti na potřebách budete muset cesta další zpracování. Například pokud chcete vykreslit vygenerovaná adresa URL ve formátu HTML budete muset předřaďte základní cesty aplikace.

`VirtualPathData.Router` Je odkaz na trasy, která adresa URL se úspěšně vygeneroval.

`VirtualPathData.DataTokens` Vlastnosti je slovník další data související s trasy, která vygenerovala adresa URL. Toto je rovnoběžky `RouteData.DataTokens`.

### <a name="creating-routes"></a>Vytváření tras

Směrování poskytuje `Route` třídu jako standardní implementace `IRouter`. `Route` používá *šablonu trasy* syntaxe pro definování vzory, které bude hledána shoda s cestě adresy URL při `RouteAsync` je volána. `Route` bude používat stejnou šablonu trasy pro generování adresy URL při `GetVirtualPath` je volána.

Většina aplikací bude vytvářet trasy voláním `MapRoute` nebo některou z podobných rozšiřující metody definované v `IRouteBuilder`. Všechny tyto metody vytvoří instanci `Route` a přidejte ho do kolekce tras.

Poznámka: `MapRoute` nepřijímá parametr obslužná rutina trasy – přidá pouze trasy, které bude zpracován adresou `DefaultHandler`. Protože je výchozí obslužnou rutinu `IRouter`, může rozhodnout není pro zpracování požadavku. ASP.NET MVC je třeba typicky nakonfigurován jako výchozí popisovač, který zpracovává pouze požadavky, které odpovídají k dispozici kontroleru a akce. Další informace o směrování MVC najdete v tématu [trasy na akce kontroleru](../mvc/controllers/routing.md).

Toto je příklad `MapRoute` volání používá typické definice trasy ASP.NET MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Tato šablona bude odpovídat cesty adresy URL jako `/Products/Details/17` a extrahovat hodnoty trasy `{ controller = Products, action = Details, id = 17 }`. Hodnoty trasy jsou určeny rozdělení cestě adresy URL do segmentů a odpovídající každý segment s *trasy parametr* název v šabloně trasy. Jsou pojmenované parametry trasy. Máte definované uzavřením názvu parametru ve složených závorkách `{ }`.

Výše uvedené šablony může také odpovídat cesty URL `/` a mohlo by dojít hodnoty `{ controller = Home, action = Index }`. K tomu dojde, protože `{controller}` a `{action}` trasy parametry mají výchozí hodnoty a `id` trasy parametr je nepovinný. Je rovno `=` následovaného hodnotu po definuje výchozí hodnotu pro parametr název parametru trasy. Otazník `?` po název parametru trasy definuje jako volitelný parametr. Parametry s výchozí hodnotou směrování *vždy* vygenerování hodnoty trasy, pokud trasa odpovídá – volitelné parametry nevytvoří hodnotu trasy, pokud se žádný odpovídající segment cesty adresy URL.

Zobrazit [trasy – – referenční informace k šablonám](#route-template-reference) důkladné popis funkcí šablonu trasy a syntaxe.

Tento příklad zahrnuje *trasy omezení*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Tato šablona bude odpovídat cesty adresy URL jako `/Products/Details/17`, ale ne `/Products/Details/Apples`. Definice parametru trasy `{id:int}` definuje *trasy omezení* pro `id` parametr trasa. Implementace omezení trasy `IRouteConstraint` a hodnoty trasy k ověření je zkontrolovat. V tomto příkladu je hodnota trasy `id` musí být převeditelný na celé číslo. Zobrazit [trasy omezení referenční](#route-constraint-reference) podrobnější vysvětlení omezení trasy, které jsou k dispozici v rámci rozhraní.

Další přetížení `MapRoute` přijmout hodnoty pro `constraints`, `dataTokens`, a `defaults`. Z těchto dalších parametrech `MapRoute` jsou definovány jako typ `object`. Typické použití těchto parametrů je předat anonymně typovaných objektů, kde názvy vlastností anonymních typů shody směrovat názvy parametrů.

Následující dva příklady vytvoření ekvivalentní tras:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Tip: Vložená syntaxe pro definování omezení a výchozích hodnot může být vhodnější pro jednoduché trasy. Existují však funkce, jako jsou tokeny dat, které nejsou podporovány vložená syntaxe.

Tento příklad ukazuje několik další funkce:

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

Tato šablona bude odpovídat cesty adresy URL jako `/Blog/All-About-Routing/Introduction` extrahuje hodnoty `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Směrování výchozí hodnoty pro `controller` a `action` jsou vytvářeny trasy, i když neexistují žádné odpovídající trasy parametry v šabloně. V šabloně postupu lze zadat výchozí hodnoty. `article` Parametr trasa je definován jako *pokrývající vše* podle vzhledu hvězdičku `*` před název parametru trasy. Parametry trasy pokrývající vše zachycení zbývající část cesty URL a můžete také hledat shody prázdný řetězec.

V tomto příkladu přidá tokeny omezení a data trasy:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Tato šablona odpovídá cesty adresy URL jako `/en-US/Products/5` a extrahuje hodnoty `{ controller = Products, action = Details, id = 5 }` a tokeny dat `{ locale = en-US }`.

![Tokeny Windows místních hodnot](routing/_static/tokens.png)

<a name="id1"></a>

### <a name="url-generation"></a>Generování adresy URL

`Route` Třídy můžete také provádět generování adresy URL kombinováním sadu hodnot trasy s jeho šablonu trasy. Toto je logicky procesu zpětné odpovídajících cestě adresy URL.

Tip: Abyste lépe pochopili generování adresy URL, představte si, jakou adresu URL, které chcete generovat a potom rozmyslete si, jak šablonu trasy odpovídají tuto adresu URL. Hodnoty by bylo vytvořeno? Jedná se o ekvivalent hrubý fungování generování adresy URL `Route` třídy.

Tento příklad používá základní styl směrování ASP.NET MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

S hodnotami trasy `{ controller = Products, action = List }`, bude tato trasa generování adresy URL `/Products/List`. Hodnoty trasy jsou substituovány za parametry odpovídající trasy k vytvoření cesty adresy URL. Protože `id` je volitelný parametr trasa, bez obav, že nemá žádnou hodnotu.

S hodnotami trasy `{ controller = Home, action = Index }`, bude tato trasa generování adresy URL `/`. Hodnoty trasy, které byly poskytnuty odpovídat výchozím hodnotám, takže segmenty odpovídající tyto hodnoty lze bezpečně vynechat. Mějte na paměti, že obě adresy URL generovány by přenosu s touto definicí trasy a vytvářejí stejné hodnoty trasy, které byly použity k vytvoření adresy URL.

Tip: Používejte aplikace pomocí ASP.NET MVC `UrlHelper` k vygenerování adres URL, bez volání do směrování přímo.

Další podrobnosti o procesu generování adresy URL najdete v tématu [odkazem generování url](#url-generation-reference).

## <a name="using-routing-middleware"></a>Použití směrování middlewaru

Přidání balíčku NuGet "Microsoft.AspNetCore.Routing".

Přidání směrování do kontejneru služby v *Startup.cs*:

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

Musí být nakonfigurované trasy v `Configure` metodu `Startup` třídy. Následující příklad používá tato rozhraní API:

* `RouteBuilder`
* `Build`
* `MapGet`  Odpovídá pouze požadavky HTTP GET
* `UseRouter`

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
{
    var trackPackageRouteHandler = new RouteHandler(context =>
    {
        var routeValues = context.GetRouteData().Values;
        return context.Response.WriteAsync(
            $"Hello! Route values: {string.Join(", ", routeValues)}");
    });

    var routeBuilder = new RouteBuilder(app, trackPackageRouteHandler);

    routeBuilder.MapRoute(
        "Track Package Route",
        "package/{operation:regex(^(track|create|detonate)$)}/{id:int}");

    routeBuilder.MapGet("hello/{name}", context =>
    {
        var name = context.GetRouteValue("name");
        // This is the route handler when HTTP GET "hello/<anything>"  matches
        // To match HTTP GET "hello/<anything>/<anything>,
        // use routeBuilder.MapGet("hello/{*name}"
        return context.Response.WriteAsync($"Hi, {name}!");
    });

    var routes = routeBuilder.Build();
    app.UseRouter(routes);
}
```

Následující tabulka ukazuje odpovědi s danou identifikátorů URI.

| Identifikátor URI | Odpověď  |
| ------- | -------- |
| /Package/Create/3  | Dobrý den! Hodnoty trasy: [operace, vytvořit], [id, 3] |
| / / 3 balíčku/sledování  | Dobrý den! Hodnoty trasy: [operace, sledovat], [id, -3] |
| / balíček/sledovat/3 / | Dobrý den! Hodnoty trasy: [operace, sledovat], [id, -3]  |
| / Package/sledování / | \<Předáno, žádná shoda > |
| ZÍSKAT /hello/Joe | Dobrý den, Joe! |
| /Hello/Joe příspěvku | \<Předáno, odpovídá pouze GET protokolu HTTP > |
| ZÍSKAT /hello/Joe/Smith | \<Předáno, žádná shoda > |

Pokud konfigurujete jednu trasu, zavolejte `app.UseRouter` předávajícího `IRouter` instance. Nebudete muset volat `RouteBuilder`.

Rozhraní framework obsahuje sadu rozšiřujících metod pro vytváření tras, jako například:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Některé z těchto metod, jako `MapGet` vyžadují `RequestDelegate` poskytované. `RequestDelegate` Se použije jako *obslužná rutina trasy* při trasa odpovídá. Jiné metody v této rodině povolit konfigurace kanálu middlewaru, který se použije jako obslužná rutina trasy. Pokud *mapy* metoda nepřijme obslužnou rutinu, jako například `MapRoute`, pak bude používat `DefaultHandler`.

`Map[Verb]` Metody omezení trasy, která má v názvu metody akce HTTP pomocí omezení. Viz například [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) a [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).

## <a name="route-template-reference"></a>Odkaz na šablonu trasy

Tokeny ve složených závorkách (`{ }`) definovat *trasy parametry* který bude vázán, pokud je nalezen odpovídající trasy. Můžete definovat více než jeden parametr trasa v segmentu směrování, ale musí být odděleny literálovou hodnotou. Například `{controller=Home}{action=Index}` nebude platné trasy, protože neexistuje žádná hodnota literálu mezi `{controller}` a `{action}`. Tyto parametry trasy musí mít název, a může zadat další atributy.

Prostý text než parametry trasy (například `{id}`) a oddělovač cesty `/` text v adrese URL se musí shodovat. Odpovídající text je velká a malá písmena a podle dekódovaný reprezentace cesty adresy URL. Tak, aby odpovídaly parametr oddělovače literálu trasy `{` nebo `}`, řídicí znak opakováním (`{{` nebo `}}`).

Vzory adres URL, které se pokusí zaznamenat název souboru s příponou volitelné mají další aspekty. Například pomocí šablony `files/{filename}.{ext?}` – když oba `filename` a `ext` existuje, naplní se obě hodnoty. Pokud pouze `filename` existuje v adrese URL trasy shody, protože koncové tečky `.` je volitelný. Následující adresy URL odpovídají této trasy:

* `/files/myFile.txt`
* `/files/myFile`

Můžete použít `*` znak jako předpona pro parametr trasy, aby vytvořit vazbu na celý URI – tento postup se nazývá *pokrývající vše* parametru. Například `blog/{*slug}` odpovídají libovolný identifikátor URI, který můžete začít `/blog` a využili všechny hodnoty (by byli přiřazeni k `slug` trasy hodnota). Parametry catch – to všechno můžete také hledat shody prázdný řetězec.

Mohou mít parametry trasy *výchozí hodnoty*, navržené tak, že zadáte výchozí po názvu parametru, oddělené `=`. Například `{controller=Home}` byste definovali `Home` jako výchozí hodnota pro `controller`. Výchozí hodnota je použita, pokud je k dispozici v adrese URL pro parametr žádná hodnota. Kromě výchozích hodnot může být volitelné parametry trasy (zadaný připojením `?` na konec názvu parametru, jak v `id?`). Rozdíl mezi volitelná a "má výchozí" je, že parametr trasa s výchozí hodnotou vždy vytváří hodnotu; Volitelný parametr má hodnotu, pouze pokud je k dispozici.

Parametry trasy také může mít omezení, které musí odpovídat hodnotě trasy vázán z adresy URL. Přidání dvojtečkou `:` a název omezení, určuje název parametru trasy *vložené omezení* na parametru trasy. Pokud omezení vyžaduje argumenty. ty jsou k dispozici uzavřený do závorek `( )` za název omezení. Několik vložených omezení je možné zadat tak připojení jiného dvojtečka `:` a název omezení. Název omezení je předán `IInlineConstraintResolver` služby k vytvoření instance `IRouteConstraint` použít při zpracování adresy URL. Například šablona trasy `blog/{article:minlength(10)}` Určuje, `minlength` omezení s argumentem `10`. Další omezení trasy popis a seznam omezení poskytovaného rámcem najdete v tématu [trasy omezení referenční](#route-constraint-reference).

Následující tabulka ukazuje některé šablony trasy a jejich chování.

| Šablona trasy | Příklad porovnávání adresy URL | Poznámky |
| -------- | -------- | ------- |
| Dobrý den  | řetězec Ahoj  | Odpovídá jen jednu cestu `/hello` |
| {Stránky = Home} | / | Odpovídá a nastaví `Page` do `Home` |
| {Stránky = Home}  | / Kontaktu  | Odpovídá a nastaví `Page` do `Contact` |
| {controller} / {action} / {id}? | / / Seznam produktů | Mapuje `Products` kontroleru a `List` akce |
| {controller} / {action} / {id}? | / Produkty/podrobnosti/123  |  Mapuje `Products` kontroleru a `Details` akce.  `id` Nastavte na 123 |
| {controller=Home}/{action=Index}/{id?} | /  |  Mapuje `Home` kontroleru a `Index` metody. `id` se ignoruje. |

Pomocí šablony je obecně nejjednodušším přístupem při směrování. Omezení a výchozí hodnoty lze také zadat mimo šablonu trasy.

Tip: Povolit [protokolování](xref:fundamentals/logging/index) zobrazíte jak součástí směrování implementací, jako například `Route`, shodovat s požadavky.

## <a name="reserved-routing-names"></a>Vyhrazené názvy směrování

Následující klíčová slova jsou vyhrazené názvy a nelze jej použít jako názvy tras nebo parametry:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Odkaz na omezení trasy

Omezení trasy provést, když `Route` má odpovídající syntaxe adresy URL příchozích a tokenizovaného cestě adresy URL do hodnoty trasy. Omezení trasy obecně zkontrolovat hodnoty trasy spojený přes šablonu trasy a provést jednoduchý Ano/ne rozhodnutí o tom, jestli je přijatelné hodnoty. Některá omezení trasy použít data mimo hodnota trasy vzít v úvahu, jestli je možné směrovat požadavek. Například `HttpMethodRouteConstraint` můžete přijmout nebo odmítnout žádost založené na jeho příkaz protokolu HTTP.

>[!WARNING]
> Vyhněte se použití omezení pro **ověřování vstupu**, protože to znamená neplatný vstup bude výsledkem chyba 404 (Nenalezeno) namísto 400 s příslušnou chybovou zprávu. By měl být použito pro omezení trasy **rozlišení** mezi podobné trasy, není k ověření vstupů pro konkrétní trasy.

Následující tabulka ukazuje některá omezení trasy a jejich očekávané chování.

| omezení | Příklad | Příklad shody | Poznámky |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Odpovídá jakémukoliv celému číslu |
| `bool`  | `{active:bool}` | `true`, `FALSE` | Odpovídá `true` nebo `false` (velká a malá písmena) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Odpovídá platný `DateTime` hodnotu (v invariantní jazykové verzi - zobrazí upozornění) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Odpovídá platný `decimal` hodnotu (v invariantní jazykové verzi - zobrazí upozornění) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | Odpovídá platný `double` hodnotu (v invariantní jazykové verzi - zobrazí upozornění) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | Odpovídá platný `float` hodnotu (v invariantní jazykové verzi - zobrazí upozornění) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Odpovídá platný `Guid` hodnota |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Odpovídá platný `long` hodnota |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Řetězec musí mít aspoň 4 znaky |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Řetězec musí obsahovat více než 8 znaků |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Řetězec musí mít délku přesně 12 znaků. |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Řetězec musí být aspoň 8 a ne více než 16 znaků |
| `min(value)` | `{age:min(18)}` | `19` | Celočíselná hodnota musí být alespoň 18 |
| `max(value)` | `{age:max(120)}` |  `91` | Celočíselná hodnota musí být více než 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Celočíselná hodnota musí být alespoň 18, ale ne více než 120 |
| `alpha` | `{name:alpha}` | `Rick` | Řetězec musí obsahovat minimálně jeden abecední znak (`a`-`z`, velká a malá písmena) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Řetězec musí odpovídat regulárnímu výrazu (viz tipy k definování regulárního výrazu) |
| `required`  | `{name:required}` | `Rick` |  Používá k vynucení, že parametr-hodnota je k dispozici během generování adresy URL |

Více, oddělit středníkem omezení se můžou uplatnit na jeden parametr. Například následující omezení omezuje parametr na celočíselnou hodnotu 1 nebo vyšší:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

>[!WARNING]
> Omezení trasy, které ověřují adresu lze převést na typ CLR (například `int` nebo `DateTime`) vždy používá invariantní jazyková verze – předpokládají adresa URL je nepřekládá. Omezení trasy poskytované rozhraním neupravujte hodnoty uložené v hodnot trasy. Všechny hodnoty trasy, které jsou analyzovány z adresy URL se uloží jako řetězce. Například [omezení trasy Float](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) se pokusí převést hodnotu trasy na typ float, ale převedená hodnota se používá jenom k ověření, jej můžete převést na typ float.

## <a name="regular-expressions"></a>Regulární výrazy

Přidá rozhraní ASP.NET Core `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` regulárního výrazu konstruktoru. Zobrazit [výčet RegexOptions](/dotnet/api/system.text.regularexpressions.regexoptions) popis těchto členů.

Regulární výrazy pomocí oddělovače a tokeny, které jsou podobné těm, které používají služby Směrování a jazyka C#. Tokeny regulární výraz musí být uvozena. Například použijte regulární výraz `^\d{3}-\d{2}-\d{4}$` ve směrování, musí mít `\` znaky v jako `\\` v C# zdrojový soubor, který řídicí `\` řídicí znak řetězce (Pokud nepoužíváte [verbatim textové literály](/dotnet/csharp/language-reference/keywords/string). `{` , `}` , ' [' A ']' znaky musí být uvozena zdvojeným je řídicí znaky oddělovače parametr směrování.  Následující tabulka ukazuje regulárního výrazu a verze uvozený uvozovacím znakem.

| Výraz               | Poznámka |
| ----------------- | ------------ |
| `^\d{3}-\d{2}-\d{4}$` | Regulární výraz |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | Uvozeny řídicími znaky  |
| `^[a-z]{2}$` | Regulární výraz |
| `^[[a-z]]{{2}}$` | Uvozeny řídicími znaky  |

Regulární výrazy použité ve směrování se často začínat `^` znak (shoda počáteční pozice tohoto řetězce) a končit `$` znak (shoda koncová pozice tohoto řetězce). `^` a `$` znaků Ujistěte se, že hodnota parametru celého postupu shoda s regulárním výrazem. Bez `^` a `$` znaky regulárního výrazu bude odpovídat jakýkoli podřetězec v rámci řetězce, což se často stává, není to, co chcete. Následující tabulka obsahuje příklady a vysvětluje, proč odpovídat nebo selhání tak, aby odpovídaly.

| Výraz               | String | Shoda | Komentář |
| ----------------- | ------------ |  ------------ |  ------------ |
| `[a-z]{2}` | Dobrý den | Ano | shody podřetězců |
| `[a-z]{2}` | 123abc456 | Ano | shody podřetězců |
| `[a-z]{2}` | mz | Ano | odpovídá výrazu |
| `[a-z]{2}` | MZ | Ano | nerozlišuje velikost písmen |
| `^[a-z]{2}$` |  Dobrý den | Ne | Zobrazit `^` a `$` výše |
| `^[a-z]{2}$` |  123abc456 | Ne | Zobrazit `^` a `$` výše |

Odkazovat na [regulárních výrazech .NET Frameworku](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) Další informace o syntaxi regulárního výrazu.

Chcete-li omezit parametr se známou sadou možných hodnot, použijte regulární výraz. Například `{action:regex(^(list|get|create)$)}` odpovídá pouze `action` trasy hodnotu `list`, `get`, nebo `create`. Pokud předaná do slovníku omezení řetězec "^ (seznam | get | vytváření) $" ekvivalentní. Omezení, předané ve slovníku omezení (ne vložené v rámci šablony), které neodpovídají jedno z známé omezení jsou také považovány za regulární výrazy.

## <a name="url-generation-reference"></a>Odkaz na generování adresy URL

Následující příklad ukazuje, jak ke generování odkazu pro trasu zadaný slovník hodnot trasy a `RouteCollection`.

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

`VirtualPath` Vygeneruje na konci příkladu výše je `/package/create/123`.

Druhý parametr `VirtualPathContext` konstruktor je kolekce *okolí hodnoty*. Ambientní hodnoty zajistí pohodlí tím, že omezíte počet hodnot, které vývojář musí zadat v rámci určité kontext požadavku. Aktuální hodnoty trasy z aktuální požadavek jsou považovány za okolí hodnoty pro generování odkazů. Například v aplikaci ASP.NET MVC, pokud jste v `About` akce `HomeController`, není nutné zadat hodnotu trasy kontroleru propojení `Index` akce (okolí hodnotu `Home` se použije).

Ambientní hodnoty, které neodpovídají parametru jsou ignorovány a okolí hodnoty jsou také ignorovány, pokud explicitně poskytnutou hodnotu přepsání, bude zleva doprava v adrese URL.

Hodnoty, které jsou explicitně zadat, ale které neodpovídají nic se přidají do řetězce dotazu. V následující tabulce jsou uvedeny výsledek při použití šablonu trasy `{controller}/{action}/{id?}`.

| Ambientní hodnoty | Explicitní hodnoty | Výsledek |
| -------------   | -------------- | ------ |
| kontroler = "Domů" | akce = "O" | `/Home/About` |
| kontroler = "Domů" | kontroler = "Order", akce = "O" | `/Order/About` |
| kontroler = "Home", color = "Red" | akce = "O" | `/Home/About` |
| kontroler = "Domů" | akce = "O", barva = "Red" | `/Home/About?color=Red`

Pokud trasa má výchozí hodnotu, která neodpovídá instalovanému parametr a explicitně zadat tuto hodnotu, musí odpovídat výchozí hodnotu. Příklad:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

Generování odkazů by pouze vygenerování odkazu pro tuto trasu, pokud jsou k dispozici odpovídající hodnoty pro kontroleru a akce.
