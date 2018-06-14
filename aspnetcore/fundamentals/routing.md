---
title: Směrování v ASP.NET Core
author: ardalis
description: Zjistit, jak je zodpovědná za mapování příchozího požadavku na obslužnou rutinu trasy funkci směrování ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/routing
ms.openlocfilehash: a23e2e1a1dd25a57e5d6189bbd5938c48078515b
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341779"
---
# <a name="routing-in-aspnet-core"></a>Směrování v ASP.NET Core

Podle [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), a [Rick Anderson](https://twitter.com/RickAndMSFT)

Funkci směrování je zodpovědná za mapování příchozího požadavku na obslužnou rutinu trasy. Trasy definované v aplikaci ASP.NET a nakonfiguruje při spuštění aplikace. Trasa může volitelně získat hodnoty z adresy URL obsažené v požadavku a potom tyto hodnoty lze použít pro zpracování požadavku. Pomocí informací trasy z aplikace ASP.NET, funkci směrování je také možné k vygenerování adres URL, které mapují na obslužné rutiny trasy. Proto směrování najdete na základě adresy URL nebo adresa URL odpovídající obslužná rutina trasy na základě informací o směrování obslužné rutiny obslužná rutina trasy.

>[!IMPORTANT]
> Tento dokument popisuje nízké úrovni ASP.NET Core směrování. ASP.NET MVC základní směrování, najdete v části [trasy, která má akce kontroleru](../mvc/controllers/routing.md)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>Směrování – základy

Směrování používá *trasy* (implementace [IRouter](/dotnet/api/microsoft.aspnetcore.routing.irouter)) na:

* mapování příchozích požadavků do *směrovat obslužné rutiny*

* vygenerování adres URL použít v odpovědi

Obecně platí aplikace má jedinou kolekci tras. Pokud dorazí požadavek, je v pořadí zpracování kolekce tras. Příchozí požadavek vypadá pro trasu, která odpovídá adrese URL žádosti voláním `RouteAsync` metodu na každý postup k dispozici v kolekci trasy. Naopak odpověď můžete používat směrování k vygenerování adres URL (například pro přesměrování nebo odkazy) na základě informací o směrování a proto vyhnout se nutnosti pevně adresy URL, která pomáhá udržovatelnosti.

Směrování je připojený k [middleware](xref:fundamentals/middleware/index) kanálu pomocí `RouterMiddleware` třídy. [Jádro ASP.NET MVC](xref:mvc/overview) přidá směrování do middlewaru v řadě jako součást její konfiguraci. Další informace o použití směrování jako samostatná součást najdete v tématu [pomocí směrování middleware](#using-routing-middleware).

<a name="url-matching-ref"></a>

### <a name="url-matching"></a>Adresa URL odpovídající

Adresa URL odpovídající je proces, podle které směrování odešle příchozí požadavek na *obslužná rutina*. Tento proces se obecně na základě dat cesty URL, ale lze rozšířit vzít v úvahu všechna data v požadavku. Možnost Expedovat požadavky k oddělení obslužné rutiny je klíčová pro škálování velikost a složitost aplikace.

Zadejte příchozí požadavky `RouterMiddleware`, který volá `RouteAsync` metodu na každý postup v pořadí. `IRouter` Zvolí instancí ohledně *zpracování* požadavek nastavením `RouteContext.Handler` na hodnotu null `RequestDelegate`. Pokud trasu nastaví obslužnou rutinu pro požadavek, bude volána trasy zpracování zastaví a obslužné rutiny pro zpracování požadavku. Pokud se pokusil všechny trasy a pro požadavek nebyl nalezen žádná obslužná rutina, middleware volá *Další* a je vyvolat další middleware v kanálu požadavku.

Primární vstup `RouteAsync` je `RouteContext.HttpContext` přidružené k aktuální žádosti. `RouteContext.Handler` a `RouteContext.RouteData` jsou výstupů, které budou nastaveny po trasa odpovídá.

Shoda během `RouteAsync` bude také nastavit vlastnosti `RouteContext.RouteData` odpovídající hodnoty, podle dosud provedené zpracování žádostí. Pokud trasa odpovídá žádosti `RouteContext.RouteData` bude obsahovat informace o stavu důležité o *výsledek*.

`RouteData.Values` je slovník *hodnot trasy* vytváří na základě trasy. Tyto hodnoty jsou obvykle určen tokenizaci adresu URL a použít tak, aby přijímal uživatelský vstup, nebo pokud chcete udělat další odesílající rozhodnutích uvnitř aplikace.

`RouteData.DataTokens`  je kontejner objektů a dat o další data související s porovnávané trasy. `DataTokens` jsou k dispozici pro podporu přiřadit stavu, ve kterém data s každou trasy, aby aplikace mohla provést rozhodnutí o později založené na směrování, která odpovídá. Tyto hodnoty jsou definované na vývojáře a proveďte **není** ovlivnit chování směrování žádným způsobem. Kromě toho hodnoty, které jsou schované na okraji v datové tokeny, jež mohou být jakéhokoli typu, na rozdíl od hodnoty trasy, které musí být snadno převést do a z řetězce.

`RouteData.Routers` je seznam tras, které trvalo součástí úspěšně odpovídající požadavku. Trasy lze vnořit do sebe navzájem a `Routers` vlastnost odráží cestu prostřednictvím logického stromu trasy, jejichž výsledkem shody. Obecně první položky v `Routers` je kolekce tras a má být použita pro generování adresy URL. Poslední položky v `Routers` je obslužné rutině trasy, který odpovídá.

### <a name="url-generation"></a>Generování adresy URL

Generování adresy URL je proces, pomocí které směrování, můžete vytvořit cestu adresy URL na základě sady hodnot trasy. To umožňuje logického oddělení mezi vaší obslužné rutiny a v adresách URL, které k nim přístup.

Probíhá podobně iterativní generování adresy URL, ale začíná uživatele nebo framework kód volání do `GetVirtualPath` metoda kolekce tras. Každý *trasy* pak bude mít jeho `GetVirtualPath` metoda volána v pořadí, až jinou hodnotu než null `VirtualPathData` je vrácen.

Primární vstup do `GetVirtualPath` jsou:

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

Trasy především používat hodnoty trasy poskytované `Values` a `AmbientValues` rozhodnout, kdy je možné ke generování adresy URL a jaké hodnoty Pokud chcete zahrnout. `AmbientValues` Jsou sadu hodnot trasy, které byly vytvořeny z odpovídající aktuální požadavek s směrování systému. Naproti tomu `Values` jsou hodnoty trasy, které určují, jak vygenerovat požadovanou adresu URL pro aktuální operaci. `HttpContext` Je k dispozici v případě, že trasu potřeba získat služeb nebo další data související s aktuálním kontextu.

Tip: Zamyslet nad `Values` jako sadu přepsání `AmbientValues`. Generování adresy URL se pokusí znovu použít hodnoty trasy z aktuální žádosti, abyste usnadnili snadnou k vygenerování adres URL pro odkazy pomocí stejného postupu nebo hodnoty trasy.

Výstup `GetVirtualPath` je `VirtualPathData`. `VirtualPathData` je paralelní z `RouteData`; obsahuje `VirtualPath` pro adresu URL výstup a některé další vlastnosti, které by měla být nastavena trasy.

`VirtualPathData.VirtualPath` Vlastnost obsahuje *virtuální cestu* vyprodukované trasy. Podle potřeby můžete zpracovat další cestu. Například pokud chcete vykreslit vygenerovaná adresa URL ve formátu HTML budete muset předřazení základní cesta aplikace.

`VirtualPathData.Router` Je odkaz na trasy, která se úspěšně vygeneroval. adresa URL.

`VirtualPathData.DataTokens` Vlastnosti je slovník další data související s trasy, která generuje adresu URL. Toto je paralelní z `RouteData.DataTokens`.

### <a name="creating-routes"></a>Vytváření tras

Směrování poskytuje `Route` třída jako standardní implementace `IRouter`. `Route` používá *šablonu trasy* syntaxe k definování vzorů, které bude odpovídat proti cestu adresy URL při `RouteAsync` je volána. `Route` použije stejnou šablonu trasy k vygenerování adresy URL při `GetVirtualPath` je volána.

Většina aplikací bude vytvářet trasy voláním `MapRoute` nebo jeden z podobné rozšiřující metody definované na `IRouteBuilder`. Všechny tyto metody se vytvořit instanci `Route` a přidat jej do kolekce tras.

Poznámka: `MapRoute` neberou parametr obslužná rutina trasy – přidá jenom trasy, která bude zpracovávat `DefaultHandler`. Vzhledem k tomu, že je výchozí obslužnou rutinu `IRouter`, mohou rozhodnout není pro zpracování požadavku. ASP.NET MVC je například typicky nakonfigurován jako výchozí obslužnou rutinu, která zpracovává jenom požadavky které odpovídají k dispozici kontroleru a akce. Další informace o směrování do MVC naleznete v tématu [trasy, která má akce kontroleru](../mvc/controllers/routing.md).

Toto je příklad `MapRoute` volání používané typické definice trasy ASP.NET MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Tato šablona bude shodovat s cestu adresy URL jako `/Products/Details/17` a hodnoty trasy pro extrahování `{ controller = Products, action = Details, id = 17 }`. Hodnoty trasy jsou určeny rozdělení do segmentů cesty URL a odpovídající každý segment se *směrovat parametr* název v šabloně trasy. Jsou pojmenované parametry trasy. Jste definované uzavřením název parametru do složených závorek `{ }`.

Výše uvedené šablony může také odpovídat cesty URL `/` a vytvoří hodnoty `{ controller = Home, action = Index }`. K tomu dojde, protože `{controller}` a `{action}` parametry trasy mají výchozí hodnoty a `id` je volitelný parametr trasy. Je rovno `=` přihlašovací následuje hodnotu po parametr název trasy, která definuje výchozí hodnotu pro parametr. Otazník `?` po parametr název trasy, která definuje jako volitelný parametr. Parametry s hodnotou výchozí směrování *vždy* vytvoření hodnoty trasy, pokud trasa odpovídá – volitelné parametry nebude vytvoření hodnoty trasy, pokud se žádné odpovídající segment cesty adresy URL.

V tématu [odkaz šablonu trasy](#route-template-reference) důkladné popis funkcí šablonu trasy a syntaxe.

Tento příklad obsahuje *směrovat omezení*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Tato šablona bude shodovat s cestu adresy URL jako `/Products/Details/17`, ale ne `/Products/Details/Apples`. Definici parametru trasy `{id:int}` definuje *směrovat omezení* pro `id` parametru trasy. Implementace omezení trasy `IRouteConstraint` a hodnoty trasy k ověření je zkontrolovat. V tomto příkladu hodnota trasy `id` se musí dát převést na celé číslo. V tématu [odkaz omezení trasy](#route-constraint-reference) podrobnější vysvětlení omezení trasy, které jsou poskytovány rozhraní.

Další přetížení `MapRoute` přijmout hodnoty pro `constraints`, `dataTokens`, a `defaults`. Tyto další parametry `MapRoute` jsou definovány jako typ `object`. Typické použití těchto parametrů je předat anonymně typu objektu, kde názvy vlastností anonymního typu odpovídal směrovat názvy parametrů.

Následující dva příklady vytvořit ekvivalentní trasy:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Tip: Vložené syntaxi pro definování omezení a výchozích hodnot může být vhodnější pro jednoduché trasy. Existují však funkce, jako je tokeny dat, které nepodporuje vložené syntaxe.

Tento příklad ukazuje několik další funkce:

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

Tato šablona bude shodovat s cestu adresy URL jako `/Blog/All-About-Routing/Introduction` , který extrahuje hodnoty `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Výchozí trasu hodnoty pro `controller` a `action` vznikají pomocí funkcí trasy, i když neexistují žádné odpovídající parametry trasy v šabloně. Výchozí hodnoty lze zadat v šabloně trasy. `article` Parametr trasy je definován jako *catch-all* podle vzhled hvězdičku `*` před název parametru trasy. Parametry trasy catch-all zaznamenat zbývající část cesty URL a také můžete přiřadit prázdný řetězec.

Tento příklad přidá tokeny omezení a data trasy:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Tato šablona bude shodovat s cestu adresy URL jako `/Products/5` , který extrahuje hodnoty `{ controller = Products, action = Details, id = 5 }` a tokeny dat `{ locale = en-US }`.

![Místní hodnoty – tokenů systému Windows](routing/_static/tokens.png)

<a name="id1"></a>

### <a name="url-generation"></a>Generování adresy URL

`Route` Třídy můžete také provádět generování adresy URL pomocí kombinace sady hodnot trasy s jeho šablonu trasy. Toto je logicky procesu zpětné odpovídajících cesty URL.

Tip: Abyste lépe pochopili generování adresy URL, představte si, jaké adresy URL, kterou chcete vygenerovat a pak myslíte o tom, jak šablonu trasy odpovídá tuto adresu URL. Co hodnoty by vytvořeného? Jedná se o ekvivalent hrubý toho, jak funguje generování adresy URL `Route` třídy.

Tento příklad používá základní trasy styl rozhraní ASP.NET MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

S hodnotami trasy `{ controller = Products, action = List }`, bude tato trasa generování adresy URL `/Products/List`. Hodnoty trasy jsou nahrazena pro odpovídající parametry trasy k vytvoření cesty URL. Protože `id` je volitelný parametr trasy, je žádný problém, že nemá žádnou hodnotu.

S hodnotami trasy `{ controller = Home, action = Index }`, bude tato trasa generování adresy URL `/`. Hodnoty trasy, které byly poskytnuty shodovala výchozí hodnoty, takže segmenty odpovídající tyto hodnoty lze bezpečně vynechat. Upozorňujeme, že obě adresy URL generované by odezvy s tuto definici trasy a vytvořit stejné hodnoty trasy, které jste použili k vytvoření adresy URL.

Tip: By měla použít aplikace pomocí ASP.NET MVC `UrlHelper` k vygenerování adres URL namísto volání do směrování přímo.

Další podrobnosti o procesu generování adresy URL najdete v tématu [odkaz generování adresu url](#url-generation-reference).

## <a name="using-routing-middleware"></a>Pomocí směrování middlewaru

Přidejte balíček NuGet "Microsoft.AspNetCore.Routing".

Přidání směrování do kontejneru služby v *Startup.cs*:

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

Musí být nakonfigurované trasy v `Configure` metoda v `Startup` třídy. Následující ukázka používá tato rozhraní API:

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

Následující tabulka obsahuje odpovědi s danou identifikátory URI.

| Identifikátor URI | Odpověď  |
| ------- | -------- |
| /Package/Create/3  | Dobrý den! Hodnoty trasy: [operaci vytvoření], [id, 3] |
| / balíček nebo sledovat/3  | Dobrý den! Hodnoty trasy: [operace, sledovat], [id, -3] |
| / balíček/sledovat/3 nebo | Dobrý den! Hodnoty trasy: [operace, sledovat], [id, -3]  |
| /Package/sledovat / | \<Přejít, nebyla zjištěna shoda > |
| ZÍSKAT /hello/Joe | Dobrý den, Jan! |
| POST /hello/Joe | \<Přejít, odpovídá jenom metody GET protokolu HTTP > |
| ZÍSKAT /hello/Joe/Smith | \<Přejít, nebyla zjištěna shoda > |

Pokud konfigurujete jednu trasu, zavolejte `app.UseRouter` předávání v `IRouter` instance. Nebudete muset volat `RouteBuilder`.

Rozhraní framework poskytuje sadu rozšiřující metody pro vytváření tras, jako:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Některé z těchto metod, jako `MapGet` vyžadují `RequestDelegate` , musíte zadat. `RequestDelegate` Se použije jako *obslužná rutina trasy* Pokud trasa odpovídá. Jiné metody v této rodině povolit konfiguraci middlewaru kanálu, který bude použit jako obslužné rutině trasy. Pokud *mapy* metoda není přijímat obslužnou rutinu, jako například `MapRoute`, pak se bude používat `DefaultHandler`.

`Map[Verb]` Metody použití omezení k omezení trasy, která má v názvu metody akce HTTP. Například v tématu [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) a [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).

## <a name="route-template-reference"></a>Odkaz na šablonu trasy

Tokeny do složených závorek (`{ }`) definovat *směrovat parametry* který bude vázán, pokud je nalezena shoda trasy. Můžete definovat více než jeden parametr trasy v segmentu trasy, ale musí být odděleny literálovou hodnotou. Například `{controller=Home}{action=Index}` nebude platnou trasu, protože není hodnota literálu mezi `{controller}` a `{action}`. Tyto parametry trasy musí mít název, a může zadat další atributy.

Literál text než parametry trasy (například `{id}`) a oddělovač cesty `/` text v adrese URL se musí shodovat. Odpovídající text je velká a malá písmena a podle dekódované reprezentace cesty adresy URL. Tak, aby odpovídaly oddělovač parametr literálu trasy `{` nebo `}`, vyhnuli opakováním znak (`{{` nebo `}}`).

Vzory adres URL, které se pokoušejí o zachycení název souboru s příponou volitelný soubor mít další aspekty. Například pomocí šablony `files/{filename}.{ext?}` – když oba `filename` a `ext` neexistuje, vyplní obě hodnoty. Pokud jenom `filename` existuje v adrese URL odpovídá trasy, protože koncové tečky `.` je volitelný. Následující adresy URL odpovídá této trase:

* `/files/myFile.txt`
* `/files/myFile`

Můžete použít `*` znak jako předpona k parametr trasy pro vazbu s ostatními identifikátor URI – tento postup se nazývá *catch-all* parametr. Například `blog/{*slug}` odpovídá žádné identifikátor URI, který je spuštěn s `/blog` a měl žádné hodnoty (které bude přiřazeno k `slug` směrování hodnotu). Catch – všechny parametry můžete také odpovídaly prázdný řetězec.

Může mít parametry trasy *výchozí hodnoty*, navržené tak, že zadáte výchozí po názvu parametru, oddělených `=`. Například `{controller=Home}` nadefinovat `Home` jako výchozí hodnota pro `controller`. Výchozí hodnota je použita, pokud je přítomen v adrese URL pro parametr žádná hodnota. Kromě výchozí hodnoty, může být volitelné parametry trasy (zadaný připojením `?` na konec názvu parametru, jako v `id?`). Rozdíl mezi volitelné a "má výchozí" je, že parametr trasy výchozí hodnotu vždy vrátí hodnotu; Volitelný parametr obsahuje hodnotu, pouze pokud je k dispozici.

Parametry trasy může mít také omezení, které se musí shodovat s hodnotou trasy vázaný z adresy URL. Přidání dvojtečkou `:` a název omezení po Určuje název parametru trasy *vložené omezení* v parametru trasy. Pokud omezení vyžaduje argumenty těch, které jsou k dispozici uzavřený v závorkách `( )` po název omezení. Několik vložených omezení lze zadat připojením jiné dvojtečkou `:` a název omezení. Předaný název omezení `IInlineConstraintResolver` služby k vytvoření instance `IRouteConstraint` použít při zpracování adresy URL. Například šablona trasy `blog/{article:minlength(10)}` Určuje `minlength` omezení s argumentem `10`. Další omezení trasy popis a seznam omezení poskytované rozhraní najdete v tématu [odkaz omezení trasy](#route-constraint-reference).

Následující tabulka ukazuje některé šablony trasy a jejich chování.

| Šablona trasy | Příklad odpovídající adresy URL | Poznámky |
| -------- | -------- | ------- |
| Dobrý den  | řetězec Ahoj  | Odpovídá jen jednu cestu `/hello` |
| {Stránky = Domů} | / | Odpovídá a nastaví `Page` na `Home` |
| {Stránky = Domů}  | / Kontakt  | Odpovídá a nastaví `Page` na `Contact` |
| {controller} / {action} / {id}? | / / Seznam produktů | Se mapuje na `Products` řadiče a `List` akce |
| {controller} / {action} / {id}? | / Produkty/podrobnosti/123  |  Se mapuje na `Products` řadiče a `Details` akce.  `id` Nastavte na 123 |
| {controller=Home}/{action=Index}/{id?} | /  |  Se mapuje na `Home` řadiče a `Index` metoda; `id` je ignorována. |

Pomocí šablony je obecně nejjednodušším přístupem při směrování. Omezení a výchozí nastavení můžete také uvést mimo šablonu trasy.

Tip: Povolit [protokolování](xref:fundamentals/logging/index) zobrazíte jak součástí směrování implementace, jako například `Route`, shodovat s požadavky.

## <a name="route-constraint-reference"></a>Odkaz na omezení trasy

Omezení trasy při spuštění `Route` má shodná syntaxe adresy URL příchozích a tokenizovaného cestu adresy URL do hodnoty trasy. Omezení trasy obecně zkontrolovat hodnota trasy přidružené prostřednictvím šablonu trasy a proveďte jednoduchou Ano/ne rozhodnutí o tom, zda je přijatelnou hodnotu. Některá omezení trasy pomocí dat mimo hodnota trasy zvažte, zda lze ho směrovat žádosti. Například `HttpMethodRouteConstraint` můžete přijmout nebo odmítnout žádost o založené na jeho příkaz HTTP.

>[!WARNING]
> Vyhněte se použití omezení pro **ověřování vstupu**, protože to znamená, že neplatný vstup bude mít za následek 404 (není nalezena) namísto 400 se příslušná chybová zpráva. Omezení trasy by měla být slouží jako **rozlišení** mezi podobné trasy Neověřovat vstupy pro daný postup.

Následující tabulka ukazuje některé omezení trasy a jejich očekávané chování.

| omezení | Příklad | Příklad odpovídá | Poznámky |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Odpovídá jakémukoliv celému číslu |
| `bool`  | `{active:bool}` | `true`, `FALSE` | Odpovídá `true` nebo `false` (velká a malá písmena) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Odpovídá platná `DateTime` hodnotu (v neutrální jazykovou verzi - zobrazí upozornění) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Odpovídá platná `decimal` hodnotu (v neutrální jazykovou verzi - zobrazí upozornění) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | Odpovídá platná `double` hodnotu (v neutrální jazykovou verzi - zobrazí upozornění) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | Odpovídá platná `float` hodnotu (v neutrální jazykovou verzi - zobrazí upozornění) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Odpovídá platná `Guid` hodnota |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Odpovídá platná `long` hodnota |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Řetězec musí mít délku minimálně 4 znaky |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Řetězec musí obsahovat více než 8 znaků |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Řetězec musí mít přesně 12 znaků. |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Řetězec musí mít alespoň 8 a ne víc než 16 znaků. |
| `min(value)` | `{age:min(18)}` | `19` | Celočíselná hodnota musí být alespoň 18 |
| `max(value)` | `{age:max(120)}` |  `91` | Celočíselná hodnota musí být delší než 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Celočíselná hodnota musí být alespoň 18, ale ne víc než 120 |
| `alpha` | `{name:alpha}` | `Rick` | Řetězec musí obsahovat jeden nebo více abecedních znaků (`a`-`z`, velká a malá písmena) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Řetězec musí odpovídat regulárnímu výrazu (viz tipy k definování regulární výraz) |
| `required`  | `{name:required}` | `Rick` |  Slouží k vynucení, že hodnota bez parametru je dostupný při generování adresy URL |

>[!WARNING]
> Omezení trasy, které Ověřte adresu URL můžete převést na typ CLR (například `int` nebo `DateTime`) vždy používat neutrální jazykovou verzi – se předpokládá, že adresa URL je nepřekládá. Omezení trasy zadaný framework neupravujte hodnotami uloženými v hodnoty trasy. Všechny hodnoty trasy, které jsou analyzovány z adresy URL se uloží jako řetězce. Například [omezení trasy Float](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) se pokusí převést hodnotu trasy na typ float, ale převedenou hodnotu slouží pouze k ověření, je možné ji převést na typ float.

## <a name="regular-expressions"></a>Regulární výrazy 

Přidá rozhraní ASP.NET Core `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` konstruktoru regulární výraz. V tématu [výčtu RegexOptions](/dotnet/api/system.text.regularexpressions.regexoptions) popis těchto členů.

Regulární výrazy použít oddělovače a tokeny, které jsou podobné těm, které jsou používané směrování a jazyka C#. Regulární výraz tokeny, je nutné uvést. Například použijte regulární výraz `^\d{3}-\d{2}-\d{4}$` ve směrování, musí mít `\` znaků zadali jako `\\` v C# zdrojový soubor, který vyhnuli `\` řídicí znak řetězce (Pokud používáte [typu verbatim textové literály](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/string). `{` , `}` , ' [' A ']' znaky musí být uvozena předchozího je řídicí znaky oddělovač parametr směrování.  Následující tabulka uvádí regulární výraz a uvozený verze.

| Výraz               | Poznámka |
| ----------------- | ------------ | 
| `^\d{3}-\d{2}-\d{4}$` | Regulární výraz |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | Uvozený  |
| `^[a-z]{2}$` | Regulární výraz |
| `^[[a-z]]{{2}}$` | Uvozený  |

Regulární výrazy použité ve směrování se často začínat `^` znak (odpovídají počáteční pozice řetězce) a končit `$` znak (odpovídají koncová pozice řetězce). `^` a `$` znaků zajistěte, aby hodnota parametru trasy celý shoda s regulárním výrazem. Bez `^` a `$` znaků s regulárním výrazem se shoda s libovolným řetězcem dílčí v rámci řetězce, který je často nechcete. Následující tabulka obsahuje některé příklady a vysvětluje, proč odpovídat nebo selhání tak, aby odpovídaly.

| Výraz               | String | Shoda | Komentář |
| ----------------- | ------------ |  ------------ |  ------------ | 
| `[a-z]{2}` | Dobrý den | Ano | shody podřetězců |
| `[a-z]{2}` | 123abc456 | Ano | shody podřetězců |
| `[a-z]{2}` | mz | Ano | odpovídá výrazu |
| `[a-z]{2}` | MZ | Ano | není malá a velká písmena |
| `^[a-z]{2}$` |  Dobrý den | Ne | v tématu `^` a `$` výše |
| `^[a-z]{2}$` |  123abc456 | Ne | v tématu `^` a `$` výše |

Odkazovat na [regulární výrazy rozhraní .NET Framework](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) Další informace o syntaxi regulárního výrazu.

Chcete-li omezit parametr, který se známou sadou možných hodnot, použijte regulární výraz. Například `{action:regex(^(list|get|create)$)}` odpovídá jen `action` směrování hodnotu `list`, `get`, nebo `create`. Pokud předaný do slovníku omezení řetězec "^ (seznamu | get | vytvořit) $" by ekvivalentní. Omezení, která se předávají ve slovníku omezení (ne vložené v šabloně na), které si odpovídají jedné ze známé omezení jsou také považovány za regulární výrazy.

## <a name="url-generation-reference"></a>Odkaz na generování adresy URL

Následující příklad ukazuje, jak pro vygenerování odkazu trasu zadaný slovník hodnot trasy a `RouteCollection`.

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

`VirtualPath` Vygeneruje na konci ukázkové výše je `/package/create/123`.

Druhý parametr `VirtualPathContext` konstruktor je kolekce *vedlejším hodnoty*. Vedlejším hodnoty zajistí pohodlí omezením počet hodnot, které vývojář musí zadat v rámci určitých kontext požadavku. Aktuální hodnoty trasy aktuální požadavek jsou považovány za vedlejším hodnoty pro generování odkazů. Například v aplikaci ASP.NET MVC, pokud jste v `About` akce `HomeController`, nepotřebujete k určení hodnoty trasy řadiče propojení `Index` akce (vedlejším hodnotu `Home` se použije).

Vedlejším hodnoty, které neodpovídají parametr ignorovány a vedlejším hodnoty v potaz při hodnotou explicitně poskytuje přepsání, budete zleva doprava v adrese URL.

Hodnoty explicitně nezadá, ale které se neshodují. nic se přidají do řetězce dotazu. V následující tabulce jsou uvedeny výsledek při použití šablonu trasy `{controller}/{action}/{id?}`.

| Vedlejším hodnoty | Explicitní hodnoty | Výsledek |
| -------------   | -------------- | ------ |
| Řadič = "Domů" | akce = "O" | `/Home/About` |
| Řadič = "Domů" | Řadič = "Order", akce = "O" | `/Order/About` |
| Řadič = "Domů", color = "Red" | akce = "O" | `/Home/About` |
| Řadič = "Domů" | akce = "O", barva = "Red" | `/Home/About?color=Red`

Pokud trasa má výchozí hodnotu, která neodpovídá parametr a explicitně zadat tuto hodnotu, musí se shodovat výchozí hodnota. Příklad:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

Generování odkazů by pouze vygenerování odkazu pro tuto trasu, pokud jsou k dispozici odpovídající hodnoty pro kontroleru a akce.
