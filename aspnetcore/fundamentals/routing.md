---
title: Směrování v ASP.NET Core
author: ardalis
description: Zjistěte, jak je zodpovědný za mapování příchozího požadavku k obslužné rutině trasy funkci směrování ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/01/2018
uid: fundamentals/routing
ms.openlocfilehash: 96df625113b0c33ee8a9e9bb7dccec9a2c28348a
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090999"
---
# <a name="routing-in-aspnet-core"></a>Směrování v ASP.NET Core

Podle [Ryanem Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), a [Rick Anderson](https://twitter.com/RickAndMSFT)

Funkce směrování je zodpovědná za mapování příchozího požadavku k obslužné rutině trasy. Trasy jsou definovány v aplikaci a nakonfigurovaná při spuštění aplikace. Trasy můžete volitelně extrakci hodnot z adresy URL, obsaženy v požadavku a tyto hodnoty je pak možné pro zpracování požadavku. Pomocí informací o směrování z aplikace, funkci směrování je také možnost k vygenerování adres URL, které se mapují k obslužné rutině trasy. Proto směrování můžete najít obslužná rutina trasy podle adresy URL, nebo najít adresu URL odpovídající obslužná rutina trasy na základě informací o obslužnou rutinu trasy.

> [!IMPORTANT]
> Tento dokument popisuje směrování nízké úrovně ASP.NET Core. Informace o směrování ASP.NET Core MVC najdete v tématu <xref:mvc/controllers/routing>.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>Základní informace o směrování

Směrování používá *trasy* (implementace <xref:Microsoft.AspNetCore.Routing.IRouter>) na:

* Příchozí požadavky na mapování *trasy obslužné rutiny*.
* Generování adresy URL použité v odpovědi.

Obecně platí aplikace má jedné kolekce tras. Když přijde požadavek, kolekce tras se zpracovávají v pořadí. Příchozí žádost vypadá pro trasu, která odpovídá adrese URL požadavku voláním <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> metodu na každý postup k dispozici v kolekci tras. Oproti tomu odpověď může využívat směrování k vygenerování adres URL (například pro přesměrování nebo odkazy) na základě informací o směrování a proto vyhnout se tak nutnosti pevně zakódovat adresy URL, která pomáhá udržovatelnost.

Směrování je připojen k [middleware](xref:fundamentals/middleware/index) profilace podle <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> třídy. [ASP.NET Core MVC](xref:mvc/overview) přidá do kanálu middleware směrování jako součást konfigurace. Další informace o použití směrování jako samostatné komponenty, naleznete v tématu [použití směrování Middleware](#use-routing-middleware) oddílu.

### <a name="url-matching"></a>Adresa URL odpovídající

Adresa URL odpovídající je proces, ve které směrování odešle příchozí požadavek na *obslužná rutina*. Tento proces je na základě dat v cestě adresy URL, ale je možné rozšířit na zvažte všechna data v žádosti. Schopnost expedovat požadavky k oddělení obslužné rutiny je klíčem k škálování velikosti a složitosti aplikace.

Zadejte příchozí požadavky `RouterMiddleware`, který volá <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> metodu na každý postup v pořadí. <xref:Microsoft.AspNetCore.Routing.IRouter> Vybere instanci, jestli se má *zpracování* požadavku tak, že nastavíte [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) na jinou hodnotu null <xref:Microsoft.AspNetCore.Http.RequestDelegate>. Pokud trasa nastaví obslužnou rutinu pro žádosti, směrovat zastaví zpracování a zpracovat požadavek, je vyvolána obslužná rutina. Pokud jsou použity všechny trasy a nenajde žádná obslužná rutina požadavku, middleware volá *Další*, a vyvolat další middleware v kanálu požadavku.

Primární vstup do `RouteAsync` je [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) přidruženého k aktuální žádosti. `RouteContext.Handler` a [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) jsou výstupy nastavit po trasa odpovídá.

Shoda během `RouteAsync` také nastaví vlastnosti `RouteContext.RouteData` na odpovídající hodnoty podle doposud provádí zpracování žádostí. Pokud trasa odpovídá žádosti `RouteContext.RouteData` obsahuje informace o stavu důležité o *výsledek*.

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) je slovník *hodnot trasy* vytvořenými trasy. Tyto hodnoty jsou obvykle určeny tokenizaci adresu URL a je možné přijímat uživatelský vstup nebo další dispatching rozhodnutí v aplikaci.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) je kontejner objektů a dat o další data související s porovnávané trasy. `DataTokens` jsou k dispozici pro podporu přiřazování dat o stavu se každý postup tak, aby aplikace mohly rozhodnutí později založené na směrování, které odpovídá. Tyto hodnoty jsou definovány pro vývojáře a proveďte **není** ovlivňují chování směrování žádným způsobem. Kromě toho hodnoty dočasně uložily v `RouteData.DataTokens` může být libovolného typu, rozdíl od `RouteData.Values`, která musí být snadno převést do a z řetězce.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers*) je seznam tras, které účastnila úspěšně odpovídající požadavek. Trasy můžete vnořit do mezi sebou. `Routers` Vlastnost odpovídá cestě prostřednictvím logického stromu tras, z kterých vzniklo shoda. Obecně platí, první položky v `Routers` je kolekce tras a byste měli použít pro generování adresy URL. Poslední položky v `Routers` je obslužná rutina trasy, který odpovídá.

### <a name="url-generation"></a>Generování adresy URL

Generování adresy URL je proces, podle kterých směrování, můžete vytvořit cestu adresy URL na základě sady hodnot trasy. To umožňuje logické rozdělení mezi vaší obslužné rutiny a adresy URL, které k nim přístup.

Generování adresy URL je iterativní probíhá podobně, ale začíná uživatele nebo architekturní kód volání do <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> metody kolekce tras. Každý *trasy* nemá jeho `GetVirtualPath` metodu volat v pořadí, dokud nebude nenulovým <xref:Microsoft.AspNetCore.Routing.VirtualPathData> je vrácena.

Primární vstupů na `GetVirtualPath` jsou:

* [VirtualPathContext.HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext*)
* [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*)

Trasy primárně používat hodnoty trasy poskytované `Values` a `AmbientValues` rozhodnout, jestli je možné generovat adresu URL a jaké hodnoty je třeba zahrnout. `AmbientValues` Sada hodnot trasy, které byly vytvořeny z odpovídajících aktuální požadavek systému směrování. Naproti tomu `Values` jsou hodnoty trasy, které určují, jak generovat požadovanou adresu URL pro aktuální operaci. `HttpContext` Je k dispozici v případě, že trasy je potřeba získat služby nebo další data související s aktuálním kontextu.

> [!TIP]
> Představte si, že [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) jako přepsání pro sadu [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*). Generování adresy URL se pokusí znovu použít hodnoty trasy z aktuální žádosti k tomu, aby k vygenerování adres URL pro odkazů pomocí stejného postupu nebo hodnoty trasy.

Výstup `GetVirtualPath` je `VirtualPathData`. `VirtualPathData` je paralelní z `RouteData`. `VirtualPathData` obsahuje `VirtualPath` pro výstupní adresy URL a některé další vlastnosti, které by měl nastavit trasy.

[VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) obsahuje vlastnost *virtuální cesta* vytvářených trasy. V závislosti na potřebách budete muset cesta další zpracování. Pokud chcete vykreslovat vygenerovaná adresa URL ve formátu HTML, předřaďte základní cesty aplikace.

[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) je odkaz na trasy, která adresa URL se úspěšně vygeneroval.

[VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) vlastnosti je slovník další data související s trasy, která vygenerovala adresa URL. Toto je rovnoběžky [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).

### <a name="creating-routes"></a>Vytváření tras

Směrování poskytuje <xref:Microsoft.AspNetCore.Routing.Route> třídu jako standardní implementace <xref:Microsoft.AspNetCore.Routing.IRouter>. `Route` používá *šablonu trasy* syntaxe pro definování vzory, které odpovídají vůči cestě adresy URL při <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> je volána. `Route` používá stejnou šablonu trasy pro generování adresy URL při `GetVirtualPath` je volána.

Většina aplikací vytvářet trasy voláním <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> nebo některou z podobných rozšiřující metody definované v <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Všechny tyto metody vytvoření instance <xref:Microsoft.AspNetCore.Routing.Route> a přidejte ho do kolekce tras.

`MapRoute` nepřijímá parametr obslužná rutina trasy. `MapRoute` Přidá trasy, které jsou zpracovávány pouze <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Protože je výchozí obslužnou rutinu `IRouter`, může rozhodnout není pro zpracování požadavku. ASP.NET Core MVC je třeba typicky nakonfigurován jako výchozí popisovač, který zpracovává pouze požadavky, které odpovídají k dispozici kontroleru a akce. Další informace o směrování MVC najdete v tématu <xref:mvc/controllers/routing>.

Následující příklad kódu je příkladem `MapRoute` volání používá typické definice trasy ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Tato šablona odpovídá cesty adresy URL jako `/Products/Details/17` a extrahuje hodnoty trasy `{ controller = Products, action = Details, id = 17 }`. Hodnoty trasy jsou určeny rozdělení cestě adresy URL do segmentů a odpovídající každý segment s *trasy parametr* název v šabloně trasy. Jsou pojmenované parametry trasy. Máte definované uzavřením názvu parametru ve složených závorkách `{ ... }`.

Předchozí šablona může také odpovídat cesty URL `/` a mohlo by dojít hodnoty `{ controller = Home, action = Index }`. K tomu dochází, `{controller}` a `{action}` trasy parametry mají výchozí hodnoty a `id` trasy parametr je nepovinný. Je rovno `=` následovaného hodnotu po definuje výchozí hodnotu pro parametr název parametru trasy. Otazník `?` po název parametru trasy definuje jako volitelný parametr. Parametry s výchozí hodnotou směrování *vždy* vygenerování hodnoty trasy, pokud trasa odpovídá. Volitelné parametry není výsledkem hodnota trasy, pokud se žádný odpovídající segment cesty adresy URL.

Zobrazit [trasy – – referenční informace k šablonám](#route-template-reference) důkladné popis funkcí šablonu trasy a syntaxe.

Tento příklad zahrnuje *trasy omezení*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Tato šablona odpovídá cesty adresy URL jako `/Products/Details/17` , ale ne `/Products/Details/Apples`. Definice parametru trasy `{id:int}` definuje [trasy omezení](#route-constraint-reference) pro `id` parametr trasa. Implementace omezení trasy <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> a hodnoty trasy k ověření je zkontrolovat. V tomto příkladu je hodnota trasy `id` musí být převeditelný na celé číslo. Zobrazit [trasy omezení referenční](#route-constraint-reference) podrobnější vysvětlení omezení trasy, které jsou k dispozici v rámci rozhraní.

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

> [!TIP]
> Vložená syntaxe pro definování omezení a výchozích hodnot může být vhodné pro jednoduché trasy. Existují však funkce, jako jsou tokeny dat, nepodporované vložená syntaxe.

Následující příklad ukazuje několik více scénářů:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Tato šablona odpovídá cesty adresy URL jako `/Blog/All-About-Routing/Introduction` a extrahuje hodnoty `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Směrování výchozí hodnoty pro `controller` a `action` jsou vytvářeny trasy, i když neexistují žádné odpovídající trasy parametry v šabloně. V šabloně postupu lze zadat výchozí hodnoty. `article` Parametr trasa je definován jako *pokrývající vše* podle vzhledu hvězdičku `*` před název parametru trasy. Parametry trasy pokrývající vše zachycení zbývající část cesty URL a můžete také hledat shody prázdný řetězec.

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

### <a name="url-generation"></a>Generování adresy URL

`Route` Třídy můžete také provádět generování adresy URL kombinováním sadu hodnot trasy s jeho šablonu trasy. Toto je logicky procesu zpětné odpovídajících cestě adresy URL.

> [!TIP]
> Abyste lépe pochopili generování adresy URL, představte si, jakou adresu URL, které chcete generovat a potom rozmyslete si, jak šablonu trasy odpovídají tuto adresu URL. Hodnoty by bylo vytvořeno? Jedná se o ekvivalent hrubý fungování generování adresy URL `Route` třídy.

Tento příklad používá základní styl směrování ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

S hodnotami trasy `{ controller = Products, action = List }`, této trasy generuje adresu URL `/Products/List`. Hodnoty trasy jsou substituovány za parametry odpovídající trasy k vytvoření cesty adresy URL. Protože `id` je volitelný parametr trasa, bez obav, že nemá žádnou hodnotu.

S hodnotami trasy `{ controller = Home, action = Index }`, této trasy generuje adresu URL `/`. Hodnoty trasy, které byly poskytnuty odpovídat výchozím hodnotám tak, aby segmenty odpovídající tyto hodnoty lze bezpečně vynechat. Obě adresy URL vygeneruje zpátečního převodu s touto definicí trasy a vytvářejí stejné hodnoty trasy, které byly použity k vytvoření adresy URL.

> [!TIP]
> Používejte aplikace pomocí ASP.NET Core MVC <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> k vygenerování adres URL, bez volání do směrování přímo.

Další informace o generování adresy URL, najdete v části [odkazem generování url](#url-generation-reference).

## <a name="use-routing-middleware"></a>Použití směrování middlewaru

::: moniker range=">= aspnetcore-2.1"

Odkaz [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) v souboru projektu vaší aplikace.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Odkaz [metabalíček Microsoft.aspnetcore.all](xref:fundamentals/metapackage) v souboru projektu vaší aplikace.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Odkaz [Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/) v souboru projektu vaší aplikace.

::: moniker-end

Přidání směrování do kontejneru služby v `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

::: moniker-end

Musí být nakonfigurované trasy v `Startup.Configure` metody. Ukázková aplikace používá tato rozhraní API:

* `RouteBuilder`
* `Build`
* `MapGet` &ndash; Odpovídá pouze požadavky HTTP GET.
* `UseRouter`

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

::: moniker-end

V následující tabulce jsou uvedeny odpovědi s danou identifikátory URI.

| Identifikátor URI                  | Odpověď                                          |
| -------------------- | ------------------------------------------------- |
| /Package/Create/3    | Dobrý den! Hodnoty trasy: [operace, vytvořit], [id, 3] |
| / / 3 balíčku/sledování    | Dobrý den! Hodnoty trasy: [operace, sledovat], [id, -3] |
| / balíček/sledovat/3 /   | Dobrý den! Hodnoty trasy: [operace, sledovat], [id, -3] |
| / Package/sledování /      | &lt;Předáno, žádná shoda&gt;                    |
| ZÍSKAT /hello/Joe       | Dobrý den, Joe!                                          |
| /Hello/Joe příspěvku      | &lt;Předáno, odpovídá pouze HTTP GET&gt;       |
| ZÍSKAT /hello/Joe/Smith | &lt;Předáno, žádná shoda&gt;                    |

Pokud konfigurujete jednu trasu, zavolejte <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> předávajícího `IRouter` instance. Nebudete muset použít <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.

Rozhraní framework obsahuje sadu rozšiřujících metod pro vytváření tras, jako například:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Některé z těchto metod, jako například `MapGet`, vyžadují `RequestDelegate` poskytované. `RequestDelegate` Slouží jako *obslužná rutina trasy* při trasa odpovídá. Jiné metody v této rodině povolit konfiguraci kanálu middleware pro použití jako obslužná rutina trasy. Pokud *mapy* metoda nepřijme obslužnou rutinu, jako například `MapRoute`, potom použije <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.

`Map[Verb]` Metody omezení trasy, která má v názvu metody akce HTTP pomocí omezení. Viz například <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> a <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Odkaz na šablonu trasy

Tokeny ve složených závorkách (`{ ... }`) definovat *trasy parametry* , které jsou vázány, pokud je nalezen odpovídající trasy. Můžete definovat více než jeden parametr trasa v segmentu směrování, ale musí být odděleny literálovou hodnotou. Například `{controller=Home}{action=Index}` není platné trasy, protože neexistuje žádná hodnota literálu mezi `{controller}` a `{action}`. Tyto parametry trasy musí mít název a může zadat další atributy.

Prostý text než parametry trasy (například `{id}`) a oddělovač cesty `/` text v adrese URL se musí shodovat. Odpovídající text je velká a malá písmena a podle dekódovaný reprezentace cesty adresy URL. Tak, aby odpovídaly parametr oddělovače literálu trasy `{` nebo `}`, řídicí znak opakováním (`{{` nebo `}}`).

Vzory adres URL, které se pokusí zaznamenat název souboru s příponou volitelné mají další aspekty. Představte si třeba šablona `files/{filename}.{ext?}`. Pokud obě `filename` a `ext` existuje, naplní se obě hodnoty. Pokud pouze `filename` existuje v adrese URL trasy shody, protože koncové tečky `.` je volitelný. Následující adresy URL, která odpovídá této trasy:

* `/files/myFile.txt`
* `/files/myFile`

Můžete použít `*` znak jako předpona pro parametr trasy, aby vytvořit vazbu na celý URI. Jedná se *pokrývající vše* parametru. Například `blog/{*slug}` odpovídá libovolný identifikátor URI, který začíná `/blog` a využili všechny hodnoty (která je přiřazená `slug` směrovat hodnota). Parametry catch – to všechno můžete také hledat shody prázdný řetězec.

::: moniker range=">= aspnetcore-2.2"

Parametr pokrývající vše řídicí sekvence příslušných znaků při trasy se používá ke generování adresy URL, včetně oddělovač cesty (`/`) znaků. Například trasy `foo/{*path}` trasy hodnotami `{ path = "my/path" }` generuje `foo/my%2Fpath`. Všimněte si uvozený uvozovacím znakem lomítka. Operace round-trip cesta oddělovače, použijte `**` předpona parametru trasy. Trasa `foo/{**path}` s `{ path = "my/path" }` generuje `foo/my/path`.

::: moniker-end

Mohou mít parametry trasy *výchozí hodnoty*, navržené tak, že zadáte výchozí po názvu parametru, které jsou odděleny symbolem rovná se (`=`). Například `{controller=Home}` definuje `Home` jako výchozí hodnota pro `controller`. Výchozí hodnota je použita, pokud je k dispozici v adrese URL pro parametr žádná hodnota. Kromě výchozí hodnoty, parametry trasy, může být volitelná, zadána přidáním otazník (`?`) na konec názvu parametru, jak v `id?`. Rozdíl mezi hodnotami nepovinný a výchozí parametry trasy je, že parametr trasa s výchozí hodnotou vždy vytváří hodnotu; Volitelný parametr má hodnotu pouze v případě, že je žádost o adresu URL zadat hodnotu.

::: moniker range=">= aspnetcore-2.2"

Parametry trasy může mít omezení, které musí odpovídat hodnotě trasy vázán z adresy URL. Přidání dvojtečkou (`:`) a název omezení, určuje název parametru trasy *vložené omezení* na parametru trasy. Pokud omezení vyžaduje argumenty, jsou uzavřeny v závorkách `( )` za název omezení. Několik vložených omezení je možné zadat tak připojení jiného dvojtečka (`:`) a název omezení. Název omezení a argumenty jsou předány <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> služby k vytvoření instance <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> použít při zpracování adresy URL. Vyžaduje-li konstruktoru omezení služby, se už přeložit injektáž závislostí aplikace služby. Například šablona trasy `blog/{article:minlength(10)}` Určuje `minlength` omezení s argumentem `10`. Další informace o omezení trasy a seznam omezení poskytovaného rámcem, najdete v článku [trasy referenční omezení](#route-constraint-reference) části.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Parametry trasy může mít omezení, které musí odpovídat hodnotě trasy vázán z adresy URL. Přidání dvojtečkou (`:`) a název omezení, určuje název parametru trasy *vložené omezení* na parametru trasy. Pokud omezení vyžaduje argumenty, jsou uzavřeny v závorkách `( )` za název omezení. Několik vložených omezení je možné zadat tak připojení jiného dvojtečka (`:`) a název omezení. Název omezení a argumenty jsou předány <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> služby k vytvoření instance <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> použít při zpracování adresy URL. Například šablona trasy `blog/{article:minlength(10)}` Určuje `minlength` omezení s argumentem `10`. Další informace o omezení trasy a seznam omezení poskytovaného rámcem, najdete v článku [trasy referenční omezení](#route-constraint-reference) části.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Parametr transformátory, které transformují parametr na hodnotu při generování odkazů a odpovídající akce a stránky pro identifikátory URI mohou mít i parametry trasy. Jako omezení, parametr transformátory jde přidat vložený parametr trasa přidáním dvojtečkou (`:`) a název transformer za název parametru trasy. Například šablona trasy `blog/{article:slugify}` Určuje `slugify` transformátoru.

::: moniker-end

Následující tabulka ukazuje některé šablony trasy a jejich chování.

| Šablona trasy                         | Příklad porovnávání adresy URL  | Poznámky                                                                  |
| -------------------------------------- | --------------------- | ---------------------------------------------------------------------- |
| Dobrý den                                  | řetězec Ahoj                | Odpovídá jen jednu cestu `/hello`                                  |
| {Stránky = Home}                            | /                     | Odpovídá a nastaví `Page` do `Home`                                      |
| {Stránky = Home}                            | / Kontaktu              | Odpovídá a nastaví `Page` do `Contact`                                   |
| {controller} / {action} / {id}?            | / / Seznam produktů        | Mapuje `Products` kontroleru a `List` akce                       |
| {controller} / {action} / {id}?            | / Produkty/podrobnosti/123 |  Mapuje `Products` kontroleru a `Details` akce.  `id` Nastavte na 123 |
| {controller=Home}/{action=Index}/{id?} | /                     |  Mapuje `Home` kontroleru a `Index` metody. `id` se ignoruje.       |

Pomocí šablony je obecně nejjednodušším přístupem při směrování. Omezení a výchozí hodnoty lze také zadat mimo šablonu trasy.

> [!TIP]
> Povolit [protokolování](xref:fundamentals/logging/index) zobrazíte jak součástí směrování implementací, jako například `Route`, shodovat s požadavky.

## <a name="reserved-routing-names"></a>Vyhrazené názvy směrování

Následující klíčová slova jsou vyhrazené názvy a nelze jej použít jako názvy tras nebo parametry:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Odkaz na omezení trasy

Omezení trasy provést, když `Route` má odpovídající syntaxe adresy URL příchozích a tokenizovaného cestě adresy URL do hodnoty trasy. Omezení trasy obecně zkontrolovat hodnoty trasy spojený přes šablonu trasy a ujistěte se, hodnota Ano/žádné rozhodnutí o, zda se hodnota je přijatelné. Některá omezení trasy použít data mimo hodnota trasy vzít v úvahu, jestli je možné směrovat požadavek. Například <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> můžete přijmout nebo odmítnout žádost založené na jeho příkaz protokolu HTTP.

> [!WARNING]
> Vyhněte se použití omezení pro **ověřování vstupu** protože to znamená neplatný vstup vede *404 - Nenalezeno* odpovědi namísto *400 - Chybný požadavek* s příslušná chybová zpráva. Omezení trasy se používají pro **rozlišení** mezi podobné trasy, není k ověření vstupů pro konkrétní trasy.

Následující tabulka ukazuje některá omezení trasy a jejich očekávané chování.

| omezení | Příklad | Příklad shody | Poznámky |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Odpovídá jakémukoliv celému číslu |
| `bool` | `{active:bool}` | `true`, `FALSE` | Odpovídá `true` nebo `false` (velká a malá písmena) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Odpovídá platný `DateTime` hodnotu (v invariantní jazykové verzi - zobrazí upozornění) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Odpovídá platný `decimal` hodnotu (v invariantní jazykové verzi - zobrazí upozornění) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Odpovídá platný `double` hodnotu (v invariantní jazykové verzi - zobrazí upozornění) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Odpovídá platný `float` hodnotu (v invariantní jazykové verzi - zobrazí upozornění) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Odpovídá platný `Guid` hodnota |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Odpovídá platný `long` hodnota |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Řetězec musí mít aspoň 4 znaky |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Řetězec musí obsahovat více než 8 znaků |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Řetězec musí mít délku přesně 12 znaků. |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Řetězec musí být aspoň 8 a ne více než 16 znaků |
| `min(value)` | `{age:min(18)}` | `19` | Celočíselná hodnota musí být alespoň 18 |
| `max(value)` | `{age:max(120)}` | `91` | Celočíselná hodnota musí být více než 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Celočíselná hodnota musí být alespoň 18, ale ne více než 120 |
| `alpha` | `{name:alpha}` | `Rick` | Řetězec musí obsahovat minimálně jeden abecední znak (`a`-`z`, velká a malá písmena) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Řetězec musí odpovídat regulárnímu výrazu (viz tipy k definování regulárního výrazu) |
| `required` | `{name:required}` | `Rick` |  Používá k vynucení, že parametr-hodnota je k dispozici během generování adresy URL |

Více, oddělit středníkem omezení se můžou uplatnit na jeden parametr. Například následující omezení omezuje parametr na celočíselnou hodnotu 1 nebo vyšší:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Směrovat omezení, která Ověřte adresu URL a jsou převedeny na typ CLR (například `int` nebo `DateTime`) vždy používejte neutrální jazykovou verzi. Tato omezení předpokládá, že adresa URL je nepřekládá. Omezení trasy poskytované rozhraním neupravujte hodnoty uložené v hodnot trasy. Všechny hodnoty trasy, které jsou analyzovány z adresy URL jsou uloženy jako řetězce. Například `float` omezení pokusí převést hodnotu trasy na typ float, ale převedená hodnota se používá jenom k ověření, je možné převést na typ plovoucí.

## <a name="regular-expressions"></a>Regulární výrazy

Přidá rozhraní ASP.NET Core `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` regulárního výrazu konstruktoru. Zobrazit <xref:System.Text.RegularExpressions.RegexOptions> popis těchto členů.

Regulární výrazy pomocí oddělovače a tokeny, které jsou podobné těm, které používají služby Směrování a jazyka C#. Tokeny regulární výraz musí být uvozena. Použít regulární výraz `^\d{3}-\d{2}-\d{4}$` ve směrování, musí mít výraz `\` znaků jako zadané v `\\` v C# zdrojový soubor, který řídicí `\` řídicí znak řetězce (Pokud nepoužíváte [doslovný řetězec literály](/dotnet/csharp/language-reference/keywords/string). `{`, `}`, `[`, A `]` znaků musí být uvozena zdvojeným je řídicí znaky oddělovače parametr směrování. Následující tabulka ukazuje regulárního výrazu a verze uvozený uvozovacím znakem.

| Výraz            | Uvozeny řídicími znaky                        |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Regulární výrazy použité ve směrování často začínat `^` znak (shoda počáteční pozice tohoto řetězce) a končit `$` znak (shoda koncová pozice tohoto řetězce). `^` a `$` znaků Ujistěte se, že hodnota parametru celého postupu shoda s regulárním výrazem. Bez `^` a `$` znaků, regulární výraz odpovídat jakýkoli podřetězec v rámci řetězce, který je často nežádoucí. Následující tabulka obsahuje příklady a vysvětluje, proč odpovídat nebo selhání tak, aby odpovídaly.

| Výraz   | String    | Shoda | Komentář               |
| ------------ | --------- |  ---- |  -------------------- |
| `[a-z]{2}`   | Dobrý den     | Ano   | shody podřetězců     |
| `[a-z]{2}`   | 123abc456 | Ano   | shody podřetězců     |
| `[a-z]{2}`   | mz        | Ano   | odpovídá výrazu    |
| `[a-z]{2}`   | MZ        | Ano   | nerozlišuje velikost písmen    |
| `^[a-z]{2}$` |  Dobrý den    | Ne    | Zobrazit `^` a `$` výše |
| `^[a-z]{2}$` | 123abc456 | Ne    | Zobrazit `^` a `$` výše |

Další informace o syntaxi regulárního výrazu, naleznete v tématu [regulárních výrazech .NET Frameworku](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Chcete-li omezit parametr se známou sadou možných hodnot, použijte regulární výraz. Například `{action:regex(^(list|get|create)$)}` odpovídá pouze `action` trasy hodnotu `list`, `get`, nebo `create`. Pokud předaná do slovníku omezení řetězec `^(list|get|create)$` je ekvivalentní. Omezení, předané ve slovníku omezení (ne vložené v rámci šablony), které neodpovídají jedno z známé omezení jsou také považovány za regulární výrazy.

::: moniker range=">= aspnetcore-2.2"

## <a name="parameter-transformer-reference"></a>Odkaz na parametr transformer

Parametr transformátory:

* Při generování odkazu pro spuštění `Route`.
* Implementace `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Byli nakonfigurováni pomocí <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.
* Přijmout hodnoty trasy a transformovat ho do novou řetězcovou hodnotu.
* Transformovanou hodnotou je používán vytvořený odkaz.

Například vlastní `slugify` transformer parametr vzoru trasy `blog\{article:slugify}` s `Url.Action(new { article = "MyTestArticle" })` generuje `blog\my-test-article`.

Parametr transformátory také používají rozhraní k transformaci identifikátor URI, na který se přeloží koncový bod. Například technologie ASP.NET Core MVC používá parametr transformátory Transformace hodnoty trasy slouží k přiřazení `area`, `controller`, `action`, a `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home:slugify}/{action=Index:slugify}/{id?}");
```

Pomocí předchozího postupu akce `SubscriptionManagementController.GetAll()` je nalezena shoda s identifikátorem URI `/subscription-management/get-all`. Parametr transformer nedojde ke změně hodnoty trasy sloužící ke generování odkazu. `Url.Action("GetAll", "SubscriptionManagement")` Vypíše `/subscription-management/get-all`.

ASP.NET Core nabízí vytváření rozhraní API pro parametr transformátory pomocí generovaného trasy:

* ASP.NET Core MVC má `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` konvence rozhraní API. Tato konvence zadaný parametr transformer platí pro všechny trasy atributů v aplikaci. Parametr transformer transformuje tokeny atribut trasy, jako se nahradí. Další informace najdete v tématu [transformátoru parametr použít k přizpůsobení náhradních tokenů](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Stránky Razor má `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` konvence rozhraní API. Tato konvence zadaný parametr transformer platí pro všechny automaticky zjistí stránky Razor. Parametr transformer transformuje složku a název segmenty souborů tras stránky Razor. Další informace najdete v tématu [transformátoru parametr použít k přizpůsobení stránky trasy](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

::: moniker-end

## <a name="url-generation-reference"></a>Odkaz na generování adresy URL

Následující příklad ukazuje, jak ke generování odkazu pro trasu zadaný slovník hodnot trasy a <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

::: moniker-end

`VirtualPath` Vygeneruje na konci předchozí ukázka je `/package/create/123`. Poskytuje slovníku `operation` a `id` hodnot "Sledovat balíček trasy" šablony trasy `package/{operation}/{id}`. Podrobnosti najdete v tématu ukázkový kód v [použití směrování Middleware](#use-routing-middleware) části nebo [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples).

Druhý parametr `VirtualPathContext` konstruktor je kolekce *okolí hodnoty*. Ambientní hodnoty zajistí pohodlí tím, že omezíte počet hodnot, které vývojář musí zadat v rámci určité kontext požadavku. Aktuální hodnoty trasy z aktuální požadavek jsou považovány za okolí hodnoty pro generování odkazů. V aplikaci ASP.NET Core MVC v `About` akce `HomeController`, není nutné zadat hodnotu trasy kontroleru propojení `Index` akce&mdash;okolí hodnotu `Home` se používá.

Ambientní hodnoty, které neodpovídají parametru jsou ignorovány a okolí hodnoty jsou také ignorovány, pokud explicitně zadaná hodnota je přepsána, bude zleva doprava v adrese URL.

Hodnoty, které jsou explicitně zadat, ale které neodpovídají nic se přidají do řetězce dotazu. V následující tabulce jsou uvedeny výsledek při použití šablonu trasy `{controller}/{action}/{id?}`.

| Ambientní hodnoty                | Explicitní hodnoty                   | Výsledek                  |
| ----------------------------- | --------------------------------- | ----------------------- |
| kontroler = "Domů"             | akce = "O"                    | `/Home/About`           |
| kontroler = "Domů"             | kontroler = "Order", akce = "O" | `/Order/About`          |
| kontroler = "Home", color = "Red" | akce = "O"                    | `/Home/About`           |
| kontroler = "Domů"             | akce = "O", barva = "Red"        | `/Home/About?color=Red` |

Pokud trasa má výchozí hodnotu, která neodpovídá instalovanému parametr a explicitně zadat tuto hodnotu, musí odpovídat výchozí hodnota:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

Generování odkazů pouze vytvoří odkaz pro tuto trasu, pokud jsou k dispozici odpovídající hodnoty pro kontroleru a akce.
