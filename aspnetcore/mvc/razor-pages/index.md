---
Title: Úvod do stránky Razor v ASP.NET Core Autor: popis Rick Anderson: kurz ASP.NET Core na stránky Razor. Obsahuje základní MVC, ASP.NET Core 2.x, pro vývoj webů a Visual Studio 2017.
je doc poskytuje přehled používání stránky Razor v ASP.NET Core k usnadnění vývoje zaměřené na stránce scénáře.
MS.Author: riande manager: wpickett ms.date: 12/09/2017 ms.topic: get-started-article ms.technology: aspnet ms.prod: asp.net core uid: mvc nebo razor stránky nebo index
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a>Úvod do stránky Razor v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Ryan Nowak](https://github.com/rynowak)

Stránky Razor je nová funkce rozhraní ASP.NET MVC jádra, která vytváří kódování zaměřené na stránce scénáře jednodušší a zvýšit produktivitu.

Pokud hledáte kurz, který používá Model-View-Controller přístup, najdete v části [Začínáme s ASP.NET MVC základní](xref:tutorials/first-mvc-app/start-mvc).

Tento dokument obsahuje úvod do stránky Razor. Není návod krok za krokem. Pokud zjistíte oddíly je obtížné postupujte podle, najdete v části [Začínáme s stránky Razor](xref:tutorials/razor-pages/razor-pages-start).

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a>Jádro ASP.NET 2.0 požadavky

Nainstalujte [.NET Core](https://www.microsoft.com/net/core) 2.0.0 nebo novější.

Pokud používáte Visual Studio, nainstalujte [Visual Studio](https://www.visualstudio.com/vs/) 2017 verze 15.3 nebo novější s následující úlohy:

* **ASP.NET a vývoje**
* **Vývoj pro různé platformy .NET core**

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a>Vytvoření projektu stránky Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

V tématu [Začínáme s stránky Razor](xref:tutorials/razor-pages/razor-pages-start) podrobné pokyny o tom, jak vytvářet stránky Razor projektu pomocí sady Visual Studio.

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

Spustit `dotnet new razor` z příkazového řádku.

Otevřete vygenerovaného *.csproj* soubor ze sady Visual Studio for Mac.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

Spustit `dotnet new razor` z příkazového řádku.

#   <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli) 

Spustit `dotnet new razor` z příkazového řádku.

---

## <a name="razor-pages"></a>Stránky Razor

Stránky Razor je povolena v *Startup.cs*:

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

Mějte na paměti na základní stránce:<a name="OnGet"></a>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

Předchozí kód mnoho vypadá soubor zobrazení syntaxe Razor. Čím je jiné je `@page` – direktiva. `@page`Vytvoří soubor do akce MVC – to znamená, že ji zpracovává požadavky přímo, bez průchodu přes řadič. `@page`musí být první direktivu Razor na stránce. `@page`má vliv na chování jiných objektů, které Razor.

Podobná stránky, můžete použít `PageModel` třídy, je uvedené v následující dva soubory. *Pages/Index2.cshtml* souboru:

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

*Pages/Index2.cshtml.cs* "" souboru kódu:

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

Podle konvence `PageModel` soubor třídy má stejný název jako soubor stránky Razor s *.cs* připojí. Je třeba na předchozí stránku Razor *Pages/Index2.cshtml*. Soubor obsahující `PageModel` třída nazývá *Pages/Index2.cshtml.cs*.

Přidružení cesty adresy URL na stránky určuje umístění stránky v systému souborů. Následující tabulka uvádí stránky Razor cestu a odpovídající adresy URL:

| Název souboru a cesty               | odpovídající adresy URL |
| ----------------- | ------------ |
| */Pages/Index.cshtml* | `/`nebo`/Index` |
| */Pages/Contact.cshtml* | `/Contact` |
| */Pages/Store/Contact.cshtml* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store`nebo`/Store/Index` |

Poznámky:

* Modul runtime vypadá pro stránky Razor soubory v *stránky* složky ve výchozím nastavení.
* `Index`Když neobsahuje adresu URL stránky, je výchozí stránky.

## <a name="writing-a-basic-form"></a>Zápis základní formulář

Funkce stránky Razor jsou navrženy tak, aby se používají u webových prohlížečů snadno běžných vzorů. [Model vazby](xref:mvc/models/model-binding), [značky Pomocníci](xref:mvc/views/tag-helpers/intro)a všechny pomocné rutiny HTML *právě pracovní* s vlastnostmi definovaný ve třídě stránky Razor. Vezměte v úvahu stránky, který implementuje "kontaktujte nás" formuláři pro základní `Contact` modelu:

Pro ukázky v tomto dokumentu `DbContext` je inicializován v [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) souboru.

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

Datový model:

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

Kontext databáze:

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

*Pages/Create.cshtml* zobrazení souboru:

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

*Pages/Create.cshtml.cs* souboru kódu na pozadí pro zobrazení:

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

Podle konvence `PageModel` třídy se nazývá `<PageName>Model` a je v stejného oboru názvů jako stránky.

`PageModel` Třída umožňuje oddělení logiku stránky od jeho prezentaci. Definuje stránky obslužné rutiny pro požadavky odeslané na stránku a data použitá k vykreslení stránky. Toto oddělení vám umožní spravovat stránky závislosti prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection) a [testování částí](xref:testing/razor-pages-testing) stránky.

Na stránce `OnPostAsync` *metoda obslužná rutina*, která se spouští `POST` požadavků (když uživatel odešle formulář). Můžete přidat metody obslužné rutiny pro všechny akce HTTP. Nejběžnější obslužné rutiny jsou:

* `OnGet`k chybě při inicializaci stavu potřebné pro stránku. [OnGet](#OnGet) ukázka.
* `OnPost`pro zpracování odesílání formuláře.

`Async` Přípona je nepovinný, ale se často používá podle konvence pro asynchronní funkce. `OnPostAsync` Kód v předchozím příkladu bude vypadat podobně jako by za normálních okolností psaní v kontroleru. Předchozí kód je typické pro stránky Razor. Většina primitiv MVC jako [model vazby](xref:mvc/models/model-binding), [ověření](xref:mvc/models/validation), a jsou sdíleny výsledky akce.  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

Předchozí `OnPostAsync` metoda:

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

Základní tok `OnPostAsync`:

Zkontrolujte chyby ověření.

*  Pokud nejsou žádné chyby, uložit data a přesměrování.
*  Pokud nejsou chyby, zobrazit stránku znovu s ověřovacích zpráv. Ověřování na straně klienta je stejný jako tradiční aplikací ASP.NET MVC jádra. V mnoha případech chyb při ověřování by být zjištěn v klientském počítači a nikdy odeslat na server.

Při úspěšném zadání data `OnPostAsync` volání metod obslužné rutiny `RedirectToPage` Pomocná metoda vrátí instanci `RedirectToPageResult`. `RedirectToPage`představuje novou výsledek akce, podobně jako `RedirectToAction` nebo `RedirectToRoute`, ale přizpůsobené pro stránky. V předchozím příkladu, je přesměrován na indexovou stránku kořenové (`/Index`). `RedirectToPage`podrobně [generování adresy URL pro stránky](#url_gen) části.

Když odeslaného formuláře došlo k chybám ověřování, (které jsou předávány serveru),`OnPostAsync` volání metod obslužné rutiny `Page` metodu helper. `Page`Vrací instanci třídy `PageResult`. Vrácení `Page` je podobná jak akce v řadiče vrátit `View`. `PageResult`Výchozí nastavení je <!-- Review  --> návratový typ pro obslužné rutiny metodu. Obslužná rutina metodu, která vrátí `void` vykreslí stránku.

`Customer` Používá vlastnost `[BindProperty]` atribut se vyjádřit výslovný souhlas s vazbou modelu.

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

Stránky Razor, ve výchozím nastavení, vazbu vlastnosti jenom s příkazy nebo GET. Vazba na vlastnosti může snížit množství kód, který máte k zápisu. Vazba snižuje kódu pomocí stejné vlastnosti k vykreslení pole formuláře (`<input asp-for="Customer.Name" />`) a přijměte vstupu.

Domovská stránka (*Index.cshtml*):

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

Kódu *Index.cshtml.cs* souboru:

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

*Index.cshtml* soubor obsahuje následující kód k vytvoření odkazu pro úpravy pro každý kontakt:

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

[Pomocná značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) použít `asp-route-{value}` atribut pro vygenerování odkazu na stránku upravit. Odkaz obsahuje data trasy s kontakt ID. Například `http://localhost:5000/Edit/1`.

*Pages/Edit.cshtml* souboru:

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

První řádek obsahuje `@page "{id:int}"` – direktiva. Omezení směrování`"{id:int}"` informuje stránky tak, aby přijímal požadavky na stránku, které obsahují `int` směrovat data. Pokud požadavek na stránku neobsahuje data trasy, která lze převést na `int`, modul runtime vrátí chybu HTTP 404 (není nalezena).

*Pages/Edit.cshtml.cs* souboru:

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

*Index.cshtml* soubor zároveň obsahuje kód k vytvoření tlačítko Odstranit pro každý kontakt zákazníka:

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

Pokud tlačítko Odstranit je vykreslena ve formátu HTML, jeho `formaction` zahrnuje parametry:

* Obraťte se na zákaznické ID určeného `asp-route-id` atribut.
* `handler` Určeným elementem `asp-page-handler` atribut.

Tady je příklad vykreslené odstranění tlačítka s zákazník obraťte se na ID `1`:

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

Při výběru tlačítka, formuláře `POST` požadavek odeslán do serveru. Podle konvence je vybraný název metody obslužné rutiny na základě hodnotu `handler` parametr podle schéma `OnPost[handler]Async`.

Protože `handler` je `delete` v tomto příkladu `OnPostDeleteAsync` metoda obslužná rutina se používá procesu `POST` požadavku. Pokud `asp-page-handler` nastavena na jinou hodnotu, jako například `remove`, metodu obslužná rutina stránky s názvem `OnPostRemoveAsync` je vybrána.

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

`OnPostDeleteAsync` Metoda:

* Přijme `id` z řetězce dotazu.
* Dotazuje databázi pro zákazníka kontaktu s `FindAsync`.
* Pokud je nalezen kontaktování zákazníků, budou se odebrat ze seznamu kontaktů zákazníka. Databáze se aktualizuje.
* Volání `RedirectToPage` přesměrovat na indexovou stránku kořenové (`/Index`).

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a>XSRF/proti útokům CSRF a stránky Razor

Nemáte psaní jakéhokoli kódu pro [antiforgery ověření](xref:security/anti-request-forgery). Antiforgery generování tokenů a ověření jsou automaticky součástí stránky Razor.

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>Pomocí stránky Razor rozložení, částečné., šablony a pomocníky značky

Stránky fungovat se všemi funkcemi zobrazovací modul Razor. Rozložení, šablony, a částečné značka Pomocníci *soubor _ViewStart.cshtml*, *_ViewImports.cshtml* pracovní stejným způsobem, tak pro běžné zobrazení syntaxe Razor.

Tato stránka umožňuje declutter díky některé z těchto funkcí.

Přidat [rozložení stránky](xref:mvc/views/layout) k *Pages/_Layout.cshtml*:

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

[Rozložení](xref:mvc/views/layout):

* Určuje rozložení každé stránce (Pokud stránce výslovný nesouhlas rozložení).
* Importuje struktury HTML jako je JavaScript a šablon.

V tématu [rozložení stránky](xref:mvc/views/layout) Další informace.

[Rozložení](xref:mvc/views/layout#specifying-a-layout) je nastavena *Pages/_ViewStart.cshtml*:

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

**Poznámka:** rozložení je v *stránky* složky. Stránky vyhledejte další zobrazení (rozložení, šablony, částečné.) hierarchicky, spouštění ve stejné složce jako aktuální stránku. Rozložení v *stránky* složky lze z libovolné stránky Razor pod *stránky* složky.

Doporučujeme **není** chápat rozložení souboru *zobrazení a sdílených* složky. *Zobrazení a sdílených* je zobrazení vzor MVC. Stránky Razor jsou určené spoléhají na hierarchii složek, není cesta konvence.

Obsahuje zobrazení vyhledávání ze stránky Razor *stránky* složky. Rozložení, šablony a částečné používáte s řadiče MVC a konvenční zobrazení syntaxe Razor *právě pracovní*.

Přidat *Pages/_ViewImports.cshtml* souboru:

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

`@namespace`se vysvětluje dále v tomto kurzu. `@addTagHelper` – Direktiva přináší [předdefinované značky Pomocníci](xref:mvc/views/tag-helpers/builtin-th/Index) pro všechny stránky v *stránky* složky.

<a name="namespace"></a>

Když `@namespace` – direktiva se používá explicitně na stránce:

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

Direktiva Nastaví obor názvů pro stránku. `@model` – Direktiva nemusí obsahovat obor názvů.

Když `@namespace` – direktiva je součástí *_ViewImports.cshtml*, zadaný obor názvů poskytuje předponu pro obor názvů generovaným na stránce, který umožňuje importovat `@namespace` – direktiva. Zbytek vygenerovaného oboru názvů (část příponu) je relativní cesta mezi složku, která obsahuje oddělené tečkou *_ViewImports.cshtml* a složce obsahující danou stránku.

Například kódu na pozadí *Pages/Customers/Edit.cshtml.cs* explicitně nastaví obor názvů:

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

*Pages/_ViewImports.cshtml* souboru nastaví následující obor názvů:

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

Vygenerovaný obor názvů pro *Pages/Customers/Edit.cshtml* Razor stránky je stejná jako souboru kódu. `@namespace` – Direktiva v byla navržená tak, třídy C# přidán do projektu a kód generovaný stránky *právě pracovní* bez nutnosti přidání `@using` direktivy pro souboru kódu.

**Poznámka:** `@namespace` funguje taky s konvenční zobrazení syntaxe Razor.

Původní *Pages/Create.cshtml* zobrazení souboru:

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

Aktualizovaný *Pages/Create.cshtml* zobrazení souboru:

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

[Projektu úvodní stránky Razor](#rpvs17) obsahuje *Pages/_ValidationScriptsPartial.cshtml*, který zachytí až ověřování na straně klienta.

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>Generování adresy URL pro stránky

`Create` Stránky, která je uvedená dříve, používá `RedirectToPage`:

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

Aplikace má následující strukturu souboru nebo složky:

* */ Stránky*

  * *Index.cshtml*
  * *Nebo smlouvy zákazníka*

    * *Create.cshtml*
    * *Edit.cshtml*
    * *Index.cshtml*

*Pages/Customers/Create.cshtml* a *Pages/Customers/Edit.cshtml* stránky přesměrovat na *Pages/Index.cshtml* po úspěšné. Řetězec `/Index` je součástí identifikátor URI k přístupu na předchozí stránku. Řetězec `/Index` lze použít ke generování identifikátory URI k *Pages/Index.cshtml* stránky. Příklad:

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

Název stránky je cesta na stránku z kořenového adresáře */stránky* složky (včetně jako úvodní `/`, například `/Index`). Předchozí ukázky generování adresy URL jsou mnohem víc bohaté funkce než jenom hardcoding adresu URL. Generování adresy URL používá [směrování](xref:mvc/controllers/routing) a vygenerovat a kódování parametry podle jak trasy, která je definována v cílovou cestu.

Generování adresy URL pro stránky podporuje relativních názvů. Následující tabulka uvádí, které Index vybrány jiné `RedirectToPage` parametry z *Pages/Customers/Create.cshtml*:

| RedirectToPage(x)| Stránka |
| ----------------- | ------------ |
| RedirectToPage("/Index") | *Pages/Index* |
| RedirectToPage("./Index"); | *Pages/Customers/Index* |
| RedirectToPage("../Index") | *Pages/Index* |
| RedirectToPage("Index")  | *Pages/Customers/Index* |

`RedirectToPage("Index")`, `RedirectToPage("./Index")`, a `RedirectToPage("../Index")` jsou *relativních názvů*. `RedirectToPage` Parametr *kombinaci* cestou k aktuální stránce k výpočtu název cílové stránky.  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

Relativní název propojení je užitečné, při vytváření lokalit se strukturou komplexní. Pokud používáte relativních názvů propojení mezi stránkami ve složce, můžete přejmenovat této složky. Všechny odkazy na i nadále fungovat, (protože jejich nezahrnuli název složky).

## <a name="tempdata"></a>TempData

ASP.NET Core zpřístupní [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) vlastnost [řadič](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller). Tato vlastnost ukládá data, dokud je pro čtení. `Keep` a `Peek` metody můžete použít k prozkoumání dat bez odstranění. `TempData`jsou užitečné pro přesměrování, pokud se data potřebná pro více než jeden požadavek.

`[TempData]` Atribut je nového v technologii ASP.NET 2.0 jádra a je podporovaná v řadičích a stránky.

Následující kód nastaví hodnotu `Message` pomocí `TempData`:

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

Následující kód v *Pages/Customers/Index.cshtml* souboru se zobrazí hodnota `Message` pomocí `TempData`.

```cshtml
<h3>Msg: @Model.Message</h3>
```

*Pages/Customers/Index.cshtml.cs* souboru kódu se vztahuje `[TempData]` atribut `Message` vlastnost.

```cs
[TempData]
public string Message { get; set; }
```

V tématu [TempData](xref:fundamentals/app-state#temp) Další informace.

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a>Více obslužné rutiny na stránce

Na následující stránce generuje kód pro dvě stránky pomocí obslužné rutiny `asp-page-handler` pomocná značky:

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

Formuláře v předchozím příkladu má dva odeslání tlačítka, každý využívající `FormActionTagHelper` odeslat na jinou adresu URL. `asp-page-handler` Atribut je Pomocníka pro `asp-page`. `asp-page-handler`generuje adresy URL, které odesílají do jednotlivých metod obslužné rutiny, které jsou definované na stránce. `asp-page`není zadán, protože je ukázka propojení na aktuální stránce.

Soubor kódu:

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

Předchozí kód používá *s názvem metody obslužná rutina*. Metody s názvem obslužné rutiny vytvářejí provedením text v názvu po `On<HTTP Verb>` a před `Async` (pokud existuje). V předchozím příkladu jsou metody stránky OnPost**JoinList**Async a OnPost**JoinListUC**asynchronní. S *OnPost* a *asynchronní* odebrat, jsou názvy obslužná rutina `JoinList` a `JoinListUC`.

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

Použití předcházející code cesty URL, kterou odesílá `OnPostJoinListAsync` je `http://localhost:5000/Customers/CreateFATH?handler=JoinList`. Cesty URL, kterou odesílá `OnPostJoinListUCAsync` je `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.

## <a name="customizing-routing"></a>Přizpůsobení směrování

Pokud chcete řetězec dotazu `?handler=JoinList` v adrese URL, můžete změnit trasy, která má název obslužné rutiny chápat část adresy obsahující cestu adresy URL. Trasy, která můžete přizpůsobit přidáním šablonu trasy v dvojitých uvozovkách po `@page` – direktiva.

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

Předchozí trasy vloží název obslužné rutiny cesty URL místo řetězec dotazu. `?` Následující `handler` znamená je volitelný parametr trasy.

Můžete použít `@page` a přidejte další segmenty a parametry k postupu na stránce. Ať je k dispozici je **připojí** trasu výchozí stránky. Použití absolutní nebo virtuální cesta ke změně stránky trasy (například `"~/Some/Other/Path"`) není podporován.

## <a name="configuration-and-settings"></a>Konfigurace a nastavení

Nakonfigurujte rozšířené možnosti, pomocí metody rozšíření `AddRazorPagesOptions` na tvůrce MVC:

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

Teď můžete použít `RazorPagesOptions` nastavit kořenový adresář pro stránky, nebo přidejte aplikace model konvence pro stránky. Jsme budete povolit další rozšíření tímto způsobem v budoucnu.

Předkompilovat zobrazení, najdete v části [kompilace zobrazení syntaxe Razor](xref:mvc/views/view-compilation) .

[Stažení nebo zobrazení ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).

V tématu [Začínáme s stránky Razor v ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), který je založený na tento úvod.

### <a name="specify-that-razor-pages-are-at-the-content-root"></a>Zadejte, zda stránky Razor se v kořenovém adresáři obsahu

Ve výchozím nastavení jsou stránky Razor root v */stránky* adresáře. Přidat [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) k [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) k určení, že vaše stránky Razor se v kořenovém adresáři obsahu ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikace:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a>Určit, že stránky Razor na vlastní kořenový adresář

Přidat [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) k [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) k určení, že vaše stránky Razor se vlastní kořenového adresáře v aplikaci (zadejte relativní cestu):

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a>Viz také

* [Začínáme se stránkami Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Konvence autorizace stránky Razor](xref:security/authorization/razor-pages-authorization)
* [Syntaxe Razor stránky vlastní trasy a stránka zprostředkovatele modelu](xref:mvc/razor-pages/razor-pages-convention-features)
* [Jednotka stránky Razor a testování integrace](xref:testing/razor-pages-testing)
