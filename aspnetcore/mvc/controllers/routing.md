---
title: "Směrování do akce Kontroleru"
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: 497ce47fa567f163cb7b1eb891408f0100d15b8a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="routing-to-controller-actions"></a>Směrování do akce Kontroleru

Podle [Ryan Nowak](https://github.com/rynowak) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Jádro ASP.NET MVC používá směrování [middleware](../../fundamentals/middleware.md) odpovídající adresy URL příchozích požadavků a jejich namapování na akce. Trasy jsou definovány v spuštění kódu nebo atributy. Postupy popisují, jak cest URL by měla odpovídat akce. Trasy se také používají k vygenerování adres URL (pro odkazy) odeslaná v odpovědi. 

Akce se buď obvykle směrují nebo směrován atribut. Uvedení trasu na kontroler nebo akce umožňuje atribut směrovat. V tématu [ve smíšeném směrování](#routing-mixed-ref-label) Další informace.

Tento dokument vysvětluje interakce mezi MVC a směrování a jak typické zpřístupnění aplikace MVC použití funkce směrování. V tématu [směrování](xref:fundamentals/routing) podrobnosti o pokročilé směrování.

## <a name="setting-up-routing-middleware"></a>Nastavení směrování middlewaru

Ve vaší *konfigurace* metoda mohou se zobrazit podobné kódu:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Uvnitř volání `UseMvc`, `MapRoute` se používá k vytvoření jedné trasy, které budete označujeme jako `default` trasy. Většinu aplikací MVC použije trasu pomocí šablony podobně jako `default` trasy.

Šablona trasy `"{controller=Home}/{action=Index}/{id?}"` může odpovídat cestu adresy URL jako `/Products/Details/5` , který extrahuje hodnoty trasy `{ controller = Products, action = Details, id = 5 }` podle tokenizaci cestu. MVC se pokusí najít řadič s názvem `ProductsController` a spuštěním akce `Details`:

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

Upozorňujeme, že v tomto příkladu by vazby modelu použít hodnotu `id = 5` nastavit `id` parametru `5` při vyvolání této akce. Najdete v článku [vazby modelu](../models/model-binding.md) další podrobnosti.

Pomocí `default` trasy:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Šablona trasy:

* `{controller=Home}`definuje `Home` jako výchozí`controller`

* `{action=Index}`definuje `Index` jako výchozí`action`

* `{id?}`definuje `id` jako volitelná

Výchozí a parametry volitelné trasy nemusí být k dispozici v cestě adresy URL pro shodu. V tématu [odkaz na šablonu trasy](../../fundamentals/routing.md#route-template-reference) podrobný popis syntaxe šablonu trasy.

`"{controller=Home}/{action=Index}/{id?}"`Cesta adresy URL se může shodovat `/` a hodnoty trasy způsobí `{ controller = Home, action = Index }`. Hodnoty pro `controller` a `action` Zkontrolujte použití výchozí hodnoty, `id` neobsahuje hodnotu vzhledem k tomu, že neexistuje žádný odpovídající segment cesty adresy URL. MVC využije tyto hodnoty trasy k vyberte `HomeController` a `Index` akce:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

Pomocí této definici řadiče a šablonu trasy `HomeController.Index` akce by byl proveden v žádném z následující cesty adresy URL:

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

Metoda zvýšení pohodlí `UseMvcWithDefaultRoute`:

```csharp
app.UseMvcWithDefaultRoute();
```

Lze použít k nahrazení:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`UseMvc`a `UseMvcWithDefaultRoute` přidá instanci `RouterMiddleware` k middlewaru v řadě. MVC nebude pracovat přímo s middleware a používá směrování pro zpracování požadavků. MVC je připojený k trasy prostřednictvím instance `MvcRouteHandler`. Kód uvnitř `UseMvc` je podobný následujícímu:

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc`všechny trasy přímo nedefinuje ho přidá do kolekce tras pro zástupný symbol `attribute` trasy. Přetížení `UseMvc(Action<IRouteBuilder>)` můžete přidat vlastní trasy a také podporuje směrováním atributů.  `UseMvc`a jeho změn přidá zástupný symbol pro atribut trasy – atribut směrování je vždy k dispozici bez ohledu na to, jak nakonfigurujete `UseMvc`. `UseMvcWithDefaultRoute`Definuje výchozí trasu a podporuje atribut směrování. [Směrováním atributů](#attribute-routing-ref-label) část obsahuje podrobné informace o směrování atribut.

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a>Konvenční směrování

`default` Trasy:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Příkladem je *konvenční směrování*. Říkáme tento styl *konvenční směrování* protože navazuje *konvence* pro cesty adresy URL:

* první segment cesty se mapuje na název řadiče

* druhý mapuje název akce.

* třetí segmentu se používá pro volitelné `id` slouží k mapování na modelu entity

Pomocí této `default` trasy, cesty URL `/Products/List` se mapuje `ProductsController.List` akce, a `/Blog/Article/17` mapuje `BlogController.Article`. Toto mapování je založena na názvy kontroleru a akce **pouze** a není založena na obory názvů, umístění zdrojových souborů nebo parametry metody.

> [!TIP]
> Použití konvenční směrování s výchozí trasu umožňuje rychle vytvářet aplikace bez nutnosti spolu s novou vzor adresy URL pro každou akci, kterou definujete. Pro aplikace s akcemi CRUD styl s konzistence pro adresy URL napříč řadičů pomůžou zjednodušit kódu a stát předvídatelnější uživatelské rozhraní.

> [!WARNING]
> `id` Je definován jako volitelné šablonou trasy, což znamená, že vaše akce můžete provést bez zadané ID jako část adresy URL. Obvykle co se stane, pokud `id` je vynechán z adresy URL je, že bude nastavena pro `0` vazby modelu a v důsledku toho odpovídajícím databáze bude nalezena žádná entita `id == 0`. Atribut směrování vám může jemně odstupňovanou kontrolu, aby se ID požadované pro některé akce a nikoli pro ostatní uživatele. Podle konvence dokumentace bude obsahovat volitelné parametry jako `id` když jsou pravděpodobně uvedena správné použití.

## <a name="multiple-routes"></a>Víc tras

Můžete přidat víc tras uvnitř `UseMvc` přidáním další volání `MapRoute`. Díky tomu můžete zadat více názvů nebo přidat konvenční trasy, které jsou vyhrazené pro určité akci, jako například:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`blog` Trasa sem *vyhrazené konvenční trasy*, což znamená, že používá konvenční směrování systému, ale je vyhrazen pro určité akce. Vzhledem k tomu `controller` a `action` nezobrazí v šabloně trasy jako parametry, budou mít pouze výchozí hodnoty a proto bude tato trasa vždy mapovat akce `BlogController.Article`.

Trasy do kolekce tras se seřadí a budou zpracovány v pořadí, které jste přidali. Ano v tomto příkladu `blog` vyzkouší se trasy před `default` trasy.

> [!NOTE]
> *Vyhrazené konvenční trasy* často používají parametry trasy catch-all jako `{*article}` k zachycení zbývající část cesty URL. Může být trasu 'příliš typu greedy, což znamená, že se shoduje adresy URL, které byste chtěli odpovídala jiným trasám. Uveďte, typu greedy' trasy později ve směrovací tabulce, chcete-li tento problém vyřešit.

### <a name="fallback"></a>Záložní volba

V rámci zpracování žádosti se MVC ověří, že hodnoty trasy můžete použít k vyhledání kontroleru a akce v aplikaci. Pokud se hodnoty trasy neshodují akce pak není trasy považovány za shodné a vyzkouší se další trasy. To se označuje jako *záložní*, a je určen ke zjednodušení případech, kde konvenční trasy překrývat.

### <a name="disambiguating-actions"></a>Nejednoznačnosti akce

Při dvě akce odpovídat prostřednictvím směrování, musí na zvolte "nejlepší" candidate, jinak se vyvolat výjimku rozlišení MVC. Příklad:

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

Tento řadič definuje dvě akce, které by odpovídalo cesty URL `/Products/Edit/17` a data trasy `{ controller = Products, action = Edit, id = 17 }`. Toto je typický vzor MVC řadiče kde `Edit(int)` zobrazí formulář upravit produktu, a `Edit(int, Product)` zpracovává odeslaného formuláře. Chcete-li to možné, bude muset zvolit MVC `Edit(int, Product)` po požadavku HTTP `POST` a `Edit(int)` když příkaz protokolu HTTP je jakýkoli jiný.

`HttpPostAttribute` ( `[HttpPost]` ) Je implementací `IActionConstraint` se povolit jenom akce, která má být vybrána při HTTP je `POST`. Přítomnost `IActionConstraint` umožňuje `Edit(int, Product)` shodovat lépe než `Edit(int)`, takže `Edit(int, Product)` vyzkouší se napřed.

Je potřeba jenom psaní vlastních `IActionConstraint` implementace v specializované scénáře, ale je důležité si uvědomit, roli atributů, například `HttpPostAttribute` -podobné atributy jsou definovány pro jiné akce HTTP. V konvenční směrování je běžné akce, které používají stejný název akce, pokud jsou součástí `show form -> submit form` pracovního postupu. Výhodou tohoto vzoru se stane po zkontrolování více zřejmá [IActionConstraint Principy](#understanding-iactionconstraint) části.

Pokud odpovídají víc tras a MVC nemůžete najít, nejlepší' směrování, vyvolá výjimku `AmbiguousActionException`.

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a>Názvy tras

Řetězce `"blog"` a `"default"` v následujících příkladech jsou názvy tras:


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Názvy tras pojmenujte trasy logické tak, aby pojmenovanou trasu lze použít pro generování adresy URL. To výrazně zjednodušuje vytvoření adresy URL při řazení tras se ověřte komplikovanější generování adresy URL. Názvy tras musí být jedinečný celou aplikaci.

Názvy tras nemají žádný vliv na adresu URL odpovídající nebo zpracování požadavků; použijí se jenom pro generování adresy URL. [Směrování](xref:fundamentals/routing) obsahuje podrobnější informace o generování adresy URL včetně generování adresy URL v pomocné rutiny specifické pro MVC.

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a>Atribut směrování

Atribut směrování používá sadu atributů k mapování akcí přímo na šablon trasy. V následujícím příkladu `app.UseMvc();` je používán `Configure` je předán metoda a žádná trasa. `HomeController` Bude shodovat s sadu podobná jaké výchozí trasu adresy URL `{controller=Home}/{action=Index}/{id?}` odpovídá:

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

`HomeController.Index()` Akce se provede v žádném z cest URL `/`, `/Home`, nebo `/Home/Index`.

> [!NOTE]
> V tomto příkladu jsou vysvětlené klíče programovací rozdíl mezi směrováním atributů a standardní metody směrování. Atribut směrování vyžaduje další vstup k určení trasu; konvenční výchozí trasu zpracovává více stručně trasy. Ale směrováním atributů umožňuje (a vyžaduje) přesné řízení, které šablon trasy použijí na každou akci.

S atributem směrování názvu kontroleru a akce názvy přehrání **žádné** role, ve kterém je vybraná akce. V tomto příkladu bude odpovídat stejné adresy URL jako v předchozím příkladu.

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> Výše uvedené šablon trasy nemusíte definovat parametry trasy pro `action`, `area`, a `controller`. Tyto parametry trasy ve skutečnosti nejsou povoleny v atribut trasy. Vzhledem k tomu, že šablona trasy je již přidružen akci, by nedávalo smysl analyzovat název akce z adresy URL.

## <a name="attribute-routing-with-httpverb-attributes"></a>Atribut směrování s atributy Http [akce]

Atribut směrování provést také použít `Http[Verb]` atributy, jako `HttpPostAttribute`. Všechny tyto atributy mohou přijímat šablonu trasy. Tento příklad ukazuje dvě akce, které odpovídají stejnou šablonu trasy:

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

Pro cestu adresy URL jako `/products` `ProductsApi.ListProducts` akce se provede, pokud je příkazem HTTP příkaz `GET` a `ProductsApi.CreateProduct` bude proveden, pokud je příkazem HTTP příkaz `POST`. Atribut směrování nejprve odpovídá adrese URL pro sadu šablon trasy definované atributy trasy. Jakmile odpovídá šablonu trasy, `IActionConstraint` omezení se použijí k určení, jaké akce lze provádět.

> [!TIP]
> Při sestavování rozhraní REST API, navíc není obvyklé, že budete chtít použít `[Route(...)]` na metodu akce. Je vhodnější použít další konkrétní `Http*Verb*Attributes` být přesně určit, co vaše rozhraní API podporuje. Očekává se, že klienti rozhraní REST API vědět, co cesty a HTTP se mapují na konkrétní logické operace.

Vzhledem k tomu, že atribut trasy se vztahuje na určité akci, je usnadňuje parametrů požadovaných jako součást definice šablonu trasy. V tomto příkladu `id` je vyžadován jako součást cesty URL.

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

`ProductsApi.GetProduct(int)` Akce se provede pro cestu adresy URL jako `/products/3` , ale ne pro cestu adresy URL jako `/products`. V tématu [směrování](../../fundamentals/routing.md) úplný popis šablon trasy a související možnosti.

## <a name="route-name"></a>Název trasy

Následující kód definuje *název trasy* z `Products_List`:

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

Názvy tras slouží ke generování adresy URL na základě konkrétní trasy. Názvy tras nemají žádný vliv na porovnávání chování směrování adres URL a používají jenom pro generování adresy URL. Názvy tras musí být jedinečný celou aplikaci.

> [!NOTE]
> Rozdíl oproti to běžné *výchozí trasu*, která definuje `id` parametr jako volitelný (`{id?}`). Tato schopnost přesněji určit rozhraní API má výhod, například umožníte `/products` a `/products/5` aby byly odeslány různé akce.

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a>Kombinování trasy

Chcete-li směrováním atributů méně opakují, atributů tras na řadiči spolu se atributů tras na jednotlivé akce. Všechny šablony trasy definované v řadiči se přidá jako předpona k šablon trasy na akce. Umístění atribut trasy na řadiči díky **všechny** akce v kontroleru využívají směrováním atributů.

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

V tomto příkladu cesty URL `/products` může odpovídat `ProductsApi.ListProducts`a cesty URL `/products/5` může odpovídat `ProductsApi.GetProduct(int)`. Obě tyto akce HTTP odpovídá pouze `GET` vzhledem k tomu, že označených pomocí `HttpGetAttribute`.

Směrovat šablony použít akci, která začínají `/` nemáte získat v kombinaci s šablon trasy použijí pro kontroler. Tento příklad odpovídá cesty adresy URL, podobně jako *výchozí trasu*.

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Doesn't combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a>Pořadí trasy atributů

Konvenční trasy, který se spustí v zadaném pořadí, na rozdíl od směrováním atributů vytvoří strom a současně odpovídá všechny trasy. To se chová jako – Pokud položky trasy byly umístěny v ideální řazení; Většina konkrétní trasy mít příležitost se provést před obecnější trasy.

Například trasu typu `blog/search/{topic}` je specifičtější než trasu jako `blog/{*article}`. Logicky mluvení `blog/search/{topic}` trasy ", nejprve ve výchozím nastavení spustí, protože se jedná pouze rozumný řazení. Použití konvenční směrování, vývojář je odpovědná za uvedení do požadovaného pořadí trasy.

Atribut trasy můžete nakonfigurovat pořadí, pomocí `Order` vlastnost všechny atributy rámec poskytovaný trasy. Trasy se zpracovávají podle vzestupném seřadit na `Order` vlastnost. Výchozí pořadí je `0`. Nastavení směrování pomocí `Order = -1` se spustit před spuštěním tras, které není nastavený pořadí. Nastavení směrování pomocí `Order = 1` se spustí po změně pořadí trasy výchozí.

> [!TIP]
> Vyhněte se v závislosti na `Order`. Pokud váš prostor adresy URL vyžaduje, aby explicitní pořadí hodnoty trasy správně, je pravděpodobně matoucí také klientům. Obecně se směrováním atributů vyberte správné směrování na odpovídající adresy URL. Pokud není výchozí pořadí používá ke generování adresy URL pro funguje, pomocí názvu trasy, protože přepsání je většinou jednodušší než použití `Order` vlastnost.

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>Token nahrazení v šablonách tras ([řadič], [akce] [Oblast])

Pro usnadnění práce trasy atributů podporují *token nahrazení* uzavřením token do hranaté závorky (`[`, `]`). Tokeny `[action]`, `[area]`, a `[controller]` nahradí hodnoty názvu akce, názvu oblasti a názvu kontroleru z akce, kde je definován trasy. V tomto příkladu může akce odpovídat cesty adresy URL, jak je popsáno v komentářích:

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

Jako poslední krok sestavování tras atributů dojde k nahrazení tokenu. Výše uvedeném příkladu bude chovají stejně jako následující kód:

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

Trasy atributů může být spojen s dědičnosti. To je zvláště efektivní, v kombinaci s tokenu nahrazení.

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPost("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

Token nahrazení platí i pro názvy trasy definované trasy atributů. `[Route("[controller]/[action]", Name="[controller]_[action]")]`vygeneruje trasy jedinečný název pro každou akci.

Tak, aby odpovídaly oddělovač literálu tokenu nahrazení `[` nebo `]`, vyhnuli opakováním znak (`[[` nebo `]]`).

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a>Víc tras

Atribut směrování podporuje definování víc tras, které dosáhnout stejné akce. Nejběžnější využití tohoto objektu je tak, aby napodoboval chování *výchozí trasu konvenční* jak je znázorněno v následujícím příkladu:

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

Vložení několika atributů tras na řadiči znamená, že každé z nich bude kombinovat k jednotlivým atributům trasy na metody akce.

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

Při několika atributů tras (které implementují `IActionConstraint`) jsou umístěny na akce, poté každý akce omezení spojuje se šablona trasy z atributu, který je definován.

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> Při použití více tras na akce může vypadat výkonné, je lepší jednoduché a dobře definovaný zachovat místo adresy URL vaší aplikace. Akce při použijte víc tras, pouze v případě potřeby, třeba zajistit podporu pro existující klienty.

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>Volitelné parametry atribut trasy, výchozí hodnoty a omezení

Trasy atributů podporují stejnou syntaxi vložených jako konvenční trasy k určení volitelné parametry, výchozí hodnoty a omezení.

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

V tématu [odkaz na šablonu trasy](../../fundamentals/routing.md#route-template-reference) podrobný popis syntaxe šablonu trasy.

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>Vlastní trasy atributů s použitím`IRouteTemplateProvider`

Všechny atributy trasy zadaný v rozhraní framework ( `[Route(...)]`, `[HttpGet(...)]` atd) implementovat `IRouteTemplateProvider` rozhraní. MVC hledá atributy na řadič třídy a metody akce při spuštění a použije parametry, které implementují aplikace `IRouteTemplateProvider` k vytvoření počáteční sadu trasy.

Můžete implementovat `IRouteTemplateProvider` definovat vlastní atributy trasy. Každý `IRouteTemplateProvider` umožňuje definovat jednu trasu pomocí šablony vlastní trasy, řazení a název:

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

Atribut z výše uvedeném příkladu automaticky nastaví `Template` k `"api/[controller]"` při `[MyApiController]` platí.

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>Přizpůsobení trasy atributů pomocí aplikačního modelu

*Aplikačního modelu* je model objektu vytvořeny při spuštění se všemi metadat používané MVC trasy a spouštět vaše akce. *Aplikačního modelu* zahrnuje všechna data shromážděná z atributů tras (prostřednictvím `IRouteTemplateProvider`). Můžete napsat *konvence* k úpravě aplikačního modelu v čase spuštění k přizpůsobení chování směrování. Tato část ukazuje jednoduchý příklad přizpůsobení směrování pomocí aplikačního modelu.

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Ve smíšeném směrování: atribut směrování vs konvenční směrování

Aplikace MVC můžete kombinovat použití konvenční směrování a směrováním atributů. Je typické používat běžné postupy pro řadiče obsluhující stránky HTML pro prohlížeče, a atribut směrování pro řadiče obsluhující rozhraní REST API.

Akce se buď obvykle směrují nebo směrován atribut. Uvedení trasu na kontroler nebo akce umožňuje atribut směrovat. Akce, které definují trasy atributů není dostupný prostřednictvím konvenční trasy a naopak. **Všechny** atribut trasy na řadiči díky všechny akce v kontroleru atributu směrovat.

> [!NOTE]
> Co rozlišuje dva typy systémů směrování je proces, použijí po adresu URL odpovídá šablonu trasy. V konvenční směrování, hodnoty trasy z shody lze vybírat vyhledávací tabulky všechny běžné směrované akcí akce a kontroler. V atributu směrování, každé šablony je již přidružen akci a je potřeba žádné další vyhledávání.

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>Generování adresy URL

Aplikace MVC můžete používat směrování na Funkce generování adresy URL pro generování odkazů URL na akce. Generování adres URL eliminuje hardcoding adresy URL, provádění kódu robustnější a udržovatelný. Tato část se zaměřuje na Funkce generování adresy URL poskytované MVC a pouze popisuje základní informace o tom, jak funguje generování adresy URL. V tématu [směrování](../../fundamentals/routing.md) podrobný popis generování adresy URL.

`IUrlHelper` Rozhraní je základní část infrastruktury mezi MVC a směrování pro generování adresy URL. Zjistíte instanci `IUrlHelper` k dispozici prostřednictvím `Url` vlastnost řadiče, zobrazení a zobrazení součásti.

V tomto příkladu `IUrlHelper` rozhraní se používá prostřednictvím `Controller.Url` vlastnost ke generování adresy URL pro další akci.

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

Pokud aplikace používá konvenční výchozí směrování, hodnota `url` proměnná bude řetězec cesty adresy URL `/UrlGeneration/Destination`. Tato cesta adresy URL je vytvořený směrování kombinací hodnoty trasy z aktuální žádosti (vedlejším hodnoty), s hodnotami předaný `Url.Action` a nahraďte těmito hodnotami do šablonu trasy:

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

Každý parametr trasy v šabloně trasy má svou hodnotu nahrazena odpovídající názvy s hodnotami a vedlejším hodnoty. Parametr trasy, který nemá hodnotu můžete použít výchozí hodnotu, pokud má jednu, nebo vynechána, pokud je volitelné (stejně jako u `id` v tomto příkladu). Generování adresy URL se nezdaří, pokud parametr všechny požadované trasy nemá odpovídající hodnotu. Pokud se nezdaří generování adresy URL pro trasu, zkusí se další trasy, dokud vyzkoušeny všechny trasy nebo je nalezena shoda.

Jako příklad `Url.Action` vyšší předpokládá konvenční směrování, ale adresa URL generování funguje podobně jako se směrováním atributů, i když koncepty se liší. S konvenční směrování, hodnoty trasy slouží k rozbalte šablony a hodnoty trasy pro `controller` a `action` obvykle zobrazují v této šabloně – to funguje, protože dodržovat odpovídala směrování adres URL *konvence*. V atributu směrování, hodnoty trasy pro `controller` a `action` nejsou povoleny se objeví v šabloně – místo toho se použije pro vyhledávání kterou šablonu použít.

Tento příklad používá směrováním atributů:

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC sestavení vyhledávací tabulky všech akcí atribut směrovat a bude odpovídat `controller` a `action` hodnoty a vyberte šablonu trasy sloužící ke generování adresy URL. V ukázce výše `custom/url/to/destination` se vygeneruje.

### <a name="generating-urls-by-action-name"></a>Generování adres URL pomocí názvu akce

`Url.Action` (`IUrlHelper` . `Action`) a všechny související přetížení všechny jsou založeny na tento nápad chcete určit, co odkazujete na zadáním názvu akce a názvu kontroleru.

> [!NOTE]
> Při použití `Url.Action`, aktuální trasy hodnoty pro `controller` a `action` jsou určené pro vás – hodnota `controller` a `action` jsou součástí obě *vedlejším hodnoty* **a** *hodnoty*. Metoda `Url.Action`, vždy používá aktuální hodnoty `action` a `controller` a vygeneruje cestu adresy URL, který směruje na aktuální akce.

Směrování pokusy o použití hodnot v vedlejším hodnoty vyplnit informace, které neposkytli při generování adresy URL. Použití trasy jako `{a}/{b}/{c}/{d}` a vedlejším hodnoty `{ a = Alice, b = Bob, c = Carol, d = David }`, směrování má dostatek informací ke generování adresy URL bez jakékoli další hodnoty – vzhledem k tomu, že všechny trasy parametry mají hodnotu. Pokud jste přidali hodnota `{ d = Donovan }`, hodnota `{ d = David }` by být ignorovány, a vygenerovaný adresa URL by obsahovat `Alice/Bob/Carol/Donovan`.

> [!WARNING]
> Jsou hierarchické cesty adresy URL. V příkladu výše, pokud jste přidali hodnota `{ c = Cheryl }`, obě hodnoty `{ c = Carol, d = David }` by ignorovány. V takovém případě již máme hodnotu `d` a generování adresy URL se nezdaří. Museli byste zadejte požadovanou hodnotu `c` a `d`.  Můžete očekávat, že dosáhl tento problém se výchozí trasa (`{controller}/{action}/{id?}`)- ale zřídka narazíte na toto chování v praxi jako `Url.Action` bude vždy explicitně zadáte `controller` a `action` hodnotu.

Delší přetížení `Url.Action` také provést další *hodnot trasy* objekt, který má jiné než zadejte hodnoty pro parametry trasy `controller` a `action`. Můžete to použít s nejčastěji zobrazí `id` jako `Url.Action("Buy", "Products", new { id = 17 })`. Podle konvence *hodnot trasy* objektu je obvykle objekt anonymního typu, ale také může být `IDictionary<>` nebo *prostý původního objektu .NET*. V řetězci dotazu jsou vloženy hodnoty žádné další trasy, které neodpovídají parametry trasy.

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> Chcete-li vytvořit absolutní adresu URL, použijte přetížení, které přijímá `protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>Generování adres URL pomocí trasy

Výše uvedený kód ukázán generování adresy URL předáním v názvu kontroleru a akce. `IUrlHelper`také poskytuje `Url.RouteUrl` řady metod. Tyto metody jsou podobná `Url.Action`, ale jejich nemáte zkopírovat aktuální hodnoty `action` a `controller` hodnoty trasy. Nejběžnější využití slouží k zadání názvu trasy používat konkrétní trasy pro generování adresy URL, obecně *bez* zadejte název a kontroler nebo akce.

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a>Generování adres URL ve formátu HTML

`IHtmlHelper`poskytuje `HtmlHelper` metody `Html.BeginForm` a `Html.ActionLink` ke generování `<form>` a `<a>` elementy v uvedeném pořadí. Tyto metody používat `Url.Action` metoda ke generování adresy URL a přijímají podobně jako argumenty. `Url.RouteUrl` Companions pro `HtmlHelper` jsou `Html.BeginRouteForm` a `Html.RouteLink` které mají podobné funkce.

TagHelpers vygenerování adres URL pomocí `form` TagHelper a `<a>` TagHelper. Obě tyto použít `IUrlHelper` pro jejich implementaci. V tématu [práci s formuláři](../views/working-with-forms.md) Další informace.

Uvnitř zobrazení `IUrlHelper` je k dispozici prostřednictvím `Url` vlastnost pro generování adresy URL všech ad-hoc nevztahuje výše.

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a>Generování adres URL ve výsledcích akce

Výše uvedených příkladech ukázaly, pomocí `IUrlHelper` v kontroleru, zatímco nejběžnější využití v kontroleru je ke generování adresy URL jako součást výsledek akce.

`ControllerBase` a `Controller` základní třídy zprostředkovávají usnadňující metody pro výsledky akce, které odkazují jiné akce. Jeden typickému využití je přesměrování po přijetí vstup uživatele.

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

Metody vytváření výsledky akce podle podobný Princip metody `IUrlHelper`.

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>Zvláštním případem pro vyhrazené konvenční trasy

Konvenční směrování můžete použít zvláštní druh názvem definice trasy *vyhrazené konvenční trasy*. V následujícím příkladu s názvem trasy `blog` je vyhrazené konvenční trasy.

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Pomocí těchto definicí cesty `Url.Action("Index", "Home")` vygeneruje cesty URL `/` s `default` trasy, ale proč? Hodnoty trasy může uhodnout `{ controller = Home, action = Index }` by být dost pro generování adres URL pomocí `blog`, a výsledkem bude `/blog?action=Index&controller=Home`.

Vyhrazené konvenční trasy spoléhají na zvláštní chování výchozí hodnoty, které nemají odpovídající parametr trasy, která trasy, která brání v dodržení "příliš chamtivého" s generování adresy URL. V takovém případě výchozí hodnoty jsou `{ controller = Blog, action = Article }`a ani `controller` ani `action` se zobrazí jako parametr trasy. Když směrování provede generování adresy URL, zadané hodnoty musí odpovídat výchozí hodnoty. Pomocí generování adresy URL `blog` selže, protože hodnoty `{ controller = Home, action = Index }` neodpovídají `{ controller = Blog, action = Article }`. Směrování pak spadne zpět na akci `default`, což je úspěšné.

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>Oblasti

[Oblasti](areas.md) se o funkci MVC sloužící k organizování související funkce do skupiny jako samostatné směrování – obor názvů (pro akce kontroleru) a strukturu složek (pro zobrazení). Použití oblastí umožňuje aplikaci používat více řadičů se stejným názvem – tak dlouho, dokud mají různé *oblasti*. Použití oblastí vytvoří hierarchii pro účely směrování přidáním jiný parametr trasy, `area` k `controller` a `action`. V této části se popisují, jak směrování komunikuje s oblastí - najdete v části [oblasti](areas.md) podrobnosti o použití oblastí se zobrazeními.

Následující příklad konfiguruje MVC používat výchozí trasu konvenční a *oblasti trasy* pro oblast s názvem `Blog`:

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

Při kontrole shody cesty adresy URL jako `/Manage/Users/AddUser`, první trasy vygeneruje hodnoty trasy `{ area = Blog, controller = Users, action = AddUser }`. `area` Hodnota trasy je produkovaný výchozí hodnotu pro `area`, ve skutečnosti trasy vytvořené `MapAreaRoute` je ekvivalentní k následujícímu:

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute`Vytvoří trasu pomocí výchozí hodnotu a omezení pro `area` pomocí názvu oblasti zadaný v tomto případě `Blog`. Výchozí hodnota zajistí, že trasy vždy vytvoří `{ area = Blog, ... }`, omezení vyžaduje hodnotu `{ area = Blog, ... }` pro generování adresy URL.

> [!TIP]
> Konvenční směrování je závislá na pořadí. Obecně platí tras se oblasti musí být umístěny dříve v tabulce směrování, jako jsou podrobnější než cesty bez oblast.

Pomocí výše uvedeném příkladu, hodnoty trasy odpovídá následující akce:

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

`AreaAttribute` Je co označuje řadič v rámci oblasti, říkáme, zda je tento řadič v `Blog` oblasti. Řadiče bez `[Area]` atribut nejsou členy žádné oblasti a bude **není** shodovat, kdy `area` hodnota trasy zajišťuje směrování. V následujícím příkladu může odpovídat pouze první řadič uvedené hodnoty trasy `{ area = Blog, controller = Users, action = AddUser }`.

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> Obor názvů každého řadiče se zde zobrazí pro úplnost – jinak by se řadiče mít názvy konfliktu a vygenerována chyba kompilátoru. Obory názvů třídy nemají vliv na směrování MVC.

První dva řadiče jsou členy oblasti a odpovídá pouze při jejich název příslušné oblasti zajišťuje `area` směrování hodnotu. Třetí řadič není členem žádné oblasti a může pouze shody, pokud žádná hodnota pro `area` zajišťuje směrování.

> [!NOTE]
> Z hlediska odpovídající *žádná hodnota*, chybí `area` hodnota je stejná jako hodnota `area` měla hodnotu null nebo prázdný řetězec.

Při provádění akce uvnitř oblasti, hodnoty trasy pro `area` budou k dispozici jako *vedlejším hodnota* pro směrování sloužící ke generování adresy URL. To znamená, že ve výchozím nastavení oblasti fungovat *trvalé* pro generování adresy URL, jak je předvedeno podle následující ukázky.

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a>Principy IActionConstraint

> [!NOTE]
> Tato část se přímý informace na interní informace o framework a způsobu, jakým MVC vybírá akci, kterou chcete provést. Typická aplikace nebudete potřebovat vlastní`IActionConstraint`

Pravděpodobně už jste použili `IActionConstraint` i v případě, že nejste obeznámení s rozhraním. `[HttpGet]` Atribut a je to podobné `[Http-VERB]` atributy implementovat třídu `IActionConstraint` za účelem omezení provádění metody akce.

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

Za předpokladu, že výchozí konvenční trasu cesty URL `/Products/Edit` byste mohli vytvořit hodnoty `{ controller = Products, action = Edit }`, který odpovídá **i** z akcí, které jsou tady uvedené. V `IActionConstraint` terminologie by říkáme, že obě tyto akce jsou považovány za kandidáty – obě shodný data trasy.

Když `HttpGetAttribute` provede, se dozvíte, který *Edit()* odpovídá *získat* a není nalezena shoda pro jiné akce HTTP. `Edit(...)` Akce nemá žádné omezení a proto bude shodovat s všechny akce HTTP. Proto za předpokladu, že `POST` – pouze `Edit(...)` odpovídá. Ale pro `GET` obě akce mohou stále odpovídat - však akce s `IActionConstraint` je považováno za *lepší* než akce bez. Proto protože `Edit()` má `[HttpGet]` je považován za zvláštní a bude vybrána, pokud obě akce se může shodovat.

Koncepčně `IActionConstraint` je forma *přetížení*, ale místo přetížení metody se stejným názvem, je přetížení mezi akce, které odpovídají stejnou adresu URL. Atribut směrování také používá `IActionConstraint` a může mít za následek akce z různých řadičů obou za kandidáty.

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a>Implementace IActionConstraint

Nejjednodušší způsob, jak implementovat `IActionConstraint` je vytvoření třídy odvozené od `System.Attribute` a umístěte ji na vaše akce a kontrolery. MVC bude automaticky zjistit některé `IActionConstraint` , se použijí jako atributy. Můžete použít model aplikace použít omezení a jedná se pravděpodobně nejvíce flexibilní přístup, protože ji umožňuje metaprogram jak uplatňují.

V následujícím příkladu omezení vybere akci založenou na *kód země* z dat trasy. [Úplné ukázku na Githubu](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

Jste zodpovědní za implementaci `Accept` metoda a výběr 'Pořadí' pro omezení provést. V takovém případě `Accept` metoda vrátí `true` k označení akce je nalezena shoda při `country` směrovat hodnota odpovídá. To se liší od `RouteValueAttribute` , neboť umožňuje záložní s-s atributy akcí. Ukázka ukazuje, že pokud byste `en-US` akce pak kód země, jako je `fr-FR` přejde zpět do více obecné řadiče, který nemá `[CountrySpecific(...)]` použít.

`Order` Vlastnost rozhoduje, které *fáze* omezení je součástí. Akce omezení spustit ve skupinách, na základě `Order`. Například všechny rozhraní zadané atributy metody HTTP používají stejné `Order` hodnotu tak, aby spouštět ve fázi stejné. Může mít libovolný počet fází, jako je nutné implementovat požadovaných zásad.

> [!TIP]
> Při rozhodování o hodnotu `Order` přemýšlení o tom, zda bude použito vaše omezení před metody HTTP. Nižší čísla se spouští jako první.
